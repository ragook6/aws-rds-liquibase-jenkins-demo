# Step 03 – EC2, Jenkins, and Liquibase Install

## Goal

Launch an EC2 instance that will run **Jenkins**, and install **Liquibase** and required tools on it.

---

## 1. Launch EC2 Instance

1. Go to **EC2 → Launch instance**.
2. Name: `jenkins-liquibase-demo`.
3. AMI: **Amazon Linux 2** (or latest Amazon Linux).
4. Instance type: `t3.small` or `t3.medium` is enough.
5. Key pair: select or create a key pair.
6. Network settings:
   - Create or select a security group that allows:
     - **SSH (22)** from your IP
     - **HTTP (80)** from your IP (for Jenkins via Nginx)
7. For now you can skip the IAM role (we add it later in Step 04).
8. Launch the instance.

---

## 2. Connect via SSH

From your local machine:

```bash
ssh -i /path/to/your-key.pem ec2-user@<EC2-Public-DNS>
```

---

## 3. Install Jenkins and Dependencies

Run on the EC2 instance:

```bash
sudo yum update -y

# Jenkins repo
sudo wget -O /etc/yum.repos.d/jenkins.repo   http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key

# Install Jenkins, Java, Git, jq, nginx
sudo yum install -y jenkins java-1.8.0-openjdk git jq nginx1
```

Enable and start services:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins

sudo systemctl enable nginx
sudo systemctl start nginx
```

---

## 4. Configure Nginx as Reverse Proxy to Jenkins

Edit nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

In the `http` block, set a simple server:

```nginx
server {
    listen       80;
    server_name  _;

    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}
```

Restart nginx:

```bash
sudo systemctl restart nginx
```

Now you should be able to open Jenkins at:

```text
http://<EC2-Public-DNS>/
```

---

## 5. Install Liquibase and PostgreSQL JDBC Driver

```bash
cd /var/lib/jenkins
sudo mkdir -p liquibase
cd liquibase
```

Download Liquibase (version example; adjust as needed):

```bash
wget https://github.com/liquibase/liquibase/releases/download/v4.27.0/liquibase-4.27.0.zip
unzip liquibase-4.27.0.zip
rm liquibase-4.27.0.zip
```

Download PostgreSQL JDBC driver:

```bash
wget https://jdbc.postgresql.org/download/postgresql-42.2.8.jar
```

You should now have:

```text
/var/lib/jenkins/liquibase/
  ├─ liquibase (binary)
  └─ postgresql-42.2.8.jar
```

---

## Next Step

Go to **Step 04 – IAM Role for Jenkins EC2**:  
`../04-iam-role-for-jenkins-ec2/README.md`
