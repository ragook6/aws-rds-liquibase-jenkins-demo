# Step 02 – CodeCommit Repo Setup

## Goal

Create an **AWS CodeCommit** repository and prepare a folder structure to store Liquibase change sets.

---

## 1. Create a CodeCommit Repository

1. In the AWS Console, go to **CodeCommit → Repositories → Create repository**.
2. Use values like:
   - **Repository name**: `DBDevopsDemoRepo`
   - **Description**: `Demo repo for Liquibase + Jenkins RDS deployment`
3. Click **Create**.

---

## 2. Clone the Repository Locally

From your local machine, using the HTTPS URL from the CodeCommit console:

```bash
git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/DBDevopsDemoRepo
cd DBDevopsDemoRepo
```

Replace `us-east-1` with your region if different.

If you haven’t configured CodeCommit credentials yet, follow AWS docs for:
- HTTPS Git credentials **or**
- Git over SSH configuration.

---

## 3. Create Liquibase Folder Structure

Inside the cloned repo:

```bash
mkdir -p liquibase
```

You’ll later place a Liquibase change set file here, for example:

```text
DBDevopsDemoRepo/
└─ liquibase/
   └─ changeset.sql
```

---

## 4. Initial Commit (Optional)

You can commit the empty folder structure:

```bash
touch liquibase/.gitkeep
git add liquibase/.gitkeep
git commit -m "Add liquibase folder structure"
git push origin master
```

---

## Next Step

Go to **Step 03 – EC2, Jenkins, and Liquibase Install**:  
`../03-ec2-jenkins-liquibase-install/README.md`
