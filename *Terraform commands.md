Here’s a **complete list of Terraform commands** — categorized for easy understanding.
Each command includes a **short description and common usage example.**

---

## 🧩 **1. Terraform Setup Commands**

| Command                           | Description                        | Example                           |
| --------------------------------- | ---------------------------------- | --------------------------------- |
| `terraform -version`              | Shows Terraform version            | `terraform -version`              |
| `terraform -help`                 | Lists all commands with short help | `terraform -help`                 |
| `terraform -install-autocomplete` | Enables auto-completion for shell  | `terraform -install-autocomplete` |

---

## ⚙️ **2. Initialization & Plugin Management**

| Command                            | Description                                                  | Example                                  |
| ---------------------------------- | ------------------------------------------------------------ | ---------------------------------------- |
| `terraform init`                   | Initializes working directory (downloads providers, modules) | `terraform init`                         |
| `terraform init -upgrade`          | Upgrades provider versions to latest                         | `terraform init -upgrade`                |
| `terraform providers`              | Lists all providers used in configuration                    | `terraform providers`                    |
| `terraform providers mirror <dir>` | Copies providers to a local directory                        | `terraform providers mirror ./providers` |

---

## 🧠 **3. Formatting, Validation, and Docs**

| Command                      | Description                                 | Example                            |
| ---------------------------- | ------------------------------------------- | ---------------------------------- |
| `terraform fmt`              | Formats Terraform files to standard style   | `terraform fmt`                    |
| `terraform fmt -recursive`   | Formats files in all subdirectories         | `terraform fmt -recursive`         |
| `terraform validate`         | Checks syntax and validity of configuration | `terraform validate`               |
| `terraform providers schema` | Displays available provider schemas         | `terraform providers schema -json` |

---

## 🧩 **4. Plan & Apply (Core Operations)**

| Command                                          | Description                                        | Example                                          |
| ------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------ |
| `terraform plan`                                 | Creates an execution plan without applying changes | `terraform plan`                                 |
| `terraform plan -out=tfplan`                     | Saves the plan to a file for later apply           | `terraform plan -out=tfplan`                     |
| `terraform apply`                                | Applies configuration changes                      | `terraform apply`                                |
| `terraform apply tfplan`                         | Applies from a saved plan file                     | `terraform apply tfplan`                         |
| `terraform destroy`                              | Destroys all managed infrastructure                | `terraform destroy`                              |
| `terraform destroy -target=aws_instance.example` | Destroys a specific resource                       | `terraform destroy -target=aws_instance.example` |

---

## 🧱 **5. State Management Commands**

| Command                           | Description                                        | Example                                                |
| --------------------------------- | -------------------------------------------------- | ------------------------------------------------------ |
| `terraform state list`            | Lists all resources in state file                  | `terraform state list`                                 |
| `terraform state show <resource>` | Shows details of a resource                        | `terraform state show aws_instance.myvm`               |
| `terraform state mv <src> <dest>` | Moves an item in the state                         | `terraform state mv aws_instance.old aws_instance.new` |
| `terraform state rm <resource>`   | Removes a resource from state (without destroying) | `terraform state rm aws_instance.unmanaged`            |
| `terraform state pull`            | Reads current remote state                         | `terraform state pull`                                 |
| `terraform state push`            | Updates remote state manually                      | `terraform state push terraform.tfstate`               |

---

## 🧩 **6. Output and Variables**

| Command                   | Description                                          | Example                        |
| ------------------------- | ---------------------------------------------------- | ------------------------------ |
| `terraform output`        | Shows output values                                  | `terraform output`             |
| `terraform output <name>` | Shows a specific output value                        | `terraform output instance_ip` |
| `terraform console`       | Opens an interactive console to evaluate expressions | `terraform console`            |
| `terraform console`       | Example: `> var.instance_type`                       |                                |

---

## 🧰 **7. Workspace Management**

| Command                             | Description             | Example                           |
| ----------------------------------- | ----------------------- | --------------------------------- |
| `terraform workspace list`          | Lists all workspaces    | `terraform workspace list`        |
| `terraform workspace new <name>`    | Creates a new workspace | `terraform workspace new dev`     |
| `terraform workspace select <name>` | Switches to a workspace | `terraform workspace select prod` |
| `terraform workspace show`          | Shows current workspace | `terraform workspace show`        |
| `terraform workspace delete <name>` | Deletes a workspace     | `terraform workspace delete old`  |

---

## 🔍 **8. Import and Graph**

| Command                            | Description                                       | Example                                              |
| ---------------------------------- | ------------------------------------------------- | ---------------------------------------------------- |
| `terraform import <resource> <id>` | Imports an existing resource into Terraform state | `terraform import aws_instance.example i-1234567890` |
| `terraform graph`                  | Generates a visual dependency graph               | `terraform graph > graph.dot`                        |

---

## 🪣 **9. Locking & State Backend**

| Command                            | Description                          | Example                           |
| ---------------------------------- | ------------------------------------ | --------------------------------- |
| `terraform force-unlock <LOCK_ID>` | Manually unlocks state file if stuck | `terraform force-unlock 1234abcd` |
| `terraform login`                  | Logs in to Terraform Cloud           | `terraform login`                 |
| `terraform logout`                 | Logs out of Terraform Cloud          | `terraform logout`                |

---

## 🧾 **10. Plan File & Metadata**

| Command                | Description                            | Example                            |
| ---------------------- | -------------------------------------- | ---------------------------------- |
| `terraform show`       | Shows details of current state or plan | `terraform show`                   |
| `terraform show -json` | Exports state or plan as JSON          | `terraform show -json > plan.json` |

---

## 🧹 **11. File & Directory Cleanup**

| Command                            | Description                                                   | Example              |
| ---------------------------------- | ------------------------------------------------------------- | -------------------- |
| `terraform refresh` *(Deprecated)* | Updates local state to match real resources                   | —                    |
| `terraform clean` *(Custom)*       | No built-in command — manually delete `.terraform/` directory | `rm -rf .terraform/` |

---

## 🧮 **12. Cloud & Automation**

| Command            | Description                                | Example                 |
| ------------------ | ------------------------------------------ | ----------------------- |
| `terraform login`  | Authenticate to Terraform Cloud/Enterprise | `terraform login`       |
| `terraform logout` | Logout from Terraform Cloud                | `terraform logout`      |
| `terraform cloud`  | Manage Terraform Cloud operations          | `terraform cloud -help` |

---

## 💡 **Quick Command Flow (Typical Workflow)**

```
terraform init
terraform fmt
terraform validate
terraform plan -out=tfplan
terraform apply tfplan
terraform output
terraform destroy
```

---

Would you like me to make this into a **one-page printable Terraform command cheat sheet (PDF or DOCX)** for your quick reference?
