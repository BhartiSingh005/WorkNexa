# WorkNexa — User Registration & Login System

> Last updated: 2026-09-02 — README updated to record today's contribution.

## Contributing

We welcome contributions. To contribute specifically to today's work (2026-09-02):

- Fork the repository.
- Create a branch named `contribution/2026-09-02-yourname`.
- Make small, focused commits and mention "Today's commits (2026-09-02)" in your pull request description.
- Open a pull request against the repository's default branch and request a review.

If you're unsure what to change, open an issue describing your idea and reference the commit(s) you want to build on.

A Flask-based user authentication system (WorkNexa) with Neon PostgreSQL database, deployed on Render.

## Features
- User registration with validation (username: 4–25 chars, password: min 6 chars)
- User login with session management
- Password hashing with bcrypt
- User dashboard with profile management
- Email update functionality
- User listing feature (requires login)
- Account deletion
- Session-based authentication
- Neon PostgreSQL database integration
- Form validation with Flask-WTF
- Email validation
- Deployment on Render


## Local Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/BhartiSingh005/WorkNexa.git
   cd WorkNexa
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   source venv/bin/activate  # macOS/Linux
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment**
   ```bash
   copy .env.example .env  # Windows
   cp .env.example .env    # macOS/Linux
   ```
   Edit `.env` with your database credentials:
   ```
   SECRET_KEY=<your-secret-key>
   DATABASE_URL=postgresql://<user>:<password>@<host>/<dbname>
   FLASK_ENV=development
   ```

5. **Run the application**
   ```bash
   python app/app.py
   ```
   > Database tables are created automatically on first run via `db.create_all()`.

6. **Access the application**
   - Home: http://localhost:5000/
   - Register: http://localhost:5000/register
   - Login: http://localhost:5000/login
   - Dashboard: http://localhost:5000/dashboard (after login)

## Project Structure
```
WorkNexa/
├── app/
│   ├── model/
│   │   ├── __init__.py
│   │   └── users.py       # User model and database
│   ├── static/
│   │   └── favicon.ico    # Static files
│   ├── templates/
│   │   ├── base.html      # Base template
│   │   ├── dashboard.html # User dashboard
│   │   ├── login.html     # Login page
│   │   └── register.html  # Registration page
│   ├── __init__.py
│   ├── app.py             # Main Flask application
│   └── form.py            # WTForms definitions
├── .env                   # Environment variables (not in repo)
├── .env.example           # Environment template
├── .gitignore             # Git ignore rules
├── render.yaml            # Render deployment config
├── requirements.txt       # Python dependencies
├── wsgi.py                # WSGI entry point for gunicorn
└── README.md              # This file
```

## Technologies Used

- **Backend**: Flask 3.1.3 (Python web framework)
- **Database**: Neon PostgreSQL (serverless PostgreSQL) via psycopg[binary]
- **ORM**: Flask-SQLAlchemy 3.1.1
- **Authentication**: bcrypt 5.0.0 for password hashing
- **Forms**: Flask-WTF 1.2.2 with WTForms 3.2.1 validation
- **Email Validation**: email-validator 2.0.0
- **Deployment**: Render (gunicorn 23.0.0)
- **Environment**: python-dotenv 1.2.2 for configuration

## User Model

| Field           | Type    | Description                  |
|-----------------|---------|------------------------------|
| `id`            | Integer | Primary key                  |
| `username`      | String  | Unique, max 80 chars         |
| `email`         | String  | Unique, max 120 chars        |
| `password_hash` | String  | bcrypt hashed password       |
| `created_at`    | DateTime| Account creation timestamp   |

## Available Routes
- `/` - Home page (redirects to dashboard if logged in, otherwise to register)
- `/register` - User registration
- `/login` - User login
- `/dashboard` - User dashboard (requires login)
- `/update-email` - Update user email (POST, requires login)
- `/fetch-users` - View all users (requires login)
- `/delete-account` - Delete user account (POST, requires login)
- `/logout` - User logout

## Deployment

### Render Deployment (Current)
The application is deployed on Render using `render.yaml` with:
- **Service name**: `worknexa-flask`
- **Database name**: `worknexa-db`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn wsgi:app`
- **Environment Variables**:
  - `SECRET_KEY`: Strong secret key for sessions
  - `DATABASE_URL`: Auto-linked from Render PostgreSQL database
  - `FLASK_ENV`: `production`

### Local Development
For local development:
1. Set up Neon database and get connection string
2. Configure `.env` file with your credentials (`FLASK_ENV=development`)
3. Run `python app/app.py`

> **Note**: The app uses `wsgi.py` as the gunicorn entry point to avoid package naming conflicts. Debug mode is off in all environments by default.

### Manual Deployment
For other platforms:
1. Set environment variables (`FLASK_ENV=production`)
2. Use a production WSGI server: `gunicorn wsgi:app`
3. Configure proper database credentials
4. Set a strong `SECRET_KEY`

## 🛠️ Tech Stack
- **Backend:** Python, Flask
- **Database:** PostgreSQL (Neon)
- **Security:** Flask-Bcrypt, Flask-WTF
- **Deployment:** Render

## License
MIT
