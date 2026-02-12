node_modules/
menu.db
// index.js ── single-file CheckEthyl-style menu database
// Usage:
//   npm init -y
//   npm install express ejs better-sqlite3
//   node index.js
// → open http://localhost:3000

const express = require('express');
const Database = require('better-sqlite3');
const path = require('path');

const app = express();
const port = process.env.PORT || 3000;

// ── Config ──────────────────────────────────────────────────────
const ADMIN_PASS = 'barkeep2025';           // ← CHANGE THIS !
const DB_PATH   = path.join(__dirname, 'menu.db');

// ── Database ────────────────────────────────────────────────────
const db = new Database(DB_PATH);

db.exec(`
  CREATE TABLE IF NOT EXISTS items (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    type        TEXT NOT NULL,          -- drink | dish | glassware
    name        TEXT NOT NULL,
    category    TEXT,
    method      TEXT,
    ingredients TEXT,                   -- JSON array string
    garnish     TEXT,
    glassware   TEXT,
    notes       TEXT,
    created_at  DATETIME DEFAULT CURRENT_TIMESTAMP
  );
`);

// ── Middleware ──────────────────────────────────────────────────
app.set('view engine', 'ejs');
app.use(express.static('public'));          // optional css/js folder
app.use(express.urlencoded({ extended: true }));

// Very basic admin check (cookie-based demo — not secure!)
app.use((req, res, next) => {
  res.locals.isAdmin = req.cookies?.adminPass === ADMIN_PASS;
  next();
});

// ── Helper: inline EJS-like templates ───────────────────────────
const layout = (title, content) => `
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>${title} | My Menu DB</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body { background:#f8f9fa; padding:1.5rem 0; }
    .container { max-width: 900px; }
    .list-group-item a { color:#212529; text-decoration:none; }
    .list-group-item a:hover { text-decoration:underline; }
  </style>
</head>
<body>
  <div class="container">
    <header class="text-center mb-5">
      <h1><a href="/" class="text-dark text-decoration-none">My Bar Menu Database</a></h1>
      <nav class="mb-4">
        <a href="/drinks" class="mx-2">Drinks</a> |
        <a href="/dishes" class="mx-2">Dishes</a> |
        <a href="/glassware" class="mx-2">Glassware</a>
        ${res.locals.isAdmin ? ' | <a href="/admin" class="text-primary">Admin</a> | <a href="/logout">Logout</a>' : ' | <a href="/admin">Admin</a>'}
      </nav>
    </header>
    ${content}
    <footer class="text-center mt-5 pt-4 border-top">
      <small>© 2026 Suraj – Hong Kong</small>
    </footer>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
`;

// ── Routes ──────────────────────────────────────────────────────

// Home
app.get('/', (req, res) => {
  const counts = {
    drinks:    db.prepare("SELECT COUNT(*) as c FROM items WHERE type='drink'    AND archived=0").get().c,
    dishes:    db.prepare("SELECT COUNT(*) as c FROM items WHERE type='dish'    AND archived=0").get().c,
    glassware: db.prepare("SELECT COUNT(*) as c FROM items WHERE type='glassware' AND archived=0").get().c,
  };

  const recent = db.prepare(`
    SELECT id, name, type FROM items
    WHERE archived=0 ORDER BY created_at DESC LIMIT 10
  `).all();

  const content = `
    <h2 class="mb-4 text-center">Overview</h2>
    <div class="row g-4">
      <div class="col-md-4"><div class="card h-100 shadow-sm">
        <div class="card-body text-center">
          <h5>Drinks <span class="badge bg-primary rounded-pill">${counts.drinks}</span></h5>
          <a href="/drinks" class="btn btn-outline-primary mt-2">View →</a>
        </div>
      </div></div>
      <div class="col-md-4"><div class="card h-100 shadow-sm">
        <div class="card-body text-center">
          <h5>Dishes <span class="badge bg-primary rounded-pill">${counts.dishes}</span></h5>
          <a href="/dishes" class="btn btn-outline-primary mt-2">View →</a>
        </div>
      </div></div>
      <div class="col-md-4"><div class="card h-100 shadow-sm">
        <div class="card-body text-center">
          <h5>Glassware <span class="badge bg-primary rounded-pill">${counts.glassware}</span></h5>
          <a href="/glassware" class="btn btn-outline-primary mt-2">View →</a>
        </div>
      </div></div>
    </div>

    <h3 class="mt-5 mb-3">Recently added</h3>
    ${recent.length === 0 ? '<p class="text-muted text-center">No items yet — add some in admin</p>' : `
      <ul class="list-group">
        ${recent.map(i => `
          <li class="list-group-item">
            <a href="/${i.type}s/${i.id}">${i.name}</a>
            <small class="text-muted">· ${i.type}</small>
          </li>
        `).join('')}
      </ul>
    `}
  `;

  res.send(layout('Home', content));
});

