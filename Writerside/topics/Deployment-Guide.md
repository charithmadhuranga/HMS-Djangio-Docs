# Deployment Guide

## **Method 1: Deploying on a VPS (DigitalOcean, AWS EC2, etc.)**

### **1. Set Up the Server**
- Provision a new **Ubuntu 20.04+** server.
- Update packages and install dependencies:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip python3-venv nginx git
```

### **2. Clone the Project & Set Up Virtual Environment**
```bash
git clone https://github.com/your-repository/doctor-channeling-system.git
cd doctor-channeling-system
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### **3. Configure Gunicorn & Supervisor**
```bash
pip install gunicorn
```
Create a **Supervisor config**:
```bash
sudo nano /etc/supervisor/conf.d/hms_prj.conf
```
Add:
```ini
[program:hms_prj]
command=/path/to/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 hms_prj.wsgi:application
directory=/path/to/project
user=youruser
autostart=true
autorestart=true
stderr_logfile=/var/log/hms_prj.err.log
stdout_logfile=/var/log/hms_prj.out.log
```
Restart Supervisor:
```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start hms_prj
```

### **4. Configure Nginx as a Reverse Proxy**
```bash
sudo nano /etc/nginx/sites-available/hms_prj
```
Add:
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```
Activate & Restart:
```bash
sudo ln -s /etc/nginx/sites-available/hms_prj /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx
```

### **5. Secure the Server with SSL (Let's Encrypt)**
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

---


# **Method 2: Deploying to AWS (Elastic Beanstalk, RDS, and S3)**

## **1. Install AWS CLI and Elastic Beanstalk CLI**
```bash
pip install awsebcli --upgrade --user
```

## **2. Configure AWS Credentials**
```bash
aws configure
```
Enter your AWS **Access Key, Secret Key, Region**, and **Output Format**.

## **3. Set Up AWS Resources**

### **Create an RDS PostgreSQL Database**
- Go to the **AWS RDS Console**.
- Create a new **PostgreSQL** database.
- Configure security groups to allow access from your EC2 instance.
- Copy the **database endpoint** for later use.

### **Create an S3 Bucket for Media Files**
- Go to the **AWS S3 Console**.
- Create a new bucket (e.g., `my-hms-media`).
- Enable public access if required.
- Copy the bucket name for later use.

## **4. Update `.env` for Production**
```ini
SECRET_KEY=your_production_secret_key
DEBUG=False
ALLOWED_HOSTS=.elasticbeanstalk.com,yourdomain.com
DATABASE_URL=postgres://username:password@your-rds-endpoint:5432/dbname
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_STORAGE_BUCKET_NAME=my-hms-media
EMAIL_BACKEND=anymail.backends.resend.EmailBackend
RESEND_API_KEY=your_resend_api_key
RESEND_SENDER_DOMAIN=your_resend_sender_domain
DEFAULT_FROM_EMAIL=your_default_email@example.com
```

you have to customize default setting in setting.py file accordingly for the postgres database as default is using sqlite db
## **5. Configure Django for S3 Storage**
Install required packages:
```bash
pip install boto3 django-storages
```
Update `settings.py`:
```python
INSTALLED_APPS += ['storages']

DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
AWS_STORAGE_BUCKET_NAME = os.getenv('AWS_STORAGE_BUCKET_NAME')
AWS_ACCESS_KEY_ID = os.getenv('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = os.getenv('AWS_SECRET_ACCESS_KEY')
```

## **6. Initialize Elastic Beanstalk**
```bash
eb init -p python-3.8 my-hms-app --region your-region
```

## **7. Deploy to Elastic Beanstalk**
```bash
eb create my-hms-env
```
Wait for deployment to complete.

## **8. Apply Migrations and Collect Static Files**
```bash
eb ssh my-hms-env
source venv/bin/activate
python manage.py migrate
python manage.py collectstatic --noinput
```

## **9. Set Environment Variables on Beanstalk**
```bash
eb setenv SECRET_KEY=your_secret_key DATABASE_URL=your_database_url \
AWS_ACCESS_KEY_ID=your_aws_access_key AWS_SECRET_ACCESS_KEY=your_aws_secret_key \
AWS_STORAGE_BUCKET_NAME=my-hms-media STRIPE_SECRET_KEY=your_stripe_key STRIPE_PUBLIC_KEY=your_stripe_public_key
```

## **10. Update Elastic Beanstalk with New Code**
```bash
eb deploy
```

## **11. Monitor Logs and Restart If Needed**
```bash
eb logs
eb restart
```

## **12. Secure the Application with HTTPS**
- Go to **AWS Certificate Manager** and request an SSL certificate.
- Attach it to the **Elastic Load Balancer** in **Elastic Beanstalk**.

Your application is now deployed on AWS using **Elastic Beanstalk, RDS, and S3** 🚀

---

This documentation provides a **detailed breakdown** of the system components, local setup, and AWS deployment process. Let me know if you need further refinements! 🚀




