<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Dashboard Financeiro — Mara Nery</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.2/dist/chart.umd.min.js"></script>
  <style>
    /* ===== RESET & BASE ===== */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:          #0d1117;
      --surface:     #161b22;
      --surface2:    #1c2230;
      --border:      #21262d;
      --accent:      #2ea44f;
      --accent-soft: #1a3a2a;
      --danger:      #f85149;
      --danger-soft: #3a1a1a;
      --warn:        #e3b341;
      --warn-soft:   #3a2e10;
      --blue:        #58a6ff;
      --blue-soft:   #0d2040;
      --purple:      #bc8cff;
      --text-1:      #e6edf3;
      --text-2:      #8b949e;
      --text-3:      #6e7681;
      --radius:      12px;
      --radius-sm:   8px;
      --shadow:      0 4px 24px rgba(0,0,0,.45);
    }

    html { font-size: 15px; }
    body {
      background: var(--bg);
      color: var(--text-1);
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      min-height: 100vh;
      padding-bottom: 48px;
    }

    /* ===== HEADER ===== */
    header {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 20px 32px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 12px;
    }
    .header-left h1 {
      font-size: 1.35rem;
      font-weight: 700;
      letter-spacing: -.3px;
      color: var(--text-1);
    }
    .header-left p {
      font-size: .8rem;
      color: var(--text-2);
      margin-top: 2px;
    }
    .badge-ref {
      background: var(--blue-soft);
      color: var(--blue);
      border: 1px solid rgba(88,166,255,.25);
      border-radius: 20px;
      padding: 5px 14px;
      font-size: .78rem;
      font-weight: 600;
      letter-spacing: .3px;
    }

    /* ===== LAYOUT ===== */
    .container { max-width: 1400px; margin: 0 auto; padding: 28px 24px 0; }

    /* ===== SECTION TITLE ===== */
    .section-title {
      font-size: .7rem;
      font-weight: 700;
      letter-spacing: 1.2px;
      text-transform: uppercase;
      color: var(--text-3);
      margin-bottom: 14px;
      padding-left: 2px;
    }

    /* ===== CARDS GRID ===== */
    .cards-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 16px;
      margin-bottom: 32px;
    }

    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px 22px;
      position: relative;
      overflow: hidden;
      transition: transform .15s, box-shadow .15s;
    }
    .card:hover { transform: translateY(-2px); box-shadow: var(--shadow); }

    .card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
      border-radius: var(--radius) var(--radius) 0 0;
    }
    .card.green::before  { background: var(--accent); }
    .card.red::before    { background: var(--danger); }
    .card.blue::before   { background: var(--blue); }
    .card.warn::before   { background: var(--warn); }
    .card.purple::before { background: var(--purple); }
    .card.neutral::before{ background: var(--text-3); }

    .card-label {
      font-size: .72rem;
      font-weight: 600;
      letter-spacing: .6px;
      text-transform: uppercase;
      color: var(--text-2);
      margin-bottom: 10px;
    }
    .card-value {
      font-size: 1.55rem;
      font-weight: 700;
      line-height: 1.1;
      letter-spacing: -.5px;
    }
    .card-value.positive { color: var(--accent); }
    .card-value.negative { color: var(--danger); }
    .card-value.blue     { color: var(--blue); }
    .card-value.warn     { color: var(--warn); }
    .card-value.neutral  { color: var(--text-1); }

    .card-sub {
      font-size: .72rem;
      color: var(--text-3);
      margin-top: 6px;
    }

    /* ===== RESULTADO HERO ===== */
    .resultado-hero {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 28px 32px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 20px;
      margin-bottom: 32px;
      position: relative;
      overflow: hidden;
    }
    .resultado-hero::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 4px;
    }
    .resultado-hero.lucro::before  { background: linear-gradient(90deg, var(--accent), #3fb950); }
    .resultado-hero.prejuizo::before { background: linear-gradient(90deg, var(--danger), #ff6b6b); }

    .resultado-label {
      font-size: .75rem;
      font-weight: 700;
      letter-spacing: 1px;
      text-transform: uppercase;
      color: var(--text-2);
      margin-bottom: 8px;
    }
    .resultado-value {
      font-size: 2.8rem;
      font-weight: 800;
      letter-spacing: -1px;
      line-height: 1;
    }
    .resultado-value.lucro   { color: var(--accent); }
    .resultado-value.prejuizo{ color: var(--danger); }

    .resultado-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 8px 18px;
      border-radius: 30px;
      font-size: .85rem;
      font-weight: 700;
      letter-spacing: .3px;
      margin-top: 10px;
    }
    .resultado-badge.lucro   { background: var(--accent-soft); color: var(--accent); border: 1px solid rgba(46,164,79,.3); }
    .resultado-badge.prejuizo{ background: var(--danger-soft); color: var(--danger); border: 1px solid rgba(248,81,73,.3); }

    .resultado-meta {
      display: flex;
      gap: 32px;
      flex-wrap: wrap;
    }
    .resultado-meta-item { text-align: center; }
    .resultado-meta-item .val {
      font-size: 1.25rem;
      font-weight: 700;
    }
    .resultado-meta-item .lbl {
      font-size: .68rem;
      color: var(--text-3);
      text-transform: uppercase;
      letter-spacing: .5px;
      margin-top: 3px;
    }

    /* ===== CHARTS GRID ===== */
    .charts-row {
      display: grid;
      grid-template-columns: 1.6fr 1fr 1fr;
      gap: 20px;
      margin-bottom: 32px;
    }
    @media (max-width: 1100px) {
      .charts-row { grid-template-columns: 1fr 1fr; }
    }
    @media (max-width: 700px) {
      .charts-row { grid-template-columns: 1fr; }
    }

    .chart-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 22px 22px 18px;
    }
    .chart-title {
      font-size: .78rem;
      font-weight: 700;
      letter-spacing: .4px;
      color: var(--text-1);
      margin-bottom: 4px;
    }
    .chart-subtitle {
      font-size: .68rem;
      color: var(--text-3);
      margin-bottom: 18px;
    }
    .chart-wrap {
      position: relative;
      width: 100%;
    }
    .chart-wrap canvas { width: 100% !important; }

    /* ===== LEGEND CUSTOM ===== */
    .legend-list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 16px;
    }
    .legend-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: .72rem;
    }
    .legend-dot {
      width: 10px; height: 10px;
      border-radius: 50%;
      margin-right: 8px;
      flex-shrink: 0;
    }
    .legend-name {
      display: flex;
      align-items: center;
      color: var(--text-2);
      flex: 1;
    }
    .legend-val { color: var(--text-1); font-weight: 600; }

    /* ===== PROGRESS BARS ===== */
    .progress-section { margin-bottom: 32px; }
    .progress-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    @media (max-width: 700px) { .progress-grid { grid-template-columns: 1fr; } }

    .progress-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 22px;
    }
    .progress-card-title {
      font-size: .78rem;
      font-weight: 700;
      color: var(--text-1);
      margin-bottom: 16px;
    }
    .progress-item { margin-bottom: 14px; }
    .progress-item:last-child { margin-bottom: 0; }
    .progress-header {
      display: flex;
      justify-content: space-between;
      margin-bottom: 6px;
    }
    .progress-name { font-size: .72rem; color: var(--text-2); }
    .progress-amount { font-size: .72rem; font-weight: 600; color: var(--text-1); }
    .progress-pct { font-size: .65rem; color: var(--text-3); margin-left: 6px; }
    .progress-bar-bg {
      height: 6px;
      background: var(--surface2);
      border-radius: 4px;
      overflow: hidden;
    }
    .progress-bar-fill {
      height: 100%;
      border-radius: 4px;
      transition: width .6s ease;
    }

    /* ===== TABLES ===== */
    .tables-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      margin-bottom: 32px;
    }
    @media (max-width: 900px) { .tables-row { grid-template-columns: 1fr; } }

    .table-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
    }
    .table-header {
      padding: 16px 20px;
      border-bottom: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .table-header h3 { font-size: .82rem; font-weight: 700; }
    .table-count {
      font-size: .68rem;
      background: var(--surface2);
      color: var(--text-2);
      padding: 3px 10px;
      border-radius: 20px;
    }
    .table-scroll { overflow-x: auto; }
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: .72rem;
    }
    thead th {
      background: var(--surface2);
      color: var(--text-3);
      font-weight: 600;
      letter-spacing: .5px;
      text-transform: uppercase;
      padding: 10px 14px;
      text-align: left;
      white-space: nowrap;
    }
    tbody tr {
      border-bottom: 1px solid var(--border);
      transition: background .1s;
    }
    tbody tr:hover { background: var(--surface2); }
    tbody tr:last-child { border-bottom: none; }
    tbody td { padding: 10px 14px; color: var(--text-2); white-space: nowrap; }
    tbody td.val { color: var(--text-1); font-weight: 600; }

    /* ===== STATUS BADGES ===== */
    .status {
      display: inline-block;
      padding: 2px 10px;
      border-radius: 20px;
      font-size: .65rem;
      font-weight: 700;
      letter-spacing: .3px;
    }
    .status-recebido, .status-quitado {
      background: var(--accent-soft);
      color: var(--accent);
      border: 1px solid rgba(46,164,79,.25);
    }
    .status-aberto {
      background: var(--blue-soft);
      color: var(--blue);
      border: 1px solid rgba(88,166,255,.25);
    }
    .status-atrasado {
      background: var(--danger-soft);
      color: var(--danger);
      border: 1px solid rgba(248,81,73,.25);
    }

    /* ===== INSIGHT STRIP ===== */
    .insight-strip {
      background: var(--surface);
      border: 1px solid var(--border);
      border-left: 4px solid var(--warn);
      border-radius: var(--radius);
      padding: 16px 20px;
      margin-bottom: 32px;
      display: flex;
      align-items: flex-start;
      gap: 14px;
    }
    .insight-icon { font-size: 1.2rem; flex-shrink: 0; margin-top: 1px; }
    .insight-text h4 { font-size: .8rem; font-weight: 700; color: var(--warn); margin-bottom: 4px; }
    .insight-text p { font-size: .75rem; color: var(--text-2); line-height: 1.5; }

    /* ===== FOOTER ===== */
    footer {
      text-align: center;
      color: var(--text-3);
      font-size: .68rem;
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px solid var(--border);
    }

    /* ===== DIVIDER ===== */
    .divider { height: 1px; background: var(--border); margin: 8px 0 28px; }
  </style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="header-left">
    <h1>Dashboard Financeiro</h1>
    <p>Mara Nery &nbsp;·&nbsp; CNPJ: 00.000.000/0001-00</p>
  </div>
  <span class="badge-ref" id="badge-ref">Março / 2026</span>
