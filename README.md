<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>배드민턴 스매싱 분석기</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

  <style>
    :root{
      --bg: #0b1020;
      --card: rgba(255,255,255,0.06);
      --card2: rgba(255,255,255,0.10);
      --stroke: rgba(255,255,255,0.12);
      --text: rgba(255,255,255,0.92);
      --muted: rgba(255,255,255,0.65);
      --accent: #7c3aed; /* purple */
      --accent2:#22c55e; /* green */
      --danger:#ef4444;
      --shadow: 0 18px 50px rgba(0,0,0,0.35);
      --radius: 18px;
    }

    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color: var(--text);
      background:
        radial-gradient(1200px 600px at 15% 10%, rgba(124,58,237,0.35), transparent 60%),
        radial-gradient(900px 500px at 80% 20%, rgba(34,197,94,0.25), transparent 55%),
        radial-gradient(900px 700px at 50% 100%, rgba(59,130,246,0.15), transparent 55%),
        var(--bg);
      min-height:100vh;
    }

    .wrap{
      max-width: 980px;
      margin: 0 auto;
      padding: 28px 18px 60px;
    }

    .header{
      display:flex;
      flex-direction:column;
      gap:10px;
      margin-bottom: 18px;
    }
    .title{
      font-size: 28px;
      font-weight: 800;
      letter-spacing: -0.02em;
      margin:0;
      display:flex;
      align-items:center;
      gap:10px;
    }
    .badge{
      display:inline-flex;
      align-items:center;
      gap:8px;
      padding: 6px 10px;
      border-radius: 999px;
      background: rgba(255,255,255,0.10);
      border: 1px solid var(--stroke);
      color: var(--muted);
      font-size: 12px;
      width: fit-content;
    }
    .grid{
      display:grid;
      grid-template-columns: 1fr;
      gap: 16px;
    }
    @media (min-width: 900px){
      .grid{ grid-template-columns: 360px 1fr; }
    }

    .card{
      background: var(--card);
      border: 1px solid var(--stroke);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .card .inner{ padding: 16px; }
    .card h2{
      margin:0 0 10px;
      font-size: 16px;
      letter-spacing:-0.01em;
    }
    .hint{ color: var(--muted); font-size: 13px; line-height: 1.45; }

    ul.tips{
      margin: 10px 0 0;
      padding-left: 18px;
      color: var(--muted);
      font-size: 13px;
      line-height: 1.55;
    }

    .controls{
      display:flex;
      flex-direction:column;
      gap:10px;
      margin-top: 12px;
    }
    input[type="file"]{
      width: 100%;
      padding: 12px;
      border-radius: 14px;
      border: 1px dashed rgba(255,255,255,0.25);
      background: rgba(0,0,0,0.15);
      color: var(--muted);
    }

    .row{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
    }
    button{
      border:0;
      border-radius: 14px;
      padding: 12px 14px;
      font-weight: 700;
      cursor:pointer;
      transition: transform .08s ease, filter .12s ease, opacity .12s ease;
      display:inline-flex;
      align-items:center;
      gap:10px;
    }
    button:active{ transform: translateY(1px); }
    .btn-primary{
      background: linear-gradient(135deg, rgba(124,58,237,1), rgba(59,130,246,1));
      color: white;
    }
    .btn-ghost{
      background: rgba(255,255,255,0.08);
      border: 1px solid var(--stroke);
      color: var(--text);
    }
    .btn-danger{
      background: rgba(239,68,68,0.18);
      border: 1px solid rgba(239,68,68,0.35);
      color: #fecaca;
    }
    button[disabled]{ opacity:0.55; cursor:not-allowed; }

    .status{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      padding: 12px 14px;
      border-top: 1px solid var(--stroke);
      background: rgba(255,255,255,0.03);
      color: var(--muted);
      font-size: 13px;
    }
    .dot{
      width:10px; height:10px; border-radius:999px;
      background: rgba(255,255,255,0.35);
      box-shadow: 0 0 0 6px rgba(255,255,255,0.05);
    }
    .dot.live{ background: var(--accent2); box-shadow: 0 0 0 6px rgba(34,197,94,0.18); }
    .dot.err{ background: var(--danger); box-shadow: 0 0 0 6px rgba(239,68,68,0.18); }

    /* Video area */
    #videoContainer{
      position: relative;
      width: 100%;
      border-radius: var(--radius);
      overflow:hidden;
      border:1px solid var(--stroke);
      background: rgba(0,0,0,0.25);
      box-shadow: var(--shadow);
      min-height: 220px;
    }
    video{ width: 100%; height: auto; display:block; }
    canvas{
      position:absolute;
      inset:0;
      width:100%;
      height:100%;
      pointer-events:none;
    }
    .overlayHUD{
      position:absolute;
      left:14px;
      top:14px;
      display:flex;
      flex-direction:column;
      gap:8px;
      z-index:5;
    }
    .pill{
      width: fit-content;
      padding: 8px 10px;
      border-radius: 999px;
      background: rgba(0,0,0,0.35);
      border: 1px solid rgba(255,255,255,0.16);
      backdrop-filter: blur(8px);
      font-size: 12px;
      color: var(--text);
      display:flex;
      align-items:center;
      gap:8px;
    }

    /* Result */
    #results{
      display:none;
      margin-top: 14px;
    }
    .resultGrid{
      display:grid;
      grid-template-columns: 1fr;
      gap: 12px;
    }
    @media(min-width: 700px){
      .resultGrid{ grid-template-columns: 1fr 1fr; }
    }
    .metric{
      background: var(--card2);
      border: 1px solid var(--stroke);
      border-radius: 16px;
      padding: 14px;
    }
    .metric .label{ color: var(--muted); font-size: 12px; }
    .metric .value{ font-size: 26px; font-weight: 900; margin-top: 6px; }
    .metric .sub{ color: var(--muted); font-size: 12px; margin-top: 4px; }

    .feedbackBox{
      margin-top: 12px;
      padding: 14px;
      border-radius: 16px;
      background: rgba(255,255,255,0.06);
      border: 1px solid var(--stroke);
      color: var(--text);
      line-height: 1.55;
    }

    #userForm{ display:none; margin-top: 12px; }
    .formRow{
      display:flex;
      gap: 10px;
      flex-wrap:wrap;
      margin-top: 10px;
    }
    .input{
      flex: 1;
      min-width: 200px;
      padding: 12px;
      border-radius: 14px;
      border: 1px solid var(--stroke);
      background: rgba(0,0,0,0.18);
      color: var(--text);
      outline:none;
    }
    .input::placeholder{ color: rgba(255,255,255,0.40); }

    #ranking{ margin-top: 18px; }
    #rankingList{ list-style:none; padding:0; margin: 8px 0 0; }
    #rankingList li{
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px solid var(--stroke);
      background: rgba(255,255,255,0.04);
      margin: 8px 0;
      color: var(--muted);
    }

    .small{
      font-size:12px;
      color: var(--muted);
    }
  </style>
