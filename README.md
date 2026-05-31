[dashboard.html](https://github.com/user-attachments/files/28442408/dashboard.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Dashboard</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>
    :root {
      --bg: #0f1117;
      --surface: #1a1d27;
      --border: #2a2d3e;
      --accent: #6c63ff;
      --accent2: #00d4aa;
      --text: #e2e8f0;
      --muted: #8892a4;
      --danger: #ff4d6d;
      --warn: #f59e0b;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Segoe UI', system-ui, sans-serif;
      min-height: 100vh;
    }

    /* ── LOGIN ── */
    #login-screen {
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      background: radial-gradient(ellipse at 50% 0%, #1e1b4b 0%, var(--bg) 70%);
    }

    .login-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 48px 40px;
      width: 100%;
      max-width: 380px;
      text-align: center;
      box-shadow: 0 24px 60px rgba(0,0,0,0.5);
    }

    .login-card h1 { font-size: 1.6rem; margin-bottom: 8px; }
    .login-card p  { color: var(--muted); font-size: 0.9rem; margin-bottom: 32px; }

    .login-card input {
      width: 100%;
      padding: 14px 16px;
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: 8px;
      color: var(--text);
      font-size: 1rem;
      margin-bottom: 12px;
      outline: none;
      transition: border-color .2s;
    }
    .login-card input:focus { border-color: var(--accent); }

    .login-card button {
      width: 100%;
      padding: 14px;
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity .2s;
    }
    .login-card button:hover { opacity: .85; }

    #login-error {
      color: var(--danger);
      font-size: .85rem;
      margin-top: 12px;
      display: none;
    }

    /* ── DASHBOARD ── */
    #dashboard { display: none; }

    header {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 16px 32px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    header h1 { font-size: 1.2rem; letter-spacing: .02em; }
    header .meta { color: var(--muted); font-size: .82rem; display: flex; align-items: center; gap: 12px; }
    header .meta span { display: flex; align-items: center; gap: 5px; }

    .dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: var(--accent2);
      box-shadow: 0 0 6px var(--accent2);
      animation: pulse 2s infinite;
    }
    @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }

    .logout-btn {
      background: transparent;
      border: 1px solid var(--border);
      color: var(--muted);
      padding: 6px 14px;
      border-radius: 6px;
      cursor: pointer;
      font-size: .82rem;
      transition: color .2s, border-color .2s;
    }
    .logout-btn:hover { color: var(--text); border-color: var(--text); }

    main { padding: 32px; max-width: 1300px; margin: 0 auto; }

    /* ── KPI CARDS ── */
    .kpi-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 16px;
      margin-bottom: 32px;
    }

    .kpi-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 24px 20px;
      position: relative;
      overflow: hidden;
    }
    .kpi-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
      background: var(--accent);
    }
    .kpi-card:nth-child(2)::before { background: var(--accent2); }
    .kpi-card:nth-child(3)::before { background: var(--warn); }
    .kpi-card:nth-child(4)::before { background: var(--danger); }

    .kpi-label { color: var(--muted); font-size: .8rem; text-transform: uppercase; letter-spacing: .08em; margin-bottom: 8px; }
    .kpi-value { font-size: 2rem; font-weight: 700; line-height: 1; }
    .kpi-sub   { color: var(--muted); font-size: .78rem; margin-top: 6px; }

    /* ── CHARTS ── */
    .charts-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
      gap: 20px;
      margin-bottom: 32px;
    }

    .chart-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 24px;
    }
    .chart-card h2 { font-size: .95rem; color: var(--muted); margin-bottom: 20px; }
    .chart-card canvas { max-height: 260px; }

    /* ── TABLE ── */
    .table-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 24px;
      overflow-x: auto;
    }
    .table-card h2 { font-size: .95rem; color: var(--muted); margin-bottom: 20px; }

    table { width: 100%; border-collapse: collapse; font-size: .88rem; }
    thead th {
      text-align: left;
      padding: 10px 14px;
      color: var(--muted);
      font-weight: 500;
      font-size: .78rem;
      text-transform: uppercase;
      letter-spacing: .06em;
      border-bottom: 1px solid var(--border);
    }
    tbody tr { border-bottom: 1px solid var(--border); transition: background .15s; }
    tbody tr:last-child { border-bottom: none; }
    tbody tr:hover { background: rgba(108,99,255,.06); }
    tbody td { padding: 12px 14px; }

    /* ── LOADING / ERROR ── */
    #loading {
      text-align: center;
      padding: 80px 20px;
      color: var(--muted);
    }
    .spinner {
      width: 36px; height: 36px;
      border: 3px solid var(--border);
      border-top-color: var(--accent);
      border-radius: 50%;
      animation: spin .8s linear infinite;
      margin: 0 auto 16px;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    #error-msg {
      background: rgba(255,77,109,.1);
      border: 1px solid rgba(255,77,109,.3);
      border-radius: 10px;
      padding: 20px 24px;
      color: var(--danger);
      margin: 40px 0;
      display: none;
    }

    @media (max-width: 600px) {
      main { padding: 16px; }
      header { padding: 12px 16px; }
      .charts-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<!-- ═══ LOGIN ═══════════════════════════════════════════════ -->
<div id="login-screen">
  <div class="login-card">
    <h1>🔐 Dashboard</h1>
    <p>Digite a senha para acessar</p>
    <input type="password" id="pwd-input" placeholder="Senha" autocomplete="off" />
    <button onclick="checkPassword()">Entrar</button>
    <div id="login-error">Senha incorreta. Tente novamente.</div>
  </div>
</div>

<!-- ═══ DASHBOARD ════════════════════════════════════════════ -->
<div id="dashboard">
  <header>
    <h1>📊 Dashboard</h1>
    <div class="meta">
      <span><div class="dot"></div> Dados ao vivo</span>
      <span id="last-update"></span>
      <button class="logout-btn" onclick="logout()">Sair</button>
    </div>
  </header>

  <main>
    <div id="loading">
      <div class="spinner"></div>
      Carregando dados da planilha…
    </div>

    <div id="error-msg"></div>

    <div id="content" style="display:none">
      <!-- KPIs -->
      <div class="kpi-grid" id="kpi-grid"></div>

      <!-- Gráficos -->
      <div class="charts-grid" id="charts-grid"></div>

      <!-- Tabela -->
      <div class="table-card">
        <h2>DADOS COMPLETOS</h2>
        <table id="data-table">
          <thead id="table-head"></thead>
          <tbody id="table-body"></tbody>
        </table>
      </div>
    </div>
  </main>
</div>

<script>
// ══════════════════════════════════════════════════════════
//  CONFIGURAÇÃO — edite apenas esta seção
// ══════════════════════════════════════════════════════════
const CONFIG = {
  // URL gerada em Arquivo → Publicar na web → CSV
  PUBLISH_CSV_URL: "https://docs.google.com/spreadsheets/d/e/2PACX-1vSHRISxbEqOewN1jZyxd_ul9xQdeLFxR7APLiWUaPGhRD9v6T93NHjbbdIEaVqDVxj6W-F-36UTc_Bu/pub?gid=1385223240&single=true&output=csv",

  // Senha de acesso ao dashboard
  // Para trocar: gere um hash SHA-256 em https://emn178.github.io/online-tools/sha256.html
  // e cole abaixo. A senha padrão é: Dashboard@2024
  PASSWORD_HASH: "49796da0976baffa02e2cc8de8c7f943d1b6bd250602b452c8f5ef096055888f",

  // Intervalo de atualização automática (ms). 60000 = 1 minuto
  REFRESH_MS: 60000,

  // Título do dashboard
  TITLE: "Dashboard"
};
// ══════════════════════════════════════════════════════════

// Atualiza o título
document.querySelectorAll("h1").forEach(h => {
  if (!h.textContent.includes("🔐") && !h.textContent.includes("📊")) return;
  if (h.textContent.includes("📊")) h.textContent = "📊 " + CONFIG.TITLE;
  if (h.textContent.includes("🔐")) h.textContent = "🔐 " + CONFIG.TITLE;
});

/* ── AUTENTICAÇÃO ── */
async function sha256(str) {
  const buf = await crypto.subtle.digest("SHA-256", new TextEncoder().encode(str));
  return Array.from(new Uint8Array(buf)).map(b => b.toString(16).padStart(2,"0")).join("");
}

async function checkPassword() {
  const pwd = document.getElementById("pwd-input").value;
  const hash = await sha256(pwd);
  if (hash === CONFIG.PASSWORD_HASH) {
    sessionStorage.setItem("auth", hash);
    showDashboard();
  } else {
    document.getElementById("login-error").style.display = "block";
    document.getElementById("pwd-input").value = "";
  }
}

document.getElementById("pwd-input").addEventListener("keydown", e => {
  if (e.key === "Enter") checkPassword();
});

function logout() {
  sessionStorage.removeItem("auth");
  location.reload();
}

async function checkSession() {
  const saved = sessionStorage.getItem("auth");
  if (saved && saved === CONFIG.PASSWORD_HASH) {
    showDashboard();
  }
}

function showDashboard() {
  document.getElementById("login-screen").style.display = "none";
  document.getElementById("dashboard").style.display = "block";
  loadData();
  setInterval(loadData, CONFIG.REFRESH_MS);
}

/* ── BUSCA DE DADOS (Publish to Web CSV — sem CORS) ── */
async function loadData() {
  try {
    const url = CONFIG.PUBLISH_CSV_URL;

    const res = await fetch(url, { cache: "no-cache" });
    if (!res.ok) throw new Error(`HTTP ${res.status} — planilha pode estar privada`);
    const csv = await res.text();

    const table = parseCSV(csv);
    renderDashboard(table);
    document.getElementById("last-update").textContent =
      "Atualizado: " + new Date().toLocaleTimeString("pt-BR");

  } catch (err) {
    document.getElementById("loading").style.display = "none";
    const errDiv = document.getElementById("error-msg");
    errDiv.style.display = "block";
    errDiv.innerHTML = `
      <strong>Não foi possível carregar os dados.</strong><br>
      ${err.message}<br><br>
      Verifique se a planilha está com acesso público (Compartilhar → Qualquer pessoa com o link → Visualizador).
    `;
  }
}

/* ── PARSER CSV → formato de tabela compatível com renderDashboard ── */
function parseCSV(csv) {
  const lines = csv.trim().split(/\r?\n/);
  if (lines.length < 1) throw new Error("Planilha vazia");

  function splitLine(line) {
    const result = []; let cur = ""; let inQ = false;
    for (let i = 0; i < line.length; i++) {
      const c = line[i];
      if (c === '"' && line[i+1] === '"') { cur += '"'; i++; }
      else if (c === '"') { inQ = !inQ; }
      else if (c === ',' && !inQ) { result.push(cur.trim()); cur = ""; }
      else { cur += c; }
    }
    result.push(cur.trim());
    return result;
  }

  const headers = splitLine(lines[0]);
  const cols = headers.map(h => ({ label: h, type: "string" }));

  const rows = lines.slice(1).map(line => {
    const vals = splitLine(line);
    return {
      c: vals.map(v => {
        if (v === "" || v == null) return { v: null };
        const num = Number(v.replace(/[R$\s%.,]/g, '').replace(',', '.'));
        return isNaN(num) || v === "" ? { v: v } : { v: num, f: v };
      })
    };
  }).filter(r => r.c.some(c => c.v != null && c.v !== ""));

  // Detecta tipos das colunas
  cols.forEach((col, i) => {
    const vals = rows.map(r => r.c[i]?.v).filter(v => v != null && v !== "");
    col.type = vals.every(v => typeof v === "number") ? "number" : "string";
  });

  return { cols, rows };
}

/* ── RENDERIZAÇÃO ── */
let charts = [];

function renderDashboard(table) {
  document.getElementById("loading").style.display  = "none";
  document.getElementById("content").style.display  = "block";

  const cols = table.cols;
  const rows = table.rows.filter(r => r.c && r.c.some(c => c && c.v != null));

  // Detecta colunas numéricas e de texto
  const numCols  = cols.map((c,i) => ({...c, i})).filter(c => c.type === "number");
  const textCols = cols.map((c,i) => ({...c, i})).filter(c => c.type === "string");

  // ── KPIs: soma + média das colunas numéricas (máx 4) ──
  const kpiGrid = document.getElementById("kpi-grid");
  kpiGrid.innerHTML = "";
  numCols.slice(0,4).forEach(col => {
    const vals = rows.map(r => r.c[col.i]?.v ?? 0).filter(v => typeof v === "number");
    const sum  = vals.reduce((a,b) => a+b, 0);
    const avg  = vals.length ? sum/vals.length : 0;
    const fmt  = n => Number.isInteger(n) ? n.toLocaleString("pt-BR") : n.toLocaleString("pt-BR",{minimumFractionDigits:2,maximumFractionDigits:2});
    kpiGrid.innerHTML += `
      <div class="kpi-card">
        <div class="kpi-label">${col.label || col.id}</div>
        <div class="kpi-value">${fmt(sum)}</div>
        <div class="kpi-sub">Média: ${fmt(avg)} · ${vals.length} registros</div>
      </div>`;
  });

  // ── Gráficos ──
  charts.forEach(c => c.destroy());
  charts = [];
  const chartsGrid = document.getElementById("charts-grid");
  chartsGrid.innerHTML = "";

  // 1. Gráfico de barras: 1ª coluna numérica agrupada pela 1ª coluna de texto
  if (numCols.length > 0 && textCols.length > 0) {
    const tc = textCols[0];
    const nc = numCols[0];
    // Agrega por label
    const agg = {};
    rows.forEach(r => {
      const lbl = String(r.c[tc.i]?.v ?? "—");
      const val = r.c[nc.i]?.v ?? 0;
      agg[lbl] = (agg[lbl] || 0) + val;
    });
    const labels = Object.keys(agg).slice(0,20);
    const data   = labels.map(l => agg[l]);

    chartsGrid.innerHTML += `<div class="chart-card"><h2>${(nc.label||nc.id).toUpperCase()} POR ${(tc.label||tc.id).toUpperCase()}</h2><canvas id="chart-bar"></canvas></div>`;
    setTimeout(() => {
      const ctx = document.getElementById("chart-bar");
      if (ctx) charts.push(new Chart(ctx, {
        type: "bar",
        data: { labels, datasets: [{ label: nc.label||nc.id, data, backgroundColor: "#6c63ff", borderRadius: 6 }] },
        options: { plugins: { legend: { display: false } }, scales: {
          x: { ticks: { color: "#8892a4" }, grid: { color: "#2a2d3e" } },
          y: { ticks: { color: "#8892a4" }, grid: { color: "#2a2d3e" } }
        }}
      }));
    }, 50);
  }

  // 2. Gráfico de linha: 2ª coluna numérica (se existir)
  if (numCols.length > 1) {
    const nc = numCols[1];
    const labels = rows.slice(0,50).map((r,i) => {
      const txt = textCols[0] ? String(r.c[textCols[0].i]?.v ?? i+1) : String(i+1);
      return txt.length > 15 ? txt.slice(0,15)+"…" : txt;
    });
    const data = rows.slice(0,50).map(r => r.c[nc.i]?.v ?? 0);

    chartsGrid.innerHTML += `<div class="chart-card"><h2>EVOLUÇÃO — ${(nc.label||nc.id).toUpperCase()}</h2><canvas id="chart-line"></canvas></div>`;
    setTimeout(() => {
      const ctx = document.getElementById("chart-line");
      if (ctx) charts.push(new Chart(ctx, {
        type: "line",
        data: { labels, datasets: [{ label: nc.label||nc.id, data, borderColor: "#00d4aa", backgroundColor: "rgba(0,212,170,.1)", tension: .4, fill: true, pointRadius: 3 }] },
        options: { plugins: { legend: { display: false } }, scales: {
          x: { ticks: { color: "#8892a4", maxTicksLimit: 10 }, grid: { color: "#2a2d3e" } },
          y: { ticks: { color: "#8892a4" }, grid: { color: "#2a2d3e" } }
        }}
      }));
    }, 50);
  }

  // ── Tabela ──
  const thead = document.getElementById("table-head");
  const tbody = document.getElementById("table-body");
  thead.innerHTML = `<tr>${cols.map(c => `<th>${c.label || c.id}</th>`).join("")}</tr>`;
  tbody.innerHTML = rows.slice(0,200).map(r =>
    `<tr>${cols.map((c,i) => {
      const v = r.c?.[i]?.v;
      if (v == null) return `<td>—</td>`;
      if (typeof v === "number") return `<td>${v.toLocaleString("pt-BR")}</td>`;
      if (typeof v === "string" && v.length > 50) return `<td title="${v}">${v.slice(0,50)}…</td>`;
      return `<td>${v}</td>`;
    }).join("")}</tr>`
  ).join("");
}

// Verifica sessão ao carregar
checkSession();
</script>
</body>
</html>