</header>

<div class="container">

  <!-- ===== RESULTADO HERO ===== -->
  <div style="margin-bottom:10px; margin-top:4px;">
    <div class="section-title">Resultado do Mês</div>
  </div>
  <div class="resultado-hero" id="resultado-hero">
    <div>
      <div class="resultado-label">Resultado do Mês (Receita − Despesa)</div>
      <div class="resultado-value" id="resultado-value">—</div>
      <div class="resultado-badge" id="resultado-badge">—</div>
    </div>
    <div class="resultado-meta">
      <div class="resultado-meta-item">
        <div class="val" id="meta-receita" style="color:var(--accent)">—</div>
        <div class="lbl">Total Receitas</div>
      </div>
      <div class="resultado-meta-item">
        <div class="val" id="meta-despesa" style="color:var(--danger)">—</div>
        <div class="lbl">Total Despesas</div>
      </div>
      <div class="resultado-meta-item">
        <div class="val" id="meta-margem" style="color:var(--warn)">—</div>
        <div class="lbl">Margem</div>
      </div>
    </div>
  </div>

  <!-- ===== CARDS KPI ===== -->
  <div class="section-title">Indicadores Principais</div>
  <div class="cards-grid">
    <div class="card green">
      <div class="card-label">Total Receitas (a receber)</div>
      <div class="card-value positive" id="kpi-receita">—</div>
      <div class="card-sub" id="kpi-receita-sub">—</div>
    </div>
    <div class="card red">
      <div class="card-label">Total Despesas (a pagar)</div>
      <div class="card-value negative" id="kpi-despesa">—</div>
      <div class="card-sub" id="kpi-despesa-sub">—</div>
    </div>
    <div class="card blue">
      <div class="card-label">Total Recebido</div>
      <div class="card-value blue" id="kpi-recebido">—</div>
      <div class="card-sub" id="kpi-recebido-sub">—</div>
    </div>
    <div class="card warn">
      <div class="card-label">Total Pago</div>
      <div class="card-value warn" id="kpi-pago">—</div>
      <div class="card-sub" id="kpi-pago-sub">—</div>
    </div>
    <div class="card neutral">
      <div class="card-label">Saldo em Aberto</div>
      <div class="card-value" id="kpi-saldo">—</div>
      <div class="card-sub">Pendente a receber − pendente a pagar</div>
    </div>
  </div>

  <!-- ===== INSIGHT ===== -->
  <div class="insight-strip" id="insight-strip">
    <div class="insight-icon">⚡</div>
    <div class="insight-text">
      <h4 id="insight-title">Análise Automática</h4>
      <p id="insight-body">Carregando análise...</p>
    </div>
  </div>

  <!-- ===== GRÁFICOS ===== -->
  <div class="section-title">Análise Visual</div>
  <div class="charts-row">

    <!-- Gráfico 1: Barras Receita vs Despesa -->
    <div class="chart-card">
      <div class="chart-title">Receitas vs Despesas</div>
      <div class="chart-subtitle">Comparação direta do mês — visão macro</div>
      <div class="chart-wrap" style="height:280px">
        <canvas id="chartBarras"></canvas>
      </div>
    </div>

    <!-- Gráfico 2: Pizza Receitas -->
    <div class="chart-card">
      <div class="chart-title">Composição das Receitas</div>
      <div class="chart-subtitle">Qualidade do fluxo de entrada</div>
      <div class="chart-wrap" style="height:200px">
        <canvas id="chartPizzaReceita"></canvas>
      </div>
      <div class="legend-list" id="legend-receita"></div>
    </div>

    <!-- Gráfico 3: Pizza Despesas -->
    <div class="chart-card">
      <div class="chart-title">Composição das Despesas</div>
      <div class="chart-subtitle">Qualidade do fluxo de saída</div>
      <div class="chart-wrap" style="height:200px">
        <canvas id="chartPizzaDespesa"></canvas>
      </div>
      <div class="legend-list" id="legend-despesa"></div>
    </div>

  </div>

  <!-- ===== BARRAS DE PROGRESSO ===== -->
  <div class="section-title">Execução por Categoria</div>
  <div class="progress-grid" style="margin-bottom:32px">
    <div class="progress-card">
      <div class="progress-card-title">Receitas por Categoria</div>
      <div id="progress-receita"></div>
    </div>
    <div class="progress-card">
      <div class="progress-card-title">Despesas por Categoria</div>
      <div id="progress-despesa"></div>
    </div>
  </div>

  <!-- ===== TABELAS ===== -->
  <div class="section-title">Detalhamento das Transações</div>
  <div class="tables-row">
    <div class="table-card">
      <div class="table-header">
        <h3>Receitas</h3>
        <span class="table-count" id="count-receita">0 registros</span>
      </div>
      <div class="table-scroll">
        <table>
          <thead>
            <tr>
              <th>Cliente</th>
              <th>Categoria</th>
              <th>Vencimento</th>
              <th>Valor</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody id="tbody-receita"></tbody>
        </table>
      </div>
    </div>
    <div class="table-card">
      <div class="table-header">
        <h3>Despesas</h3>
        <span class="table-count" id="count-despesa">0 registros</span>
      </div>
      <div class="table-scroll">
        <table>
          <thead>
            <tr>
              <th>Fornecedor</th>
              <th>Categoria</th>
              <th>Vencimento</th>
              <th>Valor</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody id="tbody-despesa"></tbody>
        </table>
      </div>
    </div>
  </div>

