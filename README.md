<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Vanilla — Aesthetic To‑Do</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&family=Space+Grotesk:wght@400;600&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg: #0f1724; --panel:#0b1117; --muted:#9aa7b2; --text:#e6eef6; --accent1:#8dd3c7; --accent2:#7cc4ff; --glass: rgba(255,255,255,0.04);
    }
    [data-theme="light"]{ --bg:#f7fafc; --panel:#ffffff; --muted:#4b5563; --text:#0b1320; --accent1:#6ee7b7; --accent2:#60a5fa; --glass: rgba(2,6,23,0.04); }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{background:linear-gradient(180deg,var(--bg),rgba(0,0,0,0.02));color:var(--text);display:flex;align-items:center;justify-content:center;padding:28px}

    .app{width:1100px;max-width:96vw;height:78vh;min-height:560px;border-radius:20px;display:grid;grid-template-columns:320px 1fr;gap:18px;padding:20px;background:linear-gradient(180deg,var(--panel),rgba(255,255,255,0.02));box-shadow:0 10px 40px rgba(2,6,23,0.45);overflow:hidden}

    /* Left sidebar */
    .sidebar{padding:14px;border-radius:14px;background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);backdrop-filter:blur(6px);display:flex;flex-direction:column;gap:12px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:46px;height:46px;border-radius:12px;background:linear-gradient(135deg,var(--accent1),var(--accent2));display:grid;place-items:center;font-weight:800;color:#07202a}
    .app-title{font-weight:700;font-family:'Space Grotesk',Inter;font-size:18px}
    .sidebar .controls{display:flex;gap:8px;align-items:center}
    .btn{border:0;padding:8px 12px;border-radius:10px;background:var(--glass);color:var(--text);cursor:pointer;font-weight:600}
    .btn.primary{background:linear-gradient(90deg,var(--accent1),var(--accent2));color:#052227}

    .list-search{display:flex;gap:8px;align-items:center}
    .list-search input{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text);outline:none}

    .lists{overflow:auto;padding-right:6px}
    .list-item{display:flex;align-items:center;justify-content:space-between;padding:10px;border-radius:10px;margin-bottom:8px;cursor:pointer}
    .list-item:hover{background:var(--glass)}
    .list-item.active{background:linear-gradient(90deg,rgba(124,196,255,0.12),rgba(141,211,199,0.08));box-shadow:inset 0 0 0 1px rgba(255,255,255,0.02)}
    .list-meta{font-size:13px;color:var(--muted)}

    /* Main */
    .main{padding:18px;border-radius:14px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);display:flex;flex-direction:column}
    .main-head{display:flex;align-items:center;gap:16px}
    .title{font-weight:700;font-size:20px}
    .meta{color:var(--muted);font-size:13px}

    .top-actions{margin-left:auto;display:flex;gap:10px}

    .new-task{display:flex;gap:10px;margin-top:14px}
    .new-task input{flex:1;padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:var(--text);outline:none}
    .new-task button{padding:10px 14px;border-radius:12px;border:0;font-weight:700;cursor:pointer;background:linear-gradient(90deg,var(--accent1),var(--accent2));color:#042026}

    .tasks{margin-top:16px;overflow:auto;padding-right:8px}
    .task{display:flex;align-items:center;gap:12px;padding:12px;border-radius:12px;margin-bottom:10px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);box-shadow:0 4px 18px rgba(2,6,23,0.25)}
    .task .left{display:flex;align-items:center;gap:12px}
    .checkbox{width:18px;height:18px;border-radius:6px;border:1px solid rgba(255,255,255,0.06);display:grid;place-items:center;cursor:pointer}
    .checkbox.checked{background:linear-gradient(90deg,var(--accent1),var(--accent2));border:0;color:#042026}
    .task .text{font-weight:600}
    .task.done .text{opacity:0.5;text-decoration:line-through}
    .task .actions{margin-left:auto;display:flex;gap:8px}
    .icon-btn{background:transparent;border:0;padding:8px;border-radius:8px;cursor:pointer;color:var(--muted)}
    .icon-btn:hover{color:var(--text);background:var(--glass)}

    .empty{padding:24px;border-radius:12px;text-align:center;color:var(--muted);border:1px dashed rgba(255,255,255,0.03)}

    /* responsive */
    @media (max-width:900px){.app{grid-template-columns:1fr;grid-template-rows:auto auto;height:90vh;padding:12px}.sidebar{order:2}.main{order:1}}
  </style>
</head>
<body>
  <div class="app" id="app">
    <div class="sidebar" role="navigation">
      <div class="brand">
        <div class="logo">V</div>
        <div>
          <div class="app-title">Vanilla</div>
          <div class="meta">Aesthetic To‑Do • Vanilla JS</div>
        </div>
      </div>

      <div class="list-search">
        <input id="searchLists" placeholder="Search lists…" aria-label="Search lists">
        <button class="btn" id="addListBtn" title="New list (N)">＋</button>
      </div>

      <div class="lists" id="lists"></div>

      <div style="display:flex;gap:8px;margin-top:auto;align-items:center">
        <button class="btn" id="exportBtn">Export</button>
        <button class="btn" id="importBtn">Import</button>
        <button class="btn" id="toggleTheme">Theme</button>
      </div>
    </div>

    <div class="main" role="main">
      <div class="main-head">
        <div>
          <div class="title" id="listTitle">Your Lists</div>
          <div class="meta" id="listMeta">Create a list to get started</div>
        </div>
        <div class="top-actions">
          <button class="btn" id="renameList">Rename</button>
          <button class="btn" id="deleteList">Delete</button>
        </div>
      </div>

      <div class="new-task" aria-label="Add a task">
        <input id="newTask" placeholder="Add a task and press Enter…" />
        <button id="addTask" title="Add task">Add</button>
      </div>

      <div class="tasks" id="taskList"></div>
    </div>
  </div>

  <template id="listTpl">
    <div class="list-item" role="option">
      <div>
        <div class="list-name"></div>
        <div class="list-meta"></div>
      </div>
      <div class="count"></div>
    </div>
  </template>

  <template id="taskTpl">
    <div class="task">
      <div class="left">
        <div class="checkbox" role="button" tabindex="0"></div>
        <div class="text" contenteditable="true" spellcheck="false"></div>
      </div>
      <div class="actions">
        <button class="icon-btn edit" title="Edit">✎</button>
        <button class="icon-btn delete" title="Delete">🗑</button>
      </div>
    </div>
  </template>

  <script>
    /* --------------------------
       State & Storage
       ---------------------------*/
    const KEY = 'vanilla-aesthetic-v1';
    const initial = () => ({ theme: 'dark', lists: [], selected: null });

    function load(){ try{ return JSON.parse(localStorage.getItem(KEY)) || initial(); }catch{ return initial(); } }
    function save(s){ localStorage.setItem(KEY, JSON.stringify(s)); }

    let state = load();
    if(!state.lists.length){ const id = crypto.randomUUID(); state.lists.push({id,name:'My First List',tasks:[]}); state.selected = id; save(state); }

    /* --------------------------
       Elements
       ---------------------------*/
    const listsEl = document.getElementById('lists');
    const listTitleEl = document.getElementById('listTitle');
    const listMetaEl = document.getElementById('listMeta');
    const taskListEl = document.getElementById('taskList');
    const newTaskEl = document.getElementById('newTask');

    const addListBtn = document.getElementById('addListBtn');
    const searchLists = document.getElementById('searchLists');
    const toggleThemeBtn = document.getElementById('toggleTheme');
    const addTaskBtn = document.getElementById('addTask');
    const renameListBtn = document.getElementById('renameList');
    const deleteListBtn = document.getElementById('deleteList');
    const exportBtn = document.getElementById('exportBtn');
    const importBtn = document.getElementById('importBtn');

    const listTpl = document.getElementById('listTpl');
    const taskTpl = document.getElementById('taskTpl');

    /* --------------------------
       Helpers
       ---------------------------*/
    function getSelected(){ return state.lists.find(l=>l.id===state.selected) || state.lists[0]; }
    function applyTheme(){ document.documentElement.setAttribute('data-theme', state.theme==='light'?'light':''); }

    /* --------------------------
       Rendering
       ---------------------------*/
    function renderLists(){
      const q = (searchLists.value||'').toLowerCase();
      listsEl.innerHTML = '';
      state.lists.filter(l=>l.name.toLowerCase().includes(q)).forEach(l=>{
        const node = listTpl.content.firstElementChild.cloneNode(true);
        node.dataset.id = l.id;
        node.querySelector('.list-name').textContent = l.name;
        node.querySelector('.list-meta').textContent = `${l.tasks.filter(t=>!t.done).length}/${l.tasks.length} open`;
        node.querySelector('.count').textContent = l.tasks.length;
        if(l.id===state.selected) node.classList.add('active');
        listsEl.appendChild(node);
      });
      if(!listsEl.children.length){ const empty = document.createElement('div'); empty.className='empty'; empty.textContent='No lists'; listsEl.appendChild(empty); }
    }

    function renderTasks(){
      const list = getSelected();
      taskListEl.innerHTML = '';
      if(!list){ listTitleEl.textContent='No list'; listMetaEl.textContent='Create one'; return; }
      listTitleEl.textContent = list.name;
      listMetaEl.textContent = `${list.tasks.length} tasks`;
      if(!list.tasks.length){ taskListEl.innerHTML='<div class="empty">No tasks yet ✨</div>'; return; }
      list.tasks.slice().sort((a,b)=>Number(a.done)-Number(b.done)||a.created-b.created).forEach(t=>{
        const node = taskTpl.content.firstElementChild.cloneNode(true);
        node.dataset.id = t.id;
        const cb = node.querySelector('.checkbox');
        const text = node.querySelector('.text');
        if(t.done){ node.classList.add('done'); cb.classList.add('checked'); cb.textContent='✓'; }
        text.textContent = t.text;
        taskListEl.appendChild(node);
      });
    }

    function render(){ applyTheme(); renderLists(); renderTasks(); }

    /* --------------------------
       Actions / CRUD
       ---------------------------*/
    function addList(name){ const id = crypto.randomUUID(); state.lists.push({id,name:name||'Untitled',tasks:[]}); state.selected=id; save(state); render(); }
    function renameSelected(newName){ const s = getSelected(); if(!s) return; s.name = newName||s.name; save(state); renderLists(); }
    function deleteSelected(){ if(!confirm('Delete this list?')) return; const idx = state.lists.findIndex(l=>l.id===state.selected); if(idx===-1) return; state.lists.splice(idx,1); state.selected = state.lists[0]?.id||null; save(state); render(); }

    function addTask(text){ const s = getSelected(); if(!s) return; s.tasks.push({id:crypto.randomUUID(),text,done:false,created:Date.now()}); save(state); renderTasks(); }
    function updateTask(listId,taskId,patch){ const l = state.lists.find(x=>x.id===listId); if(!l) return; const t = l.tasks.find(x=>x.id===taskId); if(!t) return; Object.assign(t,patch); save(state); renderTasks(); }
    function deleteTask(listId,taskId){ const l = state.lists.find(x=>x.id===listId); if(!l) return; l.tasks = l.tasks.filter(x=>x.id!==taskId); save(state); renderTasks(); }

    /* --------------------------
       Events (DOM manipulation)
       ---------------------------*/
    listsEl.addEventListener('click', e=>{
      const item = e.target.closest('.list-item'); if(!item) return; state.selected = item.dataset.id; save(state); render();
    });

    addListBtn.addEventListener('click', ()=>{ const name = prompt('List name'); if(name) addList(name); });
    renameListBtn.addEventListener('click', ()=>{ const s=getSelected(); if(!s) return; const n = prompt('Rename list', s.name); if(n!==null) renameSelected(n); });
    deleteListBtn.addEventListener('click', deleteSelected);

    // Add task
    addTaskBtn.addEventListener('click', ()=>{ const txt = newTaskEl.value.trim(); if(!txt) return; addTask(txt); newTaskEl.value=''; newTaskEl.focus(); });
    newTaskEl.addEventListener('keydown', e=>{ if(e.key==='Enter'){ e.preventDefault(); addTaskBtn.click(); } });

    // Task interactions (delegation)
    taskListEl.addEventListener('click', e=>{
      const taskNode = e.target.closest('.task'); if(!taskNode) return; const id = taskNode.dataset.id; const list = getSelected();
      if(e.target.closest('.checkbox')){
        const t = list.tasks.find(x=>x.id===id); if(!t) return; updateTask(list.id,id,{done:!t.done}); return;
      }
      if(e.target.closest('.delete')){ deleteTask(list.id,id); return; }
      if(e.target.closest('.edit')){ const textEl = taskNode.querySelector('.text'); textEl.focus(); document.execCommand && document.execCommand('selectAll',false,null); return; }
    });

    // Inline editing
    taskListEl.addEventListener('blur', e=>{
      if(!e.target.matches('.text')) return; const node = e.target.closest('.task'); const id = node.dataset.id; const list = getSelected(); const val = e.target.textContent.trim(); if(!val){ deleteTask(list.id,id); } else { updateTask(list.id,id,{text:val}); }
    }, true);

    // checkbox keyboard
    taskListEl.addEventListener('keydown', e=>{
      if(e.key===' ' && e.target.classList.contains('checkbox')){ e.preventDefault(); e.target.click(); }
      if(e.key==='Enter' && e.target.classList.contains('text')){ e.preventDefault(); e.target.blur(); }
    });

    // Search lists
    searchLists.addEventListener('input', renderLists);

    // Theme toggle
    toggleThemeBtn.addEventListener('click', ()=>{ state.theme = state.theme==='light'?'dark':'light'; save(state); applyTheme(); });

    // Export / Import
    exportBtn.addEventListener('click', ()=>{
      const data = JSON.stringify(state, null, 2);
      const blob = new Blob([data], {type:'application/json'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = 'vanilla-todo-export.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
    });
    importBtn.addEventListener('click', ()=>{
      const input = document.createElement('input'); input.type='file'; input.accept='application/json'; input.onchange = ()=>{
        const f = input.files[0]; if(!f) return; const reader = new FileReader(); reader.onload = ()=>{ try{ state = JSON.parse(reader.result); save(state); render(); }catch(err){ alert('Invalid file'); } }; reader.readAsText(f);
      }; input.click();
    });

    // Keyboard shortcuts (safe — won't interfere with typing)
    document.addEventListener('keydown', e=>{
      const tag = e.target.tagName;
      const isTyping = tag==='INPUT' || tag==='TEXTAREA' || e.target.isContentEditable;
      if(isTyping) return; // guard

      if(e.key.toLowerCase()==='t') { toggleThemeBtn.click(); }
      if(e.key.toLowerCase()==='n') { addListBtn.click(); }
      if(e.key.toLowerCase()==='e') { exportBtn.click(); }
    });

    // Prevent accidental theme toggle while focusing contenteditable (extra guard)
    window.addEventListener('beforeunload', ()=>{ save(state); });

    // Initial render
    applyTheme();
    render();
  </script>
</body>
</html>
