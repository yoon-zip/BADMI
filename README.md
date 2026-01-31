<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>badmi | Smash Analyzer</title>

  <!-- Google Analytics (GA4) -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-CNMTT9RMY8"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-CNMTT9RMY8');
  </script>

  <!-- Google AdSense (site-wide loader) -->
  <script async
    src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-3392083121307952"
    crossorigin="anonymous"></script>

  <style>
    :root{
      --bg:#0b1020;
      --card: rgba(255,255,255,0.06);
      --card2: rgba(255,255,255,0.10);
      --stroke: rgba(255,255,255,0.12);
      --text: rgba(255,255,255,0.92);
      --muted: rgba(255,255,255,0.65);
      --accent:#7c3aed;
      --accent2:#22c55e;
      --danger:#ef4444;
      --shadow: 0 18px 50px rgba(0,0,0,0.35);
      --radius: 18px;
    }
    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:var(--text);
      background:
        radial-gradient(900px 500px at 10% 10%, rgba(124,58,237,0.35), transparent 60%),
        radial-gradient(800px 450px at 90% 20%, rgba(34,197,94,0.22), transparent 55%),
        radial-gradient(900px 700px at 50% 110%, rgba(59,130,246,0.12), transparent 55%),
        var(--bg);
      min-height:100vh;
    }

    .wrap{
      max-width: 760px;
      margin: 0 auto;
      padding: 16px 14px 118px;
    }

    .topbar{
      position: sticky;
      top: 0;
      z-index: 20;
      padding: 10px 0 12px;
      background: linear-gradient(180deg, rgba(11,16,32,0.92), rgba(11,16,32,0.55), rgba(11,16,32,0));
      backdrop-filter: blur(8px);
    }

    .titleRow{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
    }
    .brand{
      display:flex;
      flex-direction:column;
      gap:4px;
      min-width: 0;
    }
    h1{
      margin:0;
      font-size: 18px;
      font-weight: 950;
      letter-spacing:-0.02em;
      white-space: nowrap;
      overflow:hidden;
      text-overflow: ellipsis;
    }
    .sub{
      font-size: 11px;
      color: var(--muted);
    }

    .lang-switch{
      display:flex;
      gap:6px;
      flex-shrink:0;
    }
    .lang-switch button{
      background: rgba(255,255,255,0.08);
      border: 1px solid var(--stroke);
      color: var(--text);
      padding: 8px 10px;
      border-radius: 12px;
      font-weight: 900;
      cursor:pointer;
      font-size: 12px;
      line-height: 1;
    }
    .lang-switch button.active{
      background: linear-gradient(135deg, rgba(124,58,237,1), rgba(59,130,246,1));
      border-color: rgba(255,255,255,0.18);
    }

    .card{
      background: var(--card);
      border: 1px solid var(--stroke);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
      margin-top: 12px;
    }
    .inner{ padding: 14px; }
    .sectionTitle{
      margin:0 0 8px;
      font-size: 14px;
      font-weight: 900;
      letter-spacing:-0.01em;
    }
    .hint{ color: var(--muted); font-size: 13px; line-height:1.5; }
    .small{ color: var(--muted); font-size: 12px; }

    details{
      border: 1px solid var(--stroke);
      border-radius: 14px;
      background: rgba(0,0,0,0.16);
      padding: 10px 12px;
      margin-top: 10px;
    }
    summary{
      cursor:pointer;
      font-weight: 900;
      font-size: 13px;
      color: var(--text);
      list-style:none;
    }
    summary::-webkit-details-marker{ display:none; }
    .tips{
      margin: 10px 0 0;
      padding-left: 18px;
      color: var(--muted);
      font-size: 13px;
      line-height:1.55;
    }

    /* Video 9:16 */
    #videoContainer{
      position: relative;
      width: 100%;
      border-radius: var(--radius);
      overflow:hidden;
      border: 1px solid var(--stroke);
      background: rgba(0,0,0,0.25);
      box-shadow: var(--shadow);
    }
    #videoContainer::before{
      content:"";
      display:block;
      padding-top: 177.78%; /* 9:16 */
    }
    video, canvas{
      position:absolute;
      inset:0;
      width:100%;
      height:100%;
      object-fit: cover;
    }
    canvas{ pointer-events:none; }

    .hud{
      position:absolute;
      left:12px;
      top:12px;
      z-index:5;
      display:flex;
      flex-direction:column;
      gap:8px;
    }
    .pill{
      width: fit-content;
      padding: 8px 10px;
      border-radius: 999px;
      background: rgba(0,0,0,0.36);
      border: 1px solid rgba(255,255,255,0.16);
      backdrop-filter: blur(8px);
      font-size: 12px;
      color: var(--text);
      display:flex;
      align-items:center;
      gap:8px;
    }

    .playOverlay{
      position:absolute;
      inset:0;
      z-index:10;
      display:grid;
      place-items:center;
      border:0;
      background: rgba(0,0,0,0.30);
      color: rgba(255,255,255,0.95);
      font-size: 18px;
      font-weight: 950;
      cursor:pointer;
    }
    .playOverlay .bubble{
      display:flex;
      align-items:center;
      gap:10px;
      padding: 14px 18px;
      border-radius: 999px;
      background: rgba(255,255,255,0.10);
      border: 1px solid rgba(255,255,255,0.18);
      backdrop-filter: blur(8px);
    }
    .playOverlay.hidden{ display:none; }

    /* Status line */
    .statusLine{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap: 12px;
      margin-top: 10px;
      color: var(--muted);
      font-size: 12px;
    }
    .dot{
      width:10px; height:10px; border-radius:999px;
      background: rgba(255,255,255,0.35);
      box-shadow: 0 0 0 6px rgba(255,255,255,0.05);
    }
    .dot.live{ background: var(--accent2); box-shadow: 0 0 0 6px rgba(34,197,94,0.18); }
    .dot.err{ background: var(--danger); box-shadow: 0 0 0 6px rgba(239,68,68,0.18); }

    /* Results */
    #results{ display:none; }
    .resultGrid{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-top: 10px;
    }
    .metric{
      background: var(--card2);
      border: 1px solid var(--stroke);
      border-radius: 16px;
      padding: 12px;
    }
    .metric .label{ color: var(--muted); font-size: 11px; }
    .metric .value{ margin-top: 6px; font-size: 24px; font-weight: 950; }
    .metric .unit{ font-size: 13px; font-weight: 800; color: rgba(255,255,255,0.70); }

    .feedbackBox{
      margin-top: 10px;
      padding: 12px;
      border-radius: 16px;
      background: rgba(255,255,255,0.06);
      border: 1px solid var(--stroke);
      line-height: 1.55;
      font-size: 13px;
    }
    .frameThumb{
      width:100%;
      border-radius: 16px;
      border: 1px solid rgba(255,255,255,0.14);
      overflow:hidden;
      background: rgba(0,0,0,0.20);
    }
    .frameThumb img{
      width:100%;
      display:block;
    }

    /* Sticky action bar */
    .actionBar{
      position: fixed;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: 50;
      padding: 10px 14px calc(10px + env(safe-area-inset-bottom));
      background: rgba(11,16,32,0.75);
      border-top: 1px solid rgba(255,255,255,0.10);
      backdrop-filter: blur(10px);
    }
    .actionRow{
      max-width: 760px;
      margin: 0 auto;
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }
    button.action{
      border:0;
      border-radius: 14px;
      padding: 14px 14px;
      font-weight: 950;
      cursor:pointer;
      transition: transform .08s ease, opacity .12s ease;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      gap:10px;
      font-size: 14px;
    }
    button.action:active{ transform: translateY(1px); }
    button.action[disabled]{ opacity:0.55; cursor:not-allowed; }

    .btn-primary{
      background: linear-gradient(135deg, rgba(124,58,237,1), rgba(59,130,246,1));
      color:white;
    }
    .btn-ghost{
      background: rgba(255,255,255,0.08);
      border: 1px solid var(--stroke);
      color: var(--text);
    }

    @media (min-width: 900px){
      h1{ font-size: 20px; }
    }
  </style>