</div><!-- /container -->

<footer>
  Dashboard Financeiro · Mara Nery · Gerado automaticamente a partir de ControledeContas.xlsx
</footer>

<script>
/* ===================================================
   DADOS EMBUTIDOS (gerados pelo Python)
   =================================================== */
const DADOS = {
  "indicadores": {
    "total_receitas": 57740.65,
    "total_despesas": 167793.31,
    "resultado": -110052.66,
    "total_recebido": 40196.99,
    "total_pago": 123379.13,
    "saldo_aberto": -26870.52
  },
  "receitas_status": {
    "Recebido": 40196.99,
    "Em Aberto": 10017.27,
    "Atrasado": 7526.39
  },
  "despesas_status": {
    "Quitado": 123379.13,
    "Em Aberto": 43374.91,
    "Atrasado": 1039.27
  },
  "categorias_receitas": {
    "Venda": 47849.5,
    "Locação": 5391.15
  },
  "categorias_despesas": {
    "Operacional": 108789.81,
    "Financeiro": 47154.02,
    "Administrativo": 9050.59,
    "Investimento": 2899.89
  },
  "receitas": [
    {"id":1,"cliente":"Empresa A","descricao":"-","categoria":"Venda","valor":3500,"status":"Recebido","vencimento":"2026-03-01","data_recebimento":"2026-03-01"},
    {"id":2,"cliente":"Empresa B","descricao":"-","categoria":"Venda","valor":3200.5,"status":"Recebido","vencimento":"2026-03-03","data_recebimento":"2026-03-04"},
    {"id":35,"cliente":"Empresa C","descricao":"-","categoria":"Venda","valor":2400,"status":"Atrasado","vencimento":"2026-03-05","data_recebimento":null},
    {"id":15,"cliente":"Empresa D","descricao":"Produto x","categoria":"Locação","valor":1250,"status":"Recebido","vencimento":"2026-06-07","data_recebimento":"2026-03-07"},
    {"id":153,"cliente":"Empresa E","descricao":"-","categoria":"Venda","valor":5876.12,"status":"Em Aberto","vencimento":"2026-03-25","data_recebimento":null},
    {"id":23,"cliente":"Empresa F","descricao":"-","categoria":"Venda","valor":7895.23,"status":"Recebido","vencimento":"2026-03-25","data_recebimento":"2026-03-10"},
    {"id":58,"cliente":"Empresa G","descricao":"-","categoria":"Venda","valor":12845,"status":"Recebido","vencimento":"2026-03-18","data_recebimento":"2026-03-17"},
    {"id":136,"cliente":"Empresa H","descricao":"Produto x","categoria":"Locação","valor":1245,"status":"Em Aberto","vencimento":"2026-03-21","data_recebimento":null},
    {"id":8956,"cliente":"Empresa I","descricao":"-","categoria":"Venda","valor":3259,"status":"Recebido","vencimento":"2026-03-07","data_recebimento":"2026-03-20"},
    {"id":835,"cliente":"Empresa J","descricao":"-","categoria":"Venda","valor":8247.26,"status":"Recebido","vencimento":"2026-03-19","data_recebimento":"2026-03-19"},
    {"id":5369,"cliente":"Empresa K","descricao":"-","categoria":"Venda","valor":5126.39,"status":"Atrasado","vencimento":"2026-03-15","data_recebimento":null},
    {"id":2588,"cliente":"Empresa L","descricao":"Produto Y","categoria":"Locação","valor":2896.15,"status":"Em Aberto","vencimento":"2026-03-31","data_recebimento":null}
  ],
  "despesas": [
    {"id":1,"fornecedor":"Neoenergia","descricao":"Energia","categoria":"Operacional","valor":896.25,"status":"Quitado","vencimento":"2026-03-01","data_pagamento":"2026-03-01"},
    {"id":42,"fornecedor":"Empresa B","descricao":"Internet","categoria":"Operacional","valor":350,"status":"Atrasado","vencimento":"2026-03-04","data_pagamento":null},
    {"id":2,"fornecedor":"Empresa C","descricao":"Office Supplies","categoria":"Operacional","valor":419.25,"status":"Quitado","vencimento":"2026-03-05","data_pagamento":"2026-03-05"},
    {"id":35,"fornecedor":"Empresa D","descricao":"Telefonia","categoria":"Operacional","valor":689.27,"status":"Atrasado","vencimento":"2026-03-15","data_pagamento":null},
    {"id":15,"fornecedor":"Simples Nacional","descricao":"Imposto","categoria":"Financeiro","valor":29863.89,"status":"Quitado","vencimento":"2026-03-13","data_pagamento":"2026-03-15"},
    {"id":153,"fornecedor":"Alvará","descricao":"Licença","categoria":"Administrativo","valor":8651.59,"status":"Em Aberto","vencimento":"2026-03-20","data_pagamento":null},
    {"id":23,"fornecedor":"F. de Pagamento","descricao":"Salários","categoria":"Operacional","valor":79569.15,"status":"Quitado","vencimento":"2026-03-05","data_pagamento":"2026-03-01"},
    {"id":58,"fornecedor":"Empresa H","descricao":"Geladeira","categoria":"Investimento","valor":2899.89,"status":"Em Aberto","vencimento":"2026-03-20","data_pagamento":null},
    {"id":136,"fornecedor":"DAS MEI","descricao":"Imposto","categoria":"Financeiro","valor":12630.59,"status":"Quitado","vencimento":"2026-03-15","data_pagamento":"2025-06-15"},
    {"id":8956,"fornecedor":"Empresa J","descricao":"Site","categoria":"Administrativo","valor":399,"status":"Em Aberto","vencimento":"2026-03-31","data_pagamento":null},
    {"id":835,"fornecedor":"ICMS","descricao":"Imposto","categoria":"Financeiro","valor":4659.54,"status":"Em Aberto","vencimento":"2026-03-29","data_pagamento":null},
    {"id":5369,"fornecedor":"Empresa K","descricao":"Segurança Priv","categoria":"Operacional","valor":639.2,"status":"Em Aberto","vencimento":"2026-03-31","data_pagamento":null},
    {"id":2588,"fornecedor":"Vale Transporte","descricao":"Colaboradores","categoria":"Operacional","valor":26125.69,"status":"Em Aberto","vencimento":"2026-03-29","data_pagamento":null}
  ],
  "referencia": "Março/2026"
};

