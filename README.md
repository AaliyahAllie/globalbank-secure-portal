# GlobalBank Secure Portal

A secure customer portal built with a Node.js/Express backend and a React frontend. This project uses local HTTPS, strong validation, secure sessions, and a small DevSecOps workflow.

## How to Run the GlobalBank Secure Portal

### 1. Clone the project

```bash
git clone YOUR_GITHUB_REPO_LINK
git clone YOUR_GITHUB_REPO_LINK
cd globalbank-secure-portal
```

### 2. Open the project in VS Code

```bash
code .
```

### 3. Backend setup

Open a terminal and run:

```bash
cd backend
npm install
npm run generate-certs
```

Create a file called `backend/.env` and add:

```env
PORT=5000
MONGO_URI=YOUR_MONGODB_CONNECTION_STRING
SESSION_SECRET=your_long_secret_key
FRONTEND_URL=https://localhost:3000
NODE_ENV=development
```

Then start the backend with:

```bash
npm run dev
```

You should see output like:

```txt
MongoDB Atlas connected
🔒 Secure GlobalBank API running on https://localhost:5000
```

Test the backend in your browser at:

```txt
https://localhost:5000
```

### 4. Frontend setup

Open a second terminal and run:

```bash
cd frontend
npm install
npm run start:https
```

The React app should open at:

```txt
https://localhost:3000
```

### 5. How to use the system

1. Open `https://localhost:3000`
2. Click **Register**
3. Create an account using:
   - Full name
   - ID number
   - Username
   - Account number
   - Strong password (example: `Password@123`)
4. You should be redirected to the customer dashboard.
5. Create an international payment.
6. View payment history.
7. Click **Logout** to end the session.

### 6. Important notes

Keep both backend and frontend running at the same time.

```bash
# Terminal 1
cd backend
npm run dev
```

```bash
# Terminal 2
cd frontend
npm run start:https
```

Do **not** push `.env` to GitHub because it contains database credentials.

## Security and validation features

### HTTPS and SSL
- Backend serves requests over `https://localhost:5000` using a self-signed local certificate.
- The frontend API base URL is configured for HTTPS.
- For production, use a trusted CA-issued certificate such as Let’s Encrypt.
- HSTS is enabled in production to help prevent MITM attacks.

### Input validation and XSS protection
- Register, Login, and Payment forms use matching frontend `RegEx` patterns.
- Backend validation sanitizes user input and rejects dangerous characters like `$` and `.`.
- Text fields are escaped server-side to reduce XSS risk.

### Injection protection
- MongoDB queries are built from validated request data, which is equivalent to using parameterized queries in ORM systems.
- This prevents query manipulation and injection attacks.

### Clickjacking and header protections
- Helmet sets `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, and `Referrer-Policy: no-referrer`.
- These headers reduce risk from clickjacking and content sniffing.

### Session hijacking protections
- Sessions use secure cookies: `Secure`, `HttpOnly`, and `SameSite=Strict`.
- Login regenerates sessions and logout destroys the cookie.
- Session timeout is set to 1 hour.

### DDoS and brute-force mitigation
- `express-rate-limit` throttles API requests.
- `express-brute` protects authentication endpoints from repeated login/register abuse.

## DevSecOps pipeline
- A GitHub Actions workflow is included at `.github/workflows/nodejs-ci.yml`.
- It runs on `push` and `pull_request`.
- It installs both backend and frontend dependencies.
- It runs dependency audits and builds the frontend.

## Notes
- Use `backend/.env.example` and `frontend/.env.example` as templates.
- The repo ignores backend `.env` and generated backend certificates.
- Browsers will warn about the self-signed localhost certificate; this is expected for local development.

### Trusting the local certificate in Windows
If the browser blocks the self-signed cert, you can trust it temporarily by:
1. Visiting `https://localhost:5000` in the browser.
2. Clicking through the advanced warning.
3. Choosing the option to proceed to the site.

For a more permanent trust setup, import `backend/certs/localhost.pem` into the Windows Trusted Root Certification Authorities store. Only do this for local development.

## Manual testing evidence
Include screenshots or video of these flows:
- Registration
- Login
- Secure payment submission
- Payment history
- Logout
- Validation errors
- MongoDB saved records

- Demo Video : https://youtu.be/SNxT5zyAs2Y?si=qOmEb8bo39opvLnH
