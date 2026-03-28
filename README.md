<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pokédex</title>
  <style>
    :root {
      color-scheme: dark;
      --bg: #070b12;
      --surface: #111827;
      --border: #4b0b0b;
      --red: #ef4444;
      --red-soft: #f87171;
      --white: #f8fafc;
      --text: #e5e7eb;
      --muted: #9ca3af;
      --shadow: 0 24px 80px rgba(15, 23, 42, 0.35);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html, body {
      min-height: 100%;
      font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, rgba(239, 68, 68, 0.16), transparent 28%),
                  radial-gradient(circle at 20% 90%, rgba(249, 115, 22, 0.08), transparent 30%),
                  var(--bg);
      color: var(--text);
    }

    body {
      display: grid;
      place-items: center;
      padding: 2rem 2rem 5.5rem;
    }

    .card {
      width: min(520px, 100%);
      background: linear-gradient(180deg, rgba(15, 23, 42, 0.98), rgba(17, 24, 39, 0.95));
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 32px;
      box-shadow: var(--shadow);
      padding: 2.5rem;
      position: relative;
      overflow: hidden;
    }

    .card::before {
      content: "";
      position: absolute;
      inset: 0;
      pointer-events: none;
      background: radial-gradient(circle at top right, rgba(248, 113, 113, 0.18), transparent 30%),
                  radial-gradient(circle at bottom left, rgba(248, 113, 113, 0.08), transparent 24%);
    }

    .header {
      position: relative;
      z-index: 1;
      text-align: center;
      margin-bottom: 2rem;
    }

    .header h1 {
      color: var(--white);
      font-size: clamp(2.5rem, 6vw, 3.6rem);
      letter-spacing: -0.04em;
      margin-bottom: 0.5rem;
    }

    .header p {
      color: var(--muted);
      font-size: 1rem;
      max-width: 34rem;
      margin: 0 auto;
      line-height: 1.7;
    }

    .form {
      position: relative;
      z-index: 1;
      display: flex;
      flex-wrap: wrap;
      gap: 0.75rem;
      justify-content: center;
      margin-bottom: 1.75rem;
    }

    .form input {
      flex: 1 1 220px;
      min-width: 0;
      padding: 1rem 1.1rem;
      border-radius: 18px;
      border: 1px solid rgba(255, 255, 255, 0.12);
      background: rgba(255, 255, 255, 0.04);
      color: var(--white);
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s ease, background 0.2s ease;
    }

    .form input::placeholder {
      color: transparent;
    }

    .form input:focus {
      border-color: rgba(239, 68, 68, 0.8);
      background: rgba(255, 255, 255, 0.08);
    }

    .input-wrap {
      position: relative;
      width: 100%;
      min-width: 0;
    }

    .fake-placeholder {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      padding: 1rem 1.1rem;
      color: rgba(255, 255, 255, 0.62);
      pointer-events: none;
      font-size: 1rem;
      white-space: nowrap;
      overflow: hidden;
      transition: opacity 0.2s ease, visibility 0.2s ease;
    }

    .fake-placeholder .cursor {
      display: inline-block;
      width: 1px;
      height: 1.1em;
      background: var(--white);
      margin-left: 0.35rem;
      animation: blink 1s steps(2, start) infinite;
    }

    .form input:not(:placeholder-shown) + .fake-placeholder,
    .form input:focus + .fake-placeholder {
      opacity: 0;
      visibility: hidden;
    }

    .form button {
      flex: 0 0 140px;
      padding: 1rem 1.2rem;
      border: none;
      border-radius: 18px;
      background: linear-gradient(135deg, #dc2626, #ef4444);
      color: var(--white);
      font-weight: 700;
      font-size: 1rem;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      box-shadow: 0 18px 30px rgba(239, 68, 68, 0.18);
    }

    .form button:hover {
      transform: translateY(-1px);
      box-shadow: 0 22px 42px rgba(239, 68, 68, 0.35), 0 0 0 8px rgba(239, 68, 68, 0.08);
    }

    .top-nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 1rem;
      flex-wrap: wrap;
      margin-bottom: 1.5rem;
      position: relative;
      z-index: 1;
    }

    .top-nav.hidden {
      display: none;
    }

    .nav-links {
      display: flex;
      align-items: center;
      gap: 1rem;
      flex-wrap: wrap;
    }

    .nav-link {
      color: rgba(255, 255, 255, 0.78);
      text-decoration: none;
      padding: 0.75rem 1rem;
      border-radius: 999px;
      transition: background 0.2s ease, color 0.2s ease;
    }

    .nav-link:hover {
      background: rgba(255, 255, 255, 0.08);
    }

    .nav-link.active {
      background: rgba(239, 68, 68, 0.24);
      color: #fff;
      box-shadow: inset 0 0 0 1px rgba(239, 68, 68, 0.18);
    }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    .nav-user {
      color: var(--white);
      font-weight: 700;
      font-size: 0.95rem;
    }

    .nav-logout {
      border: none;
      border-radius: 999px;
      padding: 0.65rem 1rem;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      cursor: pointer;
      transition: background 0.2s ease, transform 0.2s ease;
    }

    .nav-logout:hover {
      transform: translateY(-1px);
      background: rgba(255, 255, 255, 0.14);
    }

    .auth-switch {
      color: var(--muted);
      font-size: 0.95rem;
      margin-top: 1rem;
      text-align: center;
    }

    .auth-switch a {
      color: var(--white);
      text-decoration: none;
    }

    .auth-switch a:hover {
      text-decoration: underline;
    }

    .page {
      display: none;
    }

    .page.active {
      display: block;
    }

    .history {
      position: relative;
      z-index: 1;
      display: none;
      margin-bottom: 1.5rem;
    }

    .history.visible {
      display: block;
    }

    .history-label {
      color: var(--muted);
      font-size: 0.95rem;
      margin-bottom: 0.8rem;
    }

    .history-list {
      display: flex;
      flex-wrap: wrap;
      gap: 0.6rem;
    }

    .history-chip {
      display: inline-flex;
      align-items: center;
      padding: 0.6rem 0.9rem;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      font-size: 0.9rem;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    .history-chip:hover {
      transform: translateY(-1px);
      background: rgba(239, 68, 68, 0.16);
    }

    .input-wrap.shake {
      animation: shake 0.25s ease;
    }

    .spinner {
      display: inline-block;
      width: 1rem;
      height: 1rem;
      border: 3px solid rgba(255, 255, 255, 0.18);
      border-top-color: var(--white);
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
      margin-right: 0.75rem;
      vertical-align: middle;
    }

    .results {
      position: relative;
      z-index: 1;
      border-top: 1px solid rgba(255, 255, 255, 0.08);
      padding-top: 1.5rem;
      color: var(--muted);
      min-height: 10rem;
    }

    .results.empty {
      display: grid;
      place-items: center;
      text-align: center;
      color: rgba(255, 255, 255, 0.65);
    }

    .results .placeholder {
      max-width: 30rem;
      line-height: 1.8;
    }

    .results .item {
      display: grid;
      gap: 0.75rem;
    }

    .type-badges {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
      margin-top: 0.75rem;
    }

    .type-badge {
      display: inline-flex;
      align-items: center;
      padding: 0.35rem 0.8rem;
      border-radius: 999px;
      font-size: 0.85rem;
      font-weight: 700;
      text-transform: capitalize;
      color: #fff;
      letter-spacing: 0.01em;
      box-shadow: inset 0 0 0 1px rgba(255,255,255,0.14);
    }

    .favorite-btn {
      display: inline-flex;
      align-items: center;
      gap: 0.35rem;
      padding: 0.65rem 1rem;
      border: none;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      font-size: 0.95rem;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    .favorite-btn:hover {
      transform: translateY(-1px);
      background: rgba(239, 68, 68, 0.18);
    }

    .favorite-btn.active {
      background: rgba(239, 68, 68, 0.28);
    }

    .favorites {
      position: relative;
      z-index: 1;
      display: none;
      margin-top: 1.75rem;
    }

    .favorites.visible {
      display: block;
    }

    .favorites-label {
      color: var(--muted);
      font-size: 0.95rem;
      margin-bottom: 0.8rem;
    }

    .favorites-list {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
      gap: 0.85rem;
    }

    .favorite-card {
      display: flex;
      align-items: center;
      gap: 0.75rem;
      padding: 0.85rem;
      border-radius: 22px;
      background: rgba(255, 255, 255, 0.06);
      border: 1px solid rgba(255, 255, 255, 0.10);
      color: var(--white);
    }

    .favorite-card img {
      width: 2.25rem;
      height: 2.25rem;
      border-radius: 999px;
      background: rgba(255,255,255,0.12);
      object-fit: contain;
    }

    .favorite-card span {
      font-weight: 700;
      font-size: 0.95rem;
      line-height: 1;
    }

    .favorite-remove {
      margin-left: auto;
      border: none;
      background: transparent;
      color: var(--muted);
      font-size: 1rem;
      cursor: pointer;
    }

    .favorite-remove:hover {
      color: var(--white);
    }

    .favorites-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
      margin-bottom: 1rem;
    }

    .favorites-actions {
      display: flex;
      align-items: center;
      gap: 0.75rem;
      flex-wrap: wrap;
    }

    .favorite-count {
      color: var(--white);
      font-size: 0.95rem;
      font-weight: 700;
    }

    .export-btn {
      border: none;
      border-radius: 999px;
      padding: 0.65rem 1rem;
      background: rgba(239, 68, 68, 0.18);
      color: var(--white);
      cursor: pointer;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    .export-btn:hover {
      transform: translateY(-1px);
      background: rgba(239, 68, 68, 0.28);
    }

    .export-btn:disabled {
      opacity: 0.45;
      cursor: default;
    }

    .detail-page {
      display: none;
      padding-top: 0;
    }

    .detail-page.active {
      display: block;
      animation: fadeInUp 0.32s ease both;
    }

    .detail-header {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.3rem;
    }

    .detail-back {
      border: none;
      border-radius: 999px;
      padding: 0.65rem 1rem;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      cursor: pointer;
      transition: background 0.2s ease;
    }

    .detail-back:hover {
      background: rgba(255, 255, 255, 0.14);
    }

    .detail-grid {
      display: grid;
      gap: 1.25rem;
    }

    .detail-card {
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 24px;
      padding: 1.25rem;
    }

    .detail-stats {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 0.95rem;
      margin-top: 1rem;
    }

    .detail-item {
      display: flex;
      justify-content: space-between;
      gap: 0.75rem;
      color: var(--muted);
      font-size: 0.95rem;
    }

    .detail-item strong {
      color: var(--white);
    }

    .detail-chain {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
      margin-top: 0.8rem;
    }

    .chain-pill {
      padding: 0.45rem 0.8rem;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      font-size: 0.9rem;
    }

    .detail-footer {
      margin-top: 1.5rem;
      padding-top: 1rem;
      border-top: 1px solid rgba(255,255,255,0.08);
      color: var(--muted);
      font-size: 0.95rem;
    }

    .site-footer {
      position: fixed;
      left: 0;
      right: 0;
      bottom: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0.85rem 1rem;
      background: rgba(7, 11, 18, 0.96);
      color: var(--muted);
      border-top: 1px solid rgba(255,255,255,0.08);
      z-index: 20;
      font-size: 0.95rem;
    }

    .results.fade-in .item {
      animation: fadeInUp 0.32s ease both;
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-8px); }
      40%, 80% { transform: translateX(8px); }
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(12px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes blink {
      0%, 49% {
        opacity: 1;
      }
      50%, 100% {
        opacity: 0;
      }
    }

    @media (max-width: 420px) {
      .card {
        padding: 1.75rem;
      }

      .form button {
        width: 100%;
      }
    }
  
    .filter-bar {
      display: flex;
      flex-wrap: wrap;
      gap: 0.75rem;
      justify-content: center;
      margin-bottom: 1rem;
    }

    .filter-btn {
      border: none;
      border-radius: 18px;
      padding: 0.95rem 1.2rem;
      background: rgba(239, 68, 68, 0.18);
      color: var(--white);
      font-weight: 700;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    .filter-btn:hover {
      transform: translateY(-1px);
      background: rgba(239, 68, 68, 0.28);
    }

    .filter-label {
      color: var(--muted);
      font-size: 0.95rem;
      align-self: center;
    }

    .filter-panel {
      display: none;
      flex-wrap: wrap;
      gap: 0.55rem;
      justify-content: center;
      padding: 0.85rem 0.6rem 0.6rem;
      border-radius: 22px;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 255, 255, 0.08);
      margin-bottom: 1.5rem;
    }

    .filter-panel.visible {
      display: flex;
    }

    .filter-chip {
      border: none;
      border-radius: 999px;
      padding: 0.65rem 0.9rem;
      background: rgba(255, 255, 255, 0.08);
      color: var(--white);
      font-size: 0.85rem;
      cursor: pointer;
      text-transform: capitalize;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    .filter-chip:hover {
      transform: translateY(-1px);
      background: rgba(239, 68, 68, 0.16);
    }

    .filter-chip.active {
      background: linear-gradient(135deg, #dc2626, #ef4444);
      color: #fff;
    }
</style>
</head>
<body>
  <nav class="top-nav hidden">
    <div class="nav-links">
      <a href="#home" class="nav-link">Home</a>
      <a href="#favorites" class="nav-link">Favorites</a>
      <a href="#about" class="nav-link">About</a>
    </div>
    <div class="nav-right">
      <span id="navUser" class="nav-user">Guest</span>
      <button id="logoutBtn" class="nav-logout" type="button" hidden>Logout</button>
    </div>
  </nav>

  <main class="card">
    <section id="loginPage" class="page active">
      <section class="header">
        <h1>Welcome back</h1>
        <p>Login to access your personal Pokédex favorites and saved search history.</p>
      </section>

      <section class="form">
        <div class="input-wrap">
          <input id="loginUsername" type="text" placeholder=" " aria-label="Username" />
          <span class="fake-placeholder">Username<span class="cursor"></span></span>
        </div>
        <div class="input-wrap">
          <input id="loginPassword" type="password" placeholder=" " aria-label="Password" />
          <span class="fake-placeholder">Password<span class="cursor"></span></span>
        </div>
        <button id="loginBtn" type="button">Login</button>
      </section>

      <p class="auth-switch">Don't have an account? <a href="#signup">Sign up</a></p>
      <section id="loginError" class="results empty" style="display:none;"><div class="placeholder"></div></section>
    </section>

    <section id="signupPage" class="page">
      <section class="header">
        <h1>Create account</h1>
        <p>Sign up for a secure Pokédex profile and keep your favorites private.</p>
      </section>

      <section class="form">
        <div class="input-wrap">
          <input id="signupUsername" type="text" placeholder=" " aria-label="Username" />
          <span class="fake-placeholder">Username<span class="cursor"></span></span>
        </div>
        <div class="input-wrap">
          <input id="signupPassword" type="password" placeholder=" " aria-label="Password" />
          <span class="fake-placeholder">Password<span class="cursor"></span></span>
        </div>
        <button id="signupBtn" type="button">Sign Up</button>
      </section>

      <p class="auth-switch">Already have an account? <a href="#login">Log in</a></p>
      <section id="signupError" class="results empty" style="display:none;"><div class="placeholder"></div></section>
    </section>

    <section id="homePage" class="page">
      <section class="header">
        <h1>🔴⚪️ Pokédex</h1>
        <p>Search for a Pokémon by name and see the results instantly. This modern Pokédex-inspired UI is responsive, centered, and made for mobile.</p>
      </section>

      <section class="form">
        <div class="input-wrap">
          <input id="pokemonName" type="text" placeholder=" " aria-label="Pokémon name" />
          <span class="fake-placeholder">Enter Pokémon name...<span class="cursor"></span></span>
        </div>
        <button id="searchBtn" type="button">Search</button>
      </section>

      <section class="filter-bar">
        <button id="filterToggle" class="filter-btn" type="button">Filter by type</button>
        <span id="typeFilterLabel" class="filter-label">All types</span>
      </section>

      <section id="filterPanel" class="filter-panel"></section>

      <section id="history" class="history">
        <div class="history-label">Recent searches</div>
        <div id="historyList" class="history-list"></div>
      </section>

      <section id="results" class="results empty">
        <div class="placeholder">Search for a Pokémon to display results here.</div>
      </section>
    </section>

    <section id="favoritesPage" class="page">
      <div class="favorites-header">
        <div>
          <div class="favorites-label">My Favorites</div>
          <div id="favoriteCount" class="favorite-count">0 favorites</div>
        </div>
        <button id="exportBtn" class="export-btn" type="button">Export JSON</button>
      </div>
      <div id="favoritesList" class="favorites-list"></div>
    </section>

    <section id="aboutPage" class="page">
      <div class="header">
        <h2>About the Pokédex</h2>
        <p class="about-content">This Pokédex app uses the <a href="https://pokeapi.co/" target="_blank" rel="noreferrer noopener">PokéAPI</a> to fetch Pokémon details in real-time. It is built as a single-page experience with client-side routing, favorites and export features, and a polished mobile-first interface.</p>
      </div>
    </section>

    <section id="detailPage" class="page detail-page">
      <div class="detail-header">
        <button id="detailBack" class="detail-back" type="button">← Back</button>
        <h2 id="detailTitle">Pokémon detail</h2>
      </div>
      <div id="detailContent" class="detail-content"></div>
    </section>
  </main>

  <footer class="site-footer">Built with PokéAPI · Single-page Pokédex experience</footer>

  <script>
const nameInput = document.getElementById('pokemonName');
const searchBtn = document.getElementById('searchBtn');
const results = document.getElementById('results');
const historySection = document.getElementById('history');
const historyList = document.getElementById('historyList');
const favoritesList = document.getElementById('favoritesList');
const favoriteCount = document.getElementById('favoriteCount');
const exportBtn = document.getElementById('exportBtn');
const detailTitle = document.getElementById('detailTitle');
const detailContent = document.getElementById('detailContent');
const detailBack = document.getElementById('detailBack');
const navLinks = document.querySelectorAll('.nav-link');
const pages = document.querySelectorAll('.page');
const inputWrap = document.querySelector('.input-wrap');
const filterToggle = document.getElementById('filterToggle');
const filterPanel = document.getElementById('filterPanel');
const typeFilterLabel = document.getElementById('typeFilterLabel');

let searchHistory = [];
let favorites = [];
let currentPokemon = null;
let db = null;
let activeTypeFilter = 'all';

const typeColors = {
  normal: '#A8A77A', fire: '#EE8130', water: '#6390F0', electric: '#F7D02C', grass: '#7AC74C', ice: '#96D9D6',
  fighting: '#C22E28', poison: '#A33EA1', ground: '#E2BF65', flying: '#A98FF3', psychic: '#F95587', bug: '#A6B91A',
  rock: '#B6A136', ghost: '#735797', dragon: '#6F35FC', dark: '#705746', steel: '#B7B7CE', fairy: '#D685AD'
};
const allTypes = ['all', ...Object.keys(typeColors)];

function capitalize(value) {
  return value.charAt(0).toUpperCase() + value.slice(1);
}

function renderEmpty() {
  results.className = 'results empty';
  results.innerHTML = '<div class="placeholder">Search for a Pokémon to display results here.</div>';
}

function renderLoading() {
  results.className = 'results';
  results.innerHTML = '<div class="placeholder"><span class="spinner"></span>Loading Pokémon...</div>';
}

function renderError(message) {
  results.className = 'results empty';
  results.innerHTML = `<div class="placeholder">${message}</div>`;
}

function renderTypeBadge(typeName) {
  const color = typeColors[typeName] || '#777';
  return `<span class="type-badge" style="background: ${color};">${capitalize(typeName)}</span>`;
}

function filterMatches(types) {
  if (activeTypeFilter === 'all') return true;
  return types.includes(activeTypeFilter);
}

function renderResult(data) {
  const typeNames = data.types.map(type => type.type ? type.type.name : type);
  if (!filterMatches(typeNames)) {
    results.className = 'results empty';
    results.innerHTML = '<div class="placeholder">No Pokémon matches the selected filter. Clear the filter or choose another type.</div>';
    return;
  }

  const name = capitalize(data.name);
  const typesList = typeNames;
  const typeBadges = typesList.map(renderTypeBadge).join('');
  const hp = data.stats.find(stat => stat.stat.name === 'hp')?.base_stat ?? '—';
  const attack = data.stats.find(stat => stat.stat.name === 'attack')?.base_stat ?? '—';
  const defense = data.stats.find(stat => stat.stat.name === 'defense')?.base_stat ?? '—';
  const sprite = data.sprites.front_default;
  const height = (data.height / 10).toFixed(1);
  const weight = (data.weight / 10).toFixed(1);
  currentPokemon = {
    name,
    image: sprite,
    types: typesList,
    stats: data.stats,
    height: data.height,
    weight: data.weight
  };

  results.className = 'results fade-in';
  results.innerHTML = `
      <div class="item">
        <div style="display: flex; flex-wrap: wrap; gap: 1rem; align-items: center;">
          <div style="flex: 0 0 120px;">
            ${sprite ? `<img src="${sprite}" alt="${name} sprite" style="width: 100%; border-radius: 22px; background: rgba(255,255,255,0.06); padding: 0.75rem;" />` : ''}
          </div>
          <div style="min-width: 0; flex: 1 1 220px;">
            <div style="display: flex; flex-wrap: wrap; gap: 0.75rem; align-items: center; margin-bottom: 0.75rem;">
              <h2 style="color: var(--white); margin: 0;">${name}</h2>
              <button class="favorite-btn${isFavorite(name) ? ' active' : ''}" type="button" data-name="${name}">
                ${isFavorite(name) ? '★ Favorited' : '☆ Favorite'}
              </button>
            </div>
            <div class="type-badges">${typeBadges}</div>
            <p style="font-size: 0.95rem; color: #f8fafc; margin: 1rem 0 0.2rem;"><strong>Height:</strong> ${height} m • <strong>Weight:</strong> ${weight} kg</p>
            <p style="display: flex; flex-wrap: wrap; gap: 0.75rem; color: var(--muted); font-size: 0.96rem; line-height: 1.6; margin: 0;">
              <span>HP: <strong style="color: var(--white);">${hp}</strong></span>
              <span>Attack: <strong style="color: var(--white);">${attack}</strong></span>
              <span>Defense: <strong style="color: var(--white);">${defense}</strong></span>
            </p>
          </div>
        </div>
      </div>
    `;
}

function renderHistory() {
  if (!searchHistory.length) {
    historySection.classList.remove('visible');
    historyList.innerHTML = '';
    return;
  }
  historySection.classList.add('visible');
  historyList.innerHTML = searchHistory
    .map(entry => `<button type="button" class="history-chip" data-name="${entry}">${capitalize(entry)}</button>`)
    .join('');
}

function triggerShake() {
  inputWrap.classList.add('shake');
  setTimeout(() => inputWrap.classList.remove('shake'), 260);
}

function renderFavorites() {
  const filtered = favorites.filter(item => filterMatches(item.types));
  favoriteCount.textContent = `${filtered.length} favorite${filtered.length === 1 ? '' : 's'}`;
  exportBtn.disabled = filtered.length === 0;

  if (!filtered.length) {
    favoritesList.innerHTML = '<div class="placeholder">No favorites match the selected filter. Reset the filter or add more favorites.</div>';
    return;
  }

  favoritesList.innerHTML = filtered.map(item => `
      <div class="favorite-card" data-name="${item.name}">
        <img src="${item.image}" alt="${item.name} sprite" />
        <span>${item.name}</span>
        <button type="button" class="favorite-remove" data-name="${item.name}" aria-label="Remove ${item.name}">×</button>
      </div>
    `).join('');
}

function addToHistory(name) {
  const normalized = name.toLowerCase();
  searchHistory = searchHistory.filter(entry => entry !== normalized);
  searchHistory.unshift(normalized);
  if (searchHistory.length > 5) searchHistory.pop();
  renderHistory();
}

function openDatabase() {
  return new Promise((resolve, reject) => {
    if (!window.indexedDB) {
      reject(new Error('IndexedDB not supported'));
      return;
    }

    const request = indexedDB.open('pokedexDb', 1);
    request.onupgradeneeded = event => {
      const database = event.target.result;
      if (!database.objectStoreNames.contains('favorites')) {
        database.createObjectStore('favorites', { keyPath: 'name' });
      }
    };

    request.onsuccess = event => {
      db = event.target.result;
      resolve();
    };

    request.onerror = () => reject(request.error);
  });
}

function getAllFavorites() {
  return new Promise((resolve, reject) => {
    const tx = db.transaction('favorites', 'readonly');
    const store = tx.objectStore('favorites');
    const request = store.getAll();
    request.onsuccess = () => resolve(request.result || []);
    request.onerror = () => reject(request.error);
  });
}

function putFavorite(item) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction('favorites', 'readwrite');
    const store = tx.objectStore('favorites');
    const request = store.put(item);
    request.onsuccess = () => resolve();
    request.onerror = () => reject(request.error);
  });
}

function deleteFavoriteFromDb(name) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction('favorites', 'readwrite');
    const store = tx.objectStore('favorites');
    const request = store.delete(name);
    request.onsuccess = () => resolve();
    request.onerror = () => reject(request.error);
  });
}