/* ===================================================
   UTILITÁRIOS
   =================================================== */
const fmt = v => v.toLocaleString('pt-BR', { style:'currency', currency:'BRL' });
const fmtPct = v => v.toFixed(1) + '%';

function statusClass(s) {
  if (s === 'Recebido' || s === 'Quitado') return 'status-recebido';
  if (s === 'Atrasado') return 'status-atrasado';
  return 'status-aberto';
}
function statusLabel(s) {
  if (s === 'Recebido') return 'Recebido';
  if (s === 'Quitado')  return 'Quitado';
  if (s === 'Atrasado') return 'Atrasado';
  return 'Em Aberto';
}

/* ===================================================
   PREENCHER CARDS
   =================================================== */
function renderCards() {
  const ind = DADOS.indicadores;
  const rs  = DADOS.receitas_status;
  const ds  = DADOS.despesas_status;

  // Resultado hero
  const hero = document.getElementById('resultado-hero');
  const rv   = document.getElementById('resultado-value');
  const rb   = document.getElementById('resultado-badge');
  const isLucro = ind.resultado >= 0;
  hero.classList.add(isLucro ? 'lucro' : 'prejuizo');
  rv.textContent = fmt(Math.abs(ind.resultado));
  rv.classList.add(isLucro ? 'lucro' : 'prejuizo');
  rb.textContent = isLucro ? '▲ LUCRO' : '▼ PREJUÍZO';
  rb.classList.add(isLucro ? 'lucro' : 'prejuizo');

  document.getElementById('meta-receita').textContent = fmt(ind.total_receitas);
  document.getElementById('meta-despesa').textContent = fmt(ind.total_despesas);
  const margem = ind.total_receitas > 0 ? (ind.resultado / ind.total_receitas * 100) : 0;
  document.getElementById('meta-margem').textContent = fmtPct(margem);

  // KPIs
  document.getElementById('kpi-receita').textContent = fmt(ind.total_receitas);
  document.getElementById('kpi-receita-sub').textContent =
    `Recebido: ${fmt(rs.Recebido)} · Pendente: ${fmt(rs['Em Aberto'])} · Atrasado: ${fmt(rs.Atrasado)}`;

  document.getElementById('kpi-despesa').textContent = fmt(ind.total_despesas);
  document.getElementById('kpi-despesa-sub').textContent =
    `Pago: ${fmt(ds.Quitado)} · Em aberto: ${fmt(ds['Em Aberto'])} · Atrasado: ${fmt(ds.Atrasado)}`;

  document.getElementById('kpi-recebido').textContent = fmt(ind.total_recebido);
  const pctRec = ind.total_receitas > 0 ? (ind.total_recebido / ind.total_receitas * 100) : 0;
  document.getElementById('kpi-recebido-sub').textContent =
    `${fmtPct(pctRec)} das receitas já recebidas`;

  document.getElementById('kpi-pago').textContent = fmt(ind.total_pago);
  const pctPago = ind.total_despesas > 0 ? (ind.total_pago / ind.total_despesas * 100) : 0;
  document.getElementById('kpi-pago-sub').textContent =
    `${fmtPct(pctPago)} das despesas já pagas`;

  const saldo = document.getElementById('kpi-saldo');
  saldo.textContent = fmt(Math.abs(ind.saldo_aberto));
  saldo.classList.add(ind.saldo_aberto >= 0 ? 'positive' : 'negative');

  // Insight
  gerarInsight(ind, rs, ds, margem);
}

