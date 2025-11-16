# **Step 02 – GitHub Repo Setup**

*(Replaces the old CodeCommit step because new AWS accounts cannot create CodeCommit repos.)*

## **Goal**

Create a **GitHub repository** that will store all Liquibase changesets and any supporting scripts/config files.
Jenkins will later pull this repo to run Liquibase **update** and **rollback** operations on RDS/Aurora.

---

## **1. Create a New GitHub Repository**

1. Go to:
   **[https://github.com/new](https://github.com/new)**

2. Enter repository name:

   ```
   DBDevopsDemoRepo
   ```

3. Description (optional):

   ```
   This repo is for demo of DB deployment automation using Liquibase + Jenkins + RDS.
   ```

4. Repository type:

   * **Private** (recommended for any real database work)

5. Do **NOT** initialize with:

   * README
   * .gitignore
   * License

   (We will push our own files.)

6. Click **Create repository**.

---

## **2. Clone the GitHub Repo to Your Local Machine**

In your terminal:

```bash
git clone https://github.com/<YOUR-GITHUB-USERNAME>/DBDevopsDemoRepo.git
cd DBDevopsDemoRepo
```

> Replace `<YOUR-GITHUB-USERNAME>` with your GitHub account or organization name.

---

## **3. Create Folder Structure for Liquibase Files**

Create a folder to hold all Liquibase change sets:

```bash
mkdir -p liquibase
```

Your project structure now looks like:

```
DBDevopsDemoRepo/
└── liquibase/
```

(Optional) Add a `.gitkeep` file to ensure the folder appears in GitHub:

```bash
touch liquibase/.gitkeep
```

---

## **4. Initial Commit & Push**

Add the new folder and push the first commit:

```bash
git add .
git commit -m "Initial structure: added liquibase folder"
git push origin main
```

If your default branch is `master`, use:

```bash
git push origin master
```

---

## **5. (Optional) Configure GitHub Credentials for Jenkins**

Later in Step 08 (Jenkins job), Jenkins needs access to GitHub.
You will choose ONE of these two authentication methods:

### **Option A – GitHub Personal Access Token (recommended)**

1. Go to → GitHub
   **Settings → Developer Settings → Personal Access Tokens → Tokens (classic)**
2. Click **Generate new token**.
3. Select:

   * `repo` → Full control of private repositories
4. Copy the token.
5. In Jenkins:

   * Go to **Manage Jenkins → Credentials**
   * Add new credentials:

     * Type: `Username with password`
     * Username: your GitHub username
     * Password: your GitHub token

### **Option B – SSH Key Authentication**

1. Generate SSH key on Jenkins or EC2 instance.
2. Add public key to GitHub → Settings → SSH and GPG keys.
3. Use SSH repo URL:

   ```
   git@github.com:<your-account>/DBDevopsDemoRepo.git
   ```
4. Jenkins will use the private key stored in its credentials store.

---

## **6. Your Repo Is Now Ready**

Jenkins will later:

* Clone this GitHub repository
* Read the file `liquibase/changeset.sql`
* Execute Liquibase update/rollback commands
* Track changes in your RDS/Aurora DB

---

## **Next Step**

Go to:
**Step 03 – EC2, Jenkins, and Liquibase Install**
`../03-ec2-jenkins-liquibase-install/README.md`

---


