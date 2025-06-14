## VisionApp_Deployment Script
This guide explains how to deploy the **Vision** Node.js application on a Linux server using a dedicated user and a `systemd` service to ensure it runs persistently and starts automatically on boot.
---

```bash
# 📁 Step 1: Move Project to Server
# On your local machine
scp -r vision user@server_ip:path

# 📦 Step 2: Move Project to /projects
# On the server
mv path/vision /projects/

# 👤 Step 3: Create a New User
adduser vision

# 🔐 Step 4: Set Ownership and Permissions
chown -R vision:vision /projects/vision/
chmod -R 750 /projects/vision/

# 📂 Step 5: Install Node.js Dependencies
cd /projects/vision/
su vision
npm install
exit

# ⚙️ Step 6: Create systemd Service File
sudo vi /lib/systemd/system/vision.service

```ini
[Service]
Type=simple
User=vision
WorkingDirectory=/projects/vision/
ExecStart=npm run start -- --port=3000
Restart=on-failure

# 🔄 Step 7: Enable and Start the Service
Reload systemd to regconize changes
sudo systemctl daemon-reload

Start the Vision app
sudo systemctl start vision

Enable it to start on boot
sudo systemctl enable vision

Check status
sudo systemctl status vision
