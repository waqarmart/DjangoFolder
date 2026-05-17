# DjangoFolder

A minimal Django web application configured for deployment on AWS Elastic Beanstalk.

This project demonstrates a basic Django setup with a simple homepage response, WSGI/ASGI configuration, dependency management, and Elastic Beanstalk deployment configuration.

## Application Output

When the application runs successfully, the homepage displays:

```text
Hello from Elastic Beanstalk Django App!
```

## Project Purpose

The purpose of this repository is to demonstrate how to create and deploy a basic Django application using AWS Elastic Beanstalk.

This project covers:

- Basic Django project structure
- Django URL routing
- Django view creation
- WSGI application configuration
- ASGI application configuration
- Python dependency management
- AWS Elastic Beanstalk deployment setup
- Static files configuration

## Technologies Used

- Python
- Django 5.0.2
- Gunicorn
- AWS Elastic Beanstalk
- GitHub

## Repository Structure

```text
DjangoFolder/
│
├── .ebextensions/
│   └── django.config
│
├── mysite/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   ├── views.py
│   └── wsgi.py
│
├── application.py
├── manage.py
├── requirements.txt
└── README.md
```

## File Guide

### `application.py`

This file exposes the Django WSGI application for AWS Elastic Beanstalk.

```python
from mysite.wsgi import application
```

Elastic Beanstalk uses this file to locate the Django application object.

### `manage.py`

This is Django's command-line utility file. It is used to run development commands such as starting the local server, checking the project, and managing Django tasks.

Example:

```bash
python manage.py runserver
```

### `requirements.txt`

This file lists the Python packages required to run the project.

Current dependencies:

```text
Django==5.0.2
gunicorn
```

> Note: The current repository version may show these dependencies on one line. For best practice, keep each dependency on its own line.

Correct format:

```text
Django==5.0.2
gunicorn
```

### `.ebextensions/django.config`

This file configures AWS Elastic Beanstalk to use the correct WSGI path.

The WSGI path is:

```text
application:application
```

This means:

- The first `application` refers to the root-level `application.py` file.
- The second `application` refers to the imported WSGI application object.

### `mysite/settings.py`

This file contains the main Django project configuration.

Important settings include:

```python
SECRET_KEY = 'replace-me'
DEBUG = True
ALLOWED_HOSTS = ['*']
INSTALLED_APPS = [
    'django.contrib.contenttypes',
    'django.contrib.staticfiles'
]
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware'
]
ROOT_URLCONF = 'mysite.urls'
WSGI_APPLICATION = 'mysite.wsgi.application'
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'staticfiles'
```

### `mysite/urls.py`

This file defines the URL routes for the project.

The root URL `/` is connected to the `home` view:

```python
urlpatterns = [
    path('', views.home),
]
```

### `mysite/views.py`

This file contains the homepage view.

The `home` view returns a simple HTTP response:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello from Elastic Beanstalk Django App!")
```

### `mysite/wsgi.py`

This file creates the WSGI application used by production web servers and AWS Elastic Beanstalk.

```python
application = get_wsgi_application()
```

### `mysite/asgi.py`

This file creates the ASGI application for asynchronous server support.

```python
application = get_asgi_application()
```

## How to Run the Project Locally

Follow these steps to run the project on your computer.

## Step 1: Clone the Repository

```bash
git clone https://github.com/iamwaqarjaved/DjangoFolder.git
cd DjangoFolder
```

## Step 2: Create a Virtual Environment

```bash
python -m venv .venv
```

## Step 3: Activate the Virtual Environment

On Windows:

```bash
.venv\Scripts\activate
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

## Step 4: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

## Step 5: Run the Django Development Server

```bash
python manage.py runserver
```

## Step 6: Open the Application

Open this URL in your browser:

```text
http://127.0.0.1:8000/
```

Expected output:

```text
Hello from Elastic Beanstalk Django App!
```

## AWS Elastic Beanstalk Deployment Guide

This project includes Elastic Beanstalk configuration through the `.ebextensions` folder.

## Step 1: Confirm Required Files

Before deployment, confirm that these files exist:

```text
application.py
requirements.txt
.ebextensions/django.config
mysite/settings.py
mysite/wsgi.py
```

## Step 2: Confirm WSGI Path

The Elastic Beanstalk WSGI path should be:

```text
application:application
```

## Step 3: Create an Elastic Beanstalk Application

In the AWS Elastic Beanstalk console:

1. Create a new application.
2. Choose Python as the platform.
3. Upload the project source code.
4. Deploy the environment.
5. Open the Elastic Beanstalk environment URL.

## Step 4: Verify Deployment

After deployment, open the Elastic Beanstalk URL in your browser.

You should see:

```text
Hello from Elastic Beanstalk Django App!
```

## Common Commands

Run local server:

```bash
python manage.py runserver
```

Check Django project configuration:

```bash
python manage.py check
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Collect static files:

```bash
python manage.py collectstatic
```

Create a new Django app:

```bash
python manage.py startapp appname
```

## Development Workflow

A typical workflow for making changes:

```bash
git clone https://github.com/iamwaqarjaved/DjangoFolder.git
cd DjangoFolder
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py runserver
```

After editing files:

```bash
git add .
git commit -m "Update Django project"
git push origin main
```

## Troubleshooting

### Dependencies Do Not Install

Make sure `requirements.txt` is formatted correctly:

```text
Django==5.0.2
gunicorn
```

Then run:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Local Server Does Not Start

Check that the virtual environment is activated and dependencies are installed:

```bash
pip install -r requirements.txt
python manage.py runserver
```

### Elastic Beanstalk Deployment Fails

Check the WSGI path in `.ebextensions/django.config`:

```text
application:application
```

Also confirm that `application.py` exists in the project root.

### Homepage Does Not Load

Check `mysite/urls.py` and `mysite/views.py`.

The root URL should point to the `home` view:

```python
urlpatterns = [
    path('', views.home),
]
```

The view should return an HTTP response:

```python
def home(request):
    return HttpResponse("Hello from Elastic Beanstalk Django App!")
```

## Security Notes

The current project is configured for learning and development.

Before using it in production, update the following settings:

```python
DEBUG = False
ALLOWED_HOSTS = ['your-domain.com']
SECRET_KEY = 'your-secure-secret-key'
```

Recommended production improvements:

- Store `SECRET_KEY` in an environment variable.
- Set `DEBUG = False`.
- Restrict `ALLOWED_HOSTS`.
- Add secure middleware settings.
- Configure a production database if needed.
- Run `collectstatic` before deployment.
- Avoid committing sensitive values to GitHub.

## Formatting Note

Several files in this repository may appear on one line. For readability and proper Python/YAML formatting, make sure these files use correct line breaks and indentation:

```text
manage.py
application.py
requirements.txt
.ebextensions/django.config
mysite/settings.py
mysite/urls.py
mysite/views.py
mysite/wsgi.py
mysite/asgi.py
```

## Future Improvements

Possible future improvements include:

- Add HTML templates
- Add CSS styling
- Add a database model
- Enable Django admin
- Add unit tests
- Add environment variable support
- Add production-ready settings
- Add a `buildspec.yml` file for AWS CodeBuild
- Add CI/CD deployment automation
- Add custom domain support

## Author

Waqar Javed

GitHub: [iamwaqarjaved](https://github.com/iamwaqarjaved)

## License

This project is created for academic and learning purposes.
