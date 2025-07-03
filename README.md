# Secure File Share

A secure, fast, and reliable file sharing platform built with Django REST Framework (backend) and React (frontend).

---

## Features

- **User Registration & Email Verification** (Client users)
- **Login as Client or Ops User**
- **Role-based Access** (Ops can upload, Clients can download)
- **Secure File Upload & Download**
- **File Type Validation** (`.pptx`, `.docx`, `.xlsx` only, max 10MB)
- **JWT Authentication**
- **Responsive UI with Material-UI**

---

## Folder Structure

```
securefileshare/           # Django backend
securefileshare-frontend/  # React frontend
```

---

## Backend Setup (Django)

1. **Install dependencies:**
    ```sh
    cd securefileshare
    python -m venv venv
    venv\Scripts\activate  # On Windows
    pip install -r requirements.txt
    ```

2. **Configure environment variables:**
    - Edit `.env` with your DB, email, and secret key settings.

3. **Apply migrations:**
    ```sh
    python manage.py migrate
    ```

4. **Create a superuser (for admin access):**
    ```sh
    python manage.py createsuperuser
    ```

5. **Run the backend server:**
    ```sh
    python manage.py runserver
    ```

---

## Frontend Setup (React)

1. **Install dependencies:**
    ```sh
    cd securefileshare-frontend
    npm install
    ```

2. **Configure environment variables:**
    - Edit `.env` to set your backend API URL (default: `http://localhost:8000/api`).

3. **Run the frontend server:**
    ```sh
    npm start
    ```
    - App runs at [http://localhost:3000](http://localhost:3000)

---

## Usage

- **Landing Page:** Choose to log in as Client or Ops user.
- **Sign Up:** Only for Client users (Ops users are created by admin).
- **Email Verification:** Clients must verify their email before accessing files.
- **Upload Files:** Only Ops users can upload `.pptx`, `.docx`, `.xlsx` files (max 10MB).
- **Download Files:** Only verified Client users can download files.

---

## Tech Stack

- **Backend:** Django, Django REST Framework, JWT, python-magic
- **Frontend:** React, Material-UI (MUI v5), Axios, Formik, Yup, Framer Motion

---

## Development Scripts

- **Frontend**
    - `npm start` — Start React dev server
    - `npm run build` — Build for production

- **Backend**
    - `python manage.py runserver` — Start Django dev server

---

## Deployment

1. Build the frontend:
    ```sh
    cd securefileshare-frontend
    npm run build
    ```
2. Serve the build folder with your preferred web server.
3. Deploy Django backend as usual (e.g., Gunicorn + Nginx).

---

## Notes

- Make sure to install `python-magic-bin` on Windows for file type validation.
- Update allowed file types/extensions in `files/models.py` if needed.
- For any issues, check backend logs for validation errors.

---

## License

MIT License

---

## Author

- [Your Name](mailto:your.email@example.com)
