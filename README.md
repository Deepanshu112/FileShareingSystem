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


## Email Verification (`VerifyEmail` Component)

This component handles email verification for client users. When a user clicks the verification link sent to their email, they are routed to `/verify-email/:token`. The component:

- Extracts the token from the URL.
- Sends a POST request to the backend to verify the token.
- Displays a loading spinner while verifying.
- Shows a success message if verification succeeds.
- Shows an error message if verification fails or the token is invalid/expired.

### Usage

- The route should be defined as:
  ```jsx
  <Route path="/verify-email/:token" element={<VerifyEmail />} />
  ```
- The backend endpoint should accept a POST request with `{ token }` in the body.

### Example Test Cases

You can use Jest and React Testing Library to test this component. Example test cases:

```javascript
import React from 'react';
import { render, screen, waitFor } from '@testing-library/react';
import VerifyEmail from '../VerifyEmail';
import { MemoryRouter, Route, Routes } from 'react-router-dom';
import apiClient from '../../api/client';

jest.mock('../../api/client');

const renderWithRouter = (token) => {
  return render(
    <MemoryRouter initialEntries={[`/verify-email/${token}`]}>
      <Routes>
        <Route path="/verify-email/:token" element={<VerifyEmail />} />
      </Routes>
    </MemoryRouter>
  );
};

describe('VerifyEmail', () => {
  afterEach(() => {
    jest.clearAllMocks();
  });

  it('shows loading spinner while verifying', async () => {
    apiClient.post.mockImplementation(() => new Promise(() => {}));
    renderWithRouter('sometoken');
    expect(screen.getByText(/verifying your email address/i)).toBeInTheDocument();
  });

  it('shows success message on successful verification', async () => {
    apiClient.post.mockResolvedValue({ data: {} });
    renderWithRouter('validtoken');
    await waitFor(() => {
      expect(screen.getByText(/Email Verified Successfully/i)).toBeInTheDocument();
    });
  });

  it('shows error message on verification failure', async () => {
    apiClient.post.mockRejectedValue({ response: { data: { error: 'Invalid token' } } });
    renderWithRouter('invalidtoken');
    await waitFor(() => {
      expect(screen.getByText(/Verification Failed/i)).toBeInTheDocument();
      expect(screen.getByText(/Invalid token/i)).toBeInTheDocument();
    });
  });

  it('shows error if no token is provided', async () => {
    render(
      <MemoryRouter initialEntries={['/verify-email/']}>
        <Routes>
          <Route path="/verify-email/" element={<VerifyEmail />} />
        </Routes>
      </MemoryRouter>
    );
    await waitFor(() => {
      expect(screen.getByText(/Verification Failed/i)).toBeInTheDocument();
      expect(screen.getByText(/No verification token provided/i)).toBeInTheDocument();
    });
  });
});
```

---
