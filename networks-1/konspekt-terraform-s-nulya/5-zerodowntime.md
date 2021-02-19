# 5 ZeroDownTime

```text
provider "aws" {
  region = "ca-central-1"
}


resource "aws_eip" "my_static_ip" { #статичный IP адрес
  instance = aws_instance.my_webserver.id
  tags = {
    Name  = "Web Server IP"
    Owner = "NAME"
  }
}


resource "aws_instance" "my_webserver" {
  ami                    = "ami-07ab3281411d31d04"
  instance_type          = "t3.micro"
  vpc_security_group_ids = [aws_security_group.my_webserver.id]
  user_data = templatefile("user_data.sh.tpl", {
    f_name = "NAME0",
    l_name = "NAME1",
    names  = ["Vasya", "Kolya", "Petya", "John", "Donald", "Masha", "Lena", "Katya"]
  })

  tags = {
    Name  = "Web Server Build by Terraform"
    Owner = "NAME"
  }

  lifecycle { # не удаляется при обновлении параметров 
    create_before_destroy = true
  }

}


resource "aws_security_group" "my_webserver" {
  name        = "WebServer Security Group"
  description = "My First SecurityGroup"


  dynamic "ingress" {
    for_each = ["80", "443"]
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }


  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name  = "Web Server SecurityGroup"
    Owner = "NAME"
  }
}
```

lifecycle {prevent\_destroy = true}- не удаляется при обновлении параметров

lifecycle {ignore\_changes = \["ami", "user\_data"\]} - не пересоздает ресурс при изменении параметров

lifecycle {create\_before\_destroy = true} - горячая замена сервера - поднимает новый - меняет IP и потом выключает старый.

