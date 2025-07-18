# PROG8850 – Assignment 4: Database Automation with Flyway and GitHub Actions

## Overview

This project showcases the use of **Flyway** and **GitHub Actions** for automating database schema migrations in a MySQL environment. The repository contains two sets of migration scripts—initial and incremental—and a CI workflow that applies these migrations in a controlled, repeatable manner.

---

## 🗂️ Project Structure

```
.
├── .github/
│   └── workflows/
│       └── flyway.yml               # GitHub Actions workflow file
├── migrations/                      # Initial migration scripts
│   └── V1__Create_person_table.sql
│   └── V2__Add_email_column.sql
├── incremental_migrations/         # Incremental migration scripts
│   └── V3__Create_subscription_table.sql
│   └── V4__Add_status_column.sql
└── README.md
```

---

## ⚙️ Environment Setup

This project uses the following tools and configurations:

- **Database**: MySQL 8
- **Flyway Version**: 9.16.3
- **CI Environment**: GitHub Actions (runs on Ubuntu)
- **Port**: 3306

### Credentials

| Variable            | Value         |
|---------------------|---------------|
| MYSQL_ROOT_PASSWORD | rootpassword  |
| MYSQL_DATABASE      | subscriptions |
| MYSQL_USER          | subuser       |
| MYSQL_PASSWORD      | subpass       |

---

## 🔧 CI/CD Pipeline Logic (flyway.yml)

### Trigger

- The workflow is triggered on **push to the `main` branch**.

### Jobs

1. **Launch MySQL service** via Docker container with health checks.
2. **Install Flyway CLI** in the GitHub Actions runner.
3. **Repair Flyway metadata** in both `migrations/` and `incremental_migrations/` folders to resolve checksum issues.
4. **Run initial migrations** from `migrations/` folder.
5. **Run incremental migrations** from `incremental_migrations/` folder.

---

## 🚀 How to Reproduce

1. **Clone this Repository**
   ```bash
   git clone https://github.com/ojaydiddy-git/PROG8850-Assignment4-flyway.git
   cd PROG8850-Assignment4-flyway
   ```

2. **Review Migration Scripts**  
   Modify or add your `.sql` files under `migrations/` and `incremental_migrations/`.

3. **Push to `main` Branch**  
   After making any changes, push your code:
   ```bash
   git add .
   git commit -m "Add new migration"
   git push origin main
   ```

4. **Observe Workflow Execution**  
   Go to the **Actions** tab on GitHub and watch the Flyway CI pipeline execute.

---

## ✅ Notes

- The **repair step** is crucial to resolve any mismatch between the applied and resolved migrations in Flyway.
- If Flyway fails due to a **checksum mismatch**, it indicates that a migration script was modified after being applied. Either revert the changes or run `flyway repair` to fix the issue.

---

## 📜 License

This project is submitted as part of the coursework for PROG8850 and is licensed under MIT.

---