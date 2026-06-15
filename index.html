<?php
/* ============================================================
   CME OI TERMINAL — Single File PHP+HTML
   Browser fetches CME (not blocked) → PHP caches it
   Auto refresh every 5 min in background
   By Imtiaz Ali
============================================================ */

define('CACHE_FILE',    __DIR__ . '/cme_cache.json');
define('CACHE_SECONDS', 300);

if(isset($_GET['api'])){
    header('Content-Type: application/json');
    header('Access-Control-Allow-Origin: *');
    header('Cache-Control: no-store');

    /* SAVE — browser POSTs fresh CME data here */
    if($_GET['api']==='save'){
        $raw = file_get_contents('php://input');
        if(!$raw){ echo json_encode(['status'=>'error']); exit; }
        $d = json_decode($raw,true);
        if(!$d){ echo json_encode(['status'=>'error']); exit; }
        $d['fetchedAt'] = date('Y-m-d H:i:s');
        $d['fetchedTs'] = time();
        file_put_contents(CACHE_FILE, json_encode($d), LOCK_EX);
        echo json_encode(['status'=>'ok','savedAt'=>$d['fetchedAt']]);
        exit;
    }

    /* DATA — return cached data instantly */
    if($_GET['api']==='data'){
        if(!file_exists(CACHE_FILE)){
            echo json_encode(['status'=>'empty','data'=>null,'needsRefresh'=>true]);
            exit;
        }
        $raw  = file_get_contents(CACHE_FILE);
        $d    = json_decode($raw,true);
        $age  = time()-($d['fetchedTs']??0);
        echo json_encode([
            'status'       => 'ok',
            'data'         => $d,
            'cacheAge'     => $age,
            'needsRefresh' => $age >= CACHE_SECONDS
        ]);
        exit;
    }

    echo json_encode(['status'=>'error']);
    exit;
}
?>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CME OI TERMINAL</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:'Segoe UI',Arial,sans-serif;background:#050b14;color:#e0e6f0;min-height:100vh;overflow-x:hidden;}

