# Doctor House clinic server

One program for the clinic: staff accounts, patients, appointments with doctor booking, progress notes,
prescriptions, invoices with part-payments, expenses, reports, history files, secure share links,
audit log, and Excel-friendly CSV exports.

## Run it on your computer
**Windows:** double-click `run.bat` (or run it from a Command Prompt).
**Mac or Linux:** in a terminal, run `./run.sh`.

The first run installs what it needs, asks you to create the administrator account, and starts the server.
Then open **http://localhost:8000** in a browser.

### Use it on a phone or tablet (same Wi-Fi)
The script prints an address such as `http://192.168.1.20:8000`. Open that address on the phone.
Use this only on your clinic's own Wi-Fi, because the local address is not encrypted.

## New in this version
- **Reminders:** the Reminders page lists tomorrow's appointments with a WhatsApp button for each patient.
- **Patient status:** mark patients active, completed or inactive.
- **Import from Excel:** upload a CSV (save from Excel as "CSV UTF-8") to add many patients at once.
- **Password reset:** admins set a temporary password for any staff member.
- **Database backup:** admins download a copy of the SQLite database from the Audit page.

## Staff accounts and roles
| Role | Can do |
|---|---|
| Admin | Everything, including staff accounts, expenses, reports, CSV exports and the audit log |
| Doctor | Patients, schedule, progress notes, prescriptions, history files, secure share links, reports |
| Receptionist | Patients, schedule, billing and payments. Cannot open clinical notes, files or share links |

Add doctors and receptionists under **Staff**. Each person gets a temporary password and must choose their own
at first sign-in. Never share one login between people.

## Share a report with a patient or another doctor
Open the patient → **History files** → **Create secure link**. Choose how long the link lasts and how many times it
can be opened. Send the link and the passcode in two separate messages (WhatsApp or email buttons are provided).
The recipient needs no clinic login. Links stop on their own, can be cancelled, and cancel automatically after five
wrong passcodes.

## Run it online (for access from anywhere)
Use a host with HTTPS (for example Render, or any server with Docker):
1. Build and run with Docker: see the comments at the top of `Dockerfile`.
2. Set `SECRET_KEY` (long, random, secret) and `PUBLIC_BASE_URL=https://your-domain`.
3. Keep `/data` on a disk that is backed up daily. It holds the database and uploaded files.
4. Create the admin with `flask --app app create-admin`.

## Security
- Passwords are salted hashes. Five wrong sign-ins lock an account for 15 minutes; sessions end after 30 minutes idle.
- All forms carry a security token. Files are checked against their type and stored under random names.
- Share links store only a hash of the token. Every open, download and failed attempt is recorded.
- Pages are not cached or indexed, and the server sends the standard protective headers.
- Set a strong `SECRET_KEY`, keep `.secret_key` and `clinic.db` private, and back them up.

## Test it
Run `python test_server.py` and `python test_features.py` (after `pip install -r requirements.txt`).
They check sign-in, roles, uploads, share links, bookings, billing, reports, exports and the audit log.

## Keeping data safe
- Download a database backup regularly (Audit page, admin only), and copy it to another drive or place.
- For MySQL, back up with `mysqldump` on the server instead.

## Limits
- Data lives on the computer or server that runs the program. Several computers do not share records unless the
  program runs on one shared server.
- WhatsApp and email are sent by staff from their own accounts; the server does not send messages.
- Before real patient use, review your local data-protection duties and set up regular backups.
