# FitFlow: Gym Management Platform

FitFlow is a web platform for running a fitness club, built with **Django**. Members can sign up, pick a membership plan, enroll in courses and book sessions from a weekly schedule. Gym staff manage everything (members, coaches, courses, sessions, clubs and equipment) from a customized Django admin dashboard. The interface is in French.

## Features

### For members
- **Account management**: registration, login, logout and password change
- **Member profile**: personal details, phone number validation, notification preferences
- **Membership plans**: browse plans with duration, price, description and benefits, then subscribe
- **Checkout flow**: choose a payment method and confirm the subscription (simulated payment)
- **Course catalog**: browse courses with images, coach, activity and capacity, then enroll
- **Weekly planning**: sessions grouped by day (Monday to Sunday) and sorted by start time
- **Session booking**: reserve a spot in a session, with a capacity check so a session can't be overbooked

### For gym staff (admin dashboard)
- Themed admin with **django-admin-interface**
- Manage **members**, **coaches**, **guests** (trial sessions), **membership types**, **activities**, **courses**, **sessions**, **clubs** (multi-city: Casablanca, Rabat, Marrakech, Agadir, Tangier) and **equipment** (purchase date, lifespan, quantity per club)
- Search, filters, pagination, and inline views of each member's enrolled courses and booked sessions

### Public pages
- Landing page, about, coaches, schedule and contact pages

## Tech stack

| Layer     | Technology                                                     |
|-----------|----------------------------------------------------------------|
| Backend   | Python 3, Django 4.2                                           |
| Database  | SQLite (development)                                           |
| Frontend  | Django templates, Bootstrap 4, jQuery, Owl Carousel, AOS       |
| Forms     | django-crispy-forms (Bootstrap 4), django-phonenumber-field    |
| Admin     | django-admin-interface, django-colorfield                      |
| Media     | Pillow (course images)                                         |

## Data model

```
TypeAbonnement ──< Membre >──┬── ModePaiement
                             ├──<> Cours ──── Instructeur
                             └──<> Seance ─── Cours ─── Activite
Club ──<> Activite      Club ──< Equipement      Invite ── Seance (trial)
```

| Model            | Purpose                                                   |
|------------------|-----------------------------------------------------------|
| `Membre`         | Gym member: contact info, plan, payment method, bookings  |
| `TypeAbonnement` | Membership plan: name, duration (months), price, benefits |
| `Cours`          | Course: name, capacity, coach, activity, image            |
| `Seance`         | Scheduled session of a course (day and start time)        |
| `Instructeur`    | Coach                                                     |
| `Activite`       | Activity type (e.g. yoga, cardio)                         |
| `Club`           | Gym location with its activities                          |
| `Equipement`     | Equipment inventory per club                              |
| `Invite`         | Guest booked on a trial session                           |

## Getting started

### Prerequisites
- Python 3.10+
- pip

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/houdamardas1/fitflow.git
cd fitflow

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Create the database and an admin account
python manage.py migrate
python manage.py createsuperuser

# 5. Start the development server
python manage.py runserver
```

- Website: http://127.0.0.1:8000/
- Admin dashboard: http://127.0.0.1:8000/admin/

Log in to the admin to add membership plans, coaches, activities, courses and sessions, then register a member account on the website.

### Configuration

Settings are read from environment variables (see `.env.example`):

| Variable               | Default                     | Description                 |
|------------------------|-----------------------------|-----------------------------|
| `DJANGO_SECRET_KEY`    | development-only key        | Set a strong key in production |
| `DJANGO_DEBUG`         | `True`                      | Set to `False` in production |
| `DJANGO_ALLOWED_HOSTS` | `localhost,127.0.0.1`       | Comma-separated host names  |

## Project structure

```
FitFlow/         # Project settings and root URLs
SalleDeSport/    # Core app: models, views, admin, templates, static assets
account/         # Authentication: login, logout, registration, password change
manage.py
requirements.txt
```

## Notes

This is an academic project. The payment step is a simulation built for the demo: it is not connected to a payment provider, and a production version would delegate card handling to a provider such as Stripe instead of storing card details.

## Author

**Houda Mardas** · [GitHub](https://github.com/houdamardas1)
