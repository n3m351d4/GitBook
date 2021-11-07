# 7 Порядок создания ресурсов

Задается после создания чего создается ресурс. Depends\_on.

```
resource "aws_instance" "my_server_web" {
  ami                    = "ami-03a71cec707bfc3d7"
  instance_type          = "t3.micro"
  vpc_security_group_ids = [aws_security_group.my_webserver.id]

  tags = {
    Name = "Server-Web"
  }
  depends_on = [aws_instance.my_server_db, aws_instance.my_server_app]
}


```
