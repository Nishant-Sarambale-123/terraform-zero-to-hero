Absolutely! Here's a list of **Terraform syntax examples** for **each key concept**, including **advanced ones** like `dynamic`, `lifecycle`, and `provisioners`.

---

## ✅ **1. Provider**

Defines the cloud/service provider.

```hcl
provider "aws" {
  region = "us-east-1"
}
```

---

## ✅ **2. Variable**

Declare and use variables.

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

# Usage
instance_type = var.instance_type
```

---

## ✅ **3. Output**

Returns a value after deployment.

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

---

## ✅ **4. Locals**

Define reusable expressions.

```hcl
locals {
  common_tags = {
    Owner = "Nishant"
    Env   = "dev"
  }
}

resource "aws_instance" "example" {
  tags = local.common_tags
}
```

---

## ✅ **5. Expressions & Interpolation**

```hcl
# Expression
instance_type = var.environment == "prod" ? "t3.large" : "t2.micro"

# Interpolation
name = "web-${var.environment}-${count.index}"
```

---

## ✅ **6. Modules**

Reusable groups of resources.

```hcl
module "vpc" {
  source     = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
}
```

---

## ✅ **7. Data Source**

Use existing infrastructure data.

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

---

## ✅ **8. Conditional Logic**

```hcl
instance_type = var.is_production ? "t3.large" : "t2.micro"
```

---

## ✅ **9. count & for\_each**

### Using `count`:

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

### Using `for_each`:

```hcl
variable "bucket_names" {
  type = set(string)
  default = ["logs", "images"]
}

resource "aws_s3_bucket" "buckets" {
  for_each = var.bucket_names
  bucket   = each.key
}
```

---

## ✅ **10. Dynamic Block**

Used when repeating nested blocks.

```hcl
resource "aws_security_group" "example" {
  name = "example-sg"

  dynamic "ingress" {
    for_each = var.ingress_ports
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

---

## ✅ **11. Lifecycle**

Control resource behavior.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    prevent_destroy = true
    ignore_changes  = [tags["Name"]]
  }
}
```

---

## ✅ **12. Provisioners**

Run scripts after resource creation.

### Local Exec:

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Hello from Terraform"
  }
}
```

### Remote Exec (SSH):

```hcl
resource "null_resource" "remote" {
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("~/.ssh/id_rsa")
    host     = aws_instance.example.public_ip
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install nginx -y"
    ]
  }
}
```

---

Would you like these bundled as a ready `.tf` file or notes format?
