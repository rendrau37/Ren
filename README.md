# Ren
Teka teki
<!doctype html>
<html lang="id">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kuis Teka-Teki</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&amp;display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    body { font-family: 'DM Sans', sans-serif; }
    .theme-dark { --bg: transparent; --card: rgba(30,41,59,0.7); --border: #334155; --text: #f1f5f9; --muted: #94a3b8; --accent: #3b82f6; --accent2: #8b5cf6; }
    .theme-light { --bg: transparent; --card: rgba(255,255,255,0.9); --border: #e2e8f0; --text: #1e293b; --muted: #64748b; --accent: #2563eb; --accent2: #7c3aed; }
    .theme-neon { --bg: transparent; --card: rgba(10,10,30,0.85); --border: #39ff14; --text: #39ff14; --muted: #00ffff; --accent: #ff00ff; --accent2: #39ff14; }
    .theme-pastel { --bg: transparent; --card: rgba(255,240,245,0.85); --border: #f9a8d4; --text: #4c1d95; --muted: #7c3aed; --accent: #ec4899; --accent2: #8b5cf6; }
    
    .option-btn { transition: all 0.2s; background: var(--card); border-color: var(--border); color: var(--text); }
    .option-btn:hover:not(.disabled) { transform: translateY(-2px); border-color: var(--accent); }
    .option-btn:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
    .option-btn.correct { border-color: #22c55e!important; background: rgba(34,197,94,0.2)!important; }
    .option-btn.wrong { border-color: #ef4444!important; background: rgba(239,68,68,0.2)!important; }
    .option-btn.disabled { pointer-events: none; opacity: 0.7; }
    
    .timer-bar { transition: width 0.1s linear; }
    @keyframes shake { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-4px)} 75%{transform:translateX(4px)} }
    .shake { animation: shake 0.3s; }
    @keyframes pulse-green { 0%,100%{box-shadow:0 0 0 0 rgba(34,197,94,0.4)} 50%{box-shadow:0 0 0 12px rgba(34,197,94,0)} }
    .pulse-correct { animation: pulse-green 0.5s; }
    @keyframes fadeSlideIn { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }
    @keyframes fadeSlideOut { from{opacity:1;transform:translateY(0)} to{opacity:0;transform:translateY(-10px)} }
    .page-enter { animation: fadeSlideIn 0.35s ease-out forwards; }
    .page-exit { animation: fadeSlideOut 0.2s ease-in forwards; }
    
    .xp-bar { background: rgba(59,130,246,0.2); border-radius: 9999px; overflow: hidden; }
    .xp-fill { background: linear-gradient(90deg,var(--accent),var(--accent2)); height: 100%; transition: width 0.5s; }
    .explanation-box { background: rgba(59,130,246,0.1); border: 1px solid rgba(59,130,246,0.3); border-radius: 0.75rem; padding: 1rem; }
    .chart-bar { background: linear-gradient(180deg,var(--accent),var(--accent2)); border-radius: 4px 4px 0 0; transition: height 0.5s; }
    
    @keyframes confetti-fall { 0%{transform:translateY(-100%) rotate(0deg);opacity:1} 100%{transform:translateY(100vh) rotate(720deg);opacity:0} }
    .confetti-piece { position: fixed; top: -10px; width: 10px; height: 10px; animation: confetti-fall 3s ease-out forwards; pointer-events: none; z-index: 9999; }
    @keyframes levelUp { 0%{transform:scale(0.5);opacity:0} 50%{transform:scale(1.3);opacity:1} 100%{transform:scale(1);opacity:1} }
    .level-up-anim { animation: levelUp 0.6s ease-out; }
    @keyframes achievePop { 0%{transform:scale(0) rotate(-10deg)} 60%{transform:scale(1.2) rotate(5deg)} 100%{transform:scale(1) rotate(0deg)} }
    .achieve-pop { animation: achievePop 0.5s ease-out; }
    
    .mode-card { transition: all 0.2s; }
    .mode-card:hover { transform: translateY(-3px); }
    .mode-card.selected { border-color: var(--accent)!important; background: rgba(59,130,246,0.15)!important; }
    .review-correct { border-left: 4px solid #22c55e; }
    .review-wrong { border-left: 4px solid #ef4444; }
    .theme-btn { transition: all 0.15s; }
    .theme-btn.active { outline: 2px solid var(--accent); outline-offset: 2px; }
    .toast { position: fixed; bottom: 2rem; left: 50%; transform: translateX(-50%); background: var(--accent); color: #fff; padding: 0.75rem 1.5rem; border-radius: 0.75rem; font-weight: 600; z-index: 9999; animation: fadeSlideIn 0.3s ease-out; }
    
    [hidden] { display: none!important; }
    .font-scale-sm { font-size: 14px; }
    .font-scale-md { font-size: 16px; }
    .font-scale-lg { font-size: 18px; }
    .font-scale-xl { font-size: 20px; }
  </style>
 </head>
 <body class="theme-dark font-scale-md w-full min-h-screen flex items-center justify-center p-3 sm:p-6" style="background: linear-gradient(135deg, rgb(10, 10, 10), rgb(15, 23, 42), rgb(30, 27, 75));">

  <!-- Welcome Page -->
  <main id="welcome-page" class="w-full max-w-md mx-auto text-center space-y-6 flex flex-col justify-center" role="region" aria-label="Halaman utama">
   <h1 class="text-3xl sm:text-4xl font-bold text-white">🧠 Kuis Teka-Teki</h1>
   <p class="text-base sm:text-lg text-blue-300">200+ soal, 6 mode, daily challenge, koin &amp; badge!</p>
   <div id="coins-display" class="flex items-center justify-center gap-2 text-lg font-bold" style="color:#fbbf24" hidden><span>🪙</span><span id="coins-text">0</span></div>
   <div id="xp-display" class="space-y-1" hidden>
    <div class="flex justify-between text-sm"><span id="level-text" class="font-bold" style="color:var(--accent)"></span><span id="xp-text" style="color:var(--muted)"></span></div>
    <div class="xp-bar h-3"><div id="xp-fill" class="xp-fill" style="width:0%"></div></div>
   </div>
   <form id="name-form" class="space-y-4 text-left">
    <div class="space-y-2">
     <label for="player-name" class="block text-sm font-medium text-slate-200">Masukkan nama kamu:</label> 
     <input id="player-name" class="w-full px-4 py-3 rounded-lg border focus:outline-none focus:ring-2 focus:ring-blue-500 text-white bg-slate-800" style="border-color: var(--border);" type="text" autocomplete="name" required placeholder="Nama kamu...">
    </div>
    <div class="space-y-2">
     <label for="player-pin" class="block text-sm font-medium text-slate-200">PIN 4 digit:</label> 
     <input id="player-pin" class="w-full px-4 py-3 rounded-lg border focus:outline-none focus:ring-2 focus:ring-blue-500 text-white bg-slate-800" style="border-color: var(--border);" type="password" inputmode="numeric" pattern="[0-9]{4}" maxlength="4" minlength="4" autocomplete="off" required placeholder="Buat atau masukkan PIN...">
    </div>
    <p class="text-xs text-slate-400">PIN melindungi progres kamu.</p>
    <button type="submit" class="w-full py-3 rounded-lg font-bold text-lg bg-blue-600 text-white hover:bg-blue-700 transition">Mulai Kuis →</button>
   </form>
   <div class="flex gap-2 flex-wrap">
     <button id="show-leaderboard-welcome" type="button" class="flex-1 py-3 rounded-lg font-bold text-sm bg-slate-700 text-amber-400 hover:bg-slate-600 transition">🏆 Peringkat</button> 
     <button id="show-stats-welcome" type="button" class="flex-1 py-3 rounded-lg font-bold text-sm bg-slate-700 text-purple-400 hover:bg-slate-600 transition">📊 Statistik</button> 
     <button id="show-settings-welcome" type="button" class="flex-1 py-3 rounded-lg font-bold text-sm bg-slate-700 text-slate-300 hover:bg-slate-600 transition">⚙️ Pengaturan</button>
   </div>
   <div id="autosave-banner" class="rounded-lg p-3 border text-sm font-medium" style="background:rgba(234,179,8,0.15);border-color:rgba(234,179,8,0.4);color:#fbbf24" hidden>
    ⏸️ Ada permainan tersimpan! <button id="resume-btn" type="button" class="underline ml-1 font-bold">Lanjutkan</button> atau <button id="discard-btn" type="button" class="underline ml-1">Mulai baru</button>
   </div>
  </main>

  <!-- Mode Selection Page -->
  <main id="mode-page" class="w-full max-w-md text-center space-y-5" hidden role="region" aria-label="Pilih mode">
   <h2 class="text-2xl sm:text-3xl font-bold text-white">Pilih Mode</h2>
   <p class="text-sm text-blue-300">Setiap mode punya tantangan berbeda!</p>
   <div id="mode-buttons" class="grid grid-cols-2 gap-3"></div>
   <button id="mode-next-btn" type="button" class="w-full py-3 rounded-lg font-bold text-lg bg-blue-600 text-white hover:bg-blue-700 transition">Lanjut →</button>
  </main>

  <!-- Custom Mode Config -->
  <main id="custom-page" class="w-full max-w-md text-center space-y-5" hidden role="region" aria-label="Konfigurasi mode kustom">
   <h2 class="text-2xl font-bold text-white">⚙️ Mode Kustom</h2>
   <div class="space-y-4 text-left">
    <div class="rounded-lg p-4 border" style="background:var(--card);border-color:var(--border)">
      <label for="custom-count" class="block text-sm font-medium mb-1" style="color:var(--text)">Jumlah Soal</label> 
      <input id="custom-count" type="number" min="5" max="50" value="10" class="w-full px-3 py-2 rounded-lg border" style="background:var(--card);border-color:var(--border);color:var(--text)">
    </div>
    <div class="rounded-lg p-4 border" style="background:var(--card);border-color:var(--border)">
      <label for="custom-time" class="block text-sm font-medium mb-1" style="color:var(--text)">Waktu per Soal (detik)</label> 
      <input id="custom-time" type="number" min="3" max="60" value="15" class="w-full px-3 py-2 rounded-lg border" style="background:var(--card);border-color:var(--border);color:var(--text)">
    </div>
    <div class="rounded-lg p-4 border" style="background:var(--card);border-color:var(--border)">
      <label for="custom-diff" class="block text-sm font-medium mb-1" style="color:var(--text)">Kesulitan</label> 
      <select id="custom-diff" class="w-full px-3 py-2 rounded-lg border" style="background:var(--card);border-color:var(--border);color:var(--text)"> 
        <option value="0">Semua</option>
        <option value="1">Mudah</option>
        <option value="2">Sedang</option>
        <option value="3">Sulit</option> 
      </select>
    </div>
   </div>
   <button id="custom-start-btn" type="button" class="w-full py-3 rounded-lg font-bold text-lg bg-blue-600 text-white hover:bg-blue-700 transition">Mulai Custom →</button>
  </main>

  <!-- Category Page -->
  <main id="category-page" class="w-full max-w-md text-center space-y-5" hidden role="region" aria-label="Pilih kategori">
   <h2 class="text-2xl sm:text-3xl font-bold text-white">Pilih Kategori</h2>
   <p class="text-sm text-blue-300">Kesulitan menyesuaikan performa kamu otomatis 🧠</p>
   <input id="category-filter" type="text" class="w-full px-4 py-2 rounded-lg border text-sm focus:outline-none focus:ring-2" style="background:var(--card);border-color:var(--border);color:var(--text)" placeholder="🔍 Cari kategori..." aria-label="Filter kategori">
   <div id="category-buttons" class="grid grid-cols-2 gap-3"></div>
   <button id="category-start-btn" type="button" class="w-full py-3 rounded-lg font-bold text-lg bg-blue-600 text-white hover:bg-blue-700 transition">Mulai! →</button>
  </main>

  <!-- Quiz Page -->
  <main id="quiz-page" class="w-full max-w-2xl space-y-4" hidden role="region" aria-label="Kuis">
   <header class="flex justify-between items-center flex-wrap gap-2">
     <span id="category-label" class="px-3 py-1 rounded-full text-xs sm:text-sm font-medium bg-blue-950 text-blue-400">Kategori</span> 
     <span id="question-counter" class="text-sm font-medium" style="color:var(--muted)"></span>
   </header>
   <div class="flex items-center gap-3 text-sm">
     <span id="streak-display" class="text-orange-400 font-bold" hidden>🔥 0</span> 
     <span id="difficulty-badge" class="px-2 py-0.5 rounded-full text-xs font-medium"></span> 
     <span id="lives-display" class="text-red-400 font-bold" hidden>❤️❤️❤️</span>
   </div>
   <div id="timer-container" class="w-full h-2 rounded-full overflow-hidden" style="background:var(--border)">
    <div id="timer-bar" class="timer-bar h-full bg-gradient-to-r from-green-400 to-emerald-500 rounded-full" style="width:100%"></div>
   </div>
   <div class="flex justify-between text-xs" style="color:var(--muted)">
    <span id="timer-text">15s</span>
   </div>
   <h2 id="question-text" class="text-lg sm:text-xl font-semibold leading-relaxed" style="color:var(--text)"></h2>
   <div id="options-container" class="grid gap-3" role="group" aria-label="Pilihan jawaban"></div>
   <div id="explanation-box" class="explanation-box" hidden>
    <p class="text-sm font-bold mb-1" style="color:var(--accent)">📜 Penjelasan:</p>
    <p id="explanation-text" class="text-sm" style="color:var(--text)"></p>
   </div>
   <div id="xp-gain" class="text-center text-emerald-400 font-bold text-lg" hidden></div>
   <button id="next-btn" class="w-full py-3 rounded-lg font-bold text-lg transition" style="background:var(--accent);color:#fff" hidden>Selanjutnya →</button>
  </main>

  <!-- Review Page -->
  <main id="review-page" class="w-full max-w-2xl space-y-4" hidden role="region" aria-label="Review jawaban">
   <h2 class="text-2xl font-bold text-center text-white">📝 Review Jawaban</h2>
   <div id="review-list" class="space-y-3 max-h-[60vh] overflow-y-auto pr-2"></div>
   <button id="review-done-btn" type="button" class="w-full py-3 rounded-lg font-bold text-lg bg-slate-700 text-white hover:bg-slate-600 transition">← Kembali ke Hasil</button>
  </main>

  <!-- Results Page -->
  <main id="results-page" class="w-full max-w-md text-center space-y-5" hidden role="region" aria-label="Hasil kuis">
   <h2 class="text-2xl sm:text-3xl font-bold text-white">🎉 Hasil Kuis</h2>
   <p id="player-result" class="text-lg" style="color:var(--accent)"></p>
   <div id="score-display" class="text-5xl sm:text-6xl font-bold" style="color:var(--text)"></div>
   <p id="percentage-display" class="text-2xl text-emerald-400 font-bold"></p>
   <div id="coins-earned-display" class="text-lg font-bold" style="color:#fbbf24"></div>
   <div id="mode-result" class="text-sm" style="color:var(--muted)"></div>
   <div id="xp-result" class="space-y-1">
    <p id="xp-earned-text" class="font-bold" style="color:var(--accent)"></p>
    <div class="xp-bar h-3"><div id="xp-fill-result" class="xp-fill" style="width:0%"></div></div>
    <p id="level-result-text" class="text-sm" style="color:var(--muted)"></p>
   </div>
   <div id="achievements-container" class="space-y-2"></div>
   <div id="level-up-banner" class="rounded-xl p-4 border border-yellow-500/40 bg-yellow-500/10 level-up-anim" hidden>
    <p class="text-yellow-300 font-bold text-lg">⭐ LEVEL UP!</p>
    <p id="level-up-text" class="text-yellow-200 text-sm"></p>
   </div>
   <button id="share-btn" type="button" class="w-full py-3 rounded-lg font-bold bg-emerald-600 text-white hover:bg-emerald-700 transition">📤 Bagikan Hasil</button> 
   <button id="show-review-btn" type="button" class="w-full py-3 rounded-lg font-bold bg-slate-700 text-blue-400 hover:bg-slate-600 transition">📝 Review Jawaban</button> 
   <button id="restart-btn" class="w-full py-3 rounded-lg font-bold text-lg bg-blue-600 text-white hover:bg-blue-700 transition">Main Lagi</button> 
   <button id="show-leaderboard-results" type="button" class="w-full py-3 rounded-lg font-bold bg-slate-700 text-amber-400 hover:bg-slate-600 transition">🏆 Lihat Peringkat</button>
  </main>

  <!-- Leaderboard Page -->
  <main id="leaderboard-page" class="w-full max-w-md space-y-5" hidden role="region" aria-label="Peringkat">
   <h2 class="text-2xl sm:text-3xl font-bold text-center text-white">🏆 Peringkat Teratas</h2>
   <div class="flex gap-2">
     <select id="lb-filter-mode" class="flex-1 px-3 py-2 rounded-lg border text-sm" style="background:var(--card);border-color:var(--border);color:var(--text)" aria-label="Filter mode"> 
        <option value="">Semua Mode</option><option value="classic">Classic</option><option value="survival">Survival</option><option value="blitz">Blitz</option><option value="endless">Endless</option><option value="custom">Custom</option><option value="daily">Daily</option> 
     </select> 
     <select id="lb-filter-cat" class="flex-1 px-3 py-2 rounded-lg border text-sm" style="background:var(--card);border-color:var(--border);color:var(--text)" aria-label="Filter kategori"> 
        <option value="">Semua Kategori</option> 
     </select>
   </div>
   <div id="leaderboard-list" class="space-y-2"></div>
   <p id="leaderboard-empty" class="text-center" style="color:var(--muted)" hidden>Belum ada skor tersimpan.</p>
   <button id="back-btn-lb" type="button" class="w-full py-3 rounded-lg font-bold bg-slate-700 text-white hover:bg-slate-600 transition">← Kembali</button>
  </main>

  <!-- Stats Page -->
  <main id="stats-page" class="w-full max-w-md space-y-5" hidden role="region" aria-label="Statistik">
   <h2 class="text-2xl sm:text-3xl font-bold text-center text-white">📊 Statistik &amp; Perkembangan</h2>
   <div id="stats-content" class="space-y-3"></div>
   <div id="progress-chart" hidden>
    <p class="text-sm font-medium mb-2" style="color:var(--text)">📊 Perkembangan Nilai</p>
    <div id="chart-container" class="flex items-end gap-1 h-32 border-b" style="border-color:var(--border)"></div>
   </div>
   <div id="badges-section" hidden>
    <p class="text-sm font-medium mb-2" style="color:var(--text)">🏅 Badge</p>
    <div id="badges-list" class="flex flex-wrap gap-2"></div>
   </div>
   <p id="stats-empty" class="text-center" style="color:var(--muted)" hidden>Belum ada statistik.</p>
   <button id="back-btn-stats" type="button" class="w-full py-3 rounded-lg font-bold bg-slate-700 text-white hover:bg-slate-600 transition">← Kembali</button>
  </main>

  <!-- Settings Page -->
  <main id="settings-page" class="w-full max-w-md space-y-5" hidden role="region" aria-label="Pengaturan">
   <h2 class="text-2xl sm:text-3xl font-bold text-center text-white">⚙️ Pengaturan</h2>
   <div class="space-y-4">
    <div class="rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
     <p class="font-medium mb-2" style="color:var(--text)">🎨 Tema</p>
     <div id="theme-picker" class="flex gap-2 flex-wrap">
       <button type="button" data-theme="dark" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="background:#0f172a;color:#f1f5f9;border-color:#334155">Dark</button> 
       <button type="button" data-theme="light" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="background:#fff;color:#1e293b;border-color:#e2e8f0">Light</button> 
       <button type="button" data-theme="neon" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="background:#0a0a1e;color:#39ff14;border-color:#39ff14">Neon</button> 
       <button type="button" data-theme="pastel" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="background:#fff0f5;color:#4c1d95;border-color:#f9a8d4">Pastel</button>
     </div>
    </div>
    <div class="rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
     <p class="font-medium mb-2" style="color:var(--text)">🔤 Ukuran Font</p>
     <div id="font-picker" class="flex gap-2">
       <button type="button" data-fs="sm" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="border-color:var(--border);color:var(--text)">Kecil</button> 
       <button type="button" data-fs="md" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="border-color:var(--border);color:var(--text)">Normal</button> 
       <button type="button" data-fs="lg" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="border-color:var(--border);color:var(--text)">Besar</button> 
       <button type="button" data-fs="xl" class="theme-btn px-3 py-1.5 rounded-lg text-xs font-bold border" style="border-color:var(--border);color:var(--text)">Sangat Besar</button>
     </div>
    </div>
    <div class="flex items-center justify-between rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
     <span style="color:var(--text)" class="font-medium">🔊 Efek Suara</span>
     <button id="toggle-sound" type="button" class="w-12 h-6 rounded-full relative transition-colors" aria-label="Toggle suara"><span class="absolute top-0.5 w-5 h-5 bg-white rounded-full transition-all shadow"></span></button>
    </div>
    <div class="flex items-center justify-between rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
     <span style="color:var(--text)" class="font-medium">⏱️ Timer</span>
     <button id="toggle-timer" type="button" class="w-12 h-6 rounded-full relative transition-colors" aria-label="Toggle timer"><span class="absolute top-0.5 w-5 h-5 bg-white rounded-full transition-all shadow"></span></button>
    </div>
    <div class="flex items-center justify-between rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
     <span style="color:var(--text)" class="font-medium">✨ Animasi</span>
     <button id="toggle-animations" type="button" class="w-12 h-6 rounded-full relative transition-colors" aria-label="Toggle animasi"><span class="absolute top-0.5 w-5 h-5 bg-white rounded-full transition-all shadow"></span></button>
    </div>
   </div>
   <button id="back-btn-settings" type="button" class="w-full py-3 rounded-lg font-bold bg-slate-700 text-white hover:bg-slate-600 transition">← Kembali</button>
  </main>

  <script>
// Settings
let settings = JSON.parse(localStorage.getItem('quiz-settings') || '{}');
if(settings.sound === undefined) settings.sound = true;
if(settings.timer === undefined) settings.timer = true;
if(settings.animations === undefined) settings.animations = true;
if(!settings.theme) settings.theme = 'dark';
if(!settings.fontSize) settings.fontSize = 'md';

function saveSettings() { localStorage.setItem('quiz-settings', JSON.stringify(settings)); }

function applyTheme(t) {
  document.body.className = document.body.className.replace(/theme-\w+/, 'theme-' + t);
  settings.theme = t; saveSettings();
  document.querySelectorAll('#theme-picker .theme-btn').forEach(b => b.classList.toggle('active', b.dataset.theme === t));
}

function applyFontSize(s) {
  document.body.className = document.body.className.replace(/font-scale-\w+/, 'font-scale-' + s);
  settings.fontSize = s; saveSettings();
  document.querySelectorAll('#font-picker .theme-btn').forEach(b => b.classList.toggle('active', b.dataset.fs === s));
}

applyTheme(settings.theme);
applyFontSize(settings.fontSize);

document.getElementById('theme-picker').addEventListener('click', e => { const b = e.target.closest('[data-theme]'); if(b) applyTheme(b.dataset.theme); });
document.getElementById('font-picker').addEventListener('click', e => { const b = e.target.closest('[data-fs]'); if(b) applyFontSize(b.dataset.fs); });

function initToggle(id, key) {
  const btn = document.getElementById(id);
  function render() { const on = settings[key]; btn.style.background = on ? '#22c55e' : '#4b5563'; btn.querySelector('span').style.left = on ? '1.5rem' : '0.125rem'; }
  render();
  btn.addEventListener('click', () => { settings[key] = !settings[key]; saveSettings(); render(); });
}
initToggle('toggle-sound', 'sound');
initToggle('toggle-timer', 'timer');
initToggle('toggle-animations', 'animations');

// Audio Synthesizer Web Audio API
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
function playTone(f, d, t='sine', v=0.3) {
  if(!settings.sound) return;
  try {
    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
    o.type = t; o.frequency.value = f; g.gain.value = v;
    o.connect(g); g.connect(audioCtx.destination);
    o.start(); g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + d);
    o.stop(audioCtx.currentTime + d);
  } catch(e) {}
}
function soundCorrect(){ playTone(523,0.1); setTimeout(()=>playTone(659,0.1),100); setTimeout(()=>playTone(784,0.2),200); }
function soundWrong(){ playTone(200,0.3,'sawtooth',0.2); }
function soundTimeout(){ playTone(150,0.5,'square',0.15); }
function soundLevelUp(){ playTone(523,0.15); setTimeout(()=>playTone(659,0.15),150); setTimeout(()=>playTone(784,0.15),300); setTimeout(()=>playTone(1047,0.3),450); }

function spawnConfetti(){
  if(!settings.animations) return;
  const colors=['#f59e0b','#3b82f6','#22c55e','#ef4444','#8b5cf6','#ec4899'];
  for(let i=0; i<60; i++){
    const p = document.createElement('div');
    p.className = 'confetti-piece';
    p.style.left = Math.random()*100 + '%';
    p.style.background = colors[Math.floor(Math.random()*colors.length)];
    p.style.animationDelay = Math.random()*2 + 's';
    p.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
    document.body.appendChild(p);
    setTimeout(()=>p.remove(), 5000);
  }
}

function showToast(msg){ const t = document.createElement('div'); t.className = 'toast'; t.textContent = msg; document.body.appendChild(t); setTimeout(() => t.remove(), 2500); }

// XP & Coins
const XP_PER_LEVEL = 200;
function getLevel(xp){ return Math.floor(xp / XP_PER_LEVEL) + 1; }
function getXpInLevel(xp){ return xp % XP_PER_LEVEL; }
function calcXP(correct, difficulty, timeLeft, maxTime){ if(!correct) return 0; const base = difficulty === 'sulit' ? 30 : difficulty === 'sedang' ? 20 : 10; return base + Math.round((timeLeft / maxTime) * 10); }
function calcCoins(correct, difficulty){ if(!correct) return 0; return difficulty === 'sulit' ? 5 : difficulty === 'sedang' ? 3 : 2; }

// Question Bank
const questions = [
{c:"Pengetahuan Umum",q:"Berapa jumlah provinsi di Indonesia?",o:["34","38","33","36"],a:1,d:1,ex:"Indonesia memiliki 38 provinsi setelah pemekaran terbaru."},
{c:"Pengetahuan Umum",q:"Siapa penemu telepon?",o:["Thomas Edison","Alexander Graham Bell","Nikola Tesla","Michael Faraday"],a:1,d:1,ex:"Alexander Graham Bell mematenkan telepon pada tahun 1876."},
{c:"Pengetahuan Umum",q:"Apa mata uang Jepang?",o:["Won","Yuan","Yen","Baht"],a:2,d:1,ex:"Yen (¥) adalah mata uang resmi Jepang sejak 1871."},
{c:"Pengetahuan Umum",q:"Planet apa yang dijuluki Planet Merah?",o:["Venus","Mars","Jupiter","Saturnus"],a:1,d:1,ex:"Mars terlihat merah karena oksida besi di permukaannya."},
{c:"Pengetahuan Umum",q:"Apa bahasa resmi Brasil?",o:["Spanyol","Portugis","Inggris","Prancis"],a:1,d:1,ex:"Brasil adalah negara berbahasa Portugis terbesar di dunia."},
{c:"Sains",q:"Apa rumus kimia air?",o:["CO2","H2O","NaCl","O2"],a:1,d:1,ex:"H2O terdiri dari 2 atom hidrogen dan 1 atom oksigen."},
{c:"Sains",q:"Berapa kecepatan cahaya (km/detik)?",o:["150.000","300.000","450.000","600.000"],a:1,d:2,ex:"Kecepatan cahaya sekitar 299.792 km/detik."},
{c:"Sains",q:"Apa organ terbesar dalam tubuh manusia?",o:["Hati","Paru-paru","Kulit","Otak"],a:2,d:1,ex:"Kulit orang dewasa memiliki luas sekitar 2 meter persegi."},
{c:"Geografi",q:"Apa gunung tertinggi di dunia?",o:["K2","Everest","Kilimanjaro","Denali"],a:1,d:1,ex:"Everest setinggi 8.849 meter."},
{c:"Geografi",q:"Negara mana yang memiliki wilayah terluas?",o:["Kanada","China","Rusia","Amerika Serikat"],a:2,d:1,ex:"Rusia 17,1 juta km²."},
{c:"Sejarah",q:"Kapan Indonesia merdeka?",o:["17 Agustus 1945","17 Agustus 1944","1 Juni 1945","28 Oktober 1928"],a:0,d:1,ex:"Proklamasi 17 Agustus 1945."},
{c:"Sejarah",q:"Siapa presiden pertama Indonesia?",o:["Soeharto","Soekarno","Habibie","Hatta"],a:1,d:1,ex:"Soekarno presiden pertama RI."},
{c:"Hiburan",q:"Film animasi 'Frozen' diproduksi oleh?",o:["Pixar","DreamWorks","Disney","Illumination"],a:2,d:1,ex:"Walt Disney Animation Studios, 2013."},
{c:"Hiburan",q:"Siapa penulis novel Harry Potter?",o:["J.R.R. Tolkien","J.K. Rowling","Stephen King","George R.R. Martin"],a:1,d:1,ex:"J.K. Rowling 1997-2007."}
];

// Game Modes
const MODES = {
  classic: {name:"Classic", icon:"book-open", desc:"15 soal, timer normal", questions:15},
  survival: {name:"Survival", icon:"heart", desc:"3 nyawa, game over jika habis", questions:999},
  blitz: {name:"Blitz", icon:"zap", desc:"20 soal, 5 detik per soal!", questions:20},
  endless: {name:"Endless", icon:"infinity", desc:"Terus bermain sampai salah", questions:999},
  custom: {name:"Custom", icon:"sliders", desc:"Atur sendiri jumlah & waktu"},
  daily: {name:"Daily Challenge", icon:"calendar", desc:"10 soal harian, bonus koin!", questions:10}
};

// State Variables
let currentQ = 0, score = 0, playerName = "", selected = -1, selectedCategories = [];
let filteredQuestions = [], timerInterval = null, timeLeft = 15;
let allScores = [], streak = 0, bestStreak = 0, sessionAchievements = [], sessionXP = 0, sessionCoins = 0;
let answerTimes = [], questionStartTime = 0, categoryStats = {};
let playerXP = 0, playerCoins = 0, gameMode = 'classic', lives = 3;
let adaptiveAccuracy = {}, seenQuestions = new Set();
let reviewData = [];
let customConfig = {count:10, time:15, diff:0};
const AUTOSAVE_KEY = 'quiz-autosave';
const SCORES_KEY = 'quiz-scores';

// Replacement for Data SDK using localStorage
const dataHandler = {
  getScores() {
    return JSON.parse(localStorage.getItem(SCORES_KEY) || '[]');
  },
  saveScore(scoreData) {
    const scores = this.getScores();
    scores.push(scoreData);
    localStorage.setItem(SCORES_KEY, JSON.stringify(scores));
    this.refreshData();
    return { isOk: true };
  },
  refreshData() {
    allScores = this.getScores().sort((a,b) => b.percentage - a.percentage || b.score - a.score);
    if(playerName) {
      const ps = allScores.filter(s => s.player_name === playerName);
      playerXP = ps.reduce((sum, s) => sum + (s.xp_earned || 0), 0);
      playerCoins = ps.reduce((sum, s) => sum + (s.coins_earned || 0), 0);
      updateXPDisplay();
    }
    renderLeaderboard();
  }
};

// Initialize Scores
dataHandler.refreshData();

function updateXPDisplay(){
  const d = document.getElementById('xp-display');
  const cd = document.getElementById('coins-display');
  if(!playerName){ d.hidden = true; cd.hidden = true; return; }
  d.hidden = false; cd.hidden = false;
  const level = getLevel(playerXP);
  const inLevel = getXpInLevel(playerXP);
  document.getElementById('level-text').textContent = `⭐ Level ${level}`;
  document.getElementById('xp-text').textContent = `${inLevel}/${XP_PER_LEVEL} XP`;
  document.getElementById('xp-fill').style.width = Math.round((inLevel / XP_PER_LEVEL) * 100) + '%';
  document.getElementById('coins-text').textContent = playerCoins;
}

function renderLeaderboard(){
  const modeF = document.getElementById('lb-filter-mode').value;
  const catF = document.getElementById('lb-filter-cat').value;
  let filtered = allScores;
  if(modeF) filtered = filtered.filter(s => (s.game_mode || 'classic') === modeF || (modeF === 'daily' && s.is_daily_challenge));
  if(catF) filtered = filtered.filter(s => s.categories && s.categories.includes(catF));
  
  const list = document.getElementById('leaderboard-list');
  const empty = document.getElementById('leaderboard-empty');
  if(!filtered.length){ list.innerHTML = ''; empty.hidden = false; return; }
  empty.hidden = true;
  list.innerHTML = filtered.slice(0,15).map((s,i) => {
    const medal = i === 0 ? '🥇' : i === 1 ? '🥈' : i === 2 ? '🥉' : `#${i+1}`;
    return `<div class="flex items-center justify-between rounded-lg px-4 py-3 border" style="background:var(--card);border-color:var(--border)">
      <div class="flex items-center gap-3"><span class="text-lg font-bold">${medal}</span>
      <div><span style="color:var(--text)" class="font-medium">${s.player_name}</span>
      <span class="text-xs ml-2" style="color:var(--accent)">Lv${getLevel(allScores.filter(x=>x.player_name===s.player_name).reduce((sum,x)=>sum+(x.xp_earned||0),0))}</span></div></div>
      <div class="text-right"><span class="text-emerald-400 font-bold">${s.percentage}%</span><span class="text-xs ml-1" style="color:var(--muted)">${s.game_mode||'classic'}</span></div></div>`;
  }).join('');
}

document.getElementById('lb-filter-mode').addEventListener('change', renderLeaderboard);
document.getElementById('lb-filter-cat').addEventListener('change', renderLeaderboard);

// Populate Category Filters
const ALL_CATS = [
  {name:"Pengetahuan Umum", icon:"book-open"},
  {name:"Sains", icon:"flask-conical"},
  {name:"Geografi", icon:"globe"},
  {name:"Sejarah", icon:"hourglass"},
  {name:"Hiburan", icon:"star"}
];
const lbCatSelect = document.getElementById('lb-filter-cat');
ALL_CATS.forEach(c => { const o = document.createElement('option'); o.value = c.name; o.textContent = c.name; lbCatSelect.appendChild(o); });

// Navigation with Page Transitions
const allPages = ['welcome-page','mode-page','custom-page','category-page','quiz-page','results-page','leaderboard-page','stats-page','settings-page','review-page'];
function showPage(id){
  allPages.forEach(p => {
    const el = document.getElementById(p);
    if(!el.hidden && p !== id){
      el.classList.add('page-exit');
      setTimeout(() => { el.hidden = true; el.classList.remove('page-exit'); }, 200);
    }
  });
  setTimeout(() => {
    const target = document.getElementById(id);
    target.hidden = false;
    target.classList.add('page-enter');
    setTimeout(() => target.classList.remove('page-enter'), 350);
  }, 210);
}

// Autosave Game
function saveGameState(){
  const state = {playerName, currentQ, score, streak, bestStreak, lives, gameMode, selectedCategories, filteredQuestions, sessionXP, sessionCoins, answerTimes, categoryStats, reviewData, seenQs: [...seenQuestions]};
  localStorage.setItem(AUTOSAVE_KEY, JSON.stringify(state));
}
function loadGameState(){ try { return JSON.parse(localStorage.getItem(AUTOSAVE_KEY)); } catch(e) { return null; } }
function clearAutosave(){ localStorage.removeItem(AUTOSAVE_KEY); }

function checkAutosave(){ const saved = loadGameState(); if(saved && saved.currentQ > 0){ document.getElementById('autosave-banner').hidden = false; document.getElementById('player-name').value = saved.playerName || ''; } }
checkAutosave();

document.getElementById('resume-btn').addEventListener('click', () => {
  const saved = loadGameState(); if(!saved) return;
  playerName = saved.playerName; currentQ = saved.currentQ; score = saved.score; streak = saved.streak; bestStreak = saved.bestStreak; lives = saved.lives; gameMode = saved.gameMode; selectedCategories = saved.selectedCategories; filteredQuestions = saved.filteredQuestions; sessionXP = saved.sessionXP; sessionCoins = saved.sessionCoins || 0; answerTimes = saved.answerTimes || []; categoryStats = saved.categoryStats || {}; reviewData = saved.reviewData || [];
  if(saved.seenQs) saved.seenQs.forEach(q => seenQuestions.add(q));
  dataHandler.refreshData();
  document.getElementById('autosave-banner').hidden = true;
  document.getElementById('lives-display').hidden = gameMode !== 'survival';
  if(gameMode === 'survival') document.getElementById('lives-display').textContent = '❤️'.repeat(Math.max(0, lives));
  showPage('quiz-page'); showQuestion();
});
document.getElementById('discard-btn').addEventListener('click', () => { clearAutosave(); document.getElementById('autosave-banner').hidden = true; });

// User Login & Form Handler
document.getElementById('name-form').addEventListener('submit', function(e){
  e.preventDefault();
  if(audioCtx.state === 'suspended') audioCtx.resume();
  playerName = document.getElementById('player-name').value.trim();
  const pin = document.getElementById('player-pin').value.trim();
  if(!playerName || !/^[0-9]{4}$/.test(pin)){ showToast('Masukkan PIN 4 digit.'); return; }
  
  const pinHash = pin.split('').reduce((h,c) => (h*31 + c.charCodeAt(0)) >>> 0, 2166136261).toString(16);
  const playerRecords = allScores.filter(s => s.player_name === playerName);
  const knownPin = playerRecords.find(s => s.pin_hash)?.pin_hash;
  if(knownPin && knownPin !== pinHash){ showToast('PIN salah.'); return; }
  
  dataHandler.refreshData();
  clearAutosave();
  showPage('mode-page');
  showModeSelection();
});

document.getElementById('show-leaderboard-welcome').addEventListener('click', () => showPage('leaderboard-page'));
document.getElementById('show-leaderboard-results').addEventListener('click', () => showPage('leaderboard-page'));
document.getElementById('show-stats-welcome').addEventListener('click', () => showStatsPage());
document.getElementById('show-settings-welcome').addEventListener('click', () => showPage('settings-page'));
document.getElementById('back-btn-lb').addEventListener('click', () => showPage('welcome-page'));
document.getElementById('back-btn-stats').addEventListener('click', () => showPage('welcome-page'));
document.getElementById('back-btn-settings').addEventListener('click', () => showPage('welcome-page'));
document.getElementById('restart-btn').addEventListener('click', () => { clearAutosave(); showPage('welcome-page'); updateXPDisplay(); });
document.getElementById('show-review-btn').addEventListener('click', () => showReviewPage());
document.getElementById('review-done-btn').addEventListener('click', () => showPage('results-page'));

// Share Functionality
document.getElementById('share-btn').addEventListener('click', () => {
  const totalAnswered = reviewData.length;
  const pct = totalAnswered ? Math.round((score/totalAnswered)*100) : 0;
  const text = `🧠 Kuis Teka-Teki\n${playerName} mendapat ${score}/${totalAnswered} (${pct}%)\nMode: ${MODES[gameMode]?.name || gameMode} | Streak: ${bestStreak}\n+${sessionXP} XP | +${sessionCoins} 🪙`;
  navigator.clipboard.writeText(text).then(() => showToast('Hasil disalin ke clipboard! 📋')).catch(() => showToast('Gagal menyalin'));
});

// Mode Selection Rendering
function showModeSelection(){
  const container = document.getElementById('mode-buttons'); container.innerHTML = ''; gameMode = 'classic';
  Object.entries(MODES).forEach(([key, mode]) => {
    const btn = document.createElement('button'); btn.type = 'button';
    btn.className = 'mode-card px-4 py-5 rounded-xl border-2 font-medium' + (key === 'classic' ? ' selected' : '');
    btn.style.cssText = `border-color:${key === 'classic' ? 'var(--accent)' : 'var(--border)'};background:${key === 'classic' ? 'rgba(59,130,246,0.15)' : 'var(--card)'};color:var(--text)`;
    const extra = key === 'daily' ? getDailyStatus() : '';
    btn.innerHTML = `<div class="flex flex-col items-center gap-2"><i data-lucide="${mode.icon}" style="width:24px;height:24px;"></i><span class="text-sm font-bold">${mode.name}</span><span class="text-xs opacity-70">${mode.desc}</span>${extra}</div>`;
    btn.addEventListener('click', () => {
      container.querySelectorAll('.mode-card').forEach(b => { b.classList.remove('selected'); b.style.borderColor = 'var(--border)'; b.style.background = 'var(--card)'; });
      btn.classList.add('selected'); btn.style.borderColor = 'var(--accent)'; btn.style.background = 'rgba(59,130,246,0.15)'; gameMode = key;
    });
    container.appendChild(btn);
  });
  if(window.lucide) lucide.createIcons();
}

function getDailySeed(){ return new Date().toISOString().slice(0,10); }
function getDailyStatus(){
  const today = getDailySeed();
  const done = allScores.some(s => s.player_name === playerName && s.is_daily_challenge && s.played_at && s.played_at.startsWith(today));
  return done ? '<span class="text-xs text-emerald-400">✓ Selesai</span>' : '<span class="text-xs text-yellow-400">Bonus 2x koin!</span>';
}
function getDailyQuestions(){
  const seed = getDailySeed(); let hash = 0; for(let i = 0; i < seed.length; i++) hash = ((hash << 5) - hash) + seed.charCodeAt(i);
  const shuffled = [...questions].sort((a,b) => { const ha = ((hash*31)+a.q.length)%1000; const hb = ((hash*31)+b.q.length)%1000; return ha - hb; });
  return shuffled.slice(0, 10);
}

document.getElementById('mode-next-btn').addEventListener('click', () => {
  if(gameMode === 'custom'){ showPage('custom-page'); return; }
  if(gameMode === 'daily'){ startDailyChallenge(); return; }
  showPage('category-page'); showCategorySelection();
});

document.getElementById('custom-start-btn').addEventListener('click', () => {
  customConfig.count = Math.max(5, Math.min(50, parseInt(document.getElementById('custom-count').value) || 10));
  customConfig.time = Math.max(3, Math.min(60, parseInt(document.getElementById('custom-time').value) || 15));
  customConfig.diff = parseInt(document.getElementById('custom-diff').value) || 0;
  showPage('category-page'); showCategorySelection();
});

function startDailyChallenge(){
  selectedCategories = ALL_CATS.map(c => c.name);
  filteredQuestions = getDailyQuestions();
  currentQ = 0; score = 0; streak = 0; bestStreak = 0; lives = 3; sessionAchievements = []; sessionXP = 0; sessionCoins = 0; answerTimes = []; categoryStats = {}; reviewData = [];
  document.getElementById('lives-display').hidden = true;
  showPage('quiz-page'); showQuestion();
}

// Category Selection
function showCategorySelection(filter = ''){
  const container = document.getElementById('category-buttons'); container.innerHTML = '';
  const filtered = filter ? ALL_CATS.filter(c => c.name.toLowerCase().includes(filter.toLowerCase())) : ALL_CATS;
  filtered.forEach(cat => {
    const btn = document.createElement('button'); btn.type = 'button';
    const isSelected = selectedCategories.includes(cat.name);
    btn.className = 'px-4 py-5 rounded-xl border-2 font-medium transition-all';
    btn.style.cssText = `border-color:${isSelected ? 'var(--accent)' : 'var(--border)'};background:${isSelected ? 'rgba(59,130,246,0.15)' : 'var(--card)'};color:var(--text)`;
    const count = questions.filter(q => q.c === cat.name).length;
    btn.innerHTML = `<div class="flex flex-col items-center gap-2"><i data-lucide="${cat.icon}" style="width:24px;height:24px;"></i><span class="text-xs sm:text-sm">${cat.name}</span><span class="text-xs opacity-50">${count} soal</span></div>`;
    btn.addEventListener('click', () => {
      if(selectedCategories.includes(cat.name)){
        selectedCategories = selectedCategories.filter(c => c !== cat.name);
        btn.style.borderColor = 'var(--border)'; btn.style.background = 'var(--card)';
      } else {
        selectedCategories.push(cat.name);
        btn.style.borderColor = 'var(--accent)'; btn.style.background = 'rgba(59,130,246,0.15)';
      }
    });
    container.appendChild(btn);
  });
  if(window.lucide) lucide.createIcons();
}
showCategorySelection();

document.getElementById('category-filter').addEventListener('input', e => showCategorySelection(e.target.value));
document.getElementById('category-start-btn').addEventListener('click', () => { if(!selectedCategories.length) return; startQuiz(); });

function shuffle(arr){ for(let i = arr.length - 1; i > 0; i--){ const j = Math.floor(Math.random() * (i + 1)); [arr[i], arr[j]] = [arr[j], arr[i]]; } return arr; }
function smartShuffle(pool, count){
  const unseen = pool.filter(q => !seenQuestions.has(q.q));
  let sel;
  if(unseen.length >= count) sel = shuffle(unseen).slice(0, count);
  else sel = shuffle(unseen).concat(shuffle(pool.filter(q => seenQuestions.has(q.q))).slice(0, count - unseen.length));
  sel.forEach(q => seenQuestions.add(q.q));
  if(seenQuestions.size >= pool.length) seenQuestions.clear();
  return sel;
}

function startQuiz(){
  showPage('quiz-page');
  let pool = questions.filter(q => selectedCategories.includes(q.c));
  if(gameMode === 'custom' && customConfig.diff > 0) pool = pool.filter(q => q.d === customConfig.diff);
  const maxQ = gameMode === 'custom' ? customConfig.count : (MODES[gameMode]?.questions || 15);
  const count = Math.min(maxQ, pool.length);
  filteredQuestions = (gameMode === 'survival' || gameMode === 'endless') ? smartShuffle(pool, pool.length) : smartShuffle(pool, count);
  currentQ = 0; score = 0; streak = 0; bestStreak = 0; lives = 3; sessionAchievements = []; sessionXP = 0; sessionCoins = 0; answerTimes = []; categoryStats = {}; reviewData = [];
  adaptiveAccuracy = {};
  
  allScores.filter(s => s.player_name === playerName && s.stats_by_category).forEach(s => {
    try {
      const p = JSON.parse(s.stats_by_category);
      Object.entries(p).forEach(([c, st]) => {
        if(!adaptiveAccuracy[c]) adaptiveAccuracy[c] = {correct: 0, total: 0};
        adaptiveAccuracy[c].correct += st.correct; adaptiveAccuracy[c].total += st.total;
      });
    } catch(e){}
  });
  
  document.getElementById('lives-display').hidden = gameMode !== 'survival';
  if(gameMode === 'survival') document.getElementById('lives-display').textContent = '❤️❤️❤️';
  showQuestion();
}

function getAdaptiveDifficulty(category){ const acc = adaptiveAccuracy[category]; if(!acc || acc.total < 2) return 2; const pct = acc.correct / acc.total; if(pct >= 0.8) return 3; if(pct <= 0.4) return 1; return 2; }
function getTimeForQuestion(q){
  if(!settings.timer) return 999;
  if(gameMode === 'blitz') return 5;
  if(gameMode === 'custom') return customConfig.time;
  const d = Math.max(q.d || 2, getAdaptiveDifficulty(q.c));
  return d === 1 ? 15 : d === 2 ? 12 : 8;
}

function startTimer(maxTime){
  timeLeft = maxTime; clearInterval(timerInterval);
  document.getElementById('timer-container').style.display = settings.timer ? '' : 'none';
  updateTimerUI(maxTime);
  if(settings.timer) {
    timerInterval = setInterval(() => {
      timeLeft -= 0.1;
      if(timeLeft <= 0){ timeLeft = 0; clearInterval(timerInterval); timeUp(); }
      updateTimerUI(maxTime);
    }, 100);
  }
}

function updateTimerUI(maxTime){
  const pct = (timeLeft / maxTime) * 100;
  const bar = document.getElementById('timer-bar');
  bar.style.width = pct + '%';
  bar.className = 'timer-bar h-full rounded-full ' + (pct > 50 ? 'bg-gradient-to-r from-green-400 to-emerald-500' : pct > 25 ? 'bg-gradient-to-r from-yellow-400 to-orange-500' : 'bg-gradient-to-r from-red-500 to-rose-600');
  document.getElementById('timer-text').textContent = Math.ceil(timeLeft) + 's';
}

function timeUp(){
  soundTimeout(); selected = 99; streak = 0;
  const q = filteredQuestions[currentQ];
  document.querySelectorAll('.option-btn').forEach(b => { b.classList.add('disabled'); if(b.dataset.correct === 'true') b.classList.add('correct'); });
  if(!categoryStats[q.c]) categoryStats[q.c] = {correct: 0, total: 0};
  categoryStats[q.c].total++;
  reviewData.push({q: q.q, playerAnswer: '(Waktu habis)', correctAnswer: q.o[q.a], correct: false, explanation: q.ex});
  if(gameMode === 'survival'){
    lives--;
    document.getElementById('lives-display').textContent = '❤️'.repeat(Math.max(0, lives));
    if(lives <= 0){ setTimeout(() => showResults(), 800); return; }
  }
  if(gameMode === 'endless'){ setTimeout(() => showResults(), 800); return; }
  showExplanation(q); document.getElementById('next-btn').hidden = false;
  answerTimes.push(getTimeForQuestion(q)); saveGameState();
}

function showExplanation(q){ document.getElementById('explanation-box').hidden = false; document.getElementById('explanation-text').textContent = q.ex || ''; }

function showQuestion(){
  if(currentQ >= filteredQuestions.length){ showResults(); return; }
  selected = -1; const q = filteredQuestions[currentQ]; const maxTime = getTimeForQuestion(q);
  const effectiveD = Math.max(q.d || 2, getAdaptiveDifficulty(q.c));
  const counterText = (gameMode === 'survival' || gameMode === 'endless') ? `Soal ${currentQ+1}` : `${currentQ+1}/${filteredQuestions.length}`;
  document.getElementById('category-label').textContent = q.c;
  document.getElementById('question-counter').textContent = counterText;
  document.getElementById('question-text').textContent = q.q;
  document.getElementById('next-btn').hidden = true;
  document.getElementById('explanation-box').hidden = true;
  document.getElementById('xp-gain').hidden = true;
  
  const badge = document.getElementById('difficulty-badge');
  const dLabel = effectiveD === 1 ? 'Mudah' : effectiveD === 2 ? 'Sedang' : 'Sulit';
  badge.textContent = '⚡ ' + dLabel;
  badge.style.background = effectiveD === 1 ? 'rgba(34,197,94,0.2)' : effectiveD === 2 ? 'rgba(234,179,8,0.2)' : 'rgba(239,68,68,0.2)';
  badge.style.color = effectiveD === 1 ? '#4ade80' : effectiveD === 2 ? '#fbbf24' : '#f87171';
  
  if(streak >= 2){ document.getElementById('streak-display').hidden = false; document.getElementById('streak-display').textContent = '🔥 ' + streak; } 
  else document.getElementById('streak-display').hidden = true;
  
  const correctAnswer = q.o[q.a];
  let opts = shuffle(q.o.slice()).map(o => ({text: o, correct: o === correctAnswer}));
  const container = document.getElementById('options-container'); container.innerHTML = '';
  
  opts.forEach(opt => {
    const btn = document.createElement('button');
    btn.className = 'option-btn w-full text-left px-4 py-3 rounded-lg border-2 font-medium';
    btn.textContent = opt.text;
    btn.dataset.correct = opt.correct;
    btn.addEventListener('click', () => selectAnswer(opt, btn, q, maxTime));
    container.appendChild(btn);
  });
  questionStartTime = Date.now(); startTimer(maxTime);
}

function selectAnswer(opt, btn, q, maxTime){
  if(selected !== -1) return; selected = 1; clearInterval(timerInterval);
  const elapsed = (Date.now() - questionStartTime) / 1000; answerTimes.push(elapsed);
  if(!categoryStats[q.c]) categoryStats[q.c] = {correct: 0, total: 0}; categoryStats[q.c].total++;
  if(!adaptiveAccuracy[q.c]) adaptiveAccuracy[q.c] = {correct: 0, total: 0}; adaptiveAccuracy[q.c].total++;
  
  document.querySelectorAll('.option-btn').forEach(b => { b.classList.add('disabled'); if(b.dataset.correct === 'true') b.classList.add('correct'); });
  const effectiveD = Math.max(q.d || 2, getAdaptiveDifficulty(q.c));
  const diffLabel = effectiveD === 1 ? 'mudah' : effectiveD === 2 ? 'sedang' : 'sulit';
  reviewData.push({q: q.q, playerAnswer: opt.text, correctAnswer: q.o[q.a], correct: opt.correct, explanation: q.ex});
  
  if(opt.correct){
    score++; streak++; if(streak > bestStreak) bestStreak = streak; soundCorrect(); btn.classList.add('pulse-correct');
    categoryStats[q.c].correct++; adaptiveAccuracy[q.c].correct++;
    const xp = calcXP(true, diffLabel, timeLeft, maxTime); sessionXP += xp;
    const coins = calcCoins(true, diffLabel) * (gameMode === 'daily' ? 2 : 1); sessionCoins += coins;
    document.getElementById('xp-gain').textContent = `+${xp} XP | +${coins} 🪙`; document.getElementById('xp-gain').hidden = false;
  } else {
    btn.classList.add('wrong', 'shake'); soundWrong(); streak = 0;
    if(gameMode === 'survival'){
      lives--;
      document.getElementById('lives-display').textContent = '❤️'.repeat(Math.max(0, lives));
      if(lives <= 0){ showExplanation(q); setTimeout(() => showResults(), 1500); return; }
    }
    if(gameMode === 'endless'){ showExplanation(q); setTimeout(() => showResults(), 1500); return; }
  }
  showExplanation(q); document.getElementById('next-btn').hidden = false; saveGameState();
}

document.getElementById('next-btn').addEventListener('click', () => { currentQ++; if(currentQ >= filteredQuestions.length) showResults(); else showQuestion(); });

// Achievements Checks
function checkAchievements(){
  const a = sessionAchievements;
  if(bestStreak >= 5 && !a.includes('🔥 5 Beruntun')) a.push('🔥 5 Beruntun');
  if(bestStreak >= 10 && !a.includes('🔥 10 Beruntun')) a.push('🔥 10 Beruntun');
  if(score === filteredQuestions.length && currentQ >= 4 && !a.includes('💯 Sempurna')) a.push('💯 Sempurna');
  if(sessionXP >= 100 && !a.includes('💎 100 XP')) a.push('💎 100 XP');
  if(gameMode === 'survival' && score >= 20 && !a.includes('🛡️ Survivor')) a.push('🛡️ Survivor');
  if(gameMode === 'blitz' && score >= 15 && !a.includes('⚡ Blitz Master')) a.push('⚡ Blitz Master');
  const avgTime = answerTimes.length ? answerTimes.reduce((x,b) => x+b, 0) / answerTimes.length : 99;
  if(avgTime < 3 && score >= 5 && !a.includes('⚡ Kilat')) a.push('⚡ Kilat');
}

function showResults(){
  clearInterval(timerInterval); clearAutosave();
  const totalAnswered = reviewData.length; checkAchievements();
  showPage('results-page');
  const pct = totalAnswered > 0 ? Math.round((score / totalAnswered) * 100) : 0;
  const avgTime = answerTimes.length ? (answerTimes.reduce((a,b) => a+b, 0) / answerTimes.length).toFixed(1) : 0;
  
  document.getElementById('player-result').textContent = `Selamat, ${playerName}!`;
  document.getElementById('score-display').textContent = `${score}/${totalAnswered}`;
  document.getElementById('percentage-display').textContent = `${pct}%`;
  document.getElementById('coins-earned-display').textContent = `+${sessionCoins} 🪙 Koin diraih!`;
  document.getElementById('mode-result').textContent = `Mode: ${MODES[gameMode]?.name || gameMode} • Streak: ${bestStreak} • Waktu: ${avgTime}s/soal`;
  document.getElementById('xp-earned-text').textContent = `+${sessionXP} XP diraih!`;
  
  const oldLevel = getLevel(playerXP);
  const newTotal = playerXP + sessionXP;
  const newLevel = getLevel(newTotal);
  const inLvl = getXpInLevel(newTotal);
  
  document.getElementById('xp-fill-result').style.width = Math.round((inLvl / XP_PER_LEVEL) * 100) + '%';
  document.getElementById('level-result-text').textContent = `Level ${newLevel} • ${inLvl}/${XP_PER_LEVEL} XP`;
  
  const luBanner = document.getElementById('level-up-banner');
  if(newLevel > oldLevel){
    luBanner.hidden = false;
    document.getElementById('level-up-text').textContent = `Kamu naik ke Level ${newLevel}!`;
    soundLevelUp();
    if(settings.animations) spawnConfetti();
  } else {
    luBanner.hidden = true;
    if(pct === 100 && settings.animations) spawnConfetti();
  }
  
  const achContainer = document.getElementById('achievements-container');
  achContainer.innerHTML = sessionAchievements.length ? `<div class="flex flex-wrap gap-2 justify-center">${sessionAchievements.map(x => `<span class="px-3 py-1 rounded-full text-xs font-medium bg-yellow-500/20 text-yellow-300 border border-yellow-500/40 achieve-pop">${x}</span>`).join('')}</div>` : '';

  const pinHash = document.getElementById('player-pin').value.trim().split('').reduce((h,c) => (h*31 + c.charCodeAt(0)) >>> 0, 2166136261).toString(16);
  
  dataHandler.saveScore({
    player_name: playerName,
    score,
    total_questions: totalAnswered,
    percentage: pct,
    categories: selectedCategories.join(', '),
    played_at: new Date().toISOString(),
    avg_time: parseFloat(avgTime),
    achievements: sessionAchievements.join(','),
    stats_by_category: JSON.stringify(categoryStats),
    xp_earned: sessionXP,
    is_daily_challenge: gameMode === 'daily',
    game_mode: gameMode,
    streak_best: bestStreak,
    coins_earned: sessionCoins,
    pin_hash: pinHash
  });
}

function showReviewPage(){
  showPage('review-page');
  document.getElementById('review-list').innerHTML = reviewData.map((r,i) => `
    <div class="rounded-lg p-3 border ${r.correct ? 'review-correct' : 'review-wrong'}" style="background:var(--card);border-color:var(--border)">
      <p class="font-medium text-sm mb-1" style="color:var(--text)">${i+1}. ${r.q}</p>
      <p class="text-xs ${r.correct ? 'text-emerald-400' : 'text-red-400'}">Jawaban: ${r.playerAnswer}</p>
      ${!r.correct ? `<p class="text-xs text-emerald-400">Benar: ${r.correctAnswer}</p>` : ''}
      <p class="text-xs mt-1 opacity-70" style="color:var(--muted)">${r.explanation}</p>
    </div>`).join('');
}

function showStatsPage(){
  showPage('stats-page');
  const container = document.getElementById('stats-content');
  const empty = document.getElementById('stats-empty');
  const chart = document.getElementById('progress-chart');
  const badgesSection = document.getElementById('badges-section');
  const playerScores = allScores.filter(s => s.player_name === playerName);
  
  if(!playerScores.length){ container.innerHTML = ''; empty.hidden = false; chart.hidden = true; badgesSection.hidden = true; return; }
  empty.hidden = true;
  
  const totalXP = playerScores.reduce((sum,s) => sum + (s.xp_earned || 0), 0);
  const level = getLevel(totalXP);
  const totalCoins = playerScores.reduce((sum,s) => sum + (s.coins_earned || 0), 0);
  const totalGames = playerScores.length;
  const avgPct = Math.round(playerScores.reduce((a,s) => a + s.percentage, 0) / totalGames);
  const totalCorrect = playerScores.reduce((a,s) => a + s.score, 0);
  const totalQ = playerScores.reduce((a,s) => a + s.total_questions, 0);
  const accuracy = totalQ ? Math.round((totalCorrect / totalQ) * 100) : 0;
  const maxStreak = Math.max(...playerScores.map(s => s.streak_best || 0));
  const avgTimeAll = playerScores.filter(s => s.avg_time).reduce((a,s) => a + s.avg_time, 0) / (playerScores.filter(s => s.avg_time).length || 1);

  let catBreakdown = {};
  playerScores.forEach(s => {
    if(s.stats_by_category){
      try {
        Object.entries(JSON.parse(s.stats_by_category)).forEach(([c, st]) => {
          if(!catBreakdown[c]) catBreakdown[c] = {correct: 0, total: 0};
          catBreakdown[c].correct += st.correct; catBreakdown[c].total += st.total;
        });
      } catch(e){}
    }
  });

  let html = `<div class="rounded-lg p-4 border space-y-3" style="background:var(--card);border-color:var(--border)">
    <p style="color:var(--text)" class="font-bold text-lg">⭐ ${playerName} — Level ${level}</p>
    <div class="grid grid-cols-2 gap-2 text-sm">
      <div><span style="color:var(--muted)">Total Games:</span> <span style="color:var(--text)" class="font-bold">${totalGames}</span></div>
      <div><span style="color:var(--muted)">Total XP:</span> <span class="font-bold" style="color:var(--accent)">${totalXP}</span></div>
      <div><span style="color:var(--muted)">Koin:</span> <span class="font-bold" style="color:#fbbf24">🪙 ${totalCoins}</span></div>
      <div><span style="color:var(--muted)">Akurasi:</span> <span class="text-emerald-400 font-bold">${accuracy}%</span></div>
      <div><span style="color:var(--muted)">Rata-rata:</span> <span style="color:var(--text)" class="font-bold">${avgPct}%</span></div>
      <div><span style="color:var(--muted)">Best Streak:</span> <span class="text-orange-400 font-bold">🔥 ${maxStreak}</span></div>
      <div><span style="color:var(--muted)">Waktu/Soal:</span> <span style="color:var(--text)" class="font-bold">${avgTimeAll.toFixed(1)}s</span></div>
    </div></div>`;
    
  if(Object.keys(catBreakdown).length){
    html += `<div class="rounded-lg p-4 border space-y-2" style="background:var(--card);border-color:var(--border)"><p style="color:var(--text)" class="font-bold text-sm mb-2">📊 Akurasi per Kategori</p>${Object.entries(catBreakdown).map(([c,s]) => { const pct = Math.round((s.correct / s.total) * 100); return `<div><div class="flex justify-between text-xs mb-1"><span style="color:var(--muted)">${c}</span><span class="text-emerald-400">${pct}% (${s.correct}/${s.total})</span></div><div class="w-full h-2 rounded-full" style="background:var(--border)"><div class="h-full rounded-full" style="width:${pct}%;background:linear-gradient(90deg,var(--accent),var(--accent2))"></div></div></div>`; }).join('')}</div>`;
  }
  container.innerHTML = html;

  // Badges Rendering
  const allAch = new Set();
  playerScores.forEach(s => { if(s.achievements) s.achievements.split(',').filter(Boolean).forEach(a => allAch.add(a)); });
  if(allAch.size){
    badgesSection.hidden = false;
    document.getElementById('badges-list').innerHTML = [...allAch].map(a => `<span class="px-3 py-1 rounded-full text-xs font-medium bg-yellow-500/20 text-yellow-300 border border-yellow-500/40">${a}</span>`).join('');
  } else badgesSection.hidden = true;

  const recent = playerScores.slice(0,10).reverse();
  if(recent.length >= 2){
    chart.hidden = false;
    const chartC = document.getElementById('chart-container'); chartC.innerHTML = '';
    recent.forEach(s => {
      const bar = document.createElement('div');
      bar.className = 'chart-bar flex-1'; bar.style.height = s.percentage + '%'; bar.title = `${s.percentage}%`;
      chartC.appendChild(bar);
    });
  } else chart.hidden = true;
}

if(window.lucide) lucide.createIcons();
  </script>
 </body>
</html>
<script>
  // Daftar pertanyaan teka-teki
  const daftarSoal = [
    {
      pertanyaan: "Benda apa yang kalau dipotong malah jadi lebih tinggi?",
      pilihan: ["Celana", "Pohon", "Rumput", "Pensil"],
      jawaban: 0 // Indeks 0 = Celana
    },
    {
      pertanyaan: "Makin diisi, makin ringan. Apakah itu?",
      pilihan: ["Balon gas", "Ember", "Karung", "Dompet"],
      jawaban: 0 // Indeks 0 = Balon gas
    },
    {
      pertanyaan: "Apa yang selalu datang tapi tidak pernah tiba?",
      pilihan: ["Hujan", "Besok", "Tamu", "Pak paket"],
      jawaban: 1 // Indeks 1 = Besok
    },
    {
      pertanyaan: "Punya banyak gigi tapi tidak bisa menggigit?",
      pilihan: ["Hiu", "Sisir", "Gergaji", "Roda gigi"],
      jawaban: 1 // Indeks 1 = Sisir
    }
  ];

  let indexSekarang = 0;

  // Fungsi untuk menampilkan soal
  function tampilkanSoal() {
    const soal = daftarSoal[indexSekarang];
    
    // Ganti 'element-pertanyaan' dengan ID elemen tempat teks soal kamu
    document.getElementById("teks-soal").innerText = soal.pertanyaan;
    
    // Tampilkan pilihan jawaban ke tombol
    soal.pilihan.forEach((teks, i) => {
      document.getElementById(`btn-${i}`).innerText = teks;
    });
  }

  // Fungsi saat tombol jawaban diklik
  function pilihJawaban(indexPilihan) {
    if (indexPilihan === daftarSoal[indexSekarang].jawaban) {
      alert("Jawaban Benar! 🎉");
    } else {
      alert("Jawaban Salah! ❌");
    }

    // Pindah ke soal berikutnya
    indexSekarang++;

    if (indexSekarang < daftarSoal.length) {
      tampilkanSoal(); // Muat soal baru
    } else {
      alert("Kuis Selesai! Kamu hebat!");
      indexSekarang = 0; // Ulangi dari awal
      tampilkanSoal();
    }
  }

  // Jalankan pertama kali
  tampilkanSoal();
</script>

