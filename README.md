Deployed Eid Greetings DevOps Mini Project Using AWS Free Tier & GitHub Actions                              
(Step by Step guide) 🚀


📌 Project Overview
This project automates the deployment of an Eid greeting webpage using AWS Free Tier (EC2), GitHub Actions, and Apache. When the greeting.txt file is updated in the GitHub repository, GitHub Actions automatically deploys the new greeting to the EC2 instance.

🔧 Tech Stack
AWS EC2 (Free Tier) - Hosting the website

Apache Web Server - Serving the webpage

GitHub Actions - Automating the deployment process

Bash & Linux - Automating file updates


🛠️ Step-by-Step Implementation
1️⃣ Launch an EC2 Instance (Free Tier)
Go to AWS Console → EC2 → Launch Instance.

Choose Amazon Linux 2 (or Ubuntu if preferred).

Select t2.micro (Free Tier eligible).

Configure Security Group:

 Allow HTTP & SSH

Launch and download the key pair (.pem file).

Connect to the instance:

ssh -i your-key.pem ec2-user@your-public-ip

2️⃣ Install & Configure Apache
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

Set permissions:

sudo chown -R ec2-user:ec2-user /var/www/html


3️⃣ Set Up GitHub Repository
Create a new GitHub repository.

Clone it on your local machine:

git clone https://github.com/your-username/eid-greetings-devops.git
cd eid-greetings-devops

4️⃣ Create the Web Files
Inside your cloned repo, create:

📜 index.html (Main Page)
<!DOCTYPE html>
<html>
<head>
    <title>Eid Mubarak Greetings</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; margin-top: 50px; }
        h1 { color: #27ae60; }
    </style>
</head>
<body>
    <h1 id="greeting">Loading...</h1>
    <script>
        fetch('greeting.txt')
        .then(response => response.text())
        .then(data => document.getElementById('greeting').innerText = data);
    </script>
</body>
</html>


📜 greeting.txt (Message File)
Eid Mubarak! May your deployments always be successful! 🎉
Now commit and push the files to GitHub:

git add .
git commit -m "Initial Eid greeting page"
git push origin main

5️⃣ Set Up GitHub Actions for Automated Deployment
Inside your repo, create .github/workflows/deploy.yml.

📜 Create a GitHub Actions Workflow

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
✅ This script automatically updates your EC2 instance whenever you push a new greeting.


6️⃣ Set Up GitHub Secrets for Secure Access
Add SSH Key to GitHub Secrets:

Go to your repo → Settings → Secrets → Actions → New repository secret.

SSH_PRIVATE_KEY → Paste your EC2 .pem key content

EC2_PUBLIC_IP → Your EC2 instance's public IP

✅ Now, GitHub Actions can securely deploy your updates!


7️⃣ Test the CI/CD Pipeline
Edit greeting.txt and update the message:

echo -e " عيد مبارك! \nنسأل الله أن يبارك لك هذا العيد ويمنحك السعادة والسلام.\nنتمنى لك نشرات خالية من الأخطاء وعمليات نشر ناجحة دائمًا! 🚀\n\n\n Eid Mubarak! \nMay Allah bless you with joy, peace, and prosperity this Eid.\nWishing you error-free logs and successful deployments always! 🚀" > greeting.txt
Commit and push:

git add .
git commit -m "Updated Eid message"
git push origin main
GitHub Actions will run automatically, updating your EC2-hosted webpage.

Open http://your-public-ip/ in a browser to see the updated greeting.


🚀 Project Benefits :
✅ Automated Deployment using GitHub Actions 

✅ AWS Free Tier Usage - No cost involved 

✅ Simple CI/CD pipeline for real-world experience 

✅ Scalable & Customizable for future automation





🚀 Final Results :


Minimize image
Edit image
Delete image

🌙 Happy Eid & Happy Coding! 💖
