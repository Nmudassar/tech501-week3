# AWS Cloud Deployment: Two-Tier Architecture

## **1. Prerequisites**

- AWS Account with IAM access.
- SSH Key Pair to securely access EC2 instances.

### **Create SSH Key Pair**

1. Go to **AWS Console** → **EC2**.
2. Select **Key Pairs** → **Create Key Pair**.
3. **Name:** `my-key-pair`
4. **Format:** `.pem`
5. Click **Create & Download**.
6. Securely store the `.pem` file and update permissions:
   ```sh
   chmod 400 my-key-pair.pem
   ```

---

## **2. Deploy Database VM (MongoDB)**

### **Launch EC2 Instance**

1. **Region:** Ireland (eu-west-1)
2. **AMI:** Ubuntu 22.04 LTS
3. **Instance Type:** `t3.micro`
4. **Key Pair:** Select `my-key-pair.pem`
5. **Networking:**
   - **VPC:** Default
   - **Subnet:** Default subnet (Ireland)
   - **Security Group:**
     - Allow **SSH (22)** from `0.0.0.0/0`
     - Allow **MongoDB (27017)** from the **App VM Private IP** only
6. **Launch Instance**

![AWS Database
](<images/Screenshot 2025-02-06 071823.png>)

### **Install MongoDB**

1. **SSH into DB VM:**
   ```sh
   ssh -i my-key-pair.pem ubuntu@<DB_PUBLIC_IP>
   ```
2. **Install MongoDB:**
   ```sh
   sudo apt update
   sudo apt install -y mongodb
   ```
3. **Allow Remote Access:**
   ```sh
   sudo nano /etc/mongod.conf
   ```
   Change:
   ```makefile
   bindIp: 127.0.0.1
   ```
   To:
   ```makefile
   bindIp: 0.0.0.0
   ```
4. **Restart MongoDB:**
   ```sh
   sudo systemctl restart mongod
   ```

---

## **3. Deploy App VM (Node.js App)**

### **Launch EC2 Instance**

1. **AMI:** Ubuntu 22.04 LTS
2. **Instance Type:** `t3.micro`
3. **Key Pair:** Select `my-key-pair.pem`
4. **Networking:**
   - **VPC:** Default
   - **Subnet:** Default subnet (Ireland)
   - **Security Group:**
     - Allow **SSH (22)** from `0.0.0.0/0`
     - Allow **HTTP (80)**
     - Allow **Custom TCP (3000)**
5. **Launch Instance**

![AWS app 
](<images/Screenshot 2025-02-06 071753.png>)

### **Provision the App VM**

1. **SSH into App VM:**
   ```sh
   ssh -i my-key-pair.pem ubuntu@<APP_PUBLIC_IP>
   ```
2. **Install Dependencies:**
   ```sh
   sudo apt update && sudo apt upgrade -y
   sudo apt install nginx -y
   sudo apt install -y nodejs npm
   sudo npm install -g pm2
   ```
3. **Clone Node.js App:**
   ```sh
   git clone https://github.com/sameem97/tech501-sparta-app.git
   cd tech501-sparta-app
   ```
4. **Configure Reverse Proxy for Nginx:**
   ```sh
   sudo nano /etc/nginx/sites-available/default
   ```
   Replace:
   ```nginx
   try_files $uri $uri/ =404;
   ```
   With:
   ```nginx
   proxy_pass http://localhost:3000;
   ```
5. **Reload Nginx:**
   ```sh
   sudo systemctl reload nginx
   ```
6. **Set Environment Variables:**
   ```sh
   export DB_HOST=mongodb://<DB_PRIVATE_IP>:27017/posts
   ```
7. **Start Node.js App:**
   ```sh
   pm2 start app.js --name node-app
   pm2 startup
   pm2 save
   ```

---

## **4. Test the Application**

1. Open `http://<APP_PUBLIC_IP>`.
2. Navigate to `http://<APP_PUBLIC_IP>/posts`.
3. If it doesn't work, check:
   ```sh
   pm2 logs node-app
   sudo systemctl status nginx
   ```

---

## **Final Notes**

- This setup **secures MongoDB** by allowing access **only from the App VM**.
- The **App is publicly accessible via Nginx**, while **MongoDB is hidden**.
- Use **AWS Security Groups** instead of exposing MongoDB to the internet.
- For production, consider **Elastic Load Balancing (ELB)** and **Auto Scaling**.

🚀 **AWS CI/CD automation coming soon!**
