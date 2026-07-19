# ELSF — Export Logistics Support Foundation website

- **The website** is plain **HTML / CSS / JS** in the `asset/` folder. Open it
  with a web server (e.g. VS Code **Live Server**) — start at `asset/index.html`.
- **The admin page** is the only part that uses **Python** (Flask). The owner
  uses it to add updates, stories and adverts. It saves them to
  `data/content.json`, and the website reads that file to display them.

## Run the website (static)

Open the `asset/` folder with **Live Server** (or any static server) and browse
to `index.html`. That's it — no build step.

> The pages use `fetch()` to load owner-managed content from
> `data/content.json`, so use a server rather than double-clicking the files.

## Run the admin page (Python)

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

cp .env.example .env          # set ADMIN_PASSWORD
python app.py                 # http://127.0.0.1:5000/admin
```

Log in with the password from `.env`. From the dashboard you can add/remove:

- **Updates** — short dated announcements (top bar, home page, Updates page).
- **Stories** — longer articles. Give a story a slug starting with `neld` or
  `ppeld` to make it appear on the NELD / PPELD pages; all stories appear on the
  Updates page.
- **Adverts** — promotional cards on the home page (image optional; can be
  hidden without deleting).

Everything is written to `data/content.json`. Reload the website to see changes.

## Forms

The contact, membership and newsletter forms are **fully functional on the
static site, with no backend**. On submit, `js/main.js`:

1. runs the browser's built-in validation (required fields, valid email),
2. shows a confirmation message, and
3. opens the visitor's mail app with a **pre-filled email** to the ELSF inbox
   (`logisticsdialogue.neld@gmail.com`).

To send messages silently in the background instead (no mail app popup), swap the
`openMailto()` call in `initForms()` for an **EmailJS** or **Formspree** request —
the surrounding validate/confirm flow stays the same.

## Layout

```
asset/
  index.html, who-are-we.html, our-story.html, mandate.html, mission.html,
  membership.html, projects.html, neld.html, ppeld.html, updates.html,
  story.html, contact.html   ← self-contained pages (header/footer inline)
  style.css                  ← styling (ELSF logo palette: navy + sky blue)
  js/main.js                 ← nav, hero slider, forms, loads content.json
  image/                     ← images
  uploads/                   ← images uploaded from the admin page
  data/content.json          ← updates / stories / adverts (edited by admin)
app.py                       ← Flask ADMIN ONLY (serves the dashboard)
templates/admin/             ← admin login + dashboard (HTML, CSS, logo)
.env.example                 ← copy to .env and set ADMIN_PASSWORD / SECRET_KEY
requirements.txt             ← Python deps for the admin (Flask, python-dotenv)
```
