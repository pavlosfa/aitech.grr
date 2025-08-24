<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>aitech.gr — AI Trainer (1‑Page MVP, EN only)</title>
  <style>
    :root{ --bg:#0b0f19; --panel:#0f172a; --muted:#93a1b5; --text:#e6edf3; --accent:#7dd3fc; --accent2:#a78bfa; --border:#1e293b; --ok:#22c55e; --bad:#ef4444; }
    *{box-sizing:border-box}
    body{margin:0;background:#0b0f19;color:var(--text);font-family:system-ui,Segoe UI,Inter,Roboto}
    header{padding:20px;border-bottom:1px solid var(--border);position:sticky;top:0;background:rgba(11,15,25,.72);backdrop-filter:saturate(1.1) blur(10px);z-index:10}
    .wrap{max-width:1100px;margin:0 auto}
    .layout{display:grid;grid-template-columns:420px 1fr;gap:16px;align-items:start;padding:16px 20px}
    .card{background:#0f172a;border:1px solid var(--border);border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,.25)}
    .card .head{padding:12px 14px;border-bottom:1px solid #18243a;display:flex;align-items:center;justify-content:space-between}
    .card .body{padding:12px 14px}
    .muted{color:var(--muted)}
    label{display:block;margin:8px 0 6px}
    input,select,textarea,button{font-family:inherit}
    input,select,textarea{width:100%;background:#0f172a;border:1px solid var(--border);color:#fff;padding:10px 12px;border-radius:10px}
    textarea{min-height:64px}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .actions{display:flex;gap:10px;flex-wrap:wrap;margin-top:10px}
    .btn{background:#0f172a;border:1px solid var(--border);color:#dbeafe;padding:10px 12px;border-radius:10px;cursor:pointer}
    .btn.primary{background:var(--accent);color:#041018;border-color:#38bdf8;font-weight:700}
    .hint{font-size:12px;color:var(--muted)}
    .mode{display:flex;align-items:center;gap:8px}
    .switch{position:relative;width:54px;height:28px;background:#112136;border:1px solid #263b61;border-radius:999px;cursor:pointer}
    .knob{position:absolute;top:2px;left:2px;width:24px;height:24px;border-radius:999px;background:linear-gradient(180deg,#c0e8ff,#7dd3fc);transition:transform .18s}
    body.advanced .knob{transform:translateX(26px)}
    .filelist{margin-top:8px; padding:0; list-style:none}
    .filelist li{display:flex;align-items:center;gap:8px;padding:6px 8px;border:1px solid #203150;border-radius:10px;margin-bottom:6px;background:#0c1428}
    .filelist .name{flex:1}
    .filelist .ok{color:#86efac}
    .filelist .err{color:#fca5a5}
    #preview{display:grid;grid-template-columns:1fr;gap:12px;padding:12px}
    .panel{background:#0f172a;border:1px solid var(--border);border-radius:14px;padding:14px}
    .panel h3{margin:0 0 8px;font-size:18px}
    .kpi{display:flex;gap:10px;flex-wrap:wrap;margin-top:8px}
    .chip{border:1px solid #203150;border-radius:999px;padding:3px 8px;font-size:12px;color:#c7d2fe}
    .alert{display:none;margin:10px 0;padding:10px 12px;border-radius:10px}
    .alert.err{display:block;background:#1b0f0f;border:1px solid #5c2323;color:#fecaca}
    .progress{height:10px;background:#0c1428;border:1px solid #1d2e4b;border-radius:999px;overflow:hidden}
    .progress > div{height:100%;width:0;background:linear-gradient(90deg,#7dd3fc,#a78bfa)}
    .docs{max-height:160px;overflow:auto;border:1px solid #14213a;border-radius:10px;padding:10px;background:#0c1428;font-size:13px}
    body.simple .advonly{display:none !important}
    body.advanced .simpleonly{display:none !important}
    .drop{border:2px dashed #2a3c5e;border-radius:14px;padding:16px;text-align:center;background:#0c1428;color:#c7d2fe;position:relative}
    #file{position:absolute;left:-9999px;width:1px;height:1px;opacity:0}
    .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.5);z-index:50}
    .modal .box{background:#0f172a;border:1px solid #22324f;border-radius:14px;padding:18px;max-width:640px;width:92%}
    pre.code{background:#0c1428;border:1px solid #14213a;border-radius:10px;padding:10px;overflow:auto}
    @media (max-width:980px){.layout{grid-template-columns:1fr}}
  </style>
</head>
<body class="simple">
  <header>
    <div class="wrap" style="display:flex;align-items:center;justify-content:space-between;gap:12px">
      <div>
        <h1>AI Trainer — 3 simple steps</h1>
        <p class="muted">1) Upload • 2) Train • 3) Ask. Everything runs in the browser (no backend).</p>
      </div>
      <div style="display:flex; align-items:center; gap:12px">
        <div class="mode" title="Toggle Simple/Advanced">
          <small>Simple</small>
          <div class="switch" id="modeSwitch"><div class="knob"></div></div>
          <small>Advanced</small>
        </div>
      </div>
    </div>
  </header>

  <main class="wrap layout">
    <section class="card">
      <div class="head"><strong>Steps</strong><span class="hint simpleonly">Upload → Train → Ask</span><span class="hint advonly">With advanced settings</span></div>
      <div class="body">
        <h4>1) Upload data</h4>
        <div class="drop" id="drop">
          <span>Drop files here or</span>
          <button id="chooseBtn" type="button" class="btn inline">Choose files</button>
          <input type="file" id="file" multiple accept=".csv,.txt,.md,.json,.jsonl,.xlsx,.xls,.xlsm,.xltx,.xltm" />
          <div class="hint" id="excelStatus">Excel: compatibility check…</div>
        </div>
        <ul class="filelist" id="filelist"></ul>
        <div class="actions" style="margin-top:8px">
          <button id="btnSample" class="btn">Load demo</button>
          <button id="btnReadClipboard" class="btn">Clipboard from Excel</button>
        </div>
        <div class="hint simpleonly">Tip: You can also paste directly below.</div>
        <textarea id="paste" class="simpleonly" placeholder="(optional) paste text or Excel grid"></textarea>
        <div class="actions simpleonly">
          <button id="btnAddPaste" class="btn">Add text</button>
          <button id="btnAddPasteGrid" class="btn">Add from grid</button>
        </div>

        <div class="advonly">
          <h4 style="margin:14px 0 6px">Preprocessing</h4>
          <div class="row">
            <div>
              <label>Chunk size</label>
              <input id="chunk" value="600" />
            </div>
            <div>
              <label>Overlap</label>
              <input id="overlap" value="80" />
            </div>
          </div>
          <label>CSV/Excel text columns (comma)</label>
          <input id="csvCols" placeholder="e.g. title,body" />
          <label>CSV/Excel label column (for Classifier)</label>
          <input id="labelCol" placeholder="e.g. label" />
        </div>

        <h4 style="margin-top:12px">2) Train</h4>
        <div class="actions">
          <button class="btn primary" id="btnTrain">Train</button>
          <button class="btn" id="btnReset">Reset</button>
        </div>
        <div class="alert err" id="err"></div>
        <div class="progress" title="Progress"><div id="bar"></div></div>

        <div class="advonly" style="margin-top:12px">
          <h4>Local API</h4>
          <p class="muted">Turns on in‑browser API endpoints (via fetch interception). Great for demos.</p>
          <div class="actions">
            <button class="btn" id="btnEnableAPI">Enable API</button>
            <button class="btn" id="btnSyncAPI">Sync docs → API</button>
            <button class="btn" id="btnTestStatus">Test <code>/api/status</code></button>
          </div>
          <div class="row" style="margin-top:6px">
            <div>
              <label>Ask (POST /api/ask)</label>
              <input id="apiQ" placeholder="e.g. pricing" />
            </div>
            <div style="display:flex;align-items:flex-end"><button class="btn" id="btnApiAsk">Run</button></div>
          </div>
          <details style="margin-top:10px">
            <summary>API Playground (code sample)</summary>
            <pre class="code" id="apiSample">fetch('/api/ask', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ q: 'price', k: 3 })
}).then(r => r.json()).then(console.log)</pre>
            <div class="actions"><button class="btn" id="btnRunSample">Run sample</button></div>
            <div class="docs" id="apiOut"></div>
          </details>
          <div class="hint" id="apiStatus">Local API: disabled</div>
        </div>
      </div>
    </section>

    <section class="card">
      <div class="head"><strong>3) Ask the model</strong><span class="hint">Sample docs & logs below</span></div>
      <div class="body" id="preview">
        <div class="panel">
          <h3>Question (RAG)</h3>
          <input id="q" placeholder="Ask something about your data" />
          <div class="actions">
            <button class="btn primary" id="btnAsk">Ask</button>
          </div>
          <div id="answers"></div>
        </div>
        <div class="panel advonly">
          <h3>Classifier</h3>
          <input id="xtext" placeholder="Text to classify" />
          <div class="actions">
            <button class="btn" id="btnPredict">Predict</button>
          </div>
          <div id="predOut" class="docs"></div>
        </div>
        <div class="panel">
          <h3>Docs (sample) & KPIs</h3>
          <div class="docs" id="docSample"></div>
          <div class="kpi" id="stats"></div>
        </div>
        <div class="panel">
          <h3>Logs</h3>
          <pre id="logs" style="white-space:pre-wrap"></pre>
        </div>
        <div class="panel" id="how">
          <h3>How it works (brief)</h3>
          <ol class="how">
            <li><strong>Upload</strong> Excel/CSV/JSON/TXT (or Clipboard from Excel). Excel is parsed <em>locally</em> (1st sheet). Clipboard/HTML keeps <em>only the first table</em>.</li>
            <li><strong>Train</strong> — builds a retriever (TF‑IDF‑like lexical overlap). Advanced: classifier & settings.</li>
            <li><strong>Ask</strong> — ask a question, get passages. Optionally use an LLM to summarize (pluggable).</li>
          </ol>
        </div>
      </div>
    </section>
  </main>

  <div class="modal" id="excelModal" role="dialog" aria-modal="true" aria-labelledby="mTitle">
    <div class="box">
      <h3 id="mTitle">Excel parser blocked</h3>
      <p class="muted">If the browser does not support unzip:</p>
      <ol>
        <li>Excel/Sheets → Ctrl/⌘+A → Ctrl/⌘+C</li>
        <li>Click “<strong>Clipboard from Excel</strong>”</li>
        <li>Alternatively: Save As → CSV</li>
      </ol>
      <div class="actions">
        <button id="mClipboard" class="btn primary">Read from Clipboard now</button>
        <button id="mClose" class="btn">Close</button>
      </div>
    </div>
  </div>

  <div class="modal" id="pasteModal" role="dialog" aria-modal="true" aria-labelledby="pTitle">
    <div class="box">
      <h3 id="pTitle">Paste from Excel (fallback)</h3>
      <p class="muted">This environment blocks the Clipboard API. Press <strong>Ctrl/⌘+V</strong> here to paste from Excel/Sheets. Accepts HTML table or plain text grid.</p>
      <div id="pasteBox" class="pastebox" contenteditable="true"></div>
      <div class="actions">
        <button id="mPasteDone" class="btn primary">Use pasted data</button>
        <button id="mPasteClose" class="btn">Close</button>
      </div>
    </div>
  </div>

<script>
// ---------- Polyfills (no syntax‑new features) ----------
if(!('BroadcastChannel' in window)) window.BroadcastChannel = function(){ return { postMessage:function(){} }; };

// ---------- Tiny helpers (no optional chaining) ----------
function $(q, el){ return (el||document).querySelector(q); }
function $all(q, el){ return Array.prototype.slice.call((el||document).querySelectorAll(q)); }
function on(el, ev, fn){ if(el && el.addEventListener) el.addEventListener(ev, fn); }
function text(el, v){ if(typeof el==='string') el=$(el); if(el) el.textContent=v; }
function html(el, v){ if(typeof el==='string') el=$(el); if(el) el.innerHTML=v; }
function show(el, on){ if(typeof el==='string') el=$(el); if(el){ el.style.display = (on!==false? 'flex':'none'); } }
function val(el){ return el && typeof el.value!=='undefined'? el.value : ''; }

function log(s){ var e=$('#logs'); if(e){ e.textContent += s+'\n'; e.scrollTop = e.scrollHeight; } }
function setBar(p){ var b=$('#bar'); if(b){ var x=Math.max(0,Math.min(1,p))*100; b.style.width = x+'%'; } }
function showErr(m){ var e=$('#err'); if(e){ e.textContent=m; e.style.display='block'; setTimeout(function(){ if(e) e.style.display='none'; }, 4000); } }

// ---------- Parsers ----------
function parseCSV(text){ var rows=[], field='', row=[], inq=false; for(var i=0;i<text.length;i++){ var ch=text[i]; if(ch==='"'){ if(inq && text[i+1]==='"'){ field+='"'; i++; continue; } inq=!inq; continue; } if(!inq && ch===','){ row.push(field); field=''; continue; } if(!inq && (ch==='\n'||ch==='\r')){ if(field!==''||row.length){ row.push(field); rows.push(row); row=[]; field=''; } continue; } field+=ch; } if(field!==''||row.length){ row.push(field); rows.push(row); } return rows; }
function parseGrid(text){ var lines=String(text||'').split(/\r?\n/).filter(function(x){return x}); if(!lines.length) return []; var delim=lines[0].indexOf('\t')>=0? '\t' : ','; return lines.map(function(l){ return l.split(new RegExp(delim)); }); }
function parseHTMLTable(htmlStr){ try{ var doc=new DOMParser().parseFromString(htmlStr,'text/html'); var table=doc.querySelector('table'); if(!table) return []; var rows=[]; for(var i=0;i<table.rows.length;i++){ var tr=table.rows[i]; var r=[]; for(var j=0;j<tr.cells.length;j++){ r.push(tr.cells[j].innerText.replace(/\s+/g,' ').trim()); } rows.push(r); } return rows; }catch(e){ return []; } }

// ---------- File readers ----------
function readFileAsText(f){ return new Promise(function(res,rej){ var r=new FileReader(); r.onload=function(){ res(r.result); }; r.onerror=rej; r.readAsText(f); }); }
function readFileAsArrayBuffer(f){ return new Promise(function(res,rej){ var r=new FileReader(); r.onload=function(){ res(r.result); }; r.onerror=rej; r.readAsArrayBuffer(f); }); }

// ---------- Minimal XLSX (first sheet) ----------
function unzip(ab){ return new Promise(function(resolve, reject){ try{ var u8=new Uint8Array(ab); var dv=new DataView(ab); var eocd=-1; for(var i=u8.length-22;i>=Math.max(0,u8.length-65558);i--){ if(dv.getUint32(i,true)===0x06054b50){ eocd=i; break; } } if(eocd<0) return reject(new Error('ZIP EOCD not found')); var cdOffset=dv.getUint32(eocd+16,true); var p=cdOffset; var files=[]; while(dv.getUint32(p,true)===0x02014b50){ var comp=dv.getUint16(p+10,true), compSize=dv.getUint32(p+20,true), nameLen=dv.getUint16(p+28,true), extraLen=dv.getUint16(p+30,true), commentLen=dv.getUint16(p+32,true), lho=dv.getUint32(p+42,true); var name=new TextDecoder().decode(u8.subarray(p+46,p+46+nameLen)); files.push({name:name,comp:comp,compSize:compSize,lho:lho}); p+=46+nameLen+extraLen+commentLen; }
      var out=new Map();
      var next = function(idx){ if(idx>=files.length) return resolve(out); var f=files[idx]; var sig=dv.getUint32(f.lho,true); if(sig!==0x04034b50){ return next(idx+1); }
        var fnLen=dv.getUint16(f.lho+26,true), exLen=dv.getUint16(f.lho+28,true); var dataStart=f.lho+30+fnLen+exLen; var compBytes=u8.subarray(dataStart,dataStart+f.compSize);
        if(f.comp===0){ out.set(f.name, compBytes); return next(idx+1); }
        if(f.comp===8){ if(!('DecompressionStream' in window)) return reject(new Error('DecompressionStream not supported'));
          var ds; try{ ds=new DecompressionStream('deflate-raw'); }catch(e){ ds=new DecompressionStream('deflate'); }
          new Response(new Blob([compBytes]).stream().pipeThrough(ds)).arrayBuffer().then(function(buf){ out.set(f.name, new Uint8Array(buf)); next(idx+1); }, reject); return; }
        next(idx+1);
      };
      next(0);
    }catch(err){ reject(err); }
  }); }
function colLetterToIndex(r){ var m=/^[A-Z]+/.exec(r||'A'); if(!m) return 0; var col=0; for(var i=0;i<m[0].length;i++){ var ch=m[0].charCodeAt(i); col = col*26 + (ch-64); } return col-1; }
function parseSharedStrings(xml){ var doc=new DOMParser().parseFromString(xml,'application/xml'); var arr=[]; var sis=doc.getElementsByTagName('si'); for(var i=0;i<sis.length;i++){ var si=sis[i]; var s=''; var ts=si.getElementsByTagName('t'); for(var j=0;j<ts.length;j++){ s+= ts[j].textContent || ''; } arr.push(s); } return arr; }
function parseSheet(xml, shared){ var doc=new DOMParser().parseFromString(xml,'application/xml'); var sd=doc.getElementsByTagName('sheetData')[0]; if(!sd) return []; var rows=[]; var maxCol=0; var rowEls=sd.getElementsByTagName('row'); for(var i=0;i<rowEls.length;i++){ var row=rowEls[i]; var cells=[]; var cs=row.getElementsByTagName('c'); for(var j=0;j<cs.length;j++){ var c=cs[j]; var r=c.getAttribute('r'); var t=c.getAttribute('t'); var vEl=(c.getElementsByTagName('v')[0]); var v=(vEl && vEl.textContent)? vEl.textContent : '';
      var is=c.getElementsByTagName('is')[0]; var val='';
      if(t==='s'){ var idx=parseInt(v,10); val = isFinite(idx)? (shared[idx]||'') : ''; }
      else if(t==='inlineStr' && is){ var ts=is.getElementsByTagName('t'); val=''; for(var k=0;k<ts.length;k++){ val+= ts[k].textContent || ''; } }
      else { val=v; }
      var ci=colLetterToIndex(r); cells[ci]=val; if(ci>maxCol) maxCol=ci; }
    for(var cidx=0;cidx<=maxCol;cidx++){ if(typeof cells[cidx]==='undefined') cells[cidx]=''; }
    rows.push(cells); }
  return rows; }
function readXLSXArrayBuffer(ab){ return unzip(ab).then(function(files){ var td=new TextDecoder(); var workbook=files.get('xl/workbook.xml'); if(!workbook) throw new Error('workbook.xml missing'); var wbDoc=new DOMParser().parseFromString(td.decode(workbook),'application/xml'); var relsBytes=files.get('xl/_rels/workbook.xml.rels'); var relsMap=new Map(); if(relsBytes){ var rdoc=new DOMParser().parseFromString(td.decode(relsBytes),'application/xml'); var rels=rdoc.getElementsByTagName('Relationship'); for(var i=0;i<rels.length;i++){ var r=rels[i]; relsMap.set(r.getAttribute('Id'), r.getAttribute('Target')); } }
  var sstBytes=files.get('xl/sharedStrings.xml'); var shared=sstBytes? parseSharedStrings(td.decode(sstBytes)) : [];
  var out=[]; var nsRel='http://schemas.openxmlformats.org/officeDocument/2006/relationships';
  var sheets=wbDoc.getElementsByTagName('sheet');
  for(var i=0;i<sheets.length;i++){
    var s=sheets[i]; var name=s.getAttribute('name')||'Sheet';
    var rid = s.getAttribute('r:id') || (s.getAttributeNS? s.getAttributeNS(nsRel,'id') : null);
    var target = rid? (relsMap.get(rid)||'') : '';
    if(!target){ var sid=s.getAttribute('sheetId')||'1'; target='worksheets/sheet'+sid+'.xml'; }
    if(target.indexOf('xl/')!==0) target='xl/'+target;
    var sh=files.get(target); if(!sh) continue;
    var rows=parseSheet(td.decode(sh), shared);
    if(rows.length) out.push({name:name, rows:rows});
  }
  return {sheets: out}; }); }
function ensureXLSX(){ if('DecompressionStream' in window){ text('#excelStatus','Excel: OK (inline)'); return Promise.resolve(true); } text('#excelStatus','Excel: BLOCKED (no ZIP support)'); show('#excelModal', true); return Promise.resolve(false); }

// ---------- Data & Model ----------
var rawDocs=[]; var MODEL=null; // MODEL = [{id,text,tokens:Set,label?}]
function chip(k,v){ var s=document.createElement('span'); s.className='chip'; s.textContent=k+': '+v; return s; }
function updateFileCounter(){ var kpi=$('#stats'); if(kpi){ kpi.innerHTML=''; kpi.appendChild(chip('Docs', String(rawDocs.length))); if(MODEL) kpi.appendChild(chip('Mode','OVERLAP')); } }
function sampleDocs(){ var list=rawDocs.slice(0,6).map(function(d){ var body=String(d.text); var sn=body.slice(0,120).replace(/\s+/g,' '); if(body.length>120) sn+='…'; return '• '+d.id+': '+sn; }).join('\n'); text('#docSample', list || '—'); }
function addFileStatus(name,msg,ok){ var li=document.createElement('li'); li.innerHTML='<span class="name">'+name+'</span><span class="'+(ok?'ok':'err')+'">'+msg+'</span>'; var ul=$('#filelist'); if(ul) ul.appendChild(li); }
function detectKind(f){ var ext=(f.name.split('.').pop()||'').toLowerCase(); var t=(f.type||'').toLowerCase(); if(['xlsx','xls','xlsm','xltx','xltm'].indexOf(ext)>=0 || t.indexOf('spreadsheet')>=0 || t.indexOf('excel')>=0) return 'excel'; if(ext==='csv'||t.indexOf('csv')>=0) return 'csv'; if(['json','jsonl'].indexOf(ext)>=0||t.indexOf('json')>=0) return 'json'; if(['txt','md'].indexOf(ext)>=0||t.indexOf('text/')===0) return 'text'; return 'unknown'; }
function addRowsAsDocs(rows, source){ if(!rows.length){ showErr('No data found.'); return 0; } var header=rows[0].map(function(x){ return String(x||'').trim(); }); var colsEl=$('#csvCols'); var labelEl=$('#labelCol'); var cols=(colsEl? colsEl.value : '').split(',').map(function(s){return s.trim();}).filter(function(x){return x;}); var labelCol=(labelEl? labelEl.value : '').trim(); var colIdx = cols.length? cols.map(function(c){return header.indexOf(c);}).filter(function(i){return i>=0;}) : header.map(function(_,i){return i;}); var lblIdx = labelCol? header.indexOf(labelCol) : -1; var added=0; for(var r=1;r<rows.length;r++){ var row=rows[r]||[]; var parts=colIdx.map(function(i){ return String((row[i]!=null? row[i] : '')); }); var textv=parts.join(' ').trim(); if(!textv) continue; var label = lblIdx>=0? String((row[lblIdx]!=null? row[lblIdx] : '')).trim() : undefined; rawDocs.push({id:String(source)+'#'+r, text:textv, label:label}); added++; }
  updateFileCounter(); sampleDocs(); return added; }

// ---------- Clipboard ----------
var LAST_PASTE=null;
function openPasteModal(reason){ log('Clipboard fallback: '+(reason||'blocked')); LAST_PASTE=null; var box=$('#pasteBox'); if(box){ box.innerHTML=''; show('#pasteModal', true); setTimeout(function(){ box.focus(); }, 20); } }
function readClipboardExcel(){ try{ if(!navigator.clipboard){ openPasteModal('no api'); return; } if(navigator.clipboard.read){ navigator.clipboard.read().then(function(items){ for(var i=0;i<items.length;i++){ var it=items[i]; if(it.types.indexOf('text/html')>=0){ it.getType('text/html').then(function(blob){ blob.text().then(function(htmlStr){ var rows=parseHTMLTable(htmlStr); var added=addRowsAsDocs(rows,'clipboard'); addFileStatus('clipboard(html)', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); }); }); return; } }
      for(var i=0;i<items.length;i++){ var it=items[i]; if(it.types.indexOf('text/plain')>=0){ it.getType('text/plain').then(function(blob){ blob.text().then(function(txt){ var rows=parseGrid(txt); var added=addRowsAsDocs(rows,'clipboard'); addFileStatus('clipboard(text)', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); }); }); return; } }
      openPasteModal('no usable types');
    }).catch(function(){ navigator.clipboard.readText().then(function(txt){ var rows = txt.indexOf('<table')>=0? parseHTMLTable(txt) : parseGrid(txt); var added=addRowsAsDocs(rows,'clipboard'); addFileStatus('clipboard', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); }).catch(function(){ openPasteModal('blocked'); }); }); return; }
    try{ navigator.clipboard.readText().then(function(txt){ var rows = txt.indexOf('<table')>=0? parseHTMLTable(txt) : parseGrid(txt); var added=addRowsAsDocs(rows,'clipboard'); addFileStatus('clipboard', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); }).catch(function(){ openPasteModal('no permission'); }); } catch(e){ openPasteModal('exception'); }
  } catch(e){ openPasteModal('exception'); }
}
window.addEventListener('paste', function(e){ var modal=$('#pasteModal'); if(modal && modal.style.display==='flex'){ var cd=e.clipboardData; var htmlStr=cd? cd.getData('text/html') : ''; var textStr=cd? cd.getData('text/plain') : ''; LAST_PASTE = (htmlStr && htmlStr.indexOf('<table')>=0) ? {kind:'html', data:htmlStr} : {kind:'text', data:textStr}; var box=$('#pasteBox'); if(box){ if(htmlStr && htmlStr.indexOf('<table')>=0){ var tdoc=new DOMParser().parseFromString(htmlStr,'text/html'); var t=tdoc.querySelector('table'); box.innerHTML = t? t.outerHTML : ''; } else { box.innerHTML = '<pre style="white-space:pre-wrap">'+String(textStr||'').replace(/[<>]/g,function(s){return s==='<'?'&lt;':'&gt;';})+'</pre>'; } } } });
function consumePaste(){ var box=$('#pasteBox'); var rows=[]; if(LAST_PASTE){ rows = LAST_PASTE.kind==='html' ? parseHTMLTable(LAST_PASTE.data) : parseGrid(LAST_PASTE.data || (box? box.innerText : '')); } else { var htmlStr=box? box.innerHTML : ''; var textStr=box? box.innerText : ''; rows = (htmlStr.indexOf('<table')>=0) ? parseHTMLTable(htmlStr) : parseGrid(textStr); } var added=addRowsAsDocs(rows,'clipboard-paste'); addFileStatus('clipboard(paste)', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); show('#pasteModal', false); }

// ---------- Add files ----------
function addFiles(files){ for(var i=0;i<files.length;i++){ (function(f){ var kind=detectKind(f); try{ if(kind==='excel'){ ensureXLSX().then(function(ok){ if(!ok){ addFileStatus(f.name,'⛔ Excel blocked (use Clipboard/CSV)', false); return; } readFileAsArrayBuffer(f).then(function(ab){ var added=0; readXLSXArrayBuffer(ab).then(function(res){ var sheets=res.sheets; var sh=sheets && sheets[0]; if(!sh || !sh.rows || !sh.rows.length){ addFileStatus(f.name,'⚠️ 0 docs', false); } else { if(sh.rows[0].every(function(x){return String(x||'').trim()==='';})){ var width=0; for(var r=0;r<sh.rows.length;r++){ if(sh.rows[r].length>width) width=sh.rows[r].length; } sh.rows[0]=Array.from({length:width}).map(function(_,ii){return 'col_'+(ii+1)}); }
              added = addRowsAsDocs(sh.rows, f.name+':'+sh.name);
              addFileStatus(f.name, added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added);
            }
          }).catch(function(ex){ addFileStatus(f.name,'⛔ Excel error', false); showErr('Excel error: '+(ex && ex.message? ex.message : ex)); });
        }); }); }
        else if(kind==='csv'){ readFileAsText(f).then(function(txt){ var rows=parseCSV(txt); var added=addRowsAsDocs(rows,f.name); addFileStatus(f.name, added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); }); }
        else if(kind==='json'){ readFileAsText(f).then(function(txt){ var added=0; if(f.name.toLowerCase().endsWith('.jsonl')){ txt.split(/\r?\n/).filter(function(x){return x;}).forEach(function(line,idx){ try{ var o=JSON.parse(line); rawDocs.push({id:f.name+'#'+(idx+1), text:String(o.text||JSON.stringify(o)), label:o.label}); added++; }catch(e){ rawDocs.push({id:f.name+'#'+(idx+1), text:line}); added++; } }); }
          else { try{ var j=JSON.parse(txt); if(Array.isArray(j)){ for(var k=0;k<j.length;k++){ var o=j[k]; rawDocs.push({id:f.name+'#'+(k+1), text:String(o.text||JSON.stringify(o)), label:o.label}); added++; } } else { rawDocs.push({id:f.name, text: JSON.stringify(j)}); added++; } }catch(e){ rawDocs.push({id:f.name, text: txt}); added++; } }
          updateFileCounter(); sampleDocs(); addFileStatus(f.name, '✅ '+added+' docs', true);
        }); }
        else if(kind==='text'){ readFileAsText(f).then(function(txt){ rawDocs.push({id:f.name, text:txt}); updateFileCounter(); sampleDocs(); addFileStatus(f.name,'✅ 1 doc', true); }); }
        else { addFileStatus(f.name,'⛔ Unsupported', false); }
      }catch(err){ addFileStatus(f.name,'⛔ Read error', false); }
    })(files[i]); }
}

// ---------- Train, Ask, Classify ----------
var WORD_RE = /[A-Za-z0-9]{2,}/g; // ASCII fallback only (works in every browser)
function tokenize(t){ var s=String(t||'').toLowerCase(); var m=s.match(WORD_RE); return m || []; }
function train(){ if(!rawDocs.length){ showErr('Upload data first'); return; } MODEL = rawDocs.map(function(d){ return {id:d.id, text:d.text, label:d.label, tokens:new Set(tokenize(d.text))}; }); var k=$('#stats'); if(k){ k.innerHTML=''; k.appendChild(chip('Docs', String(MODEL.length))); k.appendChild(chip('Mode','OVERLAP')); } setBar(1); log('— Training complete'); }
function askLocal(q, k){ if(!MODEL) return []; if(typeof k==='undefined') k=3; var qTok=new Set(tokenize(q)); function score(doc){ var s=0; qTok.forEach(function(w){ if(doc.tokens.has(w)) s++; }); return s; }
  var arr = MODEL.slice(0);
  arr = arr.map(function(d){ return {id:d.id, text:d.text, label:d.label, score:score(d)}; }).sort(function(a,b){ return b.score-a.score; }).slice(0,k);
  return arr; }
function ask(){ if(!MODEL){ showErr('Train first'); return; } var qEl=$('#q'); var q = qEl? (qEl.value||'').trim() : ''; if(!q){ showErr('Type a question'); return; } var top=askLocal(q,3); var out=$('#answers'); if(out){ out.innerHTML=''; top.forEach(function(s){ var div=document.createElement('div'); div.style.border='1px solid #1a2a49'; div.style.borderRadius='10px'; div.style.padding='10px'; div.style.margin='8px 0'; div.innerHTML='<div class="muted">'+s.id+' — score '+s.score+(s.label? ' — label '+s.label:'')+'</div><div>'+s.text.replace(/</g,'&lt;').replace(/>/g,'&gt;')+'</div>'; out.appendChild(div); }); } }
function predict(){ var xEl=$('#xtext'); var x = xEl? (xEl.value||'').trim() : ''; if(!x){ showErr('Type text to classify'); return; } if(!MODEL){ showErr('Train first'); return; } var byLabel=new Map(); for(var i=0;i<MODEL.length;i++){ var d=MODEL[i]; if(!d.label) continue; if(!byLabel.has(d.label)) byLabel.set(d.label, []); byLabel.get(d.label).push(d); }
  if(!byLabel.size){ showErr('No labels found in data'); return; }
  var qTok=new Set(tokenize(x)); var scores=[]; byLabel.forEach(function(docs,label){ var s=0; docs.forEach(function(d){ qTok.forEach(function(w){ if(d.tokens.has(w)) s++; }); }); scores.push({label:label, score:s}); });
  scores.sort(function(a,b){ return b.score-a.score; }); var out=$('#predOut'); if(out){ out.innerHTML = scores.map(function(s){ return s.label+': '+s.score; }).join('\n'); }
}

// ---------- Local API (fetch interception) ----------
var API_ENABLED=false; var NATIVE_FETCH = window.fetch.bind(window);
function enableAPI(){ if(API_ENABLED) return; API_ENABLED=true; window.fetch = function(input, init){ init = init || {}; var url = (typeof input==='string')? input : input.url; var method = (init.method||'GET').toUpperCase(); if(url.indexOf('/api/')===0){ try{
      if(url==='/api/status' && method==='GET'){
        return Promise.resolve(new Response(JSON.stringify({ ok:true, docs: rawDocs.length, trained: !!MODEL }), {headers:{'Content-Type':'application/json'}}));
      }
      if(url==='/api/train' && method==='POST'){
        train();
        return Promise.resolve(new Response(JSON.stringify({ ok:true, trained: !!MODEL, docs: rawDocs.length }), {headers:{'Content-Type':'application/json'}}));
      }
      if(url==='/api/index' && method==='POST'){
        var body = init.body? JSON.parse(init.body) : {}; var docs = Array.isArray(body.docs)? body.docs : [];
        var added=0; for(var i=0;i<docs.length;i++){ var d=docs[i]; if(d && d.text){ rawDocs.push({id:d.id||( 'api#'+(rawDocs.length+1) ), text:String(d.text), label:d.label}); added++; } }
        updateFileCounter(); sampleDocs();
        return Promise.resolve(new Response(JSON.stringify({ ok:true, added:added, total: rawDocs.length }), {headers:{'Content-Type':'application/json'}}));
      }
      if(url==='/api/ask' && method==='POST'){
        var body2 = init.body? JSON.parse(init.body) : {}; var q=String(body2.q||''); var k=Number(body2.k||3);
        var res = askLocal(q,k).map(function(r){ return { id:r.id, label:r.label, score:r.score, text:r.text }; });
        return Promise.resolve(new Response(JSON.stringify({ ok:true, q:q, k:k, results: res }), {headers:{'Content-Type':'application/json'}}));
      }
      return Promise.resolve(new Response(JSON.stringify({ ok:false, error:'Not Found'}), {status:404, headers:{'Content-Type':'application/json'}}));
    }catch(err){ return Promise.resolve(new Response(JSON.stringify({ ok:false, error:String(err && err.message || err)}), {status:500, headers:{'Content-Type':'application/json'}})); }
  }
  return NATIVE_FETCH(input, init);
}; text('#apiStatus','Local API: enabled'); log('Local API enabled'); }
function syncAPI(){ log('Synced current docs to API context'); }
function testStatus(){ fetch('/api/status').then(function(r){ return r.json(); }).then(function(j){ var el=$('#apiOut'); if(el) el.textContent = JSON.stringify(j,null,2); }); }
function runApiAsk(){ var qEl=$('#apiQ'); var q= qEl? (qEl.value||'price') : 'price'; fetch('/api/ask',{method:'POST', headers:{'Content-Type':'application/json'}, body:JSON.stringify({q:q, k:3})}).then(function(r){ return r.json(); }).then(function(j){ var el=$('#apiOut'); if(el) el.textContent = JSON.stringify(j,null,2); }); }
function runSample(){ fetch('/api/ask',{method:'POST', headers:{'Content-Type':'application/json'}, body:JSON.stringify({q:'price', k:3})}).then(function(r){ return r.json(); }).then(function(j){ var el=$('#apiOut'); if(el) el.textContent = JSON.stringify(j,null,2); }).catch(function(e){ var el=$('#apiOut'); if(el) el.textContent=String(e); }); }

// ---------- Init wiring (no optional chaining) ----------
function wire(){
  var ms=$('#modeSwitch'); on(ms,'click', function(){ var adv=!document.body.classList.contains('advanced'); document.body.classList.toggle('advanced', adv); document.body.classList.toggle('simple', !adv); });
  var dz=$('#drop'), fi=$('#file'), choose=$('#chooseBtn');
  if(dz){ ['dragenter','dragover'].forEach(function(evt){ on(dz,evt,function(e){ e.preventDefault(); dz.classList.add('drag'); }); }); ['dragleave','drop'].forEach(function(evt){ on(dz,evt,function(e){ e.preventDefault(); if(evt==='dragleave') dz.classList.remove('drag'); }); }); on(dz,'drop',function(e){ dz.classList.remove('drag'); var fs=e.dataTransfer && e.dataTransfer.files; if(fs && fs.length) addFiles(fs); }); on(dz,'click', function(e){ if((e.target&&e.target.id)!=='chooseBtn' && fi) fi.click(); }); }
  if(choose) on(choose,'click', function(e){ e.stopPropagation(); if(fi) fi.click(); });
  if(fi) on(fi,'change', function(e){ var fs=e.target && e.target.files; if(fs && fs.length) addFiles(fs); });
  on($('#btnSample'),'click', function(){ var rows=[['title','body','label'],['Welcome','This is a tiny demo.','info'],['Pricing','Price is 9€/month.','pricing'],['Support','Email support@example.com','support']]; var csv=rows.map(function(r){return r.join(',');}).join('\n'); var f=new File([csv],'sample.csv',{type:'text/csv'}); addFiles([f]); });
  on($('#btnReadClipboard'),'click', function(){ readClipboardExcel(); });
  on($('#btnAddPaste'),'click', function(){ var t=$('#paste'); var v=(t && t.value||'').trim(); if(!v){ showErr('No text to add'); return; } rawDocs.push({id:'pasted#'+(rawDocs.length+1), text:v}); t.value=''; updateFileCounter(); sampleDocs(); addFileStatus('pasted','✅ 1 doc',true); });
  on($('#btnAddPasteGrid'),'click', function(){ var t=$('#paste'); var v=(t && t.value||'').trim(); if(!v){ showErr('No grid to add'); return; } var rows = v.indexOf('<table')>=0? parseHTMLTable(v) : parseGrid(v); var added=addRowsAsDocs(rows,'grid'); addFileStatus('grid', added?('✅ '+added+' docs'):'⚠️ 0 docs', !!added); t.value=''; });
  on($('#btnTrain'),'click', function(){ train(); });
  on($('#btnAsk'),'click', function(){ ask(); });
  on($('#btnPredict'),'click', function(){ predict(); });
  on($('#btnReset'),'click', function(){ rawDocs=[]; MODEL=null; setBar(0); text('#docSample',''); text('#logs',''); var fl=$('#filelist'); if(fl) fl.innerHTML=''; var ans=$('#answers'); if(ans) ans.innerHTML=''; updateFileCounter(); });
  on($('#mClipboard'),'click', function(){ show('#excelModal', false); readClipboardExcel(); });
  on($('#mClose'),'click', function(){ show('#excelModal', false); });
  on($('#mPasteDone'),'click', function(){ consumePaste(); });
  on($('#mPasteClose'),'click', function(){ show('#pasteModal', false); });
  on($('#btnEnableAPI'),'click', function(){ enableAPI(); });
  on($('#btnSyncAPI'),'click', function(){ syncAPI(); });
  on($('#btnTestStatus'),'click', function(){ testStatus(); });
  on($('#btnApiAsk'),'click', function(){ runApiAsk(); });
  on($('#btnRunSample'),'click', function(){ runSample(); });

  log('Ready: 1) Upload  2) Train  3) Ask');
}
if(document.readyState==='loading') document.addEventListener('DOMContentLoaded', wire); else wire();
</script>
</body>
</html>
