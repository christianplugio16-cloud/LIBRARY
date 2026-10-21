# CPSU Library Borrowing System

A responsive library borrowing application built with HTML, CSS, browser JavaScript, a Node.js API, and a persistent SQLite database.

## Requirements and startup

- Node.js 22.5 or later (the server uses Node's built-in `node:sqlite` module).
- npm.

From this folder:

```powershell
npm install
npm start
```

Open `http://localhost:3000`. The SQLite database is created as `library.sqlite` beside `server.js`; set `LIBRARY_DB` to store it at another path. Use `npm run dev` during development.

## Install on a phone

The responsive site can be installed as a home-screen app when it is served from a secure HTTPS address. `localhost` is suitable for testing on the computer running the server, but is not the phone's address; for phone installation, deploy the server behind HTTPS with a domain and persistent database storage. Open that HTTPS address on the phone while connected to the internet, then use the browser menu and choose **Install app** or **Add to Home screen**. On iPhone/iPad, use Safari's **Share → Add to Home Screen**.

The app shell can load from cache after installation, but sign-in, catalog data, borrowing, and other library operations require a live connection to the server.

In development, the database is seeded with these accounts:

| Role | Email | Password |
| --- | --- | --- |
| Administrator | `admin@gmail.com` | `Admin123!` |
| Librarian | `librarian@gmail.com` | `Library123!` |
| Student | `student@gmail.com` | `Student123!` |

New accounts must use a Gmail address. Full names and email addresses must be unique, ignoring capitalization and surrounding spaces. Student IDs use `YYYY-NNNN-V`: `YYYY` is the year the student entered CPSU, `NNNN` is their four-digit student number, and `V` is the required suffix. Student IDs are unique. The seeded student account uses `2026-0001-V`. Demo users and passwords are only seeded when `NODE_ENV` is not `production`.

## Production setup

Set `NODE_ENV=production`, `PORT` if needed, and `LIBRARY_DB` to a durable database path. On the first start, provide an `ADMIN_EMAIL` ending in `@gmail.com` and a unique `ADMIN_PASSWORD` of at least 12 characters to create the initial administrator. The session cookie is marked `Secure` in production, so serve the app over HTTPS. Use a reverse proxy or hosting platform that terminates TLS and keep the database file on persistent storage.

## Included workflows

- Role-based admin, librarian, and student workspaces.
- Administrators can search user accounts by name, Gmail address, or CPSU ID, review an individual student's borrowing activity, and securely reset student passwords.
- Public announcements appear on the welcome page; administrators and librarians can create, edit, publish, and archive announcements.
- Catalog search and category/availability filtering; book, author, and category management.
- Student self-registration and secure password hashing with Node's scrypt implementation.
- Borrow requests, librarian review, configurable loan length and student borrowing limit.
- Checkout/return inventory updates, overdue detection, configurable daily fines, and payment recording.
- Staff can record lost or damaged books with a case-specific charge and notes. Lost copies are removed from inventory; damaged copies are either returned to available stock when repairable or removed when unrepairable. Charges can be paid partially or in full and appear in student balances, notifications, and fine reports.
- Searchable, sortable, paginated transaction tables and CSV/printable reports.
- Student notifications, dashboard statistics, monthly borrowing chart, and filtered reports.

The default overdue fine is **₱2 per day**. Fine and damage/loss charges are displayed in Philippine pesos (PHP).
