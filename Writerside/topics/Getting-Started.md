# Getting Started


## **Setting Up a Local Development Environment**

### **1. Prerequisites**
Ensure you have the following installed:
- Python (>= 3.8)
- pip (Python package manager)
- Virtualenv
- SQLite (default database) or PostgreSQL (optional)

### **2. Clone the Repository**
```bash
git clone https://github.com/your-repository/doctor-channeling-system.git
cd doctor-channeling-system
```

### **3. Create a Virtual Environment**
```bash
python -m venv venv
source venv/bin/activate  # On macOS/Linux
venv\Scripts\activate    # On Windows
```

### **4. Install Dependencies**
```bash
pip install -r requirements.txt
```

### **5. Configure Environment Variables**
Create a `.env` file in the root directory and add the following:
```ini
# Django Secret Key
SECRET_KEY=your_django_secret_key

# Debug Mode (Set to False in Production)
DEBUG=True

# Allowed Hosts
ALLOWED_HOSTS=localhost,127.0.0.1

# Stripe API Keys
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_PUBLIC_KEY=your_stripe_public_key

# PayPal API Credentials
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_SECRET_ID=your_paypal_secret_id

# Email Configuration (Mailgun, Resend, or another provider)
EMAIL_BACKEND=anymail.backends.mailgun.EmailBackend
MAILGUN_API_KEY=your_mailgun_api_key
MAILGUN_SENDER_DOMAIN=your_mailgun_sender_domain
DEFAULT_FROM_EMAIL=your_default_email@example.com
```

### **6. Apply Migrations & Create Superuser**
```bash
python manage.py migrate
python manage.py createsuperuser
```
Follow the prompts to create an admin user.

### **7. Run the Development Server**
```bash
python manage.py runserver
```
Visit `http://127.0.0.1:8000/` in your browser.

### **8. Access Admin Panel**
```bash
http://127.0.0.1:8000/admin/
```
Log in with the superuser credentials you created earlier.

---
