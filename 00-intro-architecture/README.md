# Step 00 – Introduction & Architecture

## Goal

Understand what we are building and how the components fit together before touching any infrastructure.

## What You Will Build

An automated database change pipeline that:

- Stores database schema changes as **Liquibase** change sets in **AWS CodeCommit**
- Uses **Jenkins** running on an **EC2** instance to:
  - Pull changes from CodeCommit
  - Fetch database credentials from **AWS Secrets Manager**
  - Run **Liquibase** commands to:
    - Apply schema changes (**update**)
    - Roll back schema changes (**rollbackCount**)
- Sends email notifications using **Amazon SES**
- Applies changes to an **Amazon Aurora PostgreSQL (or RDS PostgreSQL)** database

## High-Level Architecture (Text)

1. Developer commits a Liquibase-formatted SQL file to CodeCommit.
2. Jenkins (on EC2) runs a job:
   - Uses the EC2 instance profile (IAM role) to access:
     - CodeCommit (to pull repo)
     - Secrets Manager (to get DB credentials)
   - Executes Liquibase with the right JDBC URL, username, and password.
3. Liquibase:
   - Applies schema changes to the target database.
   - Updates the `databasechangelog` table to track applied changes.
4. A separate Jenkins job can call Liquibase with `rollbackCount` to undo one or more changes.

## Tools and Services

- **AWS**
  - Amazon RDS / Aurora PostgreSQL
  - Amazon EC2
  - AWS CodeCommit
  - AWS Secrets Manager
  - Amazon SES
  - IAM (roles and policies)
- **Open Source**
  - Liquibase
  - Jenkins
- **Others**
  - Git
  - psql (PostgreSQL client)

## Next Step

Proceed to **Step 01 – Prerequisites & Secrets Manager**:
`../01-prereqs-and-secrets-manager/README.md`