function gerarInsight(ind, rs, ds, margem) {
  const title = document.getElementById('insight-title');
  const body  = document.getElementById('insight-body');
  const strip = document.getElementById('insight-strip');

  const totalAtrasadoRec  = rs.Atrasado;
  const totalAtrasadoDesp = ds.Atrasado;
  const pctAtrasadoRec = ind.total_receitas > 0 ? (totalAtrasadoRec / ind.total_receitas * 100) : 0;

  if (ind.resultado < 0) {
    strip.style.borderLeftColor = 'var(--danger)';
    title.style.color = 'var(--danger)';
    title.textContent = 'Atenção: Resultado Negativo no Mês';
    body.textContent =
      `As despesas (${fmt(ind.total_despesas)}) superam as receitas (${fmt(ind.total_receitas)}) em ${fmt(Math.abs(ind.resultado))}. ` +
      `Apenas ${fmtPct(margem * -1)} de margem negativa. ` +
      (pctAtrasadoRec > 0 ? `Há ${fmt(totalAtrasadoRec)} em receitas atrasadas que, se recuperadas, melhorariam o resultado. ` : '') +
      `Revise as despesas operacionais e financeiras para reequilibrar o fluxo.`;
  } else {
    title.textContent = 'Resultado Positivo — Atenção ao Fluxo Pendente';
    body.textContent =
      `Lucro de ${fmt(ind.resultado)} no mês (margem ${fmtPct(margem)}). ` +
      `Ainda há ${fmt(ds['Em Aberto'] + ds.Atrasado)} em despesas pendentes e ` +
      `${fmt(rs['Em Aberto'] + rs.Atrasado)} em receitas a confirmar. Monitore os vencimentos.`;
  }
}

