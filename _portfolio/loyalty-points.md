---
title: "Loyalty Points and Digital Payment System"
excerpt: "This project is a backend API for a loyalty points system integrated with a simulated digital payment system using Django, DRF, Celery, and PostgreSQL."
date: 2025-04-20
link: https://github.com/pramudityad/django-loyalty-points-app

header:
  teaser: /assets/images/django-loyalty-points/2017-11-27-cover.png

sidebar:
  - title: "Purpose"
    text: "Developing a robust and scalable backend system that manages loyalty points and integrates digital payment workflows."
  - title: "Highlights"
    text: "Includes role-based access, JWT authentication, Celery-based background jobs, and separation of data warehouse for analytics."
  - title: "Technologies"
    text: "Django, Django REST Framework, PostgreSQL, Celery, Redis, Docker, and Swagger (drf-yasg)."

---

This project is a backend API for a loyalty points system integrated with a simulated digital payment system. It is built with Django, Django REST Framework, Celery, and PostgreSQL, and it supports user management, points earning and redemption, simulated payment transactions, voucher management, data warehousing for transactions, background tasks for expiring points, and JWT-based authentication with role-based access control.

### Specifications

- **Backend Framework:** Django with Django REST Framework
- **Asynchronous Task Queue:** Celery with Redis broker
- **Authentication:** JWT (using djangorestframework-simplejwt)
- **Monitoring:** Flower
- **Database:** PostgreSQL (main DB and warehouse)
- **Containerization:** Docker & Docker Compose
- **Documentation:** Swagger UI via drf-yasg
- **Testing Framework:** Pytest with pytest-django

### Milestones

- **Project Initialization**
  - Created the architecture including services for web, database, Redis, Celery worker, and Flower monitor.
- **Feature Implementation**
  - Developed APIs for user management, point accumulation, and redemptions.
  - Integrated JWT authentication and role-based access.
- **Background Processing**
  - Built Celery tasks for auto-expiry of loyalty points and scheduled jobs.
- **Monitoring and Testing**
  - Added Flower for real-time monitoring and Pytest for unit testing coverage.
- **Documentation and Deployment**
  - Generated Swagger docs for API testing and simplified deployment with Docker Compose.

{% include gallery caption="No gallery available for this project." %}
