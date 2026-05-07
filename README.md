<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Department of the Navy — Org Chart</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Segoe UI',Arial,sans-serif;font-size:13px;background:#1a1a2e;color:#e0e0e0;display:flex;height:100vh;overflow:hidden}

/* SIDEBAR */
#sidebar{width:220px;min-width:220px;background:#16213e;border-right:1px solid #2a2a4a;display:flex;flex-direction:column;padding:14px;gap:10px;overflow-y:auto;z-index:10}
#sidebar h1{font-size:13px;font-weight:700;color:#a0c4ff;letter-spacing:.5px;border-bottom:1px solid #2a2a4a;padding-bottom:8px;line-height:1.4}
.legend-title{font-size:11px;font-weight:600;color:#888;text-transform:uppercase;letter-spacing:.8px;margin-top:4px}
.person-btn{display:flex;align-items:center;gap:8px;padding:7px 10px;border-radius:6px;border:none;cursor:pointer;width:100%;text-align:left;font-size:12px;font-weight:600;color:#fff;margin-bottom:4px;transition:opacity .15s}
.person-btn:hover{opacity:.85}
.person-btn .dot{width:12px;height:12px;border-radius:50%;flex-shrink:0}
.btn-sinclair{background:#d35400}
.btn-coronella{background:#c2185b}
.btn-higgins{background:#1e8449}
.btn-ash{background:#008080}
.btn-disqualified{background:#c0392b}
.btn-unassigned{background:#7d3c98}
.btn-disputed{background:#b8960a;color:#1a1a00}
.btn-whitespace{background:#aab7b8;color:#1a1a2e}

/* STATS */
.stats-box{background:#0f3460;border-radius:6px;padding:10px}
.stats-box .stat-row{display:flex;align-items:center;justify-content:space-between;padding:3px 0;font-size:12px}
.stat-row .stat-name{display:flex;align-items:center;gap:6px}
.stat-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.stat-count{font-weight:700;color:#a0c4ff}

/* CONTROLS */
.ctrl-btn{background:#2a2a4a;border:1px solid #3a3a6a;color:#ccc;padding:6px 10px;border-radius:5px;cursor:pointer;font-size:11px;width:100%;margin-bottom:4px;transition:background .15s}
.ctrl-btn:hover{background:#3a3a6a}

/* SEARCH */
#searchBox{width:100%;padding:6px 8px;background:#0f3460;border:1px solid #2a4a7a;border-radius:5px;color:#e0e0e0;font-size:12px;outline:none}
#searchBox::placeholder{color:#556}
#searchBox:focus{border-color:#4a90d9}
.search-count{font-size:11px;color:#888;text-align:center}

/* CHART AREA */
#chart-area{flex:1;overflow:auto;padding:20px 24px}
.tree-root{padding-left:0}

/* NODE */
.tree-node{margin:0;position:relative}
.node-row{display:flex;align-items:stretch;min-height:28px;margin-bottom:2px}
.node-indent{display:flex;align-items:stretch;flex-shrink:0}
.indent-unit{width:20px;position:relative;flex-shrink:0}
.indent-unit::before{content:'';position:absolute;left:9px;top:0;bottom:0;width:1px;background:#2a3a5a}
.indent-unit.last::before{bottom:50%}
.indent-unit::after{content:'';position:absolute;left:9px;top:50%;width:11px;height:1px;background:#2a3a5a}
.indent-unit:not(.connector)::after{display:none}
.connector::after{display:block}

.toggle{width:18px;height:18px;flex-shrink:0;border-radius:3px;background:#2a3a5a;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;color:#8899bb;font-size:10px;margin-right:4px;align-self:center;transition:background .15s}
.toggle:hover{background:#3a4a7a}
.toggle-spacer{width:18px;height:18px;flex-shrink:0;margin-right:4px}

.node-label{display:inline-flex;align-items:center;padding:4px 10px;border-radius:5px;cursor:pointer;transition:filter .15s;background:#1e2d4a;border:1px solid #2a3a5a;flex:1;min-width:0;user-select:none}
.node-label:hover{filter:brightness(1.2)}
.node-label.highlighted{outline:2px solid #fff;outline-offset:1px}
.node-text{flex:1;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;font-size:12px;line-height:1.3}
.node-loc{font-size:10px;color:rgba(160,196,255,.5);margin-left:10px;flex-shrink:0;font-style:italic;white-space:nowrap}
.node-id{font-size:10px;color:rgba(255,255,255,.35);margin-left:6px;flex-shrink:0;font-family:monospace}
.node-assigned{font-size:10px;color:rgba(255,255,255,.7);margin-left:6px;flex-shrink:0;font-weight:600}

.children-wrap{padding-left:20px}

/* POPUP */
#popup{position:fixed;z-index:1000;background:#1a2540;border:1px solid #3a4a7a;border-radius:8px;padding:12px 14px 10px;box-shadow:0 8px 32px rgba(0,0,0,.7);min-width:300px;max-width:340px;display:none}
#popup.visible{display:block}
#popup-name{font-size:11px;color:#a0c4ff;margin-bottom:8px;font-weight:600;max-width:300px;word-wrap:break-word;line-height:1.4;padding-right:18px}
.popup-section{font-size:10px;color:#667;text-transform:uppercase;letter-spacing:.7px;margin:8px 0 4px;font-weight:600}
.popup-ae-wrap{display:flex;flex-wrap:wrap;gap:3px}
.popup-person{padding:4px 9px;border-radius:4px;border:2px solid transparent;cursor:pointer;font-size:11px;font-weight:600;color:#fff;transition:all .12s;white-space:nowrap}
.popup-person:hover{filter:brightness(1.15)}
.popup-person.ae-active{border-color:#fff !important}
.popup-tags{display:flex;flex-wrap:wrap;gap:3px}
.popup-tag{padding:4px 9px;border-radius:4px;border:1px solid #2a3a5a;cursor:pointer;font-size:11px;font-weight:600;color:#888;background:#1a2a40;transition:all .12s}
.popup-tag:hover{filter:brightness(1.2);color:#ccc}
.popup-tag.tag-active{color:#fff}
.tag-yes.tag-active{background:#1a5c1a;border-color:#27ae60}
.tag-no.tag-active{background:#5c1a1a;border-color:#e74c3c}
.tag-undecided.tag-active{background:#3a3a4a;border-color:#888}
.tag-customer.tag-active{background:#0d3b6e;border-color:#3498db}
.tag-engaged.tag-active{background:#5c3a00;border-color:#e67e22}
.tag-notengaged.tag-active{background:#4a1a6e;border-color:#9b59b6}
.tag-doesntbuy.tag-active{background:#5c1a1a;border-color:#e74c3c}
#popup-notes{width:100%;background:#0f2040;border:1px solid #2a4a7a;border-radius:4px;color:#dde;font-size:11px;padding:5px 7px;resize:none;height:46px;margin-top:2px;font-family:inherit;line-height:1.4}
#popup-notes:focus{outline:none;border-color:#4a90d9}
.popup-clear{background:#1a2a40;border:1px solid #2a3a5a;color:#777;padding:5px 10px;border-radius:4px;cursor:pointer;font-size:11px;width:100%;margin-top:8px;transition:all .12s}
.popup-clear:hover{background:#2a3a5a;color:#ccc}
.popup-close{position:absolute;top:7px;right:9px;background:none;border:none;color:#555;cursor:pointer;font-size:14px;line-height:1}
.popup-close:hover{color:#ccc}

/* NODE BADGES */
.node-badges{display:flex;gap:3px;margin-left:7px;flex-shrink:0;align-items:center}
.badge{font-size:9px;font-weight:700;padding:1px 4px;border-radius:3px;line-height:1.5;white-space:nowrap}
.badge-t-yes{background:#1a5c1a;color:#7dff7d}
.badge-t-no{background:#5c1a1a;color:#ff9090}
.badge-t-undecided{background:#333;color:#aaa}
.badge-eng-cust{background:#0d3b6e;color:#7dc3ff}
.badge-eng-engd{background:#5c3a00;color:#ffc37d}
.badge-eng-neng{background:#4a1a6e;color:#d7b3ff}
.badge-eng-dbuy{background:#5c1a1a;color:#ffb3b3}
.badge-notes{background:#2a3a5a;color:#8899bb;cursor:default}

/* SEARCH HIGHLIGHT */
.search-match>.node-row>.node-label{outline:2px solid #f39c12;outline-offset:1px}
.search-hidden{display:none}

/* COLOR CLASSES */
.color-sinclair{background:#6e2800 !important;border-color:#d35400 !important}
.color-coronella{background:#5c0a2e !important;border-color:#c2185b !important}
.color-higgins{background:#0a4a23 !important;border-color:#1e8449 !important}
.color-ash{background:#003d3d !important;border-color:#00a0a0 !important}
.color-disqualified{background:#7b1414 !important;border-color:#c0392b !important}
.color-unassigned{background:#4a1a6e !important;border-color:#7d3c98 !important}
.color-disputed{background:#4a3c00 !important;border-color:#d4a800 !important}
.color-whitespace{background:#d5d8dc !important;border-color:#aab7b8 !important;color:#1a1a2e !important}
.color-whitespace .node-loc,.color-whitespace .node-id,.color-whitespace .node-assigned{color:rgba(26,26,46,.5) !important}

/* CONFLICT MODAL */
#conflictModal{display:none;position:fixed;inset:0;background:rgba(0,0,20,.85);z-index:2000;overflow-y:auto;padding:20px}
#conflictModalInner{background:#0d0d2b;border:1px solid #2a2a6a;border-radius:8px;max-width:780px;margin:20px auto;padding:20px}
#conflictModal h2{color:#f0c000;margin:0 0 6px;font-size:16px}
#conflictModal .cm-sub{color:#888;font-size:12px;margin-bottom:14px}
.conflict-node{border:1px solid #1e2a4a;border-radius:6px;margin-bottom:10px;overflow:hidden}
.conflict-node-header{background:#111830;padding:8px 10px;display:flex;align-items:center;justify-content:space-between;gap:8px}
.conflict-node-name{color:#a8c0e8;font-size:13px;font-weight:bold;flex:1}
.conflict-node-btns{display:flex;gap:4px;flex-shrink:0}
.conflict-field{padding:8px 10px;display:flex;align-items:center;gap:8px;border-top:1px solid #1a2244;flex-wrap:wrap}
.cf-field{color:#778;font-size:11px;width:70px;text-transform:uppercase;letter-spacing:.5px;flex-shrink:0}
.cf-val{padding:3px 8px;border-radius:4px;font-size:12px;background:#1a1a3a;color:#ccc}
.cf-arrow{color:#444;font-size:14px}
.cf-btns{display:flex;gap:4px;margin-left:auto}
.cf-btn{padding:3px 8px;border:1px solid #2a3a6a;background:#111830;color:#99a;border-radius:4px;font-size:11px;cursor:pointer;transition:background .15s}
.cf-btn:hover{background:#1e2a5a}
.cf-btn.res-local{background:#1a4a2a;border-color:#27ae60;color:#7dcea0}
.cf-btn.res-import{background:#1a2a5a;border-color:#3498db;color:#85c1e9}
.cf-btn.res-disputed{background:#4a3c00;border-color:#d4a800;color:#ffe500}
#conflictFooter{display:flex;gap:8px;justify-content:flex-end;margin-top:14px;padding-top:12px;border-top:1px solid #1e2a4a;flex-wrap:wrap}
.cm-btn{padding:7px 16px;border-radius:5px;cursor:pointer;font-size:13px;border:1px solid}
.cm-btn-disputed{background:#4a3c00;color:#ffe500;border-color:#d4a800}
.cm-btn-apply{background:#1a4a2a;color:#7dcea0;border-color:#27ae60}
.cm-btn-cancel{background:#2a1a1a;color:#e88;border-color:#7b1414}

/* EDIT MODE */
#editModeBar{display:none;background:#2a1a00;border-bottom:2px solid #d4a800;padding:6px 12px;text-align:center;font-size:12px;color:#f0c000;position:sticky;top:0;z-index:90;letter-spacing:.5px}
body.edit-mode #editModeBar{display:block}
.node-edit-btns{display:none;gap:2px;margin-left:6px;flex-shrink:0;align-items:center}
body.edit-mode .node-edit-btns{display:flex}
.node-edit-btn{padding:1px 5px;border-radius:3px;border:1px solid #2a3a6a;background:#0d0d2b;color:#667;font-size:10px;cursor:pointer;line-height:1.5;transition:background .12s}
.node-edit-btn:hover{background:#1e2a5a;color:#ccc}
.node-edit-btn.btn-eadd{border-color:#1e8449;color:#7dcea0}
.node-edit-btn.btn-eadd:hover{background:#0a2a18}
.node-edit-btn.btn-eren{border-color:#3498db;color:#85c1e9}
.node-edit-btn.btn-eren:hover{background:#0a1a3a}
.node-edit-btn.btn-edel{border-color:#7b1414;color:#e88}
.node-edit-btn.btn-edel:hover{background:#2a0a0a}

/* DRAG AND DROP */
body.edit-mode .node-row{cursor:grab}
body.edit-mode .node-row:active{cursor:grabbing}
.tree-node.drag-src>.node-row>.node-label{opacity:.35}
.tree-node.drop-child>.node-row>.node-label{outline:2px solid #f0c000 !important;box-shadow:0 0 10px rgba(240,192,0,.35) !important}
.tree-node.drop-before>.node-row{box-shadow:inset 0 2px 0 #f0c000}
.tree-node.drop-after>.node-row{box-shadow:inset 0 -2px 0 #f0c000}

/* STRUCT MODAL */
#structModal{display:none;position:fixed;inset:0;background:rgba(0,0,20,.85);z-index:2100;align-items:center;justify-content:center}
#structModal.open{display:flex}
#structModalInner{background:#0d0d2b;border:1px solid #2a2a6a;border-radius:8px;width:360px;padding:20px}
#structModal h3{margin:0 0 14px;font-size:14px;color:#a8c0e8}
.struct-field{margin-bottom:12px}
.struct-field label{display:block;font-size:11px;color:#778;margin-bottom:4px;text-transform:uppercase;letter-spacing:.5px}
.struct-field input{width:100%;box-sizing:border-box;padding:7px 10px;background:#070718;border:1px solid #2a3a6a;border-radius:4px;color:#cde;font-size:13px}
.struct-field input:focus{outline:none;border-color:#5a80c0}
#structModalFooter{display:flex;gap:8px;justify-content:flex-end;margin-top:14px}

/* FILE STATUS */
#fileSection{border-top:1px solid #2a2a4a;padding-top:10px;margin-top:2px}
#fileStatus{font-size:10px;color:#556;text-align:center;padding:2px 0;min-height:14px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
#fileStatus.ok{color:#27ae60}
#fileStatus.err{color:#e74c3c}
#fetchStatus{font-size:10px;color:#556;text-align:center;padding:2px 0;min-height:14px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
#fetchStatus.ok{color:#27ae60}
#fetchStatus.err{color:#e74c3c}
.file-section-divider{border-top:1px solid #1a2244;margin:6px 0}

/* GITHUB SETTINGS MODAL */
#ghModal{display:none;position:fixed;inset:0;background:rgba(0,0,20,.85);z-index:2200;align-items:center;justify-content:center}
#ghModal.open{display:flex}
#ghModalInner{background:#0d0d2b;border:1px solid #2a2a6a;border-radius:8px;width:400px;padding:20px}
#ghModalInner h3{margin:0 0 8px;font-size:14px;color:#a8c0e8}
.gh-note{font-size:11px;color:#667;margin:0 0 14px;line-height:1.5}
.gh-note strong{color:#a8c0e8}
.gh-token-wrap{position:relative}
.gh-token-wrap input{padding-right:32px}
.gh-eye{position:absolute;right:8px;top:50%;transform:translateY(-50%);background:none;border:none;color:#556;cursor:pointer;font-size:13px;padding:0}
.gh-eye:hover{color:#ccc}
.file-btn-row{display:flex;gap:4px}
.file-btn-row .ctrl-btn{margin-bottom:0;flex:1;padding:5px 4px;font-size:10px}

/* ROOT NODE */
.root-node>.node-row>.node-label{font-size:14px;font-weight:700;background:#0f3460;border-color:#1a5276}
/* TOP-LEVEL BRANCH (depth 1) */
.branch-node>.node-row>.node-label{font-size:13px;font-weight:700;background:#0a2040;border-color:#1a3a60}
</style>
</head>
<body>

<div id="sidebar">
  <h1>Nutanix US Navy / USMC<br>Account Mapping Tool</h1>

  <div class="legend-title">Assign to Person</div>
  <button class="person-btn btn-sinclair" onclick="setFilter('Sinclair')"><span class="dot" style="background:#e67e22"></span>Sinclair</button>
  <button class="person-btn btn-coronella" onclick="setFilter('Coronella')"><span class="dot" style="background:#e91e8c"></span>Coronella</button>
  <button class="person-btn btn-higgins" onclick="setFilter('Higgins')"><span class="dot" style="background:#27ae60"></span>Higgins</button>
  <button class="person-btn btn-ash" onclick="setFilter('Ash')"><span class="dot" style="background:#00c8c8"></span>Ash</button>
  <button class="person-btn btn-disqualified" onclick="setFilter('Disqualified')"><span class="dot" style="background:#e74c3c"></span>Disqualified</button>
  <button class="person-btn btn-unassigned" onclick="setFilter('Unassigned')"><span class="dot" style="background:#bb8fce"></span>Unassigned</button>
  <button class="person-btn btn-disputed" onclick="setFilter('Disputed')"><span class="dot" style="background:#ffe500"></span>Disputed</button>
  <button class="person-btn btn-whitespace" onclick="setFilter('Whitespace')"><span class="dot" style="background:#e8eaed;border:1px solid #aab7b8"></span>Whitespace</button>

  <div class="legend-title" style="margin-top:4px">Ownership</div>
  <div class="stats-box">
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#e67e22"></span>Sinclair</span><span class="stat-count" id="count-Sinclair">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#e91e8c"></span>Coronella</span><span class="stat-count" id="count-Coronella">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#27ae60"></span>Higgins</span><span class="stat-count" id="count-Higgins">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#00c8c8"></span>Ash</span><span class="stat-count" id="count-Ash">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#e74c3c"></span>Disqualified</span><span class="stat-count" id="count-Disqualified">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#bb8fce"></span>Unassigned</span><span class="stat-count" id="count-Unassigned">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#ffe500"></span>Disputed</span><span class="stat-count" id="count-Disputed">0</span></div>
    <div class="stat-row"><span class="stat-name"><span class="stat-dot" style="background:#e8eaed;border:1px solid #aab7b8"></span>Whitespace</span><span class="stat-count" id="count-Whitespace">0</span></div>
    <div class="stat-row" style="border-top:1px solid #2a3a5a;margin-top:4px;padding-top:6px"><span style="color:#888">No Tag</span><span class="stat-count" id="count-none">0</span></div>
  </div>

  <div class="legend-title">Search</div>
  <input id="searchBox" type="text" placeholder="Search orgs..." oninput="doSearch(this.value)">
  <div class="search-count" id="searchCount"></div>

  <div id="fileSection">
    <div class="legend-title">Shared File</div>
    <div class="file-btn-row">
      <button class="ctrl-btn" onclick="fetchSharedFile()" title="Fetch navy_org_data.json from the same URL path">↻ Sync</button>
      <button class="ctrl-btn" onclick="saveToGitHub()" title="Commit navy_org_data.json to GitHub repo">⬆ Save to Repo</button>
      <button class="ctrl-btn" onclick="showGhSettings()" title="GitHub API settings" style="flex:0;padding:5px 8px">⚙</button>
    </div>
    <div id="fetchStatus"></div>
    <div class="file-section-divider"></div>
    <div class="file-btn-row">
      <button class="ctrl-btn" onclick="connectFile()" title="Connect a local JSON file (Chrome/Edge)">⟳ Local File</button>
      <button class="ctrl-btn" onclick="newSharedFile()" title="Create a new local JSON file">+ New</button>
    </div>
    <div id="fileStatus">No local file</div>
    <button class="ctrl-btn" id="btnLoadFile" onclick="loadFromFile()" style="display:none">⬆ Reload Local</button>
  </div>

  <div class="legend-title">Controls</div>
  <button class="ctrl-btn" onclick="expandAll()">▼ Expand All</button>
  <button class="ctrl-btn" onclick="collapseAll()">▶ Collapse All</button>
  <button class="ctrl-btn" onclick="resetAll()" style="color:#e88">⟳ Reset All Colors</button>
  <button class="ctrl-btn" onclick="exportAssignments()">⬇ Export CSV</button>
  <button class="ctrl-btn" onclick="importAssignments()">⬆ Merge Import CSV</button>
  <button class="ctrl-btn" id="btnEditMode" onclick="toggleEditMode()" style="margin-top:6px;border-color:#d4a800;color:#f0c000">✎ Edit Structure</button>
</div>

<div id="chart-area">
  <div id="editModeBar">✎ STRUCTURE EDIT MODE — Click + to add a child, ✎ to rename, ✕ to delete</div>
  <div id="tree-root"></div>
</div>

<div id="popup">
  <button class="popup-close" onclick="closePopup()">✕</button>
  <div id="popup-name"></div>

  <div class="popup-section">AE</div>
  <div class="popup-ae-wrap" id="popup-ae-row"></div>

  <div class="popup-section">Target</div>
  <div class="popup-tags">
    <button class="popup-tag tag-yes"       onclick="setPopupField('target','Yes')">Yes</button>
    <button class="popup-tag tag-no"        onclick="setPopupField('target','No')">No</button>
    <button class="popup-tag tag-undecided" onclick="setPopupField('target','Undecided')">Undecided</button>
  </div>

  <div class="popup-section">Engagement</div>
  <div class="popup-tags">
    <button class="popup-tag tag-customer"   onclick="setPopupField('engagement','Customer')">Customer</button>
    <button class="popup-tag tag-engaged"    onclick="setPopupField('engagement','Engaged')">Engaged</button>
    <button class="popup-tag tag-notengaged" onclick="setPopupField('engagement','Not Engaged')">Not Engaged</button>
    <button class="popup-tag tag-doesntbuy"  onclick="setPopupField('engagement','Doesn\'t Buy')">Doesn't Buy</button>
  </div>

  <div class="popup-section">Notes</div>
  <textarea id="popup-notes" placeholder="Add notes..."></textarea>

  <button class="popup-clear" onclick="clearAll()">✕ Clear All</button>
</div>

<div id="ghModal">
  <div id="ghModalInner">
    <h3>GitHub Settings</h3>
    <p class="gh-note">Provide a <strong>Personal Access Token</strong> with <code>repo</code> write scope to commit the shared JSON directly to the repository. The token is stored only in your browser's localStorage.</p>
    <div class="struct-field">
      <label>Personal Access Token</label>
      <div class="gh-token-wrap">
        <input id="ghToken" type="password" placeholder="ghp_…" autocomplete="off">
        <button class="gh-eye" onclick="const i=document.getElementById('ghToken');i.type=i.type==='password'?'text':'password'">👁</button>
      </div>
    </div>
    <div class="struct-field">
      <label>Owner (username or org)</label>
      <input id="ghOwner" type="text" placeholder="username">
    </div>
    <div class="struct-field">
      <label>Repository name</label>
      <input id="ghRepo" type="text" placeholder="repo-name">
    </div>
    <div class="struct-field">
      <label>File path in repo</label>
      <input id="ghFile" type="text" placeholder="navy_org_data.json">
    </div>
    <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:14px">
      <button class="cm-btn cm-btn-cancel" onclick="closeGhSettings()">Cancel</button>
      <button class="cm-btn cm-btn-apply" onclick="applyGhSettings()">Save Settings</button>
    </div>
  </div>
</div>

<div id="structModal">
  <div id="structModalInner">
    <h3 id="structModalTitle">Edit Node</h3>
    <div class="struct-field">
      <label>Name</label>
      <input id="structName" type="text" placeholder="Node name">
    </div>
    <div class="struct-field">
      <label>Location (City, State)</label>
      <input id="structLoc" type="text" placeholder="e.g. San Diego, CA">
    </div>
    <div id="structModalFooter">
      <button class="cm-btn cm-btn-cancel" onclick="closeStructModal()">Cancel</button>
      <button class="cm-btn cm-btn-apply" id="structApplyBtn" onclick="applyStructModal()">Save</button>
    </div>
  </div>
</div>

<div id="conflictModal">
  <div id="conflictModalInner">
    <h2>Import Conflicts</h2>
    <div class="cm-sub" id="conflictSub"></div>
    <div id="conflictList"></div>
    <div id="conflictFooter">
      <button class="cm-btn cm-btn-disputed" onclick="resolveAllDisputed()">⚑ Mark All Disputed</button>
      <button class="cm-btn cm-btn-apply" onclick="applyConflictResolutions()">✓ Apply</button>
      <button class="cm-btn cm-btn-cancel" onclick="cancelConflict()">✕ Cancel</button>
    </div>
  </div>
</div>

<script>
// ─── DATA ────────────────────────────────────────────────────────────────────
let ORG = {id:"root",name:"Department of the Navy",children:[

  // ══════════════════════════════════════════════════════════════════════
  //  US NAVY
  // ══════════════════════════════════════════════════════════════════════
  {id:"usn",name:"US Navy",children:[

    // ── ACQUISITION ───────────────────────────────────────────────────
    {id:"usn.acq",name:"Acquisition (ASN RDA)",loc:"Arlington, VA",children:[

      {id:"23.2.0.1",name:"PEO Attack Submarines (PEO-SSN)",loc:"Washington, DC",children:[
        {id:"23.2.0.1.B",name:"Submarine Sensor Systems Program Office (PMS-435)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.C",name:"Columbia Class Submarine Program Office (PMS-397)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.D",name:"Submarine Combat & Weapons Control System Program Office (PMS-425)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.E",name:"Virginia Class Submarine Program Management Office (PMS-450)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.F",name:"Undersea Weapons Program Office (PMS-404)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.G",name:"Submarine Acoustic Systems Program Office (PMS-401)",loc:"Washington, DC",children:[]},
        {id:"23.2.0.1.H",name:"Undersea Defensive Warfare Systems Program Office (PMS-415)",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.ssbn",name:"PEO Strategic Submarines (PEO-SSBN)",loc:"Washington, DC",children:[]},

      {id:"23.2.1",name:"Office of Naval Research (ONR)",loc:"Arlington, VA",children:[
        {id:"23.2.1.1",name:"Naval Research Departments — Part 1",loc:"Arlington, VA",children:[]},
        {id:"23.2.1.2",name:"Naval Research Departments — Part 2",loc:"Arlington, VA",children:[]},
        {id:"23.2.1.3",name:"Naval Research Laboratory (NRL)",loc:"Washington, DC",children:[
          {id:"23.2.1.3.1",name:"Systems Directorate",loc:"Washington, DC",children:[]},
          {id:"23.2.1.3.2",name:"Ocean & Atmospheric Science & Technology Directorate",loc:"Washington, DC",children:[]},
          {id:"23.2.1.3.3",name:"Material Science & Component Technology",loc:"Washington, DC",children:[]}
        ]}
      ]},

      {id:"23.2.2",name:"PEO Unmanned & Small Combatants (PEO-USC)",loc:"Washington, DC",children:[
        {id:"23.2.2.B",name:"Unmanned Maritime Systems Program Office (PMS-406)",loc:"Washington, DC",children:[]},
        {id:"23.2.2.C",name:"LCS Mission Modules Program Office (PMS-420)",loc:"Washington, DC",children:[]},
        {id:"23.2.2.D",name:"Naval Special Warfare Program Office (PMS-340)",loc:"Washington, DC",children:[]},
        {id:"23.2.2.E",name:"Mine Warfare Program Office (PMS-495)",loc:"Washington, DC",children:[]},
        {id:"23.2.2.F",name:"Remote Minehunting System Program Office (PMS-403)",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.3",name:"PEO Air Anti-Submarine Warfare, Assault & Special Mission (PEO-A)",loc:"Patuxent River, MD",children:[
        {id:"23.2.3.B",name:"H-60 Multi-Mission Helicopters Program (PMA-299)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.3.C",name:"Marine Light Attack Helicopter / H-1 Program (PMA-276)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.3.D",name:"Air Anti-Submarine Warfare Systems Program Office (PMA-264)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.3.E",name:"H-53 Helicopters Program (PMA-261)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.3.F",name:"Presidential Helicopters Program Office (PMA-274)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.3.G",name:"Maritime Patrol & Reconnaissance Aircraft Program (PMA-290)",loc:"Patuxent River, MD",children:[]}
      ]},

      {id:"23.2.4",name:"PEO Tactical Aircraft Program (PEO-T)",loc:"Patuxent River, MD",children:[
        {id:"23.2.4.B",name:"Airborne Electronic Attack Systems Program (PMA-234)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.C",name:"E-6B Airborne Strategic C3 Program (PMA-271)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.D",name:"Air-to-Air Missile Systems (PMA-259)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.E",name:"Advanced Tactical Aircraft Protection Systems (PMA-272)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.F",name:"Naval Air Traffic Management Systems (PMA-213)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.G",name:"Aircraft Launch & Recovery Equipment Program Office (PMA-251)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.H",name:"F/A-18 & EA-18G Program Office (PMA-265)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.I",name:"AV-8B Weapon Systems Program (PMA-257)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.J",name:"Next Generation Air Dominance Program Office (PMA-230)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.K",name:"E-2/C-2 Airborne Command & Control Systems (PMA-231)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.4.L",name:"Naval Undergraduate Flight Training Systems (PMA-273)",loc:"Patuxent River, MD",children:[]}
      ]},

      {id:"23.2.5",name:"PEO Unmanned Aviation & Strike Weapons (PEO-W)",loc:"Patuxent River, MD",children:[
        {id:"23.2.5.B",name:"Strike Weapons — Tomahawk & Strike Planning (PMA-280/281)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.5.C",name:"Precision Strike Weapons Program Office (PMA-201)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.5.D",name:"Direct & Time Sensitive Strike Program (PMA-242)",loc:"Patuxent River, MD",children:[]}
      ]},

      {id:"23.2.6.B",name:"Strategic Systems Programs (SSP)",loc:"Washington, DC",children:[]},

      {id:"23.2.6.D",name:"PEO Digital & Enterprise Services (PEO-EIS)",loc:"San Diego, CA",children:[
        {id:"23.2.6.E",name:"Naval Enterprise Networks Program Office (PMW-205)",loc:"San Diego, CA",children:[]},
        {id:"23.2.6.F",name:"Special Networks & Intelligence Mission Applications (PMW-260)",loc:"San Diego, CA",children:[]},
        {id:"23.2.6.G",name:"Navy Commercial Cloud Services (PMW-270)",loc:"San Diego, CA",children:[]},
        {id:"23.2.6.H",name:"Special Access Programs (PMW-280)",loc:"San Diego, CA",children:[]},
        {id:"23.2.6.I",name:"Enterprise IT Strategic Sourcing (PMM-290)",loc:"San Diego, CA",children:[]}
      ]},

      {id:"23.2.6.J",name:"PEO Manpower, Logistics & Business Solutions (PEO-MLB)",loc:"Washington, DC",children:[
        {id:"23.2.6.K",name:"Navy Enterprise Business Solutions (PMW-220)",loc:"Washington, DC",children:[]},
        {id:"23.2.6.L",name:"Enterprise Systems & Services (PMW-250)",loc:"Washington, DC",children:[]},
        {id:"23.2.6.M",name:"Navy Maritime Maintenance Enterprise Solution (PMS-444)",loc:"Washington, DC",children:[]},
        {id:"23.2.6.N",name:"Logistics Integrated & Information Solutions — Marine Corps (PMW-230)",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.7.B",name:"PEO Aircraft Carriers (PEO-CV)",loc:"Washington, DC",children:[
        {id:"23.2.7.C",name:"In-Service Carrier Programs (PMS-312)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.D",name:"CVN-78 Program Office (PMS-378)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.E",name:"PCU John F. Kennedy Program Office (PMS-379)",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.7.F",name:"PEO Ships (PEO-SHIPS)",loc:"Washington, DC",children:[
        {id:"23.2.7.H",name:"DDG-1000 Program Office (PMS-500)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.I",name:"Destroyer Ship Building (PMS-400D)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.J",name:"Guided Missile Destroyer DDG(X) Program (PMS-460)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.L",name:"LPD-17 / LSD(X) Program Office",loc:"Washington, DC",children:[]},
        {id:"23.2.7.M",name:"Amphibious Warfare Program (PMS-377)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.N",name:"Support Ships, Boats and Craft Program Office (PMS-325)",loc:"Washington, DC",children:[]},
        {id:"23.2.7.O",name:"Strategic & Theater Sealift (PMS-385)",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.8",name:"PEO Integrated Warfare Systems (PEO-IWS)",loc:"Washington, DC",children:[
        {id:"23.2.8.C",name:"Command & Control (IWS 6.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.D",name:"Terminal Defense Systems (IWS 11.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.E",name:"Aegis Technical Representative (IWS 1.0F)",loc:"Dahlgren, VA",children:[]},
        {id:"23.2.8.F",name:"Above Water Sensors (IWS 2.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.G",name:"Surface Electronic Warfare Improvement Program (SEWIP)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.H",name:"Integrated Combat Systems (IWS X)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.I",name:"International & Foreign Military Sales (IWS 4.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.J",name:"DDG-1000 / Zumwalt (IWS 9.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.K",name:"Atalanta Combat Systems (IWS 80.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.B",name:"Surface Ship Weapons (IWS 3.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.C",name:"Standard Missile (IWS 3A)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.D",name:"RIM-116 Rolling Airframe Missile / CIWS Program (IWS 3B)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.E",name:"Naval Gunnery Project Office (IWS 3C)",loc:"Dahlgren, VA",children:[]},
        {id:"23.2.8.1.F",name:"Undersea Systems (IWS 5.0)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.G",name:"Advanced Development for ASW (IWS 5A)",loc:"Washington, DC",children:[]},
        {id:"23.2.8.1.H",name:"NATO Seasparrow",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.2.9",name:"Program Executive Office (Chart 23.2.9)",children:[]},

      {id:"23.2.10",name:"PEO C4I",loc:"San Diego, CA",children:[
        {id:"23.2.10.B",name:"Battlespace Awareness & Information Operations (PMW-120)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.C",name:"Shore & Expeditionary Integration Program Office (PMW-790)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.D",name:"Tactical Networks Program Office (PMW-160)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.F",name:"Undersea Communications & Integration Program Office (PMW-770)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.G",name:"Cybersecurity Program Office (PMW-130)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.H",name:"International C4I Integration Program Office (PMW-740)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.I",name:"Command & Control Program Office (PMW-150)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.J",name:"Carrier & Air Integration Program Office (PMW-750)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.K",name:"Ship Integration Program Office (PMW-760)",loc:"San Diego, CA",children:[]},
        {id:"23.2.10.L",name:"Navy Communications & GPS Navigation Program (PMW/A-170)",loc:"San Diego, CA",children:[]}
      ]},

      {id:"23.2.11",name:"PEO Aviation Common Systems & Commercial Services (PEO-CS)",loc:"Patuxent River, MD",children:[
        {id:"23.2.11.B",name:"Aircrew Systems Program Office (PMA-202)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.11.C",name:"Naval Aviation Training Systems & Ranges Program (PMA-205)",loc:"Orlando, FL",children:[]},
        {id:"23.2.11.D",name:"Air Combat Electronics Program Office (PMA-209)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.11.E",name:"Specialized & Proven Aircraft Program Office (PMA-226)",loc:"Patuxent River, MD",children:[]},
        {id:"23.2.11.F",name:"Common Aviation Support Equipment Program Office (PMA-260)",loc:"Patuxent River, MD",children:[]}
      ]}

    ]},

    // ── SYSTEMS COMMANDS (SYSCOMs) ────────────────────────────────────
    {id:"usn.sys",name:"Systems Commands (SYSCOMs)",children:[

      {id:"23.3.6.1",name:"Naval Air Systems Command (NAVAIR)",loc:"Patuxent River, MD",children:[
        {id:"23.3.6.1.3",name:"Naval Air Warfare Center Aircraft Division (NAWCAD)",loc:"Patuxent River, MD",children:[]},
        {id:"23.3.6.1.4",name:"Naval Air Warfare Center Weapons Division (NAWCWD)",loc:"China Lake, CA",children:[
          {id:"23.3.6.1.4.1",name:"Research & Engineering Group",loc:"China Lake, CA",children:[]},
          {id:"23.3.6.1.4.2",name:"Test & Evaluation Group",loc:"China Lake, CA",children:[]}
        ]}
      ]},

      {id:"23.3.6.2",name:"Naval Information Warfare Systems Command (NAVWAR)",loc:"San Diego, CA",children:[
        {id:"23.3.6.2.1",name:"NAVWAR Centers — Part 1",loc:"San Diego, CA",children:[]},
        {id:"23.3.6.2.2",name:"NAVWAR Centers — Part 2",loc:"San Diego, CA",children:[]}
      ]},

      {id:"23.3.6.3",name:"Naval Facilities Engineering Systems Command (NAVFAC)",loc:"Washington, DC",children:[]},

      {id:"23.3.6.4",name:"Naval Sea Systems Command (NAVSEA)",loc:"Washington, DC",children:[
        {id:"23.3.6.4.1",name:"Undersea Warfare (SEA-17)",loc:"Washington, DC",children:[]},
        {id:"23.3.6.4.2",name:"Naval Systems Engineering Directorate",loc:"Washington, DC",children:[]},
        {id:"23.3.6.4.4",name:"Naval Undersea Warfare Center (NUWC)",loc:"Newport, RI",children:[
          {id:"23.3.6.4.4.1",name:"Newport Division Directorates",loc:"Newport, RI",children:[]}
        ]},
        {id:"23.3.6.4.5",name:"Naval Surface Warfare Center (NSWC)",children:[
          {id:"23.3.6.4.5.1",name:"Carderock Division",loc:"West Bethesda, MD",children:[]},
          {id:"23.3.6.4.5.2",name:"Panama City Division",loc:"Panama City, FL",children:[]},
          {id:"23.3.6.4.5.3",name:"Crane Division",loc:"Crane, IN",children:[
            {id:"23.3.6.4.5.3.1",name:"Global Deterrence & Defense Department",loc:"Crane, IN",children:[
              {id:"23.3.6.4.5.3.1.B",name:"Expeditionary Warfare Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.1.C",name:"Flight Systems Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.1.D",name:"Energy, Power & Interconnect Technologies Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.1.E",name:"Fleet Maintenance & Engineering Division",loc:"Crane, IN",children:[]}
            ]},
            {id:"23.3.6.4.5.3.2",name:"Spectrum Warfare Systems Department",loc:"Crane, IN",children:[
              {id:"23.3.6.4.5.3.2.B",name:"IR/RF Systems Technologies Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.2.C",name:"Maritime Electronic Warfare Systems Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.2.D",name:"Expeditionary Electronic Warfare Systems Division",loc:"Crane, IN",children:[]}
            ]},
            {id:"23.3.6.4.5.3.4",name:"Special Warfare & Expeditionary Systems Department",loc:"Crane, IN",children:[
              {id:"23.3.6.4.5.3.4.B",name:"Small Arms Weapons Systems Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.4.C",name:"Specialized Munitions Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.4.D",name:"Maneuver & Engagement Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.4.E",name:"Expeditionary Systems Evaluation Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.4.F",name:"Surveillance & Reconnaissance Division",loc:"Crane, IN",children:[]},
              {id:"23.3.6.4.5.3.4.G",name:"Electro-Optic Technology Division",loc:"Crane, IN",children:[]}
            ]}
          ]},
          {id:"23.3.6.4.5.4",name:"Dahlgren Division",loc:"Dahlgren, VA",children:[
            {id:"23.3.6.4.5.4.C",name:"Strategic & Weapon Control Systems Department",loc:"Dahlgren, VA",children:[]},
            {id:"23.3.6.4.5.4.D",name:"Engagement Systems Department",loc:"Dahlgren, VA",children:[]},
            {id:"23.3.6.4.5.4.E",name:"Gun & Electric Weapon Systems",loc:"Dahlgren, VA",children:[]},
            {id:"23.3.6.4.5.4.F",name:"Asymmetric Strike Group",loc:"Dahlgren, VA",children:[]},
            {id:"23.3.6.4.5.4.G",name:"Electromagnetic & Sensor Systems Department",loc:"Dahlgren, VA",children:[]}
          ]}
        ]},
        {id:"23.3.6.4.6",name:"NAVSEA 21 Surface Warfare (SEA21)",loc:"Washington, DC",children:[
          {id:"23.3.6.4.6.C",name:"Surface Ship Modernization and Sustainment Program Office",loc:"Washington, DC",children:[]},
          {id:"23.3.6.4.6.E",name:"Littoral Combat Ship (LCS) Fleet Introduction & Sustainment",loc:"Washington, DC",children:[]}
        ]}
      ]},

      {id:"23.3.6.5",name:"Naval Supply Systems Command (NAVSUP)",loc:"Mechanicsburg, PA",children:[
        {id:"23.3.6.5.1",name:"NAVSUP Weapon Systems Support",loc:"Mechanicsburg, PA",children:[
          {id:"23.3.6.5.1.D",name:"Information Services Directorate",loc:"Mechanicsburg, PA",children:[]},
          {id:"23.3.6.5.1.F",name:"Ships & Submarines",loc:"Mechanicsburg, PA",children:[]},
          {id:"23.3.6.5.1.G",name:"Nuclear Reactors Supply Chain Management Directorate",loc:"Mechanicsburg, PA",children:[]}
        ]},
        {id:"23.3.6.5.2",name:"NAVSUP Fleet Logistics Centers",children:[
          {id:"23.3.6.5.2.B",name:"Fleet Logistics Center Jacksonville",loc:"Jacksonville, FL",children:[]},
          {id:"23.3.6.5.2.C",name:"Fleet Logistics Center Norfolk",loc:"Norfolk, VA",children:[]},
          {id:"23.3.6.5.2.D",name:"Fleet Logistics Center Yokosuka",loc:"Yokosuka, Japan",children:[]},
          {id:"23.3.6.5.2.E",name:"Fleet Logistics Center Puget Sound",loc:"Bremerton, WA",children:[]},
          {id:"23.3.6.5.2.F",name:"Fleet Logistics Center San Diego",loc:"San Diego, CA",children:[]},
          {id:"23.3.6.5.2.H",name:"Fleet Logistics Center Pearl Harbor",loc:"Pearl Harbor, HI",children:[]},
          {id:"23.3.6.5.2.I",name:"Fleet Logistics Center Sigonella",loc:"Sigonella, Italy",children:[]}
        ]},
        {id:"23.3.6.5.H",name:"NAVSUP Business Systems Center",loc:"Mechanicsburg, PA",children:[]}
      ]}

    ]},

    // ── FLEET & FUNCTIONAL COMMANDS ───────────────────────────────────
    {id:"usn.cmd",name:"Fleet & Functional Commands",children:[

      {id:"23.3.6.6",name:"US Fleet Cyber Command / US Tenth Fleet",loc:"Fort Meade, MD",children:[
        {id:"23.3.6.6.B",name:"Naval Network Warfare Command",loc:"Fort Meade, MD",children:[]},
        {id:"23.3.6.6.C",name:"Naval Information Warfare Training Group Norfolk",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.6.D",name:"Naval Information Warfare Training Group Gulfport",loc:"Gulfport, MS",children:[]},
        {id:"23.3.6.6.E",name:"Navy Cyber Defense Operations Command",loc:"Fort Meade, MD",children:[]},
        {id:"23.3.6.6.F",name:"Naval Computer & Telecommunications Area Master Station Atlantic",loc:"Chesapeake, VA",children:[]},
        {id:"23.3.6.6.G",name:"Naval Computer & Telecommunications Area Master Station Pacific",loc:"Wahiawa, HI",children:[]},
        {id:"23.3.6.6.H",name:"Naval Computer & Telecommunications Station San Diego",loc:"San Diego, CA",children:[]},
        {id:"23.3.6.6.J",name:"Forces Surveillance Support Center",loc:"Chesapeake, VA",children:[]}
      ]},

      {id:"23.3.3",name:"Commander Navy Installations Command (CNIC)",loc:"Washington, DC",children:[
        {id:"23.3.3.B",name:"Navy Region Mid-Atlantic",loc:"Norfolk, VA",children:[]},
        {id:"23.3.3.D",name:"Naval District Washington",loc:"Washington, DC",children:[]},
        {id:"23.3.3.E",name:"Navy Region Southeast",loc:"Jacksonville, FL",children:[]},
        {id:"23.3.3.F",name:"Navy Region Southwest",loc:"San Diego, CA",children:[]},
        {id:"23.3.3.G",name:"Navy Region Northwest",loc:"Bremerton, WA",children:[]},
        {id:"23.3.3.H",name:"Navy Region Hawaii",loc:"Pearl Harbor, HI",children:[]}
      ]},

      {id:"23.3.5",name:"Surgeon General / Bureau of Medicine & Surgery (BUMED)",loc:"Falls Church, VA",children:[
        {id:"23.3.5.C",name:"Naval Medical Forces Atlantic",loc:"Portsmouth, VA",children:[]},
        {id:"23.3.5.D",name:"Naval Medical Forces Pacific",loc:"San Diego, CA",children:[]},
        {id:"23.3.5.E",name:"Navy & Marine Corps Public Health Center",loc:"Portsmouth, VA",children:[]},
        {id:"23.3.5.F",name:"Naval Medical Forces Support Command",loc:"Jacksonville, FL",children:[]},
        {id:"23.3.5.G",name:"Navy Medical Leader & Professional Development Command",loc:"Bethesda, MD",children:[]},
        {id:"23.3.5.H",name:"Navy Medicine Operational Training Command",loc:"Pensacola, FL",children:[]},
        {id:"23.3.5.I",name:"Naval Medical Research Command",loc:"Silver Spring, MD",children:[]},
        {id:"23.3.5.J",name:"Naval Medical Readiness Logistics Command",loc:"Williamsburg, VA",children:[]},
        {id:"23.3.5.K",name:"Naval Medical Logistics Command",loc:"Fort Detrick, MD",children:[]}
      ]},

      {id:"23.3.4",name:"Warfighting Requirements & Capabilities (N9)",loc:"Washington, DC",children:[
        {id:"23.3.4.1",name:"Undersea Warfare Division",loc:"Washington, DC",children:[]},
        {id:"23.3.4.2",name:"Air Warfare Division",loc:"Washington, DC",children:[]},
        {id:"23.3.4.3",name:"Expeditionary Warfare Division",loc:"Washington, DC",children:[]},
        {id:"23.3.4.4",name:"Surface Warfare Division",loc:"Washington, DC",children:[]}
      ]},

      {id:"23.3.6.8",name:"Commander US Fleet Forces Command (COMFLTFORCOM)",loc:"Norfolk, VA",children:[
        {id:"23.3.6.8.H",name:"Commander Naval Information Forces",loc:"Suffolk, VA",children:[]},
        {id:"23.3.6.8.I",name:"Fleet Maritime Operations Center",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.J",name:"Intelligence & IW",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.L",name:"Fleet Capabilities and Force Development",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.1",name:"Naval Meteorology & Oceanography Command (NAVMETOCCOM)",loc:"Stennis Space Center, MS",children:[
          {id:"23.3.6.8.1.B",name:"Naval Oceanographic Office",loc:"Stennis Space Center, MS",children:[]},
          {id:"23.3.6.8.1.C",name:"Fleet Numerical Meteorology & Oceanography Center",loc:"Monterey, CA",children:[]},
          {id:"23.3.6.8.1.D",name:"Naval Oceanography Operations Command",loc:"Stennis Space Center, MS",children:[]},
          {id:"23.3.6.8.1.E",name:"National Ice Center",loc:"Suitland, MD",children:[]},
          {id:"23.3.6.8.1.F",name:"Naval Oceanography Mine Warfare Center",loc:"Stennis Space Center, MS",children:[]},
          {id:"23.3.6.8.1.G",name:"US Naval Observatory",loc:"Washington, DC",children:[]}
        ]},
        {id:"23.3.6.8.2.B",name:"Navy Warfare Development Command",loc:"Newport, RI",children:[]},
        {id:"23.3.6.8.2.D",name:"Commander Naval Air Force Atlantic",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.2.E",name:"Military Sealift Command",loc:"Washington, DC",children:[]},
        {id:"23.3.6.8.2.G",name:"Commander Submarine Forces / Submarine Forces Atlantic",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.2.I",name:"Commander US Naval Forces Southern Command / US 4th Fleet",loc:"Mayport, FL",children:[]},
        {id:"23.3.6.8.2.J",name:"Commander Naval Surface Force Atlantic",loc:"Norfolk, VA",children:[]},
        {id:"23.3.6.8.2.K",name:"Navy Expeditionary Combat Command",loc:"Virginia Beach, VA",children:[]}
      ]},

      {id:"23.3.6.9",name:"Commander US Pacific Fleet",loc:"Pearl Harbor, HI",children:[
        {id:"23.3.6.9.B",name:"Communications & Information Systems Directorate",loc:"Pearl Harbor, HI",children:[]},
        {id:"23.3.6.9.D",name:"Commander US Third Fleet",loc:"San Diego, CA",children:[]},
        {id:"23.3.6.9.E",name:"Commander, Submarine Force US Pacific Fleet",loc:"Pearl Harbor, HI",children:[]},
        {id:"23.3.6.9.G",name:"Intelligence & Information Operations Directorate",loc:"Pearl Harbor, HI",children:[]},
        {id:"23.3.6.9.I",name:"Commander, Naval Surface Force Pacific",loc:"San Diego, CA",children:[]},
        {id:"23.3.6.9.K",name:"Operations Directorate",loc:"Pearl Harbor, HI",children:[]},
        {id:"23.3.6.9.L",name:"Commander US Seventh Fleet",loc:"Yokosuka, Japan",children:[]},
        {id:"23.3.6.9.M",name:"Commander, Naval Air Forces / Naval Air Force Pacific",loc:"San Diego, CA",children:[
          {id:"23.3.6.9.N",name:"Naval Aviation Warfighting Development Center",loc:"Fallon, NV",children:[]}
        ]},
        {id:"23.3.6.9.P",name:"Pacific Missile Range Facility Barking Sands",loc:"Kekaha, HI",children:[]}
      ]}

    ]}
  ]},

  // ══════════════════════════════════════════════════════════════════════
  //  USMC
  // ══════════════════════════════════════════════════════════════════════
  {id:"usmc",name:"USMC",children:[

    {id:"23.4.1",name:"Marine Corps Systems Command (MCSC)",loc:"Quantico, VA",children:[
      {id:"23.4.1.C",name:"Combat Support Systems",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.D",name:"Systems Engineering & Acquisition Logistics",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.E",name:"Marine Corps Tactical Systems Support Activity (MCTSSA)",loc:"Camp Pendleton, CA",children:[]},
      {id:"23.4.1.G",name:"Technology Transition Office",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.H",name:"Marine Corps Information Operations Center",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.I",name:"Operations and Programs",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.J",name:"Ground Combat Element Systems",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.M",name:"Program Executive Office Land Systems",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.1.C",name:"Enterprise Software Licensing",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.1.D",name:"Global Combat Support Systems — Marine Corps",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.1.E",name:"MAGTF Command Element Systems",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.1.F",name:"Program Manager Training Systems",loc:"Quantico, VA",children:[]},
      {id:"23.4.1.1.J",name:"Information Systems & Infrastructure",loc:"Quantico, VA",children:[]}
    ]},

    {id:"23.4.E",name:"USMC Aviation",loc:"Quantico, VA",children:[]},
    {id:"23.4.2.B",name:"C4 Dept / Chief Information Officer",loc:"Quantico, VA",children:[]},
    {id:"23.4.2.D",name:"US Marine Corps Forces Special Operations Command (MARSOC)",loc:"Camp Lejeune, NC",children:[]},
    {id:"23.4.2.F",name:"Intelligence Department",loc:"Quantico, VA",children:[]},

    {id:"23.4.2.I",name:"Plans, Policies & Operations",loc:"Quantico, VA",children:[
      {id:"23.4.2.J",name:"Operations Division",loc:"Quantico, VA",children:[]},
      {id:"23.4.2.K",name:"Strategy & Plans Division",loc:"Quantico, VA",children:[]}
    ]},

    {id:"23.4.2.L",name:"Programs & Resources Department",loc:"Quantico, VA",children:[]},

    {id:"23.4.3",name:"Marine Corps Combat Development & Integration Command",loc:"Quantico, VA",children:[
      {id:"23.4.3.F",name:"Capabilities Development Directorate",loc:"Quantico, VA",children:[]},
      {id:"23.4.3.I",name:"Marine Corps Warfighting Laboratory / Futures Directorate",loc:"Quantico, VA",children:[]}
    ]}

  ]}

]};

// ─── STATE ────────────────────────────────────────────────────────────────────
const nodeData  = {}; // nodeId → {ae, target, engagement, notes}
const collapsed = {};
const PEOPLE = {
  Sinclair:     {cls:'color-sinclair',     hex:'#6e2800'},
  Coronella:    {cls:'color-coronella',    hex:'#5c0a2e'},
  Higgins:      {cls:'color-higgins',      hex:'#0a4a23'},
  Ash:          {cls:'color-ash',          hex:'#003d3d'},
  Disqualified: {cls:'color-disqualified', hex:'#7b1414'},
  Unassigned:   {cls:'color-unassigned',   hex:'#4a1a6e'},
  Disputed:     {cls:'color-disputed',     hex:'#4a3c00'},
  Whitespace:   {cls:'color-whitespace',   hex:'#d5d8dc'}
};

let popupNodeId = null;
let filterPerson = null;

// ─── PERSISTENCE ─────────────────────────────────────────────────────────────
function saveAssignments() {
  try { localStorage.setItem('navyOrgData', JSON.stringify({ _tree: ORG, ...nodeData })); } catch(e) {}
  writeToFile(); // no-op if not connected
}

function loadAssignments() {
  try {
    const saved = localStorage.getItem('navyOrgData') || localStorage.getItem('navyOrgAssignments');
    if (!saved) return;
    const raw = JSON.parse(saved);
    if (raw._tree) {
      ORG = raw._tree;
      const { _tree, ...data } = raw;
      Object.entries(data).forEach(([k, v]) => { nodeData[k] = (typeof v === 'string') ? {ae: v} : v; });
    } else {
      Object.entries(raw).forEach(([k, v]) => { nodeData[k] = (typeof v === 'string') ? {ae: v} : v; });
    }
  } catch(e) {}
}

// Merge incoming data — only sets fields that have values, never clears existing
function mergeNodeData(incoming) {
  if (!incoming || typeof incoming !== 'object') return;
  Object.entries(incoming).forEach(([id, fields]) => {
    if (id === '_tree') return; // structural data handled separately
    if (typeof fields === 'string') fields = {ae: fields};
    if (!fields || typeof fields !== 'object') return;
    const hasData = Object.values(fields).some(v => v);
    if (!hasData) return;
    if (!nodeData[id]) nodeData[id] = {};
    Object.entries(fields).forEach(([f, v]) => { if (v) nodeData[id][f] = v; });
  });
}

// ─── FILE SYSTEM ACCESS ───────────────────────────────────────────────────────
let fileHandle = null;

// IndexedDB helpers for persisting the file handle across page reloads
function openIDB() {
  return new Promise((res, rej) => {
    const req = indexedDB.open('navyOrgDB', 1);
    req.onupgradeneeded = e => e.target.result.createObjectStore('handles');
    req.onsuccess = e => res(e.target.result);
    req.onerror = rej;
  });
}
async function saveHandleIDB(handle) {
  const db = await openIDB();
  return new Promise((res, rej) => {
    const tx = db.transaction('handles', 'readwrite');
    tx.objectStore('handles').put(handle, 'main');
    tx.oncomplete = res; tx.onerror = rej;
  });
}
async function getHandleIDB() {
  const db = await openIDB();
  return new Promise(res => {
    const tx = db.transaction('handles', 'readonly');
    const req = tx.objectStore('handles').get('main');
    req.onsuccess = () => res(req.result || null);
    req.onerror = () => res(null);
  });
}

function setFileStatus(msg, state) { // state: 'ok' | 'err' | ''
  const el = document.getElementById('fileStatus');
  if (!el) return;
  el.textContent = msg;
  el.className = state || '';
}

async function writeToFile() {
  if (!fileHandle) return;
  try {
    const w = await fileHandle.createWritable();
    await w.write(JSON.stringify({ _tree: ORG, ...nodeData }, null, 2));
    await w.close();
  } catch(e) {
    setFileStatus('Auto-save error — reconnect', 'err');
    fileHandle = null;
    const btn = document.getElementById('btnLoadFile');
    if (btn) btn.style.display = 'none';
  }
}

async function readFromHandle(handle) {
  const file = await handle.getFile();
  const text = (await file.text()).trim();
  if (!text) return;
  const raw = JSON.parse(text);
  if (raw._tree) {
    ORG = raw._tree;
    rebuildNodeIndex();
    const { _tree, ...data } = raw;
    mergeNodeData(data);
  } else {
    mergeNodeData(raw);
  }
  try { localStorage.setItem('navyOrgData', JSON.stringify({ _tree: ORG, ...nodeData })); } catch(e) {}
}

async function activateHandle(handle) {
  fileHandle = handle;
  await saveHandleIDB(handle);
  setFileStatus('✓ ' + handle.name, 'ok');
  const btn = document.getElementById('btnLoadFile');
  if (btn) btn.style.display = '';
}

// Open an existing shared JSON file
async function connectFile() {
  if (!('showOpenFilePicker' in window)) {
    alert('File System Access API not supported in this browser. Use Chrome or Edge.');
    return;
  }
  try {
    const [handle] = await window.showOpenFilePicker({
      types: [{description: 'JSON data file', accept: {'application/json': ['.json']}}],
      multiple: false
    });
    await activateHandle(handle);
    await readFromHandle(handle);
    await writeToFile(); // write merged state back
    render();
  } catch(e) {
    if (e.name !== 'AbortError') setFileStatus('Connect failed', 'err');
  }
}

// Create a new shared JSON file
async function newSharedFile() {
  if (!('showSaveFilePicker' in window)) {
    alert('File System Access API not supported in this browser. Use Chrome or Edge.');
    return;
  }
  try {
    const handle = await window.showSaveFilePicker({
      suggestedName: 'navy_org_data.json',
      types: [{description: 'JSON data file', accept: {'application/json': ['.json']}}]
    });
    await activateHandle(handle);
    await writeToFile();
  } catch(e) {
    if (e.name !== 'AbortError') setFileStatus('Create failed', 'err');
  }
}

// Reload data from the connected file (manual refresh)
async function loadFromFile() {
  if (!fileHandle) { setFileStatus('No file connected', 'err'); return; }
  try {
    await readFromHandle(fileHandle);
    await writeToFile();
    render();
    setFileStatus('✓ ' + fileHandle.name + ' — loaded', 'ok');
  } catch(e) {
    setFileStatus('Load error — reconnect', 'err');
  }
}

// On page load: restore file handle from IndexedDB and re-request permission
async function tryRestoreFile() {
  if (!('showOpenFilePicker' in window)) {
    const sec = document.getElementById('fileSection');
    if (sec) sec.style.display = 'none';
    return;
  }
  try {
    const handle = await getHandleIDB();
    if (!handle) return;
    const perm = await handle.queryPermission({mode: 'readwrite'});
    if (perm === 'granted') {
      await activateHandle(handle);
      await readFromHandle(handle);
      render();
    } else {
      setFileStatus('File disconnected — reconnect', 'err');
    }
  } catch(e) { /* silent */ }
}

// ─── RENDER ───────────────────────────────────────────────────────────────────
function getEffectiveColor(nodeId, inheritedPerson) {
  return (nodeData[nodeId] && nodeData[nodeId].ae) || inheritedPerson || null;
}

function buildNodeEl(node, depth, inheritedPerson, isLast) {
  const person     = getEffectiveColor(node.id, inheritedPerson);
  const hasChildren = node.children && node.children.length > 0;
  const isCollapsed = collapsed[node.id];

  const wrapper = document.createElement('div');
  wrapper.className = 'tree-node';
  wrapper.dataset.id   = node.id;
  wrapper.dataset.name = node.name.toLowerCase();
  if (node.loc) wrapper.dataset.loc = node.loc.toLowerCase();

  if (depth === 1) wrapper.classList.add('branch-node');

  const row = document.createElement('div');
  row.className = 'node-row';

  // Indentation
  const indent = document.createElement('div');
  indent.className = 'node-indent';
  for (let i = 0; i < depth; i++) {
    const u = document.createElement('div');
    u.className = 'indent-unit' +
      (i === depth - 1 ? ' connector' : '') +
      (isLast && i === depth - 1 ? ' last' : '');
    indent.appendChild(u);
  }
  row.appendChild(indent);

  // Toggle
  if (hasChildren) {
    const tog = document.createElement('button');
    tog.className = 'toggle';
    tog.textContent = isCollapsed ? '▶' : '▼';
    tog.onclick = (e) => { e.stopPropagation(); toggleNode(node.id); };
    row.appendChild(tog);
  } else {
    const sp = document.createElement('div');
    sp.className = 'toggle-spacer';
    row.appendChild(sp);
  }

  // Label
  const label = document.createElement('div');
  label.className = 'node-label';
  if (person) label.classList.add(PEOPLE[person].cls);
  if (depth === 0) wrapper.classList.add('root-node');

  const txt = document.createElement('span');
  txt.className = 'node-text';
  txt.textContent = node.name;
  label.appendChild(txt);

  if (node.loc) {
    const locSpan = document.createElement('span');
    locSpan.className = 'node-loc';
    locSpan.textContent = node.loc;
    label.appendChild(locSpan);
  }

  const idSpan = document.createElement('span');
  idSpan.className = 'node-id';
  idSpan.textContent = node.id.startsWith('23.') ? node.id : '';
  label.appendChild(idSpan);

  if (person) {
    const asgn = document.createElement('span');
    asgn.className = 'node-assigned';
    asgn.textContent = person[0];
    label.appendChild(asgn);
  }

  // Badges: Target, Engagement, Notes
  const nd = nodeData[node.id] || {};
  if (nd.target || nd.engagement || nd.notes) {
    const badges = document.createElement('div');
    badges.className = 'node-badges';
    if (nd.target) {
      const b = document.createElement('span');
      const tmap = {Yes:'badge-t-yes', No:'badge-t-no', Undecided:'badge-t-undecided'};
      b.className = 'badge ' + (tmap[nd.target] || '');
      b.textContent = nd.target === 'Undecided' ? 'T?' : 'T:' + nd.target[0];
      badges.appendChild(b);
    }
    if (nd.engagement) {
      const b = document.createElement('span');
      const emap = {Customer:'badge-eng-cust', Engaged:'badge-eng-engd', 'Not Engaged':'badge-eng-neng', "Doesn't Buy":'badge-eng-dbuy'};
      const elab = {Customer:'Cust', Engaged:'Engd', 'Not Engaged':'!Eng', "Doesn't Buy":'!Buy'};
      b.className = 'badge ' + (emap[nd.engagement] || '');
      b.textContent = elab[nd.engagement] || nd.engagement;
      badges.appendChild(b);
    }
    if (nd.notes) {
      const b = document.createElement('span');
      b.className = 'badge badge-notes';
      b.textContent = '✎';
      b.title = nd.notes;
      badges.appendChild(b);
    }
    label.appendChild(badges);
  }

  label.onclick = (e) => {
    e.stopPropagation();
    if (editMode) return; // suppress popup/assign in edit mode
    if (filterPerson) {
      if (!nodeData[node.id]) nodeData[node.id] = {};
      nodeData[node.id].ae = filterPerson;
      saveAssignments();
      render();
    } else {
      openPopup(node.id, node.name, e);
    }
  };
  row.appendChild(label);

  // Edit mode buttons (shown only when edit mode is active)
  const editBtns = document.createElement('div');
  editBtns.className = 'node-edit-btns';
  const addBtn = document.createElement('button');
  addBtn.className = 'node-edit-btn btn-eadd'; addBtn.textContent = '+'; addBtn.title = 'Add child';
  addBtn.onclick = (e) => { e.stopPropagation(); structAdd(node.id); };
  editBtns.appendChild(addBtn);
  const renBtn = document.createElement('button');
  renBtn.className = 'node-edit-btn btn-eren'; renBtn.textContent = '✎'; renBtn.title = 'Rename';
  renBtn.onclick = (e) => { e.stopPropagation(); structRename(node.id); };
  editBtns.appendChild(renBtn);
  if (node.id !== 'root') {
    const delBtn = document.createElement('button');
    delBtn.className = 'node-edit-btn btn-edel'; delBtn.textContent = '✕'; delBtn.title = 'Delete';
    delBtn.onclick = (e) => { e.stopPropagation(); structDelete(node.id); };
    editBtns.appendChild(delBtn);
  }
  row.appendChild(editBtns);

  // Drag source (all nodes except root)
  if (node.id !== 'root') {
    row.draggable = editMode;
    row.ondragstart = (e) => {
      if (!editMode) { e.preventDefault(); return; }
      dragSrcId = node.id;
      wrapper.classList.add('drag-src');
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('text/plain', node.id);
    };
    row.ondragend = () => {
      wrapper.classList.remove('drag-src');
      clearDropClasses();
      dragSrcId = null; dropTargetId = null; dropPos = null;
    };
  }

  // Drop target (all nodes)
  row.ondragover = (e) => {
    if (!editMode || !dragSrcId || dragSrcId === node.id) return;
    if (isInSubtree(node.id, dragSrcId)) return; // can't drop inside self
    e.preventDefault();
    e.dataTransfer.dropEffect = 'move';
    clearDropClasses();
    const rect = row.getBoundingClientRect();
    const y = e.clientY - rect.top;
    const h = rect.height;
    dropPos      = y < h * 0.3 ? 'before' : y > h * 0.7 ? 'after' : 'child';
    dropTargetId = node.id;
    wrapper.classList.add('drop-' + dropPos);
  };
  row.ondragleave = (e) => {
    if (!wrapper.contains(e.relatedTarget)) {
      wrapper.classList.remove('drop-before','drop-child','drop-after');
      if (dropTargetId === node.id) { dropTargetId = null; dropPos = null; }
    }
  };
  row.ondrop = (e) => {
    e.preventDefault();
    const src = dragSrcId, tgt = dropTargetId, pos = dropPos;
    clearDropClasses();
    dragSrcId = null; dropTargetId = null; dropPos = null;
    if (src && tgt && src !== tgt && !isInSubtree(tgt, src)) moveNode(src, tgt, pos);
  };

  wrapper.appendChild(row);

  // Children
  if (hasChildren) {
    const childWrap = document.createElement('div');
    childWrap.className = 'children-wrap';
    if (isCollapsed) childWrap.style.display = 'none';
    node.children.forEach((child, i) => {
      childWrap.appendChild(buildNodeEl(child, depth + 1, person, i === node.children.length - 1));
    });
    wrapper.appendChild(childWrap);
  }

  return wrapper;
}

function render() {
  const root = document.getElementById('tree-root');
  root.innerHTML = '';
  root.appendChild(buildNodeEl(ORG, 0, null, true));
  updateCounts();
}

// ─── TOGGLE ───────────────────────────────────────────────────────────────────
function toggleNode(id) {
  collapsed[id] = !collapsed[id];
  const el = document.querySelector(`.tree-node[data-id="${CSS.escape(id)}"]`);
  if (!el) return;
  const childWrap = el.querySelector(':scope > .children-wrap');
  const tog = el.querySelector(':scope > .node-row .toggle');
  if (childWrap) childWrap.style.display = collapsed[id] ? 'none' : '';
  if (tog) tog.textContent = collapsed[id] ? '▶' : '▼';
}

function expandAll()  { Object.keys(collapsed).forEach(k => delete collapsed[k]); render(); }
function collapseAll() {
  function mark(n) { if (n.children && n.children.length) { collapsed[n.id] = true; n.children.forEach(mark); } }
  mark(ORG);
  render();
}

// ─── POPUP ────────────────────────────────────────────────────────────────────
function openPopup(id, name, e) {
  popupNodeId = id;
  document.getElementById('popup-name').textContent = name;

  // Build AE buttons from PEOPLE
  const aeRow = document.getElementById('popup-ae-row');
  aeRow.innerHTML = '';
  Object.entries(PEOPLE).forEach(([p, info]) => {
    const btn = document.createElement('button');
    btn.className = 'popup-person';
    const isLight = info.hex === '#d5d8dc';
    btn.style.background = isLight ? '#aab7b8' : info.hex;
    if (isLight) btn.style.color = '#1a1a2e';
    btn.textContent = p;
    btn.dataset.person = p;
    btn.onclick = () => setPopupField('ae', p);
    aeRow.appendChild(btn);
  });

  refreshPopupState();

  const popup = document.getElementById('popup');
  popup.classList.add('visible');
  const x = Math.min(e.clientX + 12, window.innerWidth - 350);
  const y = Math.min(e.clientY + 12, window.innerHeight - 380);
  popup.style.left = x + 'px';
  popup.style.top  = y + 'px';
}

function refreshPopupState() {
  const nd = nodeData[popupNodeId] || {};

  // AE active state
  document.querySelectorAll('#popup-ae-row .popup-person').forEach(btn => {
    btn.classList.toggle('ae-active', btn.dataset.person === nd.ae);
  });

  // Target active state
  const tmap = {Yes:'tag-yes', No:'tag-no', Undecided:'tag-undecided'};
  document.querySelectorAll('.popup-tag[class*=tag-y],.popup-tag[class*=tag-n],.popup-tag[class*=tag-u]').forEach(b => b.classList.remove('tag-active'));
  document.querySelectorAll('.popup-tags .popup-tag').forEach(b => {
    if (b.classList.contains('tag-yes') || b.classList.contains('tag-no') || b.classList.contains('tag-undecided')) {
      b.classList.remove('tag-active');
    }
  });
  if (nd.target) {
    const sel = document.querySelector('.popup-tag.tag-' + nd.target.toLowerCase().replace(' ','-'));
    if (sel) sel.classList.add('tag-active');
  }

  // Engagement active state
  const engClasses = ['tag-customer','tag-engaged','tag-notengaged','tag-doesntbuy'];
  engClasses.forEach(c => document.querySelector('.popup-tag.' + c)?.classList.remove('tag-active'));
  if (nd.engagement) {
    const engMap = {Customer:'tag-customer', Engaged:'tag-engaged', 'Not Engaged':'tag-notengaged', "Doesn't Buy":'tag-doesntbuy'};
    const sel = document.querySelector('.popup-tag.' + engMap[nd.engagement]);
    if (sel) sel.classList.add('tag-active');
  }

  // Notes
  document.getElementById('popup-notes').value = nd.notes || '';
}

function closePopup() {
  // Save notes on close
  if (popupNodeId) {
    const val = document.getElementById('popup-notes').value.trim();
    if (!nodeData[popupNodeId]) nodeData[popupNodeId] = {};
    if (val) nodeData[popupNodeId].notes = val;
    else delete nodeData[popupNodeId].notes;
    pruneNodeData(popupNodeId);
    saveAssignments();
  }
  document.getElementById('popup').classList.remove('visible');
  popupNodeId = null;
  render();
}

document.addEventListener('click', (e) => {
  if (popupNodeId && !document.getElementById('popup').contains(e.target)) closePopup();
});

function setPopupField(field, value) {
  if (!popupNodeId) return;
  if (!nodeData[popupNodeId]) nodeData[popupNodeId] = {};
  // Toggle: clicking the active value clears it
  if (nodeData[popupNodeId][field] === value) delete nodeData[popupNodeId][field];
  else nodeData[popupNodeId][field] = value;
  pruneNodeData(popupNodeId);
  saveAssignments();
  refreshPopupState();
  updateCounts();
  // Re-render just the one node label color (for AE changes)
  if (field === 'ae') render();
}

function pruneNodeData(id) {
  if (nodeData[id] && !Object.values(nodeData[id]).some(v => v)) delete nodeData[id];
}

function clearAll() {
  if (!popupNodeId) return;
  delete nodeData[popupNodeId];
  document.getElementById('popup-notes').value = '';
  saveAssignments();
  document.getElementById('popup').classList.remove('visible');
  popupNodeId = null;
  render();
}

function setFilter(person) {
  filterPerson = (filterPerson === person) ? null : person;
  document.querySelectorAll('.person-btn').forEach(b => b.style.outline = 'none');
  if (filterPerson) {
    const keys = Object.keys(PEOPLE);
    const idx  = keys.indexOf(filterPerson);
    const btns = document.querySelectorAll('.person-btn');
    if (idx >= 0 && btns[idx]) btns[idx].style.outline = '2px solid white';
  }
}

// ─── COUNTS ───────────────────────────────────────────────────────────────────
function countEffective(node, inherited, counts) {
  const person = (nodeData[node.id] && nodeData[node.id].ae) || inherited || null;
  if (person) counts[person] = (counts[person] || 0) + 1;
  else counts._none = (counts._none || 0) + 1;
  if (node.children) node.children.forEach(c => countEffective(c, person, counts));
}

function updateCounts() {
  const counts = {_none: 0};
  countEffective(ORG, null, counts);
  Object.keys(PEOPLE).forEach(p => {
    document.getElementById('count-' + p).textContent = counts[p] || 0;
  });
  document.getElementById('count-none').textContent = counts._none || 0;
}

// ─── SEARCH ───────────────────────────────────────────────────────────────────
function doSearch(q) {
  const term = q.trim().toLowerCase();
  const allNodes = document.querySelectorAll('.tree-node');
  let matchCount = 0;

  if (!term) {
    allNodes.forEach(n => n.classList.remove('search-match', 'search-hidden'));
    document.getElementById('searchCount').textContent = '';
    return;
  }

  // Mark matches (search name AND location)
  const matched = new Set();
  allNodes.forEach(n => {
    const name = n.dataset.name || '';
    const loc  = n.dataset.loc  || '';
    const id   = (n.dataset.id  || '').toLowerCase();
    if (name.includes(term) || id.includes(term) || loc.includes(term)) {
      matched.add(n.dataset.id);
      matchCount++;
    }
  });

  // Build ancestor set
  const ancestors = new Set();
  function markAncestors(node, path) {
    path = [...path, node.id];
    if (matched.has(node.id)) path.forEach(id => ancestors.add(id));
    if (node.children) node.children.forEach(c => markAncestors(c, path));
  }
  markAncestors(ORG, []);

  // Expand ancestors
  ancestors.forEach(aid => { if (collapsed[aid]) collapsed[aid] = false; });

  if (ancestors.size > 0) render();

  // Re-apply match highlighting and scroll to first match
  setTimeout(() => {
    let firstMatch = null;
    document.querySelectorAll('.tree-node').forEach(n => {
      const id = n.dataset.id;
      n.classList.remove('search-match', 'search-hidden');
      if (matched.has(id)) {
        n.classList.add('search-match');
        if (!firstMatch) firstMatch = n;
      } else if (!ancestors.has(id)) {
        n.classList.add('search-hidden');
      }
    });
    if (firstMatch) {
      firstMatch.scrollIntoView({behavior:'smooth', block:'center'});
    }
    document.getElementById('searchCount').textContent =
      matchCount + ' match' + (matchCount !== 1 ? 'es' : '');
  }, 20);
}

// ─── RESET ────────────────────────────────────────────────────────────────────
function resetAll() {
  if (!confirm('Clear all assignments, tags, and notes?')) return;
  Object.keys(nodeData).forEach(k => delete nodeData[k]);
  saveAssignments();
  render();
}

// ─── EXPORT / IMPORT ─────────────────────────────────────────────────────────
function parseDesignator(name) {
  const m = name.match(/\(([^)]+)\)\s*$/);
  if (m) return {facility: m[1], structure: name.replace(/\s*\([^)]+\)\s*$/, '').trim()};
  return {facility: '', structure: name};
}

function csvEsc(v) {
  v = String(v ?? '');
  return (v.includes(',') || v.includes('"') || v.includes('\n')) ? '"' + v.replace(/"/g,'""') + '"' : v;
}

function flattenForExport(node, rows) {
  if (node.id !== 'root') {
    const {facility, structure} = parseDesignator(node.name);
    const nd = nodeData[node.id] || {};
    rows.push([facility, structure, node.loc||'', nd.target||'', nd.engagement||'', nd.ae||'', nd.notes||'']);
  }
  if (node.children) node.children.forEach(c => flattenForExport(c, rows));
}

function exportAssignments() {
  const headers = ['Command/Facility','Structure','Location','Target','Engagement','AE','NOTES'];
  const rows = [];
  flattenForExport(ORG, rows);
  const csv = [headers.map(csvEsc).join(','), ...rows.map(r => r.map(csvEsc).join(','))].join('\r\n');
  const blob = new Blob([csv], {type: 'text/csv'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'navy_org_assignments.csv';
  a.click();
}

function parseCSVLine(line) {
  const out = []; let cur = ''; let inQ = false;
  for (let i = 0; i < line.length; i++) {
    const ch = line[i];
    if (ch === '"') { if (inQ && line[i+1]==='"') { cur+='"'; i++; } else inQ=!inQ; }
    else if (ch===',' && !inQ) { out.push(cur); cur=''; }
    else cur += ch;
  }
  out.push(cur);
  return out;
}

// ─── STRUCTURE EDITING ───────────────────────────────────────────────────────
let editMode = false;

function toggleEditMode() {
  editMode = !editMode;
  document.body.classList.toggle('edit-mode', editMode);
  const btn = document.getElementById('btnEditMode');
  if (btn) btn.textContent = editMode ? '✓ Exit Edit Mode' : '✎ Edit Structure';
  render(); // re-render so draggable attribute updates correctly
}

function findNodeAndParent(targetId, node, parent) {
  node = node || ORG;
  if (node.id === targetId) return { node, parent: parent || null };
  if (!node.children) return null;
  for (const c of node.children) {
    const r = findNodeAndParent(targetId, c, node);
    if (r) return r;
  }
  return null;
}

// ─── DRAG AND DROP ────────────────────────────────────────────────────────────
let dragSrcId    = null;
let dropTargetId = null;
let dropPos      = null; // 'before' | 'child' | 'after'

function clearDropClasses() {
  document.querySelectorAll('.drop-before,.drop-child,.drop-after')
    .forEach(el => el.classList.remove('drop-before','drop-child','drop-after'));
}

function isInSubtree(nodeId, subtreeRootId) {
  if (nodeId === subtreeRootId) return true;
  const root = nodeIndex[subtreeRootId];
  if (!root || !root.children) return false;
  return root.children.some(c => isInSubtree(nodeId, c.id));
}

function moveNode(srcId, targetId, position) {
  const srcResult = findNodeAndParent(srcId);
  if (!srcResult || !srcResult.parent) return;
  const { node: srcNode, parent: srcParent } = srcResult;
  srcParent.children = srcParent.children.filter(c => c.id !== srcId);

  if (position === 'child') {
    const targetNode = nodeIndex[targetId];
    if (!targetNode.children) targetNode.children = [];
    targetNode.children.push(srcNode);
    delete collapsed[targetId];
  } else {
    const tgtResult = findNodeAndParent(targetId);
    if (!tgtResult || !tgtResult.parent) {
      srcParent.children.push(srcNode); return; // fallback
    }
    const { parent: targetParent } = tgtResult;
    const idx = targetParent.children.findIndex(c => c.id === targetId);
    targetParent.children.splice(position === 'before' ? idx : idx + 1, 0, srcNode);
  }

  rebuildNodeIndex();
  saveAssignments();
  render();
}

function structDelete(id) {
  const result = findNodeAndParent(id);
  if (!result || !result.parent) return;
  const { node, parent } = result;
  const childCount = (function count(n) {
    return (n.children || []).reduce((s, c) => s + 1 + count(c), 0);
  })(node);
  const msg = childCount > 0
    ? `Delete "${node.name}" and its ${childCount} child node${childCount > 1 ? 's' : ''}?`
    : `Delete "${node.name}"?`;
  if (!confirm(msg)) return;
  parent.children = parent.children.filter(c => c.id !== id);
  // Remove nodeData for deleted subtree
  (function clean(n) {
    delete nodeData[n.id];
    (n.children || []).forEach(clean);
  })(node);
  rebuildNodeIndex();
  saveAssignments();
  render();
}

let structModalMode = null; // 'rename' | 'add'
let structModalTargetId = null;

function structRename(id) {
  const node = nodeIndex[id];
  if (!node) return;
  structModalMode = 'rename';
  structModalTargetId = id;
  document.getElementById('structModalTitle').textContent = 'Rename Node';
  document.getElementById('structName').value = node.name;
  document.getElementById('structLoc').value = node.loc || '';
  document.getElementById('structApplyBtn').textContent = 'Save';
  document.getElementById('structModal').classList.add('open');
  document.getElementById('structName').focus();
}

function structAdd(parentId) {
  structModalMode = 'add';
  structModalTargetId = parentId;
  const parent = nodeIndex[parentId];
  document.getElementById('structModalTitle').textContent = 'Add Child to: ' + (parent ? parent.name : parentId);
  document.getElementById('structName').value = '';
  document.getElementById('structLoc').value = '';
  document.getElementById('structApplyBtn').textContent = 'Add';
  document.getElementById('structModal').classList.add('open');
  document.getElementById('structName').focus();
}

function closeStructModal() {
  document.getElementById('structModal').classList.remove('open');
  structModalMode = null;
  structModalTargetId = null;
}

function applyStructModal() {
  const name = document.getElementById('structName').value.trim();
  if (!name) { document.getElementById('structName').focus(); return; }
  const loc  = document.getElementById('structLoc').value.trim();

  if (structModalMode === 'rename') {
    const node = nodeIndex[structModalTargetId];
    if (node) { node.name = name; if (loc) node.loc = loc; else delete node.loc; }
  } else if (structModalMode === 'add') {
    const parent = nodeIndex[structModalTargetId];
    if (!parent) return;
    if (!parent.children) parent.children = [];
    const newId = 'custom_' + Date.now();
    const newNode = { id: newId, name };
    if (loc) newNode.loc = loc;
    newNode.children = [];
    parent.children.push(newNode);
    // Expand parent so new child is visible
    delete collapsed[structModalTargetId];
  }

  closeStructModal();
  rebuildNodeIndex();
  saveAssignments();
  render();
}

// Allow Enter key to submit struct modal
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    if (document.getElementById('structModal').classList.contains('open')) closeStructModal();
    if (document.getElementById('conflictModal').style.display === 'block') cancelConflict();
  }
  if (e.key === 'Enter' && document.getElementById('structModal').classList.contains('open')) {
    applyStructModal();
  }
});

// ─── CONFLICT RESOLUTION ─────────────────────────────────────────────────────
let pendingConflicts = [];
let pendingIncoming  = {};

// Build a fast id→node index; called once at load and again after every tree change
const nodeIndex = {};
function rebuildNodeIndex() {
  Object.keys(nodeIndex).forEach(k => delete nodeIndex[k]);
  (function walk(node) {
    nodeIndex[node.id] = node;
    if (node.children) node.children.forEach(walk);
  })(ORG);
}
rebuildNodeIndex();

function applyImportWithConflicts(incoming) {
  const conflicts = [];
  Object.entries(incoming).forEach(([id, fields]) => {
    if (!nodeData[id]) return;
    Object.entries(fields).forEach(([field, value]) => {
      if (!value) return;
      const existing = nodeData[id][field];
      if (existing && existing !== value) {
        conflicts.push({ id, field, local: existing, imported: value, resolution: null });
      }
    });
  });

  if (conflicts.length === 0) {
    mergeNodeData(incoming);
    saveAssignments();
    render();
    return;
  }

  pendingConflicts = conflicts;
  pendingIncoming  = incoming;
  showConflictModal();
}

function showConflictModal() {
  const list = document.getElementById('conflictList');
  document.getElementById('conflictSub').textContent =
    `${pendingConflicts.length} conflict${pendingConflicts.length > 1 ? 's' : ''} found. Choose a resolution for each field.`;

  // Group by node id
  const byNode = {};
  pendingConflicts.forEach((c, idx) => {
    if (!byNode[c.id]) byNode[c.id] = [];
    byNode[c.id].push({ ...c, idx });
  });

  list.innerHTML = Object.entries(byNode).map(([id, items]) => {
    const name = (nodeIndex[id] && nodeIndex[id].name) || id;
    const fieldRows = items.map(({ field, local, imported, idx }) => `
      <div class="conflict-field" id="cf-row-${idx}">
        <span class="cf-field">${field}</span>
        <span class="cf-val">${local}</span>
        <span class="cf-arrow">→</span>
        <span class="cf-val" style="color:#85c1e9">${imported}</span>
        <div class="cf-btns">
          <button class="cf-btn" id="cf-local-${idx}"    onclick="resolveField(${idx},'local')">Keep Local</button>
          <button class="cf-btn" id="cf-import-${idx}"   onclick="resolveField(${idx},'import')">Use Import</button>
          <button class="cf-btn" id="cf-disputed-${idx}" onclick="resolveField(${idx},'disputed')">Disputed</button>
        </div>
      </div>`).join('');

    return `
      <div class="conflict-node">
        <div class="conflict-node-header">
          <span class="conflict-node-name">${name}</span>
          <div class="conflict-node-btns">
            <button class="cf-btn" onclick="resolveNode('${id}','local')">Keep All Local</button>
            <button class="cf-btn" onclick="resolveNode('${id}','import')">Use All Import</button>
            <button class="cf-btn" onclick="resolveNode('${id}','disputed')">All Disputed</button>
          </div>
        </div>
        ${fieldRows}
      </div>`;
  }).join('');

  document.getElementById('conflictModal').style.display = 'block';
}

function resolveField(idx, resolution) {
  pendingConflicts[idx].resolution = resolution;
  ['local','import','disputed'].forEach(r => {
    const btn = document.getElementById(`cf-${r}-${idx}`);
    if (btn) btn.classList.toggle(`res-${r}`, r === resolution);
  });
  const row = document.getElementById(`cf-row-${idx}`);
  if (row) row.style.opacity = resolution ? '1' : '0.6';
}

function resolveNode(nodeId, resolution) {
  pendingConflicts.forEach((c, idx) => {
    if (c.id === nodeId) resolveField(idx, resolution);
  });
}

function resolveAllDisputed() {
  pendingConflicts.forEach((_, idx) => resolveField(idx, 'disputed'));
}

function applyConflictResolutions() {
  // Check all resolved
  const unresolved = pendingConflicts.filter(c => !c.resolution);
  if (unresolved.length) {
    unresolved.forEach(c => {
      const row = document.getElementById(`cf-row-${pendingConflicts.indexOf(c)}`);
      if (row) { row.style.outline = '1px solid #e74c3c'; row.style.opacity = '1'; }
    });
    document.getElementById('conflictSub').textContent =
      `Please resolve all ${unresolved.length} remaining conflict${unresolved.length > 1 ? 's' : ''} before applying.`;
    document.getElementById('conflictSub').style.color = '#e74c3c';
    return;
  }

  // Merge non-conflicting data first (fields with no local value)
  mergeNodeData(pendingIncoming);

  // Apply resolutions (overwrite what mergeNodeData may have set for conflicted fields)
  pendingConflicts.forEach(({ id, field, local, imported, resolution }) => {
    if (!nodeData[id]) nodeData[id] = {};
    if (resolution === 'local')    nodeData[id][field] = local;
    if (resolution === 'import')   nodeData[id][field] = imported;
    if (resolution === 'disputed') { nodeData[id][field] = undefined; nodeData[id].ae = 'Disputed'; }
  });

  // Clean up undefined fields
  Object.keys(nodeData).forEach(id => {
    Object.keys(nodeData[id]).forEach(f => { if (nodeData[id][f] === undefined) delete nodeData[id][f]; });
    if (!Object.keys(nodeData[id]).length) delete nodeData[id];
  });

  cancelConflict();
  saveAssignments();
  render();
}

function cancelConflict() {
  document.getElementById('conflictModal').style.display = 'none';
  pendingConflicts = [];
  pendingIncoming  = {};
}

function importAssignments() {
  const input = document.createElement('input');
  input.type = 'file';
  input.accept = '.csv,.json';
  input.onchange = (e) => {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (ev) => {
      try {
        const text = ev.target.result.trim();

        if (text.startsWith('{')) {
          const raw = JSON.parse(text);
          if (raw._tree) {
            ORG = raw._tree;
            rebuildNodeIndex();
          }
          // normalise nodeData fields (skip _tree)
          const normalised = {};
          Object.entries(raw).forEach(([k,v]) => {
            if (k === '_tree') return;
            normalised[k] = typeof v==='string' ? {ae:v} : v;
          });
          if (Object.keys(normalised).length) {
            applyImportWithConflicts(normalised);
          } else {
            saveAssignments();
            render();
          }
        } else {
          // CSV — build lookup maps from the org tree
          const nameMap = {}, structMap = {};
          function buildMaps(node) {
            nameMap[node.name.toLowerCase()] = node.id;
            const {facility, structure} = parseDesignator(node.name);
            if (structure) structMap[structure.toLowerCase()] = node.id;
            if (facility)  structMap[facility.toLowerCase().replace(/[\s-]/g,'')] = node.id;
            if (node.children) node.children.forEach(buildMaps);
          }
          buildMaps(ORG);

          const lines = text.split(/\r?\n/);
          const hdrs = parseCSVLine(lines[0]).map(h => h.trim().toLowerCase().replace(/[^a-z]/g,''));
          const ci=hdrs.indexOf('commandfacility'), si=hdrs.indexOf('structure');
          const ti=hdrs.indexOf('target'), ei=hdrs.indexOf('engagement');
          const ai=hdrs.indexOf('ae'), ni=hdrs.indexOf('notes');
          const get = (c,idx) => idx>=0 ? (c[idx]||'').trim() : '';

          const incoming = {};
          for (let i = 1; i < lines.length; i++) {
            if (!lines[i].trim()) continue;
            const c = parseCSVLine(lines[i]);
            const facility = get(c,ci), structure = get(c,si);
            const id = nameMap[structure.toLowerCase()]
                    || structMap[structure.toLowerCase()]
                    || structMap[facility.toLowerCase().replace(/[\s-]/g,'')]
                    || nameMap[facility.toLowerCase()];
            if (!id) continue;
            const nd = {};
            const ae=get(c,ai); if (ae && PEOPLE[ae]) nd.ae=ae;
            const tgt=get(c,ti); if (tgt) nd.target=tgt;
            const eng=get(c,ei); if (eng) nd.engagement=eng;
            const notes=get(c,ni); if (notes) nd.notes=notes;
            if (Object.keys(nd).length) incoming[id]=nd;
          }
          applyImportWithConflicts(incoming);
        }
      } catch(err) { alert('Import failed: ' + err.message); }
    };
    reader.readAsText(file);
  };
  input.click();
}

// ─── GITHUB INTEGRATION ──────────────────────────────────────────────────────
let ghSettings = null;

function loadGhSettings() {
  if (ghSettings) return;
  try {
    const s = localStorage.getItem('navyOrgGH');
    if (s) { ghSettings = JSON.parse(s); return; }
  } catch(e) {}
  // Auto-detect owner/repo from GitHub Pages URL: {owner}.github.io/{repo}/...
  const host = window.location.hostname;
  const parts = window.location.pathname.split('/').filter(Boolean);
  const owner = host.endsWith('.github.io') ? host.replace('.github.io', '') : '';
  const repo  = owner && parts.length ? parts[0] : '';
  ghSettings = { token: '', owner, repo, filePath: 'navy_org_data.json' };
}

function setFetchStatus(msg, state) {
  const el = document.getElementById('fetchStatus');
  if (!el) return;
  el.textContent = msg;
  el.className = state || '';
}

async function fetchSharedFile(silent) {
  if (!silent) setFetchStatus('Syncing…', '');
  try {
    const res = await fetch('./navy_org_data.json', { cache: 'no-cache' });
    if (!res.ok) {
      if (!silent) setFetchStatus('No repo file found', 'err');
      return;
    }
    const raw = await res.json();
    let treeChanged = false;
    if (raw._tree) { ORG = raw._tree; rebuildNodeIndex(); treeChanged = true; }
    const { _tree, ...rest } = raw;
    const normalised = {};
    Object.entries(rest).forEach(([k, v]) => { normalised[k] = typeof v === 'string' ? {ae: v} : v; });
    if (Object.keys(normalised).length) {
      applyImportWithConflicts(normalised);
    } else if (treeChanged) {
      saveAssignments(); render();
    }
    const t = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
    setFetchStatus('✓ Synced ' + t, 'ok');
  } catch(e) {
    if (!silent) setFetchStatus('Sync failed', 'err');
  }
}

async function saveToGitHub() {
  loadGhSettings();
  if (!ghSettings.token || !ghSettings.owner || !ghSettings.repo) {
    showGhSettings();
    return;
  }
  setFetchStatus('Saving to GitHub…', '');
  try {
    const { token, owner, repo, filePath } = ghSettings;
    const apiUrl = `https://api.github.com/repos/${owner}/${repo}/contents/${filePath}`;
    const headers = { Authorization: `Bearer ${token}`, Accept: 'application/vnd.github+json', 'Content-Type': 'application/json' };

    // Need current SHA to update an existing file
    let sha;
    const getRes = await fetch(apiUrl, { headers });
    if (getRes.ok) sha = (await getRes.json()).sha;

    const content = btoa(unescape(encodeURIComponent(JSON.stringify({ _tree: ORG, ...nodeData }, null, 2))));
    const body = { message: 'Update org mapping data', content };
    if (sha) body.sha = sha;

    const putRes = await fetch(apiUrl, { method: 'PUT', headers, body: JSON.stringify(body) });
    if (!putRes.ok) {
      const err = await putRes.json().catch(() => ({}));
      setFetchStatus('Save failed: ' + (err.message || putRes.status), 'err');
      return;
    }
    const t = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
    setFetchStatus('✓ Saved to GitHub ' + t, 'ok');
  } catch(e) {
    setFetchStatus('Save error: ' + e.message, 'err');
  }
}

function showGhSettings() {
  loadGhSettings();
  document.getElementById('ghToken').value = ghSettings.token || '';
  document.getElementById('ghOwner').value = ghSettings.owner || '';
  document.getElementById('ghRepo').value  = ghSettings.repo  || '';
  document.getElementById('ghFile').value  = ghSettings.filePath || 'navy_org_data.json';
  document.getElementById('ghModal').classList.add('open');
  document.getElementById('ghToken').focus();
}

function closeGhSettings() {
  document.getElementById('ghModal').classList.remove('open');
}

function applyGhSettings() {
  ghSettings = {
    token:    document.getElementById('ghToken').value.trim(),
    owner:    document.getElementById('ghOwner').value.trim(),
    repo:     document.getElementById('ghRepo').value.trim(),
    filePath: document.getElementById('ghFile').value.trim() || 'navy_org_data.json'
  };
  try { localStorage.setItem('navyOrgGH', JSON.stringify(ghSettings)); } catch(e) {}
  closeGhSettings();
}

// ─── INIT ─────────────────────────────────────────────────────────────────────
loadAssignments();
rebuildNodeIndex(); // ORG may have been replaced by loadAssignments

// Collapse depth ≥ 2 by default (leave root + US Navy/USMC visible)
function initCollapse(node, depth) {
  if (depth >= 2 && node.children && node.children.length) collapsed[node.id] = true;
  if (node.children) node.children.forEach(c => initCollapse(c, depth + 1));
}
initCollapse(ORG, 0);

render();
loadGhSettings();
tryRestoreFile();
fetchSharedFile(true); // auto-fetch navy_org_data.json from same URL path on load
</script>
</body>
</html>
