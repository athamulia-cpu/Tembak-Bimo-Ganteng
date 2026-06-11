<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>🔫 Berburu Bimo • Pistol & Darah Hitam</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: linear-gradient(145deg, #1b3b1f 0%, #2a5a2f 100%);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Segoe UI', 'Fredoka', system-ui, sans-serif;
      padding: 20px;
      margin: 0;
      user-select: none;
      /* Kursor pistol custom (SVG) */
      cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='36' height='36' viewBox='0 0 36 36'><defs><filter id='shadow' x='-20%25' y='-20%25' width='140%25' height='140%25'><feDropShadow dx='1' dy='2' stdDeviation='1.5' flood-color='black' flood-opacity='0.6'/></filter></defs><g filter='url(%23shadow)' transform='rotate(-15 18 18)'><rect x='8' y='10' width='16' height='6' rx='3' fill='%23333' stroke='%23555' stroke-width='1.5'/><rect x='22' y='9' width='6' height='8' rx='2' fill='%23444' stroke='%23666' stroke-width='1.2'/><rect x='7' y='5' width='5' height='4' rx='1.5' fill='%23222'/><rect x='10' y='0' width='12' height='7' rx='3' fill='%232a2a2a' stroke='%23555' stroke-width='1.5'/><rect x='14' y='-2' width='4' height='3' rx='1' fill='%23111'/><circle cx='27' cy='13' r='3' fill='%23222' stroke='%23888' stroke-width='1.5'/><line x1='24' y1='15' x2='20' y2='18' stroke='%23555' stroke-width='2' stroke-linecap='round'/></g></svg>") 18 18, crosshair;
    }

    .game-container {
      background: #4a6b3a;
      border-radius: 60px 60px 40px 40px;
      box-shadow: 0 25px 35px rgba(0, 0, 0, 0.5), inset 0 0 15px #b6d7a8;
      padding: 25px 35px 35px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .arena {
      position: relative;
      width: 750px;
      height: 500px;
      background: radial-gradient(circle at 20% 30%, #b1cf8b, #3f6123);
      border-radius: 70px;
      box-shadow: inset 0 0 50px #1e3310, 0 15px 20px rgba(0,0,0,0.5);
      border: 5px solid #5b3e1e;
      overflow: hidden;
      cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='36' height='36' viewBox='0 0 36 36'><defs><filter id='shadow' x='-20%25' y='-20%25' width='140%25' height='140%25'><feDropShadow dx='1' dy='2' stdDeviation='1.5' flood-color='black' flood-opacity='0.6'/></filter></defs><g filter='url(%23shadow)' transform='rotate(-15 18 18)'><rect x='8' y='10' width='16' height='6' rx='3' fill='%23333' stroke='%23555' stroke-width='1.5'/><rect x='22' y='9' width='6' height='8' rx='2' fill='%23444' stroke='%23666' stroke-width='1.2'/><rect x='7' y='5' width='5' height='4' rx='1.5' fill='%23222'/><rect x='10' y='0' width='12' height='7' rx='3' fill='%232a2a2a' stroke='%23555' stroke-width='1.5'/><rect x='14' y='-2' width='4' height='3' rx='1' fill='%23111'/><circle cx='27' cy='13' r='3' fill='%23222' stroke='%23888' stroke-width='1.5'/><line x1='24' y1='15' x2='20' y2='18' stroke='%23555' stroke-width='2' stroke-linecap='round'/></g></svg>") 18 18, crosshair;
      margin-bottom: 25px;
    }

    /* Rumput dekoratif */
    .arena::before {
      content: "";
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 45%;
      background: linear-gradient(to top, #2d4a1a, transparent);
      pointer-events: none;
      border-radius: 0 0 65px 65px;
      z-index: 1;
    }

    /* Container untuk bercak darah */
    .darah-container {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 5;
    }

    /* Bercak darah hitam */
    .bercak-darah {
      position: absolute;
      background: radial-gradient(circle, #1a0a0a 10%, #0a0303 55%, transparent 80%);
      border-radius: 50%;
      opacity: 0;
      pointer-events: none;
      transform: scale(0.2);
      transition: opacity 0.3s ease-out, transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
      z-index: 5;
      filter: blur(1.5px);
    }

    .bercak-darah.muncul {
      opacity: 0.8;
      transform: scale(1);
      animation: pudarDarah 3.5s forwards;
    }

    @keyframes pudarDarah {
      0% { opacity: 0.8; transform: scale(1); }
      60% { opacity: 0.35; transform: scale(1.2); }
      100% { opacity: 0; transform: scale(0.7); }
    }

    .bimo {
      position: absolute;
      width: 75px;
      height: 90px;
      cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='36' height='36' viewBox='0 0 36 36'><defs><filter id='shadow' x='-20%25' y='-20%25' width='140%25' height='140%25'><feDropShadow dx='1' dy='2' stdDeviation='1.5' flood-color='black' flood-opacity='0.6'/></filter></defs><g filter='url(%23shadow)' transform='rotate(-15 18 18)'><rect x='8' y='10' width='16' height='6' rx='3' fill='%23333' stroke='%23555' stroke-width='1.5'/><rect x='22' y='9' width='6' height='8' rx='2' fill='%23444' stroke='%23666' stroke-width='1.2'/><rect x='7' y='5' width='5' height='4' rx='1.5' fill='%23222'/><rect x='10' y='0' width='12' height='7' rx='3' fill='%232a2a2a' stroke='%23555' stroke-width='1.5'/><rect x='14' y='-2' width='4' height='3' rx='1' fill='%23111'/><circle cx='27' cy='13' r='3' fill='%23222' stroke='%23888' stroke-width='1.5'/><line x1='24' y1='15' x2='20' y2='18' stroke='%23555' stroke-width='2' stroke-linecap='round'/></g></svg>") 18 18, pointer;
      transition: transform 0.08s ease, filter 0.1s, opacity 0.15s;
      z-index: 10;
      filter: drop-shadow(0 8px 6px rgba(0, 0, 0, 0.5));
      animation: floatBimo 2s infinite alternate ease-in-out;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    @keyframes floatBimo {
      0% { transform: translateY(0px) rotate(0deg); }
      100% { transform: translateY(-7px) rotate(1deg); }
    }

    .bimo:hover {
      filter: drop-shadow(0 0 20px rgb(255, 60, 60)) brightness(1.2);
      transform: scale(1.08);
    }

    .bimo:active {
      transform: scale(0.85);
      filter: drop-shadow(0 0 30px red);
    }

    /* Tulisan BIMO */
    .nama-bimo {
      font-weight: 900;
      font-size: 1rem;
      color: #fff9e0;
      background: #000000cc;
      padding: 3px 10px;
      border-radius: 20px;
      letter-spacing: 2px;
      margin-bottom: 2px;
      text-shadow: 0 0 8px black;
      box-shadow: 0 3px 0 #3d1e00;
      border: 1px solid #fca311;
      font-family: 'Arial Black', 'Impact', sans-serif;
      text-transform: uppercase;
      backdrop-filter: blur(2px);
      line-height: 1.2;
      white-space: nowrap;
    }

    .bimo-body {
      width: 65px;
      height: 68px;
      background: #1a1a1a;
      border-radius: 45% 45% 40% 40%;
      background: radial-gradient(circle at 30% 20%, #3d3d3d, #0a0a0a);
      box-shadow: 0 10px 0 #00000050, inset 0 -8px 0 #050505;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: relative;
    }

    .eyes {
      display: flex;
      gap: 14px;
      margin-top: 12px;
      margin-bottom: 4px;
    }

    .eye {
      width: 18px;
      height: 20px;
      background: white;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 0 8px rgba(255,255,200,0.9);
    }

    .pupil {
      width: 8px;
      height: 10px;
      background: #000;
      border-radius: 50%;
    }

    .mouth {
      width: 14px;
      height: 7px;
      background: #330000;
      border-radius: 0 0 20px 20px;
      margin-top: 2px;
      box-shadow: inset 0 2px 3px black;
    }

    /* Tidak ada cooldown indicator yang terlihat */
    .cooldown-indicator {
      display: none !important;
    }

    .skor-panel {
      background: #f7e3c0;
      border: 4px solid #5d3a1a;
      border-radius: 100px;
      padding: 15px 40px;
      display: flex;
      align-items: center;
      gap: 45px;
      box-shadow: 0 10px 0 #4b2e13, 0 15px 25px rgba(0,0,0,0.4);
      background: #eedbba;
      margin-bottom: 5px;
    }

    .skor-box {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .skor-label {
      font-size: 1.5rem;
      font-weight: bold;
      color: #2c4b1a;
      text-shadow: 1px 1px 0 white;
    }

    .skor-angka {
      background: #1e1e1e;
      color: #ffd966;
      font-size: 2.8rem;
      font-weight: 800;
      padding: 5px 22px;
      border-radius: 40px;
      box-shadow: inset 0 0 10px black, 0 4px 0 #5a3e1a;
      letter-spacing: 2px;
      font-family: 'Courier New', monospace;
    }

    .tombol {
      background: #fca311;
      border: none;
      color: #14213d;
      font-weight: bold;
      font-size: 1.3rem;
      padding: 12px 28px;
      border-radius: 40px;
      cursor: pointer;
      background: #ffb545;
      box-shadow: 0 7px 0 #b4641c, 0 8px 18px black;
      transition: 0.08s linear;
      text-transform: uppercase;
      letter-spacing: 1px;
      display: flex;
      align-items: center;
      gap: 6px;
      border: 2px solid #ffe3a5;
    }

    .tombol:active {
      transform: translateY(4px);
      box-shadow: 0 3px 0 #b4641c, 0 5px 12px black;
    }

    .info {
      color: #f9f3d9;
      font-weight: bold;
      margin-top: 10px;
      font-size: 1.05rem;
      text-shadow: 2px 2px 0 #1f3b0c;
      display: flex;
      gap: 25px;
      align-items: center;
    }

    .sound-toggle {
      background: #2e4a1c;
      color: white;
      border: none;
      padding: 5px 15px;
      border-radius: 20px;
      cursor: pointer;
      font-weight: bold;
      margin-left: 10px;
      box-shadow: 0 3px 0 #1a2e0e;
    }
  </style>
</head>
<body>
<div style="display: flex; flex-direction: column; align-items: center;">
  <div class="game-container">
    <div id="arenaBimo" class="arena">
      <!-- Container darah -->
      <div id="darahContainer" class="darah-container"></div>
      <!-- Bimo akan ditambahkan di sini -->
    </div>

    <div class="skor-panel">
      <div class="skor-box">
        <span class="skor-label">🎯 BIMO TERBIDIK</span>
        <span id="skorDisplay" class="skor-angka">0</span>
      </div>
      <button id="tombolReset" class="tombol">
        <span>↺</span> RESET SKOR
      </button>
    </div>
    <div class="info">
      <span>🔫 Klik Bimo | Bercak darah hitam | Pistol ready</span>
      <button id="soundToggle" class="sound-toggle">🔊 Sound ON</button>
    </div>
  </div>
</div>

<script>
  (function() {
    // --- KONFIGURASI ---
    const JUMLAH_BIMO = 10;
    const DELAY_RESPAWN = 3000; // 3 detik (tetap ada delay, tapi tanpa visual cooldown)
    const ARENA_ID = "arenaBimo";
    const SKOR_ID = "skorDisplay";
    const DARAH_CONTAINER_ID = "darahContainer";

    let skor = 0;
    let arenaEl = document.getElementById(ARENA_ID);
    let skorEl = document.getElementById(SKOR_ID);
    let darahContainer = document.getElementById(DARAH_CONTAINER_ID);
    let bimoElements = [];

    // Sound state
    let soundEnabled = true;
    let audioContext = null;

    // --- SOUND EFFECT DENGAN WEB AUDIO API (tembakan) ---
    function initAudioContext() {
      if (!audioContext) {
        try {
          audioContext = new (window.AudioContext || window.webkitAudioContext)();
        } catch (e) {
          console.warn("Web Audio API tidak didukung");
          soundEnabled = false;
        }
      }
      if (audioContext && audioContext.state === 'suspended') {
        audioContext.resume();
      }
    }

    function playShotSound() {
      if (!soundEnabled || !audioContext) return;
      
      try {
        if (audioContext.state === 'suspended') {
          audioContext.resume();
        }

        const now = audioContext.currentTime;

        // Oscillator untuk suara tembakan
        const osc = audioContext.createOscillator();
        const gainNode = audioContext.createGain();
        
        osc.type = 'square';
        osc.frequency.setValueAtTime(180, now);
        osc.frequency.exponentialRampToValueAtTime(50, now + 0.12);
        
        gainNode.gain.setValueAtTime(0.35, now);
        gainNode.gain.exponentialRampToValueAtTime(0.001, now + 0.18);
        
        osc.connect(gainNode);
        gainNode.connect(audioContext.destination);
        
        osc.start(now);
        osc.stop(now + 0.2);

        // White noise untuk efek letupan
        const bufferSize = audioContext.sampleRate * 0.15;
        const noiseBuffer = audioContext.createBuffer(1, bufferSize, audioContext.sampleRate);
        const data = noiseBuffer.getChannelData(0);
        for (let i = 0; i < bufferSize; i++) {
          data[i] = (Math.random() * 2 - 1) * 0.4;
        }
        const noise = audioContext.createBufferSource();
        noise.buffer = noiseBuffer;
        const noiseGain = audioContext.createGain();
        noiseGain.gain.setValueAtTime(0.25, now);
        noiseGain.gain.exponentialRampToValueAtTime(0.001, now + 0.13);
        noise.connect(noiseGain);
        noiseGain.connect(audioContext.destination);
        noise.start(now);
        noise.stop(now + 0.15);
        
      } catch (e) {
        console.warn("Gagal memutar suara:", e);
      }
    }

    // --- MEMBUAT ELEMEN BIMO ---
    function buatBimoElement() {
      const bimoDiv = document.createElement("div");
      bimoDiv.className = "bimo";

      bimoDiv.innerHTML = `
        <div class="nama-bimo">BIMO</div>
        <div class="bimo-body">
          <div class="eyes">
            <div class="eye"><div class="pupil"></div></div>
            <div class="eye"><div class="pupil"></div></div>
          </div>
          <div class="mouth"></div>
        </div>
      `;

      // Status cooldown (internal, tidak ditampilkan)
      bimoDiv.dataset.cooldown = "false";

      bimoDiv.addEventListener("click", function(e) {
        e.stopPropagation();
        tembakBimo(bimoDiv);
      });

      return bimoDiv;
    }

    // --- EFEK DARAH HITAM ---
    function buatBercakDarah(x, y) {
      if (!darahContainer) return;
      
      const bercak = document.createElement("div");
      bercak.className = "bercak-darah";
      
      // Ukuran acak antara 50px - 90px
      const size = 50 + Math.floor(Math.random() * 40);
      bercak.style.width = `${size}px`;
      bercak.style.height = `${size}px`;
      
      // Posisi
      bercak.style.left = `${x - size/2}px`;
      bercak.style.top = `${y - size/2}px`;
      
      darahContainer.appendChild(bercak);
      
      // Trigger animasi
      requestAnimationFrame(() => {
        bercak.classList.add("muncul");
      });
      
      // Hapus setelah animasi selesai
      setTimeout(() => {
        if (bercak.parentNode) {
          bercak.remove();
        }
      }, 3600);
    }

    // --- FUNGSI MENEMBAK BIMO ---
    function tembakBimo(bimoElement) {
      if (!bimoElement || !bimoElement.parentNode) return;
      
      // Cek cooldown (internal, tidak ada visual)
      if (bimoElement.dataset.cooldown === "true") {
        return; // Masih dalam delay, tidak bisa ditembak
      }

      // Ambil posisi untuk darah
      const rect = bimoElement.getBoundingClientRect();
      const arenaRect = arenaEl.getBoundingClientRect();
      const relX = rect.left - arenaRect.left + rect.width/2;
      const relY = rect.top - arenaRect.top + rect.height/2;

      // MAIN SOUND
      initAudioContext();
      playShotSound();

      // EFEK DARAH HITAM (langsung muncul, tanpa bayangan Bimo)
      buatBercakDarah(relX, relY);

      // Tambah skor
      skor += 1;
      perbaruiSkorUI();

      // Set cooldown (internal)
      bimoElement.dataset.cooldown = "true";
      
      // HILANGKAN BIMO SECARA INSTAN (tanpa animasi mengecil)
      bimoElement.style.transition = "0.05s ease";
      bimoElement.style.opacity = "0";
      bimoElement.style.pointerEvents = "none";
      bimoElement.style.transform = "scale(0.01)"; // hilang total

      // Delay 3 detik sebelum respawn (Bimo tidak terlihat)
      setTimeout(() => {
        if (bimoElement && bimoElement.parentNode) {
          // Kembalikan Bimo ke normal dan pindahkan posisi
          bimoElement.style.transition = "0.25s ease";
          bimoElement.style.opacity = "1";
          bimoElement.style.transform = "";
          bimoElement.style.pointerEvents = "auto";
          bimoElement.style.filter = "drop-shadow(0 8px 6px rgba(0, 0, 0, 0.5))";
          
          // Pindahkan ke posisi acak
          posisikanBimoAcak(bimoElement);
          
          // Reset cooldown
          bimoElement.dataset.cooldown = "false";
          
          // Animasi float ulang
          const durasi = 1.5 + Math.random() * 2.2;
          bimoElement.style.animation = `floatBimo ${durasi}s infinite alternate ease-in-out`;
          bimoElement.style.animationDelay = `${Math.random() * 1.2}s`;
        }
      }, DELAY_RESPAWN);
    }

    // --- UPDATE UI SKOR ---
    function perbaruiSkorUI() {
      if (skorEl) {
        skorEl.textContent = skor;
      }
    }

    // --- POSISI ACAK ---
    function dapatkanPosisiAcak() {
      if (!arenaEl) return { x: 50, y: 50 };
      const lebarArena = arenaEl.clientWidth;
      const tinggiArena = arenaEl.clientHeight;
      const padding = 25;
      const maxX = lebarArena - 85;
      const maxY = tinggiArena - 105;
      const x = Math.min(maxX, Math.max(padding, Math.floor(Math.random() * maxX)));
      const y = Math.min(maxY, Math.max(padding, Math.floor(Math.random() * maxY)));
      return { x, y };
    }

    function posisikanBimoAcak(bimoEl) {
      const { x, y } = dapatkanPosisiAcak();
      bimoEl.style.left = `${x}px`;
      bimoEl.style.top = `${y}px`;
    }

    // --- HAPUS & SPAWN ---
    function hapusSemuaBimo() {
      if (!arenaEl) return;
      const semuaBimo = arenaEl.querySelectorAll('.bimo');
      semuaBimo.forEach(el => el.remove());
      bimoElements = [];
    }

    function spawnSemuaBimo() {
      if (!arenaEl) return;
      hapusSemuaBimo();

      for (let i = 0; i < JUMLAH_BIMO; i++) {
        const bimoBaru = buatBimoElement();
        arenaEl.appendChild(bimoBaru);
        bimoElements.push(bimoBaru);
        posisikanBimoAcak(bimoBaru);

        const durasi = 1.5 + Math.random() * 2.3;
        bimoBaru.style.animation = `floatBimo ${durasi}s infinite alternate ease-in-out`;
        bimoBaru.style.animationDelay = `${Math.random() * 1.4}s`;
      }
    }

    // --- RESET GAME ---
    function resetGame() {
      skor = 0;
      perbaruiSkorUI();
      
      // Reset semua bimo
      bimoElements.forEach(bimo => {
        if (bimo && bimo.parentNode) {
          bimo.dataset.cooldown = "false";
          bimo.style.transition = "0.2s";
          bimo.style.opacity = "1";
          bimo.style.transform = "";
          bimo.style.pointerEvents = "auto";
          bimo.style.filter = "drop-shadow(0 8px 6px rgba(0, 0, 0, 0.5))";
          posisikanBimoAcak(bimo);
        }
      });

      // Bersihkan darah
      if (darahContainer) {
        darahContainer.innerHTML = '';
      }
    }

    // --- TOGGLE SOUND ---
    function setupSoundToggle() {
      const btn = document.getElementById("soundToggle");
      if (!btn) return;
      
      btn.addEventListener("click", () => {
        soundEnabled = !soundEnabled;
        if (soundEnabled) {
          btn.textContent = "🔊 Sound ON";
          btn.style.background = "#2e4a1c";
          initAudioContext();
        } else {
          btn.textContent = "🔇 Sound OFF";
          btn.style.background = "#5a2e2e";
        }
      });
    }

    // --- BIND EVENTS ---
    function bindEvents() {
      const tombolReset = document.getElementById("tombolReset");
      if (tombolReset) {
        tombolReset.addEventListener("click", resetGame);
      }

      setupSoundToggle();
      
      // Inisialisasi audio context pada interaksi pertama
      document.body.addEventListener('click', function once() {
        initAudioContext();
        document.body.removeEventListener('click', once);
      }, { once: true });
    }

    // --- MULAI GAME ---
    function mulaiGame() {
      arenaEl = document.getElementById(ARENA_ID);
      skorEl = document.getElementById(SKOR_ID);
      darahContainer = document.getElementById(DARAH_CONTAINER_ID);

      if (!arenaEl) {
        console.error("Arena tidak ditemukan!");
        return;
      }

      skor = 0;
      perbaruiSkorUI();
      spawnSemuaBimo();
      bindEvents();
    }

    window.addEventListener('DOMContentLoaded', mulaiGame);
  })();
</script>
</body>
</html>
