terraform {
  backend "s3" {
    bucket         = "lili-terraform-state"
    key            = "global/s3/terraform.tfstate"
    region         = "us-east-1"
    dynamo_table   = "terraform-state"
    encrypt        = true
  }
}