/* ===== LOADING ===== */
#loadingScreen{
    position:fixed;inset:0;background:#050b14;z-index:9999;
    display:flex;flex-direction:column;align-items:center;justify-content:center;
}
.ls-logo{font-size:clamp(26px,5vw,50px);font-weight:900;letter-spacing:6px;color:#fff;text-align:center;}
.ls-logo span{color:#00d4ff;}
.ls-tagline{font-size:clamp(9px,1.5vw,11px);color:#3a6080;letter-spacing:5px;text-transform:uppercase;margin-top:10px;text-align:center;padding:0 20px;}
.ls-ring-wrap{position:relative;width:110px;height:110px;margin:34px auto 0;}
.ls-ring{position:absolute;inset:0;border-radius:50%;border:2px solid transparent;animation:ringR 1.4s linear infinite;}
.ls-ring.r1{border-top-color:#00d4ff;animation-duration:1.2s;}
.ls-ring.r2{inset:12px;border-top-color:#0066cc;animation-duration:1.8s;animation-direction:reverse;}
.ls-ring.r3{inset:24px;border-top-color:#00ff88;animation-duration:2.2s;}
@keyframes ringR{to{transform:rotate(360deg);}}
.ls-center{position:absolute;inset:36px;background:radial-gradient(circle,#00d4ff15,transparent);border-radius:50%;border:1px solid #00d4ff22;display:flex;align-items:center;justify-content:center;font-size:20px;}
.ls-step{margin-top:26px;font-size:11px;color:#00d4ff;letter-spacing:3px;text-transform:uppercase;text-align:center;min-height:18px;padding:0 20px;}
.ls-bar-wrap{width:clamp(200px,50vw,320px);height:3px;background:#0f1e30;border-radius:2px;margin-top:16px;overflow:hidden;}
.ls-bar{height:100%;background:linear-gradient(90deg,#00d4ff,#0066cc,#00ff88);border-radius:2px;width:0%;transition:width 0.4s ease;}
.ls-foot{position:absolute;bottom:18px;font-size:9px;color:#1a3050;letter-spacing:3px;text-transform:uppercase;}
.ls-foot span{color:#00d4ff;}

/* ===== HEADER ===== */
.header{background:linear-gradient(135deg,#070f1c,#0d1b30);padding:13px 22px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid #0f2035;position:sticky;top:0;z-index:100;box-shadow:0 4px 20px rgba(0,0,0,0.5);flex-wrap:wrap;gap:10px;}
.hlogo{font-size:clamp(15px,3vw,22px);font-weight:900;letter-spacing:3px;color:#fff;}
.hlogo span{color:#00d4ff;}
.hright{display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.live-badge{display:flex;align-items:center;gap:6px;background:rgba(0,212,255,0.08);border:1px solid rgba(0,212,255,0.25);border-radius:20px;padding:5px 12px;font-size:11px;color:#00d4ff;font-weight:600;letter-spacing:1px;}
.live-dot{width:7px;height:7px;background:#00ff88;border-radius:50%;animation:pulse 1.5s infinite;}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1);}50%{opacity:0.4;transform:scale(1.4);}}
.refresh-info{font-size:11px;color:#3a6080;white-space:nowrap;}
.refresh-info span{color:#00d4ff;font-weight:700;}

/* ===== STATUS ===== */
.sbar{background:#060d18;padding:6px 22px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid #0a1a28;font-size:11px;flex-wrap:wrap;gap:6px;}
.stxt{color:#4a7090;}
.stxt span{color:#00d4ff;font-weight:600;}

/* ===== REFRESH OVERLAY — subtle top bar ===== */
#refreshBar{
    position:fixed;top:0;left:0;right:0;height:2px;
    background:linear-gradient(90deg,#00d4ff,#0066cc,#00ff88);
    z-index:9998;display:none;
    animation:barSlide 2s linear infinite;
}
@keyframes barSlide{0%{background-position:0% 50%;}100%{background-position:200% 50%;}}
#refreshBar.show{display:block;}

/* ===== TOAST ===== */
.toast{position:fixed;bottom:22px;right:18px;background:linear-gradient(135deg,#0d1b2a,#0f2035);border:1px solid rgba(0,212,255,0.3);border-radius:10px;padding:10px 16px;font-size:11px;color:#00d4ff;z-index:500;display:none;align-items:center;gap:10px;box-shadow:0 6px 24px rgba(0,0,0,0.4);animation:slideUp 0.3s ease;}
@keyframes slideUp{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}
.toast.show{display:flex;}
.t-spin{width:13px;height:13px;border:2px solid #0f2035;border-top:2px solid #00d4ff;border-radius:50%;animation:spin 0.8s linear infinite;flex-shrink:0;}
@keyframes spin{to{transform:rotate(360deg);}}

/* ===== MAIN ===== */
.main{padding:20px 22px;}
.tabs{display:flex;gap:4px;margin-bottom:22px;background:#070f1c;padding:5px;border-radius:10px;width:fit-content;border:1px solid #0f2035;flex-wrap:wrap;}
.tab{padding:9px 20px;border-radius:7px;border:none;font-size:12px;font-weight:700;cursor:pointer;letter-spacing:1px;transition:all 0.3s;color:#3a6080;background:transparent;text-transform:uppercase;}
.tab.active{background:linear-gradient(135deg,#00d4ff,#0066cc);color:#fff;box-shadow:0 4px 12px rgba(0,212,255,0.25);}
.tab:hover:not(.active){color:#00d4ff;}
.section{display:none;}
.section.active{display:block;}

/* ===== CARDS ===== */
.summary-row{display:grid;grid-template-columns:repeat(3,1fr);gap:13px;margin-bottom:22px;}
.sum-card{background:linear-gradient(135deg,#070f1c,#0d1828);border:1px solid #0f2035;border-radius:12px;padding:16px;display:flex;align-items:center;gap:13px;transition:border-color 0.3s;}
.sum-card:hover{border-color:#1a3a5f;}
.sum-icon{width:44px;height:44px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:19px;flex-shrink:0;}
.sum-icon.fx{background:rgba(0,212,255,0.12);border:1px solid rgba(0,212,255,0.25);}
.sum-icon.metals{background:rgba(255,215,0,0.12);border:1px solid rgba(255,215,0,0.25);}
.sum-icon.energy{background:rgba(255,120,0,0.12);border:1px solid rgba(255,120,0,0.25);}
.sum-label{font-size:10px;color:#3a6080;letter-spacing:2px;text-transform:uppercase;}
.sum-value{font-size:clamp(15px,2.5vw,19px);font-weight:700;color:#fff;margin-top:3px;}
.sum-sub{font-size:10px;color:#2a4060;margin-top:2px;}

/* ===== TABLE ===== */
.table-wrapper{background:#070f1c;border-radius:12px;border:1px solid #0f2035;overflow:hidden;margin-bottom:18px;}
.table-header{background:linear-gradient(135deg,#09141f,#0d1a2c);padding:13px 18px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid #0f2035;flex-wrap:wrap;gap:8px;}
.table-header h3{font-size:12px;font-weight:700;color:#fff;letter-spacing:1px;text-transform:uppercase;}
.badge{font-size:10px;padding:3px 10px;border-radius:20px;font-weight:600;letter-spacing:1px;}
.badge-fx{background:rgba(0,212,255,0.12);color:#00d4ff;border:1px solid rgba(0,212,255,0.25);}
.badge-metals{background:rgba(255,215,0,0.12);color:#ffd700;border:1px solid rgba(255,215,0,0.25);}
.badge-energy{background:rgba(255,140,0,0.12);color:#ff8c00;border:1px solid rgba(255,140,0,0.25);}
.tscroll{overflow-x:auto;-webkit-overflow-scrolling:touch;}
table{width:100%;border-collapse:collapse;min-width:500px;}
th{background:#050c16;padding:10px 15px;text-align:left;font-size:10px;color:#3a6080;font-weight:600;letter-spacing:2px;text-transform:uppercase;border-bottom:1px solid #0f2035;white-space:nowrap;}
td{padding:12px 15px;font-size:12px;border-bottom:1px solid #080f1a;vertical-align:middle;}
tr:last-child td{border-bottom:none;}
tr:hover td{background:rgba(0,212,255,0.03);cursor:pointer;}
.sym-name{font-weight:600;color:#c0d8f0;font-size:12px;}
.sym-tag{font-size:10px;color:#3a6080;margin-top:2px;}
.vol-val{color:#fff;font-weight:600;white-space:nowrap;}
.oi-val{color:#8ab0d0;white-space:nowrap;}
.chg-pos{color:#00ff88;font-weight:700;white-space:nowrap;}
.chg-neg{color:#ff4060;font-weight:700;white-space:nowrap;}
.chg-zero{color:#3a6080;white-space:nowrap;}
.chart-btn{background:rgba(0,212,255,0.08);color:#00d4ff;border:1px solid rgba(0,212,255,0.25);padding:5px 11px;border-radius:6px;font-size:10px;font-weight:600;cursor:pointer;transition:all 0.2s;white-space:nowrap;}
.chart-btn:hover{background:rgba(0,212,255,0.18);}

/* ===== MODAL ===== */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(3,7,14,0.93);z-index:3000;align-items:center;justify-content:center;padding:14px;}
.modal-overlay.show{display:flex;}
.modal{background:linear-gradient(160deg,#09141f,#070e18);border:1px solid #1a3050;border-radius:18px;width:100%;max-width:860px;max-height:90vh;overflow-y:auto;box-shadow:0 20px 60px rgba(0,0,0,0.7);}
.modal-header{padding:18px 24px;border-bottom:1px solid #0f2035;display:flex;align-items:center;justify-content:space-between;background:linear-gradient(135deg,#0b1a2c,#091525);border-radius:18px 18px 0 0;position:sticky;top:0;z-index:10;flex-wrap:wrap;gap:10px;}
.modal-title h2{font-size:clamp(13px,2.5vw,17px);font-weight:700;color:#fff;}
.modal-title p{font-size:10px;color:#3a6080;margin-top:3px;letter-spacing:2px;}
.modal-close{background:rgba(255,255,255,0.04);border:1px solid #1a3050;color:#5a8090;width:32px;height:32px;border-radius:8px;font-size:15px;cursor:pointer;transition:all 0.2s;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.modal-close:hover{background:rgba(255,60,60,0.12);color:#ff4060;border-color:rgba(255,60,60,0.4);}
.modal-body{padding:18px 24px;}
.chart-tabs{display:flex;gap:6px;margin-bottom:14px;flex-wrap:wrap;}
.ctab{padding:7px 16px;border-radius:6px;border:1px solid #0f2035;background:transparent;color:#3a6080;font-size:11px;font-weight:600;cursor:pointer;transition:all 0.2s;letter-spacing:1px;}
.ctab.active{background:rgba(0,212,255,0.12);color:#00d4ff;border-color:rgba(0,212,255,0.35);}
.chart-container{background:#050c16;border-radius:10px;border:1px solid #0f2035;padding:14px;margin-bottom:14px;height:250px;position:relative;}
.stats-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:14px;}
.stat-card{background:#050c16;border:1px solid #0f2035;border-radius:10px;padding:13px;text-align:center;}
.stat-label{font-size:9px;color:#3a6080;letter-spacing:2px;text-transform:uppercase;margin-bottom:5px;}
.stat-value{font-size:clamp(13px,2vw,17px);font-weight:700;color:#fff;}
.stat-sub{font-size:9px;color:#2a4060;margin-top:3px;}
.hist-table-wrap{background:#050c16;border-radius:10px;border:1px solid #0f2035;overflow:hidden;}
.hist-table-wrap h4{padding:11px 15px;font-size:10px;color:#5a8090;letter-spacing:2px;text-transform:uppercase;border-bottom:1px solid #0f2035;}
.hscroll{overflow-x:auto;}
.hist-table{width:100%;border-collapse:collapse;min-width:340px;}
.hist-table th{background:#040a12;padding:8px 13px;font-size:9px;color:#3a6080;letter-spacing:2px;text-transform:uppercase;font-weight:600;border-bottom:1px solid #0f2035;}
.hist-table td{padding:10px 13px;font-size:11px;border-bottom:1px solid #080f1a;}
.hist-table tr:last-child td{border-bottom:none;}
.empty-state{text-align:center;padding:46px 20px;color:#1a3050;}
.empty-state .icon{font-size:38px;margin-bottom:13px;}
.empty-state h3{font-size:15px;color:#2a4060;margin-bottom:6px;}

/* ===== FOOTER ===== */
.footer{text-align:center;padding:16px;color:#2a4060;font-size:10px;letter-spacing:2px;border-top:1px solid #080f1a;margin-top:8px;text-transform:uppercase;}
.footer span{color:#00d4ff;}

::-webkit-scrollbar{width:5px;height:5px;}
::-webkit-scrollbar-track{background:#050b14;}
::-webkit-scrollbar-thumb{background:#0f2035;border-radius:3px;}
::-webkit-scrollbar-thumb:hover{background:#00d4ff;}

@media(max-width:768px){
    .header{padding:11px 15px;}
    .main{padding:15px;}
    .summary-row{grid-template-columns:1fr 1fr;}
    .stats-grid{grid-template-columns:1fr 1fr;}
    .stats-grid .stat-card:last-child{grid-column:1/-1;}
    th:nth-child(5),td:nth-child(5){display:none;}
    .refresh-info{display:none;}
}
@media(max-width:480px){
    .summary-row{grid-template-columns:1fr;}
    .tabs{width:100%;}
    .tab{flex:1;text-align:center;padding:8px 6px;font-size:11px;}
    .modal-body{padding:13px 14px;}
    .stats-grid{grid-template-columns:1fr;}
    .stats-grid .stat-card:last-child{grid-column:auto;}
}
</style>
</head>
<body>

<!-- LOADING SCREEN -->
<div id="loadingScreen">
    <div class="ls-logo">CME <span>OI</span> TERMINAL</div>
    <div class="ls-tagline">Real-Time Open Interest · FX · Metals · Energy</div>
    <div class="ls-ring-wrap">
        <div class="ls-ring r1"></div>
        <div class="ls-ring r2"></div>
        <div class="ls-ring r3"></div>
        <div class="ls-center">📡</div>
    </div>
    <div class="ls-step" id="lsStep">Initializing...</div>
    <div class="ls-bar-wrap"><div class="ls-bar" id="lsBar"></div></div>
    <div class="ls-foot">Data Source: <span>CME GROUP</span> · By <span>Imtiaz Ali</span></div>
</div>

<!-- REFRESH BAR (top of page) -->
<div id="refreshBar"></div>

<!-- TOAST -->
<div class="toast" id="toast">
    <div class="t-spin"></div>
    <span id="toastMsg">Refreshing...</span>
</div>

<!-- MODAL -->
<div class="modal-overlay" id="modalOverlay">
<div class="modal">
    <div class="modal-header">
        <div class="modal-title">
            <h2 id="modalSymbolName">—</h2>
            <p>10-DAY HISTORICAL DATA · CME GROUP</p>
        </div>
        <button class="modal-close" onclick="closeModal()">✕</button>
    </div>
    <div class="modal-body">
        <div class="stats-grid" id="modalStats"></div>
        <div class="chart-tabs">
            <button class="ctab active" onclick="switchChart('volume',this)">📊 Volume</button>
            <button class="ctab" onclick="switchChart('oi',this)">📈 Open Interest</button>
            <button class="ctab" onclick="switchChart('change',this)">⚡ Change</button>
        </div>
        <div class="chart-container"><canvas id="mainChart"></canvas></div>
        <div class="hist-table-wrap">
            <h4>📋 Historical Records</h4>
            <div class="hscroll">
                <table class="hist-table">
                    <thead><tr>
                        <th>Date</th><th>Volume</th>
                        <th>Open Interest</th><th>Change</th>
                    </tr></thead>
                    <tbody id="histTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>
</div>

<!-- HEADER -->
<div class="header">
    <div class="hlogo">CME <span>OI</span> TERMINAL</div>
    <div class="hright">
        <div class="refresh-info">Last updated: <span id="lastUpdated">—</span> · Next: <span id="cdTimer">5:00</span></div>
        <div class="live-badge"><div class="live-dot"></div>LIVE</div>
    </div>
</div>

<!-- STATUS BAR -->
<div class="sbar">
    <div class="stxt" id="statusText">Loading...</div>
    <div style="font-size:11px;color:#2a4060">CME OI TERMINAL</div>
</div>

<!-- MAIN -->
<div class="main">
    <div class="tabs">
        <button class="tab active" onclick="switchTab('fx',this)">💱 FX</button>
        <button class="tab" onclick="switchTab('metals',this)">🥇 Metals</button>
        <button class="tab" onclick="switchTab('energy',this)">⚡ Energy</button>
    </div>

    <!-- FX -->
    <div class="section active" id="sec-fx">
        <div class="summary-row">
            <div class="sum-card"><div class="sum-icon fx">💱</div><div><div class="sum-label">FX Futures</div><div class="sum-value" id="fxCount">—</div><div class="sum-sub">Instruments</div></div></div>
            <div class="sum-card"><div class="sum-icon fx">📊</div><div><div class="sum-label">Total Volume</div><div class="sum-value" id="fxVol">—</div><div class="sum-sub">Contracts</div></div></div>
            <div class="sum-card"><div class="sum-icon fx">📈</div><div><div class="sum-label">Open Interest</div><div class="sum-value" id="fxOI">—</div><div class="sum-sub">Positions</div></div></div>
        </div>
        <div class="table-wrapper">
            <div class="table-header"><h3>FX Futures</h3><span class="badge badge-fx">FOREIGN EXCHANGE</span></div>
            <div class="tscroll"><div id="fxTC"><div class="empty-state"><div class="icon">💱</div><h3>Loading...</h3></div></div></div>
        </div>
    </div>

    <!-- METALS -->
    <div class="section" id="sec-metals">
        <div class="summary-row">
            <div class="sum-card"><div class="sum-icon metals">🥇</div><div><div class="sum-label">Metals Futures</div><div class="sum-value" id="metCount">—</div><div class="sum-sub">Instruments</div></div></div>
            <div class="sum-card"><div class="sum-icon metals">📊</div><div><div class="sum-label">Total Volume</div><div class="sum-value" id="metVol">—</div><div class="sum-sub">Contracts</div></div></div>
            <div class="sum-card"><div class="sum-icon metals">📈</div><div><div class="sum-label">Open Interest</div><div class="sum-value" id="metOI">—</div><div class="sum-sub">Positions</div></div></div>
        </div>
        <div class="table-wrapper">
            <div class="table-header"><h3>Metals Futures</h3><span class="badge badge-metals">PRECIOUS METALS</span></div>
            <div class="tscroll"><div id="metTC"><div class="empty-state"><div class="icon">🥇</div><h3>Loading...</h3></div></div></div>
        </div>
    </div>

    <!-- ENERGY -->
    <div class="section" id="sec-energy">
        <div class="summary-row">
            <div class="sum-card"><div class="sum-icon energy">⚡</div><div><div class="sum-label">Energy Futures</div><div class="sum-value" id="engCount">—</div><div class="sum-sub">Instruments</div></div></div>
            <div class="sum-card"><div class="sum-icon energy">📊</div><div><div class="sum-label">Total Volume</div><div class="sum-value" id="engVol">—</div><div class="sum-sub">Contracts</div></div></div>
            <div class="sum-card"><div class="sum-icon energy">📈</div><div><div class="sum-label">Open Interest</div><div class="sum-value" id="engOI">—</div><div class="sum-sub">Positions</div></div></div>
        </div>
        <div class="table-wrapper">
            <div class="table-header"><h3>Energy Futures</h3><span class="badge badge-energy">ENERGY MARKETS</span></div>
            <div class="tscroll"><div id="engTC"><div class="empty-state"><div class="icon">⚡</div><h3>Loading...</h3></div></div></div>
        </div>
    </div>
</div>

<!-- FOOTER -->
<div class="footer">Data Source <span>CME GROUP</span> BY <span>IMTIAZ ALI</span></div>

<script>
/* ============================================================
   YOUR ORIGINAL EXTRACTOR — 100% UNTOUCHED
============================================================ */
const FX = new Set([
    "Australian Dollar Futures","British Pound Futures",
    "Canadian Dollar Futures","Euro FX Futures",
    "Japanese Yen Futures","New Zealand Dollar Futures",
    "Swiss Franc Futures"
]);
const METALS = new Set(["Gold Futures","Silver Futures"]);
const ENERGY = new Set([
    "Crude Oil Futures","Natural Gas Futures",
    "WTI Crude Oil Futures","Henry Hub Natural Gas Futures"
]);
const reportPriority = ["P","F"];

function formatDate(d){
    const y=d.getFullYear();
    const m=String(d.getMonth()+1).padStart(2,"0");
    const day=String(d.getDate()).padStart(2,"0");
    return `${y}${m}${day}`;
}

async function fetchData(date,type,assetClassId){
    const url=`https://www.cmegroup.com/CmeWS/exp/voiProductsViewExport.ctl?media=xls&tradeDate=${date}&assetClassId=${assetClassId}&reportType=${type}&excluded=CEE,CEU,KCB`;
    const res=await fetch(url);
    const buffer=await res.arrayBuffer();
    const wb=XLSX.read(buffer,{type:"array"});
    const sheet=wb.Sheets[wb.SheetNames[0]];
    const rows=XLSX.utils.sheet_to_json(sheet,{header:1});
    return rows;
}

function extract(rows,symbolSet){
    let out=[];
    for(let i=0;i<rows.length;i++){
        let row=rows[i];
        let name=String(row[0]||"").trim();
        if(symbolSet.has(name)){
            out.push({
                symbol:name,
                volume:row[5]??null,
                openInterest:row[6]??null,
                change:row[7]??null
            });
        }
    }
    return out;
}

async function findLatest(){
    let d=new Date();
    for(let i=0;i<5;i++){
        let date=formatDate(d);
        for(let type of reportPriority){
            try{
                let fxRows=await fetchData(date,type,3);
                let fxData=extract(fxRows,FX);
                let metalRows=await fetchData(date,type,8);
                let metalData=extract(metalRows,METALS);
                let energyRows=await fetchData(date,type,7);
                let energyData=extract(energyRows,ENERGY);
                if(fxData.length===7&&metalData.length>=2&&energyData.length>=2){
                    return {date,type,FX:fxData,Metals:metalData,Energy:energyData};
                }
                if(fxData.length>0||metalData.length>0||energyData.length>0){
                    return {date,type,FX:fxData,Metals:metalData,Energy:energyData,partial:true};
                }
            }catch(e){console.log(e);}
        }
        d.setDate(d.getDate()-1);
    }
    return null;
}

const ASSET_ID={
    "Australian Dollar Futures":3,"British Pound Futures":3,
    "Canadian Dollar Futures":3,"Euro FX Futures":3,
    "Japanese Yen Futures":3,"New Zealand Dollar Futures":3,
    "Swiss Franc Futures":3,"Gold Futures":8,"Silver Futures":8,
    "Crude Oil Futures":7,"Natural Gas Futures":7,
    "WTI Crude Oil Futures":7,"Henry Hub Natural Gas Futures":7
};

async function fetchHistory(sym,assetId,latestDate){
    let history=[];
    let tradingDays=[];
    let td=new Date(
        parseInt(latestDate.slice(0,4)),
        parseInt(latestDate.slice(4,6))-1,
        parseInt(latestDate.slice(6,8))
    );
    while(tradingDays.length<10){
        let dow=td.getDay();
        if(dow!==0&&dow!==6) tradingDays.unshift(formatDate(td));
        td.setDate(td.getDate()-1);
    }
    for(let hDate of tradingDays){
        for(let hType of reportPriority){
            try{
                let rows=await fetchData(hDate,hType,assetId);
                let found=extract(rows,new Set([sym]));
                if(found.length>0){
                    history.push({
                        date:hDate,
                        volume:found[0].volume,
                        openInterest:found[0].openInterest,
                        change:found[0].change
                    });
                    break;
                }
            }catch(e){}
        }
    }
    return history;
}

/* ============================================================
   STATE
============================================================ */
let gData={FX:[],Metals:[],Energy:[]};
let hData={};
let chartInst=null;
let curChartKey='volume';
let curSymbol='';
let cdVal=300;
let isFetching=false;
let refreshInterval=null;

const SHORT={
    "Australian Dollar Futures":"AUD/USD","British Pound Futures":"GBP/USD",
    "Canadian Dollar Futures":"USD/CAD","Euro FX Futures":"EUR/USD",
    "Japanese Yen Futures":"USD/JPY","New Zealand Dollar Futures":"NZD/USD",
    "Swiss Franc Futures":"USD/CHF","Gold Futures":"XAU/USD",
    "Silver Futures":"XAG/USD","Crude Oil Futures":"CL",
    "Natural Gas Futures":"NG","WTI Crude Oil Futures":"WTI",
    "Henry Hub Natural Gas Futures":"HH"
};

/* ============================================================
   UTILS
============================================================ */
function numFmt(n){
    if(n===null||n===undefined||n==='') return '—';
    const v=Number(String(n).replace(/,/g,''));
    if(isNaN(v)) return '—';
    return v.toLocaleString('en-US');
}
function chgFmt(n){
    if(n===null||n===undefined||n==='') return {t:'—',c:'chg-zero'};
    const v=Number(String(n).replace(/,/g,''));
    if(isNaN(v)) return {t:'—',c:'chg-zero'};
    if(v>0) return {t:'+'+v.toLocaleString('en-US'),c:'chg-pos'};
    if(v<0) return {t:v.toLocaleString('en-US'),c:'chg-neg'};
    return {t:'0',c:'chg-zero'};
}
function pDate(s){
    if(!s||s.length<8) return s||'—';
    return s.slice(6,8)+'/'+s.slice(4,6)+'/'+s.slice(0,4);
}
function setStep(msg,pct){
    const s=document.getElementById('lsStep');
    const b=document.getElementById('lsBar');
    if(s) s.textContent=msg;
    if(b) b.style.width=pct+'%';
}
function showToast(msg){
    document.getElementById('toastMsg').textContent=msg;
    document.getElementById('toast').classList.add('show');
}
function hideToast(){
    document.getElementById('toast').classList.remove('show');
}
function showRefreshBar(){
    document.getElementById('refreshBar').classList.add('show');
}
function hideRefreshBar(){
    document.getElementById('refreshBar').classList.remove('show');
}
function hideLoading(){
    const ls=document.getElementById('loadingScreen');
    ls.style.transition='opacity 0.6s ease';
    ls.style.opacity='0';
    setTimeout(()=>{ls.style.display='none';},600);
}
function sleep(ms){return new Promise(r=>setTimeout(r,ms));}

/* ============================================================
   SAVE FRESH DATA TO PHP CACHE
============================================================ */
async function saveCache(data){
    try{
        await fetch('?api=save',{
            method:'POST',
            headers:{'Content-Type':'application/json'},
            body:JSON.stringify(data)
        });
    }catch(e){console.log('Save error:',e);}
}

/* ============================================================
   LOAD FROM PHP CACHE (instant)
============================================================ */
async function loadCache(){
    try{
        const res=await fetch('?api=data',{cache:'no-store'});
        const json=await res.json();
        if(json.status==='ok'&&json.data){
            return json;
        }
    }catch(e){}
    return null;
}

/* ============================================================
   APPLY DATA TO UI — replaces old data completely
============================================================ */
function applyData(d){
    /* Replace all data — old data auto deleted */
    gData={FX:d.FX||[],Metals:d.Metals||[],Energy:d.Energy||[]};
    hData=d.history||{};
    renderAll();

    /* Update header last updated time */
    if(d.fetchedAt){
        const t=new Date(d.fetchedAt).toLocaleTimeString('en-US',{hour:'2-digit',minute:'2-digit'});
        const el=document.getElementById('lastUpdated');
        if(el) el.textContent=t;
    }

    /* Update status bar */
    document.getElementById('statusText').innerHTML=
        `✅ Trade Date: <span>${pDate(d.tradeDate)}</span>`+
        ` &nbsp;|&nbsp; Report: <span>${d.reportType}</span>`+
        ` &nbsp;|&nbsp; FX: <span>${(d.FX||[]).length}</span>`+
        ` &nbsp;|&nbsp; Metals: <span>${(d.Metals||[]).length}</span>`+
        ` &nbsp;|&nbsp; Energy: <span>${(d.Energy||[]).length}</span>`;
}

/* ============================================================
   FETCH FRESH FROM CME (YOUR ORIGINAL EXTRACTOR)
   Then save to cache — replaces previous data
============================================================ */
async function fetchFresh(isBackground){
    if(isFetching) return; /* prevent double fetch */
    isFetching=true;

    try{
        if(isBackground){
            showRefreshBar();
            showToast('Refreshing CME data...');
        } else {
            setStep('Fetching live CME data...',25);
        }

        /* YOUR ORIGINAL findLatest */
        const result=await findLatest();

        if(!result){
            if(!isBackground) setStep('No CME data found',5);
            isFetching=false;
            hideRefreshBar();
            hideToast();
            return;
        }

        if(!isBackground) setStep('Building 10-day history...',55);

        /* Fetch history for every symbol */
        const allSyms=[
            ...result.FX.map(x=>x.symbol),
            ...result.Metals.map(x=>x.symbol),
            ...result.Energy.map(x=>x.symbol)
        ];

        const history={};
        for(let i=0;i<allSyms.length;i++){
            const sym=allSyms[i];
            if(!isBackground){
                setStep(`History: ${SHORT[sym]||sym}...`,55+Math.round((i/allSyms.length)*35));
            }
            history[sym]=await fetchHistory(sym,ASSET_ID[sym]||3,result.date);
        }

        /* Build payload */
        const payload={
            tradeDate:result.date,
            reportType:result.type,
            FX:result.FX,
            Metals:result.Metals,
            Energy:result.Energy,
            history:history
        };

        /* Save to PHP cache — this REPLACES old cache */
        await saveCache(payload);

        /* Set countdown back to 5 min */
        cdVal=300;

        /* Apply to UI — old data replaced */
        applyData(payload);

        if(!isBackground){
            setStep('Ready!',100);
            await sleep(300);
            hideLoading();
        } else {
            hideRefreshBar();
            hideToast();
            showSuccessToast();
        }

    }catch(e){
        console.error('fetchFresh error:',e);
        hideRefreshBar();
        hideToast();
        if(!isBackground){
            setStep('Error — retrying...',5);
            await sleep(4000);
            smartLoad(false);
        }
    }

    isFetching=false;
}

/* Show brief success toast */
function showSuccessToast(){
    const t=document.getElementById('toast');
    const m=document.getElementById('toastMsg');
    const sp=t.querySelector('.t-spin');
    if(sp) sp.style.display='none';
    m.textContent='✅ Data refreshed';
    t.classList.add('show');
    setTimeout(()=>{
        t.classList.remove('show');
        if(sp) sp.style.display='block';
    },3000);
}

/* ============================================================
   SMART LOAD
   1. Load cache instantly → show to user
   2. If expired → fetch fresh in background
   3. If empty → fetch fresh and wait
============================================================ */
async function smartLoad(isBackground){
    try{
        if(!isBackground) setStep('Checking server cache...',10);

        const cached=await loadCache();

        if(cached&&cached.data){
            /* Show cached data immediately — user sees data fast */
            applyData(cached.data);

            /* Sync countdown from cache age */
            cdVal=Math.max(0,300-(cached.cacheAge||0));

            if(!isBackground){
                setStep('Ready!',100);
                await sleep(300);
                hideLoading();
            }

            /* If expired → refresh in background silently */
            if(cached.needsRefresh){
                await fetchFresh(true);
            }
            return;
        }

        /* No cache at all — must fetch and wait */
        await fetchFresh(isBackground);

    }catch(e){
        console.error(e);
        if(!isBackground){
            setStep('Error — retrying...',5);
            await sleep(4000);
            smartLoad(false);
        }
    }
}

/* ============================================================
   AUTO REFRESH EVERY 5 MINUTES
   Runs in browser background silently
   Replaces old cached data with new
============================================================ */
function startAutoRefresh(){
    /* Clear any existing interval */
    if(refreshInterval) clearInterval(refreshInterval);

    refreshInterval=setInterval(async()=>{
        console.log('Auto refresh triggered');
        await fetchFresh(true);
    }, 5*60*1000); /* exactly 5 minutes */
}

/* ============================================================
   COUNTDOWN TIMER
============================================================ */
function tickCd(){
    if(cdVal>0) cdVal--;
    const m=String(Math.floor(cdVal/60)).padStart(1,'0');
    const s=String(cdVal%60).padStart(2,'0');
    const el=document.getElementById('cdTimer');
    if(el) el.textContent=m+':'+s;
}

/* ============================================================
   RENDER
============================================================ */
function renderAll(){
    renderSum('fxCount','fxVol','fxOI',gData.FX);
    renderSum('metCount','metVol','metOI',gData.Metals);
    renderSum('engCount','engVol','engOI',gData.Energy);
    renderTable('fxTC',gData.FX);
    renderTable('metTC',gData.Metals);
    renderTable('engTC',gData.Energy);
}
function renderSum(cId,vId,oId,data){
    document.getElementById(cId).textContent=data.length||'0';
    const tv=data.reduce((a,b)=>a+(Number(String(b.volume||0).replace(/,/g,''))||0),0);
    const to=data.reduce((a,b)=>a+(Number(String(b.openInterest||0).replace(/,/g,''))||0),0);
    document.getElementById(vId).textContent=numFmt(tv);
    document.getElementById(oId).textContent=numFmt(to);
}
function renderTable(id,data){
    if(!data||!data.length){
        document.getElementById(id).innerHTML='<div class="empty-state"><div class="icon">📭</div><h3>No Data</h3></div>';
        return;
    }
    const rows=data.map(item=>{
        const c=chgFmt(item.change);
        const tag=SHORT[item.symbol]||'';
        const spark=buildSpark(item.symbol);
        const safe=encodeURIComponent(item.symbol);
        return `<tr onclick="openModal('${safe}')">
            <td><div class="sym-name">${item.symbol}</div><div class="sym-tag">${tag}</div></td>
            <td class="vol-val">${numFmt(item.volume)}</td>
            <td class="oi-val">${numFmt(item.openInterest)}</td>
            <td class="${c.c}">${c.t}</td>
            <td>${spark}</td>
            <td><button class="chart-btn" onclick="event.stopPropagation();openModal('${safe}')">📊 Chart</button></td>
        </tr>`;
    }).join('');
    document.getElementById(id).innerHTML=
        `<table><thead><tr>
            <th>Instrument</th><th>Volume</th>
            <th>Open Interest</th><th>Change</th>
            <th>10D Trend</th><th>Detail</th>
        </tr></thead><tbody>${rows}</tbody></table>`;
}
function buildSpark(sym){
    const hist=hData[sym];
    if(!hist||hist.length<2) return '<span style="color:#1a3050;font-size:10px">···</span>';
    const vals=hist.map(h=>Number(String(h.volume||0).replace(/,/g,''))||0);
    const mn=Math.min(...vals),mx=Math.max(...vals),rng=mx-mn||1;
    const W=80,H=28,p=3,st=(W-p*2)/(vals.length-1);
    const pts=vals.map((v,i)=>{
        const x=p+i*st,y=H-p-((v-mn)/rng)*(H-p*2);
        return x.toFixed(1)+','+y.toFixed(1);
    }).join(' ');
    const last=vals[vals.length-1],prev=vals[vals.length-2];
    const col=last>=prev?'#00ff88':'#ff4060';
    const lx=(p+(vals.length-1)*st).toFixed(1);
    const ly=(H-p-((last-mn)/rng)*(H-p*2)).toFixed(1);
    return `<svg width="${W}" height="${H}" style="display:block;overflow:visible">
        <polyline points="${pts}" fill="none" stroke="${col}" stroke-width="1.8" stroke-linejoin="round" stroke-linecap="round"/>
        <circle cx="${lx}" cy="${ly}" r="2.5" fill="${col}"/>
    </svg>`;
}

/* ============================================================
   TABS
============================================================ */
function switchTab(id,btn){
    document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
    document.getElementById('sec-'+id).classList.add('active');
    btn.classList.add('active');
}

/* ============================================================
   MODAL
============================================================ */
function openModal(enc){
    const sym=decodeURIComponent(enc);
    curSymbol=sym;curChartKey='volume';
    document.getElementById('modalSymbolName').textContent=sym;
    document.getElementById('modalOverlay').classList.add('show');
    document.body.style.overflow='hidden';
    document.querySelectorAll('.ctab').forEach((t,i)=>t.classList.toggle('active',i===0));
    renderModalStats(sym);
    renderHistTable(sym);
    renderChart(sym,'volume');
}
function closeModal(){
    document.getElementById('modalOverlay').classList.remove('show');
    document.body.style.overflow='';
    if(chartInst){chartInst.destroy();chartInst=null;}
}
document.getElementById('modalOverlay').addEventListener('click',function(e){
    if(e.target===this) closeModal();
});
function renderModalStats(sym){
    const hist=hData[sym];
    if(!hist||!hist.length){
        document.getElementById('modalStats').innerHTML='<div style="color:#3a6080;font-size:12px;padding:10px;grid-column:1/-1">No historical data.</div>';
        return;
    }
    const lat=hist[hist.length-1],prv=hist.length>1?hist[hist.length-2]:null;
    const lv=Number(String(lat.volume||0).replace(/,/g,''))||0;
    const pv=prv?Number(String(prv.volume||0).replace(/,/g,''))||0:0;
    const lo=Number(String(lat.openInterest||0).replace(/,/g,''))||0;
    const po=prv?Number(String(prv.openInterest||0).replace(/,/g,''))||0:0;
    const vp=prv&&pv?((lv-pv)/pv*100).toFixed(1):null;
    const op=prv&&po?((lo-po)/po*100).toFixed(1):null;
    const av=hist.map(h=>Number(String(h.volume||0).replace(/,/g,''))||0);
    const avg=Math.round(av.reduce((a,b)=>a+b,0)/av.length);
    const mx=Math.max(...av);
    document.getElementById('modalStats').innerHTML=`
    <div class="stat-card">
        <div class="stat-label">Latest Volume</div>
        <div class="stat-value" style="color:#00d4ff">${numFmt(lv)}</div>
        <div class="stat-sub">${vp!==null?(vp>=0?'▲':'▼')+Math.abs(vp)+'% vs prev':'—'}</div>
    </div>
    <div class="stat-card">
        <div class="stat-label">Open Interest</div>
        <div class="stat-value" style="color:#ffd700">${numFmt(lo)}</div>
        <div class="stat-sub">${op!==null?(op>=0?'▲':'▼')+Math.abs(op)+'% vs prev':'—'}</div>
    </div>
    <div class="stat-card">
        <div class="stat-label">10D Avg Volume</div>
        <div class="stat-value" style="color:#00ff88">${numFmt(avg)}</div>
        <div class="stat-sub">Peak: ${numFmt(mx)}</div>
    </div>`;
}
function renderHistTable(sym){
    const hist=hData[sym];
    const tb=document.getElementById('histTableBody');
    if(!hist||!hist.length){
        tb.innerHTML='<tr><td colspan="4" style="text-align:center;color:#3a6080;padding:20px">No data</td></tr>';
        return;
    }
    tb.innerHTML=[...hist].reverse().map((h,i)=>{
        const c=chgFmt(h.change);
        return `<tr>
            <td style="color:${i===0?'#00d4ff':'#5a8090'};font-weight:${i===0?700:400}">
                ${pDate(h.date)}
                ${i===0?'<span style="font-size:9px;background:rgba(0,212,255,0.12);color:#00d4ff;padding:2px 6px;border-radius:4px;margin-left:6px">LATEST</span>':''}
            </td>
            <td class="vol-val">${numFmt(h.volume)}</td>
            <td class="oi-val">${numFmt(h.openInterest)}</td>
            <td class="${c.c}">${c.t}</td>
        </tr>`;
    }).join('');
}
function switchChart(key,btn){
    curChartKey=key;
    document.querySelectorAll('.ctab').forEach(t=>t.classList.remove('active'));
    btn.classList.add('active');
    if(chartInst){chartInst.destroy();chartInst=null;}
    renderChart(curSymbol,key);
}
function renderChart(sym,key){
    const hist=hData[sym];
    const ctx=document.getElementById('mainChart').getContext('2d');
    if(!hist||!hist.length) return;
    const labels=hist.map(h=>pDate(h.date));
    let vals,label,color,fill;
    if(key==='volume'){vals=hist.map(h=>Number(String(h.volume||0).replace(/,/g,''))||0);label='Volume';color='#00d4ff';fill='rgba(0,212,255,0.07)';}
    else if(key==='oi'){vals=hist.map(h=>Number(String(h.openInterest||0).replace(/,/g,''))||0);label='Open Interest';color='#ffd700';fill='rgba(255,215,0,0.07)';}
    else{vals=hist.map(h=>Number(String(h.change||0).replace(/,/g,''))||0);label='Change';color='#00ff88';fill='rgba(0,255,136,0.07)';}
    if(chartInst) chartInst.destroy();
    chartInst=new Chart(ctx,{
        type:'line',
        data:{labels,datasets:[{
            label,data:vals,borderColor:color,backgroundColor:fill,
            borderWidth:2.2,pointBackgroundColor:color,pointBorderColor:'#050c16',
            pointBorderWidth:2,pointRadius:4,pointHoverRadius:7,fill:true,tension:0.35
        }]},
        options:{
            responsive:true,maintainAspectRatio:false,
            interaction:{mode:'index',intersect:false},
            plugins:{
                legend:{display:false},
                tooltip:{backgroundColor:'#09141f',borderColor:'#1a3050',borderWidth:1,titleColor:'#5a8090',bodyColor:'#fff',padding:10,callbacks:{label:c=>`${label}: ${numFmt(c.parsed.y)}`}}
            },
            scales:{
                x:{grid:{color:'rgba(255,255,255,0.03)'},ticks:{color:'#3a6080',font:{size:9}}},
                y:{grid:{color:'rgba(255,255,255,0.03)'},ticks:{color:'#3a6080',font:{size:9},callback:v=>numFmt(v)}}
            }
        }
    });
}

/* ============================================================
   BOOT
============================================================ */
window.addEventListener('load',async()=>{
    /* 1. Load instantly from cache OR fetch fresh */
    await smartLoad(false);

    /* 2. Start countdown tick every second */
    setInterval(tickCd,1000);

    /* 3. Auto refresh every exactly 5 minutes */
    startAutoRefresh();
});
</script>
</body>
</html>
