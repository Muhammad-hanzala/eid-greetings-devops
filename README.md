  # Eid Greetings DevOps Mini Project Using AWS Free Tier & GitHub Actions 🚀

## 📌 **Project Overview**

This project automates the deployment of an Eid greeting webpage using **AWS Free Tier (EC2), GitHub Actions, and Apache**. When the `greeting.txt` file is updated in the GitHub repository, **GitHub Actions automatically deploys the new greeting** to the EC2 instance.

 🔧 **Tech Stack**

- **AWS EC2 (Free Tier)** - Hosting the website
- **Apache Web Server** - Serving the webpage
- **GitHub Actions** - Automating the deployment process
- **Bash & Linux** - Automating file updates

---

## 🛠️ **Step-by-Step Implementation**

### **1️⃣ Launch an EC2 Instance (Free Tier)**

1. Go to [AWS Console](https://aws.amazon.com/) → EC2 → Launch Instance.
2. Choose **Amazon Linux 2** (or Ubuntu if preferred).
3. Select **t2.micro** (Free Tier eligible).
4. Configure Security Group:
   - Allow **HTTP (80)** from `0.0.0.0/0`
   - Allow **SSH (22)** from your IP
5. Launch and **download the key pair** (`.pem` file).
6. Connect to the instance:
   ```bash
   ssh -i your-key.pem ec2-user@your-public-ip
   ```

### **2️⃣ Install & Configure Apache**

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

Set permissions:

```bash
sudo chown -R ec2-user:ec2-user /var/www/html
```

---

### **3️⃣ Set Up GitHub Repository**

1. Create a **new GitHub repository**.
2. Clone it on your local machine:
   ```bash
   git clone https://github.com/your-username/eid-greetings-devops.git
   cd eid-greetings-devops
   ```
3. Create a **greeting.txt** file:
   ```bash
   echo -e "💖 عيد مبارك! 💖\nنسأل الله أن يبارك لك هذا العيد ويمنحك السعادة والسلام.\nنتمنى لك نشرات خالية من الأخطاء وعمليات نشر ناجحة دائمًا! 🚀\n\n\n💖 Eid Mubarak! 💖\nMay Allah bless you with joy, peace, and prosperity this Eid.\nWishing you error-free logs and successful deployments always! 🚀" > greeting.txt
   ```
4. Commit and push:
   ```bash
   git add greeting.txt
   git commit -m "Initial Eid greeting with Arabic and English"
   git push origin main
   ```

---

### **4️⃣ Set Up GitHub Actions for Automated Deployment**

1. Inside your repo, create `.github/workflows/deploy.yml`.
2. Add the following content:
   ```yaml
   name: Deploy Eid Greeting

   on:
     push:
       branches:
         - main

   jobs:
     deploy:
       runs-on: ubuntu-latest

       steps:
       - name: Checkout code
         uses: actions/checkout@v3

       - name: Deploy to EC2
         run: |
           ssh -o StrictHostKeyChecking=no ec2-user@your-public-ip << 'EOF'
             cd /var/www/html
             echo "$(cat greeting.txt)" > index.html
           EOF
         env:
           SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
   ```
3. **Add SSH Key to GitHub Secrets:**
   - Go to your repo → Settings → Secrets → Actions → **New repository secret**.
   - Name: `EC2_SSH_KEY`
   - Value: **Your private key content (****`.pem`**** file)**

---

### **5️⃣ Test the CI/CD Pipeline**

1. Edit `greeting.txt` and update the message:
   ```bash
   echo -e "💖 عيد مبارك! 💖\nنسأل الله أن يبارك لك هذا العيد ويمنحك السعادة والسلام.\nنتمنى لك نشرات خالية من الأخطاء وعمليات نشر ناجحة دائمًا! 🚀\n\n\n💖 Eid Mubarak! 💖\nMay Allah bless you with joy, peace, and prosperity this Eid.\nWishing you error-free logs and successful deployments always! 🚀" > greeting.txt
   ```
2. Commit and push:
   ```bash
   git add greeting.txt
   git commit -m "Updated Eid greeting"
   git push origin main
   ```
3. **GitHub Actions will run automatically**, updating your EC2-hosted webpage.
4. Open **[http://your-public-ip/](http://your-public-ip/)** in a browser to see the updated greeting.

---

## 🚀 **Project Benefits**

✅ **Automated Deployment** using GitHub Actions
✅ **AWS Free Tier Usage** - No cost involved
✅ **Simple CI/CD pipeline for real-world experience**
✅ **Scalable & Customizable** for future automation

🌙 **Happy Eid & Happy Coding!** 💖


