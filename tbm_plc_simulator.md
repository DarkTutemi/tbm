<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mô Phỏng & Bản Vẽ Điện Chuẩn TBM S7-300 | MTS Perforator 2018221</title>
  <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;800&family=Outfit:wght@300;400;600;700;900&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-dark: #0b0f19;
      --bg-card: #131b2e;
      --bg-card-hover: #18233d;
      --border-color: #1e2c4f;
      --accent-blue: #00d2ff;
      --accent-cyan: #00f2fe;
      --accent-green: #00e676;
      --accent-orange: #ff9100;
      --accent-red: #ff3d71;
      --accent-purple: #9d4edd;
      --text-main: #f0f4fc;
      --text-muted: #8a9fc4;
      --glass-bg: rgba(19, 27, 46, 0.95);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Outfit', sans-serif;
      background-color: var(--bg-dark);
      color: var(--text-main);
      min-height: 100vh;
      overflow-x: hidden;
      background-image: 
        radial-gradient(circle at 10% 20%, rgba(0, 210, 255, 0.05) 0%, transparent 40%),
        radial-gradient(circle at 90% 80%, rgba(157, 78, 221, 0.05) 0%, transparent 40%);
    }

    /* Header */
    header {
      padding: 12px 28px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: var(--glass-bg);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border-color);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .logo-area {
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .badge-plc {
      background: linear-gradient(135deg, #0072ff, #00d2ff);
      color: #fff;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
      padding: 4px 10px;
      border-radius: 6px;
      font-size: 0.85rem;
      letter-spacing: 1px;
      box-shadow: 0 0 15px rgba(0, 210, 255, 0.4);
    }

    h1 {
      font-size: 1.15rem;
      font-weight: 700;
      letter-spacing: -0.5px;
    }

    /* Tab Navigation */
    .tab-nav {
      display: flex;
      gap: 8px;
      background: rgba(255, 255, 255, 0.04);
      padding: 4px;
      border-radius: 10px;
      border: 1px solid var(--border-color);
    }

    .tab-btn {
      padding: 8px 16px;
      border-radius: 8px;
      border: none;
      background: transparent;
      color: var(--text-muted);
      font-family: 'Outfit', sans-serif;
      font-weight: 600;
      font-size: 0.85rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: all 0.2s ease;
    }

    .tab-btn:hover {
      color: var(--text-main);
      background: rgba(255, 255, 255, 0.05);
    }

    .tab-btn.active {
      background: linear-gradient(135deg, #0072ff, #00d2ff);
      color: #fff;
      box-shadow: 0 0 15px rgba(0, 210, 255, 0.3);
    }

    .status-bar {
      display: flex;
      gap: 14px;
      font-size: 0.82rem;
      font-family: 'JetBrains Mono', monospace;
    }

    .status-pill {
      display: flex;
      align-items: center;
      gap: 8px;
      background: rgba(255, 255, 255, 0.05);
      padding: 5px 12px;
      border-radius: 20px;
      border: 1px solid var(--border-color);
    }

    .led {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background-color: var(--accent-green);
      box-shadow: 0 0 8px var(--accent-green);
      animation: pulse 2s infinite;
    }

    .led.danger { background-color: var(--accent-red); box-shadow: 0 0 8px var(--accent-red); }

    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(0.9); }
    }

    /* Tab Content Views */
    .tab-content {
      display: none;
      padding: 20px 28px;
      max-width: 1800px;
      margin: 0 auto;
    }

    .tab-content.active {
      display: block;
    }

    /* Layout Grid for Simulator */
    .dashboard-container {
      display: grid;
      grid-template-columns: 1fr 380px;
      gap: 20px;
    }

    /* Section Cards */
    .card {
      background: var(--bg-card);
      border: 1px solid var(--border-color);
      border-radius: 14px;
      padding: 20px;
      box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
      position: relative;
      overflow: hidden;
      margin-bottom: 20px;
    }

    .card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--accent-blue), transparent);
      opacity: 0.4;
    }

    .card-title {
      font-size: 1rem;
      font-weight: 700;
      color: var(--text-main);
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
      padding-bottom: 10px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.06);
    }

    .card-title .tag {
      font-size: 0.72rem;
      font-family: 'JetBrains Mono', monospace;
      padding: 3px 8px;
      background: rgba(0, 210, 255, 0.1);
      border: 1px solid rgba(0, 210, 255, 0.3);
      color: var(--accent-blue);
      border-radius: 4px;
    }

    /* Machine Graphic Stage */
    .tbm-stage-container {
      height: 380px;
      background: #080c14;
      border-radius: 10px;
      border: 1px solid #1a2542;
      position: relative;
      overflow: hidden;
    }

    canvas#tbmCanvas {
      width: 100%;
      height: 100%;
      display: block;
    }

    /* Metrics Grid */
    .metrics-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin-top: 18px;
    }

    .metric-box {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-color);
      border-radius: 10px;
      padding: 14px;
      text-align: center;
      transition: all 0.3s ease;
    }

    .metric-box:hover {
      background: var(--bg-card-hover);
      border-color: rgba(0, 210, 255, 0.4);
      transform: translateY(-2px);
    }

    .metric-label {
      font-size: 0.75rem;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 6px;
    }

    .metric-value {
      font-family: 'JetBrains Mono', monospace;
      font-size: 1.35rem;
      font-weight: 800;
      color: var(--accent-cyan);
    }

    .metric-unit {
      font-size: 0.75rem;
      font-weight: 400;
      color: var(--text-muted);
      margin-left: 4px;
    }

    .metric-tag {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.65rem;
      color: #627d98;
      margin-top: 4px;
      display: block;
    }

    /* Controls */
    .control-group {
      margin-bottom: 18px;
    }

    .control-label {
      display: flex;
      justify-content: space-between;
      font-size: 0.8rem;
      margin-bottom: 8px;
      color: var(--text-muted);
    }

    .control-label span.val {
      font-family: 'JetBrains Mono', monospace;
      color: var(--accent-cyan);
      font-weight: 700;
    }

    input[type="range"] {
      width: 100%;
      height: 6px;
      border-radius: 5px;
      background: #1c2744;
      outline: none;
      -webkit-appearance: none;
    }

    input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: var(--accent-blue);
      cursor: pointer;
      box-shadow: 0 0 10px var(--accent-blue);
      transition: transform 0.1s;
    }

    .btn-group {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-bottom: 12px;
    }

    .btn {
      padding: 10px 14px;
      border: 1px solid var(--border-color);
      background: #17223b;
      color: var(--text-main);
      font-family: 'Outfit', sans-serif;
      font-weight: 600;
      font-size: 0.85rem;
      border-radius: 8px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: all 0.2s;
    }

    .btn:hover {
      background: #202f52;
      border-color: var(--accent-blue);
      color: #fff;
    }

    .btn.active {
      background: linear-gradient(135deg, #0072ff, #00c6ff);
      border-color: #00f2fe;
      color: #fff;
      box-shadow: 0 0 15px rgba(0, 210, 255, 0.4);
    }

    .btn.danger {
      background: rgba(255, 61, 113, 0.15);
      border-color: rgba(255, 61, 113, 0.4);
      color: var(--accent-red);
    }

    .btn.danger:hover, .btn.danger.active {
      background: var(--accent-red);
      color: #fff;
      box-shadow: 0 0 15px rgba(255, 61, 113, 0.5);
    }

    /* Subsystems Grid */
    .subsystems-row {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      gap: 16px;
      margin-top: 20px;
    }

    .laser-target-box {
      width: 100%;
      height: 150px;
      background: #04070d;
      border-radius: 8px;
      border: 1px solid #1d2d50;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    .laser-crosshair-h {
      position: absolute;
      width: 100%;
      height: 1px;
      background: rgba(0, 210, 255, 0.3);
    }

    .laser-crosshair-v {
      position: absolute;
      height: 100%;
      width: 1px;
      background: rgba(0, 210, 255, 0.3);
    }

    .laser-ring {
      position: absolute;
      border: 1px dashed rgba(0, 210, 255, 0.25);
      border-radius: 50%;
    }

    .laser-dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: #ff0055;
      box-shadow: 0 0 12px #ff0055;
      position: absolute;
      transform: translate(-50%, -50%);
      transition: all 0.1s linear;
    }

    .plc-table {
      width: 100%;
      border-collapse: collapse;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.75rem;
      margin-top: 10px;
    }

    .plc-table th, .plc-table td {
      padding: 8px 12px;
      text-align: left;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .plc-table th {
      color: var(--text-muted);
      font-weight: 600;
      background: rgba(255, 255, 255, 0.03);
    }

    .plc-table td.val {
      color: var(--accent-cyan);
      font-weight: 600;
    }

    .jacks-bar-container {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .jack-item {
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 0.75rem;
      font-family: 'JetBrains Mono', monospace;
    }

    .jack-name {
      width: 90px;
      color: var(--text-muted);
    }

    .jack-bar-bg {
      flex: 1;
      height: 8px;
      background: #1a2542;
      border-radius: 4px;
      overflow: hidden;
    }

    .jack-bar-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #0072ff, #00d2ff);
      transition: width 0.1s ease;
    }

    .jack-val {
      width: 60px;
      text-align: right;
      color: var(--accent-cyan);
    }

    /* Diagram Section Styling */
    .diagram-subnav {
      display: flex;
      gap: 10px;
      margin-bottom: 24px;
      flex-wrap: wrap;
      position: sticky;
      top: 65px;
      z-index: 90;
      background: var(--glass-bg);
      padding: 10px 0;
      backdrop-filter: blur(10px);
    }

    .diag-sub-btn {
      padding: 9px 18px;
      background: #131b2e;
      border: 1px solid var(--border-color);
      border-radius: 8px;
      color: var(--text-muted);
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
    }

    .diag-sub-btn:hover {
      color: #fff;
      background: #1c2844;
      border-color: var(--accent-blue);
    }

    .diag-sub-btn.active {
      background: rgba(0, 210, 255, 0.15);
      border-color: var(--accent-blue);
      color: var(--accent-cyan);
      box-shadow: 0 0 12px rgba(0, 210, 255, 0.25);
    }

    .diagram-section {
      background: var(--bg-card);
      border: 1px solid var(--border-color);
      border-radius: 14px;
      padding: 26px;
      margin-bottom: 30px;
      box-shadow: 0 10px 35px rgba(0, 0, 0, 0.4);
    }

    .diag-header {
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
      padding-bottom: 14px;
      margin-bottom: 22px;
    }

    .diag-header h2 {
      font-size: 1.25rem;
      font-weight: 700;
      color: var(--accent-cyan);
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .diag-header p {
      font-size: 0.88rem;
      color: var(--text-muted);
      margin-top: 6px;
    }

    .arch-layer-container {
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .arch-layer {
      border-radius: 12px;
      padding: 18px 22px;
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-color);
    }

    .arch-layer-title {
      font-size: 0.8rem;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .arch-layer.hmi { border-color: rgba(157, 78, 221, 0.4); background: rgba(157, 78, 221, 0.04); }
    .arch-layer.hmi .arch-layer-title { color: #d084fc; }

    .arch-layer.obs { border-color: rgba(255, 61, 113, 0.4); background: rgba(255, 61, 113, 0.04); }
    .arch-layer.obs .arch-layer-title { color: #ff7096; }

    .arch-layer.fcs { border-color: rgba(0, 210, 255, 0.4); background: rgba(0, 210, 255, 0.04); }
    .arch-layer.fcs .arch-layer-title { color: #00f2fe; }

    .arch-layer.dbs { border-color: rgba(255, 145, 0, 0.4); background: rgba(255, 145, 0, 0.04); }
    .arch-layer.dbs .arch-layer-title { color: #ffb74d; }

    .arch-layer.hw { border-color: rgba(0, 230, 118, 0.4); background: rgba(0, 230, 118, 0.04); }
    .arch-layer.hw .arch-layer-title { color: #69f0ae; }

    .arch-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 12px;
    }

    .arch-node {
      background: #0d1424;
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 12px 14px;
      transition: all 0.2s;
    }

    .arch-node:hover {
      border-color: var(--accent-blue);
      transform: translateY(-2px);
      box-shadow: 0 4px 15px rgba(0, 210, 255, 0.15);
    }

    .arch-node .node-id {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
      font-size: 0.85rem;
      color: var(--accent-cyan);
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
    }

    .arch-node .node-id span.badge {
      font-size: 0.65rem;
      padding: 2px 6px;
      border-radius: 4px;
      background: rgba(255, 255, 255, 0.08);
      color: var(--text-muted);
    }

    .arch-node .node-name {
      font-weight: 600;
      font-size: 0.82rem;
      color: var(--text-main);
      margin-bottom: 4px;
    }

    .arch-node .node-desc {
      font-size: 0.72rem;
      color: var(--text-muted);
      line-height: 1.35;
    }

    /* Pipeline 14 Networks */
    .pipe-timeline {
      display: flex;
      flex-direction: column;
      gap: 12px;
      position: relative;
    }

    .pipe-timeline::before {
      content: '';
      position: absolute;
      top: 20px;
      bottom: 20px;
      left: 28px;
      width: 2px;
      background: linear-gradient(180deg, var(--accent-blue), var(--accent-purple), var(--accent-green));
    }

    .pipe-step {
      display: flex;
      gap: 20px;
      position: relative;
      z-index: 2;
    }

    .pipe-num {
      width: 56px;
      height: 56px;
      border-radius: 50%;
      background: #111a30;
      border: 2px solid var(--accent-blue);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
      font-size: 0.95rem;
      flex-shrink: 0;
      box-shadow: 0 0 15px rgba(0, 210, 255, 0.25);
    }

    .pipe-card {
      flex: 1;
      background: #0d1424;
      border: 1px solid var(--border-color);
      border-radius: 10px;
      padding: 14px 18px;
    }

    .pipe-title {
      font-size: 0.95rem;
      font-weight: 700;
      color: var(--text-main);
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
    }

    .pipe-tags {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
    }

    .pipe-tag {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      font-weight: 700;
      padding: 2px 7px;
      border-radius: 4px;
      background: rgba(0, 210, 255, 0.12);
      border: 1px solid rgba(0, 210, 255, 0.3);
      color: var(--accent-cyan);
    }

    .pipe-desc {
      font-size: 0.8rem;
      color: var(--text-muted);
      line-height: 1.4;
    }

    /* Flow Grid */
    .flow-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 16px;
    }

    .flow-col {
      background: #0d1424;
      border: 1px solid var(--border-color);
      border-radius: 10px;
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .flow-col-title {
      font-size: 0.82rem;
      font-weight: 800;
      color: var(--accent-cyan);
      text-transform: uppercase;
      letter-spacing: 0.5px;
      padding-bottom: 8px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.06);
    }

    .flow-item {
      background: #151f38;
      border: 1px solid rgba(255, 255, 255, 0.05);
      border-radius: 6px;
      padding: 10px 12px;
      font-size: 0.78rem;
    }

    .flow-item strong {
      display: block;
      color: var(--text-main);
      margin-bottom: 2px;
      font-family: 'JetBrains Mono', monospace;
    }

    .flow-item span {
      color: var(--text-muted);
      font-size: 0.72rem;
    }

    /* State Grid */
    .state-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
    }

    .state-card {
      background: #0d1424;
      border: 1px solid var(--border-color);
      border-radius: 10px;
      padding: 16px;
    }

    .state-card.active-state { border-color: var(--accent-green); background: rgba(0, 230, 118, 0.03); }
    .state-card.danger-state { border-color: var(--accent-red); background: rgba(255, 61, 113, 0.03); }

    .state-title {
      font-size: 0.9rem;
      font-weight: 700;
      color: var(--text-main);
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }

    .state-body {
      font-size: 0.78rem;
      color: var(--text-muted);
      line-height: 1.45;
    }

    .state-list {
      margin-top: 8px;
      padding-left: 16px;
    }

    .state-list li {
      margin-bottom: 4px;
    }

    /* Search & Filter Bar */
    .comp-toolbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 14px;
      margin-bottom: 18px;
      flex-wrap: wrap;
    }

    .comp-filter-bar {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .comp-filter-btn {
      padding: 7px 14px;
      border-radius: 6px;
      border: 1px solid var(--border-color);
      background: #131b2e;
      color: var(--text-muted);
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
    }

    .comp-filter-btn:hover {
      background: #1c2844;
      color: #fff;
    }

    .comp-filter-btn.active {
      background: rgba(0, 210, 255, 0.15);
      border-color: var(--accent-blue);
      color: var(--accent-cyan);
    }

    .search-box {
      background: #0d1424;
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 6px 12px;
      color: #fff;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.82rem;
      outline: none;
      min-width: 250px;
      transition: border-color 0.2s;
    }

    .search-box:focus {
      border-color: var(--accent-cyan);
    }

    .badge-sensor { background: rgba(0, 210, 255, 0.15); color: var(--accent-cyan); border: 1px solid rgba(0, 210, 255, 0.3); padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; }
    .badge-actuator { background: rgba(255, 145, 0, 0.15); color: var(--accent-orange); border: 1px solid rgba(255, 145, 0, 0.3); padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; }
    .badge-drive { background: rgba(157, 78, 221, 0.15); color: #d084fc; border: 1px solid rgba(157, 78, 221, 0.3); padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; }
    .badge-hw { background: rgba(0, 230, 118, 0.15); color: var(--accent-green); border: 1px solid rgba(0, 230, 118, 0.3); padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; }
    .badge-safety { background: rgba(255, 61, 113, 0.15); color: var(--accent-red); border: 1px solid rgba(255, 61, 113, 0.3); padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; }
  
    /* --- STYLES FOR MTS PERFORATOR BLUEPRINTS INTEGRATION --- */
    .mts-badge {
      background: linear-gradient(135deg, #ff6b6b, #ee5253);
      color: #fff;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      padding: 3px 8px;
      border-radius: 4px;
      font-size: 0.72rem;
      letter-spacing: 0.5px;
    }

    .subsystem-badge {
      display: inline-block;
      padding: 2px 7px;
      border-radius: 4px;
      font-size: 0.7rem;
      font-weight: 700;
      font-family: 'JetBrains Mono', monospace;
      text-transform: uppercase;
    }
    .subsystem-bk { background: rgba(0, 210, 255, 0.15); color: #00d2ff; border: 1px solid rgba(0, 210, 255, 0.4); }
    .subsystem-ppm { background: rgba(255, 145, 0, 0.15); color: #ff9100; border: 1px solid rgba(255, 145, 0, 0.4); }
    .subsystem-pph { background: rgba(0, 230, 118, 0.15); color: #00e676; border: 1px solid rgba(0, 230, 118, 0.4); }
    .subsystem-cc { background: rgba(157, 78, 221, 0.15); color: #c77dff; border: 1px solid rgba(157, 78, 221, 0.4); }
    .subsystem-ebox { background: rgba(255, 61, 113, 0.15); color: #ff3d71; border: 1px solid rgba(255, 61, 113, 0.4); }
    .subsystem-schott { background: rgba(0, 242, 254, 0.15); color: #00f2fe; border: 1px solid rgba(0, 242, 254, 0.4); }
    .subsystem-hz { background: rgba(255, 221, 89, 0.15); color: #ffdd59; border: 1px solid rgba(255, 221, 89, 0.4); }

    /* Console Desk Grid */
    .console-container {
      background: #0d1424;
      border: 1px solid #1e2c4f;
      border-radius: 12px;
      padding: 18px;
      margin-top: 18px;
    }
    .console-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      padding-bottom: 8px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
    }
    .console-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 16px;
    }
    .console-panel {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid rgba(255, 255, 255, 0.06);
      border-radius: 8px;
      padding: 12px;
    }
    .console-panel-title {
      font-size: 0.82rem;
      font-weight: 700;
      color: var(--accent-cyan);
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .switch-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 8px;
      padding: 6px 10px;
      background: rgba(0, 0, 0, 0.25);
      border-radius: 6px;
      border: 1px solid rgba(255, 255, 255, 0.03);
    }
    .switch-label {
      font-size: 0.76rem;
      color: var(--text-main);
      display: flex;
      flex-direction: column;
    }
    .switch-bmk {
      font-size: 0.65rem;
      font-family: 'JetBrains Mono', monospace;
      color: var(--accent-orange);
    }
    .ctrl-btn-group {
      display: flex;
      gap: 4px;
    }
    .ctrl-btn {
      padding: 5px 10px;
      font-size: 0.72rem;
      font-weight: 700;
      font-family: 'JetBrains Mono', monospace;
      border-radius: 5px;
      border: 1px solid var(--border-color);
      background: #18233d;
      color: var(--text-muted);
      cursor: pointer;
      transition: all 0.2s ease;
    }
    .ctrl-btn:hover {
      background: #223358;
      color: #fff;
    }
    .ctrl-btn.active-on {
      background: var(--accent-green);
      color: #000;
      border-color: var(--accent-green);
      box-shadow: 0 0 10px rgba(0, 230, 118, 0.5);
    }
    .ctrl-btn.active-off {
      background: var(--accent-red);
      color: #fff;
      border-color: var(--accent-red);
      box-shadow: 0 0 10px rgba(255, 61, 113, 0.5);
    }
    .ctrl-poti-box {
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .ctrl-poti-val {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      font-size: 0.75rem;
      color: var(--accent-cyan);
      min-width: 45px;
      text-align: right;
    }

    /* Motors Grid */
    .motors-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
      gap: 12px;
      margin-top: 14px;
    }
    .motor-card {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 12px;
      position: relative;
    }
    .motor-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
    }
    .motor-bmk {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      font-size: 0.8rem;
      color: var(--accent-blue);
    }
    .motor-power-badge {
      font-size: 0.68rem;
      padding: 2px 6px;
      background: rgba(255, 145, 0, 0.15);
      color: var(--accent-orange);
      border-radius: 4px;
      font-weight: 700;
      font-family: 'JetBrains Mono', monospace;
    }
    .motor-specs {
      font-size: 0.72rem;
      color: var(--text-muted);
      margin-bottom: 8px;
      line-height: 1.3;
    }
    .motor-status-led {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.72rem;
      font-family: 'JetBrains Mono', monospace;
    }

    /* Cable & Terminal Badges */
    .cable-badge-power { background: rgba(255, 145, 0, 0.15); color: #ff9100; border: 1px solid rgba(255, 145, 0, 0.4); }
    .cable-badge-control { background: rgba(0, 210, 255, 0.15); color: #00d2ff; border: 1px solid rgba(0, 210, 255, 0.4); }
    .cable-badge-bus { background: rgba(157, 78, 221, 0.15); color: #c77dff; border: 1px solid rgba(157, 78, 221, 0.4); }
    .cable-badge-safety { background: rgba(255, 61, 113, 0.15); color: #ff3d71; border: 1px solid rgba(255, 61, 113, 0.4); }
    .cable-badge-sensor { background: rgba(0, 230, 118, 0.15); color: #00e676; border: 1px solid rgba(0, 230, 118, 0.4); }

    /* Wire Colors Table */
    .color-chip-box {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      font-size: 0.75rem;
      padding: 4px 8px;
      border-radius: 4px;
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(255, 255, 255, 0.08);
      margin: 3px;
    }
    .color-chip {
      width: 14px;
      height: 14px;
      border-radius: 3px;
      display: inline-block;
      border: 1px solid rgba(255, 255, 255, 0.3);
    }

  
    /* ========================================================
       FIGURE 3.5 CONTROL PANEL & VISAM SCREEN STYLES
       ======================================================== */
    /* VisAM Screen Styling */
    .visam-container {
      background: #080d1a;
      border: 2px solid #2a3c66;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 20px;
      box-shadow: 0 0 30px rgba(0, 210, 255, 0.15), inset 0 0 20px rgba(0, 0, 0, 0.8);
      position: relative;
    }
    .visam-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: linear-gradient(90deg, #10192f, #1b284a);
      padding: 8px 14px;
      border-radius: 6px;
      border: 1px solid #2a3d68;
      margin-bottom: 14px;
      font-family: 'JetBrains Mono', monospace;
    }
    .visam-title {
      font-size: 0.9rem;
      font-weight: 800;
      color: #00f2fe;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .visam-pills {
      display: flex;
      gap: 8px;
    }
    .visam-pill {
      font-size: 0.7rem;
      padding: 3px 8px;
      border-radius: 4px;
      background: rgba(0, 0, 0, 0.5);
      border: 1px solid #2c3f6b;
      color: #9ab4db;
    }
    .visam-grid {
      display: grid;
      grid-template-columns: 1.2fr 1fr 1fr;
      gap: 14px;
    }
    .visam-card {
      background: rgba(14, 22, 42, 0.8);
      border: 1px solid #203157;
      border-radius: 8px;
      padding: 12px;
    }
    .visam-card-title {
      font-size: 0.75rem;
      font-weight: 700;
      font-family: 'JetBrains Mono', monospace;
      color: #00d2ff;
      margin-bottom: 10px;
      padding-bottom: 4px;
      border-bottom: 1px solid rgba(0, 210, 255, 0.2);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .visam-valve-box {
      display: flex;
      gap: 12px;
      margin-bottom: 12px;
    }
    .visam-valve {
      flex: 1;
      background: rgba(0, 0, 0, 0.4);
      border: 1px solid #25365e;
      border-radius: 6px;
      padding: 8px;
      text-align: center;
      transition: all 0.3s ease;
    }
    .visam-valve.open {
      border-color: #00e676;
      background: rgba(0, 230, 118, 0.08);
      box-shadow: 0 0 10px rgba(0, 230, 118, 0.2);
    }
    .visam-valve.closed {
      border-color: #ff3d71;
      background: rgba(255, 61, 113, 0.08);
    }
    .visam-valve-name {
      font-size: 0.72rem;
      font-weight: 700;
      color: var(--text-main);
      margin-bottom: 4px;
    }
    .visam-valve-state {
      font-size: 0.8rem;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
    }
    .visam-valve.open .visam-valve-state { color: #00e676; }
    .visam-valve.closed .visam-valve-state { color: #ff3d71; }

    .visam-readout-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
      padding: 4px 8px;
      background: rgba(0, 0, 0, 0.3);
      border-radius: 4px;
      font-size: 0.74rem;
    }
    .visam-readout-label {
      color: var(--text-muted);
    }
    .visam-readout-val {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      color: #00f2fe;
    }

    /* PHYSICAL FIGURE 3.5 CONTROL PANEL STYLING */
    .fig35-panel-container {
      background: linear-gradient(180deg, #182030, #111724);
      border: 3px solid #2c3e66;
      border-radius: 14px;
      padding: 22px;
      box-shadow: 0 12px 40px rgba(0, 0, 0, 0.7), inset 0 2px 4px rgba(255, 255, 255, 0.1);
      position: relative;
      margin-top: 20px;
    }
    .fig35-plate-title {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
      padding-bottom: 10px;
      border-bottom: 2px solid rgba(255, 255, 255, 0.1);
    }
    .fig35-plate-heading {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 900;
      font-size: 1.1rem;
      color: #f1f5f9;
      letter-spacing: 1px;
      text-transform: uppercase;
    }
    .fig35-plate-sub {
      font-size: 0.75rem;
      color: var(--accent-orange);
      font-family: 'JetBrains Mono', monospace;
    }

    /* 5 Physical Rows Container */
    .fig35-rows-wrap {
      display: flex;
      flex-direction: column;
      gap: 16px;
    }
    .fig35-row {
      display: flex;
      gap: 10px;
      background: rgba(10, 15, 26, 0.6);
      border: 1px solid rgba(255, 255, 255, 0.05);
      border-radius: 10px;
      padding: 12px 10px;
      align-items: flex-end;
      overflow-x: auto;
    }
    .fig35-row-badge {
      writing-mode: vertical-lr;
      transform: rotate(180deg);
      font-size: 0.65rem;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      color: var(--text-muted);
      letter-spacing: 1px;
      padding: 4px;
      text-align: center;
      align-self: center;
      background: rgba(0, 0, 0, 0.3);
      border-radius: 4px;
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    /* Individual Item Module */
    .fig35-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      min-width: 92px;
      max-width: 110px;
      flex: 1;
      text-align: center;
      background: rgba(255, 255, 255, 0.015);
      border: 1px solid rgba(255, 255, 255, 0.04);
      border-radius: 8px;
      padding: 8px 4px;
      position: relative;
      transition: all 0.2s ease;
    }
    .fig35-item:hover {
      background: rgba(255, 255, 255, 0.04);
      border-color: rgba(0, 210, 255, 0.3);
      transform: translateY(-2px);
    }
    .fig35-item-num {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
      font-size: 0.72rem;
      color: #00d2ff;
      background: rgba(0, 210, 255, 0.1);
      padding: 1px 6px;
      border-radius: 3px;
      border: 1px solid rgba(0, 210, 255, 0.25);
      margin-bottom: 6px;
    }
    .fig35-item-label {
      font-size: 0.65rem;
      color: var(--text-muted);
      line-height: 1.15;
      height: 28px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 8px;
      font-weight: 600;
    }
    .fig35-item-bmk {
      font-size: 0.6rem;
      font-family: 'JetBrains Mono', monospace;
      color: var(--accent-orange);
      margin-top: 6px;
    }

    /* Actuator Elements: Buttons, Dials, Switches, Plugs */
    /* 1. Illuminated Push Button */
    .btn-illuminated {
      width: 44px;
      height: 44px;
      border-radius: 50%;
      border: 3px solid #334466;
      background: radial-gradient(circle at 35% 35%, #2a3a5a, #111a2d);
      cursor: pointer;
      position: relative;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5), inset 0 2px 3px rgba(255, 255, 255, 0.2);
      transition: all 0.2s ease;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .btn-illuminated:hover {
      border-color: #00d2ff;
      transform: scale(1.05);
    }
    .btn-illuminated:active {
      transform: scale(0.95);
    }
    .btn-illuminated::after {
      content: '';
      width: 22px;
      height: 22px;
      border-radius: 50%;
      background: #1e2a42;
      box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.6);
      transition: all 0.3s ease;
    }
    /* Button Active Glows */
    .btn-illuminated.active-green {
      border-color: #00e676;
      box-shadow: 0 0 15px rgba(0, 230, 118, 0.6);
    }
    .btn-illuminated.active-green::after {
      background: #00e676;
      box-shadow: 0 0 12px #00e676, inset 0 1px 2px #fff;
    }
    .btn-illuminated.active-red {
      border-color: #ff3d71;
      box-shadow: 0 0 15px rgba(255, 61, 113, 0.6);
    }
    .btn-illuminated.active-red::after {
      background: #ff3d71;
      box-shadow: 0 0 12px #ff3d71, inset 0 1px 2px #fff;
    }
    .btn-illuminated.active-cyan {
      border-color: #00f2fe;
      box-shadow: 0 0 15px rgba(0, 242, 254, 0.6);
    }
    .btn-illuminated.active-cyan::after {
      background: #00f2fe;
      box-shadow: 0 0 12px #00f2fe, inset 0 1px 2px #fff;
    }
    .btn-illuminated.active-yellow {
      border-color: #ffdd59;
      box-shadow: 0 0 15px rgba(255, 221, 89, 0.6);
    }
    .btn-illuminated.active-yellow::after {
      background: #ffdd59;
      box-shadow: 0 0 12px #ffdd59, inset 0 1px 2px #fff;
    }

    /* 2. Rotary Dial / Potentiometer */
    .dial-wrap {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
    }
    .dial-knob {
      width: 44px;
      height: 44px;
      border-radius: 50%;
      background: radial-gradient(circle at 35% 35%, #3e4e73, #151d30);
      border: 3px solid #2d3d63;
      position: relative;
      cursor: pointer;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.6), inset 0 2px 2px rgba(255, 255, 255, 0.3);
      transition: transform 0.1s linear;
    }
    .dial-pointer {
      position: absolute;
      width: 3px;
      height: 14px;
      background: #00f2fe;
      top: 4px;
      left: 50%;
      transform: translateX(-50%);
      border-radius: 2px;
      box-shadow: 0 0 6px #00f2fe;
    }
    .dial-val {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.68rem;
      font-weight: 800;
      color: #00f2fe;
      background: rgba(0, 0, 0, 0.4);
      padding: 1px 5px;
      border-radius: 3px;
      border: 1px solid rgba(0, 242, 254, 0.2);
    }
    .dial-slider {
      width: 58px;
      height: 4px;
      margin-top: 2px;
      cursor: pointer;
    }

    /* 3. Rocker / Toggle Switch */
    .rocker-switch {
      width: 26px;
      height: 46px;
      background: #0d1322;
      border: 2px solid #2e3e66;
      border-radius: 6px;
      position: relative;
      cursor: pointer;
      box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.8);
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 3px;
    }
    .rocker-lever {
      width: 100%;
      height: 20px;
      background: linear-gradient(180deg, #3a4a70, #1e2842);
      border-radius: 4px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
      transition: all 0.2s ease;
    }
    .rocker-switch.state-up .rocker-lever {
      transform: translateY(0);
      background: linear-gradient(180deg, #00d2ff, #0072ff);
      box-shadow: 0 0 8px rgba(0, 210, 255, 0.5);
    }
    .rocker-switch.state-mid .rocker-lever {
      transform: translateY(10px);
      background: linear-gradient(180deg, #475569, #334155);
    }
    .rocker-switch.state-down .rocker-lever {
      transform: translateY(20px);
      background: linear-gradient(180deg, #ff9100, #d97706);
      box-shadow: 0 0 8px rgba(255, 145, 0, 0.5);
    }

    /* 4. Dummy Plug */
    .dummy-plug {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: radial-gradient(circle at 40% 40%, #161e30, #080c14);
      border: 2px solid #1c263d;
      position: relative;
      box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.8);
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .dummy-plug::after {
      content: '00';
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.65rem;
      color: #334466;
      font-weight: 700;
    }

    /* Signal Flow Loop Card */
    .flow-loop-container {
      background: #0a0f1d;
      border: 1px solid #1c2a4a;
      border-radius: 12px;
      padding: 16px;
      margin-top: 20px;
    }
    .flow-loop-grid {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      gap: 8px;
      position: relative;
      margin-top: 12px;
    }
    .flow-loop-step {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid #1f2f54;
      border-radius: 8px;
      padding: 10px 8px;
      text-align: center;
      position: relative;
    }
    .flow-loop-step::after {
      content: '→';
      position: absolute;
      right: -10px;
      top: 50%;
      transform: translateY(-50%);
      color: #00d2ff;
      font-weight: 800;
      font-size: 1.1rem;
      z-index: 2;
    }
    .flow-loop-step:last-child::after {
      content: '↺';
      color: #00e676;
    }
    .flow-step-num {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 800;
      font-size: 0.7rem;
      color: #00d2ff;
      margin-bottom: 4px;
    }
    .flow-step-title {
      font-size: 0.72rem;
      font-weight: 700;
      color: var(--text-main);
      margin-bottom: 4px;
      line-height: 1.2;
    }
    .flow-step-desc {
      font-size: 0.65rem;
      color: var(--text-muted);
      line-height: 1.2;
    }

  
    /* =========================================================================
       7-STAGE PIPELINE & ACTUATORS/SENSORS MATRIX STYLES
       ========================================================================= */
    .stages-container {
      background: #101626;
      border: 1px solid #1e2c4f;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 20px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
    }

    .stages-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      padding-bottom: 8px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
    }

    .stages-title {
      font-size: 0.95rem;
      font-weight: 700;
      color: #00d2ff;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .stages-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
      gap: 10px;
    }

    .stage-card {
      background: #141c30;
      border: 1px solid #1e2c4f;
      border-radius: 8px;
      padding: 10px 12px;
      cursor: pointer;
      transition: all 0.2s ease;
      position: relative;
      overflow: hidden;
    }

    .stage-card:hover {
      border-color: #00d2ff;
      transform: translateY(-2px);
    }

    .stage-card.active {
      border-color: #00e676;
      background: linear-gradient(145deg, #14243b, #101a2e);
      box-shadow: 0 0 12px rgba(0, 230, 118, 0.25);
    }

    .stage-num {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      font-weight: 800;
      color: #8a9fc4;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
    }

    .stage-card.active .stage-num {
      color: #00e676;
    }

    .stage-name {
      font-size: 0.82rem;
      font-weight: 700;
      color: #f0f4fc;
      margin-bottom: 4px;
      line-height: 1.2;
    }

    .stage-sub {
      font-size: 0.72rem;
      color: #8a9fc4;
      line-height: 1.25;
    }

    .stage-badge {
      display: inline-block;
      font-size: 0.65rem;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      padding: 2px 6px;
      border-radius: 4px;
      background: rgba(255, 255, 255, 0.08);
      color: #8a9fc4;
    }

    .stage-card.active .stage-badge {
      background: rgba(0, 230, 118, 0.2);
      color: #00e676;
      border: 1px solid rgba(0, 230, 118, 0.4);
    }

    /* ACTUATORS & SENSORS MATRIX */
    .matrix-container {
      background: #0e1424;
      border: 1px solid #1e2c4f;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 20px;
    }

    .matrix-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      padding-bottom: 8px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
    }

    .matrix-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
      gap: 14px;
    }

    .matrix-col {
      background: #131b2e;
      border: 1px solid #1e2c4f;
      border-radius: 10px;
      padding: 12px;
    }

    .matrix-col-title {
      font-size: 0.85rem;
      font-weight: 700;
      color: #00d2ff;
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px dashed rgba(255, 255, 255, 0.08);
      padding-bottom: 6px;
    }

    .matrix-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 6px 8px;
      margin-bottom: 6px;
      background: rgba(255, 255, 255, 0.02);
      border-radius: 6px;
      border: 1px solid rgba(255, 255, 255, 0.04);
      font-size: 0.78rem;
    }

    .matrix-item-left {
      display: flex;
      flex-direction: column;
      gap: 2px;
    }

    .matrix-tag {
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      color: #8a9fc4;
      font-size: 0.72rem;
    }

    .matrix-desc {
      color: #d1d9ec;
      font-size: 0.75rem;
    }

    .matrix-state {
      display: flex;
      align-items: center;
      gap: 6px;
      font-family: 'JetBrains Mono', monospace;
      font-weight: 700;
      font-size: 0.72rem;
      padding: 3px 8px;
      border-radius: 4px;
    }

    .state-on {
      background: rgba(0, 230, 118, 0.15);
      color: #00e676;
      border: 1px solid rgba(0, 230, 118, 0.4);
    }

    .state-off {
      background: rgba(255, 255, 255, 0.05);
      color: #64748b;
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .state-alarm {
      background: rgba(255, 61, 113, 0.2);
      color: #ff3d71;
      border: 1px solid rgba(255, 61, 113, 0.5);
      animation: pulse 1s infinite;
    }

    .state-warn {
      background: rgba(255, 145, 0, 0.2);
      color: #ff9100;
      border: 1px solid rgba(255, 145, 0, 0.5);
    }

    .cyl-meter-bar {
      height: 4px;
      width: 100%;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 2px;
      margin-top: 4px;
      overflow: hidden;
    }

    .cyl-meter-fill {
      height: 100%;
      background: linear-gradient(90deg, #00d2ff, #00e676);
      width: 50%;
      transition: width 0.3s ease;
    }

  
    /* =========================================================================
       COMPREHENSIVE MOBILE & TABLET RESPONSIVE SYSTEM (DAN DEU TRANG)
       ========================================================================= */
    html, body {
      width: 100%;
      max-width: 100vw;
      overflow-x: hidden !important;
      box-sizing: border-box;
    }

    *, *:before, *:after {
      box-sizing: border-box;
    }

    .table-responsive {
      width: 100%;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      margin-top: 10px;
      margin-bottom: 10px;
      border-radius: 8px;
    }

    .table-responsive table {
      width: 100%;
      min-width: 600px;
    }

    /* Scrollbar styling for touch/mouse */
    .table-responsive::-webkit-scrollbar,
    .fig35-row::-webkit-scrollbar,
    .tab-nav::-webkit-scrollbar {
      height: 6px;
    }
    .table-responsive::-webkit-scrollbar-thumb,
    .fig35-row::-webkit-scrollbar-thumb,
    .tab-nav::-webkit-scrollbar-thumb {
      background: rgba(0, 210, 255, 0.3);
      border-radius: 3px;
    }

    /* Base adjustments for cards */
    .card, .diagram-section, .stages-container, .matrix-container, .visam-container, .fig35-panel-container {
      max-width: 100%;
      box-sizing: border-box;
    }

    /* MOBILE & TABLET BREAKPOINTS */
    @media (max-width: 1024px) {
      .dashboard-container {
        grid-template-columns: 1fr !important;
        gap: 16px;
      }
      .visam-grid {
        grid-template-columns: 1fr !important;
        gap: 12px;
      }
      .subsystems-row {
        grid-template-columns: 1fr !important;
        gap: 12px;
      }
    }

    @media (max-width: 860px) {
      header {
        flex-direction: column !important;
        align-items: stretch !important;
        gap: 10px !important;
        padding: 10px 14px !important;
      }

      .logo-area {
        justify-content: space-between;
        width: 100%;
      }

      .tab-nav {
        display: flex !important;
        overflow-x: auto !important;
        flex-wrap: nowrap !important;
        -webkit-overflow-scrolling: touch !important;
        padding: 4px !important;
        gap: 6px !important;
        width: 100% !important;
      }

      .tab-btn {
        flex-shrink: 0 !important;
        padding: 7px 12px !important;
        font-size: 0.78rem !important;
        white-space: nowrap !important;
      }

      .status-bar {
        display: flex;
        flex-wrap: wrap;
        gap: 6px;
        font-size: 0.72rem;
        justify-content: flex-start;
      }

      .status-pill {
        padding: 3px 8px;
      }

      .tab-content {
        padding: 12px 10px !important;
      }

      .card {
        padding: 14px 10px !important;
        margin-bottom: 14px !important;
      }

      .diagram-section {
        padding: 14px 10px !important;
        margin-bottom: 16px !important;
      }

      .metrics-grid {
        grid-template-columns: repeat(2, 1fr) !important;
        gap: 8px !important;
      }

      .flow-grid {
        grid-template-columns: repeat(2, 1fr) !important;
        gap: 10px !important;
      }

      .state-grid {
        grid-template-columns: 1fr !important;
        gap: 10px !important;
      }

      .stages-grid {
        grid-template-columns: 1fr !important;
        gap: 8px !important;
      }

      .matrix-grid {
        grid-template-columns: 1fr !important;
        gap: 10px !important;
      }

      .tbm-stage-container {
        height: 260px !important;
      }

      .fig35-panel-container {
        padding: 12px 8px !important;
      }

      .fig35-plate-title {
        flex-direction: column;
        align-items: flex-start;
        gap: 4px;
      }

      .diagram-subnav {
        overflow-x: auto !important;
        flex-wrap: nowrap !important;
        -webkit-overflow-scrolling: touch !important;
        padding: 6px 0 !important;
        gap: 6px !important;
      }

      .diag-sub-btn {
        flex-shrink: 0 !important;
        padding: 6px 10px !important;
        font-size: 0.75rem !important;
        white-space: nowrap !important;
      }

      .search-box {
        min-width: 100% !important;
      }

      .console-grid {
        grid-template-columns: 1fr !important;
      }
    }

    @media (max-width: 480px) {
      .metrics-grid {
        grid-template-columns: 1fr !important;
      }

      .flow-grid {
        grid-template-columns: 1fr !important;
      }

      .btn-group {
        grid-template-columns: 1fr !important;
      }

      .visam-pills {
        flex-direction: column;
        align-items: stretch;
      }

      .visam-pill {
        width: 100%;
        justify-content: space-between;
      }
    }

  </style>
</head>
<body>

  <header>
    <div class="logo-area">
      <div class="badge-plc">mts1000-2000C3</div>
      <div>
        <h1 style="font-size: 1.05rem; display: flex; align-items: center; gap: 8px;">
          <span>MTS Perforator TBM - Order 2018221 (S7-300 PLC)</span>
          <span class="mts-badge">375 kW | 630A</span>
        </h1>
        <div style="font-size: 0.72rem; color: var(--text-muted); font-family: 'JetBrains Mono', monospace;">
          Bản vẽ 2018221-BK | 2018221-E-Box | 2018221-VC | DIN 61346 / ISO 16016
        </div>
      </div>
    </div>

    <!-- Main Tab Navigation (5 Tabs) -->
    <div class="tab-nav">
      <button class="tab-btn active" id="btnTabSim" onclick="switchTab('tab-sim')">
        <span>🎮 Mô Phỏng & Bàn ĐK (=cc)</span>
      </button>
      <button class="tab-btn" id="btnTabDiag" onclick="switchTab('tab-diag')">
        <span>📊 Sơ Đồ Điện & 5 Phân Hệ MTS</span>
      </button>
      <button class="tab-btn" id="btnTabComp" onclick="switchTab('tab-comp')">
        <span>🔧 Master BOM Bản Vẽ (118 mục)</span>
      </button>
      <button class="tab-btn" id="btnTabCables" onclick="switchTab('tab-cables')">
        <span>📋 Cáp & Trâm Kẹp (Kabelliste)</span>
      </button>
      <button class="tab-btn" id="btnTabRef" onclick="switchTab('tab-ref')">
        <span>📑 Khối Lệnh PLC (FC/DB)</span>
      </button>
    </div>

    <div class="status-bar">
      <div class="status-pill">
        <div class="led" id="plcLed"></div>
        <span>PLC: <strong id="scanCycle">12 ms (OB1)</strong></span>
      </div>
      <div class="status-pill">
        <span>INTERLOCK: <strong id="fc61Status" style="color: var(--accent-green);">FC61 OK</strong></span>
      </div>
    </div>
  </header>

  <!-- ========================================================
       TAB 1: SIMULATION DASHBOARD (ACTIVE BY DEFAULT)
       ======================================================== -->
  <div id="tab-sim" class="tab-content active">
    <div class="dashboard-container">
      <div style="display: flex; flex-direction: column; gap: 20px;">
        <div class="card">
          <div class="card-title">
            <span>HOẠT ĐỘNG KHIÊN ĐÀO & TUYẾN KÍCH ĐẨY (FC35 / FC20 / FC25 / FC40)</span>
            <span class="tag">REAL-TIME SIMULATION</span>
          </div>

          <div class="tbm-stage-container">
            <canvas id="tbmCanvas"></canvas>
          </div>

          <div class="metrics-grid">
            <div class="metric-box">
              <div class="metric-label">Quãng đường đào (LV)</div>
              <div class="metric-value"><span id="metricDist">14.82</span><span class="metric-unit">m</span></div>
              <span class="metric-tag">DB1.DBD0 (Distance Wheel)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Vận tốc kích đẩy</div>
              <div class="metric-value"><span id="metricSpeed">24.5</span><span class="metric-unit">mm/min</span></div>
              <span class="metric-tag">DB102.DBD2 (Speed Smooth)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Tốc độ đầu cắt</div>
              <div class="metric-value"><span id="metricCutterRpm">4.2</span><span class="metric-unit">RPM</span></div>
              <span class="metric-tag">FC38 / DB21 (Schürfrad)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Lực đẩy tổng cộng</div>
              <div class="metric-value"><span id="metricForce">485</span><span class="metric-unit">Tấn</span></div>
              <span class="metric-tag">DB17 (Druck-Tonnen)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Áp suất đầu cắt</div>
              <div class="metric-value"><span id="metricCutterPres">185</span><span class="metric-unit">bar</span></div>
              <span class="metric-tag">DB16 (CSG/A4VG Pump)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Góc xoắn thân máy</div>
              <div class="metric-value"><span id="metricRoll">+0.8°</span></div>
              <span class="metric-tag">FC81 / FC82 (Inclinometer)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Tia nước cao áp</div>
              <div class="metric-value"><span id="metricJetPres">320</span><span class="metric-unit">bar</span></div>
              <span class="metric-tag">FC90 / FC91 (HD-Pump)</span>
            </div>

            <div class="metric-box">
              <div class="metric-label">Dòng động cơ chính</div>
              <div class="metric-value"><span id="metricMotorAmp">142</span><span class="metric-unit">A</span></div>
              <span class="metric-tag">DB31 / SmartWire PKE</span>
            </div>
          </div>
        </div>

        <!-- ========================================================
             MÀN HÌNH GIÁM SÁT VISAM SCADA (VISAM TBM SCREEN)
             ĐỐI CHIẾU HỒ SƠ PHÁP LÝ MỤC 4.3.1.3 (TRANG 36)
             ======================================================== -->
        <div class="visam-container" id="visamScreen">
          <div class="visam-header">
            <div class="visam-title">
              <span>🖥️ MTS PERFORATOR - VisAM SCADA TBM SCREEN</span>
              <span class="mts-badge" id="visamMode">MODE: AUTOMATIC</span>
            </div>
            <div class="visam-pills">
              <span class="visam-pill">ORD: <strong>2018221</strong></span>
              <span class="visam-pill">CPU: <strong>S7-300 RUN</strong></span>
              <span class="visam-pill">BUS: <strong>PROFIBUS DP 12M</strong></span>
            </div>
          </div>

          <div class="visam-grid">
            <!-- Cột 1: ĐẦU CẮT & GƯƠNG ĐÀO -->
            <div class="visam-card">
              <div class="visam-card-title">
                <span>🪓 ĐẦU CẮT & ĐỊNH VỊ TACS</span>
                <span id="visamCutterDir" style="color: #00e676;">CW (QUAY PHẢI)</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Tốc độ đĩa cắt (Cutter Head Rev):</span>
                <span class="visam-readout-val" id="visamCutterRpm">3.8 1/min</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Áp suất đĩa cắt (Cutter Head P):</span>
                <span class="visam-readout-val" id="visamCutterPres">156 bar</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Áp suất rò vỏ (P case drain):</span>
                <span class="visam-readout-val" id="visamCaseDrain">1.4 bar</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Độ lệch Dọc / Ngang (Laser):</span>
                <span class="visam-readout-val" id="visamDev">V: +2.1mm | H: -1.5mm</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Góc xoay đầu cắt (Head Roll):</span>
                <span class="visam-readout-val" id="visamRoll">+0.85 °</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Khóa khớp khiên (Joint Lock):</span>
                <span class="visam-readout-val" id="visamLockStatus" style="color: #00e676;">🔒 LOCKED</span>
              </div>
            </div>

            <!-- Cột 2: KHOANG BÙN & CÁC VAN ĐẦU KHOAN -->
            <div class="visam-card">
              <div class="visam-card-title">
                <span>🌊 KHOANG BÙN & VAN (=bk)</span>
                <span id="visamSlurryStatus" style="color: #00d2ff;">BALANCED</span>
              </div>
              <div class="visam-valve-box">
                <div class="visam-valve closed" id="visamValveBypass">
                  <div class="visam-valve-name">VAN BY-PASS</div>
                  <div class="visam-valve-state" id="txtValveBypass">CLOSED</div>
                  <div style="font-size: 0.65rem; color: var(--text-muted);">Item 1 & 2</div>
                </div>
                <div class="visam-valve open" id="visamValveJet">
                  <div class="visam-valve-name">VAN BÉC PHUN JET</div>
                  <div class="visam-valve-state" id="txtValveJet">OPEN</div>
                  <div style="font-size: 0.65rem; color: var(--text-muted);">Item 3 & 4</div>
                </div>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Áp suất buồng đào (Slurry chamber P):</span>
                <span class="visam-readout-val" id="visamSlurryPres">1.82 bar</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Lưu lượng nạp (Charge Flow V):</span>
                <span class="visam-readout-val" id="visamChargeFlow">42.5 m³/h</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Lưu lượng xả (Discharge Flow V):</span>
                <span class="visam-readout-val" id="visamDischargeFlow">44.1 m³/h</span>
              </div>
            </div>

            <!-- Cột 3: TUYẾN KÍCH ĐẨY CHÍNH -->
            <div class="visam-card">
              <div class="visam-card-title">
                <span>🚀 TRẠM KÍCH CHÍNH (JACKS)</span>
                <span id="visamJackStatus" style="color: #00e676;">FORWARD</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Tổng chiều dài đã đẩy (Drive Length):</span>
                <span class="visam-readout-val" id="visamDriveLength" style="color: #00e676; font-size: 0.82rem;">14.825 m</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Tốc độ kích đẩy (Drive Speed):</span>
                <span class="visam-readout-val" id="visamDriveSpeed">22.0 mm/min</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Lực đẩy kích chính (Main Jack Unit):</span>
                <span class="visam-readout-val" id="visamJackForce">348 ton</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Áp suất kích chính (Jacks Pressure):</span>
                <span class="visam-readout-val" id="visamJackPres">260 bar</span>
              </div>
              <div class="visam-readout-row">
                <span class="visam-readout-label">Động cơ thủy lực chính 132kW:</span>
                <span class="visam-readout-val" id="visamMainEng" style="color: #00e676;">RUNNING (145A)</span>
              </div>
            </div>
          </div>
        </div>

        <!-- ========================================================
             BẢN VẼ HÌNH 3.5: BÀN ĐIỀU KHIỂN BEDIENPULT (=cc)
             CHÍNH XÁC 100% THEO TẬP I-HỒ SƠ PHÁP LÝ MỤC 4.3.1.3 TRANG 36
             ======================================================== -->
        
    <!-- =========================================================================
         QUY TRÌNH & LOGIC VẬN HÀNH 7 GIAI ĐOẠN (PLC SIEMENS S7-300 / VISAM)
         ========================================================================= -->
    <div class="stages-container">
      <div class="stages-header">
        <div class="stages-title">
          <span>⚙️ CHU TRÌNH VẬN HÀNH 7 GIAI ĐOẠN KHÉP KÍN (IEC 61131-3 SCL/ST)</span>
          <span class="stage-badge" id="badgeCurrentStage">GIAI ĐOẠN HIỆN TẠI: GĐ 4 (KÍCH TIẾN ĐỒNG BỘ)</span>
        </div>
        <div style="display: flex; gap: 8px;">
          <button class="filter-sub-btn active" id="btnAutoSeq" onclick="toggleAutoSequence()" style="font-size: 0.75rem; padding: 4px 10px;">
            ▶ Chạy Tự Động 7 Bước (Auto Seq)
          </button>
          <button class="filter-sub-btn" onclick="stepNextStage()" style="font-size: 0.75rem; padding: 4px 10px;">
            ⏭ Bước Tiếp Theo
          </button>
        </div>
      </div>

      <div class="stages-grid">
        <!-- Giai đoạn 1 -->
        <div class="stage-card active" id="cardStage1" onclick="setStage(1)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 1</span>
            <span class="stage-badge" id="badgeStage1">M61.0: OK</span>
          </div>
          <div class="stage-name">Khởi Tạo & Khóa An Toàn</div>
          <div class="stage-sub">OB100 / FC61: E-Stop I0.0, Phao dầu I12.0/I6.1, T° dầu &lt;70°C, Bus M76.0</div>
        </div>

        <!-- Giai đoạn 2 -->
        <div class="stage-card active" id="cardStage2" onclick="setStage(2)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 2</span>
            <span class="stage-badge" id="badgeStage2">BYPASS: OFF / SLURRY: ON</span>
          </div>
          <div class="stage-name">Tuần Hoàn Bùn Bentonite</div>
          <div class="stage-sub">FC10/11: Van Bypass đóng, van Jet mở, bơm nạp/xả Altivar 50Hz, P buồng đào &lt;4.5 bar</div>
        </div>

        <!-- Giai đoạn 3 -->
        <div class="stage-card active" id="cardStage3" onclick="setStage(3)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 3</span>
            <span class="stage-badge" id="badgeStage3">CW: 3.8 RPM</span>
          </div>
          <div class="stage-name">Đĩa Cắt & Chống Kẹt Áp</div>
          <div class="stage-sub">FC25/26/5: M20.0 chạy, van tỷ lệ PQW268, Anti-Stall tự giảm tốc khi P &gt; 300 bar</div>
        </div>

        <!-- Giai đoạn 4 -->
        <div class="stage-card active" id="cardStage4" onclick="setStage(4)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 4</span>
            <span class="stage-badge" id="badgeStage4">LỰC: 295 TẤN</span>
          </div>
          <div class="stage-name">Kích Đẩy & Đổi Ra Tấn</div>
          <div class="stage-sub">FC19/20/22: Mở chốt M12.7, kích tiến Q16.2/Q17.2, Tấn = P(bar) × 0.24543</div>
        </div>

        <!-- Giai đoạn 5 -->
        <div class="stage-card" id="cardStage5" onclick="setStage(5)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 5</span>
            <span class="stage-badge" id="badgeStage5">DEHNER 1..3: STANDBY</span>
          </div>
          <div class="stage-name">Đồng Bộ Kích Trung Gian</div>
          <div class="stage-sub">FC55: Kích tuần tự Dehner 1 (Q16.1) -&gt; Dehner 2 (Q16.3) -&gt; Dehner 3 -&gt; Kích chính</div>
        </div>

        <!-- Giai đoạn 6 -->
        <div class="stage-card active" id="cardStage6" onclick="setStage(6)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 6</span>
            <span class="stage-badge" id="badgeStage6">TACS: ON | FIN: OFF</span>
          </div>
          <div class="stage-name">Lái 3D 120° & Chống Xoay</div>
          <div class="stage-sub">FC40/43: 3 xilanh lái Q53.0..Q53.5, Inclinometer PIW370, bung cánh Wing khi Roll &gt; 1.5°</div>
        </div>

        <!-- Giai đoạn 7 -->
        <div class="stage-card active" id="cardStage7" onclick="setStage(7)">
          <div class="stage-num">
            <span>GIAI ĐOẠN 7</span>
            <span class="stage-badge" id="badgeStage7">4 BÉC JET: 380 BAR</span>
          </div>
          <div class="stage-name">Phun Nước Siêu Áp 400 bar</div>
          <div class="stage-sub">FC90/91: Bơm Speck Q48.0, P &gt; 250 bar mở 4 van Coaxial Q51.2, 51.3, 51.6, 51.7 xối gương</div>
        </div>
      </div>
    </div>

    <!-- =========================================================================
         MA TRẬN TRẠNG THÁI: CẢM BIẾN, CUỘN SOLENOID & XILANH THỦY LỰC
         ========================================================================= -->
    <div class="matrix-container">
      <div class="matrix-header">
        <div class="stages-title">
          <span>📡 MA TRẬN TRẠNG THÁI THỰC TẾ: CẢM BIẾN, CUỘN COIL SOLENOID & XILANH THỦY LỰC</span>
        </div>
        <div style="font-size: 0.78rem; font-family: 'JetBrains Mono', monospace; color: #8a9fc4;">
          PROFIBUS DP: <span style="color: #00e676;">12/12 NODES ONLINE</span> | TỶ LỆ TRUYỀN: <span style="color: #00d2ff;">12 Mbps</span>
        </div>
      </div>

      <div class="matrix-grid">
        <!-- Cột 1: Van & Cảm biến Bùn / Nước Cao Áp -->
        <div class="matrix-col">
          <div class="matrix-col-title">
            <span>💧 CỤM VAN BÙN & BÉC PHUN SIÊU ÁP</span>
            <span style="font-size: 0.72rem; color: #8a9fc4;">KHOANG ĐẦU KHOAN (=bk)</span>
          </div>

          <!-- Van Bypass Coil -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-K58 / -K59 (A52.0 / A52.1)</span>
              <span class="matrix-desc">Coil Solenoid Van Bypass (Mở / Đóng)</span>
            </div>
            <span class="matrix-state state-off" id="matCoilBypass">A52.1 ĐÓNG</span>
          </div>

          <!-- LS Bypass -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B62 (E52.0 / E52.1)</span>
              <span class="matrix-desc">Công tắc hành trình LS By-pass</span>
            </div>
            <span class="matrix-state state-warn" id="matLsBypass">E52.1 CLOSED</span>
          </div>

          <!-- Van Jet Coil -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-K60 / -K61 (A52.2 / A52.3)</span>
              <span class="matrix-desc">Coil Solenoid Van Nước Jet (Mở / Đóng)</span>
            </div>
            <span class="matrix-state state-on" id="matCoilJet">A52.2 MỞ</span>
          </div>

          <!-- LS Jet -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B64 (E52.2 / E52.3)</span>
              <span class="matrix-desc">Công tắc hành trình LS Jet</span>
            </div>
            <span class="matrix-state state-on" id="matLsJet">E52.2 OPEN</span>
          </div>

          <!-- 4 Van Coaxial Jet -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">Q51.2, 51.3, 51.6, 51.7</span>
              <span class="matrix-desc">4 Van Coaxial Béc Phun Mặt Gương 400 bar</span>
            </div>
            <span class="matrix-state state-on" id="matCoaxValves">4 BÉC PHUN ON</span>
          </div>

          <!-- Bơm Piston Speck Triplex -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-M48.1 (Q48.0) &amp; EW256</span>
              <span class="matrix-desc">Bơm Nước Cao Áp 45kW / P = 380 bar</span>
            </div>
            <span class="matrix-state state-on" id="matPumpHighPres">RUNNING (380 bar)</span>
          </div>
        </div>

        <!-- Cột 2: Xilanh Lái 3 Hướng & Chống Xoay -->
        <div class="matrix-col">
          <div class="matrix-col-title">
            <span>🧭 XILANH LÁI 120° &amp; CÁNH CHỐNG XOAY</span>
            <span style="font-size: 0.72rem; color: #8a9fc4;">KHỚP KHUYÊN (=bk)</span>
          </div>

          <!-- Xilanh Lái 1 -->
          <div class="matrix-item" style="flex-direction: column; align-items: stretch;">
            <div style="display: flex; justify-content: space-between;">
              <div class="matrix-item-left">
                <span class="matrix-tag">XILANH LÁI 1 (0° ĐỈNH) - A53.0/A53.1</span>
                <span class="matrix-desc">P = <span id="matPSteer1">142 bar</span> (EW364) | Hành trình: <span id="matPosSteer1">102 mm</span> (EW356)</span>
              </div>
              <span class="matrix-state state-off" id="matStateSteer1">HOLD</span>
            </div>
            <div class="cyl-meter-bar"><div class="cyl-meter-fill" id="barSteer1" style="width: 51%;"></div></div>
          </div>

          <!-- Xilanh Lái 2 -->
          <div class="matrix-item" style="flex-direction: column; align-items: stretch;">
            <div style="display: flex; justify-content: space-between;">
              <div class="matrix-item-left">
                <span class="matrix-tag">XILANH LÁI 2 (120° PHẢI) - A53.2/A53.3</span>
                <span class="matrix-desc">P = <span id="matPSteer2">138 bar</span> (EW366) | Hành trình: <span id="matPosSteer2">98 mm</span> (EW358)</span>
              </div>
              <span class="matrix-state state-off" id="matStateSteer2">HOLD</span>
            </div>
            <div class="cyl-meter-bar"><div class="cyl-meter-fill" id="barSteer2" style="width: 49%;"></div></div>
          </div>

          <!-- Xilanh Lái 3 -->
          <div class="matrix-item" style="flex-direction: column; align-items: stretch;">
            <div style="display: flex; justify-content: space-between;">
              <div class="matrix-item-left">
                <span class="matrix-tag">XILANH LÁI 3 (240° TRÁI) - A53.4/A53.5</span>
                <span class="matrix-desc">P = <span id="matPSteer3">140 bar</span> (EW368) | Hành trình: <span id="matPosSteer3">100 mm</span> (EW360)</span>
              </div>
              <span class="matrix-state state-off" id="matStateSteer3">HOLD</span>
            </div>
            <div class="cyl-meter-bar"><div class="cyl-meter-fill" id="barSteer3" style="width: 50%;"></div></div>
          </div>

          <!-- Chốt khóa Bàn đẩy -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-K56 / -K57 (A53.6 / A53.7) &amp; Q19.1</span>
              <span class="matrix-desc">Xilanh Chốt Khóa Khung Đẩy (Lock / Unlock)</span>
            </div>
            <span class="matrix-state state-on" id="matLockStatus">🔒 LOCKED (M12.7=0)</span>
          </div>

          <!-- Cánh Chống Xoay (Anti-Roll Wing) -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">Q51.4 / Q51.5 (Wing Solenoids)</span>
              <span class="matrix-desc">Cánh Thép Chống Xoay Ghim Vách Đất</span>
            </div>
            <span class="matrix-state state-off" id="matWingStatus">RETRACTED (THU VÀO)</span>
          </div>
        </div>

        <!-- Cột 3: Kích Chính & Kích Dehner -->
        <div class="matrix-col">
          <div class="matrix-col-title">
            <span>🚜 TRẠM KÍCH CHÍNH &amp; KÍCH TRUNG GIAN</span>
            <span style="font-size: 0.72rem; color: #8a9fc4;">CONTAINER (=pph) &amp; GIẾNG</span>
          </div>

          <!-- Kích chính -->
          <div class="matrix-item" style="flex-direction: column; align-items: stretch;">
            <div style="display: flex; justify-content: space-between;">
              <div class="matrix-item-left">
                <span class="matrix-tag">CỤM 4 XILANH KÍCH CHÍNH - A16.2 / A17.2</span>
                <span class="matrix-desc">P = <span id="matPJack">220 bar</span> (EW406) -&gt; <span id="matFJack" style="color: #00e676; font-weight: bold;">295 TẤN</span></span>
              </div>
              <span class="matrix-state state-on" id="matJackDir">TIẾN (ADVANCING)</span>
            </div>
            <div class="cyl-meter-bar"><div class="cyl-meter-fill" id="barJackStroke" style="width: 42%;"></div></div>
          </div>

          <!-- Van Tỷ Lệ Kích -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">AW264 &amp; AW266 (Van Tỷ Lệ Rexroth)</span>
              <span class="matrix-desc">Điều Tốc 22 mm/min | Chỉnh Áp 220 bar</span>
            </div>
            <span class="matrix-state state-on" id="matJackProp">OBE ACTIVE</span>
          </div>

          <!-- Dehner 1 -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">DEHNER 1 (A16.1) - Dây Rút EW336</span>
              <span class="matrix-desc">Khoảng hở: <span id="matDehner1">0 mm</span> / 1250 mm</span>
            </div>
            <span class="matrix-state state-off" id="matStateDehner1">STANDBY</span>
          </div>

          <!-- Dehner 2 -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">DEHNER 2 (A16.3) - Dây Rút EW344</span>
              <span class="matrix-desc">Khoảng hở: <span id="matDehner2">0 mm</span> / 1250 mm</span>
            </div>
            <span class="matrix-state state-off" id="matStateDehner2">STANDBY</span>
          </div>

          <!-- Dehner 3 -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">DEHNER 3 (A17.1) - Dây Rút EW380</span>
              <span class="matrix-desc">Khoảng hở: <span id="matDehner3">0 mm</span> / 1250 mm</span>
            </div>
            <span class="matrix-state state-off" id="matStateDehner3">STANDBY</span>
          </div>
        </div>

        <!-- Cột 4: Cảm biến Giám sát & An toàn -->
        <div class="matrix-col">
          <div class="matrix-col-title">
            <span>🛡️ GIÁM SÁT AN TOÀN &amp; ÁP LỰC ĐÀO</span>
            <span style="font-size: 0.72rem; color: #8a9fc4;">PLC INTERLOCKS</span>
          </div>

          <!-- Cờ Master Safety -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">CỜ LIÊN ĐỘNG TỔNG M61.0</span>
              <span class="matrix-desc">Freigaben Master Safety Check</span>
            </div>
            <span class="matrix-state state-on" id="matM61">M61.0 = TRUE (OK)</span>
          </div>

          <!-- Mức dầu bồn -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B1 / -B2 (E12.1 / E12.0)</span>
              <span class="matrix-desc">Phao Cảnh Báo &amp; Ngắt Cạn Dầu Bồn 2000L</span>
            </div>
            <span class="matrix-state state-on" id="matOilLevel">NORMAL (ĐỦ DẦU)</span>
          </div>

          <!-- Nhiệt độ & Độ ẩm Dầu -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B10 (EW404 / EW408)</span>
              <span class="matrix-desc">T° Dầu: 48.2°C (&lt;70°C) | Độ Ẩm: 14% rH</span>
            </div>
            <span class="matrix-state state-on" id="matOilTempHum">SAFE</span>
          </div>

          <!-- Áp buồng đào EPB -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B242 (EW374 - Slurry Chamber)</span>
              <span class="matrix-desc">Áp Suất Buồng Đào: <span id="matSlurryPres">1.82 bar</span> (Chuẩn EPB)</span>
            </div>
            <span class="matrix-state state-on" id="matSlurryStatus">CÂN BẰNG</span>
          </div>

          <!-- Áp rò phớt trục -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B241 (EW372 - Case Drain)</span>
              <span class="matrix-desc">Áp Suất Rò Phớt Trục Chính: 0.45 bar (&lt;2.5 bar)</span>
            </div>
            <span class="matrix-state state-on" id="matDrainStatus">PHỚT TỐT</span>
          </div>

          <!-- Áp mâm cắt & Anti-Stall -->
          <div class="matrix-item">
            <div class="matrix-item-left">
              <span class="matrix-tag">-B244 / -B3 (EW378 / EW410)</span>
              <span class="matrix-desc">Áp Đĩa Cắt: <span id="matCutterPres">185 bar</span> | Anti-Stall: STANDBY</span>
            </div>
            <span class="matrix-state state-on" id="matAntiStall">BÌNH THƯỜNG</span>
          </div>
        </div>
      </div>
    </div>


    <div class="fig35-panel-container">
          <div class="fig35-plate-title">
            <div>
              <div class="fig35-plate-heading">FIGURE 3.5. CONTROL PANEL (=cc BEDIENPULT)</div>
              <div class="fig35-plate-sub">Trích xuất Hồ sơ Pháp lý Tập I - Mục 4.3.1.3 Trang 36 | 33 Phần tử điều khiển (Item 0..32)</div>
            </div>
            <span class="mts-badge">EATON SMARTWIRE-DT</span>
          </div>

          <div class="fig35-rows-wrap">
            <!-- HÀNG 1: ROW 1 (ITEM 01 ĐẾN 09) -->
            <div class="fig35-row">
              <div class="fig35-row-badge">ROW 1</div>

              <!-- Item 01 -->
              <div class="fig35-item">
                <span class="fig35-item-num">01</span>
                <span class="fig35-item-label">Bypass Open</span>
                <button class="btn-illuminated" id="btnFig01" onclick="actuateFigItem(1)"></button>
                <span class="fig35-item-bmk">-S2.9 (EB67)</span>
              </div>

              <!-- Item 02 -->
              <div class="fig35-item">
                <span class="fig35-item-num">02</span>
                <span class="fig35-item-label">Bypass Closed</span>
                <button class="btn-illuminated active-yellow" id="btnFig02" onclick="actuateFigItem(2)"></button>
                <span class="fig35-item-bmk">-S2.8 (EB68)</span>
              </div>

              <!-- Item 03 -->
              <div class="fig35-item">
                <span class="fig35-item-num">03</span>
                <span class="fig35-item-label">Jet Open</span>
                <button class="btn-illuminated active-cyan" id="btnFig03" onclick="actuateFigItem(3)"></button>
                <span class="fig35-item-bmk">-S2.7 (EB69)</span>
              </div>

              <!-- Item 04 -->
              <div class="fig35-item">
                <span class="fig35-item-num">04</span>
                <span class="fig35-item-label">Jet Closed</span>
                <button class="btn-illuminated" id="btnFig04" onclick="actuateFigItem(4)"></button>
                <span class="fig35-item-bmk">-S2.6 (EB70)</span>
              </div>

              <!-- Item 05 -->
              <div class="fig35-item">
                <span class="fig35-item-num">05</span>
                <span class="fig35-item-label">Hydr Main ON</span>
                <button class="btn-illuminated active-green" id="btnFig05" onclick="actuateFigItem(5)"></button>
                <span class="fig35-item-bmk">-S2.5 (EB71)</span>
              </div>

              <!-- Item 06 -->
              <div class="fig35-item">
                <span class="fig35-item-num">06</span>
                <span class="fig35-item-label">Main OFF/Fail</span>
                <button class="btn-illuminated" id="btnFig06" onclick="actuateFigItem(6)"></button>
                <span class="fig35-item-bmk">-S2.4 (EB72)</span>
              </div>

              <!-- Item 07 -->
              <div class="fig35-item">
                <span class="fig35-item-num">07</span>
                <span class="fig35-item-label">Speed Charge</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig07" style="transform: rotate(45deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig07">65%</span>
                  <input type="range" class="dial-slider" min="0" max="100" value="65" oninput="updateFigPoti(7, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.1 (PEW278)</span>
              </div>

              <!-- Item 08 -->
              <div class="fig35-item">
                <span class="fig35-item-num">08</span>
                <span class="fig35-item-label">Charge ON</span>
                <button class="btn-illuminated active-green" id="btnFig08" onclick="actuateFigItem(8)"></button>
                <span class="fig35-item-bmk">-S2.3 (EB73)</span>
              </div>

              <!-- Item 09 -->
              <div class="fig35-item">
                <span class="fig35-item-num">09</span>
                <span class="fig35-item-label">Charge OFF/Rst</span>
                <button class="btn-illuminated" id="btnFig09" onclick="actuateFigItem(9)"></button>
                <span class="fig35-item-bmk">-S2.2 (EB74)</span>
              </div>
            </div>

            <!-- HÀNG 2: ROW 2 (ITEM 10 ĐẾN 16 + 2 DUMMY PLUGS) -->
            <div class="fig35-row">
              <div class="fig35-row-badge">ROW 2</div>

              <!-- Item 10 -->
              <div class="fig35-item">
                <span class="fig35-item-num">10</span>
                <span class="fig35-item-label">Steer Cyl 1</span>
                <div class="rocker-switch state-mid" id="rockerFig10" onclick="cycleRocker(10)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.10 (EB66)</span>
              </div>

              <!-- Item 11 -->
              <div class="fig35-item">
                <span class="fig35-item-num">11</span>
                <span class="fig35-item-label">Steer Cyl 2</span>
                <div class="rocker-switch state-mid" id="rockerFig11" onclick="cycleRocker(11)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.11 (EB65)</span>
              </div>

              <!-- Item 12 -->
              <div class="fig35-item">
                <span class="fig35-item-num">12</span>
                <span class="fig35-item-label">Steer Cyl 3</span>
                <div class="rocker-switch state-mid" id="rockerFig12" onclick="cycleRocker(12)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.12 (EB64)</span>
              </div>

              <!-- Item 13 -->
              <div class="fig35-item">
                <span class="fig35-item-num">13</span>
                <span class="fig35-item-label">Fin Extend</span>
                <div class="rocker-switch state-down" id="rockerFig13" onclick="cycleRocker(13)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.13 (EB63)</span>
              </div>

              <!-- Dummy Plug 00 -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare Hole</span>
              </div>

              <!-- Dummy Plug 00 -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare Hole</span>
              </div>

              <!-- Item 14 -->
              <div class="fig35-item">
                <span class="fig35-item-num">14</span>
                <span class="fig35-item-label">Speed Feed</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig14" style="transform: rotate(60deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig14">70%</span>
                  <input type="range" class="dial-slider" min="0" max="100" value="70" oninput="updateFigPoti(14, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.2 (PEW280)</span>
              </div>

              <!-- Item 15 -->
              <div class="fig35-item">
                <span class="fig35-item-num">15</span>
                <span class="fig35-item-label">Feed ON</span>
                <button class="btn-illuminated active-green" id="btnFig15" onclick="actuateFigItem(15)"></button>
                <span class="fig35-item-bmk">-S2.16 (EB76)</span>
              </div>

              <!-- Item 16 -->
              <div class="fig35-item">
                <span class="fig35-item-num">16</span>
                <span class="fig35-item-label">Feed OFF/Rst</span>
                <button class="btn-illuminated" id="btnFig16" onclick="actuateFigItem(16)"></button>
                <span class="fig35-item-bmk">-S2.17 (EB75)</span>
              </div>
            </div>

            <!-- HÀNG 3: ROW 3 (ITEM 17 ĐẾN 22 + 2 DUMMY PLUGS) -->
            <div class="fig35-row">
              <div class="fig35-row-badge">ROW 3</div>

              <!-- Item 17 -->
              <div class="fig35-item">
                <span class="fig35-item-num">17</span>
                <span class="fig35-item-label">Speed Cutting</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig17" style="transform: rotate(35deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig17">3.8 RPM</span>
                  <input type="range" class="dial-slider" min="0" max="60" value="38" oninput="updateFigPoti(17, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.6 (PEW276)</span>
              </div>

              <!-- Item 18 -->
              <div class="fig35-item">
                <span class="fig35-item-num">18</span>
                <span class="fig35-item-label">Speed Jacking</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig18" style="transform: rotate(45deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig18">22 mm/m</span>
                  <input type="range" class="dial-slider" min="0" max="50" value="22" oninput="updateFigPoti(18, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.5 (PEW274)</span>
              </div>

              <!-- Item 19 -->
              <div class="fig35-item">
                <span class="fig35-item-num">19</span>
                <span class="fig35-item-label">Pressure Jack</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig19" style="transform: rotate(55deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig19">260 bar</span>
                  <input type="range" class="dial-slider" min="50" max="400" value="260" oninput="updateFigPoti(19, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.4 (PEW272)</span>
              </div>

              <!-- Dummy Plug 00 -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare Hole</span>
              </div>

              <!-- Dummy Plug 00 -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare Hole</span>
              </div>

              <!-- Item 20 -->
              <div class="fig35-item">
                <span class="fig35-item-num">20</span>
                <span class="fig35-item-label">Speed Addition</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig20" style="transform: rotate(0deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig20">0%</span>
                  <input type="range" class="dial-slider" min="0" max="100" value="0" oninput="updateFigPoti(20, this.value)">
                </div>
                <span class="fig35-item-bmk">-R2.3 (PEW282)</span>
              </div>

              <!-- Item 21 -->
              <div class="fig35-item">
                <span class="fig35-item-num">21</span>
                <span class="fig35-item-label">Addition ON</span>
                <button class="btn-illuminated" id="btnFig21" onclick="actuateFigItem(21)"></button>
                <span class="fig35-item-bmk">-S2.19 (EB77)</span>
              </div>

              <!-- Item 22 -->
              <div class="fig35-item">
                <span class="fig35-item-num">22</span>
                <span class="fig35-item-label">Add OFF/Rst</span>
                <button class="btn-illuminated active-red" id="btnFig22" onclick="actuateFigItem(22)"></button>
                <span class="fig35-item-bmk">-S2.18 (EB78)</span>
              </div>
            </div>

            <!-- HÀNG 4: ROW 4 (ITEM 23 ĐẾN 26 + 3 DUMMY PLUGS) -->
            <div class="fig35-row">
              <div class="fig35-row-badge">ROW 4</div>

              <!-- Item 23 -->
              <div class="fig35-item">
                <span class="fig35-item-num">23</span>
                <span class="fig35-item-label">Mode Flushing</span>
                <div class="dial-wrap">
                  <div class="dial-knob" id="knobFig23" onclick="cycleSelector23()" style="transform: rotate(45deg);"><div class="dial-pointer"></div></div>
                  <span class="dial-val" id="valFig23">AUTO</span>
                </div>
                <span class="fig35-item-bmk">-S2.20 (EB79)</span>
              </div>

              <!-- Item 24 -->
              <div class="fig35-item">
                <span class="fig35-item-num">24</span>
                <span class="fig35-item-label">Single Cylinders</span>
                <div class="rocker-switch state-down" id="rockerFig24" onclick="cycleRocker(24)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.21 (EB80)</span>
              </div>

              <!-- Item 25 -->
              <div class="fig35-item">
                <span class="fig35-item-num">25</span>
                <span class="fig35-item-label">Mode Jacking</span>
                <div class="rocker-switch state-up" id="rockerFig25" onclick="cycleRocker(25)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.22 (EB81)</span>
              </div>

              <!-- Item 26 -->
              <div class="fig35-item">
                <span class="fig35-item-num">26</span>
                <span class="fig35-item-label">Cylinder Lock</span>
                <div class="rocker-switch state-up" id="rockerFig26" onclick="cycleRocker(26)">
                  <div class="rocker-lever"></div>
                </div>
                <span class="fig35-item-bmk">-S2.23 (EB82)</span>
              </div>

              <!-- Dummy Plugs (3 slots) -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
            </div>

            <!-- HÀNG 5: ROW 5 (ITEM 27 ĐẾN 32 + 3 DUMMY PLUGS) -->
            <div class="fig35-row">
              <div class="fig35-row-badge">ROW 5</div>

              <!-- Item 27 -->
              <div class="fig35-item">
                <span class="fig35-item-num">27</span>
                <span class="fig35-item-label">Disc: Left</span>
                <button class="btn-illuminated" id="btnFig27" onclick="actuateFigItem(27)"></button>
                <span class="fig35-item-bmk">-S2.33 (EB92)</span>
              </div>

              <!-- Item 28 -->
              <div class="fig35-item">
                <span class="fig35-item-num">28</span>
                <span class="fig35-item-label">Disc: STOP</span>
                <button class="btn-illuminated" id="btnFig28" onclick="actuateFigItem(28)"></button>
                <span class="fig35-item-bmk">-S2.32 (EB91)</span>
              </div>

              <!-- Item 29 -->
              <div class="fig35-item">
                <span class="fig35-item-num">29</span>
                <span class="fig35-item-label">Disc: Right</span>
                <button class="btn-illuminated active-green" id="btnFig29" onclick="actuateFigItem(29)"></button>
                <span class="fig35-item-bmk">-S2.31 (EB90)</span>
              </div>

              <!-- Item 30 -->
              <div class="fig35-item">
                <span class="fig35-item-num">30</span>
                <span class="fig35-item-label">Jacks: Forward</span>
                <button class="btn-illuminated active-green" id="btnFig30" onclick="actuateFigItem(30)"></button>
                <span class="fig35-item-bmk">-S2.30 (EB89)</span>
              </div>

              <!-- Item 31 -->
              <div class="fig35-item">
                <span class="fig35-item-num">31</span>
                <span class="fig35-item-label">Jacks: STOP</span>
                <button class="btn-illuminated" id="btnFig31" onclick="actuateFigItem(31)"></button>
                <span class="fig35-item-bmk">-S2.29 (EB88)</span>
              </div>

              <!-- Item 32 -->
              <div class="fig35-item">
                <span class="fig35-item-num">32</span>
                <span class="fig35-item-label">Jacks: Back</span>
                <button class="btn-illuminated" id="btnFig32" onclick="actuateFigItem(32)"></button>
                <span class="fig35-item-bmk">-S2.28 (EB87)</span>
              </div>

              <!-- Dummy Plugs (3 slots) -->
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
              <div class="fig35-item">
                <span class="fig35-item-num">00</span>
                <span class="fig35-item-label">Dummy Plug</span>
                <div class="dummy-plug"></div>
                <span class="fig35-item-bmk">Spare</span>
              </div>
            </div>
          </div>
        </div>

        <!-- ========================================================
             CHUỖI TRUYỀN TÍN HIỆU VÒNG LẶP KÍN (CLOSED-LOOP SIGNAL FLOW)
             ======================================================== -->
        <div class="flow-loop-container">
          <div class="card-title">
            <span>CHUỖI TRUYỀN TÍN HIỆU VÒNG LẶP KÍN (CLOSED-LOOP SIGNAL FLOW - TRANG 36)</span>
            <span class="tag">MỤC 4.3.1.3</span>
          </div>
          <p style="font-size: 0.8rem; color: var(--text-muted);">
            Sơ đồ đường đi 6 bước của dòng tín hiệu điều khiển khi thợ vận hành tác động nút bấm trên Bàn điều khiển (=cc):
          </p>
          <div class="flow-loop-grid">
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 1</div>
              <div class="flow-step-title">Nút bấm / Chiết áp</div>
              <div class="flow-step-desc">Thao tác tại cabin (=cc), truyền qua cáp dẹt SmartWire-DT về gateway.</div>
            </div>
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 2</div>
              <div class="flow-step-title">PLC Siemens S7-300</div>
              <div class="flow-step-desc">CPU 315-2 PN/DP quét khối hàm FC, tính toán liên động và bảo vệ.</div>
            </div>
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 3</div>
              <div class="flow-step-title">Mạng Cáp Profibus DP</div>
              <div class="flow-step-desc">Truyền gói tin xuống trạm Turck PicoNet (=bk) và Turck BL67 (=pph).</div>
            </div>
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 4</div>
              <div class="flow-step-title">Cơ cấu chấp hành</div>
              <div class="flow-step-desc">Cuộn solenoid van thủy lực mở dầu / Biến tần ATV630 cấp nguồn motor.</div>
            </div>
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 5</div>
              <div class="flow-step-title">Cảm biến phản hồi</div>
              <div class="flow-step-desc">Công tắc LS, cảm biến áp suất 4-20mA, đếm xung RPM gửi tín hiệu về.</div>
            </div>
            <div class="flow-loop-step">
              <div class="flow-step-num">BƯỚC 6</div>
              <div class="flow-step-title">Đèn LED & Màn hình VisAM</div>
              <div class="flow-step-desc">PLC xuất lệnh sáng đèn LED nút bấm và nhảy số trên Màn hình VisAM.</div>
            </div>
          </div>
        </div>



        <div class="subsystems-row">
          <div class="card">
            <div class="card-title">
              <span>ĐỊNH VỊ LASER TACS (FC80/81)</span>
              <span class="tag">TARGET</span>
            </div>
            <div class="laser-target-box">
              <div class="laser-crosshair-h"></div>
              <div class="laser-crosshair-v"></div>
              <div class="laser-ring" style="width: 50px; height: 50px;"></div>
              <div class="laser-ring" style="width: 100px; height: 100px;"></div>
              <div class="laser-dot" id="laserDot" style="top: 45%; left: 52%;"></div>
            </div>
            <div style="display: flex; justify-content: space-between; margin-top: 10px; font-size: 0.75rem; font-family: 'JetBrains Mono', monospace;">
              <span>ΔX: <strong id="laserX" style="color: var(--accent-cyan);">+4 mm</strong></span>
              <span>ΔY: <strong id="laserY" style="color: var(--accent-cyan);">-8 mm</strong></span>
              <span>WING: <strong id="wingStatus" style="color: var(--accent-green);">ON (FC43)</strong></span>
            </div>
          </div>

          <div class="card">
            <div class="card-title">
              <span>TRẠM KÍCH TRUNG GIAN</span>
              <span class="tag">DEHNER 1..5</span>
            </div>
            <div class="jacks-bar-container">
              <div class="jack-item">
                <span class="jack-name">Kích chính</span>
                <div class="jack-bar-bg"><div class="jack-bar-fill" id="barMainJack" style="width: 65%;"></div></div>
                <span class="jack-val" id="valMainJack">65%</span>
              </div>
              <div class="jack-item">
                <span class="jack-name">Dehner #1 (FC55)</span>
                <div class="jack-bar-bg"><div class="jack-bar-fill" id="barD1" style="width: 40%;"></div></div>
                <span class="jack-val" id="valD1">40%</span>
              </div>
              <div class="jack-item">
                <span class="jack-name">Dehner #2 (FC56)</span>
                <div class="jack-bar-bg"><div class="jack-bar-fill" id="barD2" style="width: 55%;"></div></div>
                <span class="jack-val" id="valD2">55%</span>
              </div>
              <div class="jack-item">
                <span class="jack-name">Dehner #3 (FC57)</span>
                <div class="jack-bar-bg"><div class="jack-bar-fill" id="barD3" style="width: 30%;"></div></div>
                <span class="jack-val" id="valD3">30%</span>
              </div>
            </div>
          </div>

          <div class="card">
            <div class="card-title">
              <span>TUẦN HOÀN BÙN & NƯỚC CAO ÁP</span>
              <span class="tag">HYDRAULIC</span>
            </div>
            <div class="table-responsive">
<table class="plc-table">
              <tr>
                <th>Cụm thiết bị</th>
                <th>Khối lệnh</th>
                <th>Trạng thái</th>
              </tr>
              <tr>
                <td>Bơm cấp bùn</td>
                <td>FC10 Charge</td>
                <td class="val" id="stCharge">RUNNING</td>
              </tr>
              <tr>
                <td>Bơm thải bùn</td>
                <td>FC11 Discharge</td>
                <td class="val" id="stDischarge">RUNNING</td>
              </tr>
              <tr>
                <td>Bơm tăng áp</td>
                <td>FC12 Booster</td>
                <td class="val" id="stBooster">AUTO</td>
              </tr>
              <tr>
                <td>Van cao áp BK</td>
                <td>FC91 HD-Valve</td>
                <td class="val" id="stHdValve" style="color: var(--accent-green);">OPEN</td>
              </tr>
            </table>
</div>
          </div>
        </div>
      </div>

      <!-- HMI Controls -->
      <div style="display: flex; flex-direction: column; gap: 20px;">
        <div class="card">
          <div class="card-title">
            <span>BẢNG ĐIỀU KHIỂN HMI (DB19)</span>
            <span class="tag">TOUCHSCREEN</span>
          </div>

          <div class="btn-group">
            <button class="btn active" id="btnAuto" onclick="toggleAuto()">TỰ ĐỘNG (AUTO)</button>
            <button class="btn" id="btnManual" onclick="toggleAuto()">THỦ CÔNG (MAN)</button>
          </div>

          <div class="btn-group">
            <button class="btn active" id="btnCutterCw" onclick="setCutterDir(1)">QUAY PHẢI (CW)</button>
            <button class="btn" id="btnCutterCcw" onclick="setCutterDir(-1)">QUAY TRÁI (CCW)</button>
          </div>

          <div class="control-group" style="margin-top: 14px;">
            <div class="control-label">
              <span>Tốc độ đầu cắt (Potentiometer)</span>
              <span class="val" id="txtCutterSpeed">70%</span>
            </div>
            <input type="range" id="sliderCutter" min="0" max="100" value="70" oninput="updateSliders()">
          </div>

          <div class="control-group">
            <div class="control-label">
              <span>Tốc độ kích đẩy (Jacks Speed)</span>
              <span class="val" id="txtJackSpeed">60%</span>
            </div>
            <input type="range" id="sliderJacks" min="0" max="100" value="60" oninput="updateSliders()">
          </div>

          <div class="control-group">
            <div class="control-label">
              <span>Góc bẻ lái xi lanh (Steering)</span>
              <span class="val" id="txtSteering">0.0°</span>
            </div>
            <input type="range" id="sliderSteering" min="-30" max="30" value="0" oninput="updateSliders()">
          </div>

          <div style="margin-top: 18px; display: flex; flex-direction: column; gap: 8px;">
            <button class="btn" id="btnJet" onclick="toggleJet()">💧 BẬT PHUN NƯỚC CAO ÁP (FC90/91)</button>
            <button class="btn danger" id="btnEStop" onclick="toggleEStop()">🛑 DỪNG KHẨN CẤP (E-STOP)</button>
          </div>
        </div>

        <div class="card">
          <div class="card-title">
            <span>GIÁM SÁT Ô NHỚ PLC REAL-TIME</span>
            <span class="tag">DB LOGGING</span>
          </div>

          <div class="table-responsive">
<table class="plc-table">
            <tr>
              <th>Địa chỉ PLC</th>
              <th>Mô tả tín hiệu</th>
              <th>Giá trị hiện tại</th>
            </tr>
            <tr>
              <td><code>DB57.DBD0</code></td>
              <td>Quãng đường đã kích</td>
              <td class="val" id="dbDist">14820 mm</td>
            </tr>
            <tr>
              <td><code>DB102.DBD2</code></td>
              <td>Tốc độ kích mượt</td>
              <td class="val" id="dbSpeed">24.5 mm/min</td>
            </tr>
            <tr>
              <td><code>DB21.DBD0</code></td>
              <td>Tốc độ quay Schürfrad</td>
              <td class="val" id="dbRpm">4.2 RPM</td>
            </tr>
            <tr>
              <td><code>DB16.DBW12</code></td>
              <td>Áp suất kích chính</td>
              <td class="val" id="dbPres">245 bar</td>
            </tr>
            <tr>
              <td><code>DB59.DBD4</code></td>
              <td>Lực xi lanh lái 1</td>
              <td class="val" id="dbCyl1">112 Tấn</td>
            </tr>
            <tr>
              <td><code>DB59.DBD8</code></td>
              <td>Lực xi lanh lái 2</td>
              <td class="val" id="dbCyl2">115 Tấn</td>
            </tr>
            <tr>
              <td><code>DB58.DBX0.0</code></td>
              <td>Cờ lỗi tổng (Fault)</td>
              <td class="val" id="dbFault" style="color: var(--accent-green);">0 (NO FAULT)</td>
            </tr>
          </table>
</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ========================================================
       TAB 2: SYSTEM & PLC DIAGRAMS
       ======================================================== -->
  <div id="tab-diag" class="tab-content">
    
    <div class="diagram-subnav">
      <button class="diag-sub-btn active" onclick="scrollToDiagram('diag1')">1. Kiến trúc Tổng thể (Master Architecture)</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag2')">2. 14 Networks OB1 (Pipeline)</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag3')">3. Luồng Dữ liệu (DB Mapping)</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag4')">4. Liên động An toàn (Safety Loop)</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag5')">5. 5 Phân Hệ Bản Vẽ Điện MTS</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag6')">6. Mạng Profibus & SmartWire</button>
      <button class="diag-sub-btn" onclick="scrollToDiagram('diag7')">7. Chuẩn Mã Màu Dây (DIN 61346)</button>
    </div>

    <!-- Diagram 1 -->
    <div class="diagram-section" id="diag1">
      <div class="diag-header">
        <h2>1. Sơ Đồ Kiến Trúc Toàn Bộ Hệ Thống (Master System Architecture)</h2>
        <p>Phân cấp 5 tầng kiến trúc: từ Giao diện HMI/SCADA &rarr; Khối tổ chức OB &rarr; Khối hàm công nghệ FC/FB &rarr; Vùng nhớ DB &rarr; Phần cứng Profibus DP / SmartWire-DT.</p>
      </div>

      <div class="arch-layer-container">
        <div class="arch-layer hmi">
          <div class="arch-layer-title">🖥️ TẦNG 1: GIAO DIỆN VẬN HÀNH & GIÁM SÁT (HMI & SCADA LAYER)</div>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">DB19 <span class="badge">HMI TOUCH</span></div>
              <div class="node-name">Touchscreen Interface</div>
              <div class="node-desc">Giao tiếp nút bấm, chiết áp tốc độ, chế độ Auto/Man và hiển thị trạng thái máy.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">DB57 <span class="badge">SCADA LOG</span></div>
              <div class="node-name">DataLogging2</div>
              <div class="node-desc">Thu thập định kỳ toàn bộ thông số áp suất, quãng đường, lực đẩy gửi về máy tính trung tâm.</div>
            </div>
          </div>
        </div>

        <div class="arch-layer obs">
          <div class="arch-layer-title">⚙️ TẦNG 2: CÁC KHỐI TỔ CHỨC HỆ THỐNG (ORGANIZATION BLOCKS)</div>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">OB1 <span class="badge">MAIN SCAN</span></div>
              <div class="node-name">MainControl Cycle</div>
              <div class="node-desc">Vòng quét chính 10-15ms gọi tuần tự 14 Networks công nghệ.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">OB32 / OB33 <span class="badge">CYCLIC INT</span></div>
              <div class="node-name">Cyclic Interrupts</div>
              <div class="node-desc">Ngắt chu kỳ thời gian thực 100ms gọi FB100 làm mịn tốc độ kích.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">OB100 <span class="badge">STARTUP</span></div>
              <div class="node-name">Complete Restart</div>
              <div class="node-desc">Khởi tạo tham số ban đầu, hệ số hiệu chuẩn và reset cờ truyền thông.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">OB82/85/86/121/122 <span class="badge">FAULTS</span></div>
              <div class="node-name">Fault Handlers</div>
              <div class="node-desc">Chống dừng CPU khi rớt mạng Profibus-DP, lỗi module I/O hoặc lỗi lập trình.</div>
            </div>
          </div>
        </div>

        <div class="arch-layer fcs">
          <div class="arch-layer-title">🧠 TẦNG 3: CÁC KHỐI HÀM CÔNG NGHỆ (71 FCs & 2 FBs)</div>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">FC61 / 71 / 72 / 76 <span class="badge">SAFETY</span></div>
              <div class="node-name">Liên động & An toàn</div>
              <div class="node-desc">FC61 (Freigaben), FC76 (DP Ausfall), FC71/72 (Quản lý và xóa lỗi hệ thống).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">FC14 / 25 / 26 / 38 <span class="badge">CUTTER</span></div>
              <div class="node-name">Đầu cắt Schürfrad</div>
              <div class="node-desc">FC14 (Motor), FC25 (Control), FC26 (Speed), FC38 (Đo RPM), FC4/5/140/150 (Bơm CSG/A4VG).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">FC20 / 21 / 22 / 55..57 <span class="badge">JACKS</span></div>
              <div class="node-name">Kích chính & Dehner 1..5</div>
              <div class="node-desc">FC20..22 (Kích chính), FC55..57 (Dehner 1..3), FC141/151 (Dehner 4..5), FC35 (Đo hành trình).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">FC40..42 / 80 / 81 / 82 <span class="badge">STEERING</span></div>
              <div class="node-name">Lái hướng & Laser TACS</div>
              <div class="node-desc">FC40..42 (3 Xi lanh lái 120°), FC80 (Laser TACS), FC81 (Inclinometer), FC82 (Bù xoắn), FC43 (Wing).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">FC10 / 11 / 12 / 90 / 91 <span class="badge">PUMPS & JET</span></div>
              <div class="node-name">Tuần hoàn Bùn & Nước Cao Áp</div>
              <div class="node-desc">FC10 (Charge), FC11 (Discharge), FC12 (Booster), FC90 (Bơm 400 bar), FC91 (Van phun BK).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">FC110 / 111 / 112 / 125 <span class="badge">FIELDBUS</span></div>
              <div class="node-name">SmartWire & Biến tần</div>
              <div class="node-desc">FC110/111 (SmartWire PKE), FC112 (Biến tần Altivar), FC125 / FB125 (Chẩn đoán Profibus DP).</div>
            </div>
          </div>
        </div>

        <div class="arch-layer dbs">
          <div class="arch-layer-title">💾 TẦNG 4: VÙNG NHỚ DỮ LIỆU CHÍNH (DATA BLOCKS)</div>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">DB1 / DB102 <span class="badge">DISTANCE</span></div>
              <div class="node-name">Quãng đường & Vận tốc</div>
              <div class="node-desc">DB1 (LV_DB xung đo thô), DB102 (LV_Speed_DB vận tốc sau lọc mượt).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">DB16 / DB17 <span class="badge">PRESSURE</span></div>
              <div class="node-name">Áp suất & Lực Tấn</div>
              <div class="node-desc">DB16 (Analogvalues từ cảm biến), DB17 (Druck-Tonnen bảng quy đổi Bar ra Tấn).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">DB20 / DB21 <span class="badge">SETPOINTS</span></div>
              <div class="node-name">Cài đặt Kích & Đầu cắt</div>
              <div class="node-desc">DB20 (Speed-Pres Jacks), DB21 (Speed Cutter & Torque Limit).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">DB31 / DB33 / DB112 <span class="badge">DRIVES</span></div>
              <div class="node-name">Dòng tải & Biến tần</div>
              <div class="node-desc">DB31 (Dòng điện motor PKE), DB33 (Aptomat NZM), DB112 (Bảng thanh ghi Altivar).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">DB58 / DB59 <span class="badge">FAULTS & CYL</span></div>
              <div class="node-name">Cờ lỗi & Lực xi lanh lái</div>
              <div class="node-desc">DB58 (Fault_DB bảng bit lỗi), DB59 (Cyl_Force lực 3 xi lanh lái).</div>
            </div>
          </div>
        </div>

        <div class="arch-layer hw">
          <div class="arch-layer-title">🔌 TẦNG 5: PHẦN CỨNG TRƯỜNG & MẠNG TRUYỀN THÔNG</div>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">CPU 315-2 PN/DP <span class="badge">CENTRAL PLC</span></div>
              <div class="node-name">Siemens SIMATIC S7-300</div>
              <div class="node-desc">Bộ điều khiển trung tâm quản lý toàn bộ quá trình khoan kích ngầm.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">Profibus-DP <span class="badge">12 Mbps</span></div>
              <div class="node-name">Turck BL20, BL67, SDPB IP67</div>
              <div class="node-desc">Thu thập I/O phân tán trên thân khiên đào trong môi trường bùn nước khắc nghiệt.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">SmartWire-DT <span class="badge">EATON GATEWAY</span></div>
              <div class="node-name">PKE & NZM Breakers</div>
              <div class="node-desc">Truyền thông số dòng điện, nhiệt độ và quá tải động cơ về CPU.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">Dẫn đường Laser <span class="badge">NAVIGATION</span></div>
              <div class="node-name">TACS Target & Inclinometer mts</div>
              <div class="node-desc">Định vị tọa độ quang điện tâm gương và cảm biến đo độ nghiêng / xoắn thân máy.</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Diagram 2 -->
    <div class="diagram-section" id="diag2">
      <div class="diag-header">
        <h2>2. Sơ Đồ 14 Networks Của Khối OB1 (Execution Pipeline)</h2>
        <p>Thứ tự thực thi tuần tự của chương trình PLC trong mỗi vòng quét chu kỳ 10-15ms:</p>
      </div>

      <div class="pipe-timeline">
        <div class="pipe-step">
          <div class="pipe-num">NW 1-2</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Chẩn đoán mạng Profibus-DP & Trạng thái phần cứng</span>
              <div class="pipe-tags"><span class="pipe-tag">FC125</span><span class="pipe-tag">DB100</span><span class="pipe-tag">FB125</span></div>
            </div>
            <div class="pipe-desc">Quét toàn bộ 12 trạm DP (Turck BL20/BL67/SDPB) để phát hiện sự cố đứt cáp hoặc mất nguồn trạm trước khi chạy chương trình logic.</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 3</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Kiểm tra Liên động An toàn & Cờ lỗi hệ thống</span>
              <div class="pipe-tags"><span class="pipe-tag">FC61</span><span class="pipe-tag">FC71</span><span class="pipe-tag">FC72</span><span class="pipe-tag">FC76</span><span class="pipe-tag">FC126</span></div>
            </div>
            <div class="pipe-desc">Kiểm tra cờ Freigaben (FC61) gồm nút E-Stop, áp lực dầu, nhiệt độ và mức dầu. Xử lý xóa cờ lỗi (FC71/FC72) và cập nhật thời gian hệ thống (FC126).</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 4-5</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Khởi động Bộ nguồn Thủy lực & Động cơ Điện chính</span>
              <div class="pipe-tags"><span class="pipe-tag">FC30</span><span class="pipe-tag">FC33</span><span class="pipe-tag">FC13</span><span class="pipe-tag">FC14</span><span class="pipe-tag">FC15</span></div>
            </div>
            <div class="pipe-desc">Kích hoạt trạm nguồn thủy lực container (FC30), kiểm tra nghẹt lọc dầu (FC33), khởi động động cơ chính (FC13), motor đầu cắt (FC14) và motor bơm lái (FC15).</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 6-7</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Điều khiển Chiều quay & Tốc độ Đầu cắt Schürfrad</span>
              <div class="pipe-tags"><span class="pipe-tag">FC25</span><span class="pipe-tag">FC26</span><span class="pipe-tag">FC31</span><span class="pipe-tag">FC79</span><span class="pipe-tag">FC95</span></div>
            </div>
            <div class="pipe-desc">Điều khiển quay phải (CW) / quay trái (CCW), tăng giảm tốc độ vô cấp theo chiết áp HMI (DB21), mở kênh làm mát đầu cắt (FC79) và giám sát bộ nguồn đầu khiên (FC31).</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 8-9</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Nội suy Đặc tính Lưu lượng Bơm Thủy lực (CSG vs Rexroth A4VG)</span>
              <div class="pipe-tags"><span class="pipe-tag">FC4</span><span class="pipe-tag">FC5</span><span class="pipe-tag">FC140..142</span><span class="pipe-tag">FC150..152</span></div>
            </div>
            <div class="pipe-desc">Tự động chọn khối giải thuật theo cấu hình phần cứng: Nhánh bơm CSG 132kW (FC4/FC150) hoặc Nhánh bơm Rexroth A4VG 132kW (FC5/FC140) để xuất điện áp điều khiển van tỷ lệ.</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 10</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Điều khiển Trạm Kích chính & Các Trạm Kích Trung gian (Dehner 1..5)</span>
              <div class="pipe-tags"><span class="pipe-tag">FC20</span><span class="pipe-tag">FC21</span><span class="pipe-tag">FC22</span><span class="pipe-tag">FC55</span><span class="pipe-tag">FC56</span><span class="pipe-tag">FC57</span></div>
            </div>
            <div class="pipe-desc">Điều khiển áp suất và lưu lượng kích chính, quy đổi Bar sang Tấn (DB17), phối hợp đồng bộ chu trình đẩy từng đoạn giữa trạm kích chính và các trạm Dehner 1..5.</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 11</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Điều khiển Cụm Bơm Bùn Tuần hoàn (Spülpumpen)</span>
              <div class="pipe-tags"><span class="pipe-tag">FC10</span><span class="pipe-tag">FC11</span><span class="pipe-tag">FC12</span></div>
            </div>
            <div class="pipe-desc">Điều khiển bơm cấp dung dịch bùn bentonite (Charge pump FC10), bơm hút bùn thải ra ngoài giếng (Discharge pump FC11) và bơm tăng áp đường ống dài (Booster pump FC12).</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 12-13</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Định vị Laser TACS, Đo Góc Nghiêng & Lái hướng Khiên đào</span>
              <div class="pipe-tags"><span class="pipe-tag">FC38</span><span class="pipe-tag">FC39</span><span class="pipe-tag">FC40..42</span><span class="pipe-tag">FC43</span><span class="pipe-tag">FC80</span><span class="pipe-tag">FC81</span><span class="pipe-tag">FC82</span></div>
            </div>
            <div class="pipe-desc">Đọc tọa độ laser TACS (FC80), góc Pitch/Roll từ Inclinometer (FC81), điều khiển 3 xi lanh bẻ khớp lái 120° (FC40..42) và kích hoạt cánh chống xoay Wing (FC43) khi mô-men lớn.</div>
          </div>
        </div>

        <div class="pipe-step">
          <div class="pipe-num">NW 14</div>
          <div class="pipe-card">
            <div class="pipe-title">
              <span>Đo Chiều dài Hầm, Phun Nước Cao Áp 400 bar & Giao tiếp HMI</span>
              <div class="pipe-tags"><span class="pipe-tag">FC35</span><span class="pipe-tag">FC50</span><span class="pipe-tag">FC65</span><span class="pipe-tag">FC75</span><span class="pipe-tag">FC90</span><span class="pipe-tag">FC91</span></div>
            </div>
            <div class="pipe-desc">Đọc bánh xe đo chiều dài kích (FC35 &rarr; DB1), đếm giờ chạy máy (FC65), mở bơm cao áp 400 bar (FC90) và van béc phun tia nước mặt gương (FC91) để phá đất sét.</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Diagram 3 -->
    <div class="diagram-section" id="diag3">
      <div class="diag-header">
        <h2>3. Sơ Đồ Luồng Dữ Liệu & Bản Đồ Vùng Nhớ (Data Flow Diagram)</h2>
        <p>Cách các tín hiệu từ cảm biến trường đi qua các khối giải thuật và lưu trữ vào Data Blocks:</p>
      </div>

      <div class="flow-grid">
        <div class="flow-col">
          <div class="flow-col-title">1. Cảm biến & I/O Trường</div>
          <div class="flow-item">
            <strong>Bánh xe đo xung (Encoder)</strong>
            <span>Đo chiều dài kích Längenvortrieb</span>
          </div>
          <div class="flow-item">
            <strong>Cảm biến áp suất (PIW)</strong>
            <span>Đo áp suất kích chính, đầu cắt & xi lanh</span>
          </div>
          <div class="flow-item">
            <strong>Bia ngắm Laser TACS</strong>
            <span>Tọa độ quang học X, Y tâm gương</span>
          </div>
          <div class="flow-item">
            <strong>mts Inclinometer</strong>
            <span>Góc nghiêng Pitch & góc xoay Roll</span>
          </div>
          <div class="flow-item">
            <strong>Gateway SmartWire-DT</strong>
            <span>Dòng tải ampe động cơ PKE</span>
          </div>
        </div>

        <div class="flow-col">
          <div class="flow-col-title">2. Khối Xử lý & Giải thuật</div>
          <div class="flow-item">
            <strong>FC35 & FB100 (OB32)</strong>
            <span>Đếm xung & lọc mượt trung bình động</span>
          </div>
          <div class="flow-item">
            <strong>FC22 / FC17</strong>
            <span>Nội suy Bar sang Tấn (Druck-Tonnen)</span>
          </div>
          <div class="flow-item">
            <strong>FC80 / FC81 / FC82</strong>
            <span>Tính độ lệch tâm laser & bù xoắn Roll</span>
          </div>
          <div class="flow-item">
            <strong>FC40 / FC41 / FC42</strong>
            <span>Phân bố lực 3 xi lanh lái 120°</span>
          </div>
          <div class="flow-item">
            <strong>FC61 / FC72</strong>
            <span>Kiểm tra giới hạn an toàn & cờ lỗi</span>
          </div>
        </div>

        <div class="flow-col">
          <div class="flow-col-title">3. Vùng nhớ Data Blocks</div>
          <div class="flow-item">
            <strong>DB1 & DB102</strong>
            <span>Quãng đường (mm) & Vận tốc (mm/min)</span>
          </div>
          <div class="flow-item">
            <strong>DB16 & DB17</strong>
            <span>Áp suất Bar & Lực kích Tấn</span>
          </div>
          <div class="flow-item">
            <strong>DB59</strong>
            <span>Lực tì 3 xi lanh lái (Cyl_Force)</span>
          </div>
          <div class="flow-item">
            <strong>DB21</strong>
            <span>Tốc độ đặt RPM & giới hạn mô-men</span>
          </div>
          <div class="flow-item">
            <strong>DB58</strong>
            <span>Bảng bit cờ lỗi tổng (Fault_DB)</span>
          </div>
        </div>

        <div class="flow-col">
          <div class="flow-col-title">4. Hiển thị & Ghi Log</div>
          <div class="flow-item">
            <strong>DB19 (HMI Touchscreen)</strong>
            <span>Hiển thị đồ họa số liệu cho thợ lái hầm</span>
          </div>
          <div class="flow-item">
            <strong>DB19 Nút điều khiển</strong>
            <span>Đặt tốc độ, bẻ lái, phun nước cao áp</span>
          </div>
          <div class="flow-item">
            <strong>DB57 (DataLogging2)</strong>
            <span>Ghi nhật ký khoan định kỳ theo giây</span>
          </div>
          <div class="flow-item">
            <strong>Cảnh báo Âm thanh / Đèn</strong>
            <span>Kích hoạt còi báo khi DB58 có lỗi</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Diagram 4 -->
    <div class="diagram-section" id="diag4">
      <div class="diag-header">
        <h2>4. Sơ Đồ Máy Trạng Thái & Liên Động An Toàn (Safety State Machine)</h2>
        <p>Logic bảo vệ trong FC61 (Freigaben), FC76 (DP Ausfall) và OB86 ngăn chặn sự cố hư hỏng máy:</p>
      </div>

      <div class="state-grid">
        <div class="state-card">
          <div class="state-title">
            <span>1. Khởi tạo & Kiểm tra Bus</span>
            <span class="tag" style="background: rgba(0, 210, 255, 0.2); color: var(--accent-cyan);">INIT</span>
          </div>
          <div class="state-body">
            Khi bật nguồn (OB100):
            <ul class="state-list">
              <li>FC125 quét 12 trạm Profibus DP (Turck BL20, BL67, SDPB).</li>
              <li>Nếu mất 1 trạm &rarr; OB86 kích hoạt chặn CPU STOP mode &rarr; Bật cờ lỗi rớt trạm trong DB58.</li>
              <li>Kiểm tra đường truyền SmartWire-DT đến các bộ PKE.</li>
            </ul>
          </div>
        </div>

        <div class="state-card active-state">
          <div class="state-title">
            <span>2. Kiểm tra Liên động FC61</span>
            <span class="tag" style="background: rgba(0, 230, 118, 0.2); color: var(--accent-green);">FREIGABEN</span>
          </div>
          <div class="state-body">
            FC61 cấp cờ M61.0 (Cho phép chạy) khi thỏa mãn:
            <ul class="state-list">
              <li>Nút dừng khẩn cấp (E-Stop) không bị nhấn.</li>
              <li>Mức dầu thủy lực bể chứa container & đầu khiên đạt chuẩn.</li>
              <li>Nhiệt độ dầu < 70°C, áp suất lọc không bị nghẹt.</li>
              <li>Không có động cơ nào bị quá tải dòng điện PKE.</li>
            </ul>
          </div>
        </div>

        <div class="state-card danger-state">
          <div class="state-title">
            <span>3. Dừng An toàn & Hãm Sự cố</span>
            <span class="tag" style="background: rgba(255, 61, 113, 0.2); color: var(--accent-red);">SHUTDOWN</span>
          </div>
          <div class="state-body">
            Khi phát hiện lỗi bất kỳ:
            <ul class="state-list">
              <li>Ngắt tức thì cờ Freigabe M61.0.</li>
              <li>Đóng toàn bộ van tỷ lệ kích chính và kích trung gian.</li>
              <li>Dừng động cơ cắt Schürfrad và ngắt bơm cao áp 400 bar.</li>
              <li>Lưu mã lỗi chi tiết vào DB58 để thợ vận hành khắc phục.</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  
    <!-- Diagram 5 -->
    <div class="diagram-section" id="diag5">
      <div class="diag-header">
        <h2>5. Sơ Đồ 5 Phân Hệ Bản Vẽ Điện Chuẩn MTS Perforator (DIN EN 61346-1)</h2>
        <p>Toàn bộ linh kiện, khí cụ điện và cơ cấu chấp hành trên TBM Bohrkopf 2100 được phân định theo 5 vùng không gian vật lý độc lập với mã vị trí chuẩn:</p>
      </div>

      <div class="arch-layer-container">
        <!-- Phân hệ =bk -->
        <div class="arch-layer" style="border-left: 4px solid #00d2ff; background: rgba(0, 210, 255, 0.04);">
          <div class="arch-layer-title" style="color: #00d2ff;">🚜 PHÂN HỆ =bk : KHOANG ĐẦU KHOAN KHIÊN ĐÀO (BOHRKOPF - IP68)</div>
          <p style="font-size: 0.8rem; color: #8a9fc4; margin-bottom: 12px;">Khu vực làm việc dưới áp lực đất và ngập nước bùn bentonite (5 - 10 bar). Toàn bộ thiết bị đạt chuẩn IP67/IP68.</p>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">-M15.1 <span class="badge">132kW/160kW</span></div>
              <div class="node-name">Động Cơ Đĩa Cắt Schürfrad</div>
              <div class="node-desc">Động cơ điện/thủy lực kéo mâm cào đất đá, tích hợp cảm biến nhiệt độ cuộn dây PTC và phớt chặn bùn chịu áp.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K50 .. -K55 <span class="badge">SOLENOID 24V</span></div>
              <div class="node-name">Cụm Van Xilanh Lái 120°</div>
              <div class="node-desc">6 cuộn coil van trượt thủy lực điều khiển thò/thụt 3 xilanh lái đỉnh 0°, phải 120°, trái 240° theo tim laser TACS.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K58 .. -K61 <span class="badge">VAN MÀNG</span></div>
              <div class="node-name">Cụm Van Bypass & Van Nước Jet</div>
              <div class="node-desc">Van màng điện từ đóng mở đường bùn hồi bypass (-K58/-K59) và van béc xối nước mặt gương (-K60/-K61).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-B57 & -B1 <span class="badge">SENSORS IP68</span></div>
              <div class="node-name">Đo Nghiêng & Đếm Xung Tốc Độ</div>
              <div class="node-desc">Cảm biến Seika NG3I đo góc dốc Pitch / xoay Roll 2 trục (-B57) và cảm biến tiệm cận đếm răng mâm cắt RPM (-B1).</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K1.1 .. -K2.3 <span class="badge">TURCK PICONET</span></div>
              <div class="node-name">Trạm I/O Phân Tán Đầu Khiên</div>
              <div class="node-desc">7 hộp đúc nhôm nguyên khối IP68 thu thập 12 kênh analog, 8 kênh digital và bộ đếm counter xung tốc độ cao.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-P10.1 & -E9.1 <span class="badge">VIDEO & LED</span></div>
              <div class="node-name">Camera Quan Sát & Đèn Gương Đào</div>
              <div class="node-desc">Camera màu góc rộng 115° chịu ngập nước sâu kèm đèn LED 24VDC soi qua kính thạch anh buồng đào.</div>
            </div>
          </div>
        </div>

        <!-- Phân hệ =ppm -->
        <div class="arch-layer" style="border-left: 4px solid #00e676; background: rgba(0, 230, 118, 0.04); margin-top: 16px;">
          <div class="arch-layer-title" style="color: #00e676;">⚡ PHÂN HỆ =ppm : TỦ ĐIỆN ĐỘNG LỰC CHÍNH (POWER PLANT MAIN - IP54)</div>
          <p style="font-size: 0.8rem; color: #8a9fc4; margin-bottom: 12px;">Tủ điện container trung tâm đặt trên mặt đất / miệng giếng, tiếp nhận nguồn 3 pha 400V/690V và chứa CPU điều khiển PLC.</p>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">-K22.1 <span class="badge">CPU 315-2 PN/DP</span></div>
              <div class="node-name">Bộ Vi Xử Lý Trung Tâm PLC</div>
              <div class="node-desc">Siemens S7-300 quản lý 12 trạm Profibus-DP, vòng quét 10ms xử lý toàn bộ 14 Networks công nghệ và ngắt OB86.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-Q1.1 .. -Q1.3 <span class="badge">MCCB 250A/630A</span></div>
              <div class="node-name">Aptomat Khối Động Lực Eaton NZM</div>
              <div class="node-desc">Bảo vệ ngắn mạch và quá tải cho toàn bộ động cơ bơm chính, tích hợp khối truyền thông đo dòng điện 3 pha và kWh.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K22.3 <span class="badge">SWD GATEWAY</span></div>
              <div class="node-name">Bộ Gateway Eaton SmartWire-DT</div>
              <div class="node-desc">Cầu nối chuyển đổi mạng Profibus sang đường cáp dẹt 8 lõi kết nối các bộ rơ le khởi động động cơ PKE và nút ấn.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-T15.1 <span class="badge">SOFTSTARTER</span></div>
              <div class="node-name">Bộ Khởi Động Mềm Mâm Cắt</div>
              <div class="node-desc">Hạn chế dòng khởi động và giật xung mô-men xoắn khi bắt đầu quay mâm cào đất trong điều kiện bùn sét nặng.</div>
            </div>
          </div>
        </div>

        <!-- Phân hệ =pph -->
        <div class="arch-layer" style="border-left: 4px solid #ff9100; background: rgba(255, 145, 0, 0.04); margin-top: 16px;">
          <div class="arch-layer-title" style="color: #ff9100;">🛢️ PHÂN HỆ =pph : TRẠM NGUỒN THỦY LỰC (HYDRAULIC POWER PACK CONTAINER)</div>
          <p style="font-size: 0.8rem; color: #8a9fc4; margin-bottom: 12px;">Container thủy lực chứa bồn dầu 2000L, cụm bơm piston Rexroth A4VG/A10VSO và giàn van phân phối tỷ lệ.</p>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">-B10 & -B1/-B2 <span class="badge">BỒN DẦU 2000L</span></div>
              <div class="node-name">Giám Sát Nhiệt Độ, Độ Ẩm & Mức Dầu</div>
              <div class="node-desc">Cảm biến Hydac ETS 4144 đo T° dầu (<70°C), AquaSensor đo độ ẩm rò rỉ nước (% rH) và phao ngắt an toàn cạn dầu E12.0.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K5.1 .. -K5.3 <span class="badge">AMPLIFIER OBE</span></div>
              <div class="node-name">Card Khuếch Đại Van Tỷ Lệ Rexroth</div>
              <div class="node-desc">VT-11131, VT-11118 và VT-MSPA1 biến đổi 0-10VDC (AW264..AW268) điều khiển vô cấp vận tốc, áp suất kích và đĩa cắt.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-K1.1 .. -K2.7 <span class="badge">TURCK BL67</span></div>
              <div class="node-name">Trạm I/O Thu Thập Thủy Lực</div>
              <div class="node-desc">Trạm Profibus DP-Adr 5 đọc chênh áp lọc bypass/hồi, áp suất nạp, áp suất kích nén EW406 và điều khiển van solenoid kích tiến/lùi.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-B5 (EW406) <span class="badge">0 - 400 BAR</span></div>
              <div class="node-name">Cảm Biến Áp Lực Trạm Kích Chính</div>
              <div class="node-desc">Đo áp nén của 4 xilanh đẩy cống đáy giếng, quy đổi tức thời ra Lực Tấn (tấn = bar × 0.24543) bảo vệ chống vỡ cống bê tông.</div>
            </div>
          </div>
        </div>

        <!-- Phân hệ =cc -->
        <div class="arch-layer" style="border-left: 4px solid #9d4edd; background: rgba(157, 78, 221, 0.04); margin-top: 16px;">
          <div class="arch-layer-title" style="color: #9d4edd;">🎮 PHÂN HỆ =cc : CABIN ĐIỀU KHIỂN TRUNG TÂM (CONTROL CABIN)</div>
          <p style="font-size: 0.8rem; color: #8a9fc4; margin-bottom: 12px;">Phòng vận hành tiện nghi có điều hòa nhiệt độ, nơi đặt bàn điều khiển thực tế và màn hình SCADA VisAM.</p>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">Figure 3.5 <span class="badge">33 PHẦN TỬ</span></div>
              <div class="node-name">Bàn Điều Khiển Thực Tế 5 Hàng</div>
              <div class="node-desc">19 nút ấn có đèn LED SmartWire, 8 chiết áp điều tốc/áp lực, 5 công tắc gạt xilanh lái & chế độ vận hành, 1 nắp chờ dummy.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">VisAM IPC <span class="badge">SCADA SYSTEM</span></div>
              <div class="node-name">Máy Tính Giám Sát Công Nghiệp</div>
              <div class="node-desc">Màn hình SCADA thời gian thực kết nối mạng Ethernet Industrial với CPU S7-300, trực quan hóa van, lưu lượng bùn và lực kích.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">TACS Target <span class="badge">LASER GUIDANCE</span></div>
              <div class="node-name">Màn Hình Định Vị Dẫn Đường Laser</div>
              <div class="node-desc">Hiển thị tọa độ tâm ngắm laser đỏ, độ lệch phương dọc (Dev V), lệch phương ngang (Dev H) và góc xoay khiên Roll.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-S1.1 <span class="badge">E-STOP NC</span></div>
              <div class="node-name">Nút Nhấn Dừng Khẩn Cấp Trung Tâm</div>
              <div class="node-desc">Tiếp điểm an toàn cứng cắt nguồn điều khiển lập tức qua rơ le an toàn, khóa cờ liên động M61.0 dừng mọi chuyển động.</div>
            </div>
          </div>
        </div>

        <!-- Phân hệ =hz -->
        <div class="arch-layer" style="border-left: 4px solid #ff3d71; background: rgba(255, 61, 113, 0.04); margin-top: 16px;">
          <div class="arch-layer-title" style="color: #ff3d71;">❄️ PHÂN HỆ =hz : HỆ THỐNG PHỤ TRỢ, SẤY & ĐIỀU HÒA (AUXILIARY & HEATING)</div>
          <p style="font-size: 0.8rem; color: #8a9fc4; margin-bottom: 12px;">Đảm bảo điều kiện môi trường nhiệt độ, độ ẩm ổn định cho tủ điện và bồn dầu làm việc liên tục 24/7.</p>
          <div class="arch-grid">
            <div class="arch-node">
              <div class="node-id">-E15.1 <span class="badge">SẤY DẦU</span></div>
              <div class="node-name">Thanh Điện Trở Sấy Dầu Bồn Chứa</div>
              <div class="node-desc">Tự động kích hoạt khi mùa đông nhiệt độ dầu thủy lực < 15°C để nâng độ nhớt dầu lên mức làm việc an toàn.</div>
            </div>
            <div class="arch-node">
              <div class="node-id">-E1.1 <span class="badge">ĐIỀU HÒA TỦ</span></div>
              <div class="node-name">Máy Làm Mát Tủ Điện Chính =ppm</div>
              <div class="node-desc">Duy trì nhiệt độ bên trong tủ biến tần và PLC ở mức lý tưởng 25 - 32°C, chống ngưng tụ sương ẩm làm chập vi mạch.</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Diagram 6 -->
    <div class="diagram-section" id="diag6">
      <div class="diag-header">
        <h2>6. Cấu Trúc Mạng Truyền Thông Profibus-DP & SmartWire-DT (12 Trạm Slaves)</h2>
        <p>Kiến trúc bus trường phân tán nối dài từ trạm mặt đất xuống hầm khoan sâu, tốc độ truyền dẫn 1.5 - 12 Mbps chống nhiễu công nghiệp:</p>
      </div>

      <div class="pipe-container" style="padding: 16px; background: rgba(0,0,0,0.3); border-radius: 10px;">
        <div style="font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; color: #00d2ff; margin-bottom: 14px; font-weight: bold;">
          [SIEMENS CPU 315-2 PN/DP MASTER] &lt;== Cáp Tím Đôi Xoắn Profibus-DP (Chân A Xanh lá, Chân B Đỏ) ==&gt;
        </div>

        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 12px;">
          <!-- Node 1 -->
          <div class="arch-node" style="border-left: 3px solid #00d2ff;">
            <div class="node-id">DP-Adr 1 <span class="badge">SWD GATEWAY</span></div>
            <div class="node-name">Eaton EU5C-SWD-DP (=ppm)</div>
            <div class="node-desc">Quản lý mạng cáp dẹt SmartWire-DT kết nối 33 nút bấm/chiết áp bàn điều khiển =cc và rơ le động cơ PKE.</div>
          </div>
          <!-- Node 5 -->
          <div class="arch-node" style="border-left: 3px solid #00e676;">
            <div class="node-id">DP-Adr 5 <span class="badge">TURCK BL67</span></div>
            <div class="node-name">Trạm Thủy Lực Container (=pph)</div>
            <div class="node-desc">Gateway BL67-GW-DP quản lý 2 module 4AI (áp suất kích, đĩa cắt, nhiệt độ), 2 module 2AO (van tỷ lệ) và 4 module 4DO.</div>
          </div>
          <!-- Node 7 -->
          <div class="arch-node" style="border-left: 3px solid #00e676;">
            <div class="node-id">DP-Adr 7 <span class="badge">TURCK BL20</span></div>
            <div class="node-name">Trạm Tủ Điện Chính (=ppm)</div>
            <div class="node-desc">Gateway BL20-GW-DP thu thập tín hiệu logic tủ điện, rơ le an toàn, cảnh báo nguồn và trạng thái quạt làm mát.</div>
          </div>
          <!-- Node 10 & 11 -->
          <div class="arch-node" style="border-left: 3px solid #ff9100;">
            <div class="node-id">DP-Adr 10 &amp; 11 <span class="badge">ALTIVAR VFD</span></div>
            <div class="node-name">Biến Tần Bơm Bùn Nạp &amp; Bơm Xả</div>
            <div class="node-desc">Schneider ATV71 điều khiển động cơ bơm bùn 37kW/45kW qua lệnh Profibus 000F Hex, phản hồi tần số và dòng tải Ampe.</div>
          </div>
          <!-- Node 20 & 21 -->
          <div class="arch-node" style="border-left: 3px solid #9d4edd;">
            <div class="node-id">DP-Adr 20 &amp; 21 <span class="badge">PICONET IP68</span></div>
            <div class="node-name">-K1.4 (DO) &amp; -K1.3 (4AI) (=bk)</div>
            <div class="node-desc">-K1.4 kích đèn LED soi gương hầm A51.0; -K1.3 nhận cảm biến xilanh tiến 1, 2, 3 và áp suất lái tổng EW362.</div>
          </div>
          <!-- Node 22 & 23 -->
          <div class="arch-node" style="border-left: 3px solid #9d4edd;">
            <div class="node-id">DP-Adr 22 &amp; 23 <span class="badge">PICONET IP68</span></div>
            <div class="node-name">-K1.2 (4AI) &amp; -K1.1 (4AI) (=bk)</div>
            <div class="node-desc">-K1.2 đọc áp xilanh lái 1..3 và Inclinometer EW370; -K1.1 đọc áp buồng đào EW374, áp nạp EW376, áp mâm cắt và rò phớt.</div>
          </div>
          <!-- Node 24 & 25 -->
          <div class="arch-node" style="border-left: 3px solid #ff3d71;">
            <div class="node-id">DP-Adr 24 &amp; 25 <span class="badge">PICONET DO/DI</span></div>
            <div class="node-name">-K2.2 (4DI/4DO) &amp; -K2.3 (8DO) (=bk)</div>
            <div class="node-desc">-K2.2 điều khiển van &amp; đọc LS Bypass/Jet; -K2.3 xuất lệnh kích 6 van xilanh lái 120° (A53.0..A53.5) và xilanh khóa.</div>
          </div>
          <!-- Node 30 -->
          <div class="arch-node" style="border-left: 3px solid #ff3d71;">
            <div class="node-id">DP-Adr 30 <span class="badge">COUNTER 2-CH</span></div>
            <div class="node-name">-K2.1 Bộ Đếm Xung Tốc Độ Cao (=bk)</div>
            <div class="node-desc">Đọc tần số xung từ cảm biến tiệm cận từ -B1 đếm số răng đĩa xung, tính toán tức thời tốc độ vòng quay RPM đĩa cắt.</div>
          </div>
        </div>

        <div style="margin-top: 14px; padding: 10px; background: rgba(0, 210, 255, 0.06); border-radius: 6px; font-size: 0.8rem; color: #8a9fc4;">
          <strong style="color: #00d2ff;">📌 Cơ chế chống STOP CPU khi đứt cáp:</strong> Khối tổ chức <code>OB86</code> (Rack/Station Fault) và <code>OB122</code> (I/O Access Error) được nạp vào CPU 315-2. Khi có module đứt cáp hoặc rớt mạng dưới hầm, CPU ghi cờ vào <code>DB58</code> và vẫn ở chế độ RUN an toàn để thợ tiếp tục xử lý sự cố.
        </div>
      </div>
    </div>

    <!-- Diagram 7 -->
    <div class="diagram-section" id="diag7">
      <div class="diag-header">
        <h2>7. Quy Chuẩn Mã Màu Dây &amp; Ký Hiệu Cáp Công Nghiệp (DIN EN 60204-1)</h2>
        <p>Quy chuẩn quốc tế về màu dây điện áp và ký hiệu đầu cáp trong hồ sơ mạch điện máy khoan ngầm MTS 2018221:</p>
      </div>

      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(360px, 1fr)); gap: 20px;">
        <!-- Bảng màu dây -->
        <div class="card" style="margin-bottom: 0; background: #0f1523;">
          <div class="card-title"><span>BẢNG MÃ MÀU DÂY TIÊU CHUẨN (DIN EN 60204-1)</span></div>
          <div class="table-responsive">
          <table style="width: 100%; font-size: 0.8rem; border-collapse: collapse;">
            <thead>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.1); color: #00d2ff; text-align: left;">
                <th style="padding: 8px;">Màu Dây</th>
                <th style="padding: 8px;">Cấp Điện Áp &amp; Chức Năng</th>
                <th style="padding: 8px;">Ứng Dụng Thực Tế TBM</th>
              </tr>
            </thead>
            <tbody>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#000;border:1px solid #444;border-radius:2px;margin-right:6px;"></span>Đen (Black)</td>
                <td style="padding: 8px;">AC Động Lực 3 Pha (L1)</td>
                <td style="padding: 8px;">Pha 1 cấp động cơ 132kW, bơm nguồn</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#8b4513;border-radius:2px;margin-right:6px;"></span>Nâu (Brown)</td>
                <td style="padding: 8px;">AC Động Lực 3 Pha (L2)</td>
                <td style="padding: 8px;">Pha 2 cấp nguồn động lực 400V</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#888;border-radius:2px;margin-right:6px;"></span>Xám (Grey)</td>
                <td style="padding: 8px;">AC Động Lực 3 Pha (L3)</td>
                <td style="padding: 8px;">Pha 3 cấp nguồn động lực 400V</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#00bcd4;border-radius:2px;margin-right:6px;"></span>Xanh Dương Nhạt (Light Blue)</td>
                <td style="padding: 8px;">Dây Trung Tính (Neutral - N)</td>
                <td style="padding: 8px;">Dây N lưới điện 230VAC cấp sấy, đèn</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:linear-gradient(135deg, #00e676 50%, #ffeb3b 50%);border-radius:2px;margin-right:6px;"></span>Vàng - Xanh Lá (Green-Yellow)</td>
                <td style="padding: 8px;">Tiếp Địa Bảo Vệ (PE)</td>
                <td style="padding: 8px;">Nối đất vỏ khiên khiên đào và khung bồn</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#0033aa;border-radius:2px;margin-right:6px;"></span>Xanh Đậm (Dark Blue)</td>
                <td style="padding: 8px;">DC Điều Khiển (+24VDC)</td>
                <td style="padding: 8px;">Nguồn nuôi cảm biến, cuộn coil, PLC I/O</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#fff;border:1px solid #0033aa;border-radius:2px;margin-right:6px;"></span>Xanh Sọc Trắng (Blue-White)</td>
                <td style="padding: 8px;">DC Mass Chung (0VDC / GND)</td>
                <td style="padding: 8px;">Đường về 0VDC của nguồn SITOP 24V/20A</td>
              </tr>
              <tr>
                <td style="padding: 8px;"><span style="display:inline-block;width:12px;height:12px;background:#ff9100;border-radius:2px;margin-right:6px;"></span>Cam (Orange)</td>
                <td style="padding: 8px;">Tiếp Điểm Khóa Ngoại Vi</td>
                <td style="padding: 8px;">Mạch khóa liên động có điện khi ngắt tủ</td>
              </tr>
            </tbody>
          </table>
        </div>
        </div>

        <!-- Bảng quy chuẩn cáp & trâm kẹp -->
        <div class="card" style="margin-bottom: 0; background: #0f1523;">
          <div class="card-title"><span>QUY CHUẨN CÁP &amp; TRÂM KẸP (TERMINAL STRIPS)</span></div>
          <div class="table-responsive">
          <table style="width: 100%; font-size: 0.8rem; border-collapse: collapse;">
            <thead>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.1); color: #00d2ff; text-align: left;">
                <th style="padding: 8px;">Ký Hiệu Cụm</th>
                <th style="padding: 8px;">Vị Trí &amp; Chức Năng</th>
                <th style="padding: 8px;">Đặc Điểm Cáp Đấu Nối</th>
              </tr>
            </thead>
            <tbody>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #00e676;">=ppm-X1</td>
                <td style="padding: 8px;">Cầu đấu động lực 400V chính</td>
                <td style="padding: 8px;">Cáp 3x120mm² + 70mm² bọc cao su chịu tải</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #00e676;">=ppm-X5</td>
                <td style="padding: 8px;">Cầu đấu tín hiệu điều khiển 24VDC</td>
                <td style="padding: 8px;">Cáp nhiều lõi đánh số thứ tự 1..36</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #00e676;">=ppm-X7</td>
                <td style="padding: 8px;">Cầu đấu cáp Bus Profibus DP</td>
                <td style="padding: 8px;">Cáp tím chống nhiễu chuyên dụng có bọc giáp</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #00e676;">=ppm-X8</td>
                <td style="padding: 8px;">Cầu đấu SmartWire-DT</td>
                <td style="padding: 8px;">Cáp dẹt 8 sợi SWD kèm đầu bấm kẹp kim</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #ff9100;">=pph-X10</td>
                <td style="padding: 8px;">Cụm giắc van trạm nguồn thủy lực</td>
                <td style="padding: 8px;">Cáp xoắn đôi chống nhiễu LiYCY 4x0.75mm²</td>
              </tr>
              <tr style="border-bottom: 1px solid rgba(255,255,255,0.04);">
                <td style="padding: 8px; font-weight: bold; color: #00d2ff;">=bk-X1..X11</td>
                <td style="padding: 8px;">Giắc cắm tròn M12 chống nước IP68</td>
                <td style="padding: 8px;">Đúc sẵn keo PUR chống dầu mỡ và ma sát</td>
              </tr>
              <tr>
                <td style="padding: 8px; font-weight: bold; color: #9d4edd;">Schottleiste</td>
                <td style="padding: 8px;">Bảng vách nối cáp xuyên container</td>
                <td style="padding: 8px;">Ốc siết Skintop M16/M20/M25 kèm đệm cao su</td>
              </tr>
            </tbody>
          </table>
        </div>
        </div>
      </div>
    </div>

  </div>

  <!-- ========================================================
       TAB 3: COMPONENTS & HARDWARE SPECIFICATIONS (COMPREHENSIVE)
       ======================================================== -->
    <!-- ========================================================
       TAB 3: COMPONENTS & HARDWARE SPECIFICATIONS (118 VERIFIED ITEMS)
       ======================================================== -->
  <div id="tab-comp" class="tab-content">
    <div class="card">
      <div class="card-title">
        <span>DANH MỤC TOÀN BỘ 118 LINH KIỆN, CẢM BIẾN, KHÍ CỤ ĐIỆN & ĐỘNG CƠ TBM CHUẨN BẢN VẼ</span>
        <span class="tag">MASTER BILL OF MATERIALS (BOM)</span>
      </div>

      <p style="font-size: 0.82rem; color: var(--text-muted); margin-bottom: 14px;">
        Trích xuất 100% đầy đủ từ hồ sơ thiết kế điện chính hãng <strong>MTS Perforator GmbH</strong> (Bản vẽ 2018221-BK, 2018221-E-Box 24V và 2018221-VC). Đầy đủ mã đặt hàng mts-Nr., phân hệ, địa chỉ PLC, hãng sản xuất và chức năng kỹ thuật:
      </p>

      <!-- Search & Filter Bar (Subsystems + Types) -->
      <div class="comp-toolbar" style="flex-direction: column; align-items: stretch; gap: 10px;">
        <div class="comp-filter-bar" style="flex-wrap: wrap;">
          <span style="font-size: 0.75rem; color: var(--text-muted); font-weight: 700; align-self: center; margin-right: 4px;">PHÂN HỆ:</span>
          <button class="comp-filter-btn active" id="btnSubAll" onclick="filterBySub('all')">Tất cả (All)</button>
          <button class="comp-filter-btn" id="btnSubBK" onclick="filterBySub('=bk')">=bk Đầu khiên</button>
          <button class="comp-filter-btn" id="btnSubPPM" onclick="filterBySub('=ppm')">=ppm Tủ điện chính</button>
          <button class="comp-filter-btn" id="btnSubPPH" onclick="filterBySub('=pph')">=pph Nguồn thủy lực</button>
          <button class="comp-filter-btn" id="btnSubCC" onclick="filterBySub('=cc')">=cc Bàn điều khiển</button>
          <button class="comp-filter-btn" id="btnSubEBox" onclick="filterBySub('E-Box')">E-Box & Mặt bích</button>
          <button class="comp-filter-btn" id="btnSubHZ" onclick="filterBySub('=hz')">=hz Sưởi/Đèn</button>
        </div>

        <div style="display: flex; justify-content: space-between; gap: 12px; align-items: center; flex-wrap: wrap;">
          <div class="comp-filter-bar" style="flex-wrap: wrap;">
            <span style="font-size: 0.75rem; color: var(--text-muted); font-weight: 700; align-self: center; margin-right: 4px;">LOẠI:</span>
            <button class="comp-filter-btn active" id="btnCatAll" onclick="filterComponents('all')">Tất cả</button>
            <button class="comp-filter-btn" id="btnCatSensor" onclick="filterComponents('sensor')">📡 Cảm biến</button>
            <button class="comp-filter-btn" id="btnCatActuator" onclick="filterComponents('actuator')">🚰 Van / Chấp hành</button>
            <button class="comp-filter-btn" id="btnCatDrive" onclick="filterComponents('drive')">⚡ Động cơ & Biến tần</button>
            <button class="comp-filter-btn" id="btnCatHW" onclick="filterComponents('hw')">🔌 Module & Thiết bị</button>
          </div>
          <input type="text" id="compSearchInput" class="search-box" style="width: 320px;" placeholder="🔍 Tìm kiếm mã MTS, BMK, tên thiết bị..." onkeyup="searchComponents()">
        </div>
      </div>

      <div class="table-responsive">
          <table class="plc-table" id="compTable" style="font-size: 0.78rem;">
        <thead>
          <tr>
            <th style="width: 100px;">Phân loại</th>
            <th style="width: 110px;">Địa chỉ PLC</th>
            <th style="width: 100px;">Ký hiệu BMK</th>
            <th style="width: 90px;">Mã mts-Nr.</th>
            <th style="width: 90px;">Phân hệ</th>
            <th style="width: 280px;">Tên thiết bị & Thông số kỹ thuật</th>
            <th>Mô tả chi tiết chức năng bản vẽ điện</th>
          </tr>
        </thead>
        <tbody id="compTableBody">
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW372</code></td>
            <td><strong>-B61</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P case drain (Áp suất khoang rò)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-10 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo áp suất đường dầu rò vỏ động cơ thủy lực đầu cắt (Case drain) bảo vệ phớt chặn.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW374</code></td>
            <td><strong>-B58</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P slurry chamber (Áp lực khoang đào bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-10 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất trực tiếp trong khoang bùn cân bằng gương đào (Slurry chamber balance).</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW376</code></td>
            <td><strong>-B59</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P charge line (Áp suất đường nạp bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-16 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất đường ống cấp vữa/bùn bentonite vào đầu khiên.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW378</code></td>
            <td><strong>-B60</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P cutter (Áp suất thủy lực đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-350 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo áp lực dầu thủy lực cấp cho cụm mô tơ quay đĩa cắt trên đầu khiên.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW364</code></td>
            <td><strong>-B54</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P steer. cyl. 1 (Áp suất xi lanh bẻ lái 1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-350 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất buồng đẩy xi lanh lái số 1 (góc 12 giờ).</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW366</code></td>
            <td><strong>-B55</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P steer. cyl. 2 (Áp suất xi lanh bẻ lái 2)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-350 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất buồng đẩy xi lanh lái số 2 (góc 4 giờ).</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW368</code></td>
            <td><strong>-B56</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P steer. cyl. 3 (Áp suất xi lanh bẻ lái 3)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-350 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất buồng đẩy xi lanh lái số 3 (góc 8 giờ).</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW370</code></td>
            <td><strong>-B57</strong></td>
            <td><code>115231</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Inclinometer mts (Cảm biến đo nghiêng/xoắn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">2 trục (Roll/Pitch) +/-60°, 4-20mA | MTS Sensors (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo góc nghiêng dọc và góc xoắn thân khiên đào để tính toán bù lệch hướng.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW356</code></td>
            <td><strong>-B50</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Headway cyl 1 (Hành trình xi lanh lái 1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-200 mm / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo độ thò thụt hành trình xi lanh định hướng số 1.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW358</code></td>
            <td><strong>-B51</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Headway cyl 2 (Hành trình xi lanh lái 2)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-200 mm / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo độ thò thụt hành trình xi lanh định hướng số 2.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW360</code></td>
            <td><strong>-B52</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Headway cyl 3 (Hành trình xi lanh lái 3)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-200 mm / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến đo độ thò thụt hành trình xi lanh định hướng số 3.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW362</code></td>
            <td><strong>-B53</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>P steering (Áp lực mạch nguồn bẻ lái)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-250 bar / 4-20mA, M12 | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất tổng mạch thủy lực bẻ lái đầu khiên.</td>
          </tr>
          <tr class="row-sensor" data-sub="=bk" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW478 / EW483</code></td>
            <td><strong>-B20</strong></td>
            <td><code>403938</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>RPM sensor (Cảm biến tốc độ vòng quay cutter)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bộ đếm xung Encoder 2 kênh A/B, 24VDC | Turck / IFM (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo xung tốc độ quay đĩa cắt Schürfrad, đưa vào bộ đếm high-speed counter -K2.1.</td>
          </tr>
          <tr class="row-di" data-sub="=bk" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>E52.0 / E52.1</code></td>
            <td><strong>-B62</strong></td>
            <td><code>340807</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>LS by-pass (Công tắc hành trình van by-pass)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">2x SPDT (Open E52.0 / Closed E52.1) | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Báo vị trí Mở hoàn toàn (E52.0) và Đóng hoàn toàn (E52.1) của van luân chuyển bùn by-pass.</td>
          </tr>
          <tr class="row-di" data-sub="=bk" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>E52.2 / E52.3</code></td>
            <td><strong>-B64</strong></td>
            <td><code>340807</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>LS jet (Công tắc hành trình van béc phun)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">2x SPDT (Open E52.2 / Closed E52.3) | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Báo vị trí Mở hoàn toàn (E52.2) và Đóng hoàn toàn (E52.3) của van béc phun nước áp lực cao.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A52.0 / A52.1</code></td>
            <td><strong>-K58 / -K59</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Valve by-pass (Cuộn hút van by-pass Open/Close)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van thủy lực 24VDC, DIN Type A 150mm | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A52.0 điều khiển mở van By-pass, A52.1 điều khiển đóng van By-pass qua module -K2.2.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A52.2 / A52.3</code></td>
            <td><strong>-K60 / -K61</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Valve jet (Cuộn hút van béc phun Open/Close)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van thủy lực 24VDC, DIN Type A 150mm | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A52.2 điều khiển mở van béc phun xối bùn, A52.3 điều khiển đóng van qua module -K2.2.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A53.0 / A53.1</code></td>
            <td><strong>-K50 / -K51</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Steer.cyl. 1 (Van lái xi lanh 1 Out/In)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van điện từ đảo chiều 24VDC 2A | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A53.0 đẩy xi lanh 1 duỗi ra (out), A53.1 kéo xi lanh 1 thụt vào (in) qua module -K2.3.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A53.2 / A53.3</code></td>
            <td><strong>-K52 / -K53</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Steer.cyl. 2 (Van lái xi lanh 2 Out/In)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van điện từ đảo chiều 24VDC 2A | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A53.2 đẩy xi lanh 2 duỗi ra (out), A53.3 kéo xi lanh 2 thụt vào (in) qua module -K2.3.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A53.4 / A53.5</code></td>
            <td><strong>-K54 / -K55</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Steer.cyl. 3 (Van lái xi lanh 3 Out/In)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van điện từ đảo chiều 24VDC 2A | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A53.4 đẩy xi lanh 3 duỗi ra (out), A53.5 kéo xi lanh 3 thụt vào (in) qua module -K2.3.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A53.6 / A53.7</code></td>
            <td><strong>-K56 / -K57</strong></td>
            <td><code>25011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Lock. cyl. (Van xi lanh khóa đầu khiên Out/In)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van điện từ đảo chiều 24VDC 2A | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A53.6 khóa khớp đầu khiên (Lock out), A53.7 mở khóa khớp (Lock in) qua module -K2.3.</td>
          </tr>
          <tr class="row-actuator" data-sub="=bk" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A51.0</code></td>
            <td><strong>-E9.1</strong></td>
            <td><code>406775</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Lamp head (Đèn LED chiếu sáng đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Đèn pha LED công nghiệp chống nước 24VDC | MTS Perforator (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Chiếu sáng khu vực trước gương đào phục vụ camera quan sát, điều khiển đóng cắt bởi A51.0.</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Coaxial / BNC</code></td>
            <td><strong>-P10.1</strong></td>
            <td><code>410274</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Videocamera (Camera quan sát đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC, IP68 ngâm nước sâu, góc mở 115° | MTS Special (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Camera giám sát gương đào và buồng bùn, tín hiệu truyền qua bộ phát BNC -B10.1.</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Head Video</code></td>
            <td><strong>-B1.3</strong></td>
            <td><code>115530</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>Transmitter camera head (Bộ truyền video đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chống nước IP68, đầu nối M12/BNC | MTS Perforator (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Khuếch đại và cân bằng trở kháng tín hiệu video truyền dẫn xa về trạm điều khiển trung tâm.</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 23</code></td>
            <td><strong>-K1.1</strong></td>
            <td><code>85010</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-40A-0007 (Module 4 AI Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4 Analoge Eingänge 0(4)..20 mA, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Thu thập EW372 (Case drain), EW374 (Slurry), EW376 (Charge line), EW378 (Cutter).</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 22</code></td>
            <td><strong>-K1.2</strong></td>
            <td><code>85010</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-40A-0007 (Module 4 AI Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4 Analoge Eingänge 0(4)..20 mA, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Thu thập EW364, EW366, EW368 (Áp 3 xi lanh lái) và EW370 (Inclinometer mts).</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 21</code></td>
            <td><strong>-K1.3</strong></td>
            <td><code>85010</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-40A-0007 (Module 4 AI Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4 Analoge Eingänge 0(4)..20 mA, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Thu thập EW356, EW358, EW360 (Hành trình 3 xi lanh lái) và EW362 (Áp suất lái).</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 20</code></td>
            <td><strong>-K1.4</strong></td>
            <td><code>85011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-0008D-0005 (Module 8 DO 2A Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">8 Digital Outputs 24VDC 2A, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển A51.0 (Đèn chiếu sáng đầu khiên) và các ngõ ra công suất dự phòng A51.1..A51.7.</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 30</code></td>
            <td><strong>-K2.1</strong></td>
            <td><code>85009</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-0202D-0003 (Module 2-Channel Counter)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">2 kanaliger Zähler High-Speed, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bộ đếm xung tốc độ cao EW478/EW483 tiếp nhận xung từ cảm biến encoder vòng quay đĩa cắt.</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 24</code></td>
            <td><strong>-K2.2</strong></td>
            <td><code>85059</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-0404D-0006 (Module 4 DI / 4 DO Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4 Digital Inputs / 4 Digital Outputs 2A, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đọc công tắc E52.0..E52.3 và kích mở cuộn hút van A52.0..A52.3 (By-pass và Béc phun).</td>
          </tr>
          <tr class="row-hw" data-sub="=bk" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 25</code></td>
            <td><strong>-K2.3</strong></td>
            <td><code>85011</code></td>
            <td><span class="subsystem-badge subsystem-bk">=bk</span></td>
            <td><strong>SDPB-0008D-0005 (Module 8 DO 2A Piconet)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">8 Digital Outputs 24VDC 2A, IP67 | Turck Piconet (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển A53.0..A53.7 cho 3 xi lanh bẻ lái 120 độ và xi lanh khóa thân khiên đào.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>E-Box</code></td>
            <td><strong>-U1.1</strong></td>
            <td><code>55184</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>Kompakt-SS 200x300x155mm (Vỏ hộp nguồn E-Box)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Sơn tĩnh điện, IP65, kèm tấm đế gắn thiết bị | Rittal / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Vỏ tủ nguồn phân phối DC 24V riêng biệt chống bụi ẩm cho thiết bị đầu khiên đào.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>AC 400V</code></td>
            <td><strong>-X1.3</strong></td>
            <td><code>25154</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>CEE-Stecker 400V 16A 5pol IP67 (Phích cắm nguồn E-Box)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V, 16A, 3P+N+PE, chống nước IP67 | Mennekes / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đầu cắm tiếp nhận nguồn 3 pha 400VAC từ tủ chính cấp cho biến áp nguồn E-Box.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>PKZM0</code></td>
            <td><strong>-F1.1</strong></td>
            <td><code>411977</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>Motorschutzschalter PKZM0 10..16A</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Aptomat bảo vệ động cơ / biến áp dải 10-16A | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bảo vệ quá dòng và ngắn mạch ngõ vào bộ nguồn xung QUINT-PS 24VDC 10A.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>24VDC 10A</code></td>
            <td><strong>-T1.1</strong></td>
            <td><code>34179</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>Primärschaltregler QUINT-PS-24VDC-10A</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Input 3x400-500VAC, Output 24VDC 10A (Power Boost) | Phoenix Contact (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bộ nguồn xung công nghiệp hiệu suất cao cấp nguồn ổn định cho toàn bộ cụm Piconet trên đầu khiên.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>B10A</code></td>
            <td><strong>-F1.2</strong></td>
            <td><code>411976</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>LS-Schalter B-Char. 1p 10A (Aptomat ngõ ra 24V)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Aptomat tép 1 cực 10A đường đặc tính B | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bảo vệ ngõ ra nguồn 24VDC cấp cho cáp dẫn xuyên vách ngăn đầu khiên.</td>
          </tr>
          <tr class="row-hw" data-sub="E-Box" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>CA-COM 22p</code></td>
            <td><strong>-X1.1 / -X1.2</strong></td>
            <td><code>340156</code></td>
            <td><span class="subsystem-badge subsystem-ebox">E-Box</span></td>
            <td><strong>CA-COM 22pol Einbaustift / Einbaubuchse</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Giắc cắm tròn công nghiệp 22 cực chịu va đập | ITT Cannon / NIES (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Cụm giắc cắm chuyên dụng truyền nguồn 24V và tín hiệu điều khiển ra đầu khiên đào.</td>
          </tr>
          <tr class="row-hw" data-sub="=cc" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>IPC V2</code></td>
            <td><strong>-P1.1</strong></td>
            <td><code>101022</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Industrie PC V2 + Windows 7 Ultimate (mts 100998)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Máy tính công nghiệp chuyên dụng, Win 7 Ult. | Siemens / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Máy tính trạm thu thập dữ liệu, chạy phần mềm SCADA TBM giám sát toàn bộ quá trình đào ngầm.</td>
          </tr>
          <tr class="row-hw" data-sub="=cc" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Touch HMI</code></td>
            <td><strong>-P1.2</strong></td>
            <td><code>100996</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>TFT Touchscreen 17" (Màn hình cảm ứng vận hành)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">17 inch công nghiệp, VGA/DVI, Touch USB | MTS Special (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Màn hình giao diện người - máy chính đặt trên bàn điều khiển trung tâm cabin.</td>
          </tr>
          <tr class="row-hw" data-sub="=cc" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Zusatzmonitor</code></td>
            <td><strong>-P3.1</strong></td>
            <td><code>101030</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Zusatzmonitor 7" (Màn hình camera phụ)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">7 inch màu, 24VDC, cổng BNC video camera | MTS Perforator (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Màn hình analog chuyên dụng hiển thị trực tiếp hình ảnh camera gương đào -P10.1.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB60</code></td>
            <td><strong>-S2.1</strong></td>
            <td><code>403811</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Head 24VDC (Khóa đóng nguồn 24V đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Công tắc chìa khóa 2 vị trí (Key switch) | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Khóa đóng/cắt nguồn điện điều khiển 24VDC cho toàn bộ thiết bị trên đầu khiên đào.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB62 / EB61 (AB62/AB61)</code></td>
            <td><strong>-S2.14 / -S2.15</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Cutter motor ON / OFF (Nút khởi động/dừng đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn kép có đèn LED (Xanh ON / Đỏ OFF) | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">EB62 kích chạy động cơ đầu cắt -M15.1; EB61 dừng động cơ; đèn phản hồi trạng thái AB62/AB61.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW276</code></td>
            <td><strong>-R2.6</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Cutter RPM (Chiết áp chỉnh tốc độ quay đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp xoay công nghiệp SmartWire-DT Poti | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Người vận hành xoay để đặt dải tốc độ quay 0 - 6 vòng/phút cho đầu cắt Schürfscheibe.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB90 / EB91 / EB92</code></td>
            <td><strong>-S2.31 / -S2.32 / -S2.33</strong></td>
            <td><code>345924</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Cutter Right / Stop / Left (Chọn chiều quay đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cụm nút ấn chuyển hướng 3 vị trí | Eaton RMQ-Titan (SL: 3)</span></td>
            <td style="color: var(--text-muted);">EB90 quay thuận (phải), EB91 dừng quay, EB92 quay ngược (trái) để gỡ kẹt đĩa cắt.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB71 / EB72 (AB71/AB72)</code></td>
            <td><strong>-S2.5 / -S2.4</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Main jack motor ON / OFF (Bơm thủy lực kích chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn kép có đèn LED (ON / OFF) | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">EB71 khởi động động cơ trạm nguồn thủy lực -M5.1 (132kW); EB72 dừng trạm bơm.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW272</code></td>
            <td><strong>-R2.4</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Main Jack Pressure (Chiết áp đặt áp suất kích đẩy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp xoay công nghiệp 0-400 bar | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cài đặt giới hạn áp lực đẩy tối đa của hệ thống kích đẩy chính trên bàn điều khiển.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW274</code></td>
            <td><strong>-R2.5</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Main Jack Speed (Chiết áp đặt vận tốc kích đẩy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp xoay công nghiệp 0-100 mm/min | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cài đặt lưu lượng dầu tỷ lệ điều khiển tốc độ tiến/lùi của kích đẩy ống.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB87 / EB88 / EB89</code></td>
            <td><strong>-S2.28 / -S2.29 / -S2.30</strong></td>
            <td><code>345924</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Jacks Ret / Stop / Adv (Điều khiển tiến/lùi kích)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cụm nút ấn điều hướng kích đẩy chính | Eaton RMQ-Titan (SL: 3)</span></td>
            <td style="color: var(--text-muted);">EB89 kích tiến (Advance), EB88 dừng kích (Stop), EB87 thu kích (Retract).</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB73 / EB74</code></td>
            <td><strong>-S2.3 / -S2.2</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Charge pump ON / OFF (Bơm cấp bùn bentonite)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn có đèn báo trạng thái | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Khởi động / dừng biến tần bơm nạp vữa -M10.1 (55kW) cấp vào buồng đào.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW278</code></td>
            <td><strong>-R2.1</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Charge pump RPM (Chiết áp tốc độ bơm cấp bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp SmartWire-DT 0-100% | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều chỉnh tần số biến tần ATV630 -T10.1 thay đổi lưu lượng bơm bùn vào gương khoan.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB76 / EB75</code></td>
            <td><strong>-S2.16 / -S2.17</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Discharge pump ON / OFF (Bơm hút thải xỉ bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn có đèn báo trạng thái | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Khởi động / dừng biến tần bơm hút bùn thải -M10.2 (55kW) chuyển đất đá ra ngoài bãi thải.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW280</code></td>
            <td><strong>-R2.2</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Discharge pump RPM (Chiết áp tốc độ bơm hút bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp SmartWire-DT 0-100% | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều chỉnh tần số biến tần ATV630 -T10.2 duy trì cân bằng thể tích bùn ra/vào buồng đào.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB77 / EB78</code></td>
            <td><strong>-S2.19 / -S2.18</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Booster pump 1 ON / OFF (Bơm tăng áp bùn số 1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn có đèn báo trạng thái | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Đóng ngắt biến tần bơm tăng áp bùn số 1 -M11.1 (55kW/690V) trên đường ống dài.</td>
          </tr>
          <tr class="row-sensor" data-sub="=cc" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>PEW282</code></td>
            <td><strong>-R2.3</strong></td>
            <td><code>410401</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Booster pump 1 RPM (Chiết áp tốc độ bơm tăng áp 1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Chiết áp SmartWire-DT 0-100% | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều chỉnh tốc độ bơm tăng áp số 1 đẩy bùn vượt đường dốc và cự ly khoan xa.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB69 / EB70 (AB69/AB70)</code></td>
            <td><strong>-S2.7 / -S2.6</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Jet Open / Closed (Nút đóng mở van béc phun BK)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn có đèn chỉ thị vị trí van | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Lệnh đóng/mở van béc phun xối nước áp lực cao trên đầu khiên làm tơi đất sét.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB67 / EB68 (AB67/AB68)</code></td>
            <td><strong>-S2.9 / -S2.8</strong></td>
            <td><code>343680</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>By-pass Open / Closed (Nút đóng mở van tuần hoàn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn có đèn chỉ thị vị trí van | Eaton RMQ-Titan (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Lệnh đóng/mở van By-pass tuần hoàn ngắn mạch vữa bùn tránh lắng cặn đường ống.</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>EB66..EB63</code></td>
            <td><strong>-S2.10..-S2.13</strong></td>
            <td><code>343683</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>Cyl #1..#3 & Wing (Công tắc chọn xi lanh lái & cánh)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Công tắc gạt xoay 2 vị trí (Selector switch) | Eaton Moeller (SL: 4)</span></td>
            <td style="color: var(--text-muted);">Kích hoạt chọn từng xi lanh lái riêng lẻ (1, 2, 3) hoặc cánh chống xoay (Wing).</td>
          </tr>
          <tr class="row-di" data-sub="=cc" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>Safety Loop</code></td>
            <td><strong>+pult-S3.3</strong></td>
            <td><code>405280</code></td>
            <td><span class="subsystem-badge subsystem-cc">=cc</span></td>
            <td><strong>NOT-AUS Control panel (Nút dừng khẩn cấp bàn điều khiển)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Nút ấn nấm đỏ tự giữ, 2 tiếp điểm NC an toàn | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Nút dừng khẩn cấp vị trí cabin ngắt nguồn toàn bộ trạm kích đẩy và đầu cắt ngay lập tức.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>900V AC</code></td>
            <td><strong>-M15.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Cutter motor head (Động cơ truyền động đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">900V / 200kW / 165A, 3 pha không đồng bộ | MTS Special (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Động cơ chính quay đĩa cắt TBM, cấp điện áp cao 900V từ biến áp Spartrafo -T15.2.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>Profibus DP</code></td>
            <td><strong>-T15.1</strong></td>
            <td><code>403165</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Softstarter Sirius 3RW4447-2BC44 (Khởi động mềm đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">250kW / 400V / 432A, Profibus DP communication | Siemens Sirius (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bộ khởi động mềm điện tử êm dịu, hạn chế sụt áp lưới và giám sát dòng kẹt đĩa cắt.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Trafo 900V</code></td>
            <td><strong>-T15.2</strong></td>
            <td><code>403157</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Spartrafo 400V / 900V / 300 kVA (Biến áp tăng áp đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Sơ cấp 400VAC, Thứ cấp 900VAC, Công suất 300kVA | MTS / Ruhstrat (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Tăng áp lên 900V để giảm tiết diện dây cáp động lực kéo dài hàng trăm mét vào lòng cống ngầm.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>NZM Cutter</code></td>
            <td><strong>-Q15.1</strong></td>
            <td><code>413940</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Leistungsschalter NZMH2 160..200A 1000VAC</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Aptomat khối điện áp cao 1000V, chỉnh định 160-200A | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Aptomat bảo vệ đường cáp động lực 900V cấp ra đầu khiên đào.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Pilotcheck</code></td>
            <td><strong>-K15.1</strong></td>
            <td><code>403664</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Pilotcheck GM420-D-1 (Rơ le kiểm tra dây nối đất cọc)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Giám sát điện trở dây nối đất bảo vệ PE cáp mềm | Bender (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bảo vệ an toàn điện giật: tự động cắt điện 900V nếu đứt hoặc suy giảm dây tiếp địa PE đầu khiên.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>400V Star-Delta</code></td>
            <td><strong>-M5.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Main motor container hydraulic (Động cơ bơm thủy lực chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V / 132kW / 240A, Star-Delta khởi động | MTS / ABB (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Động cơ kéo cụm bơm thủy lực piston cao áp cung cấp công suất cho các kích đẩy và mô tơ bẻ lái.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Contactor 75kW</code></td>
            <td><strong>-Q5.1 / -Q5.2 / -Q5.3</strong></td>
            <td><code>412232</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Leist.-schütz 75kW/400V (Bộ khởi động Sao-Tam Giác M5.1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Khởi động từ 3 pha 75kW, cuộn hút 24VDC | Eaton DILM150 (SL: 3)</span></td>
            <td style="color: var(--text-muted);">Cụm contactor Chạy lưới (-Q5.1), Tam giác (-Q5.2), Sao (-Q5.3) cùng rơ le thời gian -K5.4 10s.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>0.58 x In</code></td>
            <td><strong>-F5.2</strong></td>
            <td><code>412484</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Motorschutzschalter ZB150-150 (Rơ le nhiệt bơm chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Dải bảo vệ nhiệt quá tải cho động cơ 132kW | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bảo vệ quá nhiệt động cơ bơm thủy lực chính, ngắt tiếp điểm liên động an toàn khi quá tải.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>VFD Charge</code></td>
            <td><strong>-M10.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Charge pump motor (Động cơ bơm cấp bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V / 55kW / 98A, điều khiển biến tần | MTS / Siemens (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Động cơ bơm nạp dung dịch bentonite vào buồng đào giữ ổn định áp lực gương đất.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>Profibus DP</code></td>
            <td><strong>-T10.1</strong></td>
            <td><code>205310</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Altivar ATV630D75N4 (Biến tần bơm cấp bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Biến tần 55kW / 75HP, 3 pha 400V, card PBus 205316 | Schneider Altivar (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Biến tần thông minh điều khiển tốc độ vô cấp bơm nạp bùn, truyền thông Profibus DP.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>VFD Discharge</code></td>
            <td><strong>-M10.2</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Discharge pump motor (Động cơ bơm hút bùn thải)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V / 55kW / 98A, điều khiển biến tần | MTS / Siemens (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Động cơ bơm xả hút hỗn hợp bùn đất từ buồng đào chuyển ra hệ thống tách lắng bên ngoài giếng.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>Profibus DP</code></td>
            <td><strong>-T10.2</strong></td>
            <td><code>205310</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Altivar ATV630D75N4 (Biến tần bơm hút bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Biến tần 55kW / 75HP, 3 pha 400V, card PBus 205316 | Schneider Altivar (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Biến tần điều khiển lưu lượng bơm hút đồng bộ theo tốc độ đào thực tế.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>690V Booster</code></td>
            <td><strong>-M11.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Booster pump motor (Động cơ bơm tăng áp bùn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">690V / 55kW / 56A, cấp nguồn qua biến áp 90kVA | MTS / Siemens (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Động cơ bơm tăng áp đặt trên tuyến bùn dài, sử dụng điện áp 690V để chống tổn hao.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>VFD + Trafo 690V</code></td>
            <td><strong>-T11.1 / -T11.2</strong></td>
            <td><code>205310</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Altivar ATV630 + Spartrafo 400/660V 90kVA (Bộ truyền động bơm tăng áp)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Biến tần 55kW + Biến áp cách ly 90kVA | Schneider / Ruhstrat (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cụm biến tần và biến áp tăng áp cấp nguồn 690V cho bơm booster qua đường cáp dài.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>400V Steering</code></td>
            <td><strong>-M6.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Steering pump motor (Động cơ bơm thủy lực bẻ lái)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V / 7.5kW / 15.6A, starter PKE -Q6.2 | MTS / Demag (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cung cấp áp suất nguồn riêng phục vụ hệ thống xi lanh lái và điều khiển hướng đào.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>Fans 400V</code></td>
            <td><strong>-M6.2 / -M6.3</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Hydr. oil fan #1 & #2 (Quạt két làm mát dầu thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">2x 400V / 0.55kW / 1.61A, két làm mát LKI-510 | Ölkühler LKI (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Quạt thổi khí giải nhiệt két dầu thủy lực duy trì nhiệt độ dầu trong giới hạn 40-55 độ C.</td>
          </tr>
          <tr class="row-drive" data-sub="=ppm" data-cat="drive">
            <td><span class="badge-drive">ĐỘNG CƠ / BIẾN TẦN</span></td>
            <td class="val"><code>Compressor 400V</code></td>
            <td><strong>-M7.1</strong></td>
            <td><code>176449</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Compressor motor (Máy nén khí phụ trợ container)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V / 1.6kW / 3.5A, aptomat PKZM0 -F7.1 | MTS Compressor (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cung cấp khí nén phục vụ súc rửa ống, kiểm tra áp lực buồng đào và dụng cụ khí nén.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Main Switch</code></td>
            <td><strong>-Q1.1</strong></td>
            <td><code>401030</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Lasttrennschalter NZM4 3p 1250A (Cầu dao tổng trạm máy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">1250A, 3 cực, có cuộn cắt thấp áp 24VAC | Eaton Moeller (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cầu dao phụ tải đóng cắt toàn bộ trạm nguồn TBM 375kW, liên động ngắt khẩn cấp.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>MPI/DP CPU</code></td>
            <td><strong>-K22.1</strong></td>
            <td><code>115229</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>CPU 315-2 PN/DP (Bộ xử lý trung tâm PLC S7-300)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bộ nhớ 384KB, MPI/DP 12Mbps, PROFINET, MMC 128KB | Siemens SIMATIC (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bộ não điều khiển trung tâm thực thi 71 FC công nghệ, quản lý mạng Profibus DP và SmartWire.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>RS485 Repeater</code></td>
            <td><strong>-K22.4 / -K35.2</strong></td>
            <td><code>115223</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Repeater RS 485 (Bộ lặp truyền thông mở rộng Profibus)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Tốc độ lên tới 12Mbps, cách ly quang galvanic | Siemens S7 (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Khuếch đại tín hiệu đường truyền cáp dài ra đầu khiên đào và trạm bentonite chống suy hao.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>SWD Gateway</code></td>
            <td><strong>-K22.2 / -K22.3</strong></td>
            <td><code>406668</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>EU5C-SWD-DP (Bộ ghép mạng SmartWire-DT sang Profibus DP)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Địa chỉ DP 41 (Tủ chính PPM) và DP 40 (Bàn điều khiển CC) | Eaton Moeller (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Thu thập toàn bộ dữ liệu dòng tải motor PKE, aptomat NZM và phím bấm chiết áp từ bàn điều khiển.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 3</code></td>
            <td><strong>-K23.1</strong></td>
            <td><code>85072</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>BL20-E-GW-DP (Turck BL20 Gateway trạm tủ chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Gateway Profibus DP kết nối mô-đun I/O dạng lát cắt | Turck BL20 (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Giao tiếp các tín hiệu đo lưu lượng bùn, đếm mét hành trình đẩy và trạng thái contactor tủ điện.</td>
          </tr>
          <tr class="row-sensor" data-sub="=ppm" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW616</code></td>
            <td><strong>-K23.4 / -B11</strong></td>
            <td><code>85131</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Distance wheel (Bánh xe đo chiều dài hành trình đẩy LV)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Module BL20-E-2CNT + Cảm biến encoder con lăn đo mét | Turck / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo chính xác chiều dài đã kích đẩy được của từng đốt cống ngầm (đưa vào DB1.DBD0).</td>
          </tr>
          <tr class="row-sensor" data-sub="=ppm" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW420</code></td>
            <td><strong>-K23.5 / -T24.1</strong></td>
            <td><code>85077</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Flowmeter Charge line (Đồng hồ đo lưu lượng bùn cấp)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Lưu lượng kế điện từ (Magnetic flow meter) 4-20mA | Krohne / Endress (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo lưu lượng thể tích vữa bentonite cấp vào đầu khiên phục vụ cân bằng áp lực khoang đào.</td>
          </tr>
          <tr class="row-sensor" data-sub="=ppm" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW422</code></td>
            <td><strong>-K23.5 / -T24.2</strong></td>
            <td><code>85077</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Flowmeter Discharge line (Đồng hồ đo lưu lượng bùn hút)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Lưu lượng kế điện từ 4-20mA chịu bùn xỉ mài mòn | Krohne / Endress (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo lưu lượng bùn đất xả ra giếng để phát hiện sạt lở hoặc tắc nghẽn đường ống.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>24VDC 40A</code></td>
            <td><strong>-T20.1</strong></td>
            <td><code>34226</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Stromversorgung QUINT PS 40A (Bộ nguồn xung chính 24VDC)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">3x400-500VAC, Output 24VDC 40A, Power Boost | Phoenix Contact (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bộ nguồn xung công suất lớn cấp toàn bộ nguồn điều khiển cho PLC, rơ le, van và mạng bus.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>DC-UPS 20A</code></td>
            <td><strong>-G20.2 / -G20.3</strong></td>
            <td><code>30978</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>QUINT-DC-UPS/24VDC/20A + Bat 3.4AH (Bộ lưu điện dự phòng)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC 20A kèm ắc quy VRLA 24V 3.4Ah | Phoenix Contact (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đảm bảo nguồn duy trì liên tục cho CPU S7-300 và hệ thống lưu trữ dữ liệu khi mất điện đột ngột.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Ethernet TACS</code></td>
            <td><strong>-B26.1</strong></td>
            <td><code>115243</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>TACS receiverboard Ethernet (Bo mạch dẫn đường laser TACS)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bộ thu tín hiệu laser mục tiêu quang điện TACS | VMT GmbH (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Thu nhận tọa độ tia laser từ trạm toàn đạc trong giếng đẩy chiếu lên tâm gương đầu khiên.</td>
          </tr>
          <tr class="row-hw" data-sub="=ppm" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Video TR560</code></td>
            <td><strong>-B26.2</strong></td>
            <td><code>402543</code></td>
            <td><span class="subsystem-badge subsystem-ppm">=ppm</span></td>
            <td><strong>Video-Zweidraht-Empfänger NITEK TR560 (Bộ thu video 2 dây)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bộ cân bằng tín hiệu video xoắn đôi đường dài 1000m | NITEK (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Khôi phục tín hiệu video camera sắc nét từ đầu khiên đào truyền qua khoảng cách hàng trăm mét.</td>
          </tr>
          <tr class="row-hw" data-sub="=pph" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>Profibus DP 10</code></td>
            <td><strong>-K1.1</strong></td>
            <td><code>85057</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>BL67-GW-DP (Turck BL67 Gateway trạm nguồn thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Gateway Profibus DP dạng khối IP67 lắp trên cụm van | Turck BL67 (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Giao tiếp toàn bộ các cảm biến và cuộn van điện từ điều khiển trạm thủy lực với PLC.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW304</code></td>
            <td><strong>-K1.2 / -B6</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>By-pass filter (Cảm biến nghẹt lọc by-pass thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4-20mA, dải đo chênh áp lọc thủy lực | Turck / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảnh báo mức độ nghẹt cặn bẩn của lõi lọc dầu thủy lực đường by-pass.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW306</code></td>
            <td><strong>-K1.2 / -B7</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Return line filter #1 (Cảm biến nghẹt lọc dầu hồi #1)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4-20mA, chênh áp lọc dầu hồi | Turck / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Giám sát áp lực đường dầu hồi về thùng, cảnh báo thay thế phin lọc trước khi dầu bị trào van an toàn.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW308</code></td>
            <td><strong>-K1.2 / -B9</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Feeding circuit filter (Cảm biến lọc mạch nạp bù dầu)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4-20mA, đo độ sạch mạch dầu nạp | Turck / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Giám sát phin lọc của bơm nhồi thủy lực mạch kín bảo vệ bơm piston không bị xâm thực.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW310</code></td>
            <td><strong>-K1.2 / -B4</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Pres. feeding circuit (Áp suất nạp mạch thủy lực kín)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-50 bar / 4-20mA | Turck / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo áp suất nhồi bù dầu (Charge pressure) của mạch tuần hoàn kín hệ thống thủy lực.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW404 / EW408</code></td>
            <td><strong>-K1.3 / -B10</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Temp & Humidity HydrOil (Nhiệt độ & Độ ẩm dầu thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Đầu dò kép RTD nhiệt độ + cảm biến ẩm % rH trong dầu | Hydac / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">EW404 đo nhiệt độ thùng dầu; EW408 phát hiện nước bị lẫn vào dầu thủy lực (nhiễm ẩm).</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW406</code></td>
            <td><strong>-K1.3 / -B5</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Pres. Jacks (Áp lực trạm kích đẩy chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-400 bar / 4-20mA, lắp tại khối van trạm nguồn | Rexroth / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảm biến áp suất làm việc thực tế của các xi lanh kích đẩy ống tại giếng đẩy.</td>
          </tr>
          <tr class="row-sensor" data-sub="=pph" data-cat="sensor">
            <td><span class="badge-sensor">CẢM BIẾN</span></td>
            <td class="val"><code>EW410</code></td>
            <td><strong>-K1.3 / -B3</strong></td>
            <td><code>85046</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Pressure cutter (Áp suất nguồn thủy lực đĩa cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">0-350 bar / 4-20mA | Rexroth / Hydac (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đo áp suất mạch dầu cấp cho các mô tơ thủy lực quay đầu cắt Schürfrad tại trạm nguồn.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>AW264</code></td>
            <td><strong>-K1.4 / -K1</strong></td>
            <td><code>346089</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Speed jacks valve (Van tỷ lệ điều chỉnh vận tốc kích đẩy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card khuếch đại Rexroth VT-MSPA1-30 (-K5.3) -> Van tỷ lệ -K1 | Rexroth Bosch (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Ngõ ra analog 0-10V từ PLC qua card khuếch đại lái van tỷ lệ điều chỉnh vô cấp vận tốc kích đẩy.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>AW266</code></td>
            <td><strong>-K1.4 / -K3</strong></td>
            <td><code>34182</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Pres. jacks valve (Van tỷ lệ điều chỉnh áp suất kích đẩy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card khuếch đại Rexroth VT 11131-1X (-K5.1) -> Van tỷ lệ -K3 | Rexroth Bosch (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Ngõ ra analog 0-10V điều chỉnh áp lực làm việc tối đa của dàn xi lanh đẩy chính theo cài đặt.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>AW268</code></td>
            <td><strong>-K1.5 / -K13/-K14</strong></td>
            <td><code>347151</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Speed cutter valve (Van tỷ lệ điều khiển tốc độ & hướng quay đầu cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card khuếch đại Rexroth VT 11118-1X (-K5.2) -> Van tỷ lệ đảo chiều -K13/-K14 | Rexroth Bosch (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển lưu lượng và hướng dòng dầu thủy lực cấp cho mô tơ đĩa cắt (quay thuận/nghịch).</td>
          </tr>
          <tr class="row-di" data-sub="=pph" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>E12.1</code></td>
            <td><strong>-K2.1 / -B1</strong></td>
            <td><code>85052</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Oil level warning (Cảnh báo mức dầu thủy lực thấp)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Phao từ mức dầu tiếp điểm NC, 24VDC | MTS Sensors (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cảnh báo mức dầu thủy lực trong bồn chính xuống dưới ngưỡng cho phép, kích hoạt đèn vàng.</td>
          </tr>
          <tr class="row-di" data-sub="=pph" data-cat="di">
            <td><span class="badge-di">CÔNG TẮC / DI</span></td>
            <td class="val"><code>E12.0</code></td>
            <td><strong>-K2.1 / -B2</strong></td>
            <td><code>85052</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Oil level low (Báo động cạn dầu thủy lực - Dừng máy)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Phao từ mức cạn nguy hiểm tiếp điểm NC | MTS Sensors (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Báo động mức dầu cạn nguy hiểm, ngắt ngay lập tức động cơ trạm bơm thủy lực -M5.1 bảo vệ bơm.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A16.0 / A16.2</code></td>
            <td><strong>-K2.3 / -K6</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Jacks Adv/Ret Left (Van kích đẩy nhánh bên trái)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van phân phối 24VDC 2A | Rexroth / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển mở dầu cho nhóm xi lanh kích đẩy bên trái tiến (A16.2) hoặc lùi (A16.0).</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A17.0 / A17.2</code></td>
            <td><strong>-K2.4 / -K7</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Jacks Adv/Ret Right (Van kích đẩy nhánh bên phải)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van phân phối 24VDC 2A | Rexroth / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển mở dầu cho nhóm xi lanh kích đẩy bên phải tiến (A17.2) hoặc lùi (A17.0).</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A16.1 / A16.3</code></td>
            <td><strong>-K2.3 / -K10/-K11</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Interjacks 1 & 2 (Van kích trung gian Dehner 1 & 2)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van thủy lực 24VDC 2A | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Điều khiển các trạm kích phụ trung gian trên tuyến ống dài hỗ trợ giảm lực đẩy cho giếng chính.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A17.1</code></td>
            <td><strong>-K2.4 / -A17.1</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Interjack 3 (Van kích trung gian Dehner 3)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van thủy lực 24VDC 2A | Rexroth / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Điều khiển trạm kích trung gian số 3.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A17.3</code></td>
            <td><strong>-K2.4 / -K15</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Release cutter (Van giải phóng áp lực đĩa cắt)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van xả nhanh thủy lực 24VDC | Rexroth / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Mở xả nhanh dầu mạch đầu cắt giải phóng kẹt đĩa khi gặp đá cứng hoặc sự cố quá tải.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A18.2</code></td>
            <td><strong>-K2.6 / -K16</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Steering valve (Van cấp nguồn bẻ lái trạm thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van phân phối 24VDC 2A | Rexroth / MTS (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Mở đường dầu từ trạm nguồn container cấp ra các van bẻ lái trên đầu khiên đào.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A18.1 / A18.3</code></td>
            <td><strong>-K2.6 / -A18.1</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Pipe hold (Van kẹp giữ ống Set/Release)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van điều khiển xi lanh kẹp ngàm ống | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">A18.1 kẹp chặt đốt ống ngăn trôi lùi khi thu kích; A18.3 nhả kẹp khi kích đẩy.</td>
          </tr>
          <tr class="row-actuator" data-sub="=pph" data-cat="actuator">
            <td><span class="badge-actuator">VAN / CHẤP HÀNH</span></td>
            <td class="val"><code>A19.1 / A19.3</code></td>
            <td><strong>-K2.7 / -K8</strong></td>
            <td><code>85053</code></td>
            <td><span class="subsystem-badge subsystem-pph">=pph</span></td>
            <td><strong>Lock / Unlock (Van khóa liên động trạm thủy lực)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">Van an toàn thủy lực 24VDC | Rexroth / MTS (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Khóa/Mở các đường dầu áp suất cao đảm bảo an toàn trong quá trình công nhân lắp ống.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>400V 1250A</code></td>
            <td><strong>=Schottleiste-X01</strong></td>
            <td><code>055866</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>Anschlussklemme K3x240/4 (Cầu đấu cực nguồn chính)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">3x240mm² có nắp che bảo vệ an toàn | Eaton (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Đầu tiếp nhận cáp điện nguồn 3 pha cỡ lớn từ máy phát hoặc lưới điện công trường.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>400V 125A</code></td>
            <td><strong>=Schottleiste-X7.3</strong></td>
            <td><code>402913</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CEE-Anbausteckdose 125A 4pol IP67 (Ổ cắm bơm nước cao áp 37kW)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V, 125A, 4 cực, thẳng, chống nước IP67 | Elektron (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cấp nguồn động lực cho máy bơm nước rửa cao áp 37kW.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>400V 125A</code></td>
            <td><strong>=Schottleiste-X10.1 / X10.2</strong></td>
            <td><code>402913</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CEE-Anbausteckdose 125A 4pol IP67 (Ổ cắm bơm cấp/hút bùn 55kW)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V, 125A, 4 cực, thẳng, chống nước IP67 | Elektron (SL: 2)</span></td>
            <td style="color: var(--text-muted);">Cấp điện áp điều khiển từ 2 biến tần Altivar ATV630 ra động cơ bơm cấp M10.1 và bơm hút M10.2.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>690V 125A</code></td>
            <td><strong>=Schottleiste-X11.1</strong></td>
            <td><code>345611</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CEE-Anbausteckdose 690V 125A 4pol IP67 (Ổ cắm bơm tăng áp 690V)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">690V, 125A, 4 cực, thẳng, chống nước IP67 | Elektron (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cấp điện áp 690V sau biến áp tăng thế -T11.2 ra động cơ bơm tăng áp bùn M11.1.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>400V 32A</code></td>
            <td><strong>=Schottleiste-X7.1</strong></td>
            <td><code>346383</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CEE-Anbausteckdose 32A 5pol IP67 (Ổ cắm trạm phụ Bentonite)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V, 32A, 5 cực (3P+N+PE), IP67 | Elektron (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cấp nguồn cho trạm pha chế và trộn vữa bentonite hỗ trợ thi công.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>400V 16A</code></td>
            <td><strong>=Schottleiste-X7.2</strong></td>
            <td><code>346382</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CEE-Anbausteckdose 16A 5pol IP67 (Ổ cắm phụ trợ đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">400V, 16A, 5 cực, IP67 | Elektron (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Cấp nguồn phụ trợ 400V ra đầu khiên phục vụ thiết bị bảo dưỡng và bơm mỡ bôi trơn.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>CA-COM 22p</code></td>
            <td><strong>=Schottleiste-X26.1</strong></td>
            <td><code>340157</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CA-COM 22pol Einbaubuchse (Giắc nối cáp lái đầu khiên)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">22 cực tròn công nghiệp, vỏ kim loại chống nước | NIES / ITT Cannon (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Kết nối cáp lái hợp bộ LEHC 004145 truyền tín hiệu TACS, video camera và nguồn điều khiển.</td>
          </tr>
          <tr class="row-hw" data-sub="Schottleiste" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>CA-COM 17p</code></td>
            <td><strong>=Schottleiste-X35.2</strong></td>
            <td><code>402385</code></td>
            <td><span class="subsystem-badge subsystem-schott">Schottleiste</span></td>
            <td><strong>CA-COM 17pol BuoUe (Giắc cắm điều khiển trạm bentonite)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">17 cực tròn công nghiệp, có khóa gài an toàn | NIES (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Kết nối điều khiển tự động hóa trạm bơm bentonite ngoài mặt đất.</td>
          </tr>
          <tr class="row-hw" data-sub="=hz" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>RCD 40A</code></td>
            <td><strong>=hz-F1.1</strong></td>
            <td><code>279217</code></td>
            <td><span class="subsystem-badge subsystem-hz">=hz</span></td>
            <td><strong>Fehlerstromschutzschalter 4p/40A/30mA (Rơ le chống dòng rò)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">4 cực, 40A, dòng rò tác động 30mA (FI-40/4/003-A) | Eaton (SL: 1)</span></td>
            <td style="color: var(--text-muted);">Bảo vệ chống giật và chống hỏa hoạn toàn bộ hệ thống sưởi và chiếu sáng container.</td>
          </tr>
          <tr class="row-hw" data-sub="=hz" data-cat="hw">
            <td><span class="badge-hw">MODULE / PHẦN CỨNG</span></td>
            <td class="val"><code>B16A</code></td>
            <td><strong>=hz-F1.2..-F1.7</strong></td>
            <td><code>344886</code></td>
            <td><span class="subsystem-badge subsystem-hz">=hz</span></td>
            <td><strong>LS-Schalter B-Char. 1p 16A (6 Aptomat nhánh sưởi/đèn)</strong><br><span style="font-size: 0.72rem; color: var(--text-muted);">6x Aptomat tép 1 cực 16A đường đặc tính B (FAZ-B16) | Eaton (SL: 6)</span></td>
            <td style="color: var(--text-muted);">Cấp nguồn sưởi ấm phòng điều khiển (-X1.2), phòng biến tần (-X1.3, -X1.4), phòng thủy lực (-X1.5, -X1.6) và chiếu sáng container (-F1.7).</td>
          </tr>

        </tbody>
      </table>
        </div>
    </div>
  </div>


  <!-- ========================================================
       TAB 4: CABLE SCHEDULE & TERMINAL STRIPS (KABELLISTE & KLEMMENPLAN)
       ======================================================== -->
  <div id="tab-cables" class="tab-content">
    
    <!-- Section 1: KABELLISTE -->
    <div class="card">
      <div class="card-title">
        <span>BẢNG TRA CỨU TOÀN BỘ 82 ĐƯỜNG CÁP ĐIỆN HỆ THỐNG TBM (KABELLISTE SHEET 1 & 2)</span>
        <span class="tag">BẢN VẼ 2018221-VC SHEET 44-45 & 72-73</span>
      </div>
      <p style="font-size: 0.82rem; color: var(--text-muted); margin-bottom: 14px;">
        Trích xuất 100% đầy đủ danh mục cáp từ bản vẽ kỹ thuật MTS Perforator GmbH. Bao gồm cáp động lực hạ thế 1x240mm², cáp kéo dài 900V/690V, cáp điều khiển bọc kim Ölflex, cáp mạng kéo co giãn UNITRONIC BUS PB FD P A và cáp lai hợp bộ đầu khiên LEHC.
      </p>

      <div class="comp-toolbar">
        <div class="comp-filter-bar">
          <button class="comp-filter-btn active" id="btnCableAll" onclick="filterCables('all')">Tất cả (82 cáp)</button>
          <button class="comp-filter-btn" id="btnCablePower" onclick="filterCables('power')">⚡ Cáp Động Lực</button>
          <button class="comp-filter-btn" id="btnCableControl" onclick="filterCables('control')">🔌 Cáp Điều Khiển</button>
          <button class="comp-filter-btn" id="btnCableSafety" onclick="filterCables('safety')">🚨 Mạch Dừng Khẩn</button>
          <button class="comp-filter-btn" id="btnCableBus" onclick="filterCables('bus')">🌐 Cáp Mạng Bus / Ethernet</button>
          <button class="comp-filter-btn" id="btnCableSensor" onclick="filterCables('sensor')">📡 Cáp Cảm Biến</button>
        </div>

        <input type="text" id="cableSearchInput" class="search-box" placeholder="🔍 Tìm kiếm mã cáp, loại cáp, nguồn/đích..." onkeyup="searchCables()">
      </div>

      <div class="table-responsive">
          <table class="plc-table" id="cableTable" style="font-size: 0.78rem;">
        <thead>
          <tr>
            <th style="width: 40px; text-align: center;">STT</th>
            <th style="width: 140px;">Mã cáp (Cable Des.)</th>
            <th style="width: 100px;">Nhóm</th>
            <th style="width: 240px;">Quy cách cáp (Type & Spec)</th>
            <th style="width: 220px;">Thiết bị xuất phát (Resource)</th>
            <th>Thiết bị đích (Target Destination)</th>
          </tr>
        </thead>
        <tbody id="cableTableBody">
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">1</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W1.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>NSGAFÖU 1x240</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x240mm² (L=3.2m, 1 lõi)</span></td>
            <td><code>=ppm-Q1.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Main switch</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Input 400V 630A</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">2</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W1.4</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>NSGAFÖU 1x2,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2.5mm² (L=-, 1 lõi)</span></td>
            <td><code>=ppm-Q1.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Main switch</span></td>
            <td><code>=ppm-F1.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Control trafo fuse</span></td>
          </tr>
          <tr class="row-cable" data-cat="safety">
            <td style="text-align: center; color: var(--text-muted);">3</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W3.1</code></td>
            <td><span class="subsystem-badge cable-badge-safety">AN TOÀN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=3m, 4 lõi)</span></td>
            <td><code>=ppm-X1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
            <td><code>=ppm-S3.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD room E-Stop</span></td>
          </tr>
          <tr class="row-cable" data-cat="safety">
            <td style="text-align: center; color: var(--text-muted);">4</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W3.2</code></td>
            <td><span class="subsystem-badge cable-badge-safety">AN TOÀN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=9m, 4 lõi)</span></td>
            <td><code>=ppm-X1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
            <td><code>=ppm-S3.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Power pack E-Stop</span></td>
          </tr>
          <tr class="row-cable" data-cat="safety">
            <td style="text-align: center; color: var(--text-muted);">5</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W3.3</code></td>
            <td><span class="subsystem-badge cable-badge-safety">AN TOÀN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=6m, 4 lõi)</span></td>
            <td><code>=ppm-X1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
            <td><code>=cc+pult-S3.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Control panel E-Stop</span></td>
          </tr>
          <tr class="row-cable" data-cat="safety">
            <td style="text-align: center; color: var(--text-muted);">6</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W3.4</code></td>
            <td><span class="subsystem-badge cable-badge-safety">AN TOÀN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=5.5m, 4 lõi)</span></td>
            <td><code>=ppm-X1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
          </tr>
          <tr class="row-cable" data-cat="safety">
            <td style="text-align: center; color: var(--text-muted);">7</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W3.5</code></td>
            <td><span class="subsystem-badge cable-badge-safety">AN TOÀN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=25m, 4 lõi)</span></td>
            <td><code>=ppm-X3.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Emergency Off</span></td>
            <td><code>=ppm-S3.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Shaft E-Stop</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">8</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W5.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4x50mm²</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x50mm² (L=7.4m, 4 lõi)</span></td>
            <td><code>=ppm-F5.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Motor protection</span></td>
            <td><code>=ppm-M5.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Main motor 132kW U1/V1/W1</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">9</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W5.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4x50mm²</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x50mm² (L=7.4m, 4 lõi)</span></td>
            <td><code>=ppm-Q5.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Delta contactor</span></td>
            <td><code>=ppm-M5.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Main motor 132kW U2/V2/W2</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">10</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W6.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G2,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G2.5mm² (L=8.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering pump</span></td>
            <td><code>=ppm-M6.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering pump 7.5kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">11</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W6.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>Ölflex 4x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x1.5mm² (L=9.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydr. oil fan #1</span></td>
            <td><code>=ppm-M6.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydr. oil fan #1 0.55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">12</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W6.3</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>Ölflex 4x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x1.5mm² (L=9.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydr. oil fan #2</span></td>
            <td><code>=ppm-M6.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydr. oil fan #2 0.55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">13</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W7.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>Ölflex 4x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x1.5mm² (L=8.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Compressor</span></td>
            <td><code>=ppm-M7.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Compressor 1.6kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">14</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W7.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G6</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G6mm² (L=5.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bentonite</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bentonite socket X7.1</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">15</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W7.3</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G2,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G2.5mm² (L=5.5m, 4 lõi)</span></td>
            <td><code>=ppm-X8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Head</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Head socket X7.2</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">16</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W7.4</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G25</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G25mm² (L=5.5m, 4 lõi)</span></td>
            <td><code>=ppm-F7.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">High pressure water</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Water pump socket X7.3 (37kW)</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">17</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=2.5m, 4 lõi)</span></td>
            <td><code>=ppm-F10.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge pump breaker</span></td>
            <td><code>=ppm-T10.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD Charge pump 55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">18</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=2.5m, 4 lõi)</span></td>
            <td><code>=ppm-F10.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge pump breaker</span></td>
            <td><code>=ppm-T10.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD Discharge pump 55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">19</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.3</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=5m, 4 lõi)</span></td>
            <td><code>=ppm-T10.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD Charge output</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge socket X10.1</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">20</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.4</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=5m, 4 lõi)</span></td>
            <td><code>=ppm-T10.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD Discharge output</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge socket X10.2</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">21</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.5</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=5m, 4 lõi)</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge socket X10.1</span></td>
            <td><code>=ppm-M10.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge motor 55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">22</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W10.6</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=5m, 4 lõi)</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge socket X10.2</span></td>
            <td><code>=ppm-M10.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge motor 55kW</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">23</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W11.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=2.5m, 4 lõi)</span></td>
            <td><code>=ppm-F11.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster breaker 100A</span></td>
            <td><code>=ppm-T11.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">VFD Booster pump</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">24</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W11.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4x16mm²</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x16mm² (L=2.5m, 4 lõi)</span></td>
            <td><code>=ppm-T11.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Trafo 90kVA 690V</span></td>
            <td><code>=ppm-F11.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Motor breaker 65A</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">25</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W11.3</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4x16mm²</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x16mm² (L=5m, 4 lõi)</span></td>
            <td><code>=ppm-F11.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Motor breaker 65A</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster socket X11.1 (690V)</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">26</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W11.4</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>H07RN-F 4G35</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4G35mm² (L=2.5m, 4 lõi)</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster socket X11.1</span></td>
            <td><code>=ppm-M11.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster motor 55kW/690V</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">27</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W15.1</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>NSGAFÖU 1x120</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x120mm² (L=-, 4 lõi)</span></td>
            <td><code>=ppm-F15.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cutter breaker 400A</span></td>
            <td><code>=ppm-T15.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Softstarter Sirius 3RW4447</span></td>
          </tr>
          <tr class="row-cable" data-cat="power">
            <td style="text-align: center; color: var(--text-muted);">28</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W15.2</code></td>
            <td><span class="subsystem-badge cable-badge-power">ĐỘNG LỰC</span></td>
            <td><strong>NSGAFÖU 1x70</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x70mm² (L=-, 3 lõi)</span></td>
            <td><code>=ppm-T15.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Spartrafo 300kVA 900V</span></td>
            <td><code>=ppm+mr-Q15.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">NZM Cutter 1000V</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">29</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W20.2</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 3x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">3x1.5mm² (L=5m, 3 lõi)</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC Distribution</span></td>
            <td><code>=cc-X6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">IPC 24V DC Cabin</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">30</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W21.3</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 3x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">3x1.5mm² (L=5m, 3 lõi)</span></td>
            <td><code>=cc-X6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cabin 24V DC</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC Cabin Feed</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">31</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W21.4</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 3x1,5</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">3x1.5mm² (L=5.5m, 3 lõi)</span></td>
            <td><code>=ppm-X5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering cable 22p</span></td>
            <td><code>=ppm-Q21.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Head 24V supply</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">32</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W21.5</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>7/8" Power 10m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">5-pole 7/8" (L=10m, 5 lõi)</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC Power Pack</span></td>
            <td><code>=pph-X1.6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 Power feed PPH</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">33</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.1</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=0.5m, 2 lõi)</span></td>
            <td><code>=ppm-X22.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD SPS</span></td>
            <td><code>=ppm-K22.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">RS485 Repeater</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">34</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.2</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=0.75m, 2 lõi)</span></td>
            <td><code>=ppm-X22.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD PPM</span></td>
            <td><code>=ppm-K22.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">RS485 Repeater</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">35</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.3</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=0.3m, 2 lõi)</span></td>
            <td><code>=ppm-X22.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD PPM</span></td>
            <td><code>=ppm-X22.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD Pult</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">36</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.4</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=0.4m, 2 lõi)</span></td>
            <td><code>=ppm-X22.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD Pult</span></td>
            <td><code>=ppm-K23.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Gateway BL20 PPM</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">37</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.6</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=4m, 2 lõi)</span></td>
            <td><code>=ppm-K22.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Repeater</span></td>
            <td><code>=ppm-X30.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cutter motor plug</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">38</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.7</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=5.5m, 2 lõi)</span></td>
            <td><code>=ppm-X5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering cable 22p</span></td>
            <td><code>=ppm-K22.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Repeater</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">39</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W22.8</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>SWD-Rund 8-pol</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">8-pole circular (L=4.5m, 8 lõi)</span></td>
            <td><code>=ppm-X22.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD Pult adapter</span></td>
            <td><code>=cc-X2.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">SWD Console Cabin</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">40</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W24.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 4x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4x0.75mm² (L=5.5m, 4 lõi)</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">LV Distance</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Distance wheel plug X24.3</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">41</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W24.2</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Verb.Kabel 4pol CA-Com</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">4-pole CA-COM (L=25m, 4 lõi)</span></td>
            <td><code>=ppm-X24.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Distance wheel</span></td>
            <td><code>=ppm-X24.5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Distance wheel -B11</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">42</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W24.3</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 7x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">7x0.75mm² (L=5.5m, 6 lõi)</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC Flowmeter</span></td>
            <td><code>=ppm+Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Flowmeter intermediate</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">43</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W24.4</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 7x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">7x0.75mm² (L=25m, 6 lõi)</span></td>
            <td><code>=ppm-X24.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Flowmeter plug</span></td>
            <td><code>=ppm+Flowmeter</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge line meter T24.1</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">44</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>LEHC 004145 rev.0</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">22-core hybrid armored (L=5.5m, 16 lõi)</span></td>
            <td><code>=ppm-X5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering cable 22p</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Steering cable head X26.1</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">45</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.2</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>Patchkabel 1m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">CAT5e RJ45 (L=1m, 8 lõi)</span></td>
            <td><code>=ppm-K26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Switch 4 Port</span></td>
            <td><code>=ppm-B26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">TACS Board</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">46</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.3</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>Patchkabel 1m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">CAT5e RJ45 (L=1m, 8 lõi)</span></td>
            <td><code>=ppm-K26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Switch 4 Port</span></td>
            <td><code>=ppm-B26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">TACS Board port 2</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">47</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.4</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>Patchkabel 7,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">CAT5e RJ45 (L=7.5m, 8 lõi)</span></td>
            <td><code>=ppm-K26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Switch 4 Port</span></td>
            <td><code>=cc-P1.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">IPC Machine Cabin</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">48</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.5</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>Patchkabel 1m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">CAT5e RJ45 (L=1m, 8 lõi)</span></td>
            <td><code>=ppm-K26.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Switch 4 Port</span></td>
            <td><code>=ppm-K22.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">CPU 315 PN port</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">49</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W26.6</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Verbindungskabel 7"</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">Coaxial + Power (L=10m, 2 lõi)</span></td>
            <td><code>=cc-P3.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Zusatzmonitor 7"</span></td>
            <td><code>=ppm-B26.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Video Receiver TR560</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">50</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W30.1</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=3m, 2 lõi)</span></td>
            <td><code>=ppm-X30.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge pump plug</span></td>
            <td><code>=ppm-X30.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cutter motor plug</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">51</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W30.2</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=3m, 2 lõi)</span></td>
            <td><code>=ppm-X30.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge pump plug</span></td>
            <td><code>=ppm-X30.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Charge pump plug</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">52</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W30.3</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=3m, 2 lõi)</span></td>
            <td><code>=ppm-X30.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster pump plug</span></td>
            <td><code>=ppm-X30.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Discharge pump plug</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">53</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W30.4</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=3m, 2 lõi)</span></td>
            <td><code>=pph-K1.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Gateway BL67 PPH</span></td>
            <td><code>=ppm-X30.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Booster pump plug</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">54</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W35.1</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>UNITRONIC BUS PB FD</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">1x2x0.64mm (L=-, 2 lõi)</span></td>
            <td><code>=ppm-K35.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Repeater Bentonite</span></td>
            <td><code>=Schottleiste</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Bentonite socket X35.2</span></td>
          </tr>
          <tr class="row-cable" data-cat="bus">
            <td style="text-align: center; color: var(--text-muted);">55</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm-W35.2</code></td>
            <td><span class="subsystem-badge cable-badge-bus">MẠNG BUS</span></td>
            <td><strong>Patchkabel 7,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">CAT5e RJ45 (L=7.5m, 8 lõi)</span></td>
            <td><code>=ppm-K35.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">CPU Bentonite</span></td>
            <td><code>=cc-P1.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">IPC Machine</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">56</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=ppm+mr-W25.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>Oelflex 2x0,75</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">2x0.75mm² (L=-, 2 lõi)</span></td>
            <td><code>=ppm-X7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">24VDC</span></td>
            <td><code>=ppm+mr-Q15.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">NZM Cutter 1000V contact</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">57</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W1.3</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>7/8" Power 0,6m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">5-pole 7/8" (L=0.6m, 5 lõi)</span></td>
            <td><code>=pph-X1.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 supply in</span></td>
            <td><code>=pph-X1.5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 supply out</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">58</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W2.5</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>7/8" Power 0,6m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">5-pole 7/8" (L=0.6m, 5 lõi)</span></td>
            <td><code>=pph-X2.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 module supply</span></td>
            <td><code>=pph-X2.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 module supply</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">59</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W2.6</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>5x1,5mm²</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">5x1.5mm² (L=-, 5 lõi)</span></td>
            <td><code>=pph-X2.6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 Versorgungsltg</span></td>
            <td><code>=pph+hydr-X10</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydraulic terminal X10</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">60</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W3.1</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X3.6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">By-pass filter EW304</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">61</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W3.2</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X3.7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Return line filter #1 EW306</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">62</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W3.3</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X3.8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B9</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Feeding circuit filter EW308</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">63</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W3.8</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=5m, 2 lõi)</span></td>
            <td><code>=pph-X3.13</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pres feeding circuit EW310</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">64</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W4.1</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel 8pol Bu. ger.</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 8-pin (L=-, 4 lõi)</span></td>
            <td><code>=pph-X4.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Hydraulic terminal X10</span></td>
            <td><code>=pph-B10</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Temp & Humidity HydrOil</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">65</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W4.2</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph-X4.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-X10</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Temp sensor to X10</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">66</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W4.3</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph-X4.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-X10</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Humidity sensor to X10</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">67</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W4.4</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X4.5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pres. Jacks EW406</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">68</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W4.5</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X4.7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 AI module</span></td>
            <td><code>=pph-B3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pressure cutter EW410</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">69</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.3</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph+hydr-K5.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT-MSPA1</span></td>
            <td><code>=pph-X5.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Speed jacks AO AW264</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">70</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.4</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph+hydr-K5.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT 11131</span></td>
            <td><code>=pph-X5.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pres jacks AO AW266</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">71</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.5</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph+hydr-K5.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT 11118</span></td>
            <td><code>=pph-X5.5</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Speed cutter AO AW268</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">72</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.6</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel Bu. ger. 5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=5m, 2 lõi)</span></td>
            <td><code>=pph-X5.7</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT 11131</span></td>
            <td><code>=pph-K3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pres. Jacks proportional valve</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">73</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.7</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel Bu. ger. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X5.8</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT 11118</span></td>
            <td><code>=pph-K14</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cutter right prop coil</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">74</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.8</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel Bu. ger. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X5.9</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT 11118</span></td>
            <td><code>=pph-K13</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Cutter left prop coil</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">75</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W5.9</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel Bu. ger. 3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=3m, 2 lõi)</span></td>
            <td><code>=pph-X5.6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Card VT-MSPA1</span></td>
            <td><code>=pph-K1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Speed jacks prop valve</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">76</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W6.1</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=5m, 4 lõi)</span></td>
            <td><code>=pph-X6.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DI module</span></td>
            <td><code>=pph-B1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Oil level warning E12.1</span></td>
          </tr>
          <tr class="row-cable" data-cat="sensor">
            <td style="text-align: center; color: var(--text-muted);">77</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W6.2</code></td>
            <td><span class="subsystem-badge cable-badge-sensor">CẢM BIẾN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=5m, 4 lõi)</span></td>
            <td><code>=pph-X6.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DI module</span></td>
            <td><code>=pph-B2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Oil level low E12.0</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">78</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W7.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 0,3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=0.3m, 4 lõi)</span></td>
            <td><code>=pph-X7.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DO module</span></td>
            <td><code>=pph-X7.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Interjacks 1 & 2 valves</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">79</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W8.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 0,3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=0.3m, 4 lõi)</span></td>
            <td><code>=pph-X8.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DO module</span></td>
            <td><code>=pph-X8.4</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Interjack 3 valve</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">80</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W8.2</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 2m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=2m, 4 lõi)</span></td>
            <td><code>=pph-X8.6</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DO module</span></td>
            <td><code>=pph-K15</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Release cutter valve</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">81</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W9.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 St. gew. <-> Bu. ger. 0,3m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=0.3m, 4 lõi)</span></td>
            <td><code>=pph-X9.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">BL67 DO module</span></td>
            <td><code>=pph-X9.3</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Release pump 1 & Steering</span></td>
          </tr>
          <tr class="row-cable" data-cat="control">
            <td style="text-align: center; color: var(--text-muted);">82</td>
            <td><code style="color: var(--accent-orange); font-weight: 700;">=pph-W10.1</code></td>
            <td><span class="subsystem-badge cable-badge-control">ĐIỀU KHIỂN</span></td>
            <td><strong>M12 Kabel m. Stecker gew. 1,5m</strong><br><span style="font-size: 0.7rem; color: var(--text-muted);">M12 4-pin (L=1.5m, 2 lõi)</span></td>
            <td><code>=pph+hydr-K5.2</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pre Links/Rechts</span></td>
            <td><code>=pph-X10.1</code><br><span style="font-size: 0.72rem; color: var(--text-muted);">Pre valves direction control</span></td>
          </tr>

        </tbody>
      </table>
        </div>
    </div>

    <!-- Section 2: KLEMMENPLAN -->
    <div class="card" style="margin-top: 24px;">
      <div class="card-title">
        <span>SƠ ĐỒ ĐẤU DÂY TRÂM KẸP THỰC TẾ (KLEMMENPLAN STRIP TERMINALS)</span>
        <span class="tag">BẢN VẼ 2018221-VC SHEET 39-43 & 61</span>
      </div>
      <p style="font-size: 0.82rem; color: var(--text-muted); margin-bottom: 12px;">
        Chi tiết các trâm kẹp nối dây trung tâm phân phối nguồn và tín hiệu giữa Tủ chính (=ppm), Bàn điều khiển cabin (=cc), Trạm nguồn thủy lực (=pph), Đầu khiên (=bk) và Mặt bích ngoài (Schottleiste):
      </p>

        <div class="card" style="margin-top: 16px;">
          <div class="card-title">
            <span>TRÂM KẸP =ppm-X1: Trâm Kẹp Nguồn Điều Khiển & Dừng Khẩn Cấp (Hauptschaltschrank Sheet 39)</span>
            <span class="tag">=ppm-X1</span>
          </div>
          <div class="table-responsive">
<table class="plc-table" style="font-size: 0.78rem;">
            <thead>
              <tr>
                <th style="width: 140px;">Cực kẹp (Terminal)</th>
                <th style="width: 220px;">Đích bên ngoài (Dest. External)</th>
                <th style="width: 220px;">Đích nội bộ tủ (Dest. Internal)</th>
                <th style="width: 90px;">Cỡ dây</th>
                <th>Chức năng kỹ thuật & Tín hiệu liên kết</th>
              </tr>
            </thead>
            <tbody>
                        <tr>
            <td><code style="color: var(--accent-cyan);">1:a / 1:b / 1:Pe</code></td>
            <td><code>-F1.2 (3) / -F1.2 (1)</code></td>
            <td><code>-K2.1 (L2) / -K2.1 (L1)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Cấp nguồn biến áp điều khiển & Rơ le giám sát pha K2.1</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">2:a / 2:b / 2:Pe</code></td>
            <td><code>-F1.2 (5)</code></td>
            <td><code>-K2.1 (L3)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Pha L3 nguồn phụ trạm máy</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">3:a / 3:b / 3:Pe</code></td>
            <td><code>-K2.1 (11)</code></td>
            <td><code>-T2.1 (400V) / -T2.1 (0)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Nguồn sơ cấp 400VAC biến áp STI0,16 (-T2.1)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">4:a / 4:b / 4:Pe</code></td>
            <td><code>-T2.1 (24V) / -Q1.1 (32)</code></td>
            <td><code>-F2.1 (1) / -F2.1 (4) / -F2.1 (3)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Nguồn thứ cấp 24VAC qua aptomat B6A -F2.1 và cuộn thấp áp U< Q1.1</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">5:a / 5:b / 5:Pe</code></td>
            <td><code>-K2.2 (A2) / -F1.1 (A1)</code></td>
            <td><code>-F1.1 (A2) / -F1.1 (A1)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Nguồn cấp rơ le giám sát dòng rò 3UG4625 (-F1.1)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">6:a / 6:b / 6:Pe</code></td>
            <td><code>-F1.1 (12) / -F1.1 (11)</code></td>
            <td><code>-K2.2 (S21) / -K2.2 (S11)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.75mm²</span></td>
            <td>Chuỗi tiếp điểm an toàn tới Rơ le Emergency Off ESR5-NO-21 (-K2.2)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">7:a / 7:b / 7:Pe</code></td>
            <td><code>-W3.1 (1/2) -> =ppm-S3.1</code></td>
            <td><code>-K2.2 (S21/S11)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Kênh an toàn 1: Nút dừng khẩn phòng biến tần (VFD room E-Stop)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">8:a / 8:b / 8:Pe</code></td>
            <td><code>-W3.2 (1/2) -> =ppm-S3.2</code></td>
            <td><code>-S3.1 (2a/2b)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Kênh an toàn 2: Nút dừng khẩn phòng nguồn thủy lực (Power pack E-Stop)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">9:a / 9:b / 9:Pe</code></td>
            <td><code>-W3.3 (1/2) -> =cc+pult-S3.3</code></td>
            <td><code>-S3.2 (2a/2b)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Kênh an toàn 3: Nút dừng khẩn cabin điều khiển (Control panel E-Stop)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">10:a / 10:b / 10:Pe</code></td>
            <td><code>-W3.4 (1/2) -> =Schottleiste-X3.3</code></td>
            <td><code>=cc+pult-S3.3 (2a/2b)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Kênh an toàn 4: Giắc cắm dừng khẩn mặt bích chuyển tiếp giếng đẩy</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">11:a / 11:b / 11:Pe</code></td>
            <td><code>-W3.5 (1/2) -> =ppm-S3.4</code></td>
            <td><code>-K2.2 (S12/S22)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Kênh an toàn 5: Nút dừng khẩn đáy giếng (Shaft E-Stop) khép mạch về -K2.2</td>
          </tr>

            </tbody>
          </table>
</div>
        </div>
        <div class="card" style="margin-top: 16px;">
          <div class="card-title">
            <span>TRÂM KẸP =ppm-X5: Trâm Kẹp Cáp Lái & TACS 22 Cực (Hauptschaltschrank Sheet 40)</span>
            <span class="tag">=ppm-X5</span>
          </div>
          <div class="table-responsive">
<table class="plc-table" style="font-size: 0.78rem;">
            <thead>
              <tr>
                <th style="width: 140px;">Cực kẹp (Terminal)</th>
                <th style="width: 220px;">Đích bên ngoài (Dest. External)</th>
                <th style="width: 220px;">Đích nội bộ tủ (Dest. Internal)</th>
                <th style="width: 90px;">Cỡ dây</th>
                <th>Chức năng kỹ thuật & Tín hiệu liên kết</th>
              </tr>
            </thead>
            <tbody>
                        <tr>
            <td><code style="color: var(--accent-cyan);">1:a / 1:b</code></td>
            <td><code>Cáp LEHC rev.0 sợi BK / VT</code></td>
            <td><code>-B26.1 chân 8 / 9</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.34mm²</span></td>
            <td>Tín hiệu truyền thông bo mạch laser TACS (RS485 Rx/Tx)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">2:a / 2:b</code></td>
            <td><code>Cáp LEHC sợi PK / GY</code></td>
            <td><code>-B26.1 chân 10 / 11</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.34mm²</span></td>
            <td>Đồng bộ xung laser mục tiêu TACS</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">3:a / 3:b</code></td>
            <td><code>Cáp LEHC sợi GN / YE</code></td>
            <td><code>-B26.1 chân 6 / 7</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.34mm²</span></td>
            <td>Dữ liệu góc nghiêng / cảm biến bù tọa độ</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">4:a / 4:b</code></td>
            <td><code>Cáp LEHC sợi BU / RD</code></td>
            <td><code>-B26.2 chân Sig+ / Sig-</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.75mm²</span></td>
            <td>Tín hiệu Video camera gương đào đưa vào bộ thu NITEK TR560</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">5:a / 5:b</code></td>
            <td><code>Cáp LEHC sợi BK1 / BK2</code></td>
            <td><code>-Q21.1 chân 2 / 4</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Nguồn công suất 24VDC cấp ra đầu khiên đào</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">6:a / 6:b</code></td>
            <td><code>Cáp LEHC sợi BK3 / GNYE</code></td>
            <td><code>-Q21.1 / PE</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Dây âm 0VDC và nối đất chống nhiễu</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">7:a / 7:b</code></td>
            <td><code>Cáp LEHC sợi WH / BN</code></td>
            <td><code>-K22.4 chân A2 / B2</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">0.64mm</span></td>
            <td>Đường truyền bus Profibus DP kéo dài ra đầu khiên (-K22.7)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">8:a / 8:b / Pe</code></td>
            <td><code>Cáp LEHC sợi PK / GY vỏ kim loại</code></td>
            <td><code>Vỏ máy / PE chống nhiễu</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">Shield</span></td>
            <td>Vỏ bọc kim chống nhiễu cao tần trong môi trường hầm bùn</td>
          </tr>

            </tbody>
          </table>
</div>
        </div>
        <div class="card" style="margin-top: 16px;">
          <div class="card-title">
            <span>TRÂM KẸP =ppm-X7: Trâm Kẹp Nguồn 24VDC & Đo Lưu Lượng (Hauptschaltschrank Sheet 41-42)</span>
            <span class="tag">=ppm-X7</span>
          </div>
          <div class="table-responsive">
<table class="plc-table" style="font-size: 0.78rem;">
            <thead>
              <tr>
                <th style="width: 140px;">Cực kẹp (Terminal)</th>
                <th style="width: 220px;">Đích bên ngoài (Dest. External)</th>
                <th style="width: 220px;">Đích nội bộ tủ (Dest. Internal)</th>
                <th style="width: 90px;">Cỡ dây</th>
                <th>Chức năng kỹ thuật & Tín hiệu liên kết</th>
              </tr>
            </thead>
            <tbody>
                        <tr>
            <td><code style="color: var(--accent-cyan);">1:a / 1:b / 1:Pe</code></td>
            <td><code>-W20.2 -> =cc-X6</code></td>
            <td><code>-F20.4 chân 8 / 3 (Machine 6A)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">3x1.5mm²</span></td>
            <td>Cấp nguồn 24VDC có cầu chì điện tử cho IPC và màn hình cảm ứng cabin</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">2:a / 2:b</code></td>
            <td><code>-W21.5 -> =pph-X1.6</code></td>
            <td><code>-F21.1 chân 10 (PPH 10A)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">7/8" 5p</span></td>
            <td>Cấp nguồn 24VDC cho trạm nguồn thủy lực container BL67</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">3:a / 3:b</code></td>
            <td><code>-W21.3 -> =cc-X6</code></td>
            <td><code>-F21.1 chân 11 (CC 4A)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">3x1.5mm²</span></td>
            <td>Cấp nguồn nút bấm và đèn báo bàn điều khiển trung tâm</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">6:a / 6:b</code></td>
            <td><code>-K22.1 (L+/M) CPU</code></td>
            <td><code>-T20.1 (24V 40A)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">2.5mm²</span></td>
            <td>Nguồn cấp ổn định cho CPU 315-2 PN/DP</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">10:a / 10:b</code></td>
            <td><code>-K23.1 BL20</code></td>
            <td><code>-T20.1 (U_L / GND_L)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Nguồn nuôi module I/O Turck BL20 tủ chính</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">14:a / 14:b</code></td>
            <td><code>-W24.3 -> =ppm+Schottleiste</code></td>
            <td><code>-K23.5 (22/21) EW420</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">7x0.75mm²</span></td>
            <td>Tín hiệu dòng 4-20mA lưu lượng kế bùn cấp Charge line T24.1</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">15:a / 15:b</code></td>
            <td><code>-W24.3 -> =ppm+Schottleiste</code></td>
            <td><code>-K23.5 (23/24) EW422</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">7x0.75mm²</span></td>
            <td>Tín hiệu dòng 4-20mA lưu lượng kế bùn xả Discharge line T24.2</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">16:a / 16:b</code></td>
            <td><code>-W24.1 -> =Schottleiste-X24.3</code></td>
            <td><code>-K23.4 (4/5) EW616</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x0.75mm²</span></td>
            <td>Tín hiệu xung đếm mét bánh xe đo quãng đường Distance Wheel -B11</td>
          </tr>

            </tbody>
          </table>
</div>
        </div>
        <div class="card" style="margin-top: 16px;">
          <div class="card-title">
            <span>TRÂM KẸP =ppm-X8: Trâm Kẹp Cấp Nguồn Động Lực Động Cơ (Hauptschaltschrank Sheet 43)</span>
            <span class="tag">=ppm-X8</span>
          </div>
          <div class="table-responsive">
<table class="plc-table" style="font-size: 0.78rem;">
            <thead>
              <tr>
                <th style="width: 140px;">Cực kẹp (Terminal)</th>
                <th style="width: 220px;">Đích bên ngoài (Dest. External)</th>
                <th style="width: 220px;">Đích nội bộ tủ (Dest. Internal)</th>
                <th style="width: 90px;">Cỡ dây</th>
                <th>Chức năng kỹ thuật & Tín hiệu liên kết</th>
              </tr>
            </thead>
            <tbody>
                        <tr>
            <td><code style="color: var(--accent-cyan);">1 / 2 / 3 / Pe</code></td>
            <td><code>-W6.2 -> =ppm-M6.2</code></td>
            <td><code>-Q6.3 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x1.5mm²</span></td>
            <td>Động cơ quạt két dầu thủy lực #1 (0.55kW, 400V, 1.61A)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">4 / 5 / 6 / Pe</code></td>
            <td><code>-W6.3 -> =ppm-M6.3</code></td>
            <td><code>-Q6.4 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x1.5mm²</span></td>
            <td>Động cơ quạt két dầu thủy lực #2 (0.55kW, 400V, 1.61A)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">7 / 8 / 9 / Pe</code></td>
            <td><code>-W6.1 -> =ppm-M6.1</code></td>
            <td><code>-Q6.2 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4G2.5mm²</span></td>
            <td>Động cơ bơm thủy lực bẻ lái (7.5kW, 400V, 15.6A)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">10 / 11 / 12 / Pe</code></td>
            <td><code>-W7.1 -> =ppm-M7.1</code></td>
            <td><code>-F7.1 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4x1.5mm²</span></td>
            <td>Máy nén khí phụ trợ container (1.6kW, 400V, 3.5A)</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">13 / 14 / 15 / Pe</code></td>
            <td><code>-W7.2 -> =Schottleiste-X7.1</code></td>
            <td><code>-F7.2 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4G6mm²</span></td>
            <td>Ổ cắm công nghiệp cấp trạm vữa bentonite CEE 32A</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">16 / 17 / 18 / Pe</code></td>
            <td><code>-W7.3 -> =Schottleiste-X7.2</code></td>
            <td><code>-Q7.1 (2/4/6)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">4G2.5mm²</span></td>
            <td>Ổ cắm công nghiệp cấp đầu khiên đào CEE 16A</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">19 / N / PE</code></td>
            <td><code>-T15.1 (A1/A2)</code></td>
            <td><code>-F20.5 (1) / -T20.2 (0)</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">2.5mm²</span></td>
            <td>Nguồn điều khiển 230VAC cấp cho Khởi động mềm Sirius 3RW4447</td>
          </tr>

            </tbody>
          </table>
</div>
        </div>
        <div class="card" style="margin-top: 16px;">
          <div class="card-title">
            <span>TRÂM KẸP =pph+hydr-X10: Hộp Nối Dây Trạm Nguồn Thủy Lực (Hydraulikaggregat Sheet 61)</span>
            <span class="tag">=pph+hydr-X10</span>
          </div>
          <div class="table-responsive">
<table class="plc-table" style="font-size: 0.78rem;">
            <thead>
              <tr>
                <th style="width: 140px;">Cực kẹp (Terminal)</th>
                <th style="width: 220px;">Đích bên ngoài (Dest. External)</th>
                <th style="width: 220px;">Đích nội bộ tủ (Dest. Internal)</th>
                <th style="width: 90px;">Cỡ dây</th>
                <th>Chức năng kỹ thuật & Tín hiệu liên kết</th>
              </tr>
            </thead>
            <tbody>
                        <tr>
            <td><code style="color: var(--accent-cyan);">1:a / 1:b / 1:Pe</code></td>
            <td><code>-W2.6 -> =pph-X2.6</code></td>
            <td><code>-X2.6 chân 5/2/3</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">5x1.5mm²</span></td>
            <td>Cấp nguồn 24VDC cho dàn cuộn van điện từ trạm thủy lực</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">5:a / 5:b / 5:Pe</code></td>
            <td><code>-W4.2 -> =pph-X4.3</code></td>
            <td><code>-X4.1 chân 1/2</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">M12 4p</span></td>
            <td>Tín hiệu nhiệt độ dầu thủy lực EW404 từ cảm biến -B10</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">6:a / 6:b</code></td>
            <td><code>-K5.3 (VT-MSPA1)</code></td>
            <td><code>-K5.3 chân 1/2</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Tín hiệu điều khiển van tỷ lệ vận tốc kích đẩy -K1</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">7:a / 7:b / 7:Pe</code></td>
            <td><code>-W4.3 -> =pph-X4.4</code></td>
            <td><code>-X4.1 chân 6/7</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">M12 4p</span></td>
            <td>Tín hiệu độ ẩm dầu thủy lực EW408 từ cảm biến -B10</td>
          </tr>
          <tr>
            <td><code style="color: var(--accent-cyan);">8:a / 8:b</code></td>
            <td><code>-K5.1 (VT 11131)</code></td>
            <td><code>-K5.1 chân 1/2</code></td>
            <td><span style="font-size: 0.72rem; color: var(--text-muted);">1.5mm²</span></td>
            <td>Tín hiệu điều khiển van tỷ lệ áp suất kích đẩy -K3</td>
          </tr>

            </tbody>
          </table>
</div>
        </div>

    </div>

  </div>


<div id="tab-ref" class="tab-content">
    <div class="card">
      <div class="card-title">
        <span>BẢNG TRA CỨU KHỐI LỆNH (FC / FB) & VÙNG NHỚ DỮ LIỆU (DB) TRONG DỰ ÁN</span>
        <span class="tag">SIMATIC S7 CODEBASE</span>
      </div>

      <div class="table-responsive">
<table class="plc-table" style="font-size: 0.82rem;">
        <thead>
          <tr style="background: rgba(255, 255, 255, 0.05);">
            <th style="width: 100px;">Khối lệnh</th>
            <th style="width: 220px;">Tên ký hiệu (Symbol)</th>
            <th style="width: 240px;">Cụm thiết bị điều khiển</th>
            <th>Chức năng chi tiết trong chương trình PLC</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td class="val">OB1</td>
            <td>MainControl</td>
            <td>Toàn hệ thống</td>
            <td>Khối tổ chức chính thực thi chu trình quét tuần hoàn (14 Networks).</td>
          </tr>
          <tr>
            <td class="val">OB32 / OB33</td>
            <td>CYC_INT2 / 3</td>
            <td>Bộ lọc tốc độ</td>
            <td>Ngắt chu kỳ thời gian thực (Cyclic Interrupt) gọi FB100 tính vận tốc kích.</td>
          </tr>
          <tr>
            <td class="val">OB100</td>
            <td>COMPLETE RESTART</td>
            <td>Khởi tạo nguồn</td>
            <td>Nạp tham số offset, giá trị áp lực mặc định và reset cờ truyền thông.</td>
          </tr>
          <tr>
            <td class="val">FC14 / 25 / 26</td>
            <td>Cutter Control & Speed</td>
            <td>Đầu cắt Schürfrad</td>
            <td>Điều khiển chạy thuận/nghịch, tăng giảm tốc độ vô cấp và giới hạn áp lực cắt.</td>
          </tr>
          <tr>
            <td class="val">FC4 / FC150</td>
            <td>Q_Cutter_CSG</td>
            <td>Bơm nguồn CSG 132kW</td>
            <td>Tính toán đường đặc tính lưu lượng cho cụm bơm thủy lực CSG.</td>
          </tr>
          <tr>
            <td class="val">FC5 / FC140</td>
            <td>Q_Cutter_A4VG</td>
            <td>Bơm Rexroth A4VG 132kW</td>
            <td>Tính toán đường đặc tính lưu lượng cho cụm bơm thủy lực Rexroth A4VG.</td>
          </tr>
          <tr>
            <td class="val">FC20 / 21 / 22</td>
            <td>Jacks Control/Speed/Pres</td>
            <td>Trạm kích đẩy chính</td>
            <td>Điều áp van tỷ lệ kích chính, quy đổi áp suất sang lực Tấn (DB17).</td>
          </tr>
          <tr>
            <td class="val">FC55 / 56 / 57</td>
            <td>Interjack 1, 2, 3</td>
            <td>Trạm kích trung gian</td>
            <td>Đồng bộ hóa chu trình kích đẩy phân đoạn chống vỡ đốt cống bê tông.</td>
          </tr>
          <tr>
            <td class="val">FC40 / 41 / 42</td>
            <td>Steering cylinder 1..3</td>
            <td>3 Xi lanh lái khớp khiên</td>
            <td>Điều khiển bẻ góc khiên đào theo 3 trục 120°, tính lực phân bố (DB59).</td>
          </tr>
          <tr>
            <td class="val">FC80 / 81 / 82</td>
            <td>TACS / Inclinometer / Roll</td>
            <td>Định vị Laser & Đo nghiêng</td>
            <td>Tính độ lệch tâm laser, góc dốc Pitch và bù góc xoay khiên Roll.</td>
          </tr>
          <tr>
            <td class="val">FC43 / FC44</td>
            <td>Wing / Locking cylinder</td>
            <td>Cánh chống xoay khiên</td>
            <td>Bung cánh ghim vào vách đất khi mô-men đầu cắt làm xoay thân máy.</td>
          </tr>
          <tr>
            <td class="val">FC90 / FC91</td>
            <td>Hochdruck Pumpe / Valve</td>
            <td>Tia nước cao áp 400 bar</td>
            <td>Điều khiển bơm cao áp và béc phun nước mặt gương phá đất sét dính.</td>
          </tr>
          <tr>
            <td class="val">FC61 / FC71 / 72</td>
            <td>Freigaben / Fault Clr / 3</td>
            <td>Liên động & Bảo vệ an toàn</td>
            <td>Kiểm tra toàn bộ điều kiện liên động trước khi cho phép vận hành.</td>
          </tr>
          <tr>
            <td class="val">FC110 / FC112</td>
            <td>PKE / AltivarControl</td>
            <td>SmartWire & Biến tần VFD</td>
            <td>Đọc dòng tải motor từ rơ le điện tử PKE và giao tiếp biến tần Schneider Altivar.</td>
          </tr>
          <tr>
            <td class="val">DB1</td>
            <td>LV_DB</td>
            <td>Cảm biến đo chiều dài</td>
            <td>Lưu trữ xung đếm và chiều dài hầm đã đào (Längenvortrieb).</td>
          </tr>
          <tr>
            <td class="val">DB16 / DB17</td>
            <td>Analogvalues / Druck-Tonnen</td>
            <td>Bảng áp suất & Lực</td>
            <td>Chứa toàn bộ giá trị analog và bảng tra quy đổi Bar sang Tấn.</td>
          </tr>
          <tr>
            <td class="val">DB19</td>
            <td>Touchscreen</td>
            <td>Màn hình HMI</td>
            <td>Vùng nhớ trao đổi nút bấm và thanh trượt điều khiển với người vận hành.</td>
          </tr>
          <tr>
            <td class="val">DB57 / DB58</td>
            <td>DataLogging2 / Fault_DB</td>
            <td>Ghi dữ liệu & Báo lỗi</td>
            <td>Lưu trữ nhật ký khoan phục vụ SCADA và bảng cờ lỗi hệ thống.</td>
          </tr>
          <tr>
            <td class="val">DB102</td>
            <td>LV_Speed_DB</td>
            <td>Vận tốc kích đẩy</td>
            <td>Vận tốc kích tức thời sau khi qua bộ lọc mượt số FB100.</td>
          </tr>
        </tbody>
      </table>
</div>
    </div>
  </div>

  <script>
    // Tab Switching Function
    function switchTab(tabId) {
      document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
      document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));

      const activeTabEl = document.getElementById(tabId);
      if (activeTabEl) activeTabEl.classList.add('active');
      
      if (tabId === 'tab-sim') {
        document.getElementById('btnTabSim').classList.add('active');
        resizeCanvas();
      } else if (tabId === 'tab-diag') {
        document.getElementById('btnTabDiag').classList.add('active');
      } else if (tabId === 'tab-comp') {
        document.getElementById('btnTabComp').classList.add('active');
      } else if (tabId === 'tab-ref') {
        document.getElementById('btnTabRef').classList.add('active');
      }
    }

    function scrollToDiagram(diagId) {
      const el = document.getElementById(diagId);
      if (el) {
        el.scrollIntoView({ behavior: 'smooth', block: 'start' });
        document.querySelectorAll('.diag-sub-btn').forEach(btn => btn.classList.remove('active'));
        const targetBtn = document.querySelector(`.diag-sub-btn[onclick*="${diagId}"]`);
        if (targetBtn) {
          targetBtn.classList.add('active');
        }
      }
    }

    // Component Filter Function
    function filterComponents(category) {
      document.querySelectorAll('.comp-filter-btn').forEach(btn => btn.classList.remove('active'));
      if (event && event.target) event.target.classList.add('active');

      const rows = document.querySelectorAll('#compTable tbody tr');
      rows.forEach(row => {
        if (category === 'all') {
          row.style.display = '';
        } else if (category === 'sensor') {
          row.style.display = row.classList.contains('row-sensor') ? '' : 'none';
        } else if (category === 'di') {
          row.style.display = row.classList.contains('row-di') ? '' : 'none';
        } else if (category === 'actuator') {
          row.style.display = row.classList.contains('row-actuator') ? '' : 'none';
        } else if (category === 'drive') {
          row.style.display = row.classList.contains('row-drive') ? '' : 'none';
        } else if (category === 'hw') {
          row.style.display = row.classList.contains('row-hw') ? '' : 'none';
        }
      });
    }

    // Component Search Function
    function searchComponents() {
      const query = document.getElementById('compSearchInput').value.toLowerCase().trim();
      const rows = document.querySelectorAll('#compTable tbody tr');
      rows.forEach(row => {
        const text = row.innerText.toLowerCase();
        if (text.includes(query)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // State Variables for Simulation
    let isRunning = true;
    let isAuto = true;
    let cutterDir = 1;
    let cutterSpeedPct = 70;
    let jackSpeedPct = 60;
    let steeringAngle = 0;
    let jetActive = true;
    let isEStop = false;

    let distanceMm = 14820;
    let cutterAngle = 0;
    let strokeJack = 65;
    let rollAngle = 0.8;
    let laserOffsetX = 4;
    let laserOffsetY = -8;

    const canvas = document.getElementById('tbmCanvas');
    const ctx = canvas ? canvas.getContext('2d') : null;

    function resizeCanvas() {
      if (!canvas || !canvas.parentElement) return;
      canvas.width = canvas.parentElement.clientWidth;
      canvas.height = canvas.parentElement.clientHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    setTimeout(resizeCanvas, 150);

    function toggleAuto() {
      isAuto = !isAuto;
      document.getElementById('btnAuto').classList.toggle('active', isAuto);
      document.getElementById('btnManual').classList.toggle('active', !isAuto);
    }

    function setCutterDir(dir) {
      cutterDir = dir;
      document.getElementById('btnCutterCw').classList.toggle('active', dir === 1);
      document.getElementById('btnCutterCcw').classList.toggle('active', dir === -1);
    }

    function updateSliders() {
      cutterSpeedPct = parseInt(document.getElementById('sliderCutter').value);
      jackSpeedPct = parseInt(document.getElementById('sliderJacks').value);
      steeringAngle = parseFloat(document.getElementById('sliderSteering').value);

      document.getElementById('txtCutterSpeed').innerText = cutterSpeedPct + '%';
      document.getElementById('txtJackSpeed').innerText = jackSpeedPct + '%';
      document.getElementById('txtSteering').innerText = (steeringAngle > 0 ? '+' : '') + steeringAngle.toFixed(1) + '°';
    }

    function toggleJet() {
      jetActive = !jetActive;
      const btn = document.getElementById('btnJet');
      btn.classList.toggle('active', jetActive);
      const valveEl = document.getElementById('stHdValve');
      if (valveEl) {
        valveEl.innerText = jetActive ? 'OPEN' : 'CLOSED';
        valveEl.style.color = jetActive ? 'var(--accent-green)' : 'var(--text-muted)';
      }
    }

    function toggleEStop() {
      isEStop = !isEStop;
      const btn = document.getElementById('btnEStop');
      btn.classList.toggle('active', isEStop);
      
      const led = document.getElementById('plcLed');
      const fc61 = document.getElementById('fc61Status');
      const dbFault = document.getElementById('dbFault');

      if (isEStop) {
        led.className = 'led danger';
        fc61.innerText = 'E-STOP INTERLOCK';
        fc61.style.color = 'var(--accent-red)';
        dbFault.innerText = '1 (E-STOP ACTIVATED)';
        dbFault.style.color = 'var(--accent-red)';
      } else {
        led.className = 'led';
        fc61.innerText = 'FC61 OK';
        fc61.style.color = 'var(--accent-green)';
        dbFault.innerText = '0 (NO FAULT)';
        dbFault.style.color = 'var(--accent-green)';
      }
    }

    function animate() {
      if (!canvas || !ctx) return;
      const w = canvas.width;
      const h = canvas.height;

      ctx.clearRect(0, 0, w, h);

      if (!isEStop) {
        const speedMmSec = (jackSpeedPct / 100) * 0.8;
        distanceMm += speedMmSec;

        const rpm = (cutterSpeedPct / 100) * 6.0;
        cutterAngle += (rpm * 360 / 60) * 0.016 * cutterDir;

        strokeJack = (strokeJack + (speedMmSec * 0.05)) % 100;

        laserOffsetX = 4 + Math.sin(Date.now() * 0.002) * 2;
        laserOffsetY = -8 + Math.cos(Date.now() * 0.0015) * 3 + (steeringAngle * 0.5);
      }

      ctx.fillStyle = '#111726';
      ctx.fillRect(0, 0, w, h);

      const tunnelTop = h * 0.2;
      const tunnelBottom = h * 0.8;
      const tunnelHeight = tunnelBottom - tunnelTop;

      ctx.strokeStyle = '#1e2942';
      ctx.lineWidth = 1;
      for (let y = 15; y < h; y += 25) {
        ctx.beginPath();
        ctx.moveTo(0, y);
        ctx.lineTo(w, y);
        ctx.stroke();
      }

      const pipeStartX = 50;
      const tbmLength = 220;
      const headX = w * 0.65;
      const tailX = headX - tbmLength;

      const segWidth = 80;
      for (let px = pipeStartX; px < tailX; px += segWidth) {
        ctx.fillStyle = '#222f4d';
        ctx.fillRect(px, tunnelTop, segWidth - 4, tunnelHeight);
        ctx.strokeStyle = '#3b4e7a';
        ctx.strokeRect(px, tunnelTop, segWidth - 4, tunnelHeight);

        ctx.strokeStyle = '#00d2ff22';
        ctx.beginPath();
        ctx.moveTo(px + segWidth - 4, tunnelTop);
        ctx.lineTo(px + segWidth - 4, tunnelBottom);
        ctx.stroke();
      }

      ctx.strokeStyle = 'rgba(255, 0, 85, 0.4)';
      ctx.lineWidth = 2;
      ctx.setLineDash([8, 4]);
      ctx.beginPath();
      ctx.moveTo(0, h * 0.5);
      ctx.lineTo(tailX + 40, h * 0.5 + (steeringAngle * 0.8));
      ctx.stroke();
      ctx.setLineDash([]);

      ctx.save();
      ctx.translate(tailX, tunnelTop);
      ctx.rotate((steeringAngle * Math.PI) / 180 * 0.05);

      const gradShield = ctx.createLinearGradient(0, 0, 0, tunnelHeight);
      gradShield.addColorStop(0, '#3a4d78');
      gradShield.addColorStop(0.5, '#546b9e');
      gradShield.addColorStop(1, '#2a3859');
      ctx.fillStyle = gradShield;
      ctx.fillRect(0, 0, tbmLength, tunnelHeight);
      ctx.strokeStyle = '#00d2ff';
      ctx.lineWidth = 2;
      ctx.strokeRect(0, 0, tbmLength, tunnelHeight);

      ctx.strokeStyle = '#ff9100';
      ctx.lineWidth = 3;
      ctx.beginPath();
      ctx.moveTo(tbmLength * 0.65, 0);
      ctx.lineTo(tbmLength * 0.65, tunnelHeight);
      ctx.stroke();

      ctx.fillStyle = '#ff9100';
      ctx.fillRect(tbmLength * 0.5, 20, 35, 12);
      ctx.fillRect(tbmLength * 0.5, tunnelHeight - 32, 35, 12);

      ctx.fillStyle = '#00e676';
      ctx.fillRect(tbmLength * 0.3, -10, 30, 10);
      ctx.fillRect(tbmLength * 0.3, tunnelHeight, 30, 10);

      const cutterX = tbmLength;
      const cutterRadius = tunnelHeight * 0.52;
      const cutterCenterY = tunnelHeight * 0.5;

      ctx.save();
      ctx.translate(cutterX, cutterCenterY);
      ctx.rotate((cutterAngle * Math.PI) / 180);

      ctx.fillStyle = '#1c2842';
      ctx.beginPath();
      ctx.arc(0, 0, cutterRadius, -Math.PI / 2, Math.PI / 2);
      ctx.closePath();
      ctx.fill();
      ctx.strokeStyle = '#00f2fe';
      ctx.lineWidth = 3;
      ctx.stroke();

      for (let a = 0; a < Math.PI * 2; a += Math.PI / 3) {
        ctx.strokeStyle = '#00d2ff';
        ctx.lineWidth = 4;
        ctx.beginPath();
        ctx.moveTo(0, 0);
        ctx.lineTo(Math.cos(a) * cutterRadius, Math.sin(a) * cutterRadius);
        ctx.stroke();

        ctx.fillStyle = '#ff3d71';
        ctx.beginPath();
        ctx.arc(Math.cos(a) * (cutterRadius * 0.75), Math.sin(a) * (cutterRadius * 0.75), 6, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.restore();

      if (jetActive && !isEStop) {
        ctx.strokeStyle = 'rgba(0, 210, 255, 0.8)';
        ctx.lineWidth = 2;
        for (let i = 0; i < 5; i++) {
          ctx.beginPath();
          ctx.moveTo(cutterX, cutterCenterY - 40 + i * 20);
          ctx.lineTo(cutterX + 35 + Math.random() * 15, cutterCenterY - 50 + i * 25);
          ctx.stroke();
        }
      }

      ctx.restore();

      if (!isEStop) {
        const rpmVal = ((cutterSpeedPct / 100) * 5.8).toFixed(1);
        const presVal = Math.round(110 + (cutterSpeedPct * 1.2));
        const speedVal = ((jackSpeedPct / 100) * 38.0).toFixed(1);
        const forceVal = Math.round(250 + (jackSpeedPct * 4.5));

        const elDist = document.getElementById('metricDist');
        if (elDist) {
          elDist.innerText = (distanceMm / 1000).toFixed(3);
          document.getElementById('metricSpeed').innerText = speedVal;
          document.getElementById('metricCutterRpm').innerText = rpmVal;
          document.getElementById('metricForce').innerText = forceVal;
          document.getElementById('metricCutterPres').innerText = presVal;
          document.getElementById('metricMotorAmp').innerText = Math.round(80 + (cutterSpeedPct * 0.9));

          document.getElementById('dbDist').innerText = Math.round(distanceMm) + ' mm';
          document.getElementById('dbSpeed').innerText = speedVal + ' mm/min';
          document.getElementById('dbRpm').innerText = rpmVal + ' RPM';
          document.getElementById('dbPres').innerText = Math.round(180 + jackSpeedPct) + ' bar';

          const dot = document.getElementById('laserDot');
          if (dot) {
            dot.style.left = (50 + (laserOffsetX * 2.5)) + '%';
            dot.style.top = (50 + (laserOffsetY * 2.5)) + '%';
            document.getElementById('laserX').innerText = (laserOffsetX > 0 ? '+' : '') + laserOffsetX.toFixed(1) + ' mm';
            document.getElementById('laserY').innerText = (laserOffsetY > 0 ? '+' : '') + laserOffsetY.toFixed(1) + ' mm';
          }

          const bMain = document.getElementById('barMainJack');
          if (bMain) {
            bMain.style.width = Math.round(strokeJack) + '%';
            document.getElementById('valMainJack').innerText = Math.round(strokeJack) + '%';
            
            const d1 = Math.round((strokeJack * 0.8 + 15) % 100);
            document.getElementById('barD1').style.width = d1 + '%';
            document.getElementById('valD1').innerText = d1 + '%';

            const d2 = Math.round((strokeJack * 0.6 + 35) % 100);
            document.getElementById('barD2').style.width = d2 + '%';
            document.getElementById('valD2').innerText = d2 + '%';

            const d3 = Math.round((strokeJack * 0.9 + 5) % 100);
            document.getElementById('barD3').style.width = d3 + '%';
            document.getElementById('valD3').innerText = d3 + '%';
          }
        }
      }

      requestAnimationFrame(animate);
    }

    animate();
  
    // =========================================================================
    // FIGURE 3.5 CONTROL PANEL & VISAM SCADA CONTROLLERS
    // =========================================================================
    let bypassOpen = false;
    let jetOpen = true;
    let mainEngineRunning = true;
    let chargePumpRunning = true;
    let feedPumpRunning = true;
    let boosterPumpRunning = false;
    let cuttingDiscState = 'CW'; // 'LEFT', 'STOP', 'CW' (RIGHT)
    let jackingState = 'ADV'; // 'RET', 'STOP', 'ADV'
    let steerCylStates = { 10: 'mid', 11: 'mid', 12: 'mid', 13: 'down', 24: 'down', 25: 'up', 26: 'up' };
    let flushModeIdx = 2; // 0: SETUP, 1: MANUAL, 2: AUTOMATIC
    const flushModes = ['SETUP', 'MANUAL', 'AUTOMATIC'];

    function actuateFigItem(itemNum) {
      if (itemNum === 1) { // Bypass Open
        bypassOpen = true;
        const b1 = document.getElementById('btnFig01');
        const b2 = document.getElementById('btnFig02');
        if (b1) b1.classList.add('active-cyan');
        if (b2) b2.classList.remove('active-yellow');
        updateVisamValves();
      } else if (itemNum === 2) { // Bypass Closed
        bypassOpen = false;
        const b1 = document.getElementById('btnFig01');
        const b2 = document.getElementById('btnFig02');
        if (b1) b1.classList.remove('active-cyan');
        if (b2) b2.classList.add('active-yellow');
        updateVisamValves();
      } else if (itemNum === 3) { // Jet Open
        jetOpen = true;
        jetActive = true;
        const b3 = document.getElementById('btnFig03');
        const b4 = document.getElementById('btnFig04');
        if (b3) b3.classList.add('active-cyan');
        if (b4) b4.classList.remove('active-yellow');
        updateVisamValves();
      } else if (itemNum === 4) { // Jet Closed
        jetOpen = false;
        jetActive = false;
        const b3 = document.getElementById('btnFig03');
        const b4 = document.getElementById('btnFig04');
        if (b3) b3.classList.remove('active-cyan');
        if (b4) b4.classList.add('active-yellow');
        updateVisamValves();
      } else if (itemNum === 5) { // Main Engine ON
        mainEngineRunning = true;
        const b5 = document.getElementById('btnFig05');
        const b6 = document.getElementById('btnFig06');
        if (b5) b5.classList.add('active-green');
        if (b6) b6.classList.remove('active-red');
        const v = document.getElementById('visamMainEng');
        if (v) { v.innerText = 'RUNNING (145A)'; v.style.color = '#00e676'; }
      } else if (itemNum === 6) { // Main Engine OFF
        mainEngineRunning = false;
        const b5 = document.getElementById('btnFig05');
        const b6 = document.getElementById('btnFig06');
        if (b5) b5.classList.remove('active-green');
        if (b6) b6.classList.add('active-red');
        const v = document.getElementById('visamMainEng');
        if (v) { v.innerText = 'OFF (0A)'; v.style.color = '#ff3d71'; }
      } else if (itemNum === 8) { // Charge Pump ON
        chargePumpRunning = true;
        const b8 = document.getElementById('btnFig08');
        const b9 = document.getElementById('btnFig09');
        if (b8) b8.classList.add('active-green');
        if (b9) b9.classList.remove('active-red');
        updateVisamFlows();
      } else if (itemNum === 9) { // Charge Pump OFF
        chargePumpRunning = false;
        const b8 = document.getElementById('btnFig08');
        const b9 = document.getElementById('btnFig09');
        if (b8) b8.classList.remove('active-green');
        if (b9) b9.classList.add('active-red');
        updateVisamFlows();
      } else if (itemNum === 15) { // Feed Pump ON
        feedPumpRunning = true;
        const b15 = document.getElementById('btnFig15');
        const b16 = document.getElementById('btnFig16');
        if (b15) b15.classList.add('active-green');
        if (b16) b16.classList.remove('active-red');
        updateVisamFlows();
      } else if (itemNum === 16) { // Feed Pump OFF
        feedPumpRunning = false;
        const b15 = document.getElementById('btnFig15');
        const b16 = document.getElementById('btnFig16');
        if (b15) b15.classList.remove('active-green');
        if (b16) b16.classList.add('active-red');
        updateVisamFlows();
      } else if (itemNum === 21) { // Additional Pump ON
        boosterPumpRunning = true;
        const b21 = document.getElementById('btnFig21');
        const b22 = document.getElementById('btnFig22');
        if (b21) b21.classList.add('active-green');
        if (b22) b22.classList.remove('active-red');
      } else if (itemNum === 22) { // Additional Pump OFF
        boosterPumpRunning = false;
        const b21 = document.getElementById('btnFig21');
        const b22 = document.getElementById('btnFig22');
        if (b21) b21.classList.remove('active-green');
        if (b22) b22.classList.add('active-red');
      } else if (itemNum === 27) { // Cutter LEFT
        cuttingDiscState = 'LEFT';
        cutterSpeed = -3.8;
        const b27 = document.getElementById('btnFig27');
        const b28 = document.getElementById('btnFig28');
        const b29 = document.getElementById('btnFig29');
        if (b27) b27.classList.add('active-yellow');
        if (b28) b28.classList.remove('active-red');
        if (b29) b29.classList.remove('active-green');
        const vc = document.getElementById('visamCutterDir');
        if (vc) vc.innerText = 'LEFT (CCW)';
      } else if (itemNum === 28) { // Cutter STOP
        cuttingDiscState = 'STOP';
        cutterSpeed = 0.0;
        const b27 = document.getElementById('btnFig27');
        const b28 = document.getElementById('btnFig28');
        const b29 = document.getElementById('btnFig29');
        if (b27) b27.classList.remove('active-yellow');
        if (b28) b28.classList.add('active-red');
        if (b29) b29.classList.remove('active-green');
        const vc = document.getElementById('visamCutterDir');
        if (vc) vc.innerText = 'STOPPED';
        const vr = document.getElementById('visamCutterRpm');
        if (vr) vr.innerText = '0.0 RPM';
      } else if (itemNum === 29) { // Cutter RIGHT
        cuttingDiscState = 'CW';
        cutterSpeed = 3.8;
        const b27 = document.getElementById('btnFig27');
        const b28 = document.getElementById('btnFig28');
        const b29 = document.getElementById('btnFig29');
        if (b27) b27.classList.remove('active-yellow');
        if (b28) b28.classList.remove('active-red');
        if (b29) b29.classList.add('active-green');
        const vc = document.getElementById('visamCutterDir');
        if (vc) vc.innerText = 'RIGHT (CW)';
      } else if (itemNum === 30) { // Jacking Forward
        jackingState = 'ADV';
        const b30 = document.getElementById('btnFig30');
        const b31 = document.getElementById('btnFig31');
        const b32 = document.getElementById('btnFig32');
        if (b30) b30.classList.add('active-green');
        if (b31) b31.classList.remove('active-red');
        if (b32) b32.classList.remove('active-yellow');
        const vj = document.getElementById('visamJackStatus');
        if (vj) vj.innerText = 'ADVANCING';
      } else if (itemNum === 31) { // Jacking STOP
        jackingState = 'STOP';
        const b30 = document.getElementById('btnFig30');
        const b31 = document.getElementById('btnFig31');
        const b32 = document.getElementById('btnFig32');
        if (b30) b30.classList.remove('active-green');
        if (b31) b31.classList.add('active-red');
        if (b32) b32.classList.remove('active-yellow');
        const vj = document.getElementById('visamJackStatus');
        if (vj) vj.innerText = 'STOPPED';
      } else if (itemNum === 32) { // Jacking Back
        jackingState = 'RET';
        const b30 = document.getElementById('btnFig30');
        const b31 = document.getElementById('btnFig31');
        const b32 = document.getElementById('btnFig32');
        if (b30) b30.classList.remove('active-green');
        if (b31) b31.classList.remove('active-red');
        if (b32) b32.classList.add('active-yellow');
        const vj = document.getElementById('visamJackStatus');
        if (vj) vj.innerText = 'RETRACTING';
      }
    }

    function updateFigPoti(itemNum, val) {
      val = parseFloat(val);
      const valEl = document.getElementById('valFig' + (itemNum < 10 ? '0' + itemNum : itemNum));
      const knob = document.getElementById('knobFig' + (itemNum < 10 ? '0' + itemNum : itemNum));

      if (itemNum === 7) { // Charge speed
        if (valEl) valEl.innerText = val + '%';
        if (knob) knob.style.transform = 'rotate(' + (val * 2.7 - 135) + 'deg)';
        updateVisamFlows();
      } else if (itemNum === 14) { // Feed speed
        if (valEl) valEl.innerText = val + '%';
        if (knob) knob.style.transform = 'rotate(' + (val * 2.7 - 135) + 'deg)';
        updateVisamFlows();
      } else if (itemNum === 17) { // Cutting speed (0-6.0 RPM)
        if (valEl) valEl.innerText = val + ' RPM';
        if (knob) knob.style.transform = 'rotate(' + (val * 45 - 135) + 'deg)';
        if (cuttingDiscState !== 'STOP') {
          cutterSpeed = (cuttingDiscState === 'LEFT' ? -1 : 1) * val;
          const vr = document.getElementById('visamCutterRpm');
          if (vr) vr.innerText = val + ' RPM';
        }
      } else if (itemNum === 18) { // Jacking speed (0-50 mm/min)
        if (valEl) valEl.innerText = val + ' mm/min';
        if (knob) knob.style.transform = 'rotate(' + (val * 5.4 - 135) + 'deg)';
        const vjs = document.getElementById('visamJackSpeed');
        if (vjs) vjs.innerText = val + ' mm/min';
        if (typeof jackSpeedPct !== 'undefined') {
          jackSpeedPct = Math.round((val / 50) * 100);
        }
      } else if (itemNum === 19) { // Jacking pressure (50-400 bar)
        if (valEl) valEl.innerText = val + ' bar';
        if (knob) knob.style.transform = 'rotate(' + ((val - 50) * 0.77 - 135) + 'deg)';
        const vjp = document.getElementById('visamJackPres');
        const vjf = document.getElementById('visamJackForce');
        if (vjp) vjp.innerText = val + ' bar';
        if (vjf) vjf.innerText = Math.round(val * 1.34) + ' ton';
      } else if (itemNum === 20) { // Addition speed
        if (valEl) valEl.innerText = val + '%';
        if (knob) knob.style.transform = 'rotate(' + (val * 2.7 - 135) + 'deg)';
      }
    }

    function cycleRocker(itemNum) {
      const el = document.getElementById('rockerFig' + itemNum);
      const states = ['up', 'mid', 'down'];
      const cur = steerCylStates[itemNum] || 'mid';
      const nextIdx = (states.indexOf(cur) + 1) % states.length;
      const next = states[nextIdx];
      steerCylStates[itemNum] = next;

      if (el) {
        el.classList.remove('state-up', 'state-mid', 'state-down');
        el.classList.add('state-' + next);
      }

      // Deflect TACS laser on steer toggle
      if (typeof laserOffsetY !== 'undefined' && itemNum === 10) laserOffsetY = next === 'up' ? -8 : (next === 'down' ? 8 : 2.1);
      if (typeof laserOffsetX !== 'undefined' && itemNum === 11) laserOffsetX = next === 'up' ? 8 : (next === 'down' ? -8 : -1.5);
      if (itemNum === 26) {
        const isLocked = next !== 'down';
        const vls = document.getElementById('visamLockStatus');
        if (vls) {
          vls.innerText = isLocked ? '🔒 LOCKED' : '🔓 UNLOCKED';
          vls.style.color = isLocked ? '#00e676' : '#ff9100';
        }
      }
    }

    function cycleSelector23() {
      flushModeIdx = (flushModeIdx + 1) % flushModes.length;
      const mode = flushModes[flushModeIdx];
      const valEl = document.getElementById('valFig23');
      const visMode = document.getElementById('visamMode');
      const knob = document.getElementById('knobFig23');
      if (valEl) valEl.innerText = mode;
      if (visMode) visMode.innerText = 'MODE: ' + mode;
      if (knob) knob.style.transform = 'rotate(' + (flushModeIdx * 45) + 'deg)';
    }

    function updateVisamValves() {
      const vBypass = document.getElementById('visamValveBypass');
      const vJet = document.getElementById('visamValveJet');
      const txtBypass = document.getElementById('txtValveBypass');
      const txtJet = document.getElementById('txtValveJet');

      if (vBypass && txtBypass) {
        if (bypassOpen) {
          vBypass.className = 'visam-valve open';
          txtBypass.innerText = 'OPEN';
        } else {
          vBypass.className = 'visam-valve closed';
          txtBypass.innerText = 'CLOSED';
        }
      }

      if (vJet && txtJet) {
        if (jetOpen) {
          vJet.className = 'visam-valve open';
          txtJet.innerText = 'OPEN';
        } else {
          vJet.className = 'visam-valve closed';
          txtJet.innerText = 'CLOSED';
        }
      }
    }

    function updateVisamFlows() {
      const pCharge = document.getElementById('valFig07') ? parseInt(document.getElementById('valFig07').innerText) : 65;
      const pFeed = document.getElementById('valFig14') ? parseInt(document.getElementById('valFig14').innerText) : 70;

      const qCharge = chargePumpRunning ? ((pCharge / 100) * 65.0).toFixed(1) : '0.0';
      const qFeed = feedPumpRunning ? ((pFeed / 100) * 63.0).toFixed(1) : '0.0';

      const vcf = document.getElementById('visamChargeFlow');
      const vdf = document.getElementById('visamDischargeFlow');
      if (vcf) vcf.innerText = qCharge + ' m³/h';
      if (vdf) vdf.innerText = qFeed + ' m³/h';
    }

    // =========================================================================
    // SUB-SYSTEM & CABLE FILTERS & SEARCH
    // =========================================================================
    function filterBySub(sub) {
      const rows = document.querySelectorAll('#bomTableBody tr, #componentList .comp-card, .bom-row');
      rows.forEach(r => {
        if (sub === 'all') {
          r.style.display = '';
        } else {
          const text = r.innerText || '';
          r.style.display = text.includes(sub) ? '' : 'none';
        }
      });
      // Active class for buttons
      document.querySelectorAll('.filter-sub-btn').forEach(b => {
        if (b.getAttribute('onclick') && b.getAttribute('onclick').includes(sub)) {
          b.classList.add('active');
        } else {
          b.classList.remove('active');
        }
      });
    }

    function filterCables(cat) {
      const rows = document.querySelectorAll('#cablesTableBody tr, .cable-row');
      rows.forEach(r => {
        if (cat === 'all') {
          r.style.display = '';
        } else {
          const cType = r.getAttribute('data-type') || r.innerText || '';
          r.style.display = cType.toLowerCase().includes(cat.toLowerCase()) ? '' : 'none';
        }
      });
      document.querySelectorAll('.filter-cable-btn').forEach(b => {
        if (b.getAttribute('onclick') && b.getAttribute('onclick').includes(cat)) {
          b.classList.add('active');
        } else {
          b.classList.remove('active');
        }
      });
    }

    function searchCables() {
      const input = document.getElementById('cableSearchInput') || document.querySelector('input[placeholder*="cáp" i], input[placeholder*="cable" i]');
      if (!input) return;
      const q = input.value.toLowerCase().trim();
      const rows = document.querySelectorAll('#cablesTableBody tr, .cable-row');
      rows.forEach(r => {
        const text = (r.innerText || '').toLowerCase();
        r.style.display = text.includes(q) ? '' : 'none';
      });
    }

  
    // =========================================================================
    // 7-STAGE PIPELINE & ACTUATORS/SENSORS MATRIX CONTROLLER
    // =========================================================================
    let currentStage = 4;
    let autoSeqInterval = null;
    let autoSeqActive = false;

    function setStage(stageNum) {
      currentStage = stageNum;
      for (let i = 1; i <= 7; i++) {
        const card = document.getElementById('cardStage' + i);
        if (card) {
          if (i <= stageNum) {
            card.classList.add('active');
          } else {
            card.classList.remove('active');
          }
        }
      }

      const badge = document.getElementById('badgeCurrentStage');
      const stageNames = [
        '',
        'GĐ 1: KHỞI TẠO & AN TOÀN TỔNG (FC61)',
        'GĐ 2: TUẦN HOÀN BÙN BENTONITE (FC10/11)',
        'GĐ 3: ĐĨA CẮT & CHỐNG KẸT ÁP (FC25/26)',
        'GĐ 4: KÍCH ĐẨY CHÍNH & ĐỔI TẤN (FC20/22)',
        'GĐ 5: ĐỒNG BỘ KÍCH TRUNG GIAN (FC55)',
        'GĐ 6: LÁI 3D 120° & CHỐNG XOAY (FC40/43)',
        'GĐ 7: TIA NƯỚC SIÊU ÁP 400 BAR (FC90/91)'
      ];
      if (badge) badge.innerText = 'GIAI ĐOẠN HIỆN TẠI: ' + stageNames[stageNum];

      // Automatically align panel elements according to stage
      if (stageNum === 1) { // Safety & Init
        actuateFigItem(5); // Hyd main ON
      } else if (stageNum === 2) { // Slurry
        actuateFigItem(2); // Bypass Closed
        actuateFigItem(3); // Jet Open
        actuateFigItem(8); // Charge pump ON
        actuateFigItem(15); // Feed pump ON
      } else if (stageNum === 3) { // Cutter
        actuateFigItem(29); // Cutter Right
        updateFigPoti(17, 3.8);
      } else if (stageNum === 4) { // Jacking
        actuateFigItem(30); // Jacking Fwd
        updateFigPoti(18, 22);
        updateFigPoti(19, 220);
      } else if (stageNum === 5) { // Dehner
        // Simulate Dehner stroke
        const d1 = document.getElementById('matDehner1');
        const s1 = document.getElementById('matStateDehner1');
        if (d1) d1.innerText = '450 mm';
        if (s1) { s1.innerText = 'ADVANCING (A16.1)'; s1.className = 'matrix-state state-on'; }
      } else if (stageNum === 6) { // Steering & Anti-roll
        cycleRocker(10);
      } else if (stageNum === 7) { // High pressure water
        actuateFigItem(3);
      }
      updateMatrixVisuals();
    }

    function stepNextStage() {
      let next = currentStage + 1;
      if (next > 7) next = 1;
      setStage(next);
    }

    function toggleAutoSequence() {
      const btn = document.getElementById('btnAutoSeq');
      if (!autoSeqActive) {
        autoSeqActive = true;
        if (btn) {
          btn.innerText = '⏹ Dừng Auto Sequence';
          btn.classList.add('active-cyan');
        }
        autoSeqInterval = setInterval(() => {
          stepNextStage();
        }, 3500);
      } else {
        autoSeqActive = false;
        if (btn) {
          btn.innerText = '▶ Chạy Tự Động 7 Bước (Auto Seq)';
          btn.classList.remove('active-cyan');
        }
        clearInterval(autoSeqInterval);
      }
    }

    function updateMatrixVisuals() {
      // 1. Bypass & Jet
      const mCoilBypass = document.getElementById('matCoilBypass');
      const mLsBypass = document.getElementById('matLsBypass');
      const mCoilJet = document.getElementById('matCoilJet');
      const mLsJet = document.getElementById('matLsJet');

      if (mCoilBypass && mLsBypass) {
        if (bypassOpen) {
          mCoilBypass.innerText = 'A52.0 MỞ (ON)';
          mCoilBypass.className = 'matrix-state state-on';
          mLsBypass.innerText = 'E52.0 OPEN';
          mLsBypass.className = 'matrix-state state-on';
        } else {
          mCoilBypass.innerText = 'A52.1 ĐÓNG (OFF)';
          mCoilBypass.className = 'matrix-state state-off';
          mLsBypass.innerText = 'E52.1 CLOSED';
          mLsBypass.className = 'matrix-state state-warn';
        }
      }

      if (mCoilJet && mLsJet) {
        if (jetOpen) {
          mCoilJet.innerText = 'A52.2 MỞ (ON)';
          mCoilJet.className = 'matrix-state state-on';
          mLsJet.innerText = 'E52.2 OPEN';
          mLsJet.className = 'matrix-state state-on';
        } else {
          mCoilJet.innerText = 'A52.3 ĐÓNG (OFF)';
          mCoilJet.className = 'matrix-state state-off';
          mLsJet.innerText = 'E52.3 CLOSED';
          mLsJet.className = 'matrix-state state-warn';
        }
      }

      // 2. Coaxial Jet valves & High pres pump
      const mCoax = document.getElementById('matCoaxValves');
      const mPumpHP = document.getElementById('matPumpHighPres');
      if (mCoax && mPumpHP) {
        if (jetOpen && mainEngineRunning) {
          mCoax.innerText = '4 BÉC PHUN ON';
          mCoax.className = 'matrix-state state-on';
          mPumpHP.innerText = 'RUNNING (380 bar)';
          mPumpHP.className = 'matrix-state state-on';
        } else {
          mCoax.innerText = '4 BÉC PHUN TẮT';
          mCoax.className = 'matrix-state state-off';
          mPumpHP.innerText = 'STANDBY (0 bar)';
          mPumpHP.className = 'matrix-state state-off';
        }
      }

      // 3. Steering Cylinders
      const s1 = steerCylStates[10] || 'mid';
      const s2 = steerCylStates[11] || 'mid';
      const s3 = steerCylStates[12] || 'mid';

      const matS1 = document.getElementById('matStateSteer1');
      const matS2 = document.getElementById('matStateSteer2');
      const matS3 = document.getElementById('matStateSteer3');
      const bar1 = document.getElementById('barSteer1');
      const bar2 = document.getElementById('barSteer2');
      const bar3 = document.getElementById('barSteer3');
      const p1 = document.getElementById('matPSteer1');
      const p2 = document.getElementById('matPSteer2');
      const p3 = document.getElementById('matPSteer3');
      const pos1 = document.getElementById('matPosSteer1');
      const pos2 = document.getElementById('matPosSteer2');
      const pos3 = document.getElementById('matPosSteer3');

      if (matS1 && bar1) {
        if (s1 === 'up') {
          matS1.innerText = 'THÒ (A53.0)'; matS1.className = 'matrix-state state-on';
          bar1.style.width = '75%'; if (p1) p1.innerText = '185 bar'; if (pos1) pos1.innerText = '150 mm';
        } else if (s1 === 'down') {
          matS1.innerText = 'THỤT (A53.1)'; matS1.className = 'matrix-state state-warn';
          bar1.style.width = '25%'; if (p1) p1.innerText = '110 bar'; if (pos1) pos1.innerText = '50 mm';
        } else {
          matS1.innerText = 'HOLD'; matS1.className = 'matrix-state state-off';
          bar1.style.width = '50%'; if (p1) p1.innerText = '140 bar'; if (pos1) pos1.innerText = '100 mm';
        }
      }

      if (matS2 && bar2) {
        if (s2 === 'up') {
          matS2.innerText = 'THÒ (A53.2)'; matS2.className = 'matrix-state state-on';
          bar2.style.width = '70%'; if (p2) p2.innerText = '175 bar'; if (pos2) pos2.innerText = '140 mm';
        } else if (s2 === 'down') {
          matS2.innerText = 'THỤT (A53.3)'; matS2.className = 'matrix-state state-warn';
          bar2.style.width = '30%'; if (p2) p2.innerText = '105 bar'; if (pos2) pos2.innerText = '60 mm';
        } else {
          matS2.innerText = 'HOLD'; matS2.className = 'matrix-state state-off';
          bar2.style.width = '50%'; if (p2) p2.innerText = '138 bar'; if (pos2) pos2.innerText = '98 mm';
        }
      }

      if (matS3 && bar3) {
        if (s3 === 'up') {
          matS3.innerText = 'THÒ (A53.4)'; matS3.className = 'matrix-state state-on';
          bar3.style.width = '72%'; if (p3) p3.innerText = '178 bar'; if (pos3) pos3.innerText = '144 mm';
        } else if (s3 === 'down') {
          matS3.innerText = 'THỤT (A53.5)'; matS3.className = 'matrix-state state-warn';
          bar3.style.width = '28%'; if (p3) p3.innerText = '108 bar'; if (pos3) pos3.innerText = '56 mm';
        } else {
          matS3.innerText = 'HOLD'; matS3.className = 'matrix-state state-off';
          bar3.style.width = '50%'; if (p3) p3.innerText = '140 bar'; if (pos3) pos3.innerText = '100 mm';
        }
      }

      // 4. Lock & Wing
      const matLock = document.getElementById('matLockStatus');
      const isLocked = steerCylStates[26] !== 'down';
      if (matLock) {
        if (isLocked) {
          matLock.innerText = '🔒 LOCKED (M12.7=0)';
          matLock.className = 'matrix-state state-on';
        } else {
          matLock.innerText = '🔓 UNLOCKED (M12.7=1)';
          matLock.className = 'matrix-state state-warn';
        }
      }

      const matWing = document.getElementById('matWingStatus');
      const finState = steerCylStates[13] || 'down';
      if (matWing) {
        if (finState === 'up') {
          matWing.innerText = 'DEPLOYED (GHIM VÁCH - Q51.4)';
          matWing.className = 'matrix-state state-on';
        } else {
          matWing.innerText = 'RETRACTED (THU VÀO)';
          matWing.className = 'matrix-state state-off';
        }
      }

      // 5. Jacking State & Tonnage
      const matJDir = document.getElementById('matJackDir');
      const matPJ = document.getElementById('matPJack');
      const matFJ = document.getElementById('matFJack');
      const barJ = document.getElementById('barJackStroke');
      const curPJ = document.getElementById('valFig19') ? parseInt(document.getElementById('valFig19').innerText) : 220;

      if (matPJ) matPJ.innerText = curPJ + ' bar';
      if (matFJ) matFJ.innerText = Math.round(curPJ * 1.34) + ' TẤN';

      if (matJDir && barJ) {
        if (jackingState === 'ADV') {
          matJDir.innerText = 'TIẾN (A16.2/A17.2)';
          matJDir.className = 'matrix-state state-on';
          barJ.style.width = ((Date.now() / 80) % 100) + '%';
        } else if (jackingState === 'RET') {
          matJDir.innerText = 'LÙI (A16.0/A17.0)';
          matJDir.className = 'matrix-state state-warn';
          barJ.style.width = '20%';
        } else {
          matJDir.innerText = 'STOPPED';
          matJDir.className = 'matrix-state state-off';
        }
      }

      // 6. Cutter & Anti-Stall
      const matCutterP = document.getElementById('matCutterPres');
      const matAnti = document.getElementById('matAntiStall');
      if (matCutterP && matAnti) {
        if (cuttingDiscState !== 'STOP') {
          matCutterP.innerText = '185 bar';
          matAnti.innerText = 'BÌNH THƯỜNG (<300 bar)';
          matAnti.className = 'matrix-state state-on';
        } else {
          matCutterP.innerText = '0 bar';
          matAnti.innerText = 'STOPPED';
          matAnti.className = 'matrix-state state-off';
        }
      }
    }

  </script>
</body>
</html>
