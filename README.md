<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Property Investment Analyzer</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Lora:wght@600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #111418;
    --surface: #181c22;
    --surface2: #1f242d;
    --surface3: #252b36;
    --border: #2d3340;
    --border-light: #353d4d;
    --accent: #4ade80;
    --accent-dim: rgba(74,222,128,0.12);
    --accent2: #60a5fa;
    --accent2-dim: rgba(96,165,250,0.12);
    --warn: #fbbf24;
    --warn-dim: rgba(251,191,36,0.12);
    --danger: #f87171;
    --danger-dim: rgba(248,113,113,0.12);
    --text: #e2e8f0;
    --text-secondary: #94a3b8;
    --text-muted: #4a5568;
    --radius: 10px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: 'Inter', sans-serif; min-height: 100vh; font-size: 14px; line-height: 1.5; }
  .container { max-width: 1240px; margin: 0 auto; padding: 0 28px; }

  header { padding: 28px 0 24px; border-bottom: 1px solid var(--border); margin-bottom: 24px; }
  .header-inner { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
  .logo { font-family: 'Lora', serif; font-size: 22px; font-weight: 700; color: var(--text); }
  .logo span { color: var(--accent); }
  .logo-sub { font-size: 11px; color: var(--text-muted); letter-spacing: 1.5px; text-transform: uppercase; margin-top: 3px; }
  .header-meta { text-align: right; }
  .header-meta .addr { font-size: 14px; font-weight: 500; }
  .header-meta .sub { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }

  /* Save bar */
  .save-bar { display: flex; align-items: center; gap: 10px; background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 11px 16px; margin-bottom: 16px; flex-wrap: wrap; }
  .save-bar-label { font-size: 11px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--text-muted); margin-right: 2px; flex-shrink:0; }
  .saved-deals-list { display: flex; gap: 8px; flex-wrap: wrap; flex: 1; }
  .saved-deal-btn { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text-secondary); font-family: 'Inter', sans-serif; font-size: 12px; padding: 5px 10px; cursor: pointer; transition: all 0.15s; display: inline-flex; align-items: center; gap: 5px; }
  .saved-deal-btn:hover { border-color: var(--accent2); color: var(--text); }
  .saved-deal-btn.active { border-color: var(--accent); color: var(--accent); background: var(--accent-dim); }
  .del-x { color: var(--text-muted); font-size: 15px; line-height: 1; padding: 0 1px; }
  .del-x:hover { color: var(--danger); }
  .save-spacer { flex: 1; min-width: 8px; }
  .btn { display: inline-flex; align-items: center; gap: 6px; padding: 7px 14px; border-radius: 6px; font-family: 'Inter', sans-serif; font-size: 12px; font-weight: 500; cursor: pointer; border: 1px solid transparent; transition: all 0.15s; white-space: nowrap; }
  .btn-primary { background: var(--accent); color: #0d1a10; border-color: var(--accent); }
  .btn-primary:hover { background: #86efac; }
  .btn-ghost { background: transparent; color: var(--text-secondary); border-color: var(--border); }
  .btn-ghost:hover { border-color: var(--border-light); color: var(--text); background: var(--surface2); }

  /* Prop bar */
  .prop-bar { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 20px; margin-bottom: 16px; display: flex; gap: 18px; flex-wrap: wrap; align-items: center; }
  .prop-field { display: flex; align-items: center; gap: 8px; }
  .prop-field label { font-size: 12px; color: var(--text-secondary); white-space: nowrap; }
  .prop-input { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: 'Inter', sans-serif; font-size: 13px; padding: 6px 10px; transition: border-color 0.15s; }
  .prop-input:focus { outline: none; border-color: var(--accent2); }

  /* Tabs */
  .tabs { display: flex; gap: 3px; background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 4px; margin-bottom: 22px; }
  .tab { flex: 1; padding: 9px 14px; border: none; background: transparent; color: var(--text-secondary); font-family: 'Inter', sans-serif; font-size: 13px; font-weight: 500; cursor: pointer; border-radius: 7px; transition: all 0.15s; border-bottom: 2px solid transparent; }
  .tab:not(.active):hover { color: var(--text); background: var(--surface2); }
  .tab.active { background: var(--surface3); color: var(--text); }
  .tab.active.t-flip { border-bottom-color: var(--accent); }
  .tab.active.t-rent { border-bottom-color: var(--accent2); }
  .tab.active.t-subto { border-bottom-color: var(--warn); }
  .tab.active.t-rehab { border-bottom-color: var(--danger); }

  .panel { display: none; }
  .panel.active { display: block; }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
  @media (max-width: 820px) { .grid-2 { grid-template-columns: 1fr; } }

  .card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; }
  .card-title { font-size: 11px; font-weight: 600; letter-spacing: 1.2px; text-transform: uppercase; color: var(--text-secondary); margin-bottom: 14px; padding-bottom: 10px; border-bottom: 1px solid var(--border); }
  .mt-18 { margin-top: 18px; }

  .field-group { display: flex; flex-direction: column; gap: 9px; }
  .field { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
  .field label { color: var(--text-secondary); font-size: 13px; flex: 1; }

  .ci-wrap { position: relative; width: 140px; flex-shrink: 0; }
  .ci-prefix { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: var(--text-muted); font-size: 13px; pointer-events: none; z-index: 1; }
  .ci-suffix { position: absolute; right: 9px; top: 50%; transform: translateY(-50%); color: var(--text-muted); font-size: 11px; pointer-events: none; }
  .ci-display { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: 'Inter', sans-serif; font-size: 13px; padding: 7px 10px 7px 22px; width: 100%; text-align: right; transition: border-color 0.15s, background 0.15s; }
  .ci-display:focus { outline: none; border-color: var(--accent2); background: var(--surface3); }
  .ci-display.pct { padding-left: 10px; padding-right: 24px; color: var(--warn); }
  .ci-display.plain { padding-left: 10px; }
  .field select { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: 'Inter', sans-serif; font-size: 13px; padding: 7px 10px; cursor: pointer; width: 140px; }
  .field select:focus { outline: none; border-color: var(--accent2); }

  .out-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 13px; gap: 8px; }
  .out-row:last-child { border-bottom: none; }
  .out-row .lbl { color: var(--text-secondary); }
  .out-row .val { font-weight: 500; font-variant-numeric: tabular-nums; }
  .out-row.total { margin-top: 4px; padding-top: 11px; border-top: 1px solid var(--border-light); border-bottom: none; }
  .out-row.total .lbl { color: var(--text); font-weight: 600; }
  .out-row.total .val { font-size: 16px; }

  .positive { color: var(--accent) !important; }
  .negative { color: var(--danger) !important; }
  .neutral  { color: var(--warn)   !important; }

  .summary-bar { display: grid; grid-template-columns: repeat(4,1fr); gap: 12px; margin-bottom: 20px; }
  @media (max-width:700px) { .summary-bar { grid-template-columns: repeat(2,1fr); } }
  .summary-pill { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 16px; }
  .sp-val { font-family: 'Lora', serif; font-size: 20px; font-weight: 700; line-height: 1.1; color: var(--text); font-variant-numeric: tabular-nums; }
  .sp-lbl { font-size: 11px; color: var(--text-secondary); margin-top: 4px; font-weight: 500; }

  .deal-badge { display: inline-flex; align-items: center; gap: 6px; padding: 7px 16px; border-radius: 999px; font-size: 12px; font-weight: 600; margin-top: 14px; }
  .deal-badge.yes   { background: var(--accent-dim);  color: var(--accent);  border: 1px solid var(--accent); }
  .deal-badge.no    { background: var(--danger-dim);  color: var(--danger);  border: 1px solid var(--danger); }
  .deal-badge.maybe { background: var(--warn-dim);    color: var(--warn);    border: 1px solid var(--warn); }

  .metrics-grid { display: grid; grid-template-columns: repeat(2,1fr); gap: 12px; margin-top: 16px; }
  .metric-box { background: var(--surface2); border: 1px solid var(--border); border-radius: 8px; padding: 14px; }
  .metric-box .m-val { font-family: 'Lora', serif; font-size: 22px; font-weight: 700; line-height: 1.1; font-variant-numeric: tabular-nums; }
  .metric-box .m-lbl { font-size: 11px; color: var(--text-secondary); margin-top: 4px; font-weight: 500; }

  .scenario-table { width: 100%; border-collapse: collapse; font-size: 13px; margin-top: 6px; }
  .scenario-table th { color: var(--text-secondary); font-size: 11px; font-weight: 600; letter-spacing: 0.8px; text-transform: uppercase; text-align: right; padding: 6px 8px; border-bottom: 1px solid var(--border); }
  .scenario-table th:first-child { text-align: left; }
  .scenario-table td { padding: 7px 8px; text-align: right; border-bottom: 1px solid var(--border); font-variant-numeric: tabular-nums; }
  .scenario-table td:first-child { text-align: left; color: var(--text-secondary); }
  .scenario-table tr.hl td { background: var(--accent-dim); }
  .scenario-table tr.hl td:first-child { color: var(--accent); font-weight: 600; }
  .pp { color: var(--accent); font-weight: 600; } .pn { color: var(--danger); } .pm { color: var(--warn); }

  .rehab-item { display: grid; grid-template-columns: 1fr 90px 70px 80px; gap: 8px; align-items: center; padding: 7px 0; border-bottom: 1px solid var(--border); font-size: 13px; }
  .rehab-item.hdr { color: var(--text-muted); font-size: 11px; font-weight: 600; letter-spacing: 0.8px; text-transform: uppercase; padding-bottom: 6px; }
  .rehab-item input { background: var(--surface2); border: 1px solid var(--border); border-radius: 5px; color: var(--text); font-family: 'Inter', sans-serif; font-size: 13px; padding: 5px 8px; width: 100%; text-align: center; transition: border-color 0.15s; }
  .rehab-item input:focus { outline: none; border-color: var(--accent2); }
  .rehab-est { color: var(--accent); font-weight: 500; text-align: right; font-variant-numeric: tabular-nums; }
  .sec-div { font-size: 11px; font-weight: 600; letter-spacing: 1.2px; text-transform: uppercase; color: var(--text-secondary); padding: 14px 0 6px; border-top: 1px solid var(--border); margin-top: 6px; }

  /* Modal */
  .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.7); backdrop-filter: blur(4px); z-index: 100; display: flex; align-items: center; justify-content: center; opacity: 0; pointer-events: none; transition: opacity 0.2s; }
  .modal-overlay.open { opacity: 1; pointer-events: all; }
  .modal { background: var(--surface); border: 1px solid var(--border-light); border-radius: 14px; padding: 28px; width: 420px; max-width: 95vw; transform: translateY(12px); transition: transform 0.2s; }
  .modal-overlay.open .modal { transform: translateY(0); }
  .modal h2 { font-family: 'Lora', serif; font-size: 18px; font-weight: 700; margin-bottom: 6px; }
  .modal p { font-size: 13px; color: var(--text-secondary); margin-bottom: 18px; }
  .modal input[type="text"] { width: 100%; background: var(--surface2); border: 1px solid var(--border-light); border-radius: 7px; color: var(--text); font-family: 'Inter', sans-serif; font-size: 14px; padding: 10px 14px; margin-bottom: 16px; transition: border-color 0.15s; }
  .modal input[type="text"]:focus { outline: none; border-color: var(--accent2); }
  .modal-actions { display: flex; gap: 10px; justify-content: flex-end; }

  hr { border: none; border-top: 1px solid var(--border); margin: 14px 0; }
  ::-webkit-scrollbar { width: 5px; } ::-webkit-scrollbar-track { background: var(--bg); } ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
  input[type=number]::-webkit-inner-spin-button, input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; }
  input[type=number] { -moz-appearance: textfield; }

  /* ── Tooltips ── */
  .tip-wrap { display: inline-flex; align-items: center; gap: 5px; flex: 1; }
  .tip-btn {
    display: inline-flex; align-items: center; justify-content: center;
    width: 16px; height: 16px; border-radius: 50%;
    background: var(--surface3); border: 1px solid var(--border-light);
    color: var(--text-muted); font-size: 10px; font-weight: 700;
    cursor: pointer; flex-shrink: 0; line-height: 1;
    transition: background 0.15s, color 0.15s, border-color 0.15s;
    font-style: normal; font-family: 'Inter', sans-serif;
  }
  .tip-btn:hover { background: var(--accent2-dim); color: var(--accent2); border-color: var(--accent2); }

  .tip-popover {
    position: fixed; z-index: 200;
    background: var(--surface3); border: 1px solid var(--border-light);
    border-radius: 9px; padding: 12px 14px;
    max-width: 260px; box-shadow: 0 8px 24px rgba(0,0,0,0.5);
    pointer-events: none; opacity: 0;
    transition: opacity 0.15s;
  }
  .tip-popover.visible { opacity: 1; pointer-events: none; }
  .tip-popover .tip-title { font-size: 12px; font-weight: 600; color: var(--text); margin-bottom: 5px; }
  .tip-popover .tip-body  { font-size: 12px; color: var(--text-secondary); line-height: 1.5; }
  .tip-popover .tip-good  { font-size: 11px; color: var(--accent); margin-top: 6px; font-weight: 500; }

  /* ── Settings modal extras ── */
  .settings-section { margin-bottom: 20px; }
  .settings-section h3 { font-size: 11px; font-weight: 600; letter-spacing: 1.2px; text-transform: uppercase; color: var(--text-secondary); margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid var(--border); }
  .settings-row { display: flex; flex-direction: column; gap: 5px; margin-bottom: 12px; }
  .settings-row label { font-size: 12px; color: var(--text-secondary); }
  .settings-row input[type="text"], .settings-row input[type="password"] {
    width: 100%; background: var(--surface2); border: 1px solid var(--border-light);
    border-radius: 7px; color: var(--text); font-family: 'Inter', sans-serif;
    font-size: 13px; padding: 9px 12px; transition: border-color 0.15s;
  }
  .settings-row input:focus { outline: none; border-color: var(--accent2); }
  .settings-hint { font-size: 11px; color: var(--text-muted); line-height: 1.5; margin-top: 3px; }
  .settings-hint a { color: var(--accent2); text-decoration: none; }
  .settings-hint a:hover { text-decoration: underline; }
  .api-status { display: inline-flex; align-items: center; gap: 5px; font-size: 11px; margin-top: 6px; }
  .api-status.connected { color: var(--accent); }
  .api-status.disconnected { color: var(--text-muted); }

  /* lookup btn in prop bar */
  .lookup-btn {
    display: inline-flex; align-items: center; gap: 5px;
    padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: 500;
    cursor: pointer; border: 1px solid var(--border); background: var(--surface2);
    color: var(--text-secondary); transition: all 0.15s; white-space: nowrap;
    font-family: 'Inter', sans-serif;
  }
  .lookup-btn:hover { border-color: var(--accent2); color: var(--accent2); background: var(--accent2-dim); }
  .lookup-btn.loading { opacity: 0.6; pointer-events: none; }
  .lookup-btn.has-key { border-color: var(--accent); color: var(--accent); background: var(--accent-dim); }