// Category pages
app.get('/:cat(drinks|dishes|glassware)', (req, res) => {
  const cat = req.params.cat;
  const type = cat === 'drinks' ? 'drink' : cat === 'dishes' ? 'dish' : 'glassware';

  const items = db.prepare(`
    SELECT id, name, category FROM items
    WHERE type = ? AND archived = 0 ORDER BY name
  `).all(type);

  const content = `
    <h2>${cat.charAt(0).toUpperCase() + cat.slice(1)} (${items.length})</h2>
    ${items.length === 0 ? '<p class="text-muted">Empty for now.</p>' : `
      <ul class="list-group">
        ${items.map(i => `
          <li class="list-group-item">
            <a href="/${cat}/${i.id}">${i.name}</a>
            ${i.category ? `<small class="text-muted">· ${i.category}</small>` : ''}
          </li>
        `).join('')}
      </ul>
    `}
  `;

  res.send(layout(cat.charAt(0).toUpperCase() + cat.slice(1), content));
});

// Item detail
app.get('/:cat(drinks|dishes|glassware)/:id(\\d+)', (req, res) => {
  const item = db.prepare('SELECT * FROM items WHERE id = ?').get(req.params.id);
  if (!item) return res.status(404).send(layout('Not Found', '<h1>404 – Item not found</h1>'));

  let ings = [];
  try { ings = JSON.parse(item.ingredients || '[]'); } catch {}

  const content = `
    <div class="card shadow">
      <div class="card-header">
        <h3 class="mb-0">${item.name}</h3>
        ${item.category ? `<small class="text-muted">${item.category}</small>` : ''}
      </div>
      <div class="card-body">
        ${item.method     ? `<h5>Method</h5><p>${item.method}</p>` : ''}
        ${ings.length     ? `<h5>Ingredients</h5><ul>${ings.map(i => `<li>${i}</li>`).join('')}</ul>` : ''}
        ${item.garnish    ? `<p><strong>Garnish:</strong> ${item.garnish}</p>` : ''}
        ${item.glassware  ? `<p><strong>Glass:</strong> ${item.glassware}</p>` : ''}
        ${item.notes      ? `<p><strong>Notes:</strong> ${item.notes}</p>` : ''}
      </div>
      <div class="card-footer text-muted small">
        Added ${new Date(item.created_at).toLocaleDateString()}
      </div>
    </div>
    <a href="/${req.params.cat}" class="btn btn-outline-secondary mt-3">← Back</a>
  `;

  res.send(layout(item.name, content));
});

