Here are **detailed notes on Terraform**, covering all core concepts with full explanations and examples. These notes are useful for learning and interview preparation.

---

# ✅ **Terraform Detailed Notes**

---

## 🔸 What is Terraform?

**Terraform** is an open-source Infrastructure as Code (IaC) tool created by **HashiCorp**. It allows you to define and provision infrastructure using a high-level configuration language called **HCL (HashiCorp Configuration Language)**.

---

## 🔸 Key Features of Terraform

| Feature                     | Description                                                                    |
| --------------------------- | ------------------------------------------------------------------------------ |
| Infrastructure as Code      | Infrastructure is defined in code (HCL or JSON) for automation and versioning. |
| Execution Plan              | Terraform shows a plan before applying changes.                                |
| Resource Graph              | Builds a dependency graph for parallel creation of resources.                  |
| Change Automation           | Applies only the required changes (minimal changes).                           |
| Multi-cloud Support         | Works across AWS, Azure, GCP, VMware, etc.                                     |
| Provider-based Architecture | Uses plugins (providers) to manage different platforms.                        |

---

## 🔸 Terraform Core Concepts

### 1. **Providers**

* A provider is a plugin that interacts with APIs of cloud providers like AWS, Azure, GCP.
* Examples: `aws`, `azurerm`, `google`, `kubernetes`, `vsphere`.

```hcl
provider "aws" {
  region = "us-west-1"
}
```

---

### 2. **Resources**

* The most important element in Terraform.
* Represents a component like EC2 instance, S3 bucket, etc.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

---

### 3. **Variables**

* Used to make configurations reusable and dynamic.
* Declared using `variable` blocks.

```hcl
variable "instance_type" {
  default = "t2.micro"
}
```

Used like:

```hcl
instance_type = var.instance_type
```

---

### 4. **Outputs**

* Used to display useful information after execution.
* Helpful for passing values to other modules or just printing.

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

### 5. **State File**

* Stores metadata and current state of infrastructure.
* Default: `terraform.tfstate`
* Important for determining what to add/update/delete.

🛑 **Never manually edit the state file!**

---

### 6. **Modules**

* Reusable group of Terraform resources.
* Encourages DRY (Don't Repeat Yourself) principle.

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
}
```

---

### 7. **Data Sources**

* Used to fetch external data (e.g., AWS AMIs, existing resources).

```hcl
data "aws_ami" "latest" {
  most_recent = true
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*"]
  }
  owners = ["amazon"]
}
```

---

### 8. **Terraform CLI Commands**

| Command              | Description                                 |
| -------------------- | ------------------------------------------- |
| `terraform init`     | Initializes Terraform in the directory      |
| `terraform plan`     | Shows execution plan                        |
| `terraform apply`    | Applies the plan and creates resources      |
| `terraform destroy`  | Deletes all resources defined in the config |
| `terraform validate` | Validates the configuration files           |
| `terraform fmt`      | Formats code to standard style              |
| `terraform show`     | Shows current state                         |
| `terraform state`    | Advanced manipulation of state file         |

---

### 9. **Terraform Lifecycle**

1. **Write**: Define infrastructure using `.tf` files.
2. **Initialize**: Run `terraform init`.
3. **Plan**: Preview changes using `terraform plan`.
4. **Apply**: Run `terraform apply` to provision.
5. **Maintain**: Modify `.tf` files to manage updates.
6. **Destroy**: Use `terraform destroy` when no longer needed.

---

### 10. **Backends**

* Define where the state file is stored.
* Can be **local** (default) or **remote** (like S3, Terraform Cloud).

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

---

### 11. **Provisioners**

* Used to execute scripts or commands after resource creation.
* Not recommended unless necessary.

```hcl
resource "aws_instance" "web" {
  # ...
  provisioner "remote-exec" {
    inline = ["sudo apt-get update"]
  }
}
```

---

### 12. **Terraform Functions**

* Built-in functions like `join()`, `lookup()`, `file()`, `length()`, etc.

```hcl
join(", ", ["apple", "banana"])  # returns "apple, banana"
```

---

### 13. **Workspaces**

* Allow managing different environments (dev, prod) with the same config.

```bash
terraform workspace new dev
terraform workspace select prod
```

---

### 14. **Terraform Lock File**

* `terraform.lock.hcl` ensures consistent provider versions.
* Auto-generated during `terraform init`.

---

## 🔸 Best Practices

* Use modules to organize code.
* Keep state file secure (use remote backend + encryption).
* Use version control (Git).
* Don’t hardcode credentials (use environment variables or IAM roles).
* Enable locking and state versioning in remote backend (S3 + DynamoDB for AWS).
* Use workspaces or separate state files for environments.

---

## 🔸 Interview Questions (Basic)

1. **What is Terraform?**

   > Terraform is an open-source Infrastructure as Code tool that lets you define cloud resources in a declarative way using HCL.

2. **What is a provider in Terraform?**

   > A provider is a plugin responsible for interacting with APIs of external platforms like AWS, Azure, etc.

3. **What is the use of Terraform state?**

   > It tracks the current state of infrastructure to detect configuration changes.

4. **What are modules in Terraform?**

   > Modules are reusable blocks of Terraform code used to organize and reuse code across projects.

5. **How do you manage multiple environments in Terraform?**

   > By using workspaces or separate backend configurations for each environment.

---

## 🔸 Interview Questions (Advanced)

1. **How does Terraform detect resource changes?**

   > It compares the desired state in `.tf` files with the current state stored in the `terraform.tfstate` file.

2. **What happens if you delete the state file?**

   > Terraform loses track of existing resources and may try to recreate them.

3. **What is the difference between `terraform plan` and `terraform apply`?**

   > `plan` shows what will be done; `apply` performs those changes.

4. **What is the role of the backend in Terraform?**

   > A backend determines where the state is stored and how it's accessed.

5. **Can you use Terraform to manage existing infrastructure?**

   > Yes, using `terraform import` to bring existing resources under Terraform management.

---

Would you like me to create **a PDF version** of these notes or break them into **separate files per concept** for easier study?