</style>
</head>
<body>

<!-- Modal -->
<div class="modal-overlay" id="save-modal">
  <div class="modal">
    <h2>Save Deal</h2>
    <p>Give this analysis a name to load it later.</p>
    <input type="text" id="save-name-input" placeholder="e.g. 123 Oak St — Flip" maxlength="60">
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeModal()">Cancel</button>
      <button class="btn btn-primary" onclick="confirmSave()">Save Deal</button>
    </div>
  </div>
</div>

<!-- Tooltip popover (singleton) -->
<div class="tip-popover" id="tip-popover">
  <div class="tip-title" id="tip-title"></div>
  <div class="tip-body"  id="tip-body"></div>
  <div class="tip-good"  id="tip-good"></div>
</div>

<!-- Settings modal -->
<div class="modal-overlay" id="settings-modal">
  <div class="modal" style="width:500px">
    <h2>⚙️ Settings</h2>
    <p>Configure integrations and app preferences.</p>

    <div class="settings-section">
      <h3>Rentcast API — Property Lookup</h3>
      <div class="settings-row">
        <label>API Key</label>
        <input type="password" id="rc-api-key" placeholder="Paste your Rentcast API key here">
        <div class="settings-hint">
          Get a free key (50 lookups/mo) at <a href="https://rentcast.io" target="_blank">rentcast.io</a>.
          Once entered, the <strong>Lookup Property</strong> button in the address bar will auto-fill
          SQFT, beds/baths, year built, estimated ARV, and market rent.
        </div>
        <div class="api-status disconnected" id="rc-status">● No key saved</div>
      </div>
    </div>

    <div class="settings-section">
      <h3>Deal Defaults</h3>
      <div class="settings-row">
        <label>Default HML Interest Rate (%)</label>
        <input type="text" id="def-hml-rate" placeholder="10">
      </div>
      <div class="settings-row">
        <label>Default Project Months</label>
        <input type="text" id="def-months" placeholder="6">
      </div>
      <div class="settings-row">
        <label>Default Refi LTV (%)</label>
        <input type="text" id="def-ltv" placeholder="75">
      </div>
      <div class="settings-row">
        <label>Default Min Profit % of ARV</label>
        <input type="text" id="def-min-pct" placeholder="12">
      </div>
    </div>

    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeSettings()">Cancel</button>
      <button class="btn btn-primary" onclick="saveSettings()">Save Settings</button>
    </div>
  </div>
</div>

