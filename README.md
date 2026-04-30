# Photo Sharing App

A Django web application for uploading group photos, detecting faces with machine learning, grouping photos by person, and downloading or sharing those grouped albums.

## Features

- Room-code based registration and login
- Multiple photo upload
- Face detection and clustering using `face_recognition`, OpenCV, NumPy, and DBSCAN
- Automatically generated person-wise albums
- View, delete, and download uploaded photos
- Download matched photos as a ZIP file
- Optional Telegram photo sharing per detected person

## Tech Stack

- Python 3.10
- Django 4.0.1
- SQLite
- OpenCV
- face-recognition / dlib
- scikit-learn
- Pillow
- Bootstrap templates

## Project Structure

```text
.
+-- backend/
|   +-- core/               # Django project settings and root URLs
|   +-- home/               # Main app: models, views, routes, migrations
|   +-- static/             # Static files and uploaded images
|   +-- templates/          # HTML templates
|   +-- manage.py
|   +-- Procfile
|   +-- Aptfile
+-- requirements.txt
+-- setup.sh
+-- README.md
```

## Main Pages

| URL | Description |
| --- | --- |
| `/landing` | Landing page |
| `/register` | Create a new room code |
| `/login` | Login with room code and password |
| `/` | Upload and view photos |
| `/process` | Process uploaded photos and group faces |
| `/albumGallery` | View detected people |
| `/album/<id>/` | View photos matched to a person |
| `/download/<id>/` | Download a person's matched photos as ZIP |

## Setup

Clone the project and open the project directory.

```bash
cd "Data Science Project"
```

Create and activate a virtual environment.

```bash
python -m venv venv
```

On Windows:

```bash
venv\Scripts\activate
```

On macOS/Linux:

```bash
source venv/bin/activate
```

Install dependencies.

```bash
pip install -r requirements.txt
```

> Note: `dlib` and `face-recognition` can require extra system build tools. If installation fails, install the required C++ build tools/CMake for your operating system, then retry.

## Environment Variables

Telegram sharing is optional. To enable it, create `backend/.env` and add:

```env
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
```

If this variable is missing, the app will still run, but Telegram sending will show an error when used.

## Database Setup

Run migrations from the `backend` directory.

```bash
cd backend
python manage.py makemigrations
python manage.py migrate
```

Create an admin user if needed.

```bash
python manage.py createsuperuser
```

## Run The App

From the `backend` directory:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/landing
```

## How It Works

1. A user creates a room code and sets a password.
2. The user logs in with the room code.
3. The user uploads group photos.
4. The app detects faces in each uploaded image.
5. Face embeddings are clustered using DBSCAN.
6. Each cluster becomes a person album.
7. The user can view, download, or send matched photos.

## Important Notes

- Uploaded images are stored under `backend/static/images/`.
- The app currently uses SQLite for local development.
- `DEBUG=True` is enabled in settings, so this project is not production-ready without configuration changes.
- The face processing step depends on `dlib` and `face_recognition`.
- The root `venv` is the working virtual environment for this project.

## Development Commands

Run Django checks:

```bash
python manage.py check
```

Run migrations:

```bash
python manage.py migrate
```

Start the server:

```bash
python manage.py runserver
```

## Deployment Notes

The project includes:

- `backend/Procfile` for a Gunicorn-based web process
- `backend/Aptfile` with `libgl1`, required by OpenCV in some Linux environments

Before deploying, update:

- `SECRET_KEY`
- `DEBUG`
- `ALLOWED_HOSTS`
- Static and media file handling
- Database configuration