</head>

<body>
  <div class="wrap">
    <div class="topbar">
      <div class="titleRow">
        <div class="brand">
          <h1 id="t_title">🏸 배드민턴 스매싱 분석기</h1>
          <div class="sub" id="t_sub">세로 촬영 권장(촬영 팁 아래 클릭)</div>
        </div>

        <div class="lang-switch" aria-label="language switch">
          <button type="button" data-lang="ko" class="active">KR</button>
          <button type="button" data-lang="en">EN</button>
          <button type="button" data-lang="zh">中文</button>
        </div>
      </div>
    </div>

    <!-- Upload -->
    <div class="card">
      <div class="inner">
        <div class="sectionTitle" id="t_step1">1) 영상 업로드</div>
        <div class="hint" id="t_step1_hint">
          업로드 후 아래 하단의 <b>분석 시작</b>을 누르고,
          신체 분석이 잘 되었나 보고 싶다면 <b>재생</b> 버튼을 눌러주세요.
          결과는 금방 나와요.
        </div>

        <div style="margin-top:10px;">
          <input type="file" id="videoUpload" accept="video/mp4,video/quicktime,video/webm" style="width:100%;" />
        </div>

        <details>
          <summary id="t_tips_title">촬영 팁 보기</summary>
          <ul class="tips" id="t_tips_list"></ul>
        </details>

        <div class="statusLine">
          <div style="display:flex; align-items:center; gap:10px;">
            <span class="dot" id="statusDot"></span>
            <span id="statusText">대기 중</span>
          </div>
          <span id="loading" style="display:none; font-weight:950; color: rgba(255,255,255,0.85);">분석 중…</span>
        </div>

        <div class="small" style="margin-top:10px;" id="t_privacy">
          ※ 업로드한 영상은 브라우저에서만 처리됩니다.
        </div>
      </div>
    </div>

    <!-- Video -->
    <div class="card">
      <div class="inner">
        <div class="sectionTitle" id="t_step2">2) 하단 ‘분석 시작’ 버튼 누르기</div>
        <div class="hint" id="t_step2_hint">
          영상 가이드에 맞게 찍어주셔야 분석이 잘 되어요.
        </div>
      </div>

      <div id="videoContainer">
        <div class="hud">
          <div class="pill" id="hudPill">⏳ 열심히 분석할 준비 중이에요</div>
        </div>

        <button id="playOverlay" class="playOverlay" type="button">
          <div class="bubble">▶ <span id="t_play">재생</span></div>
        </button>

        <video id="video" muted playsinline></video>
        <canvas id="output_canvas"></canvas>
      </div>
    </div>

    <!-- Results -->
    <div class="card" id="results">
      <div class="inner" id="resultsInner"></div>
    </div>
  </div>

  <!-- Sticky action bar -->
  <div class="actionBar">
    <div class="actionRow">
      <button class="action btn-primary" id="startButton">⚙️ <span id="t_start">분석 시작</span></button>
      <button class="action btn-ghost" id="resetButton" disabled>↩️ <span id="t_reset">초기화</span></button>
    </div>
  </div>

