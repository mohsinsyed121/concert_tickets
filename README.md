# CONCERT Ticket Booking (Django)

Flow: Select ticket → Name/Email/Phone → Order ID → UPI QR → Pay → Enter UTR →
Payment verification → Ticket ID (CONCERT-XXXXX) + unique QR → Email → Gate scan.

## Quick start (sabse aasaan)
Zip extract karo → Windows pe `run.bat` double-click karo (Mac/Linux: `./run.sh`).
Pehli baar admin username/password poochega, phir server chalu ho jayega.
Home page pe upar-right mein **Admin Login** button hai.

## Setup (manual)
```
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```
Open http://127.0.0.1:8000/

## Configure (env vars or ticketsite/settings.py)
- `UPI_ID` = your real UPI ID (e.g. `mohsin@okhdfcbank`), `UPI_PAYEE`
- `TICKET_TYPES` in settings.py = ticket names and prices
- `AUTO_VERIFY=1` = demo mode, payment auto-confirmed after UTR entry
- `AUTO_VERIFY=0` (default) = real mode: open /admin/ → Orders → select
  VERIFYING orders → action "Mark payment as verified". Ticket is created and emailed.
- Real emails: set `EMAIL_HOST_USER`, `EMAIL_HOST_PASSWORD` (Gmail app password),
  `DEFAULT_FROM_EMAIL`. Without these, emails print in the terminal.

## Pages
- `/` book tickets · `/admin/` verify payments · `/gate/` entry scanner (staff login required)
  Each ticket can be scanned once; the second scan shows "Already used".

## Going live
Set `DEBUG=0`, a strong `SECRET_KEY`, `ALLOWED_HOSTS`, use HTTPS + gunicorn, and consider
PostgreSQL. For fully automatic payment verification use a gateway (Razorpay/Cashfree webhooks)
and call `order.confirm()` from the webhook.

## Admin panel
Styled dashboard at /admin/ (revenue, paid tickets, "needs verification" count, sales by type),
colored status badges, search and filters.