</head>

<body>
  <div class="wrap">
    <div class="header">
      <h1 class="title">🏸 배드민턴 스매싱 분석기</h1>
      <div class="badge">권장: 720p↑ · 30fps↑ · 측면/45도 · 5~10초 · 100MB↓</div>
    </div>

    <div class="grid">
      <!-- Left: Upload / Controls -->
      <div class="card">
        <div class="inner">
          <h2>업로드 & 실행</h2>
          <div class="hint">
            영상 업로드 후 <b>분석 시작</b>을 누르면, 분석이 끝났을 때 <b>최종 결과만</b> 보여줘요.
          </div>

          <ul class="tips">
            <li>밝은 조명 + 어두운 배경이면 인식 안정적</li>
            <li>팔/손목이 프레임 밖으로 나가면 속도 추정이 흔들려요</li>
          </ul>

          <div class="controls">
            <input type="file" id="videoUpload" accept="video/mp4,video/quicktime,video/webm" />
            <div class="row">
              <button class="btn-primary" id="startButton">🚀 분석 시작</button>
              <button class="btn-ghost" id="resetButton" disabled>↩️ 초기화</button>
              <button class="btn-danger" id="stopButton" disabled>⏹️ 중지</button>
            </div>
            <div class="small">※ 업로드한 파일은 브라우저에서만 처리(클라이언트 분석)됩니다.</div>
          </div>
        </div>

        <div class="status">
          <div style="display:flex; align-items:center; gap:10px;">
            <span class="dot" id="statusDot"></span>
            <span id="statusText">대기 중</span>
          </div>
          <span id="loading" style="display:none; font-weight:800; color: rgba(255,255,255,0.85);">분석 중…</span>
        </div>
      </div>

      <!-- Right: Video -->
      <div>
        <div id="videoContainer">
          <div class="overlayHUD">
            <div class="pill" id="hudPill">⏳ 업로드 후 분석 시작</div>
          </div>
          <video id="video" autoplay muted playsinline></video>
          <canvas id="output_canvas"></canvas>
        </div>

        <!-- Results -->
        <div class="card" id="results">
          <div class="inner" id="resultsInner">
            <!-- 리렌더로 교체될 영역 -->
          </div>
        </div>

        <!-- Ranking form -->
        <div class="card" id="userForm">
          <div class="inner">
            <h2>랭킹 등록</h2>
            <div class="hint">최종 점수를 랭킹에 올릴 수 있어요.</div>
            <div class="formRow">
              <input class="input" type="text" id="nickname" placeholder="닉네임" />
              <input class="input" type="text" id="instagram" placeholder="인스타그램 ID (예: @user)" />
            </div>
            <div class="row" style="margin-top:10px;">
              <button class="btn-primary" id="saveScoreButton">🏆 등록</button>
            </div>
          </div>
        </div>

        <!-- Ranking -->
        <div class="card" id="ranking">
          <div class="inner">
            <h2>랭킹 TOP 10</h2>
            <ul id="rankingList"></ul>
          </div>
        </div>
      </div>
    </div>
  </div>

