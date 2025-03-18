# Getting Started


# Initial Setup  Guide

## Step 1 : using uv package manager or pip (macos and linux)

using uv package manager
```
uv venv

. .venv/bin/activate

##for windows you only have to change the virutlenv activating method to .\venv\Scripts\activate

uv sync 
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser 
```

using pip package manager

```
pip3 install virtualenv

virtualenv .venv

. .venv/bin/activate

##for windows you only have to change the virutlenv activating method to .\venv\Scripts\activate

pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser 
```


## Step 2 :  create .env file and add all necessary api keys.Use .env.example as a base.

```ini
# Django Secret Key
SECRET_KEY=your_django_secret_key

# Debug Mode (Set to False in Production)
DEBUG=True

# Allowed Hosts
ALLOWED_HOSTS=localhost,127.0.0.1
CSRF_TRUSTED_ORIGINS=https://*.mydomain.com,https://*.127.0.0.1

# Stripe API Keys
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_PUBLIC_KEY=your_stripe_public_key

# PayPal API Credentials
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_SECRET_ID=your_paypal_secret_id

# Email Configuration (Mailgun, Resend, or another provider)
RESEND_API_KEY=
EMAIL_BACKEND=anymail.backends.resend.EmailBackend
FROM_EMAIL=onboarding@resend.dev
DEFAULT_FROM_EMAIL=onboarding@resend.dev
SERVER_EMAIL=onboarding@resend.dev
```




```
cp .env.example .env

#edit above .env file and add api keys for email backend,stripe,paypal and allowdhost,csrf trusted host accordingly

python manage.py runserver

```


## Step 3 :  adding  medical service to the system using admin panel

1. go to link  [http://127.0.0.1:8000/](http://127.0.0.1:8000/) then you can see that services page is empty.we have to add new services to the system

2. go to link [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)  and enter the super user email and password you have created in the previous step

3. go to services and create services you offer as a clinic or hospital..upload images,add pricing and save.All the services you submited will show on the landing page services sections

##  Step 4 :  adding doctors to the system and allocating them for each services

use the admin panel to create doctor accounts and allocate them to each services you have created before.(doctors also can use the default signup menue to create their accounts update their profile but admin must have allocate each doctor to the respected service)


##  Step 5 :  adding patient to the system

Patients can be added using normal signup menu and admin can use admin panel to add patient details as well


## Now you have a initial working system you can add feature and extend this as you like

---