/* ===================================================
   GRÁFICO 1 — BARRAS
   =================================================== */
function renderBarras() {
  const ind = DADOS.indicadores;
  const ctx = document.getElementById('chartBarras').getContext('2d');

  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: ['Receitas', 'Despesas'],
      datasets: [
        {
          label: 'Total do Mês',
          data: [ind.total_receitas, ind.total_despesas],
          backgroundColor: ['rgba(46,164,79,.85)', 'rgba(248,81,73,.85)'],
          borderColor:      ['#2ea44f', '#f85149'],
          borderWidth: 1.5,
          borderRadius: 8,
          borderSkipped: false
        },
        {
          label: 'Realizado',
          data: [ind.total_recebido, ind.total_pago],
          backgroundColor: ['rgba(46,164,79,.35)', 'rgba(248,81,73,.35)'],
          borderColor:      ['#2ea44f', '#f85149'],
          borderWidth: 1,
          borderRadius: 8,
          borderSkipped: false
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          labels: { color: '#8b949e', font: { size: 11 }, boxWidth: 12, padding: 16 }
        },
        tooltip: {
          callbacks: {
            label: ctx => ' ' + fmt(ctx.parsed.y)
          }
        }
      },
      scales: {
        x: {
          ticks: { color: '#8b949e', font: { size: 12, weight: '600' } },
          grid:  { color: 'rgba(255,255,255,.04)' }
        },
        y: {
          ticks: {
            color: '#8b949e',
            font: { size: 10 },
            callback: v => 'R$ ' + (v/1000).toFixed(0) + 'k'
          },
          grid: { color: 'rgba(255,255,255,.06)' }
        }
      }
    }
  });
}

