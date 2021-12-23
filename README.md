provider "aws" {
  region     = "ap-south-1"
  access_key = "AKIAZDLT7TUBZLT5YORO"
  secret_key = "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"
}
provider "aws" {
  region     = "eu-central-1"
  alias = "second_sqs_region"
  access_key = "AKIAZDLT7TUBZLT5YORO"
  secret_key = "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"
}
provider "aws" {
  region     = "ap-southeast-1"
  alias = "third_sqs_region"
  access_key = "AKIAZDLT7TUBZLT5YORO"
  secret_key = "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"
}
resource "aws_sns_topic" "sns_updates" {
  name = "maheshpoc-update"
 
}
resource "aws_sqs_queue" "sqs_updates" {
  name = "maheshpoc-update"
}
resource "aws_sqs_queue" "sqs_updates_2" {
  name = "maheshpoc-update"
  provider = aws.second_sqs_region
}

resource "aws_sqs_queue" "sqs_updates_3" {
  name = "maheshpoc-update"
   provider = aws.third_sqs_region
}

resource "aws_sns_topic_subscription" "sns_subscription" {
  topic_arn = aws_sns_topic.sns_updates.arn
  protocol  = "sqs"
  endpoint  = aws_sqs_queue.sqs_updates.arn
}
resource "aws_sns_topic_subscription" "sqs_second_subscription" {
  topic_arn = aws_sns_topic.sns_updates.arn
  protocol  = "sqs"
  endpoint  = aws_sqs_queue.sqs_updates_2.arn
}
resource "aws_sns_topic_subscription" "sqs_third_subscription" {
  topic_arn = aws_sns_topic.sns_updates.arn
  protocol  = "sqs"
  endpoint  = aws_sqs_queue.sqs_updates_3.arn
}
