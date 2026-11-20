# karren12a7-ngoan
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>PulseWave — Trình Nghe Nhạc Hiện Đại</title>
  <style>
    :root {
      --bg-main: #07090e;
      --bg-panel: rgba(18, 22, 34, 0.78);
      --bg-card: rgba(28, 34, 52, 0.72);
      --accent: #7c5cff;
      --accent-2: #00e5ff;
      --text: #f3f5fa;
      --muted: #9aa3b8;
      --border: rgba(255, 255, 255, 0.09);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    }

    body {
      min-height: 100vh;
      background:
        radial-gradient(circle at 15% 15%, rgba(124, 92, 255, 0.25), transparent 42%),
        radial-gradient(circle at 85% 20%, rgba(0, 229, 255, 0.18), transparent 40%),
        var(--bg-main);
      color: var(--text);
      display: flex;
      justify-content: center;
      padding: 24px 16px 120px;
    }

    .app {
      width: 100%;
      max-width: 1080px;
      display: grid;
      grid-template-columns: 330px 1fr;
      gap: 24px;
    }

    @media (max-width: 840px) {
      .app { grid-template-columns: 1fr; }
    }

    .glass {
      background: var(--bg-panel);
      backdrop-filter: blur(18px);
      border: 1px solid var(--border);
      border-radius: 24px;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.45);
    }

    .now-playing {
      padding: 24px;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      height: fit-content;
    }

    .brand {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 22px;
      font-weight: 700;
    }

    .brand span {
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      font-size: 1.25rem;
    }

    .cover-wrap {
      width: 210px;
      height: 210px;
      border-radius: 26px;
      overflow: hidden;
      margin-bottom: 20px;
      box-shadow: 0 18px 36px rgba(124, 92, 255, 0.28);
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #2b1e5c, #0c3b4e);
      transition: transform 0.4s ease;
    }

    .playing .cover-wrap { transform: scale(1.03); }

    .cover-svg {
      width: 100%;
      height: 100%;
      display: block;
    }

    .track-title { font-size: 1.25rem; font-weight: 700; margin-bottom: 6px; }
    .track-artist { color: var(--muted); font-size: 0.92rem; margin-bottom: 18px; }

    .visualizer {
      display: flex;
      align-items: flex-end;
      justify-content: center;
      gap: 5px;
      height: 30px;
      margin-bottom: 10px;
    }

    .bar {
      width: 5px;
      height: 8px;
      border-radius: 999px;
      background: linear-gradient(180deg, var(--accent-2), var(--accent));
      transition: height 0.2s ease;
    }

    .playing .bar { animation: bounce 0.8s infinite ease-in-out; }
    .playing .bar:nth-child(2) { animation-delay: 0.15s; }
    .playing .bar:nth-child(3) { animation-delay: 0.3s; }
    .playing .bar:nth-child(4) { animation-delay: 0.1s; }
    .playing .bar:nth-child(5) { animation-delay: 0.25s; }
    .playing .bar:nth-child(6) { animation-delay: 0.35s; }

    @keyframes bounce {
      0%, 100% { height: 7px; }
      50% { height: 28px; }
    }

    .library { padding: 24px; }

    .top-bar {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }

    .top-bar h1 { font-size: 1.35rem; }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
    }

    .search-box {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 10px 14px;
      color: var(--text);
      width: 210px;
      outline: none;
    }

    .search-box:focus { border-color: var(--accent); }

    .upload-btn {
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      color: #07090e;
      font-weight: 700;
      font-size: 0.85rem;
      padding: 10px 16px;
      border-radius: 14px;
      cursor: pointer;
      border: none;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: opacity 0.2s;
    }

    .upload-btn:hover { opacity: 0.9; }

    .track-list { display: flex; flex-direction: column; gap: 10px; }

    .track-item {
      display: grid;
      grid-template-columns: 46px 1fr auto;
      gap: 14px;
      align-items: center;
      padding: 10px 14px;
      border-radius: 16px;
      background: var(--bg-card);
      border: 1px solid transparent;
      cursor: pointer;
      transition: 0.2s ease;
    }

    .track-item:hover {
      border-color: rgba(255, 255, 255, 0.16);
      transform: translateY(-1px);
    }

    .track-item.active {
      background: linear-gradient(90deg, rgba(124, 92, 255, 0.24), rgba(0, 229, 255, 0.08));
      border-color: rgba(124, 92, 255, 0.55);
    }

    .thumb {
      width: 46px;
      height: 46px;
      border-radius: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 1.1rem;
      color: #fff;
    }

    .track-meta .name { font-weight: 600; font-size: 0.96rem; margin-bottom: 2px; }
    .track-meta .artist { color: var(--muted); font-size: 0.84rem; }
    .track-duration { color: var(--muted); font-size: 0.85rem; }

    .dock {
      position: fixed;
      bottom: 16px;
      left: 50%;
      transform: translateX(-50%);
      width: calc(100% - 32px);
      max-width: 1080px;
      padding: 14px 22px;
      display: grid;
      grid-template-columns: auto 1fr auto;
      gap: 20px;
      align-items: center;
      z-index: 20;
    }

    @media (max-width: 660px) {
      .dock { grid-template-columns: 1fr; gap: 10px; text-align: center; }
    }

    .controls { display: flex; align-items: center; justify-content: center; gap: 10px; }

    button.ctrl {
      background: var(--bg-card);
      color: var(--text);
      border: 1px solid var(--border);
      border-radius: 999px;
      width: 38px;
      height: 38px;
      cursor: pointer;
      font-size: 0.95rem;
      transition: 0.2s;
    }

    button.ctrl:hover { border-color: var(--accent); }

    button.play-btn {
      width: 46px;
      height: 46px;
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      color: #07090e;
      font-weight: 700;
      border: none;
    }

    button.toggled { border-color: var(--accent-2); color: var(--accent-2); }

    .progress-wrap {
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 0.8rem;
      color: var(--muted);
    }

    input[type="range"] {
      width: 100%;
      accent-color: var(--accent);
      cursor: pointer;
    }

    .volume-wrap { display: flex; align-items: center; justify-content: center; gap: 8px; }
    .volume-wrap input { width: 90px; }
  </style>