/* ===================================================
   GRÁFICO 2 — PIZZA RECEITAS
   =================================================== */
function renderPizzaReceita() {
  const rs  = DADOS.receitas_status;
  const ctx = document.getElementById('chartPizzaReceita').getContext('2d');
  const labels = ['Recebido', 'Em Aberto', 'Atrasado'];
  const values = [rs.Recebido, rs['Em Aberto'], rs.Atrasado];
  const colors = ['#2ea44f', '#58a6ff', '#f85149'];

  new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels,
      datasets: [{ data: values, backgroundColor: colors, borderColor: '#161b22', borderWidth: 3, hoverOffset: 6 }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      cutout: '65%',
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: c => ' ' + fmt(c.parsed) } }
      }
    }
  });

  const total = values.reduce((a,b) => a+b, 0);
  const leg = document.getElementById('legend-receita');
  labels.forEach((l,i) => {
    const pct = total > 0 ? (values[i]/total*100).toFixed(1) : '0.0';
    leg.innerHTML += `
      <div class="legend-item">
        <span class="legend-name"><span class="legend-dot" style="background:${colors[i]}"></span>${l}</span>
        <span class="legend-val">${fmt(values[i])} <span style="color:var(--text-3);font-weight:400">(${pct}%)</span></span>
      </div>`;
  });
}

