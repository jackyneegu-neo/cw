<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Spread Trade Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg:        #f0f2f5;
  --bg2:       #ffffff;
  --bg3:       #f8f9fc;
  --border:    #dde1ea;
  --border2:   #c8cedb;
  --text:      #1a1f2e;
  --muted:     #6b7a99;
  --accent:    #d97706;
  --accent2:   #b45309;
  --long:      #1d4ed8;
  --long-bg:   #eff6ff;
  --long-bd:   #bfdbfe;
  --short:     #dc2626;
  --short-bg:  #fff1f1;
  --short-bd:  #fecaca;
  --pos:       #16a34a;
  --pos-bg:    #f0fdf4;
  --neg:       #dc2626;
  --neg-bg:    #fef2f2;
  --warn:      #d97706;
  --warn-bg:   #fffbeb;
  --mono:      'JetBrains Mono', monospace;
  --sans:      'DM Sans', sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{font-family:var(--sans);background:var(--bg);color:var(--text);min-height:100vh;padding:16px;font-size:14px;line-height:1.5}

/* ── TOP BAR ── */
.topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;padding-bottom:14px;border-bottom:1px solid var(--border);gap:12px;flex-wrap:wrap}
.topbar-left{display:flex;align-items:center;gap:12px}
.logo{font-family:var(--mono);font-size:13px;font-weight:700;letter-spacing:.12em;color:var(--accent);text-transform:uppercase}
.instrument-badge{font-size:11px;padding:3px 10px;border-radius:4px;border:1px solid var(--border2);color:var(--muted);font-family:var(--mono);letter-spacing:.06em}
.topbar-right{display:flex;align-items:center;gap:8px;flex-wrap:wrap}
.btn{font-size:11px;padding:5px 14px;border-radius:6px;border:1px solid var(--border2);background:var(--bg3);color:var(--muted);cursor:pointer;font-family:var(--mono);font-weight:600;letter-spacing:.06em;transition:all .15s}
.btn:hover{border-color:var(--border2);color:var(--text);background:var(--bg2)}
.btn-reset{border-color:#2a4a7a;color:var(--long);background:var(--long-bg)}
.btn-reset:hover{background:#1f3560}
.btn-clear{border-color:#5a2a2a;color:var(--short);background:var(--short-bg)}
.btn-clear:hover{background:#3a1818}
.net-badge{font-family:var(--mono);font-size:13px;font-weight:700;padding:5px 14px;border-radius:6px;border:1px solid;min-width:90px;text-align:center}
.net-pos{background:var(--pos-bg);border-color:#1a6b3a;color:var(--pos)}
.net-neg{background:var(--neg-bg);border-color:#6b1a1a;color:var(--neg)}
.net-neu{background:var(--bg3);border-color:var(--border);color:var(--muted)}

/* ── LAYOUT ── */
.main-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:14px}
.full-width{grid-column:1/-1}

/* ── PANELS ── */
.panel{background:var(--bg2);border:1px solid var(--border);border-radius:10px;overflow:hidden}
.panel-head{padding:10px 14px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border)}
.panel-head-long{background:var(--long-bg);border-bottom-color:var(--long-bd)}
.panel-head-short{background:var(--short-bg);border-bottom-color:var(--short-bd)}
.leg-label{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.12em}
.leg-label-long{color:var(--long)}
.leg-label-short{color:var(--short)}
.open-btn{font-size:10px;padding:3px 10px;border-radius:4px;border:none;cursor:pointer;font-family:var(--mono);font-weight:700;letter-spacing:.08em;transition:all .15s}
.open-on{background:#14321e;color:var(--pos);border:1px solid #1a6b3a}
.open-off{background:var(--bg3);color:var(--muted);border:1px solid var(--border)}
.panel-body{padding:0}

/* ── ROWS ── */
.row{display:flex;align-items:center;justify-content:space-between;padding:7px 14px;border-bottom:1px solid var(--border);transition:background .1s}
.row:last-child{border-bottom:none}
.row:hover{background:rgba(255,255,255,.02)}
.row-label{font-size:11px;color:var(--muted);font-family:var(--sans);flex:1;letter-spacing:.01em}
.row-input{width:120px;text-align:right;border:1px solid var(--border);border-radius:5px;background:var(--bg3);color:var(--text);font-family:var(--mono);font-size:12px;font-weight:600;outline:none;padding:4px 8px;transition:border-color .15s}
.row-input:focus{border-color:var(--accent);background:#ffffff;box-shadow:0 0 0 2px rgba(217,119,6,.15)}
.row-input::placeholder{color:var(--border2)}
.row-sep{height:1px;background:var(--border2);margin:4px 14px;opacity:.5}
.calc-val{font-family:var(--mono);font-size:12px;font-weight:700;text-align:right;min-width:120px;padding:2px 0}
.c-pos{color:var(--pos)}
.c-neg{color:var(--neg)}
.c-neu{color:var(--text)}
.c-warn{color:var(--warn)}
.c-muted{color:var(--muted)}
select.row-input{cursor:pointer;-webkit-appearance:none;appearance:none}

/* ── SECTION LABELS ── */
.section-label{font-family:var(--mono);font-size:10px;font-weight:700;letter-spacing:.14em;color:var(--muted);padding:8px 14px 6px;text-transform:uppercase;border-bottom:1px solid var(--border);background:rgba(255,255,255,.02)}

/* ── MTM CARDS ── */
.mtm-top{padding:12px 14px 8px;border-bottom:1px solid var(--border)}
.cur-price-row{display:flex;align-items:center;gap:10px;margin-bottom:0}
.cur-label{font-size:11px;color:var(--muted);white-space:nowrap;font-family:var(--mono);letter-spacing:.06em}
.cur-input{flex:1;border:1px solid var(--border2);border-radius:6px;padding:7px 12px;font-family:var(--mono);font-size:16px;font-weight:700;color:var(--accent2);background:var(--bg3);outline:none;text-align:right}
.cur-input:focus{border-color:var(--accent);background:#ffffff;box-shadow:0 0 0 2px rgba(217,119,6,.15)}
.mtm-cards{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;padding:10px 14px}
.mc{border-radius:7px;padding:10px 12px;text-align:center;border:1px solid var(--border)}
.mc-label{font-size:10px;color:var(--muted);letter-spacing:.08em;text-transform:uppercase;font-family:var(--mono);margin-bottom:4px}
.mc-val{font-family:var(--mono);font-size:17px;font-weight:700}

/* ── ANALYSIS PANEL ── */
.analysis-grid{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;padding:10px 14px}
.ac{border-radius:7px;padding:10px 12px;border:1px solid var(--border);background:var(--bg3)}
.ac-label{font-size:10px;color:var(--muted);letter-spacing:.08em;font-family:var(--mono);margin-bottom:5px;text-transform:uppercase}
.ac-val{font-family:var(--mono);font-size:14px;font-weight:700;color:var(--text)}
.ac-sub{font-size:10px;color:var(--muted);margin-top:3px;font-family:var(--mono)}

/* ── RISK METER ── */
.risk-bar-wrap{padding:10px 14px 14px}
.risk-bar-label{font-size:10px;color:var(--muted);font-family:var(--mono);letter-spacing:.08em;text-transform:uppercase;margin-bottom:6px;display:flex;justify-content:space-between;align-items:center}
.risk-bar-track{height:6px;background:#e5e9f2;border-radius:3px;overflow:hidden;border:1px solid var(--border)}
.risk-bar-fill{height:100%;border-radius:3px;transition:width .4s ease,background .4s ease;width:0%}
.risk-tags{display:flex;gap:6px;margin-top:8px;flex-wrap:wrap}
.risk-tag{font-size:10px;padding:3px 8px;border-radius:4px;font-family:var(--mono);font-weight:600;letter-spacing:.04em;border:1px solid}

/* ── EXIT / PAIR ── */
.exit-grid{display:grid;grid-template-columns:1fr 1fr;gap:0}
.exit-col{border-right:1px solid var(--border)}
.exit-col:last-child{border-right:none}
.pair-pnl-row{padding:12px 14px;display:flex;align-items:center;justify-content:space-between;border-top:1px solid var(--border2)}
.pair-label{font-size:11px;color:var(--muted);font-family:var(--mono);letter-spacing:.04em}
.pair-val{font-family:var(--mono);font-size:22px;font-weight:700}

/* ── PULLBACK ── */
.pb-grid{display:grid;grid-template-columns:1fr 1fr;gap:0}
.pb-col{border-right:1px solid var(--border)}
.pb-col:last-child{border-right:none}

/* ── IMBALANCE WARNING ── */
.imbalance-banner{margin:10px 14px;padding:8px 12px;border-radius:6px;font-size:11px;font-family:var(--mono);border:1px solid;display:none;align-items:center;gap:8px}
.imbalance-banner.show{display:flex}
.imbalance-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}

/* ── RESPONSIVE ── */
@media(max-width:700px){
  .main-grid{grid-template-columns:1fr}
  .full-width{grid-column:1}
  .analysis-grid{grid-template-columns:1fr 1fr}
  .mtm-cards{grid-template-columns:1fr 1fr}
  .mtm-cards .mc:last-child{grid-column:1/-1}
  .exit-grid{grid-template-columns:1fr}
  .exit-col{border-right:none;border-bottom:1px solid var(--border)}
  .exit-col:last-child{border-bottom:none}
  .pb-grid{grid-template-columns:1fr}
  .pb-col{border-right:none;border-bottom:1px solid var(--border)}
  .topbar{flex-direction:column;align-items:flex-start}
}
@media(max-width:420px){
  body{padding:10px}
  .analysis-grid{grid-template-columns:1fr 1fr}
  .row-input{width:100px;font-size:11px}
  .calc-val{min-width:100px;font-size:11px}
}
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-left">
    <span class="logo">SpreadCalc</span>
    <span class="instrument-badge">COMMODITIES</span>
  </div>
  <div class="topbar-right">
    <button class="btn btn-reset" id="btn-reset">↺ Reset</button>
    <button class="btn btn-clear" id="btn-clear">✕ Clear</button>
    <span class="net-badge net-neu" id="net-badge">—</span>
    <span style="font-size:10px;color:var(--muted);font-family:var(--mono)">NET MTM</span>
  </div>
</div>

<div class="main-grid">

  <!-- LONG LEG -->
  <div class="panel">
    <div class="panel-head panel-head-long">
      <span class="leg-label leg-label-long">▲ LONG LEG</span>
      <button class="open-btn open-on" id="long-open-btn" tabindex="-1">OPEN</button>
    </div>
    <div class="panel-body">
      <div class="section-label">Entry Setup</div>
      <div class="row"><span class="row-label">Entry price</span><input class="row-input" id="l_entry" type="number" placeholder="0" tabindex="1"></div>
      <div class="row"><span class="row-label">Qty (lots)</span><input class="row-input" id="l_qty" type="number" placeholder="0" tabindex="3"></div>
      <div class="row"><span class="row-label">Value / point</span><input class="row-input" id="l_vpp" type="number" placeholder="0" tabindex="5"></div>
      <div class="row"><span class="row-label">Default range</span><input class="row-input" id="l_range" type="number" placeholder="0" tabindex="7"></div>
      <div class="section-label">Risk Levels</div>
      <div class="row"><span class="row-label">Stop offset</span><input class="row-input" id="l_stop_off" type="number" placeholder="0" tabindex="9"></div>
      <div class="row"><span class="row-label">Target offset</span><input class="row-input" id="l_tgt_off" type="number" placeholder="0" tabindex="11"></div>
      <div class="section-label">Calculated</div>
      <div class="row"><span class="row-label">Stop price</span><span class="calc-val c-neg" id="l_stop_px">—</span></div>
      <div class="row"><span class="row-label">Target price</span><span class="calc-val c-pos" id="l_tgt_px">—</span></div>
      <div class="row"><span class="row-label">Break-even</span><span class="calc-val c-muted" id="l_be">—</span></div>
      <div class="row"><span class="row-label">P&amp;L at stop</span><span class="calc-val c-neg" id="l_stop_pnl">—</span></div>
      <div class="row"><span class="row-label">P&amp;L at target</span><span class="calc-val c-pos" id="l_tgt_pnl">—</span></div>
      <div class="row"><span class="row-label">Risk : Reward</span><span class="calc-val" id="l_rr">—</span></div>
    </div>
  </div>

  <!-- SHORT LEG -->
  <div class="panel">
    <div class="panel-head panel-head-short">
      <span class="leg-label leg-label-short">▼ SHORT LEG</span>
      <button class="open-btn open-on" id="short-open-btn" tabindex="-1">OPEN</button>
    </div>
    <div class="panel-body">
      <div class="section-label">Entry Setup</div>
      <div class="row"><span class="row-label">Entry price</span><input class="row-input" id="s_entry" type="number" placeholder="0" tabindex="2"></div>
      <div class="row"><span class="row-label">Qty (lots)</span><input class="row-input" id="s_qty" type="number" placeholder="0" tabindex="4"></div>
      <div class="row"><span class="row-label">Value / point</span><input class="row-input" id="s_vpp" type="number" placeholder="0" tabindex="6"></div>
      <div class="row"><span class="row-label">Default range</span><input class="row-input" id="s_range" type="number" placeholder="0" tabindex="8"></div>
      <div class="section-label">Risk Levels</div>
      <div class="row"><span class="row-label">Stop offset</span><input class="row-input" id="s_stop_off" type="number" placeholder="0" tabindex="10"></div>
      <div class="row"><span class="row-label">Target offset</span><input class="row-input" id="s_tgt_off" type="number" placeholder="0" tabindex="12"></div>
      <div class="section-label">Calculated</div>
      <div class="row"><span class="row-label">Stop price</span><span class="calc-val c-neg" id="s_stop_px">—</span></div>
      <div class="row"><span class="row-label">Target price</span><span class="calc-val c-pos" id="s_tgt_px">—</span></div>
      <div class="row"><span class="row-label">Break-even</span><span class="calc-val c-muted" id="s_be">—</span></div>
      <div class="row"><span class="row-label">P&amp;L at stop</span><span class="calc-val c-neg" id="s_stop_pnl">—</span></div>
      <div class="row"><span class="row-label">P&amp;L at target</span><span class="calc-val c-pos" id="s_tgt_pnl">—</span></div>
      <div class="row"><span class="row-label">Risk : Reward</span><span class="calc-val" id="s_rr">—</span></div>
    </div>
  </div>

  <!-- ANALYSIS -->
  <div class="panel full-width">
    <div class="section-label" style="border-bottom:none;padding-bottom:4px">Spread Analysis</div>
    <div class="analysis-grid">
      <div class="ac">
        <div class="ac-label">Max Loss</div>
        <div class="ac-val c-neg" id="a_maxloss">—</div>
        <div class="ac-sub">both legs at stop</div>
      </div>
      <div class="ac">
        <div class="ac-label">Max Gain</div>
        <div class="ac-val c-pos" id="a_maxgain">—</div>
        <div class="ac-sub">both legs at target</div>
      </div>
      <div class="ac">
        <div class="ac-label">Spread R:R</div>
        <div class="ac-val" id="a_rr">—</div>
        <div class="ac-sub" id="a_rr_sub">combined ratio</div>
      </div>
      <div class="ac">
        <div class="ac-label">Imbalance</div>
        <div class="ac-val" id="a_imb">—</div>
        <div class="ac-sub" id="a_imb_sub">lot size skew</div>
      </div>
    </div>
    <div class="risk-bar-wrap">
      <div class="risk-bar-label">
        <span>Risk Exposure</span>
        <span id="risk-pct-label" style="color:var(--text)">—</span>
      </div>
      <div class="risk-bar-track"><div class="risk-bar-fill" id="risk-bar"></div></div>
      <div class="risk-tags" id="risk-tags"></div>
    </div>
    <div class="imbalance-banner" id="imb-banner">
      <div class="imbalance-dot" id="imb-dot"></div>
      <span id="imb-text"></span>
    </div>
  </div>

  <!-- MTM -->
  <div class="panel full-width">
    <div class="section-label">Live Spread P&amp;L (MTM)</div>
    <div class="mtm-top">
      <div class="cur-price-row">
        <span class="cur-label">CUR PRICE</span>
        <input class="cur-input" id="cur_px" type="number" placeholder="Enter live price..." tabindex="13">
      </div>
    </div>
    <div class="mtm-cards">
      <div class="mc" id="mc-long" style="background:var(--long-bg);border-color:var(--long-bd)">
        <div class="mc-label">Long Unrealised</div>
        <div class="mc-val c-neu" id="m_long_unreal">—</div>
      </div>
      <div class="mc" id="mc-short" style="background:var(--short-bg);border-color:var(--short-bd)">
        <div class="mc-label">Short Unrealised</div>
        <div class="mc-val c-neu" id="m_short_unreal">—</div>
      </div>
      <div class="mc" id="mc-net" style="background:var(--bg3)">
        <div class="mc-label">Net Spread P&amp;L</div>
        <div class="mc-val c-neu" id="m_net">—</div>
      </div>
    </div>
  </div>

  <!-- EXIT -->
  <div class="panel full-width">
    <div class="section-label">Final Pair P&amp;L (Exit-Based)</div>
    <div class="exit-grid">
      <div class="exit-col">
        <div class="row"><span class="row-label">Long exit price</span><input class="row-input" id="l_exit" type="number" placeholder="—" tabindex="14"></div>
        <div class="row"><span class="row-label">Long realised P&amp;L</span><span class="calc-val c-neu" id="l_real">—</span></div>
      </div>
      <div class="exit-col">
        <div class="row"><span class="row-label">Short exit price</span><input class="row-input" id="s_exit" type="number" placeholder="—" tabindex="15"></div>
        <div class="row"><span class="row-label">Short realised P&amp;L</span><span class="calc-val c-neu" id="s_real">—</span></div>
      </div>
    </div>
    <div class="row" style="padding:7px 14px;border-top:1px solid var(--border)">
      <span class="row-label">Brokerage (per trade × 2)</span>
      <input class="row-input" id="brok" type="number" placeholder="0" tabindex="16">
    </div>
    <div class="pair-pnl-row">
      <span class="pair-label">NET PAIR P&amp;L (after brokerage)</span>
      <span class="pair-val c-neu" id="pair_pnl">—</span>
    </div>
  </div>

  <!-- PULLBACK -->
  <div class="panel full-width">
    <div class="section-label">Exit-Spread Controls (Pullback Band)</div>
    <div class="pb-grid">
      <div class="pb-col">
        <div class="row">
          <span class="row-label">Mode</span>
          <select class="row-input" id="pb_mode" tabindex="17">
            <option value="none">None</option>
            <option value="long">Long pullback</option>
            <option value="short">Short pullback</option>
          </select>
        </div>
        <div class="row"><span class="row-label">Pullback amount</span><input class="row-input" id="pb_amt" type="number" placeholder="0" tabindex="18"></div>
      </div>
      <div class="pb-col">
        <div class="row"><span class="row-label">Pullback exit price</span><span class="calc-val c-neu" id="pb_px">—</span></div>
        <div class="row"><span class="row-label">Realised if pullback hits</span><span class="calc-val c-neu" id="pb_long_real">—</span></div>
      </div>
    </div>
    <div class="row" style="border-top:1px solid var(--border)">
      <span class="row-label">Realised short if close now</span><span class="calc-val c-neu" id="pb_short_now">—</span>
    </div>
    <div class="pair-pnl-row">
      <span class="pair-label">TOTAL IF SCENARIO PLAYS OUT</span>
      <span class="pair-val c-neu" id="pb_total">—</span>
    </div>
  </div>

</div><!-- /main-grid -->

<script type="text/javascript">
(function(){

var DEFAULTS={
  l_entry:256000,l_qty:1,l_vpp:1,l_range:10000,l_stop_off:6000,l_tgt_off:5000,
  s_entry:256500,s_qty:1,s_vpp:1,s_range:10000,s_stop_off:3500,s_tgt_off:5500,
  cur_px:261397,l_exit:'',s_exit:'',brok:0,pb_amt:0,pb_mode:'none'
};

function g(id){var el=document.getElementById(id);if(!el)return 0;var v=parseFloat(el.value);return isNaN(v)?0:v;}
function gv(id){var el=document.getElementById(id);return el?el.value:'';}
function fmtN(n,dec){if(isNaN(n)||n===null)return '—';dec=dec||0;return n.toLocaleString('en-IN',{maximumFractionDigits:dec,minimumFractionDigits:dec});}
function fmt(n){if(n===null||isNaN(n))return '—';return(n>=0?'+':'')+n.toLocaleString('en-IN',{minimumFractionDigits:2,maximumFractionDigits:2});}
function setT(id,t){var el=document.getElementById(id);if(el)el.textContent=t;}
function setC(el,v,isMetric){
  var b=isMetric?'mc-val':'calc-val';
  if(v===null||isNaN(v)){el.className=b+' c-neu';return;}
  el.className=b+(v>0?' c-pos':v<0?' c-neg':' c-neu');
}
function setPairC(el,v){
  if(v===null||isNaN(v)){el.className='pair-val c-neu';return;}
  el.className='pair-val'+(v>0?' c-pos':v<0?' c-neg':' c-neu');
}

function rrLabel(stop,target){
  if(!stop||!target||stop===0)return '—';
  var r=Math.abs(target/stop);
  var cls=r>=2?'c-pos':r>=1?'c-warn':'c-neg';
  return {text:'1 : '+r.toFixed(2),cls:cls};
}

function calc(){
  var le=g('l_entry'),lq=g('l_qty'),lv=g('l_vpp'),lso=g('l_stop_off'),lto=g('l_tgt_off');
  var se=g('s_entry'),sq=g('s_qty'),sv=g('s_vpp'),sso=g('s_stop_off'),sto=g('s_tgt_off');
  var cp=g('cur_px'),brok=g('brok');

  /* ── LONG LEG ── */
  var lSP=le-lso, lTP=le+lto;
  var lSPnl=(lSP-le)*lq*lv, lTPnl=(lTP-le)*lq*lv;
  setT('l_stop_px',fmtN(lSP));
  setT('l_tgt_px', fmtN(lTP));
  setT('l_be', le>0?fmtN(le):'—');
  var el=document.getElementById('l_stop_pnl'); el.textContent=fmt(lSPnl); setC(el,lSPnl);
  el=document.getElementById('l_tgt_pnl');     el.textContent=fmt(lTPnl); setC(el,lTPnl);
  var lRR=rrLabel(Math.abs(lSPnl),Math.abs(lTPnl));
  el=document.getElementById('l_rr');
  if(lRR==='—'){el.textContent='—';el.className='calc-val c-neu';}
  else{el.textContent=lRR.text;el.className='calc-val '+lRR.cls;}

  /* ── SHORT LEG ── */
  var sSP=se+sso, sTP=se-sto;
  var sSPnl=(se-sSP)*sq*sv, sTPnl=(se-sTP)*sq*sv;
  setT('s_stop_px',fmtN(sSP));
  setT('s_tgt_px', fmtN(sTP));
  setT('s_be', se>0?fmtN(se):'—');
  el=document.getElementById('s_stop_pnl'); el.textContent=fmt(sSPnl); setC(el,sSPnl);
  el=document.getElementById('s_tgt_pnl');  el.textContent=fmt(sTPnl); setC(el,sTPnl);
  var sRR=rrLabel(Math.abs(sSPnl),Math.abs(sTPnl));
  el=document.getElementById('s_rr');
  if(sRR==='—'){el.textContent='—';el.className='calc-val c-neu';}
  else{el.textContent=sRR.text;el.className='calc-val '+sRR.cls;}

  /* ── ANALYSIS ── */
  var maxLoss = lSPnl + sSPnl;
  var maxGain = lTPnl + sTPnl;
  el=document.getElementById('a_maxloss'); el.textContent=fmt(maxLoss); setC(el,maxLoss);
  el=document.getElementById('a_maxgain'); el.textContent=fmt(maxGain); setC(el,maxGain);

  // Spread R:R
  var spreadRR = (maxLoss!==0)?Math.abs(maxGain/maxLoss):null;
  el=document.getElementById('a_rr');
  if(spreadRR!==null&&!isNaN(spreadRR)){
    el.textContent='1 : '+spreadRR.toFixed(2);
    el.className='ac-val'+(spreadRR>=2?' c-pos':spreadRR>=1?' c-warn':' c-neg');
    setT('a_rr_sub', spreadRR>=2?'favourable ratio':spreadRR>=1?'acceptable':'poor ratio');
  } else {
    el.textContent='—'; el.className='ac-val c-neu';
    setT('a_rr_sub','combined ratio');
  }

  // Imbalance — compare notional exposure of each leg
  var lNotional = lq * lv * (le||1);
  var sNotional = sq * sv * (se||1);
  var imbPct = (lNotional>0&&sNotional>0)?Math.abs(lNotional-sNotional)/Math.max(lNotional,sNotional)*100:null;
  el=document.getElementById('a_imb');
  var imbBanner=document.getElementById('imb-banner');
  if(imbPct!==null){
    el.textContent=imbPct.toFixed(1)+'%';
    if(imbPct>20){
      el.className='ac-val c-neg';
      setT('a_imb_sub','high imbalance');
      imbBanner.className='imbalance-banner show';
      document.getElementById('imb-dot').style.background='var(--neg)';
      imbBanner.style.background='var(--neg-bg)';
      imbBanner.style.borderColor='#6b1a1a';
      setT('imb-text','Warning: Leg notional imbalance is '+imbPct.toFixed(1)+'% — spread may not hedge cleanly. Consider adjusting lot sizes.');
    } else if(imbPct>10){
      el.className='ac-val c-warn';
      setT('a_imb_sub','moderate skew');
      imbBanner.className='imbalance-banner show';
      document.getElementById('imb-dot').style.background='var(--warn)';
      imbBanner.style.background='var(--warn-bg)';
      imbBanner.style.borderColor='#7a5f00';
      setT('imb-text','Note: Leg notional imbalance is '+imbPct.toFixed(1)+'% — monitor directional exposure.');
    } else {
      el.className='ac-val c-pos';
      setT('a_imb_sub','balanced');
      imbBanner.className='imbalance-banner';
    }
  } else {
    el.textContent='—'; el.className='ac-val c-neu';
    imbBanner.className='imbalance-banner';
  }

  // Risk bar — % of max gain used as risk
  var riskBar=document.getElementById('risk-bar');
  var riskPctLabel=document.getElementById('risk-pct-label');
  var riskTags=document.getElementById('risk-tags');
  if(maxGain>0&&maxLoss<0){
    var riskRatio=Math.abs(maxLoss)/maxGain*100;
    var barPct=Math.min(riskRatio,100);
    var barColor=barPct>80?'var(--neg)':barPct>50?'var(--warn)':'var(--pos)';
    riskBar.style.width=barPct+'%';
    riskBar.style.background=barColor;
    riskPctLabel.textContent=riskRatio.toFixed(0)+'% risk/reward';
    riskPctLabel.style.color=barColor;
    // Tags
    var tags=[];
    if(spreadRR!==null){tags.push({t:(spreadRR>=2?'Good R:R':spreadRR>=1?'Avg R:R':'Poor R:R'),c:spreadRR>=2?'var(--pos)':spreadRR>=1?'var(--warn)':'var(--neg)'});}
    if(imbPct!==null){tags.push({t:(imbPct>20?'Imbalanced':imbPct>10?'Slight Skew':'Balanced'),c:imbPct>20?'var(--neg)':imbPct>10?'var(--warn)':'var(--pos)'});}
    riskTags.innerHTML=tags.map(function(t){
      return '<span class="risk-tag" style="color:'+t.c+';border-color:'+t.c+';background:rgba(0,0,0,.3)">'+t.t+'</span>';
    }).join('');
  } else {
    riskBar.style.width='0%';
    riskPctLabel.textContent='—';
    riskTags.innerHTML='';
  }

  /* ── MTM ── */
  var lU=(cp-le)*lq*lv, sU=(se-cp)*sq*sv, net=lU+sU;
  el=document.getElementById('m_long_unreal'); el.textContent=fmt(lU); setC(el,lU,true);
  el=document.getElementById('m_short_unreal');el.textContent=fmt(sU); setC(el,sU,true);
  el=document.getElementById('m_net');         el.textContent=fmt(net);setC(el,net,true);
  var mc=document.getElementById('mc-net');
  mc.style.background=net>0?'var(--pos-bg)':net<0?'var(--neg-bg)':'var(--bg3)';
  mc.style.borderColor=net>0?'#1a6b3a':net<0?'#6b1a1a':'var(--border)';

  // Top badge
  var badge=document.getElementById('net-badge');
  if(cp===0||le===0){badge.textContent='—';badge.className='net-badge net-neu';}
  else{badge.textContent=(net>=0?'+':'')+net.toFixed(2);badge.className='net-badge '+(net>0?'net-pos':net<0?'net-neg':'net-neu');}

  /* ── EXIT ── */
  var lExv=gv('l_exit'),sExv=gv('s_exit');
  var lEv=lExv!==''?parseFloat(lExv):null, sEv=sExv!==''?parseFloat(sExv):null;
  var lR=lEv!==null?(lEv-le)*lq*lv:null, sR=sEv!==null?(se-sEv)*sq*sv:null;
  el=document.getElementById('l_real');
  if(lR!==null){el.textContent=fmt(lR);setC(el,lR);}else{el.textContent='—';el.className='calc-val c-neu';}
  el=document.getElementById('s_real');
  if(sR!==null){el.textContent=fmt(sR);setC(el,sR);}else{el.textContent='—';el.className='calc-val c-neu';}
  var pnlEl=document.getElementById('pair_pnl');
  if(lR!==null&&sR!==null){var pp=lR+sR-(brok*2);pnlEl.textContent=fmt(pp);setPairC(pnlEl,pp);}
  else{pnlEl.textContent='—';pnlEl.className='pair-val c-neu';}

  /* ── PULLBACK ── */
  var mode=document.getElementById('pb_mode').value, pba=g('pb_amt');
  var pbLR=document.getElementById('pb_long_real'),pbSN=document.getElementById('pb_short_now'),pbTot=document.getElementById('pb_total');
  if(mode==='long'&&pba>0){
    var pbPx=cp-pba,rl=(pbPx-le)*lq*lv,rs=(se-cp)*sq*sv,tot=rl+rs;
    setT('pb_px',fmtN(pbPx));
    pbLR.textContent=fmt(rl);setC(pbLR,rl);
    pbSN.textContent=fmt(rs);setC(pbSN,rs);
    pbTot.textContent=fmt(tot);setPairC(pbTot,tot);
  } else if(mode==='short'&&pba>0){
    var pbPx=cp+pba,rs=(se-pbPx)*sq*sv,rl=(cp-le)*lq*lv,tot=rl+rs;
    setT('pb_px',fmtN(pbPx));
    pbLR.textContent=fmt(rs);setC(pbLR,rs);
    pbSN.textContent=fmt(rl);setC(pbSN,rl);
    pbTot.textContent=fmt(tot);setPairC(pbTot,tot);
  } else {
    setT('pb_px','—');
    ['pb_long_real','pb_short_now'].forEach(function(id){var e=document.getElementById(id);if(e){e.textContent='—';e.className='calc-val c-neu';}});
    pbTot.textContent='—';pbTot.className='pair-val c-neu';
  }
}

function applyValues(vals){
  Object.keys(vals).forEach(function(id){var el=document.getElementById(id);if(el)el.value=vals[id];});
  var lb=document.getElementById('long-open-btn');if(lb){lb.textContent='OPEN';lb.className='open-btn open-on';}
  var sb=document.getElementById('short-open-btn');if(sb){sb.textContent='OPEN';sb.className='open-btn open-on';}
  calc();
}

function clearAll(){
  document.querySelectorAll('input').forEach(function(el){el.value='';});
  var pb=document.getElementById('pb_mode');if(pb)pb.value='none';
  ['l_stop_pnl','l_tgt_pnl','l_rr','s_stop_pnl','s_tgt_pnl','s_rr',
   'm_long_unreal','m_short_unreal','m_net',
   'l_real','s_real','pb_long_real','pb_short_now'].forEach(function(id){
    var e=document.getElementById(id);if(e){e.textContent='—';e.className=(id.indexOf('m_')===0?'mc-val':'calc-val')+' c-neu';}
  });
  ['l_stop_px','l_tgt_px','l_be','s_stop_px','s_tgt_px','s_be','pb_px'].forEach(function(id){setT(id,'—');});
  ['a_maxloss','a_maxgain','a_rr','a_imb'].forEach(function(id){var e=document.getElementById(id);if(e){e.textContent='—';e.className='ac-val c-neu';}});
  setT('a_rr_sub','combined ratio');setT('a_imb_sub','lot size skew');
  var pnl=document.getElementById('pair_pnl');if(pnl){pnl.textContent='—';pnl.className='pair-val c-neu';}
  var pbt=document.getElementById('pb_total');if(pbt){pbt.textContent='—';pbt.className='pair-val c-neu';}
  var badge=document.getElementById('net-badge');if(badge){badge.textContent='—';badge.className='net-badge net-neu';}
  var mc=document.getElementById('mc-net');if(mc){mc.style.background='var(--bg3)';mc.style.borderColor='var(--border)';}
  var rb=document.getElementById('risk-bar');if(rb)rb.style.width='0%';
  setT('risk-pct-label','—');
  var rt=document.getElementById('risk-tags');if(rt)rt.innerHTML='';
  document.getElementById('imb-banner').className='imbalance-banner';
  var lb=document.getElementById('long-open-btn');if(lb){lb.textContent='OPEN';lb.className='open-btn open-on';}
  var sb=document.getElementById('short-open-btn');if(sb){sb.textContent='OPEN';sb.className='open-btn open-on';}
}

function init(){
  document.querySelectorAll('input,select').forEach(function(el){
    el.addEventListener('input',calc);
    el.addEventListener('change',calc);
    el.addEventListener('keyup',calc);
  });
  var lb=document.getElementById('long-open-btn');
  if(lb)lb.addEventListener('click',function(){var on=this.textContent==='OPEN';this.textContent=on?'CLOSED':'OPEN';this.className='open-btn '+(on?'open-off':'open-on');});
  var sb=document.getElementById('short-open-btn');
  if(sb)sb.addEventListener('click',function(){var on=this.textContent==='OPEN';this.textContent=on?'CLOSED':'OPEN';this.className='open-btn '+(on?'open-off':'open-on');});
  document.getElementById('btn-reset').addEventListener('click',function(){applyValues(DEFAULTS);});
  document.getElementById('btn-clear').addEventListener('click',function(){clearAll();});
  clearAll();
}

if(document.readyState==='complete'||document.readyState==='interactive'){setTimeout(init,100);}
else{window.onload=init;document.addEventListener('DOMContentLoaded',init);setTimeout(init,800);}
})();
</script>
</body>
</html>
