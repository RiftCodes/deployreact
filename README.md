# Deploying a React-Redux Frontend App on Your Server

Since your **React + Redux app** is a purely frontend application with **Material-UI** and no backend, you can serve it as a **static site** using NGINX. This guide provides a step-by-step approach to deploying your app on your server.

---

## **Step 1: Access Your Server**
If your site is on a remote server, connect to it via SSH:
```sh
ssh your-user@your-server-ip
```
For example:
```sh
ssh ubuntu@192.168.1.100
```
Replace `your-user` and `your-server-ip` with your actual username and server IP.

---

## **Step 2: Install Node.js and npm**
Check if Node.js is installed:
```sh
node -v
npm -v
```
If not installed, install Node.js (LTS version):
```sh
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```
Verify installation:
```sh
node -v
npm -v
```

---

## **Step 3: Install Git (Optional)**
If your project is hosted on **GitHub**, install Git:
```sh
sudo apt install git -y
```
Verify installation:
```sh
git --version
```

---

## **Step 4: Upload Your React App**
### **Option 1: Clone from GitHub (Recommended)**
If your project is on GitHub, pull it into your server:
```sh
cd /var/www
git clone https://github.com/your-username/your-repo.git my-react-app
```
Replace `your-username/your-repo` with your actual GitHub repository.
Then move into your project folder:
```sh
cd my-react-app
```

### **Option 2: Manually Upload Files**
If you don’t use Git, you can **upload files** using **SFTP/SCP**:
```sh
scp -r /local/path/to/your-project your-user@your-server-ip:/var/www/my-react-app
```

---

## **Step 5: Install Dependencies**
Navigate to your project folder:
```sh
cd /var/www/my-react-app
```
Install dependencies:
```sh
npm install
```

---

## **Step 6: Build the React App**
To generate a production-ready build, run:
```sh
npm run build
```
This creates a `build/` directory containing optimized static files.

---

## **Step 7: Install and Configure NGINX**
NGINX is used as a **reverse proxy** to serve your React app.

### **1️⃣ Install NGINX**
```sh
sudo apt install nginx -y
```
Verify installation:
```sh
nginx -v
```

### **2️⃣ Configure NGINX for React**
Create a new NGINX configuration file:
```sh
sudo nano /etc/nginx/sites-available/react-app
```
Add the following configuration:
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/my-react-app/build;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;
}
```
Replace `yourdomain.com` with your actual domain.

### **3️⃣ Enable the NGINX Configuration**
Create a symlink to enable this configuration:
```sh
sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/
```
Check for syntax errors:
```sh
sudo nginx -t
```
If everything is correct, restart NGINX:
```sh
sudo systemctl restart nginx
```
Allow firewall access for NGINX:
```sh
sudo ufw allow 'Nginx Full'
```

---

## **Step 8: Secure Your Site with HTTPS (Optional)**
If you have a **domain name**, you can enable free SSL using **Let's Encrypt**.

### **1️⃣ Install Certbot**
```sh
sudo apt install certbot python3-certbot-nginx -y
```

### **2️⃣ Generate an SSL Certificate**
```sh
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
Follow the on-screen instructions.

### **3️⃣ Auto-Renew SSL**
Ensure your SSL certificate is auto-renewed:
```sh
sudo certbot renew --dry-run
```

---

## **Step 9: Restart Everything & Test**
Ensure everything is running:
```sh
sudo systemctl restart nginx
```

### **Access Your Site**
- **If using a domain:** Open `https://yourdomain.com`
- **If using an IP:** Open `http://your-server-ip`

---

## **🎉 Your React Website is Live! 🚀**
Your **React & Redux website** is now fully deployed and accessible!