<div class="container">
  <header>
    <div class="header-inner">
      <div>
        <div class="logo">Property<span>Analyzer</span></div>
        <div class="logo-sub">Investment Underwriting Tool</div>
      </div>
      <div class="header-meta">
        <div class="addr" id="hdr-addr">No Address Entered</div>
        <div class="sub" id="hdr-sub">—</div>
      </div>
    </div>
  </header>

  <!-- Save/Load bar -->
  <div class="save-bar">
    <span class="save-bar-label">Saved</span>
    <div class="saved-deals-list" id="saved-deals-list"></div>
    <div class="save-spacer"></div>
    <button class="btn btn-ghost" onclick="newDeal()">⊘ New Deal</button>
    <button class="btn btn-ghost" onclick="openSettings()" title="Settings">⚙️ Settings</button>
    <button class="btn btn-primary" onclick="openSaveModal()">＋ Save Deal</button>
  </div>

  <!-- Property info -->
  <div class="prop-bar">
    <div class="prop-field"><label>Address</label><input class="prop-input" id="prop-address" type="text" placeholder="123 Main St" style="width:190px" oninput="updateHeader()"></div>
    <div class="prop-field"><label>Bed/Bath/Gar</label><input class="prop-input" id="prop-bbb" type="text" placeholder="3/2/1" style="width:78px"></div>
    <div class="prop-field"><label>SQFT</label><input class="prop-input" id="prop-sqft" type="text" placeholder="1,000" style="width:88px" value="1,000" oninput="syncAll()"></div>
    <div class="prop-field"><label>ARV ($)</label><input class="prop-input" id="prop-arv" type="text" placeholder="150,000" style="width:108px" value="150,000" oninput="syncAll()"></div>
    <div class="prop-field"><label>Year Built</label><input class="prop-input" id="prop-year" type="text" placeholder="1985" style="width:74px"></div>
    <div class="prop-field"><label>HOA/mo</label><input class="prop-input" id="prop-hoa" type="text" placeholder="0" style="width:78px" value="0" oninput="syncAll()"></div>
    <button class="lookup-btn" id="lookup-btn" onclick="lookupProperty()" title="Auto-fill property details from Rentcast">🔍 Lookup Property</button>
  </div>

  <!-- Tabs -->
  <div class="tabs">
    <button class="tab active t-flip"  onclick="showTab('flip',this)">🔨 Flip</button>
    <button class="tab t-rent"         onclick="showTab('rental',this)">🏠 Rental / BRRRR</button>
    <button class="tab t-subto"        onclick="showTab('subto',this)">📋 Sub-To</button>
    <button class="tab t-rehab"        onclick="showTab('rehab',this)">🔧 Rehab Builder</button>
  </div>

  <!-- ====== FLIP ====== -->
  <div id="panel-flip" class="panel active">
    <div class="summary-bar">
      <div class="summary-pill"><div class="sp-val positive" id="f-s-profit">$0</div><div class="sp-lbl">Est. Profit</div></div>
      <div class="summary-pill"><div class="sp-val" id="f-s-roi">0%</div><div class="sp-lbl">ROI</div></div>
      <div class="summary-pill"><div class="sp-val" id="f-s-tpc">$0</div><div class="sp-lbl">Total Project Cost</div></div>
      <div class="summary-pill"><div class="sp-val" id="f-s-deal">—</div><div class="sp-lbl">Deal Status</div></div>
    </div>
    <div class="grid-2">
      <div>
        <div class="card">
          <div class="card-title">Acquisition & Costs</div>
          <div class="field-group">
            <div class="field"><label>Purchase Price</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-purchase" value="100,000" oninput="calcFlip()"></div></div>
            <div class="field"><label>Rehab Budget</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-rehab" value="3,650" oninput="calcFlip()"></div></div>
            <div class="field"><label>Closing Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-cc" value="3,000" oninput="calcFlip()"></div></div>
            <div class="field"><label>Sellers Closing Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-scc" value="0" oninput="calcFlip()"></div></div>
            <div class="field"><label>Holding Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-hold" value="3,000" oninput="calcFlip()"></div></div>
            <div class="field"><label>Project Months</label><div class="ci-wrap"><input class="ci-display plain" id="f-months" value="6" oninput="calcFlip()"></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Hard Money Loan (HML)</div>
          <div class="field-group">
            <div class="field"><label>HML LTV (fraction of TPC)</label><div class="ci-wrap"><input class="ci-display pct" id="f-hml-ltv" value="1" oninput="calcFlip()"><span class="ci-suffix">×</span></div></div>
            <div class="field"><label>HML Points</label><div class="ci-wrap"><input class="ci-display pct" id="f-hml-pts" value="0" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>HML Interest Rate (%/yr)</label><div class="ci-wrap"><input class="ci-display pct" id="f-hml-rate" value="10" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Interest Type</label><select id="f-int-type" onchange="calcFlip()"><option value="annual">Annual</option><option value="monthly">Monthly</option></select></div>
            <div class="field"><label>GAP Points</label><div class="ci-wrap"><input class="ci-display pct" id="f-gap-pts" value="0" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>GAP Interest Rate (%/yr)</label><div class="ci-wrap"><input class="ci-display pct" id="f-gap-rate" value="0" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Resale Costs</div>
          <div class="field-group">
            <div class="field"><label>RE Commission</label><div class="ci-wrap"><input class="ci-display pct" id="f-recomm" value="3" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Resale Closing Costs</label><div class="ci-wrap"><input class="ci-display pct" id="f-rcc" value="1" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Deal Thresholds</div>
          <div class="field-group">
            <div class="field"><label>Min Profit % of ARV</label><div class="ci-wrap"><input class="ci-display pct" id="f-min-pct" value="12" oninput="calcFlip()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Min Profit $</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="f-min-dol" value="25,000" oninput="calcFlip()"></div></div>
          </div>
        </div>
      </div>
      <div>
        <div class="card">
          <div class="card-title">Cost Breakdown</div>
          <div class="out-row"><span class="lbl">Purchase Price</span><span class="val" id="fo-purchase">—</span></div>
          <div class="out-row"><span class="lbl">Rehab Budget</span><span class="val" id="fo-rehab">—</span></div>
          <div class="out-row"><span class="lbl">Closing Costs</span><span class="val" id="fo-cc">—</span></div>
          <div class="out-row"><span class="lbl">Holding Costs</span><span class="val" id="fo-hold">—</span></div>
          <div class="out-row"><span class="lbl">HML Loan Amount</span><span class="val" id="fo-hml-loan">—</span></div>
          <div class="out-row"><span class="lbl">HML Points Cost</span><span class="val" id="fo-hml-pts">—</span></div>
          <div class="out-row"><span class="lbl">HML Interest</span><span class="val" id="fo-hml-int">—</span></div>
          <div class="out-row"><span class="lbl">GAP Loan Amount</span><span class="val" id="fo-gap-loan">—</span></div>
          <div class="out-row"><span class="lbl">GAP Interest</span><span class="val" id="fo-gap-int">—</span></div>
          <div class="out-row total"><span class="lbl">Total Project Cost</span><span class="val" id="fo-tpc">—</span></div>
          <div class="out-row" style="border:none;padding-top:6px"><span class="lbl">TPC as % of ARV</span><span class="val" id="fo-tpc-pct">—</span></div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Profit Analysis</div>
          <div class="out-row"><span class="lbl">ARV</span><span class="val" id="fo-arv">—</span></div>
          <div class="out-row"><span class="lbl">RE Commission</span><span class="val" id="fo-recomm">—</span></div>
          <div class="out-row"><span class="lbl">Resale Closing Costs</span><span class="val" id="fo-rcc">—</span></div>
          <div class="out-row"><span class="lbl">Net Resale Proceeds</span><span class="val" id="fo-net">—</span></div>
          <div class="out-row total"><span class="lbl">Estimated Profit</span><span class="val" id="fo-profit">—</span></div>
          <div class="out-row" style="border:none;padding-top:6px"><span class="lbl">Profit % of ARV</span><span class="val" id="fo-profit-pct">—</span></div>
          <div class="out-row" style="border:none;padding-top:0"><span class="lbl">ROI on Cash Invested</span><span class="val" id="fo-roi">—</span></div>
          <div id="flip-deal-badge"></div>
        </div>
        <div class="card mt-18">
          <div class="card-title">ARV Scenario Table</div>
          <table class="scenario-table"><thead><tr><th>Sale Price</th><th>Net (−CC)</th><th>Profit</th></tr></thead><tbody id="flip-scenario-body"></tbody></table>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== RENTAL ====== -->
  <div id="panel-rental" class="panel">
    <div class="summary-bar">
      <div class="summary-pill"><div class="sp-val positive" id="r-s-cf">$0/mo</div><div class="sp-lbl">Monthly Cash Flow</div></div>
      <div class="summary-pill"><div class="sp-val" id="r-s-coc">0%</div><div class="sp-lbl">Cash-on-Cash</div></div>
      <div class="summary-pill"><div class="sp-val" id="r-s-cap">0%</div><div class="sp-lbl">Cap Rate</div></div>
      <div class="summary-pill"><div class="sp-val" id="r-s-dscr">0.00</div><div class="sp-lbl">DSCR</div></div>
    </div>
    <div class="grid-2">
      <div>
        <div class="card">
          <div class="card-title">All-In Cost Basis</div>
          <div class="field-group">
            <div class="field"><label>Purchase Price</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-purchase" value="100,000" oninput="calcRental()"></div></div>
            <div class="field"><label>Rehab Budget</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-rehab" value="3,650" oninput="calcRental()"></div></div>
            <div class="field"><label>Closing Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-cc" value="3,000" oninput="calcRental()"></div></div>
            <div class="field"><label>Holding Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-hold" value="3,000" oninput="calcRental()"></div></div>
            <div class="field"><label>HML Interest ($)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-hml-int" value="5,483" oninput="calcRental()"></div></div>
            <div class="field"><label>HML Points ($)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-hml-pts" value="0" oninput="calcRental()"></div></div>
            <div class="field"><label>GAP Points ($)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-gap-pts" value="0" oninput="calcRental()"></div></div>
            <div class="field"><label>GAP Interest ($)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-gap-int" value="0" oninput="calcRental()"></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">BRRRR Refinance</div>
          <div class="field-group">
            <div class="field"><label>ARV</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-arv" value="150,000" oninput="calcRental()"></div></div>
            <div class="field"><label>Refi LTV</label><div class="ci-wrap"><input class="ci-display pct" id="r-ltv" value="75" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Interest Rate (%/yr)</label><div class="ci-wrap"><input class="ci-display pct" id="r-rate" value="7" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Loan Term (Years)</label><div class="ci-wrap"><input class="ci-display plain" id="r-term" value="30" oninput="calcRental()"></div></div>
            <div class="field"><label>Refi Points Cost</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-refi-pts" value="0" oninput="calcRental()"></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Income & Monthly Expenses</div>
          <div class="field-group">
            <div class="field"><label>Gross Monthly Rent</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-rent" value="1,800" oninput="calcRental()"></div></div>
            <div class="field"><label>Vacancy Rate</label><div class="ci-wrap"><input class="ci-display pct" id="r-vacancy" value="8" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Property Mgmt</label><div class="ci-wrap"><input class="ci-display pct" id="r-mgmt" value="8" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Maintenance</label><div class="ci-wrap"><input class="ci-display pct" id="r-maint" value="5" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>CapEx Reserve</label><div class="ci-wrap"><input class="ci-display pct" id="r-capex" value="5" oninput="calcRental()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Insurance ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-ins" value="120" oninput="calcRental()"></div></div>
            <div class="field"><label>Property Taxes ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-tax" value="120" oninput="calcRental()"></div></div>
            <div class="field"><label>HOA ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-hoa-mo" value="0" oninput="calcRental()"></div></div>
            <div class="field"><label>Other ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="r-other" value="0" oninput="calcRental()"></div></div>
          </div>
        </div>
      </div>
      <div>
        <div class="card">
          <div class="card-title">BRRRR Summary</div>
          <div class="out-row"><span class="lbl">All-In Cost Basis</span><span class="val" id="ro-allin">—</span></div>
          <div class="out-row"><span class="lbl">Refi Loan Amount</span><span class="val" id="ro-refi">—</span></div>
          <div class="out-row"><span class="lbl">Refi Points Cost</span><span class="val" id="ro-refi-pts">—</span></div>
          <div class="out-row total"><span class="lbl">Cash Left In Deal</span><span class="val" id="ro-cash-left">—</span></div>
          <div class="out-row" style="border:none;padding-top:6px"><span class="lbl">Equity at Refi (ARV − Loan)</span><span class="val" id="ro-equity">—</span></div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Monthly Cash Flow</div>
          <div class="out-row"><span class="lbl">Gross Monthly Rent</span><span class="val" id="ro-rent">—</span></div>
          <div class="out-row"><span class="lbl">Vacancy Loss</span><span class="val" id="ro-vac">—</span></div>
          <div class="out-row"><span class="lbl">Effective Gross Income</span><span class="val" id="ro-egi">—</span></div>
          <div class="out-row"><span class="lbl">Property Management</span><span class="val" id="ro-mgmt">—</span></div>
          <div class="out-row"><span class="lbl">Maintenance</span><span class="val" id="ro-maint">—</span></div>
          <div class="out-row"><span class="lbl">CapEx Reserve</span><span class="val" id="ro-capex">—</span></div>
          <div class="out-row"><span class="lbl">Insurance</span><span class="val" id="ro-ins">—</span></div>
          <div class="out-row"><span class="lbl">Property Taxes</span><span class="val" id="ro-tax">—</span></div>
          <div class="out-row"><span class="lbl">HOA</span><span class="val" id="ro-hoa">—</span></div>
          <div class="out-row"><span class="lbl">Net Operating Income (NOI)</span><span class="val" id="ro-noi">—</span></div>
          <div class="out-row"><span class="lbl">Mortgage P&amp;I</span><span class="val" id="ro-mort">—</span></div>
          <div class="out-row total"><span class="lbl">Monthly Cash Flow</span><span class="val" id="ro-cf">—</span></div>
          <div class="out-row" style="border:none;padding-top:6px"><span class="lbl">Annual Cash Flow</span><span class="val" id="ro-acf">—</span></div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Key Metrics</div>
          <div class="metrics-grid">
            <div class="metric-box"><div class="m-val" id="r-m-dscr">—</div><div class="m-lbl">DSCR</div></div>
            <div class="metric-box"><div class="m-val" id="r-m-coc">—</div><div class="m-lbl">Cash-on-Cash</div></div>
            <div class="metric-box"><div class="m-val" id="r-m-cap">—</div><div class="m-lbl">Cap Rate</div></div>
            <div class="metric-box"><div class="m-val" id="r-m-grm">—</div><div class="m-lbl">Gross Rent Mult.</div></div>
          </div>
          <div id="rental-deal-badge"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== SUB-TO ====== -->
  <div id="panel-subto" class="panel">
    <div class="summary-bar">
      <div class="summary-pill"><div class="sp-val positive" id="st-s-profit">$0</div><div class="sp-lbl">Est. Profit</div></div>
      <div class="summary-pill"><div class="sp-val" id="st-s-allin">$0</div><div class="sp-lbl">All-In Cost</div></div>
      <div class="summary-pill"><div class="sp-val" id="st-s-fees">$0</div><div class="sp-lbl">Total Resale Fees</div></div>
      <div class="summary-pill"><div class="sp-val" id="st-s-monthly">$0/mo</div><div class="sp-lbl">Monthly Payment</div></div>
    </div>
    <div class="grid-2">
      <div>
        <div class="card">
          <div class="card-title">Acquisition Costs</div>
          <div class="field-group">
            <div class="field"><label>Entry Fee</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-entry" value="8,000" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Back Owed</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-back" value="0" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Soft Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-soft" value="1,800" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Closing Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-cc" value="500" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Acquisition Fee</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-acqfee" value="2,500" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Renovation</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-reno" value="3,650" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Months to Renovate</label><div class="ci-wrap"><input class="ci-display plain" id="st-months" value="6" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Money Cost (%/yr)</label><div class="ci-wrap"><input class="ci-display pct" id="st-money-cost" value="12" oninput="calcSubTo()"><span class="ci-suffix">%</span></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Existing Mortgage (Sub-To Terms)</div>
          <div class="field-group">
            <div class="field"><label>Mortgage Balance</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-bal" value="53,000" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Interest Rate (%/yr)</label><div class="ci-wrap"><input class="ci-display pct" id="st-mort-rate" value="5" oninput="calcSubTo()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Amortized (years)</label><div class="ci-wrap"><input class="ci-display plain" id="st-amort" value="30" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Years into Loan</label><div class="ci-wrap"><input class="ci-display plain" id="st-yr-in" value="0" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Taxes ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-taxes" value="47.17" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Insurance ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-ins" value="93.83" oninput="calcSubTo()"></div></div>
            <div class="field"><label>HOA ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-hoa" value="0" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Mortgage Insurance ($/mo)</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-mi" value="0" oninput="calcSubTo()"></div></div>
          </div>
        </div>
      </div>
      <div>
        <div class="card">
          <div class="card-title">Resale / Exit</div>
          <div class="field-group">
            <div class="field"><label>ARV</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-arv" value="150,000" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Resale Closing Costs</label><div class="ci-wrap"><span class="ci-prefix">$</span><input class="ci-display" id="st-res-cc" value="3,750" oninput="calcSubTo()"></div></div>
            <div class="field"><label>Seller Realtor Comm.</label><div class="ci-wrap"><input class="ci-display pct" id="st-sell-comm" value="1" oninput="calcSubTo()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Buyer Realtor Comm.</label><div class="ci-wrap"><input class="ci-display pct" id="st-buy-comm" value="3" oninput="calcSubTo()"><span class="ci-suffix">%</span></div></div>
            <div class="field"><label>Concessions</label><div class="ci-wrap"><input class="ci-display pct" id="st-conc" value="1" oninput="calcSubTo()"><span class="ci-suffix">%</span></div></div>
          </div>
        </div>
        <div class="card mt-18">
          <div class="card-title">Summary</div>
          <div class="out-row"><span class="lbl">Subtotal (pre-carry)</span><span class="val" id="sto-subtotal">—</span></div>
          <div class="out-row"><span class="lbl">Money Cost (carrying)</span><span class="val" id="sto-money-cost">—</span></div>
          <div class="out-row total"><span class="lbl">Total All-In</span><span class="val" id="sto-allin">—</span></div>
          <hr>
          <div class="out-row"><span class="lbl">Principal</span><span class="val" id="sto-prin">—</span></div>
          <div class="out-row"><span class="lbl">Interest</span><span class="val" id="sto-int">—</span></div>
          <div class="out-row"><span class="lbl">Taxes</span><span class="val" id="sto-taxes">—</span></div>
          <div class="out-row"><span class="lbl">Insurance</span><span class="val" id="sto-ins">—</span></div>
          <div class="out-row"><span class="lbl">HOA</span><span class="val" id="sto-hoa">—</span></div>
          <div class="out-row total"><span class="lbl">Total Monthly Payment</span><span class="val" id="sto-monthly">—</span></div>
          <hr>
          <div class="out-row"><span class="lbl">ARV</span><span class="val" id="sto-arv">—</span></div>
          <div class="out-row"><span class="lbl">Seller Realtor Fee</span><span class="val" id="sto-sell-fee">—</span></div>
          <div class="out-row"><span class="lbl">Buyer Realtor Fee</span><span class="val" id="sto-buy-fee">—</span></div>
          <div class="out-row"><span class="lbl">Concessions</span><span class="val" id="sto-conc">—</span></div>
          <div class="out-row"><span class="lbl">Resale Closing Costs</span><span class="val" id="sto-res-cc">—</span></div>
          <div class="out-row total"><span class="lbl">Total Resale Fees</span><span class="val" id="sto-total-fees">—</span></div>
          <hr>
          <div class="out-row total" style="border-top:2px solid var(--accent2);">
            <span class="lbl" style="color:var(--accent2)">Estimated Profit</span>
            <span class="val" style="font-size:18px" id="sto-profit">—</span>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== REHAB ====== -->
  <div id="panel-rehab" class="panel">
    <div class="summary-bar">
      <div class="summary-pill"><div class="sp-val positive" id="rh-s-total">$0</div><div class="sp-lbl">Total Rehab</div></div>
      <div class="summary-pill"><div class="sp-val" id="rh-s-int">$0</div><div class="sp-lbl">Interior</div></div>
      <div class="summary-pill"><div class="sp-val" id="rh-s-sys">$0</div><div class="sp-lbl">Systems</div></div>
      <div class="summary-pill"><div class="sp-val" id="rh-s-ext">$0</div><div class="sp-lbl">Kitchen/Bath/Ext.</div></div>
    </div>
    <div class="card">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;flex-wrap:wrap;gap:10px;">
        <div class="card-title" style="margin:0;border:none;padding:0;">Line Item Estimator</div>
        <div style="font-size:12px;color:var(--text-secondary);">Property SQFT: <span id="rh-sqft-ref" style="color:var(--accent);font-weight:600;">1,000</span> — auto-applied to /SQFT items</div>
      </div>
      <div id="rehab-list"></div>
      <div style="display:flex;justify-content:space-between;align-items:center;padding:16px 0 4px;border-top:2px solid var(--accent);margin-top:10px;">
        <span style="font-size:14px;font-weight:600;">Grand Total</span>
        <span style="font-family:'Lora',serif;font-size:28px;font-weight:700;color:var(--accent);" id="rh-grand-total">$0</span>
      </div>
      <div style="text-align:right;margin-top:10px;">
        <button class="btn btn-primary" onclick="pushRehabToFlip()">Push to Flip &amp; Rental →</button>
      </div>
    </div>
  </div>

  <div style="height:60px"></div>
