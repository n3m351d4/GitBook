# 3 Создание Web сервера



```text
provider "aws" {
  region = "eu-central-1"
}


resource "aws_instance" "my_webserver" {
  ami                    = "ami-03a71cec707bfc3d7" # Amazon Linux AMI
  instance_type          = "t3.micro"
  vpc_security_group_ids = [aws_security_group.my_webserver.id] # sec group создастся для нового инстанса
  user_data              = <<EOF

#!/bin/bash
yum -y update
yum -y install httpd
myip=`curl http://169.254.169.254/latest/meta-data/local-ipv4`
echo "<h2>WebServer with IP: $myip</h2><br>Build by Terraform!"  >  /var/www/html/index.html
sudo service httpd start
chkconfig httpd on
EOF

  tags = {
    Name = "Web Server Build by Terraform"
    Owner = "Name"
  }
}


resource "aws_security_group" "my_webserver" {
  name = "WebServer Security Group"
  description = "My First SecurityGroup"

  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port = 0
    to_port = 0
    protocol = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "Web Server SecurityGroup"
    Owner = "Name"
  }
}
```



Awsregion.info – названия регионов

cidr\_blocks - откуда разрешать подключения

ingress - входящий трафик

egress - исходящий

В &lt;&lt;EOF EOF можно записать скрипт, который будет выполняться при запуске

/\* ЫЫЫ \*/ - комментарий

Скрипт можно держать в отдельном файле:

```text
user_data = file("user_data.sh")
```

Скрипты могут быть динамичными:

```text
user_data = templatefile("user_data.sh.tpl"), {
    f_name = "Name",
    l_name = "1Name",
    names = ["First". "Second", "Third", "Forth", "Fifth"])
```

Тогда, в скрипте:

```text
#!/bin/bash
yum -y update
yum -y install httpd

myip=`curl http://169.254.169.254/latest/meta-data/local-ipv4`

cat <<EOF > /var/www/html/index.html
<html>
Owner ${f_name} ${lname}<br>

%{ for x in names ~}
Hello to ${x}<br>
%{ endfor ~}
<html>

sudo service httpd start
chkconfig httpd on
EOF
```

_terraform console_ - можно отправить команду templatefile с содержимым и посмотреть результат работы шаблона без разворачивания.