// ── Admin ───────────────────────────────────────────────────────
app.get('/admin', (req, res) => {
  if (!res.locals.isAdmin) {
    const content = `
      <div class="row justify-content-center">
        <div class="col-md-5">
          <h2 class="text-center mb-4">Admin Login</h2>
          <form method="POST" action="/admin">
            <div class="mb-3">
              <label class="form-label">Password</label>
              <input type="password" name="pass" class="form-control" required autofocus>
            </div>
            <button type="submit" class="btn btn-primary w-100">Login</button>
          </form>
        </div>
      </div>
    `;
    return res.send(layout('Admin Login', content));
  }

  // Logged in → show form + list
  const items = db.prepare('SELECT id, name, type, created_at FROM items ORDER BY created_at DESC LIMIT 30').all();

  const content = `
    <h2>Admin – Add Item</h2>
    <form method="POST" action="/admin/add" class="mb-5">
      <div class="row g-3">
        <div class="col-md-3">
          <label class="form-label">Type</label>
          <select name="type" class="form-select" required>
            <option value="drink">Drink</option>
            <option value="dish">Dish</option>
            <option value="glassware">Glassware</option>
          </select>
        </div>
        <div class="col-md-9">
          <label class="form-label">Name *</label>
          <input type="text" name="name" class="form-control" required>
        </div>
      </div>
      <div class="mt-3">
        <label class="form-label">Category (optional)</label>
        <input type="text" name="category" class="form-control">
      </div>
      <div class="mt-3">
        <label class="form-label">Method</label>
        <input type="text" name="method" class="form-control">
      </div>
      <div class="mt-3">
        <label class="form-label">Ingredients (one per line)</label>
        <textarea name="ingredients" class="form-control" rows="4"></textarea>
      </div>
      <div class="row g-3 mt-1">
        <div class="col-md-6">
          <label class="form-label">Garnish</label>
          <input type="text" name="garnish" class="form-control">
        </div>
        <div class="col-md-6">
          <label class="form-label">Glassware</label>
          <input type="text" name="glassware" class="form-control">
        </div>
      </div>
      <div class="mt-3">
        <label class="form-label">Notes</label>
        <textarea name="notes" class="form-control" rows="2"></textarea>
      </div>
      <button type="submit" class="btn btn-success mt-4">Add Item</button>
    </form>

    <h3>Recent Items</h3>
    ${items.length ? `
      <div class="table-responsive">
        <table class="table table-sm table-hover">
          <thead><tr><th>ID</th><th>Name</th><th>Type</th><th>Added</th></tr></thead>
          <tbody>
            ${items.map(i => `
              <tr>
                <td>${i.id}</td>
                <td>${i.name}</td>
                <td>${i.type}</td>
                <td>${new Date(i.created_at).toLocaleDateString()}</td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    ` : '<p>No items yet.</p>'}
  `;

  res.send(layout('Admin', content));
});

app.post('/admin', (req, res) => {
  if (req.body.pass === ADMIN_PASS) {
    res.cookie('adminPass', ADMIN_PASS, { httpOnly: true, maxAge: 86400000 });
    res.redirect('/admin');
  } else {
    res.redirect('/admin');
  }
});

app.post('/admin/add', (req, res) => {
  if (!res.locals.isAdmin) return res.status(403).send('Not allowed');

  const { type, name, category, method, ingredients, garnish, glassware, notes } = req.body;

  let ingArray = [];
  if (ingredients?.trim()) {
    ingArray = ingredients.split('\n').map(l => l.trim()).filter(Boolean);
  }

  db.prepare(`
    INSERT INTO items (type, name, category, method, ingredients, garnish, glassware, notes)
    VALUES (?, ?, ?, ?, ?, ?, ?, ?)
  `).run(type, name, category||null, method||null, JSON.stringify(ingArray), garnish||null, glassware||null, notes||null);

  res.redirect('/admin');
});

app.get('/logout', (req, res) => {
  res.clearCookie('adminPass');
  res.redirect('/');
});

// ── Start ───────────────────────────────────────────────────────
app.listen(port, () => {
  console.log(`→ http://localhost:${port}`);
  console.log(`Admin password: ${ADMIN_PASS}  (change it!)`);
});
