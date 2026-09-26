# cPanel deployment for muhupay.com

## 1) Prepare the project for production

1. Make sure Python 3.12+ is available in the hosting environment if possible.
2. Upload this project to a Git repository or a Git-enabled cPanel repository.
3. Create a `.env` file in the project root with values similar to:

```env
SECRET_KEY=your-long-random-secret
DEBUG=False
ALLOWED_HOSTS=muhupay.com,www.muhupay.com
CSRF_TRUSTED_ORIGINS=https://muhupay.com,https://www.muhupay.com
DB_ENGINE=mysql
DB_NAME=your_db_name
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=3306
```

4. Install dependencies from the hosting terminal:

```bash
python3 -m pip install --upgrade pip
python3 -m pip install -r requirements.txt
```

5. Run migrations:

```bash
python3 manage.py migrate
python3 manage.py collectstatic --noinput
python3 manage.py createsuperuser
```

6. Start the app with a WSGI entry point. For cPanel, use Passenger or a Python app config if available.

---

## 2) cPanel database setup (MySQL)

1. Open cPanel > MySQL Databases.
2. Create a new database and database user.
3. Grant the user all privileges to that database.
4. Note these values:
   - Database name
   - DB username
   - DB password
   - Host: usually localhost
5. Add the DB details to the `.env` file.

---

## 3) Domain setup

1. In cPanel, go to Domains or Addon Domains.
2. Add the domain or subdomain such as `muhupay.com`.
3. Make sure the document root points to the project’s public directory.
4. If using a subdomain like `www.muhupay.com`, create that as well.
5. Add the domain names to `ALLOWED_HOSTS` and `CSRF_TRUSTED_ORIGINS` as shown above.

---

## 4) Git-based future updates

### Option A: GitHub + cPanel

1. Create a GitHub repository and push the project.
2. In cPanel, open Git Version Control.
3. Clone the repository into the production folder.
4. After each update:

```bash
git pull origin main
python3 -m pip install -r requirements.txt
python3 manage.py migrate
python3 manage.py collectstatic --noinput
```

### Option B: Git repo in cPanel

1. In cPanel > Git Version Control, create a repository.
2. Connect it to your remote GitHub or a cPanel repo.
3. Pull updates from the repo when changes are ready.

---

## 5) Recommended production checklist

- Set `DEBUG=False`.
- Keep secret credentials in `.env` only.
- Use MySQL instead of SQLite for production.
- Set `ALLOWED_HOSTS` to the live domain names.
- Run `collectstatic` before deployment.
- Keep a backup of the database.
- Use HTTPS via cPanel/SSL certificate on `muhupay.com`.

---

## 6) Quick update commands

```bash
git pull origin main
python3 manage.py migrate
python3 manage.py collectstatic --noinput
```
