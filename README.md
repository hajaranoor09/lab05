# Product Configurator (jQuery + Express + MySQL)

## Run it

**Terminal 1 - backend (port 3000)**
```
cd backend
npm install
node server.js
```

**Terminal 2 - frontend (port 5500, a different port, so CORS is needed)**
```
cd frontend
python3 -m http.server 5500      # or: npx http-server -p 5500
```
Open http://localhost:5500

## Database (XAMPP / MySQL)
1. Open the XAMPP Control Panel and **Start MySQL** (Apache is optional, see below).
2. Run the backend (`npm install`, then `node server.js`). It creates the `configurator` database, the tables and 3 seed products automatically.
   Or do it by hand: phpMyAdmin (http://localhost/phpmyadmin) -> Import -> choose `backend/schema.sql`.
3. Tables: `products` (`GET /api/products`), `options` (`GET /api/options`: the Customize colours, variants, sizes, fabrics, fonts and the text/gold prices) `configurations` (written by `POST /api/configurations`), `settings` (promo discount % and Premium price, `GET /api/settings`), `purchases` (Premium purchases, `POST /api/purchases`) and `promo_claims` (promo discounts earned, `POST /api/promo-claims`).
4. Extra endpoints: `GET /api/configurations`, `GET /api/configurations/:id`, `DELETE /api/configurations/:id`, `GET /api/products/:id`.
5. In the page, **Show saved configurations (database)** lists stored rows and lets you delete them. Card details are never stored.

XAMPP defaults are used (host 127.0.0.1, user `root`, empty password, port 3307). If yours differ, set `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` before starting.

To serve the frontend from XAMPP Apache instead of `python -m http.server`, copy the `frontend` folder to `C:\xampp\htdocs\configurator` and open http://localhost/configurator/. The Node backend must still run on port 3000.

## Where each question is implemented

| Question | File | What to look for |
|---|---|---|
| Q1 jQuery | `frontend/app.js` | `$('#id')` selectors, `.on()` events, `.val()`, `.text()`, `.html()`, `.attr()`, `.css()`, `.toggle()`, `.fadeIn()`, `.append()` |
| Q3 Backend + Database | `backend/server.js`, `backend/db.js` | `GET /api/products`, `POST /api/configurations`, `express.json()`, custom logging middleware, `cors()`, MySQL tables `products` and `configurations` |

## Demo / screenshot checklist
1. Open DevTools -> Network (filter Fetch/XHR) and keep the backend terminal visible.
2. Watch the promo video (or press Skip) -> 10% discount is applied to the total.
3. Change colour, variant, size and text -> the preview updates live.
4. Pick a Premium option -> the purchase window opens. Pay with any name and 16 digits -> Premium unlocked.
5. Products now load automatically from the MySQL database when the page opens. Click **Save configuration (POST)** to store a row in the database.
6. Open phpMyAdmin to see the saved row in the `configurations` table.