/* ===================================================
   GRÁFICO 3 — PIZZA DESPESAS
   =================================================== */
function renderPizzaDespesa() {
  const ds  = DADOS.despesas_status;
  const ctx = document.getElementById('chartPizzaDespesa').getContext('2d');
  const labels = ['Quitado', 'Em Aberto', 'Atrasado'];
  const values = [ds.Quitado, ds['Em Aberto'], ds.Atrasado];
  const colors = ['#2ea44f', '#58a6ff', '#f85149'];

  new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels,
      datasets: [{ data: values, backgroundColor: colors, borderColor: '#161b22', borderWidth: 3, hoverOffset: 6 }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      cutout: '65%',
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: c => ' ' + fmt(c.parsed) } }
      }
    }
  });

  const total = values.reduce((a,b) => a+b, 0);
  const leg = document.getElementById('legend-despesa');
  labels.forEach((l,i) => {
    const pct = total > 0 ? (values[i]/total*100).toFixed(1) : '0.0';
    leg.innerHTML += `
      <div class="legend-item">
        <span class="legend-name"><span class="legend-dot" style="background:${colors[i]}"></span>${l}</span>
        <span class="legend-val">${fmt(values[i])} <span style="color:var(--text-3);font-weight:400">(${pct}%)</span></span>
      </div>`;
  });
}

/* ===================================================
   BARRAS DE PROGRESSO — CATEGORIAS
   =================================================== */
function renderProgress() {
  const catRec  = DADOS.categorias_receitas;
  const catDesp = DADOS.categorias_despesas;

  const colorsRec  = ['#2ea44f','#3fb950','#56d364','#7ee787'];
  const colorsDesp = ['#f85149','#ff7b72','#ffa198','#e3b341'];

  function buildProgress(containerId, catObj, colors) {
    const total = Object.values(catObj).reduce((a,b) => a+b, 0);
    const sorted = Object.entries(catObj).sort((a,b) => b[1]-a[1]);
    const el = document.getElementById(containerId);
    sorted.forEach(([name, val], i) => {
      const pct = total > 0 ? (val/total*100) : 0;
      el.innerHTML += `
        <div class="progress-item">
          <div class="progress-header">
            <span class="progress-name">${name}</span>
            <span class="progress-amount">${fmt(val)}<span class="progress-pct">${fmtPct(pct)}</span></span>
          </div>
          <div class="progress-bar-bg">
            <div class="progress-bar-fill" style="width:${pct}%;background:${colors[i % colors.length]}"></div>
          </div>
        </div>`;
    });
  }

  buildProgress('progress-receita', catRec,  colorsRec);
  buildProgress('progress-despesa', catDesp, colorsDesp);
}

/* ===================================================
   TABELAS
   =================================================== */
function renderTabelas() {
  // Receitas
  const tbR = document.getElementById('tbody-receita');
  document.getElementById('count-receita').textContent = DADOS.receitas.length + ' registros';
  DADOS.receitas.forEach(r => {
    const venc = r.vencimento ? r.vencimento.slice(5).split('-').reverse().join('/') + '/' + r.vencimento.slice(0,4) : '—';
    tbR.innerHTML += `
      <tr>
        <td>${r.cliente}</td>
        <td>${r.categoria || '—'}</td>
        <td>${venc}</td>
        <td class="val">${fmt(r.valor)}</td>
        <td><span class="status ${statusClass(r.status)}">${statusLabel(r.status)}</span></td>
      </tr>`;
  });

  // Despesas
  const tbD = document.getElementById('tbody-despesa');
  document.getElementById('count-despesa').textContent = DADOS.despesas.length + ' registros';
  DADOS.despesas.forEach(d => {
    const venc = d.vencimento ? d.vencimento.slice(5).split('-').reverse().join('/') + '/' + d.vencimento.slice(0,4) : '—';
    tbD.innerHTML += `
      <tr>
        <td>${d.fornecedor}</td>
        <td>${d.categoria || '—'}</td>
        <td>${venc}</td>
        <td class="val">${fmt(d.valor)}</td>
        <td><span class="status ${statusClass(d.status)}">${statusLabel(d.status)}</span></td>
      </tr>`;
  });
}

/* ===================================================
   INIT
   =================================================== */
document.addEventListener('DOMContentLoaded', () => {
  renderCards();
  renderBarras();
  renderPizzaReceita();
  renderPizzaDespesa();
  renderProgress();
  renderTabelas();
});
</script>
</body>
</html>