function loadFavorites() {
  return getAllFavorites().then(items => {
    favorites = items;
    renderFavorites();
  });
}

function isFavorite(name) {
  return favorites.some(item => item.name.toLowerCase() === name.toLowerCase());
}

function addFavorite(pokemon) {
  if (!pokemon || !pokemon.name) return;
  if (isFavorite(pokemon.name)) return;
  const item = {
    name: pokemon.name,
    image: pokemon.image,
    types: pokemon.types
  };
  favorites.unshift(item);
  putFavorite(item).then(() => renderFavorites());
}

function removeFavorite(name) {
  favorites = favorites.filter(item => item.name.toLowerCase() !== name.toLowerCase());
  deleteFavoriteFromDb(name).then(() => {
    renderFavorites();
    if (currentPokemon && currentPokemon.name.toLowerCase() === name.toLowerCase()) {
      renderResult({
        name: currentPokemon.name,
        image: currentPokemon.image,
        types: currentPokemon.types,
        stats: currentPokemon.stats,
        height: currentPokemon.height,
        weight: currentPokemon.weight
      });
    }
  });
}

function toggleFilterPanel() {
  filterPanel.classList.toggle('visible');
}

function setFilter(type) {
  activeTypeFilter = type;
  typeFilterLabel.textContent = type === 'all' ? 'All types' : `${capitalize(type)} only`;
  buildFilterPanel();
  if (currentPokemon) {
    renderResult({
      name: currentPokemon.name,
      image: currentPokemon.image,
      types: currentPokemon.types,
      stats: currentPokemon.stats,
      height: currentPokemon.height,
      weight: currentPokemon.weight
    });
  }
  renderFavorites();
}

