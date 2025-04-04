# Default Django Project Structure

When you create a new Django project using `django-admin startproject projectname`, it generates the following basic structure:

```
projectname/
│
├── projectname/               # Main project package
│   ├── __init__.py            # Marks this as a Python package
│   ├── settings.py            # Project settings/configuration
│   ├── urls.py                # URL declarations (route definitions)
│   ├── asgi.py                # ASGI config for async servers
│   └── wsgi.py                # WSGI config for deployment
│
└── manage.py                  # Command-line utility
```

## Key Files Explained:

1. **manage.py**:

   - A command-line utility that lets you interact with your Django project
   - Used for running development server, migrations, creating apps, etc.

2. **settings.py**:

   - Contains all project configuration
   - Database settings, installed apps, middleware, templates, etc.
   - SECRET_KEY (important for production security)

3. **urls.py**:

   - URL declarations for your project (URL routing)
   - Maps URLs to views

4. **wsgi.py** and **asgi.py**:

   - Entry points for WSGI/ASGI servers
   - Used when deploying to production

5. ****init**.py**:
   - Empty file that tells Python this directory should be considered a package

## After Adding an App

When you add an app with `python manage.py startapp appname`, the structure expands:

```
projectname/
│
├── appname/                   # Your new app
│   ├── migrations/            # Database migrations
│   ├── __init__.py
│   ├── admin.py               # Admin interface config
│   ├── apps.py                # App config
│   ├── models.py              # Database models
│   ├── tests.py               # Tests
│   └── views.py               # View functions
│
├── projectname/               # Original project files
└── manage.py
```

This is the basic structure that Django provides out of the box, which you then expand with your own models, views, templates, and static files as your project grows.
