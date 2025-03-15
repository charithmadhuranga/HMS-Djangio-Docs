# Developer Guide


# **Doctor Channeling System Documentation**

## **Overview**

This project is a **Doctor Channeling System** built using **Django** and **Bootstrap 5**, integrating:

- **Stripe & PayPal** for payment processing.
- **Mailgun or resend (Anymail)** for email notifications.
- **Custom user authentication** (Doctors & Patients).
- **Appointment scheduling and medical records management**.

## **Project Structure**

The project consists of the following Django apps:

- `base` – Handles services, appointments, and payments.
- `doctor` – Manages doctor profiles, appointments, and medical records.
- `patient` – Manages patient profiles, appointment tracking, and notifications.
- `userauths` – Handles user authentication (registration, login, logout).
- `hms_prj` – Main project directory containing settings and URL configurations.

---

# **1. Base App**

## **Models (`models.py`)** {id="models-models-py_1"}

- `Service` – Stores service details and costs.
- `Appointment` – Links patients, doctors, and services.
- `MedicalRecord` – Stores diagnosis and treatment details.
- `LabTest` – Stores lab test information and results.
- `Prescription` – Stores prescribed medications.
- `Billing` – Manages payment details.

## **Views (`views.py`)** {id="views-views-py_1"}

Handles service listing, booking, and payment processing:

- `index()` – Displays services and doctors.
- `book_appointment()` – Allows patients to schedule appointments.
- `checkout()` – Handles payment processing (Stripe, PayPal).
- `paypal_payment_verify()` & `stripe_payment_verify()` – Confirms payment transactions.

---

# **2. Doctor App**

## **Models (`models.py`)** {id="models-models-py_2"}

- `Doctor` – Stores doctor profiles and availability.
- `Notification` – Tracks appointment-related notifications.

## **Views (`views.py`)** {id="views-views-py_2"}

- `dashboard()` – Displays earnings, notifications, and scheduled appointments.
- `appointments()` – Lists a doctor’s appointments.
- `add_medical_report()` – Adds medical records to an appointment.
- `payments()` – Displays payments received by the doctor.

---

# **3. Patient App**

## **Models (`models.py`)** {id="models-models-py_3"}

- `Patient` – Stores patient profiles and medical information.
- `Notification` – Tracks appointment updates for patients.

## **Views (`views.py`)** {id="views-views-py_3"}

- `dashboard()` – Displays active appointments and payment history.
- `appointments()` – Lists a patient’s scheduled appointments.
- `notifications()` – Shows unread notifications.

---

# **4. UserAuths App**

## **Models (`models.py`)**

- `User` – Custom user model supporting **email-based authentication**.

## **Views (`views.py`)**

- `register_view()` – Handles user registration.
- `login_view()` – Manages user login.
- `logout_view()` – Logs users out.

## **URLs (`urls.py`)** {id="urls-urls-py_1"}

| URL Pattern | View Function | Description |
|-------------|--------------|-------------|
| `/sign-up/` | `register_view` | Handles user registration. |
| `/sign-in/` | `login_view` | Handles user login. |
| `/sign-out/` | `logout_view` | Logs the user out. |

---

# **5. HMS Project Configuration**

## **Settings (`settings.py`)**

### **Installed Apps**
Includes core Django apps, `jazzmin` (admin panel), and `anymail` (email backend).

### **Authentication**
Uses a **custom user model (`User`)** with email authentication.

### **Database**
Uses SQLite (modifiable for production).

### **Static & Media Files**
Manages static assets (CSS, JS) and media files (uploads).

### **Payment Integration**
Configured for **Stripe** and **PayPal**.

### **Email Configuration**
Uses **Anymail** with Mailgun or Resend.

## **URLs (`urls.py`)**

| URL Pattern | App | Description |
|-------------|-----|-------------|
| `/admin/` | Django Admin | Admin panel access. |
| `/auth/` | `userauths` | User authentication. |
| `/` | `base` | Main application. |
| `/doctor/` | `doctor` | Doctor-related actions. |
| `/patient/` | `patient` | Patient-related actions. |

---

# **Next Steps**

- Deployment Guide 
- Database Diagram 

This documentation provides a **detailed breakdown** of the system components and their functionalities. 🚀