function buildFilterPanel() {
  filterPanel.innerHTML = allTypes.map(type => `
      <button type="button" class="filter-chip${type === activeTypeFilter ? ' active' : ''}" data-type="${type}">${type}</button>
    `).join('');
}

function handleSearch(searchTerm) {
  const rawValue = typeof searchTerm === 'string' ? searchTerm.trim() : nameInput.value.trim();
  if (!rawValue) {
    triggerShake();
    renderEmpty();
    return;
  }
  nameInput.value = rawValue;
  renderLoading();
  fetch(`https://pokeapi.co/api/v2/pokemon/${encodeURIComponent(rawValue.toLowerCase())}`)
    .then(response => {
      if (!response.ok) throw new Error('not-found');
      return response.json();
    })
    .then(data => {
      addToHistory(rawValue);
      renderResult(data);
    })
    .catch(error => {
      if (error.message === 'not-found') {
        renderError(`Sorry, we couldn't find "${rawValue}". Try another Pokémon name.`);
      } else {
        renderError('Unable to fetch Pokémon data. Check your connection and try again.');
      }
    });
}

function showPage(route) {
  const normalized = route ? route.toLowerCase() : 'home';
  let pageId = 'homePage';
  if (normalized === 'favorites') pageId = 'favoritesPage';
  else if (normalized === 'about') pageId = 'aboutPage';
  else if (normalized.startsWith('detail/')) pageId = 'detailPage';
  pages.forEach(page => page.classList.toggle('active', page.id === pageId));
  navLinks.forEach(link => {
    const href = link.getAttribute('href').slice(1);
    link.classList.toggle('active', href === normalized);
  });
}

