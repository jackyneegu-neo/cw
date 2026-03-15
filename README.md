<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Spread Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#f0f2f5;--bg2:#ffffff;--bg3:#f8f9fc;
  --border:#dde1ea;--border2:#c8cedb;
  --text:#1a1f2e;--muted:#6b7a99;
  --accent:#d97706;--accent2:#b45309;
  --long:#1d4ed8;--long-bg:#eff6ff;--long-bd:#bfdbfe;
  --short:#dc2626;--short-bg:#fff1f1;--short-bd:#fecaca;
  --pos:#16a34a;--pos-bg:#f0fdf4;
  --neg:#dc2626;--neg-bg:#fef2f2;
  --warn:#d97706;--warn-bg:#fffbeb;
  --mono:'JetBrains Mono',monospace;
  --sans:'DM Sans',sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--sans);background:var(--bg);color:var(--text);min-height:100vh;padding:16px;font-size:14px}
.topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;padding-bottom:14px;border-bottom:1px solid var(--border);gap:12px;flex-wrap:wrap}
.topbar-left{display:flex;align-items:center;gap:12px}
.logo{font-family:var(--mono);font-size:13px;font-weight:700;letter-spacing:.12em;color:var(--accent)}
.instrument-badge{font-size:11px;padding:3px 10px;border-radius:4px;border:1px solid var(--border2);color:var(--muted);font-family:var(--mono)}
.topbar-right{display:flex;align-items:center;gap:8px;flex-wrap:wrap}
.btn{font-size:11px;padding:5px 14px;border-radius:6px;border:1px solid var(--border2);background:var(--bg3);color:var(--muted);cursor:pointer;font-family:var(--mono);font-weight:600;transition:all .15s}
.btn-reset{border-color:#bfdbfe;color:var(--long);background:var(--long-bg)}
.btn-clear{border-color:#fecaca;color:var(--short);background:var(--short-bg)}
.net-badge{font-family:var(--mono);font-size:13px;font-weight:700;padding:5px 14px;border-radius:6px;border:1px solid;min-width:90px;text-align:center}
.net-pos{background:var(--pos-bg);border-color:#bbf7d0;color:var(--pos)}
.net-neg{background:var(--neg-bg);border-color:#fecaca;color:var(--neg)}
.net-neu{background:var(--bg3);border-color:var(--border);color:var(--muted)}
.main-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:14px}
.full-width{grid-column:1/-1}
.panel{background:var(--bg2);border:1px solid var(--border);border-radius:10px;overflow:hidden}
.panel-head{padding:10px 14px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border)}
.panel-head-long{background:var(--long-bg);border-bottom-color:var(--long-bd)}
.panel-head-short{background:var(--short-bg);border-bottom-color:var(--short-bd)}
.leg-label{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.12em}
.leg-label-long{color:var(--long)}
.leg-label-short{color:var(--short)}
.open-btn{font-size:10px;padding:3px 10px;border-radius:4px;border:none;cursor:pointer;font-family:var(--mono);font-weight:700;letter-spacing:.08em}
.open-on{background:#dcfce7;color:var(--pos);border:1px solid #bbf7d0}
.open-off{background:var(--bg3);color:var(--muted);border:1px solid var(--border)}
.section-label{font-family:var(--mono);font-size:10px;font-weight:700;letter-spacing:.14em;color:var(--muted);padding:7px 14px 5px;text-transform:uppercase;border-bottom:1px solid var(--border);background:var(--bg3)}
.row{display:flex;align-items:center;justify-content:space-between;padding:7px 14px;border-bottom:1px solid var(--border)}
.row:last-child{border-bottom:none}
.row:hover{background:#f5f7fb}
.row-label{font-size:11px;color:var(--muted);flex:1}
.row-input{width:130px;text-align:right;border:1px solid var(--border);border-radius:5px;background:#fff;color:var(--text);font-family:var(--mono);font-size:12px;font-weight:600;outline:none;padding:5px 8px;transition:border-color .15s}
.row-input:focus{border-color:var(--accent);background:#fff;box-shadow:0 0 0 2px rgba(217,119,6,.12);color:var(--text)}
.row-input::placeholder{color:#c8cedb;font-weight:400}
.calc-val{font-family:var(--mono);font-size:12px;font-weight:700;text-align:right;min-width:130px}
.c-pos{color:var(--pos)}.c-neg{color:var(--neg)}.c-neu{color:var(--text)}.c-warn{color:var(--warn)}.c-muted{color:var(--muted)}
select.row-input{cursor:pointer}
.analysis-grid{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;padding:10px 14px}
.ac{border-radius:7px;padding:10px 12px;border:1px solid var(--border);background:var(--bg3)}
.ac-label{font-size:10px;color:var(--muted);letter-spacing:.08em;font-family:var(--mono);margin-bottom:5px;text-transform:uppercase}
.ac-val{font-family:var(--mono);font-size:14px;font-weight:700}
.ac-sub{font-size:10px;color:var(--muted);margin-top:3px;font-family:var(--mono)}
.risk-bar-wrap{padding:10px 14px 14px}
.risk-bar-label{font-size:10px;color:var(--muted);font-family:var(--mono);letter-spacing:.08em;text-transform:uppercase;margin-bottom:6px;display:flex;justify-content:space-between}
.risk-bar-track{height:6px;background:#e5e9f2;border-radius:3px;overflow:hidden;border:1px solid var(--border)}
.risk-bar-fill{height:100%;border-radius:3px;transition:width .4s,background .4s;width:0%}
.risk-tags{display:flex;gap:6px;margin-top:8px;flex-wrap:wrap}
.risk-tag{font-size:10px;padding:3px 8px;border-radius:4px;font-family:var(--mono);font-weight:600;border:1px solid}
.imbalance-banner{margin:0 14px 10px;padding:8px 12px;border-radius:6px;font-size:11px;font-family:var(--mono);border:1px solid;display:none;align-items:center;gap:8px}
.imbalance-banner.show{display:flex}
.imbalance-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}
.mtm-top{padding:12px 14px;border-bottom:1px solid var(--border)}
.cur-price-row{display:flex;align-items:center;gap:10px}
.cur-label{font-size:11px;color:var(--muted);white-space:nowrap;font-family:var(--mono);letter-spacing:.06em}
.cur-input{flex:1;border:1px solid var(--border2);border-radius:6px;padding:7px 12px;font-family:var(--mono);font-size:16px;font-weight:700;color:var(--accent2);background:#fff;outline:none;text-align:right}
.cur-input:focus{border-color:var(--accent);box-shadow:0 0 0 2px rgba(217,119,6,.12);color:var(--accent2)}
.mtm-cards{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;padding:10px 14px}
.mc{border-radius:7px;padding:10px 12px;text-align:center;border:1px solid var(--border);background:var(--bg3);transition:background .3s,border-color .3s}
.mc-label{font-size:10px;color:var(--muted);letter-spacing:.08em;text-transform:uppercase;font-family:var(--mono);margin-bottom:4px}
.mc-val{font-family:var(--mono);font-size:17px;font-weight:700}
.exit-grid{display:grid;grid-template-columns:1fr 1fr}
.exit-col{border-right:1px solid var(--border)}
.exit-col:last-child{border-right:none}
.pair-pnl-row{padding:12px 14px;display:flex;align-items:center;justify-content:space-between;border-top:1px solid var(--border2);background:var(--bg3)}
.pair-label{font-size:11px;color:var(--muted);font-family:var(--mono);letter-spacing:.04em}
.pair-val{font-family:var(--mono);font-size:22px;font-weight:700}
.pb-grid{display:grid;grid-template-columns:1fr 1fr}
.pb-col{border-right:1px solid var(--border)}
.pb-col:last-child{border-right:none}
@media(max-width:700px){
  .main-grid{grid-template-columns:1fr}.full-width{grid-column:1}
  .analysis-grid{grid-template-columns:1fr 1fr}
  .mtm-cards{grid-template-columns:1fr 1fr}.mtm-cards .mc:last-child{grid-column:1/-1}
  .exit-grid,.pb-grid{grid-template-columns:1fr}
  .exit-col,.pb-col{border-right:none;border-bottom:1px solid var(--border)}
  .exit-col:last-child,.pb-col:last-child{border-bottom:none}
  .topbar{flex-direction:column;align-items:flex-start}
}
@media(max-width:420px){
  body{padding:10px}
  .row-input{width:110px}.calc-val{min-width:110px;font-size:11px}
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
  <!-- LONG -->
  <div class="panel">
    <div class="panel-head panel-head-long">
      <span class="leg-label leg-label-long">▲ LONG LEG</span>
      <button class="open-btn open-on" id="long-open-btn" tabindex="-1">OPEN</button>
    </div>
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
    <div class="row" style="border-bottom:none"><span class="row-label">Risk : Reward</span><span class="calc-val c-muted" id="l_rr">—</span></div>
  </div>

  <!-- SHORT -->
  <div class="panel">
    <div class="panel-head panel-head-short">
      <span class="leg-label leg-label-short">▼ SHORT LEG</span>
      <button class="open-btn open-on" id="short-open-btn" tabindex="-1">OPEN</button>
    </div>
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
    <div class="row" style="border-bottom:none"><span class="row-label">Risk : Reward</span><span class="calc-val c-muted" id="s_rr">—</span></div>
  </div>

  <!-- ANALYSIS -->
  <div class="panel full-width">
    <div class="section-label" style="border-bottom:none">Spread Analysis</div>
    <div class="analysis-grid">
      <div class="ac"><div class="ac-label">Max Loss</div><div class="ac-val c-muted" id="a_maxloss">—</div><div class="ac-sub">both legs at stop</div></div>
      <div class="ac"><div class="ac-label">Max Gain</div><div class="ac-val c-muted" id="a_maxgain">—</div><div class="ac-sub">both legs at target</div></div>
      <div class="ac"><div class="ac-label">Spread R:R</div><div class="ac-val c-muted" id="a_rr">—</div><div class="ac-sub" id="a_rr_sub">combined ratio</div></div>
      <div class="ac"><div class="ac-label">Imbalance</div><div class="ac-val c-muted" id="a_imb">—</div><div class="ac-sub" id="a_imb_sub">lot size skew</div></div>
    </div>
    <div class="risk-bar-wrap">
      <div class="risk-bar-label"><span>Risk Exposure</span><span id="risk-pct-label" style="color:var(--muted)">—</span></div>
      <div class="risk-bar-track"><div class="risk-bar-fill" id="risk-bar"></div></div>
      <div class="risk-tags" id="risk-tags"></div>
    </div>
    <div class="imbalance-banner" id="imb-banner"><div class="imbalance-dot" id="imb-dot"></div><span id="imb-text"></span></div>
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
        <div class="mc-label">Long Unrealised</div><div class="mc-val c-muted" id="m_long_unreal">—</div>
      </div>
      <div class="mc" id="mc-short" style="background:var(--short-bg);border-color:var(--short-bd)">
        <div class="mc-label">Short Unrealised</div><div class="mc-val c-muted" id="m_short_unreal">—</div>
      </div>
      <div class="mc" id="mc-net">
        <div class="mc-label">Net Spread P&amp;L</div><div class="mc-val c-muted" id="m_net">—</div>
      </div>
    </div>
  </div>

  <!-- EXIT -->
  <div class="panel full-width">
    <div class="section-label">Final Pair P&amp;L (Exit-Based)</div>
    <div class="exit-grid">
      <div class="exit-col">
        <div class="row"><span class="row-label">Long exit price</span><input class="row-input" id="l_exit" type="number" placeholder="—" tabindex="14"></div>
        <div class="row" style="border-bottom:none"><span class="row-label">Long realised P&amp;L</span><span class="calc-val c-muted" id="l_real">—</span></div>
      </div>
      <div class="exit-col">
        <div class="row"><span class="row-label">Short exit price</span><input class="row-input" id="s_exit" type="number" placeholder="—" tabindex="15"></div>
        <div class="row" style="border-bottom:none"><span class="row-label">Short realised P&amp;L</span><span class="calc-val c-muted" id="s_real">—</span></div>
      </div>
    </div>
    <div class="row" style="border-top:1px solid var(--border);border-bottom:none">
      <span class="row-label">Brokerage (per trade × 2)</span>
      <input class="row-input" id="brok" type="number" placeholder="0" tabindex="16">
    </div>
    <div class="pair-pnl-row">
      <span class="pair-label">NET PAIR P&amp;L (after brokerage)</span>
      <span class="pair-val c-muted" id="pair_pnl">—</span>
    </div>
  </div>

  <!-- PULLBACK -->
  <div class="panel full-width">
    <div class="section-label">Exit-Spread Controls (Pullback Band)</div>
    <div class="pb-grid">
      <div class="pb-col">
        <div class="row"><span class="row-label">Mode</span>
          <select class="row-input" id="pb_mode" tabindex="17">
            <option value="none">None</option>
            <option value="long">Long pullback</option>
            <option value="short">Short pullback</option>
          </select>
        </div>
        <div class="row" style="border-bottom:none"><span class="row-label">Pullback amount</span><input class="row-input" id="pb_amt" type="number" placeholder="0" tabindex="18"></div>
      </div>
      <div class="pb-col">
        <div class="row"><span class="row-label">Pullback exit price</span><span class="calc-val c-muted" id="pb_px">—</span></div>
        <div class="row" style="border-bottom:none"><span class="row-label">Realised if pullback hits</span><span class="calc-val c-muted" id="pb_long_real">—</span></div>
      </div>
    </div>
    <div class="row" style="border-top:1px solid var(--border);border-bottom:none">
      <span class="row-label">Realised short if close now</span><span class="calc-val c-muted" id="pb_short_now">—</span>
    </div>
    <div class="pair-pnl-row">
      <span class="pair-label">TOTAL IF SCENARIO PLAYS OUT</span>
      <span class="pair-val c-muted" id="pb_total">—</span>
    </div>
  </div>
</div>

<script type="text/javascript">
(function(){

var DEFAULTS={
  l_entry:256000,l_qty:1,l_vpp:1,l_range:10000,l_stop_off:6000,l_tgt_off:5000,
  s_entry:256500,s_qty:1,s_vpp:1,s_range:10000,s_stop_off:3500,s_tgt_off:5500,
  cur_px:'',l_exit:'',s_exit:'',brok:0,pb_amt:0,pb_mode:'none'
};

/* ── helpers ── */
function gNum(id){var el=document.getElementById(id);if(!el||el.value==='')return null;var v=parseFloat(el.value);return isNaN(v)?null:v;}
function gStr(id){var el=document.getElementById(id);return el?el.value:'';}
function setT(id,t){var el=document.getElementById(id);if(el)el.textContent=t;}
function fmt(n){if(n===null||isNaN(n))return '—';return(n>=0?'+':'')+n.toLocaleString('en-IN',{minimumFractionDigits:2,maximumFractionDigits:2});}
function fmtN(n){if(n===null||isNaN(n))return '—';return n.toLocaleString('en-IN',{maximumFractionDigits:0});}

function setCV(id,val,fmt_fn){
  var el=document.getElementById(id);if(!el)return;
  if(val===null){el.textContent='—';el.className='calc-val c-muted';return;}
  el.textContent=(fmt_fn||fmt)(val);
  el.className='calc-val '+(val>0?'c-pos':val<0?'c-neg':'c-neu');
}
function setMV(id,val){
  var el=document.getElementById(id);if(!el)return;
  if(val===null){el.textContent='—';el.className='mc-val c-muted';return;}
  el.textContent=fmt(val);
  el.className='mc-val '+(val>0?'c-pos':val<0?'c-neg':'c-neu');
}
function setAV(id,val,txt,cls){
  var el=document.getElementById(id);if(!el)return;
  el.textContent=txt||'—';
  el.className='ac-val '+(cls||'c-muted');
}
function setPV(id,val){
  var el=document.getElementById(id);if(!el)return;
  if(val===null){el.textContent='—';el.className='pair-val c-muted';return;}
  el.textContent=fmt(val);
  el.className='pair-val '+(val>0?'c-pos':val<0?'c-neg':'c-neu');
}

function calcRR(stopAbs,tgtAbs){
  if(!stopAbs||!tgtAbs||stopAbs===0)return null;
  return tgtAbs/stopAbs;
}

function calc(){
  var le=gNum('l_entry'), lq=gNum('l_qty'), lv=gNum('l_vpp');
  var lso=gNum('l_stop_off'), lto=gNum('l_tgt_off');
  var se=gNum('s_entry'), sq=gNum('s_qty'), sv=gNum('s_vpp');
  var sso=gNum('s_stop_off'), sto=gNum('s_tgt_off');
  var cp=gNum('cur_px'), brok=gNum('brok')||0;

  /* ── LONG LEG ── only calculate if entry + offsets filled ── */
  var hasLong = le!==null && lq!==null && lv!==null;
  var lSP=null,lTP=null,lSPnl=null,lTPnl=null,lRR=null;
  if(hasLong && lso!==null){lSP=le-lso;}
  if(hasLong && lto!==null){lTP=le+lto;}
  if(hasLong && lSP!==null){lSPnl=(lSP-le)*lq*lv;}
  if(hasLong && lTP!==null){lTPnl=(lTP-le)*lq*lv;}
  if(lSPnl!==null&&lTPnl!==null){lRR=calcRR(Math.abs(lSPnl),Math.abs(lTPnl));}

  setT('l_stop_px', lSP!==null?fmtN(lSP):'—');
  setT('l_tgt_px',  lTP!==null?fmtN(lTP):'—');
  setT('l_be',      le!==null?fmtN(le):'—');
  setCV('l_stop_pnl', lSPnl);
  setCV('l_tgt_pnl',  lTPnl);
  var lrrEl=document.getElementById('l_rr');
  if(lRR!==null){lrrEl.textContent='1 : '+lRR.toFixed(2);lrrEl.className='calc-val '+(lRR>=2?'c-pos':lRR>=1?'c-warn':'c-neg');}
  else{lrrEl.textContent='—';lrrEl.className='calc-val c-muted';}

  /* ── SHORT LEG ── */
  var hasShort = se!==null && sq!==null && sv!==null;
  var sSP=null,sTP=null,sSPnl=null,sTPnl=null,sRR=null;
  if(hasShort && sso!==null){sSP=se+sso;}
  if(hasShort && sto!==null){sTP=se-sto;}
  if(hasShort && sSP!==null){sSPnl=(se-sSP)*sq*sv;}
  if(hasShort && sTP!==null){sTPnl=(se-sTP)*sq*sv;}
  if(sSPnl!==null&&sTPnl!==null){sRR=calcRR(Math.abs(sSPnl),Math.abs(sTPnl));}

  setT('s_stop_px', sSP!==null?fmtN(sSP):'—');
  setT('s_tgt_px',  sTP!==null?fmtN(sTP):'—');
  setT('s_be',      se!==null?fmtN(se):'—');
  setCV('s_stop_pnl', sSPnl);
  setCV('s_tgt_pnl',  sTPnl);
  var srrEl=document.getElementById('s_rr');
  if(sRR!==null){srrEl.textContent='1 : '+sRR.toFixed(2);srrEl.className='calc-val '+(sRR>=2?'c-pos':sRR>=1?'c-warn':'c-neg');}
  else{srrEl.textContent='—';srrEl.className='calc-val c-muted';}

  /* ── ANALYSIS ── only when both legs have enough data ── */
  var hasBoth = lSPnl!==null && lTPnl!==null && sSPnl!==null && sTPnl!==null;
  var maxLoss=null,maxGain=null,spreadRR=null,imbPct=null;
  if(hasBoth){
    maxLoss=lSPnl+sSPnl; maxGain=lTPnl+sTPnl;
    if(maxLoss!==0)spreadRR=Math.abs(maxGain/maxLoss);
  }
  if(hasLong&&hasShort){
    var lN=lq*lv*(le||1), sN=sq*sv*(se||1);
    if(lN>0&&sN>0)imbPct=Math.abs(lN-sN)/Math.max(lN,sN)*100;
  }

  if(maxLoss!==null)setAV('a_maxloss',maxLoss,fmt(maxLoss),maxLoss>0?'c-pos':maxLoss<0?'c-neg':'c-neu');
  else setAV('a_maxloss',null,'—','c-muted');
  if(maxGain!==null)setAV('a_maxgain',maxGain,fmt(maxGain),maxGain>0?'c-pos':maxGain<0?'c-neg':'c-neu');
  else setAV('a_maxgain',null,'—','c-muted');

  if(spreadRR!==null){
    setAV('a_rr',spreadRR,'1 : '+spreadRR.toFixed(2),spreadRR>=2?'c-pos':spreadRR>=1?'c-warn':'c-neg');
    setT('a_rr_sub',spreadRR>=2?'favourable ratio':spreadRR>=1?'acceptable':'poor ratio');
  } else {setAV('a_rr',null,'—','c-muted');setT('a_rr_sub','combined ratio');}

  var imbBanner=document.getElementById('imb-banner');
  if(imbPct!==null){
    if(imbPct>20){
      setAV('a_imb',imbPct,imbPct.toFixed(1)+'%','c-neg');setT('a_imb_sub','high imbalance');
      imbBanner.className='imbalance-banner show';imbBanner.style.background='var(--neg-bg)';imbBanner.style.borderColor='#fecaca';
      document.getElementById('imb-dot').style.background='var(--neg)';
      setT('imb-text','Warning: Leg imbalance '+imbPct.toFixed(1)+'% — spread may not hedge cleanly. Adjust lot sizes.');
    } else if(imbPct>10){
      setAV('a_imb',imbPct,imbPct.toFixed(1)+'%','c-warn');setT('a_imb_sub','moderate skew');
      imbBanner.className='imbalance-banner show';imbBanner.style.background='var(--warn-bg)';imbBanner.style.borderColor='#fde68a';
      document.getElementById('imb-dot').style.background='var(--warn)';
      setT('imb-text','Note: Leg imbalance '+imbPct.toFixed(1)+'% — monitor directional exposure.');
    } else {
      setAV('a_imb',imbPct,imbPct.toFixed(1)+'%','c-pos');setT('a_imb_sub','balanced');
      imbBanner.className='imbalance-banner';
    }
  } else {setAV('a_imb',null,'—','c-muted');setT('a_imb_sub','lot size skew');imbBanner.className='imbalance-banner';}

  var rb=document.getElementById('risk-bar'),rpl=document.getElementById('risk-pct-label'),rt=document.getElementById('risk-tags');
  if(maxGain!==null&&maxLoss!==null&&maxGain>0&&maxLoss<0){
    var rr=Math.abs(maxLoss)/maxGain*100,barPct=Math.min(rr,100);
    var bc=barPct>80?'var(--neg)':barPct>50?'var(--warn)':'var(--pos)';
    rb.style.width=barPct+'%';rb.style.background=bc;
    rpl.textContent=rr.toFixed(0)+'% risk/reward';rpl.style.color=bc;
    var tags=[];
    if(spreadRR!==null)tags.push({t:spreadRR>=2?'Good R:R':spreadRR>=1?'Avg R:R':'Poor R:R',c:spreadRR>=2?'var(--pos)':spreadRR>=1?'var(--warn)':'var(--neg)'});
    if(imbPct!==null)tags.push({t:imbPct>20?'Imbalanced':imbPct>10?'Slight Skew':'Balanced',c:imbPct>20?'var(--neg)':imbPct>10?'var(--warn)':'var(--pos)'});
    rt.innerHTML=tags.map(function(t){return '<span class="risk-tag" style="color:'+t.c+';border-color:'+t.c+'">'+t.t+'</span>';}).join('');
  } else {rb.style.width='0%';rpl.textContent='—';rpl.style.color='var(--muted)';rt.innerHTML='';}

  /* ── MTM — only when cur_px AND entry prices filled ── */
  var badge=document.getElementById('net-badge');
  var mcNet=document.getElementById('mc-net');
  if(cp===null||le===null||se===null){
    setMV('m_long_unreal',null);setMV('m_short_unreal',null);setMV('m_net',null);
    badge.textContent='—';badge.className='net-badge net-neu';
    mcNet.style.background='var(--bg3)';mcNet.style.borderColor='var(--border)';
  } else {
    var lU=(cp-le)*lq*lv, sU=(se-cp)*sq*sv, net=lU+sU;
    setMV('m_long_unreal',lU);setMV('m_short_unreal',sU);setMV('m_net',net);
    mcNet.style.background=net>0?'var(--pos-bg)':net<0?'var(--neg-bg)':'var(--bg3)';
    mcNet.style.borderColor=net>0?'#bbf7d0':net<0?'#fecaca':'var(--border)';
    badge.textContent=(net>=0?'+':'')+net.toFixed(2);
    badge.className='net-badge '+(net>0?'net-pos':net<0?'net-neg':'net-neu');
  }

  /* ── EXIT — only when both entry AND exit prices filled ── */
  var lExStr=gStr('l_exit'), sExStr=gStr('s_exit');
  var lEv=(lExStr!==''&&le!==null)?parseFloat(lExStr):null;
  var sEv=(sExStr!==''&&se!==null)?parseFloat(sExStr):null;
  var lR=(lEv!==null&&!isNaN(lEv))?(lEv-le)*lq*lv:null;
  var sR=(sEv!==null&&!isNaN(sEv))?(se-sEv)*sq*sv:null;
  setCV('l_real',lR);
  setCV('s_real',sR);
  if(lR!==null&&sR!==null){setPV('pair_pnl',lR+sR-(brok*2));}
  else setPV('pair_pnl',null);

  /* ── PULLBACK ── only when cur_px + entries filled ── */
  var mode=gStr('pb_mode'), pba=gNum('pb_amt');
  var pbTot=document.getElementById('pb_total');
  if(cp!==null&&le!==null&&se!==null&&pba!==null&&pba>0&&mode!=='none'){
    if(mode==='long'){
      var pbPx=cp-pba,rl=(pbPx-le)*lq*lv,rs=(se-cp)*sq*sv,tot=rl+rs;
      setT('pb_px',fmtN(pbPx));setCV('pb_long_real',rl);setCV('pb_short_now',rs);setPV('pb_total',tot);
    } else if(mode==='short'){
      var pbPx=cp+pba,rs=(se-pbPx)*sq*sv,rl=(cp-le)*lq*lv,tot=rl+rs;
      setT('pb_px',fmtN(pbPx));setCV('pb_long_real',rs);setCV('pb_short_now',rl);setPV('pb_total',tot);
    }
  } else {
    setT('pb_px','—');setCV('pb_long_real',null);setCV('pb_short_now',null);setPV('pb_total',null);
  }
}

/* ── reset & clear ── */
function applyValues(vals){
  Object.keys(vals).forEach(function(id){var el=document.getElementById(id);if(el)el.value=vals[id];});
  var lb=document.getElementById('long-open-btn');if(lb){lb.textContent='OPEN';lb.className='open-btn open-on';}
  var sb=document.getElementById('short-open-btn');if(sb){sb.textContent='OPEN';sb.className='open-btn open-on';}
  calc();
}

function clearAll(){
  document.querySelectorAll('input').forEach(function(el){el.value='';});
  var pb=document.getElementById('pb_mode');if(pb)pb.value='none';
  var ids=['l_stop_pnl','l_tgt_pnl','l_rr','s_stop_pnl','s_tgt_pnl','s_rr',
           'm_long_unreal','m_short_unreal','m_net','l_real','s_real',
           'pb_long_real','pb_short_now','a_maxloss','a_maxgain','a_rr','a_imb'];
  ids.forEach(function(id){
    var e=document.getElementById(id);if(!e)return;
    e.textContent='—';
    var base=id.indexOf('m_')===0?'mc-val':id.indexOf('a_')===0?'ac-val':'calc-val';
    e.className=base+' c-muted';
  });
  ['l_stop_px','l_tgt_px','l_be','s_stop_px','s_tgt_px','s_be','pb_px',
   'risk-pct-label','a_rr_sub','a_imb_sub'].forEach(function(id){
    var e=document.getElementById(id);
    if(e)e.textContent=id==='a_rr_sub'?'combined ratio':id==='a_imb_sub'?'lot size skew':'—';
  });
  setPV('pair_pnl',null);setPV('pb_total',null);
  var badge=document.getElementById('net-badge');if(badge){badge.textContent='—';badge.className='net-badge net-neu';}
  var mcNet=document.getElementById('mc-net');if(mcNet){mcNet.style.background='var(--bg3)';mcNet.style.borderColor='var(--border)';}
  var rb=document.getElementById('risk-bar');if(rb)rb.style.width='0%';
  var rt=document.getElementById('risk-tags');if(rt)rt.innerHTML='';
  document.getElementById('imb-banner').className='imbalance-banner';
  var lb=document.getElementById('long-open-btn');if(lb){lb.textContent='OPEN';lb.className='open-btn open-on';}
  var sb=document.getElementById('short-open-btn');if(sb){sb.textContent='OPEN';sb.className='open-btn open-on';}
}

/* ── init ── */
function init(){
  document.querySelectorAll('input,select').forEach(function(el){
    el.addEventListener('input',calc);
    el.addEventListener('change',calc);
    el.addEventListener('keyup',calc);
  });
  document.getElementById('long-open-btn').addEventListener('click',function(){
    var on=this.textContent==='OPEN';this.textContent=on?'CLOSED':'OPEN';this.className='open-btn '+(on?'open-off':'open-on');
  });
  document.getElementById('short-open-btn').addEventListener('click',function(){
    var on=this.textContent==='OPEN';this.textContent=on?'CLOSED':'OPEN';this.className='open-btn '+(on?'open-off':'open-on');
  });
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
