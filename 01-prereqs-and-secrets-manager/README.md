# Step 01 – Prerequisites & Secrets Manager

## Goal

Make sure your AWS and database prerequisites are ready and store database credentials in **AWS Secrets Manager** for secure access from Jenkins.

---

## 1. Prerequisites Checklist

You should have:

1. An **AWS account** with permissions to:
   - Create RDS / Aurora
   - Use IAM, Secrets Manager, EC2, SES, CodeCommit
2. A **PostgreSQL-compatible database**:
   - Amazon Aurora PostgreSQL **or**
   - Amazon RDS for PostgreSQL
3. A database **user** with permission to:
   - Create tables
   - Drop tables
   - Read and write to tables
4. On your **local machine**:
   - AWS CLI installed and configured (`aws configure`)
   - Git installed

If you already have a suitable Aurora/RDS instance, you can reuse it.  
If not, create one (single-AZ, small instance is fine for demo).

---

## 2. Create a Secret in AWS Secrets Manager

We will store the DB username, password, and optionally host/port as a JSON secret.

1. Go to **AWS Console → Secrets Manager → Store a new secret**.
2. **Secret type**:  
   `Credentials for RDS database`.
3. Enter your database credentials:
   - **Username**: your DB user (for example `dbdevopsuser`)
   - **Password**: your DB user password
4. Click **Next**.

On the next page:

5. **Secret name** – use something like:  
   `dbdevopsAuroraCreds`
6. (Optional) Add a description, for example:  
   `Credentials for Liquibase demo on Aurora PostgreSQL`
7. You may enable automatic rotation (optional for this demo).
8. Click **Next → Next → Store**.

---

## 3. Add Extra Fields to the Secret (Optional but Helpful)

If you want to keep **host**, **port**, and **dbname** in the same secret:

1. After creating the secret, open it in Secrets Manager.
2. Click **Edit → Edit secret value**.
3. Change the JSON to something like:

```json
{
  "username": "dbdevopsuser",
  "password": "SuperSecurePassword123",
  "host": "your-aurora-endpoint.amazonaws.com",
  "port": "5432",
  "dbname": "devopsdb"
}
```

4. Save changes.

We will parse these fields using `aws secretsmanager get-secret-value` and `jq` in later steps.

---

## 4. Test Access via AWS CLI (Optional but Recommended)

From your local machine:

```bash
aws secretsmanager get-secret-value   --secret-id dbdevopsAuroraCreds   --region us-east-1
```

You should see a JSON response containing `SecretString`.

---

## Next Step

Go to **Step 02 – CodeCommit Repo Setup**:  
`../02-codecommit-repo-setup/README.md`