function handleRoute() {
  const hash = window.location.hash.slice(1);
  if (hash.startsWith('detail/')) {
    const name = hash.replace('detail/', '');
    showPage(hash);
    showDetail(name);
    return;
  }
  showPage(hash || 'home');
}

function renderDetailLoading() {
  detailTitle.textContent = 'Loading details';
  detailContent.innerHTML = '<div class="placeholder"><span class="spinner"></span>Loading details...</div>';
}

function parseEvolutionChain(chain) {
  const names = [chain.species.name];
  if (chain.evolves_to?.length) {
    chain.evolves_to.forEach(next => names.push(...parseEvolutionChain(next)));
  }
  return names;
}

function showDetail(name) {
  if (!name) return;
  detailTitle.textContent = capitalize(name);
  detailContent.innerHTML = '';
  renderDetailLoading();
  const query = encodeURIComponent(name.toLowerCase());
  fetch(`https://pokeapi.co/api/v2/pokemon/${query}`)
    .then(res => {
      if (!res.ok) throw new Error('not-found');
      return res.json();
    })
    .then(data => {
      const abilities = data.abilities.map(a => capitalize(a.ability.name)).join(', ');
      const stats = data.stats.reduce((acc, stat) => {
        acc[stat.stat.name] = stat.base_stat;
        return acc;
      }, {});
      const types = data.types.map(t => t.type.name);
      const sprite = data.sprites.front_default;
      return fetch(data.species.url)
        .then(res => res.json())
        .then(species => fetch(species.evolution_chain.url))
        .then(res => res.json())
        .then(chainData => ({ data, abilities, stats, types, sprite, evolution: chainData.chain }));
    })
    .then(({ data, abilities, stats, types, sprite, evolution }) => {
      const chainNames = parseEvolutionChain(evolution);
      detailContent.innerHTML = `
        <div class="detail-grid">
          <div class="detail-card">
            <div style="display:flex; gap:1rem; align-items:center; flex-wrap:wrap;">
              ${sprite ? `<img src="${sprite}" alt="${capitalize(data.name)} sprite" style="width:120px; border-radius:24px; background: rgba(255,255,255,0.08); padding:0.75rem;" />` : ''}
              <div>
                <h3 style="margin:0; color: var(--white);">${capitalize(data.name)}</h3>
                <div class="type-badges">${types.map(renderTypeBadge).join('')}</div>
              </div>
            </div>
            <div class="detail-stats">
              <div class="detail-item"><span>HP</span><strong>${stats.hp ?? '—'}</strong></div>
              <div class="detail-item"><span>Attack</span><strong>${stats.attack ?? '—'}</strong></div>
              <div class="detail-item"><span>Defense</span><strong>${stats.defense ?? '—'}</strong></div>
              <div class="detail-item"><span>Speed</span><strong>${stats.speed ?? '—'}</strong></div>
              <div class="detail-item"><span>Sp. Attack</span><strong>${stats['special-attack'] ?? '—'}</strong></div>
              <div class="detail-item"><span>Sp. Defense</span><strong>${stats['special-defense'] ?? '—'}</strong></div>
            </div>
          </div>
          <div class="detail-card">
            <h3 style="margin-top:0; color: var(--white);">Abilities</h3>
            <p style="margin:0; color: var(--muted);">${abilities}</p>
            <h3 style="margin:1rem 0 0.5rem 0; color: var(--white);">Evolution chain</h3>
            <div class="detail-chain">${chainNames.map(name => `<span class="chain-pill">${capitalize(name)}</span>`).join('')}</div>
          </div>
        </div>
        <div class="detail-footer">Data provided by PokéAPI. Tap back to return to the previous view.</div>
      `;
    })
    .catch(error => {
      detailContent.innerHTML = `<div class="placeholder">Unable to load details. ${error.message === 'not-found' ? 'Pokémon not found.' : 'Please try again.'}</div>`;
    });
}