</div>

<script>
// ── Tooltip definitions ────────────────────────
const TIPS = {
  // Property bar
  'prop-arv':    { title:'ARV — After Repair Value', body:'The estimated market value of the property after all renovations are complete. This is your ceiling — every cost is measured against it.', good:'Pull comps from Zillow/MLS within 0.5 mi, similar SQFT ±15%.' },
  'prop-sqft':   { title:'Square Footage', body:'Heated/livable square footage of the home. Used to auto-calculate per-sqft rehab line items in the Rehab Builder tab.' },
  'prop-hoa':    { title:'HOA — Homeowners Association Fee', body:'Monthly fee charged by the neighborhood association. Factors into holding costs on a flip and monthly expenses on a rental.' },

  // Flip
  'f-purchase':  { title:'Purchase Price', body:'The price you are paying to acquire the property. This is your biggest lever — lower purchase price = more profit margin.' },
  'f-rehab':     { title:'Rehab Budget', body:'Total estimated cost of all repairs and renovations. Use the Rehab Builder tab to build a line-by-line estimate, then push it here.' },
  'f-cc':        { title:'Closing Costs (Buy-Side)', body:'Fees paid at closing when you purchase — title, escrow, lender fees, inspections, etc. Typically 1–3% of purchase price.' },
  'f-scc':       { title:'Sellers Closing Costs', body:'Any closing costs you agree to cover on behalf of the seller as a negotiation concession.' },
  'f-hold':      { title:'Holding Costs', body:'Ongoing costs while you own the property during the project — utilities, insurance, property taxes, lawn care. Estimate $500–$1,000/mo.' },
  'f-months':    { title:'Project Months', body:'How long from purchase to closing on the resale. Affects HML interest calculations. Be conservative — most projects run longer than planned.' },
  'f-hml-ltv':   { title:'HML LTV — Loan-to-Value', body:'The fraction of your Total Project Cost that the Hard Money Lender will finance. 1.0 = 100% of TPC covered. Most HMLs cap at 0.65–0.75 of ARV.', good:'Common range: 0.65–0.80 of ARV, or 100% of purchase+rehab.' },
  'f-hml-pts':   { title:'HML Points', body:'Upfront fee charged by the Hard Money Lender, expressed as a percentage of the loan amount. 1 point = 1% of loan.', good:'Typical range: 1–3 points.' },
  'f-hml-rate':  { title:'HML Interest Rate', body:'Annual interest rate charged by the Hard Money Lender on the outstanding loan balance.', good:'Common range: 9–14% annually.' },
  'f-gap-pts':   { title:'GAP Loan Points', body:'Points (upfront fee) charged by a second "gap" lender that funds the portion your HML doesn\'t cover.', good:'Often 2–4 points for gap capital.' },
  'f-gap-rate':  { title:'GAP Loan Interest Rate', body:'Annual interest rate on the gap loan. Gap lenders charge more than HML since they are in second position.', good:'Typical range: 12–18% annually.' },
  'f-recomm':    { title:'RE Commission', body:'Real estate agent commission paid at resale, as a % of the sale price. Paid to buyer\'s and/or seller\'s agent.', good:'Buyer\'s agent: ~2.5–3%. Seller\'s agent: 2.5–3%. Total: 5–6%.' },
  'f-rcc':       { title:'Resale Closing Costs', body:'Title, escrow, transfer taxes, and misc fees paid when you sell the property. Typically 1–2% of sale price.' },
  'f-min-pct':   { title:'Minimum Profit % of ARV', body:'Your personal deal threshold — the profit must be at least this % of ARV to qualify as a "deal." 12% is a common floor.', good:'Many experienced investors require 15–20% of ARV.' },
  'f-min-dol':   { title:'Minimum Profit $ Amount', body:'Your minimum acceptable dollar profit regardless of percentages. Ensures small deals aren\'t worth your time.', good:'Common floor: $20,000–$30,000 minimum.' },

  // Rental / BRRRR
  'r-hml-int':   { title:'Front-End HML Interest', body:'Total interest paid to the Hard Money Lender during the rehab/hold period before the refi. This is a sunk cost included in your all-in basis.' },
  'r-ltv':       { title:'Refi LTV — Loan-to-Value', body:'The percentage of ARV the bank will lend on the refinanced property. 75% means you borrow 75% of ARV.', good:'Most conventional lenders: 75–80% LTV. Portfolio lenders: 70–80%.' },
  'r-arv':       { title:'ARV — After Repair Value', body:'Estimated market value after renovations. The refi loan is based on this number, so getting your comps right is critical.' },
  'r-vacancy':   { title:'Vacancy Rate', body:'% of the year the unit sits empty between tenants. Used to reduce gross rent to effective income.', good:'Budget 8–10% in most markets (≈1 month/year vacant).' },
  'r-mgmt':      { title:'Property Management Fee', body:'Monthly fee paid to a property manager, as % of effective gross income. Even if self-managing, budget this for future scaling.', good:'Typical range: 8–12% of collected rent.' },
  'r-maint':     { title:'Maintenance Reserve', body:'Budget for routine repairs — leaky faucets, HVAC filters, appliances, etc. Applied as % of effective gross income.', good:'Rule of thumb: 5–10% of rents.' },
  'r-capex':     { title:'CapEx Reserve', body:'Capital expenditure reserve for big-ticket replacements — roof, HVAC, water heater, flooring. These are infrequent but expensive.', good:'Budget 5–10% of rents; newer/rehabbed properties need less.' },
  'r-ltv2':      { title:'Refi LTV', body:'The loan-to-value ratio used in BRRRR refinancing.' },
  'ro-cash-left':{ title:'Cash Left In Deal', body:'All-In Cost minus the Refi Loan Amount. Negative = you pulled money OUT (the BRRRR ideal). Positive = money still tied up.', good:'BRRRR goal: $0 or negative (full capital recycling).' },
  'r-s-dscr':    { title:'DSCR', body:'Debt Service Coverage Ratio. NOI ÷ Mortgage payment. Shows if rental income covers the debt.', good:'Lenders typically require ≥1.25. Below 1.0 = negative cash flow.' },
  'r-s-coc':     { title:'Cash-on-Cash Return', body:'Annual cash flow ÷ cash invested. The true return on your out-of-pocket money.', good:'Target: 8–12%+ annually. Below 6% is weak.' },
  'r-s-cap':     { title:'Cap Rate', body:'Net Operating Income ÷ Property Value. Measures return independent of financing.', good:'8–10%+ is solid. Below 5% is low (common in expensive markets).' },

  // Sub-To
  'st-entry':    { title:'Entry Fee', body:'The upfront cash paid directly to the seller to acquire the property subject-to their existing mortgage. This is your "purchase price" in a sub-to deal.' },
  'st-back':     { title:'Back Owed', body:'Any past-due mortgage payments, HOA arrears, or liens you agree to cure to get the deal done. Paid at acquisition.' },
  'st-soft':     { title:'Soft Costs', body:'Ongoing soft costs during the renovation period — utilities kept on, lawn maintenance, etc.' },
  'st-acqfee':   { title:'Acquisition Fee', body:'Fee paid to a wholesaler or deal finder who brought you the sub-to opportunity.' },
  'st-money-cost': { title:'Money Cost', body:'The cost of any private money or hard money used to fund the acquisition and renovation, expressed as an annual % rate.', good:'Budget 10–15% annually for private capital.' },
  'st-bal':      { title:'Mortgage Balance', body:'The remaining principal owed on the seller\'s existing mortgage that you are taking over subject-to. You make their payments, but the loan stays in their name.' },
  'st-mort-rate':{ title:'Existing Mortgage Rate', body:'The interest rate on the seller\'s loan. This is the key advantage of sub-to — you inherit their rate, which could be well below current market rates.' },
  'st-amort':    { title:'Amortization Term', body:'The original loan term in years. Used to calculate the principal/interest split of each payment.' },
  'st-yr-in':    { title:'Years Into Loan', body:'How many years the seller has already been paying. Affects how much principal vs. interest is in each payment — older loans have more principal.' },
  'st-sell-comm':{ title:'Seller Realtor Commission', body:'Commission % paid to the listing agent at resale.' },
  'st-buy-comm': { title:'Buyer Realtor Commission', body:'Commission % paid to the buyer\'s agent at resale.' },
  'st-conc':     { title:'Concessions', body:'Seller concessions to the buyer — closing cost credits, price reductions, or repair allowances agreed to close the deal.', good:'Budget 1–2% as a cushion.' },

  // Metrics
  'dscr':     { title:'DSCR — Debt Service Coverage Ratio', body:'Net Operating Income ÷ Annual Debt Service. Tells you if your rental income covers the mortgage.', good:'≥1.25 = strong. 1.0–1.25 = tight. <1.0 = losing money.' },
  'coc':      { title:'Cash-on-Cash Return', body:'Annual cash flow ÷ total cash invested. Your actual annual return on the dollars you put in.', good:'Target 8–12%+. Below 6% is generally not worth the risk.' },
  'caprate':  { title:'Cap Rate — Capitalization Rate', body:'NOI ÷ Property Value (or purchase price). Measures return as if you paid all cash — no financing effect.', good:'Single family: 5–10%. Multi-family: 5–8% in most markets.' },
  'grm':      { title:'GRM — Gross Rent Multiplier', body:'Purchase Price ÷ Annual Gross Rent. Lower = better deal. Quick screening metric before deep analysis.', good:'Below 8x is generally strong. 10–12x is common in average markets.' },
  'brrrr':    { title:'BRRRR Strategy', body:'Buy, Rehab, Rent, Refinance, Repeat. You rehab a distressed property, rent it, then cash-out refi to pull your capital back out and do it again.', good:'Goal: recycle 100% of capital so each deal costs you nothing long-term.' },
  'tpc':      { title:'TPC — Total Project Cost', body:'Every dollar spent to acquire, fix, finance, and hold the property through to sale. Your all-in cost basis.', good:'Keep TPC below 75% of ARV to maintain a safe profit margin.' },
  'hml':      { title:'HML — Hard Money Loan', body:'Short-term, asset-based loan from a private lender used to fund fix-and-flip projects. Higher rates, faster approval than banks.', good:'Typical terms: 9–14% interest, 1–3 points, 6–18 month term.' },
  'gap':      { title:'GAP Loan', body:'Secondary financing that covers the "gap" between what the HML will lend and your total project cost. Higher cost, second-lien position.', good:'Use sparingly — gap capital is expensive and eats into profit.' },
};