</head>
<body>
  <div class="app" id="app">
    <aside class="glass now-playing">
      <div class="brand">
        <span>PulseWave</span>
        <small style="color: var(--muted);">STUDIO</small>
      </div>
      <div class="cover-wrap" id="coverWrap"></div>
      <div class="visualizer">
        <div class="bar"></div><div class="bar"></div><div class="bar"></div>
        <div class="bar"></div><div class="bar"></div><div class="bar"></div>
      </div>
      <div class="track-title" id="title">Midnight Synth</div>
      <div class="track-artist" id="artist">PulseWave Originals</div>
    </aside>

    <main class="glass library">
      <div class="top-bar">
        <h1>Danh sách phát</h1>
        <div class="actions">
          <input type="text" class="search-box" id="search" placeholder="Tìm bài hát..." />
          <label class="upload-btn" title="Chọn file nhạc MP3/WAV từ thiết bị">
            + Thêm nhạc từ máy
            <input type="file" id="fileInput" accept="audio/*" multiple hidden />
          </label>
        </div>
      </div>
      <div class="track-list" id="trackList"></div>
    </main>
  </div>

  <footer class="glass dock">
    <div class="controls">
      <button class="ctrl" id="shuffleBtn" title="Trộn bài">🔀</button>
      <button class="ctrl" id="prevBtn" title="Bài trước">⏮</button>
      <button class="ctrl play-btn" id="playBtn" title="Phát / Tạm dừng">▶</button>
      <button class="ctrl" id="nextBtn" title="Bài tiếp">⏭</button>
      <button class="ctrl" id="repeatBtn" title="Lặp lại">🔁</button>
    </div>

    <div class="progress-wrap">
      <span id="currentTime">0:00</span>
      <input type="range" id="progress" value="0" min="0" max="100" step="0.1" />
      <span id="duration">0:00</span>
    </div>

    <div class="volume-wrap">
      <span>🔊</span>
      <input type="range" id="volume" min="0" max="1" step="0.02" value="0.8" />
    </div>
  </footer>

  <audio id="audio"></audio>

  <script>
    // Bộ tạo âm thanh nền mặc định hoạt động ngoại tuyến 100% không cần mạng
    function createSynthTrackWav(notes, bpm) {
      const sampleRate = 22050;
      const beatDur = 60 / bpm;
      const totalSamples = Math.floor(sampleRate * beatDur * notes.length);
      const buffer = new ArrayBuffer(44 + totalSamples * 2);
      const view = new DataView(buffer);

      const writeStr = (offset, str) => {
        for (let i = 0; i < str.length; i++) view.setUint8(offset + i, str.charCodeAt(i));
      };

      writeStr(0, "RIFF");
      view.setUint32(4, 36 + totalSamples * 2, true);
      writeStr(8, "WAVE");
      writeStr(12, "fmt ");
      view.setUint32(16, 16, true);
      view.setUint16(20, 1, true);
      view.setUint16(22, 1, true);
      view.setUint32(24, sampleRate, true);
      view.setUint32(28, sampleRate * 2, true);
      view.setUint16(32, 2, true);
      view.setUint16(34, 16, true);
      writeStr(36, "data");
      view.setUint32(40, totalSamples * 2, true);

      let offset = 44;
      notes.forEach((freq) => {
        const samplesPerNote = Math.floor(sampleRate * beatDur);
        for (let i = 0; i < samplesPerNote; i++) {
          const t = i / sampleRate;
          const env = Math.min(1, i / 400) * Math.exp(-3.2 * (i / samplesPerNote));
          const val = freq > 0
            ? (Math.sin(2 * Math.PI * freq * t) * 0.65 + Math.sin(2 * Math.PI * freq * 0.5 * t) * 0.35) * env
            : 0;
          view.setInt16(offset, Math.max(-1, Math.min(1, val)) * 22000, true);
          offset += 2;
        }
      });

      const blob = new Blob([buffer], { type: "audio/wav" });
      return URL.createObjectURL(blob);
    }

    const tracks = [
      {
        title: "Midnight Neon (Demo)",
        artist: "PulseWave Synth",
        duration: "0:12",
        colors: ["#7c5cff", "#00e5ff"],
        src: createSynthTrackWav([261.63, 329.63, 392.00, 493.88, 523.25, 493.88, 392.00, 329.63, 293.66, 349.23, 440.00, 523.25, 493.88, 392.00, 329.63, 261.63], 80)
      },
      {
        title: "Morning Chill (Demo)",
        artist: "Lofi Studio",
        duration: "0:12",
        colors: ["#ff5e7e", "#ff9966"],
        src: createSynthTrackWav([349.23, 440.00, 523.25, 659.25, 587.33, 493.88, 392.00, 329.63, 349.23, 440.00, 523.25, 587.33, 523.25, 440.00, 392.00, 349.23], 80)
      },
      {
        title: "Deep Horizon (Demo)",
        artist: "Ambient Flow",
        duration: "0:12",
        colors: ["#00b09b", "#96c93d"],
        src: createSynthTrackWav([220.00, 261.63, 329.63, 392.00, 440.00, 392.00, 329.63, 261.63, 196.00, 246.94, 293.66, 392.00, 329.63, 293.66, 246.94, 220.00], 80)
      }
    ];

    const audio = document.getElementById("audio");
    const app = document.getElementById("app");
    const coverWrap = document.getElementById("coverWrap");
    const titleEl = document.getElementById("title");
    const artistEl = document.getElementById("artist");
    const trackListEl = document.getElementById("trackList");
    const searchEl = document.getElementById("search");
    const fileInput = document.getElementById("fileInput");
    const playBtn = document.getElementById("playBtn");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");
    const shuffleBtn = document.getElementById("shuffleBtn");
    const repeatBtn = document.getElementById("repeatBtn");
    const progressEl = document.getElementById("progress");
    const currentTimeEl = document.getElementById("currentTime");
    const durationEl = document.getElementById("duration");
    const volumeEl = document.getElementById("volume");

    let currentIndex = 0;
    let isShuffle = false;
    let isRepeat = false;

    const fmt = (sec) => {
      if (!Number.isFinite(sec)) return "0:00";
      const m = Math.floor(sec / 60);
      const s = Math.floor(sec % 60).toString().padStart(2, "0");
      return `${m}:${s}`;
    };

    function renderCoverSVG(colors) {
      return `
        <svg class="cover-svg" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="g" x1="0%" y1="0%" x2="100%" y2="100%">
              <stop offset="0%" stop-color="${colors[0]}" />
              <stop offset="100%" stop-color="${colors[1]}" />
            </linearGradient>
          </defs>
          <rect width="200" height="200" fill="url(#g)" />
          <circle cx="100" cy="100" r="62" fill="rgba(7,9,14,0.35)" />
          <circle cx="100" cy="100" r="22" fill="#07090e" />
          <circle cx="100" cy="100" r="7" fill="#ffffff" />
        </svg>
      `;
    }

    function renderList(filter = "") {
      const q = filter.trim().toLowerCase();
      trackListEl.innerHTML = "";
      tracks.forEach((t, idx) => {
        if (q && !t.title.toLowerCase().includes(q) && !t.artist.toLowerCase().includes(q)) return;
        const item = document.createElement("div");
        item.className = `track-item ${idx === currentIndex ? "active" : ""}`;
        item.innerHTML = `
          <div class="thumb" style="background: linear-gradient(135deg, ${t.colors[0]}, ${t.colors[1]})">♪</div>
          <div class="track-meta">
            <div class="name">${t.title}</div>
            <div class="artist">${t.artist}</div>
          </div>
          <div class="track-duration">${t.duration}</div>
        `;
        item.onclick = () => { loadTrack(idx); playAudio(); };
        trackListEl.appendChild(item);
      });
    }

    function loadTrack(index) {
      currentIndex = (index + tracks.length) % tracks.length;
      const t = tracks[currentIndex];
      audio.src = t.src;
      coverWrap.innerHTML = renderCoverSVG(t.colors);
      titleEl.textContent = t.title;
      artistEl.textContent = t.artist;
      renderList(searchEl.value);
    }

    function playAudio() {
      audio.play().then(() => {
        playBtn.textContent = "⏸";
        app.classList.add("playing");
      }).catch(() => {});
    }

    function pauseAudio() {
      audio.pause();
      playBtn.textContent = "▶";
      app.classList.remove("playing");
    }

    playBtn.onclick = () => (audio.paused ? playAudio() : pauseAudio());
    prevBtn.onclick = () => { loadTrack(currentIndex - 1); playAudio(); };
    nextBtn.onclick = () => {
      if (isShuffle && tracks.length > 1) {
        let nextIdx;
        do { nextIdx = Math.floor(Math.random() * tracks.length); } while (nextIdx === currentIndex);
        loadTrack(nextIdx);
      } else {
        loadTrack(currentIndex + 1);
      }
      playAudio();
    };

    shuffleBtn.onclick = () => {
      isShuffle = !isShuffle;
      shuffleBtn.classList.toggle("toggled", isShuffle);
    };

    repeatBtn.onclick = () => {
      isRepeat = !isRepeat;
      repeatBtn.classList.toggle("toggled", isRepeat);
    };

    audio.ontimeupdate = () => {
      if (!audio.duration) return;
      progressEl.value = (audio.currentTime / audio.duration) * 100;
      currentTimeEl.textContent = fmt(audio.currentTime);
      durationEl.textContent = fmt(audio.duration);
    };

    progressEl.oninput = () => {
      if (!audio.duration) return;
      audio.currentTime = (progressEl.value / 100) * audio.duration;
    };

    volumeEl.oninput = () => { audio.volume = volumeEl.value; };

    audio.onended = () => {
      if (isRepeat) {
        audio.currentTime = 0;
        playAudio();
      } else {
        nextBtn.click();
      }
    };

    searchEl.oninput = (e) => renderList(e.target.value);

    // Tải file MP3/WAV trực tiếp từ máy của người dùng
    fileInput.onchange = (e) => {
      const files = Array.from(e.target.files);
      if (!files.length) return;
      const palettes = [
        ["#8e2de2", "#4a00e0"],
        ["#00c6ff", "#0072ff"],
        ["#f953c6", "#b91d73"],
        ["#11998e", "#38ef7d"]
      ];
      files.forEach((file, i) => {
        const url = URL.createObjectURL(file);
        const cleanName = file.name.replace(/\.[^/.]+$/, "");
        tracks.push({
          title: cleanName,
          artist: "Nhạc cá nhân",
          duration: "Local",
          colors: palettes[(tracks.length + i) % palettes.length],
          src: url
        });
      });
      loadTrack(tracks.length - files.length);
      playAudio();
    };

    audio.volume = volumeEl.value;
    loadTrack(0);
  </script>
</body>
</html>
