<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Shiv Mobiles</title>
  <meta name="theme-color" content="#0b0b0b" />
  <style>
    :root{
      --bg:#0b0b0b; --card:#111; --muted:#bfbfbf; --accent:#00d1b2; --glass: rgba(255,255,255,0.03);
      --radius:12px; --max-width:1100px;
    }
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,Segoe UI,Roboto,Arial;background:var(--bg);color:#fff;}
    header{background:linear-gradient(90deg,#070707, #0f0f0f);padding:18px;border-bottom:1px solid rgba(255,255,255,0.03)}
    .wrap{max-width:var(--max-width);margin:0 auto;padding:18px}
    .brand{display:flex;gap:14px;align-items:center}
    .logo{width:48px;height:48px;border-radius:8px;background:linear-gradient(135deg,#1b1b1b,#0f0f0f);display:flex;align-items:center;justify-content:center;font-weight:700}
    h1{margin:0;font-size:20px}
    nav{margin-left:auto;display:flex;gap:12px}
    a.btn{padding:8px 12px;border-radius:10px;background:var(--glass);text-decoration:none;color:#ddd;border:1px solid rgba(255,255,255,0.03)}

    main{padding:28px 0}
    .controls{display:flex;gap:12px;flex-wrap:wrap;align-items:center;margin-bottom:20px}
    select,input[type=text]{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:10px;color:#fff}
    .search{flex:1;min-width:200px}

    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:18px}
    .card{background:var(--card);padding:12px;border-radius:var(--radius);border:1px solid rgba(255,255,255,0.03);box-shadow:0 6px 18px rgba(0,0,0,0.5)}
    .card img{width:100%;height:180px;object-fit:cover;border-radius:8px}
    .card h3{margin:10px 0 6px;font-size:16px}
    .meta{display:flex;justify-content:space-between;align-items:center;color:var(--muted);font-size:14px}
    .tag{background:rgba(255,255,255,0.03);padding:6px 8px;border-radius:8px;font-size:13px}

    .empty{padding:40px;text-align:center;color:var(--muted)}

    /* Admin modal */
    .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:linear-gradient(180deg,rgba(0,0,0,0.6),rgba(0,0,0,0.8));z-index:50}
    .modal.show{display:flex}
    .panel{background:#0e0e0e;padding:20px;border-radius:14px;width:100%;max-width:680px;border:1px solid rgba(255,255,255,0.04)}
    .panel h2{margin:0 0 10px}
    .form-row{display:flex;gap:10px;margin-bottom:10px}
    .form-row input,.form-row select,.form-row textarea{flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:#0b0b0b;color:#fff}
    textarea{min-height:90px;resize:vertical}
    .actions{display:flex;gap:10px;justify-content:flex-end;margin-top:12px}
    button.primary{background:var(--accent);border:none;padding:10px 14px;border-radius:8px;color:#021;cursor:pointer}
    button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px 12px;border-radius:8px;color:#fff;cursor:pointer}

    footer{padding:18px;border-top:1px solid rgba(255,255,255,0.03);text-align:center;color:var(--muted)}

    /* small */
    @media(max-width:600px){.form-row{flex-direction:column}}
  </style>
</head>
<body>
  <header>
    <div class="wrap" style="display:flex;align-items:center">
      <div class="brand">
        <div class="logo">SM</div>
        <div>
          <h1>Shiv Mobiles</h1>
          <div style="font-size:12px;color:var(--muted)">Best second-hand mobiles — updated daily</div>
        </div>
      </div>
      <nav class="flex items-center justify-between px-6 py-4 bg-white border-b border-blue-400 shadow-sm">
    <div class="flex items-center gap-3">
        <img src="assets/logo.png" alt="Shiv Mobiles Logo" class="w-12 h-12 rounded-xl border border-blue-500 shadow-md">
        <h1 class="text-2xl font-bold text-blue-700">Shiv Mobiles</h1>
    </div>

    <div class="space-x-4 hidden md:flex">
        <button id="homeBtn" class="px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-500 transition">Home</button>
        <button id="sellBtn" class="px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-500 transition">Sell Phone</button>
        <button id="adminBtn" class="px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-500 transition">Admin</button>
    </div>
</nav>
    </div>
  </header>

  <main class="wrap">
    <section id="home">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
        <h2 style="margin:0">Available Phones</h2>
        <div style="color:var(--muted);font-size:13px">You can add phones from Admin panel (10/day recommended)</div>
      </div>

      <div class="controls">
        <input class="search" id="searchInput" type="text" placeholder="Search by model, price or condition" oninput="renderGrid()">
        <select id="brandFilter" onchange="renderGrid()">
          <option value="all">All Brands</option>
        </select>
        <select id="sortBy" onchange="renderGrid()">
          <option value="new">Newest</option>
          <option value="priceLow">Price: Low → High</option>
          <option value="priceHigh">Price: High → Low</option>
        </select>
      </div>

      <div id="grid" class="grid"></div>
      <div id="empty" class="empty" style="display:none">No phones found. Add phones from Admin panel.</div>
    </section>

    <section id="sell" style="margin-top:36px">
      <h2>Sell Your Phone</h2>
      <p style="color:var(--muted);max-width:700px">Fill details and we'll contact you. (Or use Admin panel to manage listings directly.)</p>
      <div class="panel" style="max-width:700px;margin-top:12px">
        <div class="form-row"><input id="s_name" placeholder="Your name"></div>
        <div class="form-row"><input id="s_phoneModel" placeholder="Phone model (e.g. Samsung S21)"></div>
        <div class="form-row"><input id="s_price" placeholder="Expected price (₹)"></div>
        <div class="form-row"><select id="s_brand"><option>Samsung</option><option>Apple</option><option>Vivo</option><option>Realme</option><option>Xiaomi</option><option>Other</option></select></div>
        <div class="form-row"><textarea id="s_desc" placeholder="Condition / Notes"></textarea></div>
        <div style="display:flex;gap:10px;align-items:center">
          <input id="s_image" type="file" accept="image/*">
          <img id="s_preview" style="width:84px;height:64px;object-fit:cover;border-radius:6px;display:none">
        </div>
        <div class="actions">
          <button class="ghost" onclick="clearSellForm()">Clear</button>
          <button class="primary" onclick="submitSellForm()">Submit</button>
        </div>
      </div>
    </section>
  </main>

  <footer>
    © Shiv Mobiles • Built for sellers — Manage listings via Admin (password protected)
  </footer>

  <!-- Admin Modal -->
  <div id="modal" class="modal">
    <div class="panel">
      <h2 id="modalTitle">Admin Login</h2>

      <div id="loginForm">
        <div class="form-row"><input id="adminPass" type="password" placeholder="Enter admin password"></div>
        <div class="actions"><button class="ghost" onclick="closeModal()">Cancel</button><button class="primary" onclick="adminLogin()">Login</button></div>
      </div>

      <div id="adminPanel" style="display:none">
        <div style="display:flex;gap:10px;align-items:center;margin-bottom:12px">
          <div style="font-weight:600">Add new phone</div>
          <div style="margin-left:auto;color:var(--muted);font-size:13px">Default pass: <span style="font-family:monospace">shiv123</span></div>
        </div>
        <div class="form-row"><input id="a_brand" placeholder="Brand (e.g. Samsung)"></div>
        <div class="form-row"><input id="a_model" placeholder="Model (e.g. S21)"></div>
        <div class="form-row"><input id="a_price" placeholder="Price (₹)"></div>
        <div class="form-row"><textarea id="a_desc" placeholder="Condition / notes"></textarea></div>
        <div style="display:flex;gap:10px;align-items:center;margin-bottom:10px">
          <input id="a_image" type="file" accept="image/*">
          <img id="a_preview" style="width:88px;height:64px;object-fit:cover;border-radius:6px;display:none">
        </div>
        <div style="display:flex;gap:8px;justify-content:flex-end">
          <button class="ghost" onclick="logoutAdmin()">Logout</button>
          <button class="ghost" onclick="clearAdminForm()">Clear</button>
          <button class="primary" onclick="addPhone()">Add Phone</button>
        </div>
        <hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:12px 0">
        <div style="max-height:220px;overflow:auto">
          <div id="adminList"></div>
        </div>
      </div>

    </div>
  </div>

  <script>
    // Simple storage-based site for managing listings. Data persists in browser (localStorage).
    const STORAGE_KEY = 'shivMobilesData_v1';
    let data = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');

    // init brands
    const brands = ['Samsung','Apple','Vivo','Realme','Xiaomi','OnePlus','Motorola','Other'];
    const brandFilter = document.getElementById('brandFilter');
    brands.forEach(b=>{const opt=document.createElement('option');opt.value=b;opt.textContent=b;brandFilter.appendChild(opt)});

    function saveData(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(data)); }

    function renderGrid(){
      const grid = document.getElementById('grid');
      const empty = document.getElementById('empty');
      grid.innerHTML='';
      const q = document.getElementById('searchInput').value.toLowerCase();
      const brand = document.getElementById('brandFilter').value;
      const sort = document.getElementById('sortBy').value;
      let list = data.slice();
      if(brand!=='all') list = list.filter(x=>x.brand===brand);
      if(q) list = list.filter(x=> (x.model+ ' ' + x.price + ' ' + x.desc + ' ' + x.brand).toLowerCase().includes(q));
      if(sort==='priceLow') list.sort((a,b)=>Number(a.price)-Number(b.price));
      else if(sort==='priceHigh') list.sort((a,b)=>Number(b.price)-Number(a.price));
      else list.sort((a,b)=>b.addedAt - a.addedAt);

      if(list.length===0){ empty.style.display='block'; return } else empty.style.display='none';

      list.forEach(item=>{
        const card = document.createElement('div'); card.className='card';
        card.innerHTML = `
          <img src="${item.image||'data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' width=\'400\' height=\'300\'><rect width=\'100%\' height=\'100%\' fill=\'#111111\'/><text x=\'50%\' y=\'50%\' fill=\'#777\' dominant-baseline=\'middle\' text-anchor=\'middle\'>No Image</text></svg>'}" alt="${item.model}">
          <h3>${escapeHtml(item.brand)} ${escapeHtml(item.model)}</h3>
          <div class="meta"><div class="tag">₹ ${formatNumber(item.price)}</div><div style="text-align:right"><div style="font-size:13px;color:var(--muted)">${escapeHtml(item.desc||'')}</div><div style="font-size:12px;color:var(--muted);margin-top:6px">${timeAgo(item.addedAt)}</div></div></div>`;
        grid.appendChild(card);
      });
    }

    function formatNumber(n){return Number(n).toLocaleString('en-IN')}
    function timeAgo(ts){const diff=(Date.now()-ts)/1000; if(diff<60) return 'Just now'; if(diff<3600) return Math.floor(diff/60)+'m ago'; if(diff<86400) return Math.floor(diff/3600)+'h ago'; return Math.floor(diff/86400)+'d ago'}
    function escapeHtml(s){ if(!s) return ''; return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;') }

    // Admin modal behavior
    const modal = document.getElementById('modal');
    const adminBtn = document.getElementById('adminBtn');
    adminBtn.addEventListener('click',()=>{ openModal() });
    function openModal(){ modal.classList.add('show'); document.getElementById('modalTitle').textContent='Admin Login'; document.getElementById('loginForm').style.display='block'; document.getElementById('adminPanel').style.display='none'; }
    function closeModal(){ modal.classList.remove('show'); }

    window.addEventListener('click',e=>{ if(e.target===modal) closeModal(); })

    function adminLogin(){ const pass = document.getElementById('adminPass').value; if(!pass) return alert('Enter password'); if(pass==='shiv123'){ document.getElementById('loginForm').style.display='none'; document.getElementById('adminPanel').style.display='block'; document.getElementById('modalTitle').textContent='Admin Panel'; renderAdminList(); } else alert('Wrong password') }
    function logoutAdmin(){ document.getElementById('adminPass').value=''; openModal(); }

    // Admin add phone
    document.getElementById('a_image').addEventListener('change', (e)=>previewImage(e.target, 'a_preview'));
    document.getElementById('s_image').addEventListener('change', (e)=>previewImage(e.target, 's_preview'));

    function previewImage(input, previewId){ const file = input.files[0]; const img = document.getElementById(previewId); if(!file){ img.style.display='none'; img.src=''; return } const reader = new FileReader(); reader.onload = ()=>{ img.src = reader.result; img.style.display='block' }; reader.readAsDataURL(file); }

    function clearAdminForm(){ document.getElementById('a_brand').value=''; document.getElementById('a_model').value=''; document.getElementById('a_price').value=''; document.getElementById('a_desc').value=''; document.getElementById('a_image').value=''; document.getElementById('a_preview').style.display='none'; }

    function addPhone(){ const brand = document.getElementById('a_brand').value.trim(); const model = document.getElementById('a_model').value.trim(); const price = document.getElementById('a_price').value.trim(); const desc = document.getElementById('a_desc').value.trim(); const imgEl = document.getElementById('a_preview'); if(!brand||!model||!price) return alert('Brand, model and price required'); const image = imgEl.src||''; const item = {id:Date.now(), brand, model, price, desc, image, addedAt:Date.now()}; data.unshift(item); saveData(); renderGrid(); renderAdminList(); clearAdminForm(); alert('Phone added ✅') }

    function renderAdminList(){ const el = document.getElementById('adminList'); el.innerHTML=''; if(data.length===0) el.innerHTML='<div style="color:var(--muted);padding:10px">No listings yet</div>'; data.forEach(item=>{ const row=document.createElement('div'); row.style.display='flex'; row.style.justifyContent='space-between'; row.style.alignItems='center'; row.style.padding='8px 6px'; row.style.borderBottom='1px solid rgba(255,255,255,0.02)'; row.innerHTML=`<div><strong>${escapeHtml(item.brand)} ${escapeHtml(item.model)}</strong><div style="font-size:13px;color:var(--muted)">₹ ${formatNumber(item.price)} • ${timeAgo(item.addedAt)}</div></div><div style="display:flex;gap:8px;align-items:center"><button class="ghost" onclick="editPhone(${item.id})">Edit</button><button class="ghost" onclick="deletePhone(${item.id})">Delete</button></div>`; el.appendChild(row); }) }

    function editPhone(id){ const item = data.find(d=>d.id===id); if(!item) return; document.getElementById('a_brand').value=item.brand; document.getElementById('a_model').value=item.model; document.getElementById('a_price').value=item.price; document.getElementById('a_desc').value=item.desc; if(item.image){ document.getElementById('a_preview').src=item.image; document.getElementById('a_preview').style.display='block'; }
      // delete original and allow re-add as new (simple approach)
      deletePhone(id);
    }

    function deletePhone(id){ if(!confirm('Delete this listing?')) return; data = data.filter(d=>d.id!==id); saveData(); renderGrid(); renderAdminList(); }

    // Sell form (public)
    function clearSellForm(){ document.getElementById('s_name').value=''; document.getElementById('s_phoneModel').value=''; document.getElementById('s_price').value=''; document.getElementById('s_desc').value=''; document.getElementById('s_image').value=''; document.getElementById('s_preview').style.display='none'; }
    function submitSellForm(){ const name=document.getElementById('s_name').value.trim(); const model=document.getElementById('s_phoneModel').value.trim(); const price=document.getElementById('s_price').value.trim(); const brand=document.getElementById('s_brand').value; const desc=document.getElementById('s_desc').value.trim(); const img = document.getElementById('s_preview').src||''; if(!name||!model||!price) return alert('Name, model and price are required'); // For now we store public submissions separately
      const sellers = JSON.parse(localStorage.getItem('shivMobiles_sellers_v1')||'[]'); sellers.unshift({id:Date.now(),name,brand,model,price,desc,img,addedAt:Date.now()}); localStorage.setItem('shivMobiles_sellers_v1',JSON.stringify(sellers)); alert('Thanks! We will contact you soon.'); clearSellForm(); }

    // helper: open sell form from nav
    function openSellFormFromNav(){ document.getElementById('s_name').focus(); window.scrollTo({top:document.getElementById('sell').offsetTop-60,behavior:'smooth'}); }
    function scrollToSection(id){ document.getElementById(id).scrollIntoView({behavior:'smooth'}); }

    // small helpers
    function escapeHtml(s){ if(!s) return ''; return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;') }

    // load persisted image if file inputs changed (for admin edit flow)
    document.getElementById('a_image').addEventListener('change',function(){ const f=this.files[0]; if(!f) return; const r=new FileReader(); r.onload=()=>{ document.getElementById('a_preview').src=r.result; document.getElementById('a_preview').style.display='block'; }; r.readAsDataURL(f); });

    // initial render
    renderGrid();

    // expose a window method for debugging
    window._shivData = {get:()=>data,save:saveData};
  </script>
</body>
</html>