// Tooltip engine
let tipTimeout = null;
function showTip(btnEl, key) {
  const def = TIPS[key]; if (!def) return;
  const pop = document.getElementById('tip-popover');
  document.getElementById('tip-title').textContent = def.title;
  document.getElementById('tip-body').textContent  = def.body;
  const goodEl = document.getElementById('tip-good');
  goodEl.textContent = def.good || ''; goodEl.style.display = def.good ? '' : 'none';
  // Position
  const r = btnEl.getBoundingClientRect();
  const pw = 260, ph = 120;
  let left = r.right + 10;
  let top  = r.top - 10;
  if (left + pw > window.innerWidth - 12) left = r.left - pw - 10;
  if (top + ph > window.innerHeight - 12) top = window.innerHeight - ph - 12;
  pop.style.left = left + 'px';
  pop.style.top  = top  + 'px';
  clearTimeout(tipTimeout);
  pop.classList.add('visible');
}
function hideTip() {
  tipTimeout = setTimeout(() => document.getElementById('tip-popover').classList.remove('visible'), 120);
}

// Inject tooltip buttons next to labels that have a tip key
function injectTooltips() {
  // Map of element id → tip key (for input fields)
  const fieldMap = {
    'f-purchase':'f-purchase','f-rehab':'f-rehab','f-cc':'f-cc','f-scc':'f-scc',
    'f-hold':'f-hold','f-months':'f-months','f-hml-ltv':'f-hml-ltv',
    'f-hml-pts':'f-hml-pts','f-hml-rate':'f-hml-rate','f-gap-pts':'f-gap-pts',
    'f-gap-rate':'f-gap-rate','f-recomm':'f-recomm','f-rcc':'f-rcc',
    'f-min-pct':'f-min-pct','f-min-dol':'f-min-dol',
    'r-hml-int':'r-hml-int','r-arv':'r-arv','r-ltv':'r-ltv',
    'r-vacancy':'r-vacancy','r-mgmt':'r-mgmt','r-maint':'r-maint',
    'r-capex':'r-capex','prop-arv':'prop-arv','prop-sqft':'prop-sqft','prop-hoa':'prop-hoa',
    'st-entry':'st-entry','st-back':'st-back','st-soft':'st-soft',
    'st-acqfee':'st-acqfee','st-money-cost':'st-money-cost','st-bal':'st-bal',
    'st-mort-rate':'st-mort-rate','st-amort':'st-amort','st-yr-in':'st-yr-in',
    'st-sell-comm':'st-sell-comm','st-buy-comm':'st-buy-comm','st-conc':'st-conc',
  };
  // For each field input, find its sibling label and append a tip button
  Object.entries(fieldMap).forEach(([inputId, tipKey]) => {
    const input = document.getElementById(inputId); if (!input) return;
    const field = input.closest('.field') || input.closest('.prop-field'); if (!field) return;
    const label = field.querySelector('label'); if (!label) return;
    if (label.querySelector('.tip-btn')) return; // already injected
    const btn = document.createElement('button');
    btn.type = 'button'; btn.className = 'tip-btn'; btn.textContent = 'i';
    btn.addEventListener('mouseenter', () => showTip(btn, tipKey));
    btn.addEventListener('mouseleave', hideTip);
    btn.addEventListener('click', e => { e.stopPropagation(); showTip(btn, tipKey); });
    label.appendChild(btn);
  });

  // Metric boxes
  const metricMap = { 'r-m-dscr':'dscr','r-m-coc':'coc','r-m-cap':'caprate','r-m-grm':'grm' };
  Object.entries(metricMap).forEach(([valId, tipKey]) => {
    const box = document.getElementById(valId)?.closest('.metric-box'); if (!box) return;
    const lbl = box.querySelector('.m-lbl'); if (!lbl || lbl.querySelector('.tip-btn')) return;
    const btn = document.createElement('button');
    btn.type='button'; btn.className='tip-btn'; btn.textContent='i';
    btn.style.marginLeft='4px'; btn.style.verticalAlign='middle';
    btn.addEventListener('mouseenter', () => showTip(btn, tipKey));
    btn.addEventListener('mouseleave', hideTip);
    lbl.appendChild(btn);
  });

  // Summary pill labels
  const pillMap = { 'r-s-dscr':'dscr','r-s-coc':'coc','r-s-cap':'caprate' };
  Object.entries(pillMap).forEach(([valId, tipKey]) => {
    const pill = document.getElementById(valId)?.closest('.summary-pill'); if (!pill) return;
    const lbl = pill.querySelector('.sp-lbl'); if (!lbl || lbl.querySelector('.tip-btn')) return;
    const btn = document.createElement('button');
    btn.type='button'; btn.className='tip-btn'; btn.textContent='i';
    btn.style.marginLeft='4px'; btn.style.verticalAlign='middle';
    btn.addEventListener('mouseenter', () => showTip(btn, tipKey));
    btn.addEventListener('mouseleave', hideTip);
    lbl.appendChild(btn);
  });

  // Output row labels (TPC, HML, GAP, BRRRR, cash-left)
  const rowLblMap = [
    ['fo-tpc','tpc'],['fo-hml-loan','hml'],['fo-gap-loan','gap'],
    ['ro-cash-left','ro-cash-left'],
  ];
  rowLblMap.forEach(([valId, tipKey]) => {
    const row = document.getElementById(valId)?.closest('.out-row'); if (!row) return;
    const lbl = row.querySelector('.lbl'); if (!lbl || lbl.querySelector('.tip-btn')) return;
    const btn = document.createElement('button');
    btn.type='button'; btn.className='tip-btn'; btn.textContent='i';
    btn.style.marginLeft='4px'; btn.style.verticalAlign='middle';
    btn.addEventListener('mouseenter', () => showTip(btn, tipKey));
    btn.addEventListener('mouseleave', hideTip);
    lbl.appendChild(btn);
  });
}