<script type="module">
  import { PoseLandmarker, FilesetResolver, DrawingUtils } from "https://cdn.skypack.dev/@mediapipe/tasks-vision@0.10.0";

  // Helpers
  const $ = (id) => document.getElementById(id);
  function safeGtag(...args){
    try{ if (typeof gtag === "function") gtag(...args); }catch(e){}
  }
  function clamp(n,a,b){ return Math.max(a, Math.min(b,n)); }

  // i18n (✅ KO 문구 변경사항 기준으로 EN/中文도 동기화)
  const i18n = {
    ko: {
      title: "🏸 배드민턴 스매싱 분석기",
      sub: "세로 촬영 권장(촬영 팁 아래 클릭)",
      step1: "1) 영상 업로드",
      step1_hint_html:
        "업로드 후 아래 하단의 <b>분석 시작</b>을 누르고, " +
        "신체 분석이 잘 되었나 보고 싶다면 <b>재생</b> 버튼을 눌러주세요. " +
        "결과는 금방 나와요.",
      tips_title: "촬영 팁 보기",
      step2: "2) 하단 ‘분석 시작’ 버튼 누르기",
      step2_hint: "영상 가이드에 맞게 찍어주셔야 분석이 잘 되어요.",
      play: "재생",
      start: "분석 시작",
      reset: "초기화",
      privacy: "※ 업로드한 영상은 브라우저에서만 처리됩니다.",
      hud_wait: "⏳ 열심히 분석할 준비 중이에요",
      hud_ready: "✅ 준비 완료! 영상 위 재생을 눌러주세요",
      hud_analyzing: "🔎 분석 중… (끝나면 결과 표시)",
      hud_done: "✅ 분석 완료! 아래 결과 확인",
      status_idle: "대기 중",
      status_ready: "준비 완료",
      status_analyzing: "분석 중",
      status_done: "분석 완료",
      status_initfail: "초기화 실패",
      status_nofile: "영상이 없어요",
      result_title: "✅ 분석 결과",
      score: "자세 점수",
      speed: "추정 손목 속도",
      save_story: "📱 스토리 이미지 저장 (1080×1920)",
      feedback_label: "피드백",
      tips_list: [
        "세로 촬영 권장",
        "전신(발~라켓 팔 손목)이 프레임 안에 들어오게",
        "밝은 조명 + 어두운 배경이 인식 안정적",
        "정면 보다는 측면 추천",
        "5~10초, 100MB 이하"
      ],
      alerts: {
        upload: "영상을 업로드하세요.",
        max: "파일 크기는 100MB 이하여야 합니다.",
        initfail: "초기화 실패. 브라우저 콘솔(F12) 확인 또는 Chrome 최신 버전 사용을 권장합니다.",
        playfail: "재생에 실패했어요. 모바일은 사용자 제스처가 필요할 수 있어요."
      },
      save_done_title: "✅ 저장 완료!",
      save_done_body:
        "스토리 이미지가 저장됐어요.\n\n" +
        "iPhone: 파일 앱 > 다운로드에서 확인 → 공유 버튼 → 사진에 저장(또는 인스타로 공유)\n" +
        "Android: 다운로드(Downloads) 폴더에서 확인 → 갤러리에 표시됨(기기별 상이)"
    },

    en: {
      title: "🏸 Badminton Smash Analyzer",
      sub: "Vertical filming recommended (tap Filming Tips below)",
      step1: "1) Upload Video",
      step1_hint_html:
        "After uploading, tap <b>Start Analysis</b> at the bottom. " +
        "If you want to check whether body tracking works well, press <b>Play</b>. " +
        "Results appear quickly.",
      tips_title: "Filming Tips",
      step2: "2) Tap ‘Start Analysis’ at the bottom",
      step2_hint: "Please follow the filming guide for stable tracking.",
      play: "Play",
      start: "Start Analysis",
      reset: "Reset",
      privacy: "※ Your video is processed locally in your browser.",
      hud_wait: "⏳ Getting ready to analyze…",
      hud_ready: "✅ Ready! Press Play on the video",
      hud_analyzing: "🔎 Analyzing… (results after it ends)",
      hud_done: "✅ Done! Check results below",
      status_idle: "Idle",
      status_ready: "Ready",
      status_analyzing: "Analyzing",
      status_done: "Completed",
      status_initfail: "Init failed",
      status_nofile: "No video",
      result_title: "✅ Result",
      score: "Posture Score",
      speed: "Estimated Wrist Speed",
      save_story: "📱 Save Story Image (1080×1920)",
      feedback_label: "Feedback",
      tips_list: [
        "Vertical (9:16) recommended",
        "Keep full body in frame (feet → hitting wrist)",
        "Bright lighting + dark background helps tracking",
        "Side view is better than front view",
        "5–10 seconds, under 100MB"
      ],
      alerts: {
        upload: "Please upload a video.",
        max: "File must be under 100MB.",
        initfail: "Initialization failed. Check console or use the latest Chrome.",
        playfail: "Playback failed. Mobile may require a user gesture."
      },
      save_done_title: "✅ Saved!",
      save_done_body:
        "Your story image has been saved.\n\n" +
        "iPhone: Files app > Downloads → Share → Save Image (or share to Instagram)\n" +
        "Android: Check Downloads folder → it may appear in Gallery (varies by device)"
    },

    zh: {
      title: "🏸 羽毛球扣杀分析器",
      sub: "建议竖屏拍摄（点击下方拍摄提示）",
      step1: "1) 上传视频",
      step1_hint_html:
        "上传后点击底部的 <b>开始分析</b>。 " +
        "如果想确认身体追踪是否正常，请点击 <b>播放</b>。 " +
        "结果会很快出现。",
      tips_title: "拍摄提示",
      step2: "2) 点击底部“开始分析”按钮",
      step2_hint: "请按拍摄指南录制，识别会更稳定。",
      play: "播放",
      start: "开始分析",
      reset: "重置",
      privacy: "※ 视频仅在浏览器本地处理。",
      hud_wait: "⏳ 正在准备分析…",
      hud_ready: "✅ 已就绪！点击视频播放",
      hud_analyzing: "🔎 分析中…（结束后出结果）",
      hud_done: "✅ 完成！查看下方结果",
      status_idle: "待机",
      status_ready: "已就绪",
      status_analyzing: "分析中",
      status_done: "已完成",
      status_initfail: "初始化失败",
      status_nofile: "未选择视频",
      result_title: "✅ 分析结果",
      score: "动作评分",
      speed: "估计手腕速度",
      save_story: "📱 保存Story图片 (1080×1920)",
      feedback_label: "反馈",
      tips_list: [
        "建议竖屏拍摄",
        "全身入镜（脚 → 挥拍手腕）",
        "明亮光线 + 深色背景更稳定",
        "侧面比正面更推荐",
        "5–10秒，低于100MB"
      ],
      alerts: {
        upload: "请先上传视频。",
        max: "文件需小于100MB。",
        initfail: "初始化失败。请查看控制台或使用最新版Chrome。",
        playfail: "播放失败。手机端可能需要用户点击触发播放。"
      },
      save_done_title: "✅ 已保存！",
      save_done_body:
        "Story 图片已保存。\n\n" +
        "iPhone：文件App > 下载 → 分享 → 存储到照片（或分享到 Instagram）\n" +
        "Android：查看 Downloads 文件夹 → 可能会显示在相册（因机型而异）"
    }
  };

  // UI refs
  const statusDot = $("statusDot");
  const statusText = $("statusText");
  const loadingEl = $("loading");
  const hudPill = $("hudPill");
  const startBtn = $("startButton");
  const resetBtn = $("resetButton");

  const videoEl = $("video");
  const canvasEl = $("output_canvas");
  const ctx = canvasEl.getContext("2d");
  const playOverlay = $("playOverlay");

  function setStatus(mode, text){
    statusText.textContent = text;
    statusDot.classList.remove("live","err");
    if (mode === "live") statusDot.classList.add("live");
    if (mode === "err") statusDot.classList.add("err");
  }
  function setHUD(text){ hudPill.textContent = text; }
  function setLoading(on){ loadingEl.style.display = on ? "inline" : "none"; }

  // Language
  let currentLang = "ko";
  function setLang(lang){
    currentLang = lang;
    const t = i18n[lang];

    $("t_title").textContent = t.title;
    $("t_sub").textContent = t.sub;
    $("t_step1").textContent = t.step1;
    $("t_step1_hint").innerHTML = t.step1_hint_html;
    $("t_tips_title").textContent = t.tips_title;
    $("t_step2").textContent = t.step2;
    $("t_step2_hint").textContent = t.step2_hint;
    $("t_play").textContent = t.play;
    $("t_start").textContent = t.start;
    $("t_reset").textContent = t.reset;
    $("t_privacy").textContent = t.privacy;

    const tipsUl = $("t_tips_list");
    tipsUl.innerHTML = "";
    t.tips_list.forEach(x=>{
      const li = document.createElement("li");
      li.textContent = x;
      tipsUl.appendChild(li);
    });

    document.querySelectorAll(".lang-switch button").forEach(b=>{
      b.classList.toggle("active", b.dataset.lang === lang);
    });

    // HUD/Status text도 언어에 맞춰 자연스럽게 업데이트
    if (!prepared) {
      setHUD(t.hud_wait);
      setStatus("", t.status_idle);
    }

    safeGtag("event","change_language",{ lang });
  }
  document.querySelectorAll(".lang-switch button").forEach(btn=>{
    btn.addEventListener("click", ()=> setLang(btn.dataset.lang));
  });

  // Pose
  let poseLandmarker = null;
  let drawingUtils = null;
  let prepared = false;
  let isAnalyzing = false;
  let lastVideoTime = -1;

  // Speed sampling (world landmarks)
  let lastWrist3d = null;
  let lastTimeSec = null;
  let maxSpeedKmh = 0;

  // Score samples (simple, stable MVP)
  let scoreSamples = [];

  // final capture
  let finalPoseFrameUrl = null;
  let finalScore = 0;
  let finalSpeed = 0;

  async function initPoseLandmarker(){
    try{
      const vision = await FilesetResolver.forVisionTasks(
        "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.0/wasm"
      );
      poseLandmarker = await PoseLandmarker.createFromOptions(vision, {
        baseOptions: {
          modelAssetPath: "https://storage.googleapis.com/mediapipe-models/pose_landmarker/pose_landmarker_heavy/float16/latest/pose_landmarker_heavy.task",
          delegate: "GPU"
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
      setStatus("err", i18n[currentLang].status_initfail);
      alert(i18n[currentLang].alerts.initfail);
      return false;
    }
  }

  function calculateScoreStable(landmarks){
    // 아주 거친 MVP 스코어(0~10): 너무 튀지 않게 안정성 위주
    // - 팔꿈치가 어깨보다 지나치게 낮으면 감점
    // - 무릎이 완전히 펴져 있으면 감점
    let s = 10;

    const shoulder = landmarks[12] || {x:0,y:0};
    const elbow    = landmarks[14] || {x:0,y:0};
    const hipL     = landmarks[23] || {x:0,y:0};
    const kneeL    = landmarks[25] || {x:0,y:0};
    const ankleL   = landmarks[27] || {x:0,y:0};

    if (elbow.y > shoulder.y + 0.08) s -= 3; // elbow too low
    const legAngle = calculateAngle(hipL, kneeL, ankleL);
    if (legAngle > 165) s -= 2; // leg too straight
    if (legAngle < 75) s -= 1;  // too deep (rare)

    return clamp(s, 0, 10);
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

  function computeFinalScore(){
    if (scoreSamples.length === 0) return 0;
    // 상위 30% 평균(너무 흔들리지 않게)
    const sorted = [...scoreSamples].sort((a,b)=>b-a);
    const topCount = Math.max(6, Math.floor(sorted.length * 0.30));
    const top = sorted.slice(0, topCount);
    const avgTop = top.reduce((sum,v)=>sum+v,0) / top.length;
    return Math.round(avgTop * 10) / 10;
  }

  function sampleWristSpeedFromWorld(results){
    const wl = results.worldLandmarks?.[0];
    if (!wl) return;
    const wrist = wl[16];
    const t = videoEl.currentTime;

    if (lastWrist3d && lastTimeSec != null){
      const dt = t - lastTimeSec;
      if (dt > 0.0001){
        const dx = wrist.x - lastWrist3d.x;
        const dy = wrist.y - lastWrist3d.y;
        const dz = wrist.z - lastWrist3d.z;
        const v_ms = Math.sqrt(dx*dx + dy*dy + dz*dz) / dt;
        const v_kmh = Math.round(v_ms * 3.6);
        maxSpeedKmh = Math.max(maxSpeedKmh, Math.min(300, v_kmh));
      }
    }
    lastWrist3d = wrist;
    lastTimeSec = t;
  }

  function getFeedback(score, speed){
    if (currentLang === "ko"){
      let fb =
        score >= 8 ? "좋아요! 자세가 안정적이에요. " :
        score >= 5 ? "괜찮아요! 몇 포인트만 다듬으면 더 좋아져요. " :
        "조금만 수정하면 확 좋아질 수 있어요. ";
      fb += `최고 추정 손목 속도는 ${speed}km/h예요. `;
      fb += "팁: 팔꿈치를 더 높게, 무릎은 살짝 굽히고, 몸 중심을 유지해보세요.";
      return fb;
    }
    if (currentLang === "zh"){
      let fb =
        score >= 8 ? "很好！动作很稳定。 " :
        score >= 5 ? "不错！再调整几个点会更好。 " :
        "稍微调整就能提升很多。 ";
      fb += `最高估计手腕速度：${speed} km/h。 `;
      fb += "建议：抬高肘部、膝盖微屈、保持重心稳定。";
      return fb;
    }
    // EN
    let fb =
      score >= 8 ? "Nice! Your form looks stable. " :
      score >= 5 ? "Good! A few tweaks will make it better. " :
      "A couple of adjustments can help a lot. ";
    fb += `Peak estimated wrist speed: ${speed} km/h. `;
    fb += "Tip: keep your elbow higher, bend knees slightly, and stay balanced.";
    return fb;
  }

  // Prepare / Load video
  async function startAnalysis(){
    const t = i18n[currentLang];
    const file = $("videoUpload").files[0];
    if (!file){ alert(t.alerts.upload); setStatus("err", t.status_nofile); return; }
    if (file.size > 100 * 1024 * 1024){ alert(t.alerts.max); return; }

    // reset session
    prepared = false;
    isAnalyzing = false;
    lastVideoTime = -1;
    scoreSamples = [];
    lastWrist3d = null;
    lastTimeSec = null;
    maxSpeedKmh = 0;
    finalPoseFrameUrl = null;

    $("results").style.display = "none";
    playOverlay.classList.remove("hidden");

    setHUD(t.hud_wait);
    setStatus("live", t.status_analyzing);
    setLoading(true);

    safeGtag("event","analysis_start_click",{ lang: currentLang });

    if (!poseLandmarker){
      const ok = await initPoseLandmarker();
      if (!ok){ setLoading(false); return; }
    }

    // load video
    const src = URL.createObjectURL(file);
    videoEl.src = src;
    videoEl.load();

    videoEl.onloadedmetadata = () => {
      canvasEl.width = videoEl.videoWidth;
      canvasEl.height = videoEl.videoHeight;

      prepared = true;
      setLoading(false);
      setStatus("live", t.status_ready);
      setHUD(t.hud_ready);

      resetBtn.disabled = false;
      safeGtag("event","analysis_ready",{ lang: currentLang });
    };
  }

  // Play + analyze loop
  async function startPlayback(){
    const t = i18n[currentLang];
    if (!prepared) return;

    try{
      await videoEl.play();
      playOverlay.classList.add("hidden");

      isAnalyzing = true;
      setStatus("live", t.status_analyzing);
      setHUD(t.hud_analyzing);
      setLoading(true);

      safeGtag("event","video_play",{ lang: currentLang });
      requestAnimationFrame(analyzeLoop);
    }catch(e){
      console.error(e);
      alert(t.alerts.playfail);
    }
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
        const lm = results.landmarks[0];

        drawingUtils.drawLandmarks(lm, { color:"#ff4d4d", radius:4 });
        drawingUtils.drawConnectors(lm, PoseLandmarker.POSE_CONNECTIONS, { color:"#22c55e", lineWidth:2 });

        scoreSamples.push(calculateScoreStable(lm));
        sampleWristSpeedFromWorld(results);
      }
    }catch(e){
      console.error("Detection failed:", e);
    }

    ctx.restore();
    requestAnimationFrame(analyzeLoop);
  }

  function finishAnalysis(){
    const t = i18n[currentLang];

    isAnalyzing = false;
    setLoading(false);

    finalScore = computeFinalScore();
    finalSpeed = maxSpeedKmh || 0;
    finalPoseFrameUrl = canvasEl.toDataURL("image/png");

    const feedback = getFeedback(finalScore, finalSpeed);
    renderResults(finalScore, finalSpeed, feedback, finalPoseFrameUrl);

    $("results").style.display = "block";
    setStatus("live", t.status_done);
    setHUD(t.hud_done);

    $("results").scrollIntoView({ behavior:"smooth", block:"start" });
    safeGtag("event","analysis_complete",{ score: finalScore, speed: finalSpeed, lang: currentLang });
  }

  function renderResults(score, speed, feedback, poseFrameUrl){
    const t = i18n[currentLang];

    $("resultsInner").innerHTML = `
      <div class="sectionTitle">${t.result_title}</div>

      <div class="frameThumb" style="margin-top:10px;">
        <img src="${poseFrameUrl}" alt="pose frame" />
      </div>

      <div class="resultGrid">
        <div class="metric">
          <div class="label">${t.score}</div>
          <div class="value">${score}<span class="unit"> / 10</span></div>
        </div>
        <div class="metric">
          <div class="label">${t.speed}</div>
          <div class="value">${speed}<span class="unit"> km/h</span></div>
        </div>
      </div>

      <div class="feedbackBox">
        <b>${t.feedback_label}:</b><br/>
        ${feedback}
      </div>

      <div style="margin-top:10px;">
        <button class="action btn-ghost" id="downloadStoryButton" style="width:100%;">${t.save_story}</button>
      </div>
    `;

    $("downloadStoryButton").addEventListener("click", async () => {
      await downloadStoryImage({ poseFrameUrl, score, speed, feedback });
    });
  }

  // Story image generator (1080x1920)
  async function downloadStoryImage({ poseFrameUrl, score, speed, feedback }){
    const t = i18n[currentLang];
    const W = 1080, H = 1920;
    const out = document.createElement("canvas");
    out.width = W; out.height = H;
    const c = out.getContext("2d");

    // bg
    const g = c.createLinearGradient(0,0,W,H);
    g.addColorStop(0, "#0b1020");
    g.addColorStop(1, "#1b1450");
    c.fillStyle = g;
    c.fillRect(0,0,W,H);

    // title
    c.fillStyle = "rgba(255,255,255,0.92)";
    c.font = "900 54px system-ui, -apple-system, Segoe UI, Roboto";
    c.fillText(
      currentLang === "ko" ? "🏸 스매싱 분석 결과" :
      currentLang === "zh" ? "🏸 扣杀分析结果" :
      "🏸 Smash Analysis",
      70, 140
    );

    // frame
    const img = await loadImage(poseFrameUrl);
    const cardX = 70, cardY = 210, cardW = 940, cardH = 1040;

    c.save();
    roundRectPath(c, cardX, cardY, cardW, cardH, 36);
    c.clip();
    const crop = coverCrop(img.width, img.height, cardW, cardH);
    c.drawImage(img, crop.sx, crop.sy, crop.sw, crop.sh, cardX, cardY, cardW, cardH);
    c.restore();

    // pills
    drawPill(c, 70, 1320, 450, 150,
      currentLang === "ko" ? `자세 점수  ${score}/10` :
      currentLang === "zh" ? `动作评分  ${score}/10` :
      `Score  ${score}/10`
    );
    drawPill(c, 560, 1320, 450, 150,
      currentLang === "ko" ? `추정 속도  ${speed} km/h` :
      currentLang === "zh" ? `估计速度  ${speed} km/h` :
      `Speed  ${speed} km/h`
    );

    // feedback
    c.fillStyle = "rgba(255,255,255,0.86)";
    c.font = "700 34px system-ui, -apple-system, Segoe UI, Roboto";
    wrapText(c, feedback, 70, 1530, 940, 44, 6);

    // footer
    c.fillStyle = "rgba(255,255,255,0.45)";
    c.font = "700 26px system-ui, -apple-system, Segoe UI, Roboto";
    c.fillText("Made with badmi", 70, 1860);

    const link = document.createElement("a");
    link.download = "smash_story.png";
    link.href = out.toDataURL("image/png");
    link.click();

    safeGtag("event","download_story_image",{ score, speed, lang: currentLang });

    // ✅ 저장 완료 팝업 + 갤러리/파일 안내
    setTimeout(() => {
      alert(`${t.save_done_title}\n\n${t.save_done_body}`);
    }, 50);
  }

  function loadImage(src){
    return new Promise((res, rej) => {
      const i = new Image();
      i.onload = () => res(i);
      i.onerror = rej;
      i.src = src;
    });
  }
  function coverCrop(iw, ih, tw, th){
    const ir = iw/ih, tr = tw/th;
    let sw, sh, sx, sy;
    if (ir > tr){
      sh = ih;
      sw = ih * tr;
      sx = (iw - sw)/2;
      sy = 0;
    } else {
      sw = iw;
      sh = iw / tr;
      sx = 0;
      sy = (ih - sh)/2;
    }
    return { sx, sy, sw, sh };
  }
  function roundRectPath(ctx, x, y, w, h, r){
    ctx.beginPath();
    ctx.moveTo(x+r, y);
    ctx.arcTo(x+w, y, x+w, y+h, r);
    ctx.arcTo(x+w, y+h, x, y+h, r);
    ctx.arcTo(x, y+h, x, y, r);
    ctx.arcTo(x, y, x+w, y, r);
    ctx.closePath();
  }
  function drawPill(ctx, x, y, w, h, text){
    ctx.save();
    roundRectPath(ctx, x, y, w, h, 30);
    ctx.fillStyle = "rgba(255,255,255,0.12)";
    ctx.fill();
    ctx.strokeStyle = "rgba(255,255,255,0.18)";
    ctx.lineWidth = 2;
    ctx.stroke();

    ctx.fillStyle = "rgba(255,255,255,0.92)";
    ctx.font = "900 36px system-ui, -apple-system, Segoe UI, Roboto";
    ctx.fillText(text, x+26, y+92);
    ctx.restore();
  }
  function wrapText(ctx, text, x, y, maxWidth, lineHeight, maxLines){
    const words = String(text).split(" ");
    let line = "";
    let lines = 0;

    for (let n=0; n<words.length; n++){
      const test = line + words[n] + " ";
      if (ctx.measureText(test).width > maxWidth && n>0){
        ctx.fillText(line.trim(), x, y + lines*lineHeight);
        line = words[n] + " ";
        lines++;
        if (lines >= maxLines) break;
      } else {
        line = test;
      }
    }
    if (lines < maxLines) ctx.fillText(line.trim(), x, y + lines*lineHeight);
  }

  // Events
  $("videoUpload").addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (file && file.size > 100 * 1024 * 1024){
      alert(i18n[currentLang].alerts.max);
      e.target.value = "";
      return;
    }
    safeGtag("event","video_upload",{ lang: currentLang });
  });

  startBtn.addEventListener("click", startAnalysis);
  resetBtn.addEventListener("click", () => {
    // reset everything
    isAnalyzing = false;
    prepared = false;
    scoreSamples = [];
    maxSpeedKmh = 0;
    lastWrist3d = null;
    lastTimeSec = null;
    finalPoseFrameUrl = null;

    ctx.clearRect(0,0,canvasEl.width, canvasEl.height);
    videoEl.pause();
    videoEl.src = "";

    $("videoUpload").value = "";
    $("results").style.display = "none";
    playOverlay.classList.remove("hidden");
    resetBtn.disabled = true;

    const t = i18n[currentLang];
    setHUD(t.hud_wait);
    setStatus("", t.status_idle);
    setLoading(false);

    safeGtag("event","reset",{ lang: currentLang });
  });

  playOverlay.addEventListener("click", startPlayback);

  videoEl.addEventListener("ended", () => {
    if (prepared) finishAnalysis();
    playOverlay.classList.remove("hidden");
  });

  videoEl.addEventListener("pause", () => {
    playOverlay.classList.remove("hidden");
  });

  // Init UI
  setLang("ko");
  setLoading(false);
  resetBtn.disabled = true;
</script>
</body>
</html>
