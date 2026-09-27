## The First Formula of Coding in tf:

```jsx
<BLOCK> <parameters> { 
	arguments
	arguments

}
```

## Common types of Block:

```jsx
BLOCK:
 1. Resource
 2. Output
 3. Variable 
```

## Specifying the Region

```jsx
provider "aws" {
    region = "us-east-2"
}
```

## Creating a very basic s3 Bucket

```jsx
resource aws_s3_bucket my_bucket {
 
  bucket = "anew-terraform"
 
}
```

## Creating a key pair for:

1. first type ‘ssh-keygen’ in terminal and it will give you both public and private key, now to configure it in aws do this:

 

```jsx
resource aws_key_pair my_key {
  key_name   = "my-key" 
  public_key = file("keypair.pub")
  }
```

## Configuring default VPC

```jsx
resource "aws_default_vpc" "default" {
  tags = {
    Name = "Default VPC"
  }
}
```

## Creating a ec2 Instance

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
  key_name      = aws_key_pair.my_keyfortf.key_name
  ami           = "ami-0e5497a77ef21b5ac" 
  instance_type = "t2.micro"
  security_groups = [aws_security_group.securitygr_from_tf.name]

  tags = {
    Name = "myinstance-from-tf"
  }

  root_block_device {
    volume_size = 8
    volume_type = "gp2"
  }
}
```

## Variables in Terraform:

```jsx
variable "aws_storage_info"{
    default=15
    type=number
}

// using variable in file , see last lines for variable usage
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
  key_name      = aws_key_pair.my_keyfortf.key_name
  ami           = "ami-0e5497a77ef21b5ac" 
  instance_type = "t2.micro"
  security_groups = [aws_security_group.securitygr_from_tf.name]

  tags = {
    Name = "myinstance-from-tf"
  }

  root_block_device {
    volume_size = var.aws_storage_info // here is the used variable
    volume_type = "gp2"
  }
}
```

## Adv Variable:

```jsx
variable "aws_instance_basic" {
    description = "Basic AWS instance configuration"
    type        = object({
        ami           = string
        instance_type = string
        key_name      = string
        security_groups = list(string)
    })
    default = {
        ami           = "ami-0e5497a77ef21b5ac"
        instance_type = "t2.micro"
        key_name      = "keyforterraform"
        security_groups = ["securitygroupfrom_tf"]
    }
  
}

// now the file of main
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups

  tags = {
    Name = "myinstance-from-tf"
  }

  root_block_device {
    volume_size = 10
    volume_type = "gp2"
  }
}
```

## Outputs in Terraform:

```jsx
// WE can create multiple outputs like this

output "ec2_public_ip" {
   value = aws_instance.my_instance.public_ip
 }

 output "ec2_private_ip" {
   value = aws_instance.my_instance.private_ip
 }

output "ec2_instance_id" {
   value = aws_instance.my_instance.id
 }

output "ec2_instance_ami" {
   value = aws_instance.my_instance.ami
 }

 output "ec2_instance_type" {
   value = aws_instance.my_instance.instance_type
 }

 output "ec2_instance_key_name" {
   value = aws_instance.my_instance.key_name
 }

 output "ec2_instance_security_groups" {
   value = aws_instance.my_instance.security_groups
 }

 output "ec2_instance_root_block_device_volume_size" {
   value = aws_instance.my_instance.root_block_device[0].volume_size
 }
 
output "ec2_instance_public_dns" {
   value = aws_instance.my_instance.public_dns
 }

```

## Running a Bash script on our ec2 instance:

```jsx
#!bin/bash

sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx

echo "<h1>Welcome to my Nginx server</h1>" | sudo tee /var/www/html/index.html

```

Now we have to just specify user_data in our instance terraform file:

 

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups
  user_data = file("install_nginx_script.sh") // here we are using it

  tags = {
    Name = "myinstance-from-tf"
  }

  root_block_device {
    volume_size = 10
    volume_type = "gp2"
  }
}
```

## Meta Arguments:

First let talk about `count` meta argument: It allows us to create multiple instances with just one count argument , will also do one simple change in outputs terraform file 

> There is just one problem with the count, it will create all the instances with the same name, can use for_each to counter this problem
> 

Now Code :

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups
  user_data = file("install_nginx_script.sh")
  count = 2 // see heree is the count argument

  tags = {
    Name = "myinstance-from-tf"
  }

  root_block_device {
    volume_size = 10
    volume_type = "gp2"
  }
}
```

This is the change in output file [*]:

```jsx
output "ec2_public_ip" {
   value = aws_instance.my_instance[*].public_ip
 }

 output "ec2_private_ip" {
   value = aws_instance.my_instance[*].private_ip
 }

output "ec2_instance_id" {
   value = aws_instance.my_instance[*].id
 }

output "ec2_instance_ami" {
   value = aws_instance.my_instance[*].ami
 }

 output "ec2_instance_type" {
   value = aws_instance.my_instance[*].instance_type
 }

 output "ec2_instance_key_name" {
   value = aws_instance.my_instance[*].key_name
 }

 output "ec2_instance_security_groups" {
   value = aws_instance.my_instance[*].security_groups
 }

 output "ec2_instance_root_block_device_volume_size" {
   value = aws_instance.my_instance[*].root_block_device[0].volume_size
 }