// ── Settings ───────────────────────────────────
const SETTINGS_KEY = 'pa_settings_v1';

function loadSettings() {
  try { return JSON.parse(localStorage.getItem(SETTINGS_KEY)) || {}; } catch { return {}; }
}
function openSettings() {
  const s = loadSettings();
  document.getElementById('rc-api-key').value   = s.rcKey   || '';
  document.getElementById('def-hml-rate').value = s.defHmlRate || '10';
  document.getElementById('def-months').value   = s.defMonths  || '6';
  document.getElementById('def-ltv').value      = s.defLtv     || '75';
  document.getElementById('def-min-pct').value  = s.defMinPct  || '12';
  updateApiStatus(s.rcKey);
  document.getElementById('settings-modal').classList.add('open');
}
function closeSettings() { document.getElementById('settings-modal').classList.remove('open'); }
function saveSettings() {
  const s = {
    rcKey:      document.getElementById('rc-api-key').value.trim(),
    defHmlRate: document.getElementById('def-hml-rate').value.trim(),
    defMonths:  document.getElementById('def-months').value.trim(),
    defLtv:     document.getElementById('def-ltv').value.trim(),
    defMinPct:  document.getElementById('def-min-pct').value.trim(),
  };
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
  updateApiStatus(s.rcKey);
  updateLookupBtn(!!s.rcKey);
  closeSettings();
}
function updateApiStatus(key) {
  const el = document.getElementById('rc-status');
  if (key) { el.textContent = '● API key saved'; el.className = 'api-status connected'; }
  else      { el.textContent = '● No key saved';  el.className = 'api-status disconnected'; }
}
function updateLookupBtn(hasKey) {
  const btn = document.getElementById('lookup-btn');
  if (!btn) return;
  btn.className = 'lookup-btn' + (hasKey ? ' has-key' : '');
  btn.title = hasKey ? 'Auto-fill from Rentcast' : 'Add a Rentcast API key in Settings to enable';
}
document.getElementById('settings-modal').addEventListener('click', e => {
  if (e.target === document.getElementById('settings-modal')) closeSettings();
});

// ── Rentcast Lookup ────────────────────────────
async function lookupProperty() {
  const s = loadSettings();
  if (!s.rcKey) {
    openSettings();
    return;
  }
  const address = document.getElementById('prop-address').value.trim();
  if (!address) { alert('Enter a property address first.'); return; }
  const btn = document.getElementById('lookup-btn');
  btn.classList.add('loading'); btn.textContent = '⏳ Looking up…';
  try {
    const url = `https://api.rentcast.io/v1/properties?address=${encodeURIComponent(address)}&limit=1`;
    const res = await fetch(url, { headers: { 'X-Api-Key': s.rcKey, 'Accept': 'application/json' } });
    if (!res.ok) throw new Error(`API error ${res.status}`);
    const data = await res.json();
    const prop = Array.isArray(data) ? data[0] : data?.properties?.[0] || data;
    if (!prop) throw new Error('No property found');
    // Fill fields
    const sqft = prop.squareFootage || prop.livingArea || '';
    const beds = prop.bedrooms || '';
    const baths = prop.bathrooms || '';
    const garage = prop.garageSpaces || '';
    const year = prop.yearBuilt || '';
    const hoa = prop.hoaFee || 0;
    const arvEst = prop.price || prop.estimatedValue || prop.zestimate || '';
    const rentEst = prop.rentEstimate || prop.estimatedRent || '';
    if (sqft)   { document.getElementById('prop-sqft').value = Number(sqft).toLocaleString(); }
    if (beds || baths || garage) {
      document.getElementById('prop-bbb').value = [beds, baths, garage].filter(Boolean).join('/');
    }
    if (year)   { document.getElementById('prop-year').value = year; }
    if (hoa)    { document.getElementById('prop-hoa').value  = Number(hoa).toLocaleString(); }
    if (arvEst) {
      const fmtd = Number(arvEst).toLocaleString();
      document.getElementById('prop-arv').value = fmtd;
      document.getElementById('r-arv').value    = fmtd;
    }
    if (rentEst) { document.getElementById('r-rent').value = Number(rentEst).toLocaleString(); }
    syncAll(); calcSubTo();
    btn.textContent = '✓ Filled!';
    setTimeout(() => { btn.textContent = '🔍 Lookup Property'; btn.classList.remove('loading'); }, 2000);
  } catch(e) {
    btn.textContent = '✗ Error'; btn.classList.remove('loading');
    setTimeout(() => { btn.textContent = '🔍 Lookup Property'; }, 2000);
    alert(`Lookup failed: ${e.message}\n\nCheck your API key in Settings or verify the address format.`);
  }
}

// ── Utilities ──────────────────────────────────
const $ = id => document.getElementById(id);

function pv(id) {
  const el = $(id);
  if (!el) return 0;
  return parseFloat(el.value.replace(/,/g,'').replace(/\$/g,'').trim()) || 0;
}
function pp(id) { return pv(id) / 100; }

function fmt(n, d=0) {
  if (isNaN(n) || n===null) return '—';
  const abs = Math.abs(n);
  const s = abs.toLocaleString('en-US',{minimumFractionDigits:d,maximumFractionDigits:d});
  return n < 0 ? `($${s})` : `$${s}`;
}
function fmtPct(n,d=1) { return isNaN(n)?'—':`${(n*100).toFixed(d)}%`; }
function fmtX(n)       { return isNaN(n)?'—':`${n.toFixed(2)}x`; }

function commaify(el) {
  const raw = el.value.replace(/,/g,'').replace(/[^0-9.]/g,'');
  if (!raw || raw==='.') return;
  const parts = raw.split('.');
  const prev = el.value.length, cur = el.selectionStart;
  parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g,',');
  el.value = parts.join('.');
  const diff = el.value.length - prev;
  try { el.setSelectionRange(cur+diff,cur+diff); } catch(e){}
}

function attachCommas() {
  document.querySelectorAll('.ci-display:not(.pct):not(.plain)').forEach(el=>el.addEventListener('input',()=>commaify(el)));
  ['prop-sqft','prop-arv','prop-hoa'].forEach(id=>{
    const el=$(id); if(el) el.addEventListener('input',()=>commaify(el));
  });
}

function cv(el,n){ if(!el) return; el.className='val '+(n>0?'positive':n<0?'negative':'neutral'); }

function setBadge(id,state) {
  const el=$(id); if(!el) return;
  if(state===true)  el.innerHTML='<span class="deal-badge yes">✓ Deal!</span>';
  else if(state===false) el.innerHTML='<span class="deal-badge no">✗ Not a Deal</span>';
  else el.innerHTML='<span class="deal-badge maybe">~ Marginal</span>';
}

function showTab(name,btn) {
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  $('panel-'+name).classList.add('active');
  if(btn) btn.classList.add('active');
}

function updateHeader() {
  const a = $('prop-address').value||'No Address Entered';
  $('hdr-addr').textContent=a;
}

function syncAll() {
  const arv=pv('prop-arv'), sqft=pv('prop-sqft');
  $('hdr-sub').textContent=`ARV: ${fmt(arv)}  ·  SQFT: ${sqft.toLocaleString()}`;
  const rr=$('rh-sqft-ref'); if(rr) rr.textContent=sqft.toLocaleString();
  calcFlip(); calcRental(); calcRehab();
}

// ── Flip ───────────────────────────────────────
function calcFlip() {
  const purchase=pv('f-purchase'),rehab=pv('f-rehab'),cc=pv('f-cc'),scc=pv('f-scc'),hold=pv('f-hold'),months=pv('f-months');
  const hmlLtvF=pv('f-hml-ltv'),hmlPtsPct=pp('f-hml-pts'),hmlRatePct=pp('f-hml-rate');
  const gapPtsPct=pp('f-gap-pts'),gapRatePct=pp('f-gap-rate');
  const reCommPct=pp('f-recomm'),rccPct=pp('f-rcc');
  const intType=$('f-int-type').value, arv=pv('prop-arv');

  const base=purchase+rehab+cc+scc+hold;
  const hmlLoan=hmlLtvF*base;
  const hmlPts=hmlLoan*hmlPtsPct;
  const hmlInt=intType==='annual'?hmlLoan*hmlRatePct*(months/12):hmlLoan*hmlRatePct*months;
  const hmlFees=hmlPts+hmlInt;
  const gapLoan=hmlFees, gapInt=gapLoan*gapRatePct*(months/12);
  const tpc=base+hmlFees+gapInt, tpcPct=arv>0?tpc/arv:0;
  const reComm=arv*reCommPct, rcc=arv*rccPct, netResale=arv-reComm-rcc;
  const profit=netResale-tpc, profitPct=arv>0?profit/arv:0, roi=tpc>0?profit/tpc:0;

  $('fo-purchase').textContent=fmt(purchase); $('fo-rehab').textContent=fmt(rehab);
  $('fo-cc').textContent=fmt(cc+scc); $('fo-hold').textContent=fmt(hold);
  $('fo-hml-loan').textContent=fmt(hmlLoan); $('fo-hml-pts').textContent=fmt(hmlPts);
  $('fo-hml-int').textContent=fmt(hmlInt); $('fo-gap-loan').textContent=fmt(gapLoan);
  $('fo-gap-int').textContent=fmt(gapInt); $('fo-tpc').textContent=fmt(tpc);
  $('fo-tpc-pct').textContent=fmtPct(tpcPct); $('fo-arv').textContent=fmt(arv);
  $('fo-recomm').textContent=fmt(-reComm); $('fo-rcc').textContent=fmt(-rcc);
  $('fo-net').textContent=fmt(netResale);
  const pe=$('fo-profit'); pe.textContent=fmt(profit); cv(pe,profit);
  $('fo-profit-pct').textContent=fmtPct(profitPct); $('fo-roi').textContent=fmtPct(roi);

  const minPct=pp('f-min-pct'),minDol=pv('f-min-dol');
  const isDeal=profit>=minDol&&profitPct>=minPct;
  const isMaybe=(profit>=minDol*.8||profitPct>=minPct*.8)&&!isDeal;

  const pd=$('f-s-profit'); pd.textContent=fmt(profit); pd.className='sp-val '+(profit>0?'positive':'negative');
  $('f-s-roi').textContent=fmtPct(roi); $('f-s-tpc').textContent=fmt(tpc);
  const dd=$('f-s-deal'); dd.textContent=isDeal?'Deal!':isMaybe?'Marginal':'Pass';
  dd.className='sp-val '+(isDeal?'positive':isMaybe?'neutral':'negative');
  setBadge('flip-deal-badge',isDeal?true:isMaybe?null:false);
  buildScenario(tpc,reCommPct,rccPct,arv);
}

