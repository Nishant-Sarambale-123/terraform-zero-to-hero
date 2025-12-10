Here is a **clean, short, notebook-friendly notes + diagram** for DynamoDB locking.

---

# 📘 **Terraform DynamoDB Locking — Notes (Easy & Short)**

### **Why lock?**

To prevent **two people** from changing the **same Terraform state** at the same time.

---

# 🔒 **How the lock works**

1. You run **terraform plan/apply**
2. Terraform creates a **lock item** in DynamoDB
   → Means: *“State is busy, don’t touch”*
3. If someone else runs Terraform → they get **lock error**
4. When Terraform completes, it **deletes the lock**
5. If Terraform crashes → lock stays → use `terraform force-unlock`

---

# 🧱 **What DynamoDB stores**

A simple item like:

```
LockID: terraform-state
Info: who/when
```

---

# 🎯 **One-line summary**

**“Terraform puts a lock record in DynamoDB so only one apply can run at a time.”**

---

# 🖼️ **ASCII Diagram (Very Simple)**

```
          +---------------------------+
          |   terraform apply         |
          +------------+--------------+
                       |
                       v
           +-----------------------+
           | Create Lock in        |
           | DynamoDB Table        |
           | (LockID item)         |
           +----------+------------+
                      |
        +-------------+--------------+
        |                            |
        v                            v
 Other User Runs              Current User Runs
 terraform apply              terraform apply
        |                            |
        v                            v
  +------------+           +----------------------+
  | FAIL: Lock |           | Continue…            |
  | Exists     |           | State operations     |
  +------------+           +----------------------+

                       |
                       v
           +------------------------+
           | Delete Lock Item       |
           | Terraform Finished      |
           +------------------------+
```

---

If you want, I can also prepare **Terraform backend S3 + DynamoDB notebook notes** or **handwritten-style notes**.



Here is the **simple and interview-ready explanation** of how **Terraform DynamoDB backend locking** works:

---

# ✅ **How Terraform Locking Works in DynamoDB**

When you use S3 + DynamoDB backend:

```hcl
terraform {
  backend "s3" {
    bucket         = "tf-state-bucket"
    key            = "prod/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-lock"
  }
}
```

Terraform uses **DynamoDB** to create a **lock** so that **two people cannot run `terraform apply` at the same time**.

---

# 🔒 **What Exactly Gets Stored in DynamoDB?**

Terraform writes **one item** in the DynamoDB table:

### Example DynamoDB item:

| Attribute                  | Value                                        |
| -------------------------- | -------------------------------------------- |
| **LockID** (Partition Key) | `"terraform-prod/terraform.tfstate-md5hash"` |
| **Info**                   | JSON string containing who took the lock     |
| **Operation**              | `"OperationTypeApply"`                       |
| **Who**                    | `"nishant@laptop"`                           |
| **Version**                | Terraform version                            |
| **Created**                | Timestamp                                    |

This entry means:

👉 “Terraform state is currently locked. Someone is running `plan` or `apply`.”

---

# 🔁 **Step-by-Step: How the Lock Works**

## **1️⃣ You run `terraform plan` / `apply`**

Terraform tries to create a lock record in DynamoDB using:

```
PutItem with ConditionExpression: attribute_not_exists(LockID)
```

This means:

✔ Create lock
❌ Fail if lock already exists

If the lock exists → Terraform shows:

```
Error acquiring the state lock
```

---

## **2️⃣ Terraform Applies Your Changes**

While lock exists:

* Nobody else can run `apply`
* Prevents state corruption
* Ensures consistency

---

## **3️⃣ Terraform Finishes → Lock Released**

Terraform deletes the lock entry from DynamoDB:

```
DeleteItem where LockID = "<same>"
```

Now the state is **unlocked** and others can apply.

---

# 🔧 **If Something Goes Wrong**

If Terraform crashes → lock stays.

You manually remove:

```sh
terraform force-unlock <LOCK_ID>
```

---

# 🎯 **Interview-Perfect Short Answer**

**“Terraform uses DynamoDB for state locking. It writes a lock item into the DynamoDB table using a conditional write. As long as this item exists, other Terraform operations cannot proceed. Once Terraform completes, it deletes the record, releasing the lock.”**

---

# ⭐ Want a diagram or notes version for your notebook?