<script type="module">
  import { PoseLandmarker, FilesetResolver, DrawingUtils } from "https://cdn.skypack.dev/@mediapipe/tasks-vision@0.10.0";
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.14.1/firebase-app.js";
  import { getDatabase, ref, push, onValue } from "https://www.gstatic.com/firebasejs/10.14.1/firebase-database.js";

  // Firebase 초기화
  const firebaseConfig = {
    apiKey: "AIzaSyCuv6H6jyABFX3jpBoA13PLMz5hxI_Pbt8",
    authDomain: "badmi-581a6.firebaseapp.com",
    databaseURL: "https://badmi-581a6-default-rtdb.firebaseio.com",
    projectId: "badmi-581a6",
    storageBucket: "badmi-581a6.firebasestorage.app",
    messagingSenderId: "272556457679",
    appId: "1:272556457679:web:3326840f45d7ca2048c1ba",
    measurementId: "G-CNMTT9RMY8"
  };
  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  // DOM
  const $ = (id) => document.getElementById(id);
  const statusDot = $("statusDot");
  const statusText = $("statusText");
  const loadingEl = $("loading");
  const hudPill = $("hudPill");

  const videoEl = $("video");
  const canvasEl = $("output_canvas");
  const ctx = canvasEl.getContext("2d");

  const startBtn = $("startButton");
  const stopBtn = $("stopButton");
  const resetBtn = $("resetButton");

  // html2canvas guard
  if (typeof html2canvas === "undefined") {
    console.error("html2canvas is not loaded. Check CDN.");
    alert("이미지 저장 기능 로드 실패. CDN을 확인하세요.");
  }

  // Pose
  let poseLandmarker;
  let drawingUtils;
  let isAnalyzing = false;
  let lastVideoTime = -1;

  // Aggregation (최종 점수만 내기 위한 누적)
  let lastWristPos = null;
  let speedSamples = [];
  let scoreSamples = [];
  let maxSpeed = 0;
  let finalScore = null;
  let finalSpeed = null;

  function setStatus(mode, text){
    statusText.textContent = text;
    statusDot.classList.remove("live","err");
    if (mode === "live") statusDot.classList.add("live");
    if (mode === "err") statusDot.classList.add("err");
  }

  function setHUD(text){
    hudPill.textContent = text;
  }

  function setLoading(on){
    loadingEl.style.display = on ? "inline" : "none";
  }

  function clamp(n, a, b){ return Math.max(a, Math.min(b, n)); }

  // (중요) 결과 카드를 "새로 생성"하는 방식 (리렌더)
  function renderFinalResults({ score, speed, feedback }){
    const el = $("resultsInner");

    el.innerHTML = `
      <h2>최종 분석 결과</h2>
      <div class="resultGrid">
        <div class="metric">
          <div class="label">최종 자세 점수</div>
          <div class="value">${score}<span style="font-size:16px; font-weight:700; color: rgba(255,255,255,0.70);"> / 10</span></div>
          <div class="sub">영상 전체에서 안정 구간 중심으로 계산</div>
        </div>
        <div class="metric">
          <div class="label">추정 스매싱 속도</div>
          <div class="value">${speed}<span style="font-size:16px; font-weight:700; color: rgba(255,255,255,0.70);"> km/h</span></div>
          <div class="sub">손목 이동 기반 단순 추정 (참고용)</div>
        </div>
      </div>

      <div class="feedbackBox">${feedback}</div>

      <div class="row" style="margin-top: 12px;">
        <button class="btn-ghost" id="downloadButton">📸 결과 이미지 다운로드</button>
      </div>
    `;

    // 렌더 후 이벤트 다시 바인딩
    $("downloadButton").addEventListener("click", downloadImage);
  }

  function getFeedback(score, speed){
    let fb =
      score >= 8 ? "우수한 자세예요. " :
      score >= 5 ? "전반적으로 양호하지만, 몇 군데만 다듬으면 더 좋아요. " :
      "자세 교정 포인트가 꽤 있어요. ";
    fb += `최대 추정 속도는 ${speed}km/h로 기록됐어요. `;
    fb += "팁: (1) 임팩트 순간 팔 각도 (2) 상체 균형 (3) 무릎 굽힘/체중 이동을 체크해보세요.";
    return fb;
  }

  async function initPoseLandmarker(){
    try{
      const vision = await FilesetResolver.forVisionTasks(
        "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.0/wasm"
      );
      poseLandmarker = await PoseLandmarker.createFromOptions(vision, {
        baseOptions: {
          modelAssetPath: "https://storage.googleapis.com/mediapipe-models/pose_landmarker/pose_landmarker_heavy/float16/latest/pose_landmarker_heavy.task",
          delegate: "GPU",
        },
        runningMode: "VIDEO",
        numPoses: 1,
        minPoseDetectionConfidence: 0.3,
        minPosePresenceConfidence: 0.3,
        minTrackingConfidence: 0.3,
        outputWorldLandmarks: true
      });
      drawingUtils = new DrawingUtils(ctx);
      return true;
    }catch(e){
      console.error("PoseLandmarker init failed:", e);
      setStatus("err","초기화 실패");
      alert("PoseLandmarker 초기화 실패. 콘솔(F12)을 확인하거나 Chrome 최신 버전을 사용해보세요.");
      return false;
    }
  }

  function calculateAngle(a,b,c){
    const ab = { x: a.x - b.x, y: a.y - b.y };
    const cb = { x: c.x - b.x, y: c.y - b.y };
    const dot = ab.x*cb.x + ab.y*cb.y;
    const modAb = Math.hypot(ab.x, ab.y);
    const modCb = Math.hypot(cb.x, cb.y);
    if (modAb === 0 || modCb === 0) return 0;
    const cos = clamp(dot / (modAb*modCb), -1, 1);
    return Math.acos(cos) * 180 / Math.PI;
  }

  // 프레임별 점수는 "누적"만 하고, 화면에는 최종만 보여줌
  function sampleScoreAndSpeed(landmarks){
    let score = 10;

    const shoulder = landmarks[12] || {x:0,y:0};
    const elbow    = landmarks[14] || {x:0,y:0};
    const wrist    = landmarks[16] || {x:0,y:0};

    const armAngle = calculateAngle(shoulder, elbow, wrist);
    if (armAngle > 150 || armAngle < 90) score -= 2;

    const hip = landmarks[24] || {x:0,y:0};
    const torsoAngle = Math.atan2(shoulder.y - hip.y, shoulder.x - hip.x) * 180 / Math.PI;
    if (Math.abs(torsoAngle) > 20) score -= 1;

    const hipLeft   = landmarks[23] || {x:0,y:0};
    const kneeLeft  = landmarks[25] || {x:0,y:0};
    const ankleLeft = landmarks[27] || {x:0,y:0};
    const legAngle = calculateAngle(hipLeft, kneeLeft, ankleLeft);
    if (legAngle > 120) score -= 1;

    score = clamp(score, 0, 10);

    // speed sample
    let speed = 0;
    if (lastWristPos){
      const dx = wrist.x - lastWristPos.x;
      const dy = wrist.y - lastWristPos.y;
      const dist = Math.hypot(dx, dy);
      speed = Math.round(dist * 60 * 0.036);
      speed = clamp(speed, 0, 300);
      maxSpeed = Math.max(maxSpeed, speed);
      speedSamples.push(speed);
    }
    lastWristPos = {x: wrist.x, y: wrist.y};

    scoreSamples.push(score);
  }

  // 최종 점수 산정 (추천)
  // - 영상 전체 평균은 노이즈가 섞일 수 있어서,
  //   상위 30% 점수 구간 평균을 "최종 자세 점수"로 쓰면 꽤 그럴듯해짐.
  function computeFinalScore(){
    if (scoreSamples.length === 0) return 0;

    const sorted = [...scoreSamples].sort((a,b)=>b-a);
    const topCount = Math.max(5, Math.floor(sorted.length * 0.30));
    const top = sorted.slice(0, topCount);
    const avgTop = top.reduce((s,v)=>s+v,0) / top.length;

    return Math.round(avgTop * 10) / 10; // 소수 1자리
  }

  function computeFinalSpeed(){
    // 최종 속도는 "최대값"이 UX상 직관적
    return maxSpeed || 0;
  }

  async function analyzeLoop(){
    if (!isAnalyzing || !poseLandmarker) return;

    const nowInMs = performance.now();
    if (videoEl.currentTime === lastVideoTime){
      requestAnimationFrame(analyzeLoop);
      return;
    }
    lastVideoTime = videoEl.currentTime;

    ctx.save();
    ctx.clearRect(0,0,canvasEl.width, canvasEl.height);
    ctx.drawImage(videoEl, 0,0, canvasEl.width, canvasEl.height);

    try{
      const results = await poseLandmarker.detectForVideo(videoEl, nowInMs);
      if (results.landmarks && results.landmarks.length > 0){
        const landmarks = results.landmarks[0];
        drawingUtils.drawLandmarks(landmarks, { color:"#ff4d4d", radius:4 });
        drawingUtils.drawConnectors(landmarks, PoseLandmarker.POSE_CONNECTIONS, { color:"#22c55e", lineWidth:2 });
        sampleScoreAndSpeed(landmarks);
      }
    }catch(e){
      console.error("Detection failed:", e);
    }

    ctx.restore();
    requestAnimationFrame(analyzeLoop);
  }

  async function startAnalysis(){
    // validate file
    const file = $("videoUpload").files[0];
    if (!file){
      alert("영상을 업로드하세요.");
      return;
    }
    if (file.size > 100 * 1024 * 1024){
      alert("파일 크기는 100MB 이하여야 합니다.");
      return;
    }

    // reset aggregation
    lastWristPos = null;
    speedSamples = [];
    scoreSamples = [];
    maxSpeed = 0;
    finalScore = null;
    finalSpeed = null;

    // hide results until end
    $("results").style.display = "none";
    $("userForm").style.display = "none";

    // init
    setStatus("live","초기화 중");
    setHUD("⚙️ 모델 로딩 중…");
    setLoading(true);

    if (!poseLandmarker){
      const ok = await initPoseLandmarker();
      if (!ok){
        setLoading(false);
        return;
      }
    }

    // load video
    const src = URL.createObjectURL(file);
    videoEl.src = src;

    videoEl.onloadedmetadata = () => {
      canvasEl.width = videoEl.videoWidth;
      canvasEl.height = videoEl.videoHeight;

      isAnalyzing = true;
      lastVideoTime = -1;

      setStatus("live","분석 중");
      setHUD("🔎 분석 중… (결과는 영상 끝나면 표시)");
      setLoading(true);

      startBtn.disabled = true;
      stopBtn.disabled = false;
      resetBtn.disabled = true;

      videoEl.play().catch(e => console.error("Video play failed:", e));
      requestAnimationFrame(analyzeLoop);
    };

    videoEl.onended = () => finishAnalysis();
  }

  function finishAnalysis(){
    isAnalyzing = false;
    setLoading(false);
    setStatus("live","분석 완료");
    setHUD("✅ 분석 완료! 아래에서 최종 결과 확인");

    finalScore = computeFinalScore();
    finalSpeed = computeFinalSpeed();

    // 결과 카드 "리렌더" (요청한 방식)
    renderFinalResults({
      score: finalScore,
      speed: finalSpeed,
      feedback: getFeedback(finalScore, finalSpeed)
    });

    $("results").style.display = "block";
    $("userForm").style.display = "block";

    startBtn.disabled = false;
    stopBtn.disabled = true;
    resetBtn.disabled = false;
  }

  function stopAnalysis(){
    if (!isAnalyzing) return;
    isAnalyzing = false;
    setLoading(false);
    setStatus("err","중지됨");
    setHUD("⏹️ 중지됨 (다시 시작 가능)");
    startBtn.disabled = false;
    stopBtn.disabled = true;
    resetBtn.disabled = false;
  }

  function resetAll(){
    stopAnalysis();
    // clear canvas overlay
    ctx.clearRect(0,0,canvasEl.width, canvasEl.height);
    // reset video
    videoEl.pause();
    videoEl.src = "";
    $("videoUpload").value = "";
    $("results").style.display = "none";
    $("userForm").style.display = "none";
    setStatus("","대기 중");
    setHUD("⏳ 업로드 후 분석 시작");
    resetBtn.disabled = true;
  }

  function downloadImage(){
    // 결과 카드만 캡쳐 (원하면 전체 wrap 캡쳐로 바꿔도 됨)
    html2canvas($("results")).then(canvas => {
      const link = document.createElement("a");
      link.download = "badminton_smash_analysis.png";
      link.href = canvas.toDataURL("image/png");
      link.click();
    }).catch(e => {
      console.error("html2canvas failed:", e);
      alert("이미지 저장 실패. 콘솔을 확인하세요.");
    });
  }

  function saveScore(){
    const nickname = $("nickname").value.trim();
    const instagram = $("instagram").value.trim();
    if (!nickname || !instagram){
      alert("닉네임과 인스타그램 ID를 입력하세요.");
      return;
    }
    if (finalScore === null){
      alert("분석을 먼저 완료해주세요.");
      return;
    }
    const data = {
      score: finalScore,
      nickname,
      instagram,
      timestamp: Date.now()
    };
    push(ref(db, "scores"), data)
      .then(() => {
        alert("등록 완료! 랭킹을 확인하세요.");
        $("userForm").style.display = "none";
      })
      .catch(e => {
        console.error("Failed to save score:", e);
        alert("랭킹 등록 실패. 콘솔을 확인하세요.");
      });
  }

  // Ranking fetch
  onValue(ref(db, "scores"), snapshot => {
    const list = $("rankingList");
    list.innerHTML = "";
    const ranks = [];
    snapshot.forEach(child => ranks.push(child.val()));
    ranks.sort((a,b)=> (b.score - a.score) || (b.timestamp - a.timestamp));
    ranks.slice(0,10).forEach((r, i) => {
      const li = document.createElement("li");
      li.textContent = `${i+1}위: ${r.nickname} (${r.instagram}) - 점수: ${r.score}`;
      list.appendChild(li);
    });
  }, err => console.error("Failed to fetch rankings:", err));

  // Events
  $("videoUpload").addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (file && file.size > 100 * 1024 * 1024){
      alert("파일 크기는 100MB 이하여야 합니다.");
      e.target.value = "";
    }
    resetBtn.disabled = !file;
  });

  startBtn.addEventListener("click", startAnalysis);
  stopBtn.addEventListener("click", stopAnalysis);
  resetBtn.addEventListener("click", resetAll);
  $("saveScoreButton").addEventListener("click", saveScore);

  // Initial status
  setStatus("", "대기 중");
</script>
</body>
</html>
