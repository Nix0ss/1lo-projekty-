<!-- ================================================
     PROJEKTY UCZNIÓW 3D — I LO RZESZÓW
     Fragment do wklejenia w WordPress jako "Własny HTML"
     Autor: uczeń I LO Rzeszów
     Wymaga: konta Supabase z wykonanym schema.sql
================================================ -->

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<style>
:root {
  --blue: #1a3a6b; --blue-light: #2563eb; --blue-pale: #eff6ff; --blue-mid: #dbeafe;
  --accent: #f59e0b; --accent-light: #fef3c7; --green: #16a34a; --green-light: #dcfce7;
  --red: #dc2626; --red-light: #fee2e2; --gray-50: #f8fafc; --gray-100: #f1f5f9;
  --gray-200: #e2e8f0; --gray-300: #cbd5e1; --gray-500: #64748b; --gray-700: #334155;
  --gray-900: #0f172a; --shadow: 0 1px 3px rgba(0,0,0,0.1); --shadow-md: 0 4px 6px rgba(0,0,0,0.07);
  --radius: 10px; --radius-sm: 6px;
}
#proj-wrap * { box-sizing: border-box; }
#proj-wrap { font-family: 'Segoe UI', system-ui, sans-serif; color: var(--gray-900); }
.proj-hero { background: linear-gradient(135deg,#1a3a6b,#1e4d9b); color:#fff; padding:2.5rem 1.5rem 2rem; text-align:center; margin-bottom:2rem; border-radius:var(--radius); }
.proj-hero h2 { font-size:1.9rem; font-weight:800; margin:0 0 0.4rem; }
.proj-hero p { opacity:.85; font-size:.95rem; max-width:560px; margin:0 auto 1.2rem; }
.proj-stats { display:flex; gap:2rem; justify-content:center; flex-wrap:wrap; }
.proj-stat .num { font-size:1.8rem; font-weight:800; color:var(--accent); }
.proj-stat .lab { font-size:.75rem; opacity:.8; }
.proj-tabs { display:flex; gap:.5rem; margin-bottom:1.5rem; flex-wrap:wrap; }
.proj-tab { padding:.55rem 1.1rem; border:2px solid var(--gray-200); border-radius:var(--radius-sm); background:#fff; cursor:pointer; font-size:.88rem; font-weight:600; color:var(--gray-700); transition:all .15s; font-family:inherit; }
.proj-tab:hover { border-color:var(--blue-light); color:var(--blue-light); }
.proj-tab.active { background:var(--blue); border-color:var(--blue); color:#fff; }
.proj-actions { display:flex; gap:.75rem; align-items:center; margin-bottom:1.25rem; flex-wrap:wrap; }
.proj-search { flex:1; min-width:200px; padding:.6rem 1rem; border:1.5px solid var(--gray-200); border-radius:var(--radius-sm); font-size:.9rem; font-family:inherit; color:var(--gray-900); outline:none; transition:border-color .15s; }
.proj-search:focus { border-color:var(--blue-light); }
.pbtn { padding:.6rem 1.2rem; border-radius:var(--radius-sm); border:none; cursor:pointer; font-size:.88rem; font-weight:600; font-family:inherit; transition:all .15s; display:inline-flex; align-items:center; gap:.35rem; text-decoration:none; }
.pbtn-primary { background:var(--blue-light); color:#fff; }
.pbtn-primary:hover { background:#1d4ed8; }
.pbtn-accent { background:var(--accent); color:#1a1a1a; }
.pbtn-accent:hover { background:#d97706; }
.pbtn-success { background:var(--green); color:#fff; }
.pbtn-danger { background:var(--red); color:#fff; }
.pbtn-ghost { background:#fff; color:var(--gray-700); border:1.5px solid var(--gray-200); }
.pbtn-ghost:hover { background:var(--gray-100); }
.pbtn-sm { padding:.35rem .75rem; font-size:.8rem; }
.pbtn:disabled { opacity:.5; cursor:not-allowed; }
.proj-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(270px,1fr)); gap:1.25rem; }
.proj-card { background:#fff; border:1.5px solid var(--gray-200); border-radius:var(--radius); overflow:hidden; box-shadow:var(--shadow); transition:transform .15s,box-shadow .15s; display:flex; flex-direction:column; }
.proj-card:hover { transform:translateY(-2px); box-shadow:var(--shadow-md); }
.proj-thumb { height:155px; background:linear-gradient(135deg,var(--blue-pale),var(--blue-mid)); display:flex; align-items:center; justify-content:center; position:relative; overflow:hidden; }
.proj-thumb img { width:100%; height:100%; object-fit:cover; }
.proj-thumb .icon3d { font-size:3rem; opacity:.3; }
.proj-thumb .fext { position:absolute; top:8px; right:8px; background:var(--blue); color:#fff; font-size:.68rem; font-weight:700; padding:2px 7px; border-radius:4px; }
.proj-body { padding:1rem; flex:1; display:flex; flex-direction:column; }
.proj-title { font-size:.95rem; font-weight:700; margin-bottom:.25rem; }
.proj-desc { font-size:.82rem; color:var(--gray-500); line-height:1.5; flex:1; margin-bottom:.65rem; display:-webkit-box; -webkit-line-clamp:3; -webkit-box-orient:vertical; overflow:hidden; }
.proj-meta { font-size:.77rem; color:var(--gray-500); margin-bottom:.65rem; display:flex; gap:.65rem; flex-wrap:wrap; }
.proj-card-actions { display:flex; gap:.5rem; }
.proj-empty { text-align:center; padding:3.5rem 1rem; color:var(--gray-500); grid-column:1/-1; }
.proj-empty .eicon { font-size:2.5rem; margin-bottom:.75rem; }
.proj-loading { text-align:center; padding:2.5rem; color:var(--gray-500); grid-column:1/-1; }
.spinner { width:32px; height:32px; border:3px solid var(--gray-200); border-top-color:var(--blue-light); border-radius:50%; animation:spin .7s linear infinite; margin:0 auto .75rem; }
@keyframes spin { to { transform:rotate(360deg); } }
.pmodal-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,.55); z-index:99999; align-items:center; justify-content:center; padding:1rem; }
.pmodal-overlay.open { display:flex; }
.pmodal { background:#fff; border-radius:var(--radius); max-width:560px; width:100%; max-height:90vh; overflow-y:auto; box-shadow:0 20px 60px rgba(0,0,0,.2); }
.pmodal-hd { padding:1.1rem 1.4rem; border-bottom:1.5px solid var(--gray-200); display:flex; align-items:center; justify-content:space-between; }
.pmodal-hd h3 { font-size:1.05rem; font-weight:700; color:var(--blue); margin:0; }
.pmodal-close { background:none; border:none; font-size:1.3rem; cursor:pointer; color:var(--gray-500); }
.pmodal-body { padding:1.4rem; }
.pmodal-ft { padding:.9rem 1.4rem; border-top:1.5px solid var(--gray-200); display:flex; gap:.65rem; justify-content:flex-end; }
.fg { margin-bottom:1rem; }
.fg label { display:block; font-size:.83rem; font-weight:600; margin-bottom:.3rem; color:var(--gray-700); }
.fg input, .fg textarea { width:100%; padding:.55rem .8rem; box-sizing:border-box; border:1.5px solid var(--gray-200); border-radius:var(--radius-sm); font-size:.88rem; font-family:inherit; color:var(--gray-900); outline:none; transition:border-color .15s; background:#fff; }
.fg input:focus, .fg textarea:focus { border-color:var(--blue-light); }
.fg textarea { resize:vertical; min-height:85px; }
.fg-hint { font-size:.76rem; color:var(--gray-500); margin-top:.25rem; }
.fg-row { display:grid; grid-template-columns:1fr 1fr; gap:.75rem; }
.upload-zone { border:2px dashed var(--gray-300); border-radius:var(--radius); padding:1.5rem; text-align:center; cursor:pointer; background:var(--gray-50); position:relative; transition:all .15s; }
.upload-zone:hover, .upload-zone.drag { border-color:var(--blue-light); background:var(--blue-pale); }
.upload-zone input[type=file] { position:absolute; inset:0; opacity:0; cursor:pointer; }
.upload-zone .uz-icon { font-size:1.8rem; margin-bottom:.35rem; }
.upload-zone p { font-size:.82rem; color:var(--gray-500); margin:0; }
.upload-zone strong { color:var(--blue-light); }
.uz-selected { font-size:.82rem; color:var(--green); font-weight:600; margin-top:.4rem; }
.prog-bar { height:5px; background:var(--gray-200); border-radius:3px; overflow:hidden; margin-top:.4rem; }
.prog-fill { height:100%; background:var(--blue-light); border-radius:3px; transition:width .3s; }
.badge { display:inline-block; padding:2px 8px; border-radius:20px; font-size:.71rem; font-weight:700; }
.badge-pending { background:var(--accent-light); color:#92400e; }
.badge-zatwierdzony { background:var(--green-light); color:#14532d; }
.badge-odrzucony { background:var(--red-light); color:#7f1d1d; }
.adm-tabs { display:flex; border-bottom:2px solid var(--gray-200); margin-bottom:1.25rem; }
.adm-tab { padding:.6rem 1.1rem; border:none; background:none; cursor:pointer; font-size:.88rem; font-weight:600; color:var(--gray-500); border-bottom:2px solid transparent; margin-bottom:-2px; font-family:inherit; }
.adm-tab.active { color:var(--blue); border-bottom-color:var(--blue); }
.adm-card { background:#fff; border:1.5px solid var(--gray-200); border-radius:var(--radius); padding:1.1rem; margin-bottom:.9rem; display:flex; gap:.9rem; box-shadow:var(--shadow); }
.adm-thumb { width:75px; height:75px; background:var(--blue-pale); border-radius:var(--radius-sm); flex-shrink:0; display:flex; align-items:center; justify-content:center; font-size:1.6rem; overflow:hidden; }
.adm-thumb img { width:100%; height:100%; object-fit:cover; }
.adm-info { flex:1; min-width:0; }
.adm-info h4 { font-size:.95rem; font-weight:700; margin:0 0 .2rem; }
.adm-info p { font-size:.8rem; color:var(--gray-500); margin:0 0 .4rem; line-height:1.5; }
.adm-meta { font-size:.76rem; color:var(--gray-500); display:flex; gap:.75rem; flex-wrap:wrap; margin-bottom:.6rem; }
.adm-actions { display:flex; gap:.4rem; flex-wrap:wrap; }
.adm-login { max-width:360px; margin:3rem auto; background:#fff; border-radius:var(--radius); padding:1.75rem; box-shadow:var(--shadow-md); border:1.5px solid var(--gray-200); }
.adm-login h3 { font-size:1.1rem; font-weight:700; color:var(--blue); margin:0 0 1.25rem; text-align:center; }
.ptoast { position:fixed; bottom:1.5rem; right:1.5rem; background:var(--gray-900); color:#fff; padding:.8rem 1.1rem; border-radius:var(--radius); font-size:.88rem; font-weight:500; z-index:999999; opacity:0; transform:translateY(10px); transition:all .25s; max-width:300px; pointer-events:none; }
.ptoast.show { opacity:1; transform:translateY(0); }
.ptoast.ok { background:var(--green); }
.ptoast.err { background:var(--red); }
.info-box { background:var(--accent-light); border:1.5px solid #f59e0b; border-radius:var(--radius); padding:.9rem 1.1rem; margin-bottom:1.25rem; font-size:.86rem; color:#92400e; }
.det-thumb { width:100%; height:190px; background:var(--blue-pale); border-radius:var(--radius-sm); display:flex; align-items:center; justify-content:center; font-size:3.5rem; margin-bottom:1rem; overflow:hidden; }
.det-thumb img { width:100%; height:100%; object-fit:contain; }
.det-row { display:flex; justify-content:space-between; padding:.45rem 0; border-bottom:1px solid var(--gray-100); font-size:.86rem; }
.det-row:last-child { border-bottom:none; }
.det-label { color:var(--gray-500); font-weight:600; }
@media(max-width:600px) { .proj-grid{grid-template-columns:1fr;} .adm-card{flex-direction:column;} .adm-thumb{width:100%;height:100px;} .fg-row{grid-template-columns:1fr;} }
</style>

<div id="proj-wrap">

  <!-- HERO -->
  <div class="proj-hero">
    <h2>🖨 Biblioteka projektów 3D</h2>
    <p>Przeglądaj, pobieraj i drukuj projekty tworzone przez uczniów I LO Rzeszów na szkolnej drukarce 3D.</p>
    <div class="proj-stats">
      <div class="proj-stat"><div class="num" id="p3d-statCount">–</div><div class="lab">projektów</div></div>
      <div class="proj-stat"><div class="num" id="p3d-statAuthors">–</div><div class="lab">autorów</div></div>
      <div class="proj-stat"><div class="num" id="p3d-statClasses">–</div><div class="lab">klas</div></div>
    </div>
  </div>

  <!-- ZAKŁADKI -->
  <div class="proj-tabs">
    <button class="proj-tab active" onclick="p3d_switch('gallery')">🗂 Galeria projektów</button>
    <button class="proj-tab" onclick="p3d_switch('add')">➕ Dodaj projekt</button>
    <button class="proj-tab" onclick="p3d_switch('admin')">🔒 Panel admina</button>
  </div>

  <!-- GALERIA -->
  <div id="p3d-gallery">
    <div class="proj-actions">
      <input class="proj-search" type="text" id="p3d-search" placeholder="🔍 Szukaj projektu, autora, klasy…" oninput="p3d_filter()">
      <button class="pbtn pbtn-accent" onclick="p3d_switch('add')">➕ Dodaj projekt</button>
    </div>
    <div id="p3d-grid" class="proj-grid">
      <div class="proj-loading"><div class="spinner"></div>Ładowanie…</div>
    </div>
  </div>

  <!-- DODAJ -->
  <div id="p3d-add" style="display:none">
    <div class="info-box">⏳ <strong>Weryfikacja:</strong> Projekt trafi do kolejki. Administrator sprawdzi go i opublikuje lub poprosi o poprawki. Czas oczekiwania: 1–3 dni robocze.</div>
    <div style="background:#fff;border:1.5px solid var(--gray-200);border-radius:var(--radius);padding:1.5rem;box-shadow:var(--shadow);">
      <div class="fg"><label>Nazwa projektu *</label><input type="text" id="p3d-nazwa" placeholder="np. Uchwyt na długopisy" maxlength="80"></div>
      <div class="fg"><label>Opis projektu *</label><textarea id="p3d-opis" placeholder="Do czego służy, jak był zaprojektowany, jakie ustawienia druku polecasz…"></textarea></div>
      <div class="fg-row">
        <div class="fg"><label>Imię i nazwisko *</label><input type="text" id="p3d-autor" placeholder="Jan Kowalski"></div>
        <div class="fg"><label>Klasa *</label><input type="text" id="p3d-klasa" placeholder="np. 2B"></div>
      </div>
      <div class="fg">
        <label>Plik 3D (STL, OBJ, 3MF) *</label>
        <div class="upload-zone" id="p3d-drop3d">
          <input type="file" id="p3d-plik" accept=".stl,.obj,.3mf" onchange="p3d_h3d(this)">
          <div class="uz-icon">📦</div>
          <p><strong>Kliknij lub przeciągnij plik</strong> – STL, OBJ, 3MF</p>
          <div class="uz-selected" id="p3d-name3d" style="display:none"></div>
        </div>
        <div class="fg-hint">Max. 50 MB</div>
      </div>
      <div class="fg">
        <label>Zdjęcie / miniatura (opcjonalnie)</label>
        <div class="upload-zone" id="p3d-dropT">
          <input type="file" id="p3d-thumb" accept="image/*" onchange="p3d_hT(this)">
          <div class="uz-icon">🖼</div>
          <p><strong>Kliknij lub przeciągnij zdjęcie</strong> – JPG, PNG, WEBP</p>
          <div class="uz-selected" id="p3d-nameT" style="display:none"></div>
        </div>
      </div>
      <div id="p3d-prog" style="display:none;margin-bottom:1rem;">
        <div style="font-size:.83rem;color:var(--gray-500);margin-bottom:.25rem;" id="p3d-progL">Przesyłanie…</div>
        <div class="prog-bar"><div class="prog-fill" id="p3d-progF" style="width:0%"></div></div>
      </div>
      <div style="display:flex;gap:.65rem;justify-content:flex-end;">
        <button class="pbtn pbtn-ghost" onclick="p3d_switch('gallery')">Anuluj</button>
        <button class="pbtn pbtn-primary" id="p3d-submitBtn" onclick="p3d_submit()">📤 Wyślij do weryfikacji</button>
      </div>
    </div>
  </div>

  <!-- ADMIN -->
  <div id="p3d-admin" style="display:none">
    <div id="p3d-login">
      <div class="adm-login">
        <h3>🔒 Panel Administratora</h3>
        <div class="fg"><label>Hasło</label><input type="password" id="p3d-pass" placeholder="••••••••" onkeydown="if(event.key==='Enter')p3d_login()"></div>
        <button class="pbtn pbtn-primary" style="width:100%" onclick="p3d_login()">Zaloguj się</button>
      </div>
    </div>
    <div id="p3d-dash" style="display:none">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.25rem;flex-wrap:wrap;gap:.5rem;">
        <strong style="font-size:1.05rem;color:var(--blue);">Panel Administratora</strong>
        <button class="pbtn pbtn-ghost pbtn-sm" onclick="p3d_logout()">Wyloguj</button>
      </div>
      <div class="adm-tabs">
        <button class="adm-tab active" onclick="p3d_admTab('oczekuje')">⏳ Oczekujące <span id="p3d-pending" style="background:var(--accent);color:#1a1a1a;border-radius:10px;padding:1px 6px;font-size:.72rem;margin-left:3px;"></span></button>
        <button class="adm-tab" onclick="p3d_admTab('zatwierdzony')">✅ Zatwierdzone</button>
        <button class="adm-tab" onclick="p3d_admTab('odrzucony')">❌ Odrzucone</button>
      </div>
      <div id="p3d-admList"></div>
    </div>
  </div>

</div><!-- /proj-wrap -->

<!-- MODALS -->
<div class="pmodal-overlay" id="p3d-viewModal">
  <div class="pmodal">
    <div class="pmodal-hd"><h3 id="p3d-mTitle">Projekt</h3><button class="pmodal-close" onclick="p3d_closeM('p3d-viewModal')">✕</button></div>
    <div class="pmodal-body" id="p3d-mBody"></div>
    <div class="pmodal-ft" id="p3d-mFt"></div>
  </div>
</div>
<div class="pmodal-overlay" id="p3d-rejectModal">
  <div class="pmodal" style="max-width:400px">
    <div class="pmodal-hd"><h3>❌ Odrzuć projekt</h3><button class="pmodal-close" onclick="p3d_closeM('p3d-rejectModal')">✕</button></div>
    <div class="pmodal-body">
      <p style="font-size:.88rem;margin-bottom:.9rem;color:var(--gray-700);">Podaj powód odrzucenia (uczeń zobaczy tę informację):</p>
      <textarea id="p3d-rejectR" style="width:100%;padding:.55rem;border:1.5px solid var(--gray-200);border-radius:var(--radius-sm);font-family:inherit;font-size:.88rem;min-height:90px;resize:vertical;color:var(--gray-900);box-sizing:border-box;" placeholder="np. Plik jest uszkodzony. Proszę przesłać w formacie STL."></textarea>
    </div>
    <div class="pmodal-ft">
      <button class="pbtn pbtn-ghost" onclick="p3d_closeM('p3d-rejectModal')">Anuluj</button>
      <button class="pbtn pbtn-danger" onclick="p3d_confirmReject()">Odrzuć projekt</button>
    </div>
  </div>
</div>
<div class="ptoast" id="p3d-toast"></div>

<script>
// ════════════════════════════════════════════════════════
//  KONFIGURACJA — zmień te dwie wartości na dane Supabase
// ════════════════════════════════════════════════════════
const P3D_URL  = 'https://kdweomrlxtxmuydlqbwr.supabase.co';
const P3D_KEY  = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Imtkd2VvbXJseHR4bXV5ZGxxYndyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODEzNjAwNTgsImV4cCI6MjA5NjkzNjA1OH0.qQF1fODevj-n2PktM2NNaCq6yXpMOlsvoypfPTlmGEw';
const P3D_PASS = 'admin1lo2024'; // ← ZMIEŃ NA WŁASNE HASŁO ADMINA
// ════════════════════════════════════════════════════════

const _sb = supabase.createClient(P3D_URL, P3D_KEY);
let _projects=[], _admTab='oczekuje', _rejectId=null, _adminOk=false;

function p3d_switch(v){
  ['gallery','add','admin'].forEach(n=>document.getElementById('p3d-'+n).style.display=n===v?'block':'none');
  document.querySelectorAll('#proj-wrap .proj-tab').forEach((b,i)=>b.classList.toggle('active',['gallery','add','admin'][i]===v));
  if(v==='gallery') p3d_load();
}
function p3d_toast(m,t=''){const el=document.getElementById('p3d-toast');el.textContent=m;el.className='ptoast show '+t;setTimeout(()=>el.className='ptoast',3400);}
function p3d_esc(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function p3d_ext(n){return n.split('.').pop().toUpperCase();}
function p3d_fd(s){return new Date(s).toLocaleDateString('pl-PL',{day:'numeric',month:'short',year:'numeric'});}
function p3d_closeM(id){document.getElementById(id).classList.remove('open');}
document.querySelectorAll('.pmodal-overlay').forEach(el=>el.addEventListener('click',e=>{if(e.target===el)el.classList.remove('open');}));

async function p3d_load(){
  document.getElementById('p3d-grid').innerHTML='<div class="proj-loading"><div class="spinner"></div>Ładowanie…</div>';
  const{data,error}=await _sb.from('projekty').select('*').eq('status','zatwierdzony').order('created_at',{ascending:false});
  if(error){document.getElementById('p3d-grid').innerHTML='<div class="proj-empty" style="grid-column:1/-1"><div class="eicon">⚠️</div><p>Błąd: '+error.message+'</p></div>';return;}
  _projects=data||[];
  document.getElementById('p3d-statCount').textContent=_projects.length;
  document.getElementById('p3d-statAuthors').textContent=new Set(_projects.map(p=>p.autor)).size;
  document.getElementById('p3d-statClasses').textContent=new Set(_projects.map(p=>p.klasa)).size;
  p3d_render(_projects);
}
function p3d_filter(){const q=document.getElementById('p3d-search').value.toLowerCase();p3d_render(_projects.filter(p=>p.nazwa.toLowerCase().includes(q)||p.autor.toLowerCase().includes(q)||p.klasa.toLowerCase().includes(q)||p.opis.toLowerCase().includes(q)));}
function p3d_render(list){
  const g=document.getElementById('p3d-grid');
  if(!list.length){g.innerHTML='<div class="proj-empty"><div class="eicon">📭</div><p>Brak projektów. Bądź pierwszy!</p></div>';return;}
  g.innerHTML=list.map(p=>`<div class="proj-card"><div class="proj-thumb">${p.miniatura_url?`<img src="${p.miniatura_url}" alt="${p3d_esc(p.nazwa)}" loading="lazy">`:'<div class="icon3d">📦</div>'}<span class="fext">${p3d_ext(p.plik_nazwa)}</span></div><div class="proj-body"><div class="proj-title">${p3d_esc(p.nazwa)}</div><div class="proj-desc">${p3d_esc(p.opis)}</div><div class="proj-meta"><span>👤 ${p3d_esc(p.autor)}</span><span>🏫 ${p3d_esc(p.klasa)}</span><span>📅 ${p3d_fd(p.created_at)}</span></div><div class="proj-card-actions"><button class="pbtn pbtn-ghost pbtn-sm" onclick="p3d_detail('${p.id}')">👁 Szczegóły</button><a href="${p.plik_url}" download="${p3d_esc(p.plik_nazwa)}" class="pbtn pbtn-primary pbtn-sm">⬇ Pobierz</a></div></div></div>`).join('');
}
function p3d_detail(id){
  const p=_projects.find(x=>x.id===id);if(!p)return;
  document.getElementById('p3d-mTitle').textContent=p.nazwa;
  document.getElementById('p3d-mBody').innerHTML=`<div class="det-thumb">${p.miniatura_url?`<img src="${p.miniatura_url}" alt="">`:'📦'}</div><p style="font-size:.9rem;line-height:1.7;color:var(--gray-700);margin-bottom:1rem;">${p3d_esc(p.opis)}</p><div class="det-row"><span class="det-label">Autor</span><span>${p3d_esc(p.autor)}</span></div><div class="det-row"><span class="det-label">Klasa</span><span>${p3d_esc(p.klasa)}</span></div><div class="det-row"><span class="det-label">Format</span><span>${p3d_ext(p.plik_nazwa)}</span></div><div class="det-row"><span class="det-label">Plik</span><span style="font-size:.8rem;word-break:break-all">${p3d_esc(p.plik_nazwa)}</span></div><div class="det-row"><span class="det-label">Dodano</span><span>${p3d_fd(p.created_at)}</span></div>`;
  document.getElementById('p3d-mFt').innerHTML=`<button class="pbtn pbtn-ghost" onclick="p3d_closeM('p3d-viewModal')">Zamknij</button><a href="${p.plik_url}" download="${p3d_esc(p.plik_nazwa)}" class="pbtn pbtn-primary">⬇ Pobierz ${p3d_ext(p.plik_nazwa)}</a>`;
  document.getElementById('p3d-viewModal').classList.add('open');
}
function p3d_h3d(i){const f=i.files[0];if(!f)return;const el=document.getElementById('p3d-name3d');el.textContent='✅ '+f.name+' ('+(f.size/1024/1024).toFixed(1)+' MB)';el.style.display='block';}
function p3d_hT(i){const f=i.files[0];if(!f)return;const el=document.getElementById('p3d-nameT');el.textContent='✅ '+f.name;el.style.display='block';}
['p3d-drop3d','p3d-dropT'].forEach(id=>{const el=document.getElementById(id);el.addEventListener('dragover',e=>{e.preventDefault();el.classList.add('drag');});el.addEventListener('dragleave',()=>el.classList.remove('drag'));el.addEventListener('drop',e=>{e.preventDefault();el.classList.remove('drag');});});
function p3d_setP(pct,lbl){document.getElementById('p3d-progF').style.width=pct+'%';document.getElementById('p3d-progL').textContent=lbl;}
async function p3d_submit(){
  const nazwa=document.getElementById('p3d-nazwa').value.trim(),opis=document.getElementById('p3d-opis').value.trim(),autor=document.getElementById('p3d-autor').value.trim(),klasa=document.getElementById('p3d-klasa').value.trim(),plik3d=document.getElementById('p3d-plik').files[0],thumb=document.getElementById('p3d-thumb').files[0];
  if(!nazwa||!opis||!autor||!klasa){p3d_toast('❗ Wypełnij wszystkie wymagane pola.','err');return;}
  if(!plik3d){p3d_toast('❗ Wybierz plik 3D.','err');return;}
  if(plik3d.size>50*1024*1024){p3d_toast('❗ Plik za duży (max 50 MB).','err');return;}
  const btn=document.getElementById('p3d-submitBtn');btn.disabled=true;btn.textContent='⏳ Przesyłanie…';document.getElementById('p3d-prog').style.display='block';
  try{
    const ts=Date.now(),e3=plik3d.name.split('.').pop(),path3=`pliki/${ts}_${Math.random().toString(36).slice(2)}.${e3}`;
    p3d_setP(10,'Przesyłanie pliku 3D…');
    const{error:e1}=await _sb.storage.from('projekty3d').upload(path3,plik3d);if(e1)throw e1;
    const{data:u1}=_sb.storage.from('projekty3d').getPublicUrl(path3);
    let miniatura_url=null;
    if(thumb){p3d_setP(50,'Przesyłanie miniatury…');const eT=thumb.name.split('.').pop(),pT=`miniatury/${ts}_${Math.random().toString(36).slice(2)}.${eT}`;const{error:e2}=await _sb.storage.from('projekty3d').upload(pT,thumb);if(!e2){const{data:u2}=_sb.storage.from('projekty3d').getPublicUrl(pT);miniatura_url=u2.publicUrl;}}
    p3d_setP(80,'Zapisywanie…');
    const{error:e3}=await _sb.from('projekty').insert({nazwa,opis,autor,klasa,plik_nazwa:plik3d.name,plik_url:u1.publicUrl,miniatura_url,status:'oczekuje'});if(e3)throw e3;
    p3d_setP(100,'Gotowe!');p3d_toast('✅ Projekt wysłany! Czeka na weryfikację.','ok');
    setTimeout(()=>{['p3d-nazwa','p3d-opis','p3d-autor','p3d-klasa'].forEach(i=>document.getElementById(i).value='');document.getElementById('p3d-plik').value='';document.getElementById('p3d-thumb').value='';document.getElementById('p3d-name3d').style.display='none';document.getElementById('p3d-nameT').style.display='none';document.getElementById('p3d-prog').style.display='none';btn.disabled=false;btn.textContent='📤 Wyślij do weryfikacji';p3d_switch('gallery');},1500);
  }catch(err){p3d_toast('❌ Błąd: '+(err.message||JSON.stringify(err)),'err');btn.disabled=false;btn.textContent='📤 Wyślij do weryfikacji';document.getElementById('p3d-prog').style.display='none';}
}
function p3d_login(){if(document.getElementById('p3d-pass').value===P3D_PASS){_adminOk=true;document.getElementById('p3d-login').style.display='none';document.getElementById('p3d-dash').style.display='block';p3d_loadAdm();}else p3d_toast('❌ Błędne hasło.','err');}
function p3d_logout(){_adminOk=false;document.getElementById('p3d-login').style.display='block';document.getElementById('p3d-dash').style.display='none';document.getElementById('p3d-pass').value='';}
function p3d_admTab(tab){_admTab=tab;document.querySelectorAll('#proj-wrap .adm-tab').forEach((b,i)=>b.classList.toggle('active',['oczekuje','zatwierdzony','odrzucony'][i]===tab));p3d_loadAdm();}
async function p3d_loadAdm(){
  document.getElementById('p3d-admList').innerHTML='<div class="proj-loading"><div class="spinner"></div>Ładowanie…</div>';
  const{data,error}=await _sb.from('projekty').select('*').eq('status',_admTab).order('created_at',{ascending:_admTab==='oczekuje'});
  const{count}=await _sb.from('projekty').select('*',{count:'exact',head:true}).eq('status','oczekuje');
  document.getElementById('p3d-pending').textContent=count||'';
  if(error||!data||!data.length){document.getElementById('p3d-admList').innerHTML='<div class="proj-empty"><div class="eicon">📭</div><p>Brak projektów.</p></div>';return;}
  document.getElementById('p3d-admList').innerHTML=data.map(p=>`<div class="adm-card"><div class="adm-thumb">${p.miniatura_url?`<img src="${p.miniatura_url}" alt="">`:'📦'}</div><div class="adm-info"><h4>${p3d_esc(p.nazwa)} <span class="badge badge-${p.status==='oczekuje'?'pending':p.status}">${p.status==='oczekuje'?'Oczekuje':p.status==='zatwierdzony'?'Zatwierdzony':'Odrzucony'}</span></h4><p>${p3d_esc(p.opis)}</p><div class="adm-meta"><span>👤 ${p3d_esc(p.autor)}</span><span>🏫 ${p3d_esc(p.klasa)}</span><span>📁 ${p3d_esc(p.plik_nazwa)}</span><span>📅 ${p3d_fd(p.created_at)}</span></div>${p.powod_odrzucenia?`<div style="font-size:.8rem;color:var(--red);margin-bottom:.5rem;">Powód: ${p3d_esc(p.powod_odrzucenia)}</div>`:''}<div class="adm-actions"><a href="${p.plik_url}" target="_blank" class="pbtn pbtn-ghost pbtn-sm">📥 Pobierz</a>${p.status!=='zatwierdzony'?`<button class="pbtn pbtn-success pbtn-sm" onclick="p3d_approve('${p.id}')">✅ Zatwierdź</button>`:''}${p.status!=='odrzucony'?`<button class="pbtn pbtn-danger pbtn-sm" onclick="p3d_openReject('${p.id}')">❌ Odrzuć</button>`:''}<button class="pbtn pbtn-ghost pbtn-sm" onclick="p3d_del('${p.id}')" style="color:var(--red)">🗑 Usuń</button></div></div></div>`).join('');
}
async function p3d_approve(id){const{error}=await _sb.from('projekty').update({status:'zatwierdzony',powod_odrzucenia:null}).eq('id',id);if(error){p3d_toast('❌ '+error.message,'err');return;}p3d_toast('✅ Zatwierdzono!','ok');p3d_loadAdm();}
function p3d_openReject(id){_rejectId=id;document.getElementById('p3d-rejectR').value='';document.getElementById('p3d-rejectModal').classList.add('open');}
async function p3d_confirmReject(){const r=document.getElementById('p3d-rejectR').value.trim();if(!r){p3d_toast('❗ Podaj powód.','err');return;}const{error}=await _sb.from('projekty').update({status:'odrzucony',powod_odrzucenia:r}).eq('id',_rejectId);if(error){p3d_toast('❌ '+error.message,'err');return;}p3d_closeM('p3d-rejectModal');p3d_toast('Odrzucono.');p3d_loadAdm();}
async function p3d_del(id){if(!confirm('Na pewno usunąć?'))return;const{error}=await _sb.from('projekty').delete().eq('id',id);if(error){p3d_toast('❌ '+error.message,'err');return;}p3d_toast('🗑 Usunięto.');p3d_loadAdm();}

p3d_load();
</script>