detailBack.addEventListener('click', () => {
  window.location.hash = '#home';
});

filterToggle.addEventListener('click', toggleFilterPanel);
filterPanel.addEventListener('click', event => {
  const button = event.target.closest('.filter-chip');
  if (!button) return;
  setFilter(button.dataset.type);
});

searchBtn.addEventListener('click', () => handleSearch());
nameInput.addEventListener('keydown', event => {
  if (event.key === 'Enter') {
    event.preventDefault();
    handleSearch();
  }
});

results.addEventListener('click', event => {
  const button = event.target.closest('.favorite-btn');
  if (!button) return;
  const name = button.dataset.name;
  if (!currentPokemon || currentPokemon.name !== name) return;
  if (isFavorite(name)) removeFavorite(name);
  else addFavorite(currentPokemon);
  renderResult({
    name: currentPokemon.name,
    image: currentPokemon.image,
    types: currentPokemon.types,
    stats: currentPokemon.stats,
    height: currentPokemon.height,
    weight: currentPokemon.weight
  });
});

historyList.addEventListener('click', event => {
  const target = event.target.closest('.history-chip');
  if (!target) return;
  handleSearch(target.dataset.name);
});

favoritesList.addEventListener('click', event => {
  const button = event.target.closest('.favorite-remove');
  if (!button) return;
  removeFavorite(button.dataset.name);
});

exportBtn.addEventListener('click', exportFavorites);

window.addEventListener('hashchange', handleRoute);
window.addEventListener('click', event => {
  if (!filterPanel.contains(event.target) && event.target !== filterToggle) {
    filterPanel.classList.remove('visible');
  }
});

buildFilterPanel();
setFilter('all');
renderEmpty();
openDatabase()
  .then(loadFavorites)
  .catch(() => {
    renderFavorites();
  });
handleRoute();
</script>
</body>
</html>
