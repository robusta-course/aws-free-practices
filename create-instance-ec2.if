# Định nghĩa provider cho AWS
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.61"
    }
  }
}

provider "aws" {
  region = "ap-southeast-1" # region cần tạo resource ví dụ: "ap-southeast-1"
}

data "aws_caller_identity" "current" {}

data "aws_region" "current" {}

# Gan key pair để đăng nhập vào EC2
resource "aws_key_pair" "my_key_pair" {
  key_name   = "my-aws-key"
  public_key = file("~/.ssh/id_rsa.pub") # Đường dẫn đến public key
}

# Tạo một security group cho phép kết nối SSH
resource "aws_security_group" "allow_ssh" {
  name        = "allow_ssh"
  description = "Allow SSH inbound traffic"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # Cho phep tat ca ip ket noi den port 22
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"] # mo tat ca ket noi tu ben trong ra ngoai
  }
}

# Tạo một EC2 instance với IP public
resource "aws_instance" "my_ec2_instance" {
  ami           = "ami-047126e50991d067b" # ID ubuntu 24.04
  instance_type = "t2.micro"               # Miễn phí cho AWS Free Tier
  key_name      = aws_key_pair.my_key_pair.key_name

  # Gán security group
  vpc_security_group_ids = [aws_security_group.allow_ssh.id]
  
  #gan subnet
   subnet_id = "subnet-04328ecc73b8cf351" # gan subnet can tao instance

  # Gán IP public
  associate_public_ip_address = true

  tags = {
    Name = "terraform-ec2-example"
  }
}

# show IP public instance sau khi tao xong
output "instance_public_ip" {
  value = aws_instance.my_ec2_instance.public_ip
}

output "my_key_pair" {
  value = aws_instance.my_ec2_instance.key_name
}