output "ec2_instance_public_dns" {
   value = aws_instance.my_instance[*].public_dns
 }

```

## for_each Meta Argument:

It is so simple we can do this simple way, but just need a loop type thing on our output file

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
 
 for_each = tomap({
    "instance_1_name" = "first_instance"
    "instance_2_name" = "second_instance"
  })

  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups
  user_data = file("install_nginx_script.sh")

  tags = {
    Name = each.value
  }

  root_block_device {
    volume_size = 10
    volume_type = "gp2"
  }
}
```

HERE LIKE THIS ALSO SIMPLE:

```jsx
output "ec2_public_ip" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.public_ip
   }
 }

 output "ec2_private_ip" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.private_ip
   }
 }

output "ec2_instance_id" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.id
   }    
 }

output "ec2_instance_ami" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.ami
   }
 }

 output "ec2_instance_type" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.instance_type
   }
 }

 output "ec2_instance_key_name" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.key_name
   }
 }

 output "ec2_instance_security_groups" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.security_groups
   }
 }

 output "ec2_instance_root_block_device_volume_size" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.root_block_device[0].volume_size
   }
 }

output "ec2_instance_public_dns" {
   value = {
    for instance_name, instance in aws_instance.my_instance : instance_name => instance.public_dns
   }
 }

```

## Depends on Meta Argument

This means wait until what we have written inside depends_on to be up , don’t create until our depends_on is not up or build 

JUST A SIMPLE DEPENDS_ON ARGUMENT

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
 
 for_each = tomap({
    "instance_1_name" = "first_instance"
    "instance_2_name" = "second_instance"
  })

  depends_on = [ aws_security_group.securitygr_from_tf ] // here it is

  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups
  user_data = file("install_nginx_script.sh")

  tags = {
    Name = each.value
  }

  root_block_device {
    volume_size = 10
    volume_type = "gp2"
  }
}
```

## Conditional Expressions in terraform:

Just making a new variable as envoir

```
variable "aws_instance_basic" {
    description = "Basic AWS instance configuration"
    type        = object({
        ami           = string
        instance_type = string
        key_name      = string
        security_groups = list(string)
    })
    default = {
        ami           = "ami-0e5497a77ef21b5ac"
        instance_type = "t2.micro"
        key_name      = "keyforterraform"
        security_groups = ["securitygroupfrom_tf"]
    }
  
}

variable "aws_storage_info"{
    default=15
    type=number
}

variable "envoir" {
    default = "prod"
    type    = string
}
```

```jsx
resource aws_key_pair my_keyfortf {
  key_name   = "keyforterraform" 
  public_key = file("keypair.pub")
  }

// configuring default vpc

resource aws_vpc default {
 
}

resource aws_security_group securitygr_from_tf {
  name        = "securitygroupfrom_tf"
  description = "Security group made from terraform"
  vpc_id      = aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] 
    }

    egress { 
        from_port   = 0 
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]  
    }

    tags={
        Name = "security_groupfrom_tf"
    }
}

resource aws_instance my_instance {
 
 for_each = tomap({
    "instance_1_name" = "first_instance"
    "instance_2_name" = "second_instance"
  })

  depends_on = [ aws_security_group.securitygr_from_tf ]

  key_name      = var.aws_instance_basic.key_name
  ami           = var.aws_instance_basic.ami 
  instance_type = var.aws_instance_basic.instance_type
  security_groups = var.aws_instance_basic.security_groups
  user_data = file("install_nginx_script.sh")

  tags = {
    Name = each.value
  }

  root_block_device {
    volume_size = var.envoir == "prod" ? 8 : var.aws_storage_info // here imp
    volume_type = "gp2"
  }
}
```

## Cloud State to Terraform:

Imagine if we have an instance running in our aws but we want to stop it from terraform

COMANDS:

1. terraform state list ⇒ will give current state

> See terraform import in trainwithshubham video 4:07:00
> 

## Where to keep our Terraform tf.statefiles?

IF we put it in github, and somehow the tfstate files get deleted it can create a panic in whole infrastructure 

## LEARN ABOUT STATE CONFLICT BY SEARCHING (IMP FOR INTERVIEW )

## CONCEPT of Remote Backend in terraform:

Here we do state file locking: 

!image.png

We simply put our terraform state file inside a s3 bucket and uses dynamo-db to store the lockId generated by s3 bucket . So what happens is that when 1 person performs changes in s3 bucket file it will generate a lockId while this lockId is there no one else can change the s3 bucket files until the person1 releases the lock Id , Releasing lockID means just removing it. NOW s3 also provides lockId without dynamo read about it..

 

## Terraform Workspaces

COMMANDS:

1. terraform state list
2. terraform workspace list
3. terraform workspace new <enterNewWorkspaceName>
4. terraform workspace select <enterWorkspaceName>

## REMEMBER 3 THINGS TO MANAGE INFRA USING  :

1. TERRAFORM WORKSPACE
2. GITHUB BRANCH
3. IN TERRAFORM VARIABLES FILE THE ENVIRONMENT VARIABLE AND IN THE TAGS use that it as Environment = var.env
