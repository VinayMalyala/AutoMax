# AutoMax

AutoMax is a modern, modular `Django-based car listings platform` designed with clean architecture and scalable development in mind.
The project includes:

- A `main` listings app for managing and displaying car posts

- A `users` app for authentication, registration, and user profiles

- Environment variable management using django-environ and a project-level .env file

- Production-ready structure with support for static files, .env isolation, and a clear directory layout

- AutoMax serves as both a practical Django learning project and a foundation for building advanced automotive marketplace solutions.

👉 Live Demo: 
<br>
🔗 https://vinaymalyala-automax.onrender.com
<br>
🔗 https://vinaymalyala.pythonanywhere.com/


---

## 📸 Screenshots
- **Main Page**
  
  <img width="1889" height="965" alt="Image" src="https://github.com/user-attachments/assets/e5b095fa-0ece-4818-8275-de29be7b64de" />
  
- **Home Page**
  
  <img width="1892" height="681" alt="Image" src="https://github.com/user-attachments/assets/432c53cc-c1d6-4b86-87c9-1c36aa739cd0" />
  
- **Car Listing Page**

  <img width="1887" height="961" alt="Image" src="https://github.com/user-attachments/assets/2c2e7647-3337-486f-9c86-0f5bc6905d3d" />
  
- **User Profile**

  <img width="1887" height="973" alt="Image" src="https://github.com/user-attachments/assets/fffaf288-3836-4387-85d4-8bf10b36cbca" />
  

## 📂 Project Structure
- `src/` — Django project root (this repository)
- `src/automax/` — Django settings and WSGI/ASGI setup
- `src/main/` — Main application for listings
- `src/users/` — User authentication/profile app
- `requirements.txt` — Python dependencies

---

## ⚙️ Prerequisites
- Python 3.11+ (project used 3.13 in VS Code settings)
- pip
- (Optional) Visual Studio Code + Python extension
- Git (if you want to clone this repository)
  
```pwsh
git clone https://github.com/VinayMalyala/AutoMax.git
 ```

---

## 🚀 Quick local setup (Windows PowerShell)
1. Create and activate a virtual environment:

```pwsh
python -m venv venv
& "venv\Scripts\Activate.ps1"
```

2. Install dependencies:

```pwsh
pip install -r requirements.txt
```

3. Environment variables — `.env` file

This project uses a `.env` file located in `automax/.env` (relative to `src/`) for sensitive values like `SECRET_KEY` and `DEBUG`.

Create `src/automax/.env` and add (example):

```
SECRET_KEY=your-secret-key-here
DEBUG=True
# Additional vars if you use them:
# DATABASE_URL=sqlite:///db.sqlite3
# OTHER_VAR=value
ENV_INJECTION_TEST=1
```

➡️ Never commit .env to Git.
<br>
**Important:** Keep your real `SECRET_KEY` private and never commit `.env` to version control. Add it to `.gitignore`.

4. Database migrations and superuser

```pwsh
python manage.py migrate
python manage.py createsuperuser
```

5. Collect static (if relevant for your environment/deployment)

```pwsh
python manage.py collectstatic --noinput
```

6. Run the development server

```pwsh
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser.

---

## 🧪 Using VS Code with env files
- Project contains a workspace setting `src/.vscode/settings.json` with:

```json
{
  "python.terminal.useEnvFile": true,
  "python.envFile": "${workspaceFolder}/automax/.env"
}
```

This should make the integrated terminal pick up variables from `automax/.env`. If `ENV_INJECTION_TEST` is present, verify it in a new terminal:

```pwsh
echo $Env:ENV_INJECTION_TEST
python -c "import os; print(os.getenv('ENV_INJECTION_TEST'))"
```

If the terminal shows `None`, try the following steps:
- Close all integrated terminals, reload the VS Code window (Command Palette → Developer: Reload Window), and open a new terminal.
- Ensure the Python extension is enabled (View → Extensions).
- Make sure `python.envFile` path is correct (either workspace or user settings). We include both workspace and user-level settings for convenience.

If VS Code still doesn't inject env values, you can manually load them into a terminal session (example PowerShell script):

```pwsh
Get-Content 'd:\All Folders\VS CODE\Django\django_app_udemy\src\automax\.env' | ForEach-Object {
  if ($_ -and -not ($_ -match '^\s*#')) {
    $parts = $_ -split '='
    $name = $parts[0].Trim()
    $value = ($parts[1..($parts.Length-1)] -join '=').Trim()
    Set-Item -Path "Env:$name" -Value $value
  }
}
```

This loads `.env` variables for the current PowerShell session (process scope).

---

## 🛠 Common commands
- Activate venv: `& "venv\Scripts\Activate.ps1"`
- Install deps: `pip install -r requirements.txt`
- Migrate: `python manage.py migrate`
- Create superuser: `python manage.py createsuperuser`
- Run dev server: `python manage.py runserver`
- Tests: `python manage.py test`

---

## 🌐 Production & deployment notes
- This project includes `whitenoise` for static asset serving in production — adjust `ALLOWED_HOSTS`, set `DEBUG=False`, and configure `STATIC_ROOT` before deploying.
- Use a proper secret-management solution (e.g. environment variables in CI/CD or secrets manager) instead of `.env` in production.

---

## 🧱 About the Codebase
- `automax/settings.py` uses `django-environ` to load environment variables. The project explicitly loads `automax/.env` so Django reads the same `.env` used by your IDE's terminals.
- Important apps: `main` (listings), `users` (authentication & profiles), and the usual Django stack.

---

## 🧩 Helpful troubleshooting
- If `python manage.py` reports Django not installed, ensure your virtual environment is activated and `pip install -r requirements.txt` was run.
- If env variables are missing for the terminal session, reload VS Code and open a new terminal to force re-read of env files.
- For PowerShell 'variable parsing' errors when manually setting env variables, use `Set-Item -Path "Env:<NAME>" -Value <value>` or the `[System.Environment]::SetEnvironmentVariable(...)` approach.