function buildScenario(tpc,reCommPct,rccPct,arv) {
  const base=Math.round(arv/5000)*5000;
  const tbody=$('flip-scenario-body'); tbody.innerHTML='';
  [-3,-2,-1,0,1,2,3,4,5,6,7,8].forEach(s=>{
    const p=base+s*5000; if(p<=0) return;
    const net=p*(1-reCommPct-rccPct), profit=net-tpc;
    const hl=Math.abs(p-arv)<2500;
    const cls=profit>10000?'pp':profit<0?'pn':'pm';
    tbody.innerHTML+=`<tr class="${hl?'hl':''}"><td>${fmt(p)}</td><td>${fmt(net)}</td><td class="${cls}">${fmt(profit)}</td></tr>`;
  });
}

// ── Rental ─────────────────────────────────────
function calcRental() {
  const purchase=pv('r-purchase'),rehab=pv('r-rehab'),cc=pv('r-cc'),hold=pv('r-hold');
  const hmlInt=pv('r-hml-int'),hmlPts=pv('r-hml-pts'),gapPts=pv('r-gap-pts'),gapInt=pv('r-gap-int');
  const arv=pv('r-arv'),ltv=pp('r-ltv'),rate=pp('r-rate'),term=pv('r-term'),refiPts=pv('r-refi-pts');
  const rent=pv('r-rent'),vacPct=pp('r-vacancy'),mgmtPct=pp('r-mgmt'),maintPct=pp('r-maint'),capexPct=pp('r-capex');
  const ins=pv('r-ins'),taxes=pv('r-tax'),hoa=pv('r-hoa-mo'),other=pv('r-other');

  const allIn=purchase+rehab+cc+hold+hmlInt+hmlPts+gapPts+gapInt;
  const refiLoan=arv*ltv, cashLeft=allIn-refiLoan+refiPts, equity=arv-refiLoan;
  const mr=rate/12,n=term*12;
  let mort=0;
  if(mr>0&&n>0) mort=refiLoan*(mr*Math.pow(1+mr,n))/(Math.pow(1+mr,n)-1);
  else if(n>0) mort=refiLoan/n;

  const vacLoss=-rent*vacPct, egi=rent+vacLoss;
  const mgmtA=-egi*mgmtPct, maintA=-egi*maintPct, capexA=-egi*capexPct;
  const noi=egi+mgmtA+maintA+capexA-ins-taxes-hoa-other;
  const cf=noi-mort, acf=cf*12;
  const dscr=mort>0?noi/mort:0, coc=cashLeft>0?acf/cashLeft:0;
  const capRate=allIn>0?(noi*12)/allIn:0, grm=rent>0?allIn/(rent*12):0;

  $('ro-allin').textContent=fmt(allIn); $('ro-refi').textContent=fmt(refiLoan);
  $('ro-refi-pts').textContent=fmt(refiPts);
  const cl=$('ro-cash-left'); cl.textContent=fmt(cashLeft); cv(cl,-cashLeft);
  $('ro-equity').textContent=fmt(equity); $('ro-rent').textContent=fmt(rent);
  $('ro-vac').textContent=fmt(vacLoss); $('ro-egi').textContent=fmt(egi);
  $('ro-mgmt').textContent=fmt(mgmtA); $('ro-maint').textContent=fmt(maintA);
  $('ro-capex').textContent=fmt(capexA); $('ro-ins').textContent=fmt(-ins);
  $('ro-tax').textContent=fmt(-taxes); $('ro-hoa').textContent=fmt(-hoa);
  $('ro-noi').textContent=fmt(noi); $('ro-mort').textContent=fmt(-mort);
  const cfe=$('ro-cf'); cfe.textContent=fmt(cf); cv(cfe,cf);
  $('ro-acf').textContent=fmt(acf);

  const sm=(id,v,g,w)=>{ const e=$(id); e.className='m-val '+(v>=g?'positive':v>=w?'neutral':'negative'); };
  $('r-m-dscr').textContent=dscr.toFixed(2); sm('r-m-dscr',dscr,1.25,1);
  $('r-m-coc').textContent=fmtPct(coc); sm('r-m-coc',coc*100,8,0);
  $('r-m-cap').textContent=fmtPct(capRate); sm('r-m-cap',capRate*100,8,5);
  $('r-m-grm').textContent=fmtX(grm);

  const scd=$('r-s-cf'); scd.textContent=fmt(cf)+'/mo'; scd.className='sp-val '+(cf>=0?'positive':'negative');
  $('r-s-coc').textContent=fmtPct(coc); $('r-s-cap').textContent=fmtPct(capRate); $('r-s-dscr').textContent=dscr.toFixed(2);
  const isDeal=cf>0&&dscr>=1.25, isMaybe=cf>0&&dscr>=1;
  setBadge('rental-deal-badge',isDeal?true:isMaybe?null:false);
}

// ── Sub-To ─────────────────────────────────────
function calcSubTo() {
  const entry=pv('st-entry'),back=pv('st-back'),soft=pv('st-soft'),cc=pv('st-cc'),acqFee=pv('st-acqfee'),reno=pv('st-reno');
  const months=pv('st-months'),mcp=pp('st-money-cost');
  const sub=entry+back+soft+cc+acqFee+reno, mc=sub*mcp*(months/12), allIn=sub+mc;
  const bal=pv('st-bal'),mr=pp('st-mort-rate'),amort=pv('st-amort'),yrIn=pv('st-yr-in');
  const taxes=pv('st-taxes'),ins=pv('st-ins'),hoa=pv('st-hoa'),mi=pv('st-mi');
  const mrate=mr/12, n=amort*12;
  let pmt=0;
  if(mrate>0&&n>0) pmt=bal*(mrate*Math.pow(1+mrate,n))/(Math.pow(1+mrate,n)-1);
  else if(n>0) pmt=bal/n;
  const pmtsMade=yrIn*12;
  let curBal=bal;
  if(pmtsMade>0&&mrate>0) curBal=bal*(Math.pow(1+mrate,n)-Math.pow(1+mrate,pmtsMade))/(Math.pow(1+mrate,n)-1);
  const intPart=curBal*mrate, prinPart=pmt-intPart;
  const totalMo=prinPart+intPart+taxes+ins+hoa+mi;
  const arv=pv('st-arv'),resCc=pv('st-res-cc'),sellC=pp('st-sell-comm'),buyC=pp('st-buy-comm'),concP=pp('st-conc');
  const sf=arv*sellC, bf=arv*buyC, conc=arv*concP, totalFees=resCc+sf+bf+conc;
  const profit=arv-totalFees-allIn;

  $('sto-subtotal').textContent=fmt(sub); $('sto-money-cost').textContent=fmt(mc); $('sto-allin').textContent=fmt(allIn);
  $('sto-prin').textContent=fmt(prinPart); $('sto-int').textContent=fmt(intPart);
  $('sto-taxes').textContent=fmt(taxes); $('sto-ins').textContent=fmt(ins); $('sto-hoa').textContent=fmt(hoa);
  $('sto-monthly').textContent=fmt(totalMo); $('sto-arv').textContent=fmt(arv);
  $('sto-sell-fee').textContent=fmt(-sf); $('sto-buy-fee').textContent=fmt(-bf);
  $('sto-conc').textContent=fmt(-conc); $('sto-res-cc').textContent=fmt(-resCc);
  $('sto-total-fees').textContent=fmt(totalFees);
  const pe=$('sto-profit'); pe.textContent=fmt(profit); cv(pe,profit);
  $('st-s-profit').textContent=fmt(profit); $('st-s-profit').className='sp-val '+(profit>0?'positive':'negative');
  $('st-s-allin').textContent=fmt(allIn); $('st-s-fees').textContent=fmt(totalFees);
  $('st-s-monthly').textContent=fmt(totalMo)+'/mo';
}

// ── Rehab ──────────────────────────────────────
const rehabItems=[
  ['Interior – Flooring','Refinish Hardwood Floor',1.75,'/SQFT',0],
  ['Interior – Flooring','New Hardwoods 1.5"',10,'/SQFT',0],
  ['Interior – Flooring','New Hardwoods 2"',5,'/SQFT',0],
  ['Interior – Flooring','Vinyl Plank',2.35,'/SQFT',0],
  ['Interior – Flooring','Carpet',1.8,'/SQFT',0],
  ['Interior – General','Interior Paint 2 Tone',2.7,'/SQFT',0],
  ['Interior – General','Drywall Repair / 1,000 SQFT',1000,'lot',0],
  ['Interior – General','Wallpaper Removal',125,'/room',0],
  ['Interior – General','Interior Door – Hollow Slab',135,'/ea',0],
  ['Interior – General','Interior Door Hardware',25,'/ea',0],
  ['Interior – General','Bifold Door w/ Framing',400,'/ea',0],
  ['Interior – General','Interior Door – Pre-Hung',225,'/ea',0],
  ['Interior – General','Front Entry Door',500,'/ea',0],
  ['Interior – General','Exterior Door Hardware',75,'/ea',2],
  ['Interior – General','Exterior Insulated Side Door',525,'/ea',0],
  ['Interior – General','Sliding Glass Door',1000,'/ea',0],
  ['Interior – General','Trim Out (Casing, Crown, Baseboard)',3.5,'/LF',0],
  ['Interior – General','Finish Out Labor',800,'flat',1],
  ['Interior – General','Light Fixtures',120,'/100SQFT',0],
  ['Interior – General','Bedbug Spray/Heat Treat',500,'var',0],
  ['Interior – General','Termite Treatment',700,'var',0],
  ['Interior – General','Demo',1250,'var',0],
  ['Interior – General','Dumpsters',350,'/ea',1],
  ['Interior – General','Final Cleaning',350,'flat',1],
  ['Interior – General','Staging',1,'/SQFT',0],
  ['Kitchen','Hinges and Pulls',250,'kit',0],
  ['Kitchen','Cabinets – Uppers',125,'/LF',0],
  ['Kitchen','Cabinets – Lowers',140,'/LF',0],
  ['Kitchen','Cabinet Door Faces Only',65,'/door',0],
  ['Kitchen','Countertops – Laminate',40,'/LF',0],
  ['Kitchen','Countertops – Granite/Quartz',80,'/LF',0],
  ['Kitchen','Appliances',2000,'set',0],
  ['Kitchen','Kitchen Sink + Faucet',350,'/ea',0],
  ['Bathrooms','Full Bath Renovation',3500,'/ea',0],
  ['Bathrooms','Half Bath Renovation',1500,'/ea',0],
  ['Bathrooms','Tub Surround',600,'/ea',0],
  ['Bathrooms','Vanity + Mirror',450,'/ea',0],
  ['Bathrooms','Toilet',300,'/ea',0],
  ['Systems','HVAC – Full Replace',6000,'/ea',0],
  ['Systems','HVAC – Service/Tune',200,'/ea',0],
  ['Systems','Water Heater',800,'/ea',0],
  ['Systems','Electrical Panel',2500,'/ea',0],
  ['Systems','Plumbing – Rough-In',3000,'flat',0],
  ['Systems','Roof – Full Replace',8000,'flat',0],
  ['Systems','Roof – Repair',1500,'flat',0],
  ['Exterior','Exterior Paint',2.5,'/SQFT',0],
  ['Exterior','Landscaping / Cleanup',800,'flat',0],
  ['Exterior','Driveway Repair',1200,'flat',0],
  ['Exterior','Fence',2000,'flat',0],
  ['Exterior','Deck / Patio',3500,'flat',0],
  ['Exterior','Garage Door',900,'/ea',0],
  ['Exterior','Windows (per window)',400,'/ea',0],
];

function buildRehabUI() {
  const c=$('rehab-list'); c.innerHTML=''; let curSec='';
  rehabItems.forEach((item,i)=>{
    const [sec,name,cost,unit,dq]=item;
    if(sec!==curSec){
      curSec=sec;
      c.innerHTML+=`<div class="sec-div">${sec}</div>`;
      c.innerHTML+=`<div class="rehab-item hdr"><span>Item</span><span style="text-align:right">$/Unit</span><span style="text-align:center">Qty</span><span style="text-align:right">Estimate</span></div>`;
    }
    c.innerHTML+=`<div class="rehab-item" id="rh-row-${i}">
      <span>${name} <span style="color:var(--text-muted);font-size:11px">${unit}</span></span>
      <span style="color:var(--text-secondary);text-align:right">${cost>=100?'$'+cost.toLocaleString():'$'+cost}</span>
      <input type="number" min="0" value="${dq}" id="rh-qty-${i}" oninput="calcRehab()">
      <span class="rehab-est" id="rh-est-${i}">$0</span>
    </div>`;
  });
}

function calcRehab() {
  const sqft=pv('prop-sqft')||1000;
  let grand=0; const st={};
  rehabItems.forEach((item,i)=>{
    const [sec,,cost,unit]=item;
    const qty=parseFloat($(`rh-qty-${i}`)?.value)||0;
    let e=0;
    if(unit==='/SQFT') e=cost*sqft*qty;
    else if(unit==='/100SQFT') e=cost*(sqft/100)*qty;
    else e=cost*qty;
    const el=$(`rh-est-${i}`); if(el) el.textContent=e>0?fmt(e):'$0';
    grand+=e; st[sec]=(st[sec]||0)+e;
  });
  $('rh-grand-total').textContent=fmt(grand);
  $('rh-s-total').textContent=fmt(grand);
  const interior=['Interior – Flooring','Interior – General'].reduce((s,k)=>s+(st[k]||0),0);
  const sys=st['Systems']||0;
  const ext=['Kitchen','Bathrooms','Exterior'].reduce((s,k)=>s+(st[k]||0),0);
  $('rh-s-int').textContent=fmt(interior); $('rh-s-sys').textContent=fmt(sys); $('rh-s-ext').textContent=fmt(ext);
  const rr=$('rh-sqft-ref'); if(rr) rr.textContent=sqft.toLocaleString();
  return grand;
}

function pushRehabToFlip() {
  const total=calcRehab();
  const f=total.toLocaleString('en-US',{maximumFractionDigits:0});
  $('f-rehab').value=f; $('r-rehab').value=f;
  calcFlip(); calcRental();
  alert(`✓ Rehab total of ${fmt(total)} pushed to Flip & Rental tabs.`);
}

// ── Save / Load ────────────────────────────────
const SK='pa_deals_v3';

const INPUT_IDS=[
  'prop-address','prop-bbb','prop-sqft','prop-arv','prop-year','prop-hoa',
  'f-purchase','f-rehab','f-cc','f-scc','f-hold','f-months',
  'f-hml-ltv','f-hml-pts','f-hml-rate','f-int-type','f-gap-pts','f-gap-rate',
  'f-recomm','f-rcc','f-min-pct','f-min-dol',
  'r-purchase','r-rehab','r-cc','r-hold','r-hml-int','r-hml-pts','r-gap-pts','r-gap-int',
  'r-arv','r-ltv','r-rate','r-term','r-refi-pts',
  'r-rent','r-vacancy','r-mgmt','r-maint','r-capex','r-ins','r-tax','r-hoa-mo','r-other',
  'st-entry','st-back','st-soft','st-cc','st-acqfee','st-reno','st-months','st-money-cost',
  'st-bal','st-mort-rate','st-amort','st-yr-in','st-taxes','st-ins','st-hoa','st-mi',
  'st-arv','st-res-cc','st-sell-comm','st-buy-comm','st-conc',
];

const DEFAULTS={
  'prop-sqft':'1,000','prop-arv':'150,000','prop-hoa':'0',
  'f-purchase':'100,000','f-rehab':'3,650','f-cc':'3,000','f-scc':'0','f-hold':'3,000','f-months':'6',
  'f-hml-ltv':'1','f-hml-pts':'0','f-hml-rate':'10','f-gap-pts':'0','f-gap-rate':'0',
  'f-recomm':'3','f-rcc':'1','f-min-pct':'12','f-min-dol':'25,000',
  'r-purchase':'100,000','r-rehab':'3,650','r-cc':'3,000','r-hold':'3,000',
  'r-hml-int':'5,483','r-hml-pts':'0','r-gap-pts':'0','r-gap-int':'0',
  'r-arv':'150,000','r-ltv':'75','r-rate':'7','r-term':'30','r-refi-pts':'0',
  'r-rent':'1,800','r-vacancy':'8','r-mgmt':'8','r-maint':'5','r-capex':'5',
  'r-ins':'120','r-tax':'120','r-hoa-mo':'0','r-other':'0',
  'st-entry':'8,000','st-back':'0','st-soft':'1,800','st-cc':'500','st-acqfee':'2,500','st-reno':'3,650',
  'st-months':'6','st-money-cost':'12','st-bal':'53,000','st-mort-rate':'5','st-amort':'30',
  'st-yr-in':'0','st-taxes':'47.17','st-ins':'93.83','st-hoa':'0','st-mi':'0',
  'st-arv':'150,000','st-res-cc':'3,750','st-sell-comm':'1','st-buy-comm':'3','st-conc':'1',
};

function captureState() {
  const s={inputs:{},rehab:{}};
  INPUT_IDS.forEach(id=>{ const e=$(id); if(e) s.inputs[id]=e.value; });
  rehabItems.forEach((_,i)=>{ const e=$(`rh-qty-${i}`); if(e) s.rehab[i]=e.value; });
  return s;
}

function applyState(s) {
  if(s.inputs) INPUT_IDS.forEach(id=>{ const e=$(id); if(e&&s.inputs[id]!==undefined) e.value=s.inputs[id]; });
  if(s.rehab) Object.entries(s.rehab).forEach(([i,v])=>{ const e=$(`rh-qty-${i}`); if(e) e.value=v; });
  updateHeader(); syncAll(); calcSubTo(); calcRehab();
}

function getDeals() { try{ return JSON.parse(localStorage.getItem(SK))||[]; } catch{ return []; } }
function setDeals(d) { localStorage.setItem(SK,JSON.stringify(d)); }

let activeDeal=null;

function renderDeals() {
  const deals=getDeals(), c=$('saved-deals-list');
  c.innerHTML='';
  if(!deals.length){ c.innerHTML='<span style="font-size:12px;color:var(--text-muted)">No saved deals yet</span>'; return; }
  deals.forEach((d,i)=>{
    const isAct=d.name===activeDeal;
    const btn=document.createElement('button');
    btn.className='saved-deal-btn'+(isAct?' active':'');
    btn.innerHTML=`<span>${d.name}</span><span class="del-x" data-i="${i}" title="Delete">×</span>`;
    btn.addEventListener('click',e=>{
      if(e.target.classList.contains('del-x')) deleteDeal(parseInt(e.target.dataset.i));
      else loadDeal(i);
    });
    c.appendChild(btn);
  });
}

function loadDeal(i) {
  const d=getDeals()[i]; if(!d) return;
  activeDeal=d.name; applyState(d.state); renderDeals();
}

function deleteDeal(i) {
  const deals=getDeals(), name=deals[i]?.name;
  if(!confirm(`Delete "${name}"?`)) return;
  deals.splice(i,1); setDeals(deals);
  if(activeDeal===name) activeDeal=null;
  renderDeals();
}

function openSaveModal() {
  const addr=$('prop-address').value.trim();
  $('save-name-input').value=addr?addr+' — Analysis':'';
  $('save-modal').classList.add('open');
  setTimeout(()=>$('save-name-input').focus(),50);
}

function closeModal() { $('save-modal').classList.remove('open'); }

function confirmSave() {
  const name=$('save-name-input').value.trim();
  if(!name){ $('save-name-input').focus(); return; }
  const deals=getDeals(), idx=deals.findIndex(d=>d.name===name);
  const entry={name,state:captureState(),savedAt:Date.now()};
  if(idx>=0) deals[idx]=entry; else deals.push(entry);
  setDeals(deals); activeDeal=name; renderDeals(); closeModal();
}

$('save-name-input').addEventListener('keydown',e=>{ if(e.key==='Enter') confirmSave(); if(e.key==='Escape') closeModal(); });
$('save-modal').addEventListener('click',e=>{ if(e.target===$('save-modal')) closeModal(); });

function newDeal() {
  if(!confirm('Start a new blank deal? Unsaved changes will be lost.')) return;
  activeDeal=null;
  INPUT_IDS.forEach(id=>{ const e=$(id); if(e) e.value=DEFAULTS[id]||''; });
  rehabItems.forEach((_,i)=>{ const e=$(`rh-qty-${i}`); if(e) e.value=0; });
  updateHeader(); syncAll(); calcSubTo(); calcRehab(); renderDeals();
}

// ── Init ───────────────────────────────────────
window.onload=()=>{
  attachCommas();
  buildRehabUI();
  calcFlip(); calcRental(); calcSubTo(); calcRehab();
  updateHeader(); syncAll();
  renderDeals();
  injectTooltips();
  // Apply saved settings defaults
  const s = loadSettings();
  if (s.defHmlRate) { $('f-hml-rate').value = s.defHmlRate; }
  if (s.defMonths)  { $('f-months').value   = s.defMonths; }
  if (s.defLtv)     { $('r-ltv').value      = s.defLtv; }
  if (s.defMinPct)  { $('f-min-pct').value  = s.defMinPct; }
  updateLookupBtn(!!s.rcKey);
  calcFlip(); calcRental();
};
</script>
</body>
</html>
