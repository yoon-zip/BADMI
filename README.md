<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Badmi – Smash Analyzer</title>

  <!-- Google tag (gtag.js) -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-CNMTT9RMY8"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-CNMTT9RMY8');
  </script>

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
      padding: 16px 14px 118px; /* space for sticky bar */
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

    /* Collapsible tips */
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

    /* Video area: 9:16 for reels */
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

    /* Play overlay button */
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

    /* Form */
    #userForm{ display:none; }
    .formRow{
      display:flex;
      flex-direction:column;
      gap:10px;
      margin-top: 10px;
    }
    .input{
      width:100%;
      padding: 12px;
      border-radius: 14px;
      border: 1px solid var(--stroke);
      background: rgba(0,0,0,0.18);
      color: var(--text);
      outline:none;
      font-size: 14px;
    }
    .input::placeholder{ color: rgba(255,255,255,0.40); }

    /* Ranking list */
    #rankingList{
      list-style:none;
      padding:0;
      margin: 8px 0 0;
    }
    #rankingList li{
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px solid var(--stroke);
      background: rgba(255,255,255,0.04);
      margin: 8px 0;
      color: var(--muted);
      font-size: 13px;
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
      grid-template-columns: 1.2fr 0.8fr 0.8fr;
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
    .btn-danger{
      background: rgba(239,68,68,0.18);
      border: 1px solid rgba(239,68,68,0.35);
      color: #fecaca;
    }

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
          <div class="sub" id="t_sub">Reels(세로 9:16) 촬영 권장 · 손목(라켓 헤드) 추정 속도</div>
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
          업로드 후 아래 하단의 <b>분석 준비</b>를 누르고, 영상 위 <b>재생</b> 버튼을 눌러주세요.
          결과는 영상이 끝나면 <b>최종 점수</b>로만 보여요.
        </div>

        <div style="margin-top:10px;">
          <input type="file" id="videoUpload" accept="video/mp4,video/quicktime,video/webm" style="width:100%;" />
        </div>

        <details>
          <summary id="t_tips_title">촬영 팁 보기</summary>
          <ul class="tips" id="t_tips_list">
            <li>세로(9:16) 릴스/스토리 촬영 권장</li>
            <li>전신(발~라켓 팔 손목)이 프레임 안에 들어오게</li>
            <li>밝은 조명 + 어두운 배경이 인식 안정적</li>
            <li>측면 또는 45도 각도 추천</li>
            <li>5~10초, 100MB 이하</li>
          </ul>
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
        <div class="sectionTitle" id="t_step2">2) 영상 재생 & 피펫</div>
        <div class="hint" id="t_step2_hint">
          아래에서 분석 준비를 한 다음, 영상 위 <b>재생</b>을 누르면 피펫이 따라다닙니다.
        </div>
      </div>

      <div id="videoContainer">
        <div class="hud">
          <div class="pill" id="hudPill">⏳ 업로드 후 분석 준비</div>
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

    <!-- Ranking form -->
    <div class="card" id="userForm">
      <div class="inner">
        <div class="sectionTitle" id="t_step3">3) 랭킹 등록</div>
        <div class="hint" id="t_step3_hint">최종 점수를 랭킹에 올릴 수 있어요.</div>
        <div class="formRow">
          <input class="input" type="text" id="nickname" placeholder="닉네임" />
          <input class="input" type="text" id="instagram" placeholder="인스타그램 ID (예: @user)" />
        </div>
        <div style="margin-top:10px;">
          <button class="action btn-primary" id="saveScoreButton" style="width:100%;">🏆 <span id="t_save_rank">등록하기</span></button>
        </div>
      </div>
    </div>

    <!-- Ranking -->
    <div class="card">
      <div class="inner">
        <div class="sectionTitle" id="t_rank_title">랭킹 TOP 10</div>
        <ul id="rankingList"></ul>
      </div>
    </div>
  </div>

  <!-- Sticky action bar -->
  <div class="actionBar">
    <div class="actionRow">
      <button class="action btn-primary" id="prepareButton">⚙️ <span id="t_prepare">분석 준비</span></button>
      <button class="action btn-danger" id="stopButton" disabled>⏹️ <span id="t_stop">중지</span></button>
      <button class="action btn-ghost" id="resetButton" disabled>↩️ <span id="t_reset">초기화</span></button>
    </div>
  </div>

<script type="module">
  import { PoseLandmarker, FilesetResolver, DrawingUtils } from "https://cdn.skypack.dev/@mediapipe/tasks-vision@0.10.0";
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.14.1/firebase-app.js";
  import { getDatabase, ref, push, onValue } from "https://www.gstatic.com/firebasejs/10.14.1/firebase-database.js";

  // Firebase (keep as provided)
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

  // Helpers
  const $ = (id) => document.getElementById(id);
  function safeGtag(...args){
    try{ if (typeof gtag === "function") gtag(...args); }catch(e){}
  }
  function clamp(n,a,b){ return Math.max(a, Math.min(b,n)); }

  // i18n
  const i18n = {
    ko: {
      title: "🏸 배드민턴 스매싱 분석기",
      sub: "Reels(세로 9:16) 촬영 권장 · 손목(라켓 헤드) 추정 속도",
      step1: "1) 영상 업로드",
      step1_hint: "업로드 후 아래 하단의 분석 준비를 누르고, 영상 위 재생 버튼을 눌러주세요. 결과는 영상이 끝나면 최종 점수로만 보여요.",
      tips_title: "촬영 팁 보기",
      step2: "2) 영상 재생 & 피펫",
      step2_hint: "아래에서 분석 준비를 한 다음, 영상 위 재생을 누르면 피펫이 따라다닙니다.",
      play: "재생",
      prepare: "분석 준비",
      stop: "중지",
      reset: "초기화",
      rank_title: "랭킹 TOP 10",
      step3: "3) 랭킹 등록",
      step3_hint: "최종 점수를 랭킹에 올릴 수 있어요.",
      save_rank: "등록하기",
      privacy: "※ 업로드한 영상은 브라우저에서만 처리됩니다.",
      hud_wait: "⏳ 업로드 후 분석 준비",
      hud_ready: "✅ 준비 완료! 영상 위 재생을 눌러주세요",
      hud_analyzing: "🔎 분석 중… (끝나면 결과 표시)",
      hud_done: "✅ 분석 완료! 아래 결과 확인",
      status_idle: "대기 중",
      status_preparing: "준비 중",
      status_ready: "준비 완료",
      status_analyzing: "분석 중",
      status_done: "분석 완료",
      status_stopped: "중지됨",
      status_initfail: "초기화 실패",
      result_title: "✅ 최종 분석 결과",
      score: "자세 점수",
      speed: "추정 속도(손목)",
      save_story: "📱 스토리 이미지 저장(1080×1920)",
      feedback_label: "피드백",
      tips_list: [
        "세로(9:16) 릴스/스토리 촬영 권장",
        "전신(발~라켓 팔 손목)이 프레임 안에 들어오게",
        "밝은 조명 + 어두운 배경이 인식 안정적",
        "측면 또는 45도 각도 추천",
        "5~10초, 100MB 이하"
      ],
      alerts: {
        upload: "영상을 업로드하세요.",
        max: "파일 크기는 100MB 이하여야 합니다.",
        initfail: "PoseLandmarker 초기화 실패. 콘솔(F12) 확인 또는 Chrome 최신 버전 사용을 권장합니다.",
        playfail: "재생에 실패했어요. 모바일은 사용자 제스처가 필요할 수 있어요.",
        need_finish: "분석을 먼저 완료해주세요.",
        need_profile: "닉네임과 인스타그램 ID를 입력하세요.",
        saved: "등록 완료! 랭킹을 확인하세요."
      }
    },
    en: {
      title: "🏸 Badminton Smash Analyzer",
      sub: "Reels (vertical 9:16) recommended · Wrist (racket-head proxy) speed",
      step1: "1) Upload Video",
      step1_hint: "Upload a clip, tap Prepare, then press Play on the video. Results show only once the video ends.",
      tips_title: "Show filming tips",
      step2: "2) Play & Pipette Overlay",
      step2_hint: "After Prepare, press Play on the video to see the overlay tracking you.",
      play: "Play",
      prepare: "Prepare",
      stop: "Stop",
      reset: "Reset",
      rank_title: "Top 10 Ranking",
      step3: "3) Submit to Ranking",
      step3_hint: "You can submit your final score to the leaderboard.",
      save_rank: "Submit",
      privacy: "※ Video is processed locally in your browser.",
      hud_wait: "⏳ Upload then Prepare",
      hud_ready: "✅ Ready! Press Play on the video",
      hud_analyzing: "🔎 Analyzing… (results after it ends)",
      hud_done: "✅ Done! Check results below",
      status_idle: "Idle",
      status_preparing: "Preparing",
      status_ready: "Ready",
      status_analyzing: "Analyzing",
      status_done: "Completed",
      status_stopped: "Stopped",
      status_initfail: "Init failed",
      result_title: "✅ Final Result",
      score: "Posture Score",
      speed: "Estimated Wrist Speed",
      save_story: "📱 Save Story Image (1080×1920)",
      feedback_label: "Feedback",
      tips_list: [
        "Vertical 9:16 (Reels/Story) recommended",
        "Keep full body in frame (feet → hitting wrist)",
        "Bright lighting + dark background helps tracking",
        "Side or 45-degree angle recommended",
        "5–10s clip, under 100MB"
      ],
      alerts: {
        upload: "Please upload a video.",
        max: "File must be under 100MB.",
        initfail: "PoseLandmarker init failed. Check console or use latest Chrome.",
        playfail: "Playback failed. Mobile may require a user gesture.",
        need_finish: "Please finish analysis first.",
        need_profile: "Please enter nickname and Instagram ID.",
        saved: "Submitted! Check the leaderboard."
      }
    },
    zh: {
      title: "🏸 羽毛球扣杀分析器",
      sub: "建议竖屏 9:16（Reels/Story）· 手腕（拍头代理）速度",
      step1: "1) 上传视频",
      step1_hint: "上传后点击“准备分析”，再点视频上的“播放”。结果会在视频结束后一次性显示。",
      tips_title: "查看拍摄建议",
      step2: "2) 播放与骨架覆盖",
      step2_hint: "点击准备分析后，按视频上的播放即可看到骨架跟踪。",
      play: "播放",
      prepare: "准备分析",
      stop: "停止",
      reset: "重置",
      rank_title: "排行榜 TOP 10",
      step3: "3) 提交排行榜",
      step3_hint: "可将最终分数提交到排行榜。",
      save_rank: "提交",
      privacy: "※ 视频在浏览器本地处理。",
      hud_wait: "⏳ 上传后点击准备分析",
      hud_ready: "✅ 已就绪！点击视频播放",
      hud_analyzing: "🔎 分析中…（结束后出结果）",
      hud_done: "✅ 完成！查看下方结果",
      status_idle: "待机",
      status_preparing: "准备中",
      status_ready: "已就绪",
      status_analyzing: "分析中",
      status_done: "已完成",
      status_stopped: "已停止",
      status_initfail: "初始化失败",
      result_title: "✅ 最终结果",
      score: "动作评分",
      speed: "估计手腕速度",
      save_story: "📱 保存Story图片 (1080×1920)",
      feedback_label: "反馈",
      tips_list: [
        "建议竖屏 9:16（Reels/Story）拍摄",
        "保证全身入镜（脚 → 挥拍手腕）",
        "明亮光线 + 深色背景有助识别",
        "建议侧面或45度角拍摄",
        "5–10秒，低于100MB"
      ],
      alerts: {
        upload: "请先上传视频。",
        max: "文件需小于100MB。",
        initfail: "初始化失败。请查看控制台或使用最新版Chrome。",
        playfail: "播放失败。手机端可能需要用户点击触发播放。",
        need_finish: "请先完成分析。",
        need_profile: "请输入昵称与Instagram ID。",
        saved: "提交成功！查看排行榜。"
      }
    }
  };

  let currentLang = "ko";
  function setLang(lang){
    currentLang = lang;
    const t = i18n[lang];

    $("t_title").textContent = t.title;
    $("t_sub").textContent = t.sub;
    $("t_step1").textContent = t.step1;
    $("t_step1_hint").innerHTML = t.step1_hint.replaceAll("분석 준비","<b>분석 준비</b>").replaceAll("재생","<b>재생</b>");
    $("t_tips_title").textContent = t.tips_title;
    $("t_step2").textContent = t.step2;
    $("t_step2_hint").innerHTML = t.step2_hint.replaceAll("Prepare","<b>Prepare</b>").replaceAll("Play","<b>Play</b>").replaceAll("재생","<b>재생</b>");
    $("t_play").textContent = t.play;
    $("t_prepare").textContent = t.prepare;
    $("t_stop").textContent = t.stop;
    $("t_reset").textContent = t.reset;
    $("t_rank_title").textContent = t.rank_title;
    $("t_step3").textContent = t.step3;
    $("t_step3_hint").textContent = t.step3_hint;
    $("t_save_rank").textContent = t.save_rank;
    $("t_privacy").textContent = t.privacy;

    // tips list
    const tipsUl = $("t_tips_list");
    tipsUl.innerHTML = "";
    t.tips_list.forEach(x=>{
      const li = document.createElement("li");
      li.textContent = x;
      tipsUl.appendChild(li);
    });

    // buttons active
    document.querySelectorAll(".lang-switch button").forEach(b=>{
      b.classList.toggle("active", b.dataset.lang === lang);
    });

    safeGtag("event","change_language",{ lang });
  }
  document.querySelectorAll(".lang-switch button").forEach(btn=>{
    btn.addEventListener("click", ()=> setLang(btn.dataset.lang));
  });

  // UI refs
  const statusDot = $("statusDot");
  const statusText = $("statusText");
  const loadingEl = $("loading");
  const hudPill = $("hudPill");

  const videoEl = $("video");
  const canvasEl = $("output_canvas");
  const ctx = canvasEl.getContext("2d");

  const prepareBtn = $("prepareButton");
  const stopBtn = $("stopButton");
  const resetBtn = $("resetButton");
  const playOverlay = $("playOverlay");

  function setStatus(mode, text){
    statusText.textContent = text;
    statusDot.classList.remove("live","err");
    if (mode === "live") statusDot.classList.add("live");
    if (mode === "err") statusDot.classList.add("err");
  }
  function setHUD(text){ hudPill.textContent = text; }
  function setLoading(on){ loadingEl.style.display = on ? "inline" : "none"; }

  // Pose
  let poseLandmarker;
  let drawingUtils;
  let isAnalyzing = false;
  let prepared = false;
  let lastVideoTime = -1;

  // Score samples
  let scoreSamples = [];
  let finalScore = null;

  // Wrist speed from world landmarks
  let lastWrist3d = null;
  let lastTimeSec = null;
  let maxSpeedKmh = 0;
  let finalSpeed = null;

  // Frame capture for story image
  let finalPoseFrameUrl = null;

  // --- Scoring rubric (simple MVP) ---
  // 10 points = 5 items x 2 points:
  // (1) prep posture (2) elbow-up/backswing (3) reach up (4) rotation proxy (5) lower body/balance
  function calculateScore(landmarks){
    let s = 10;

    // indices (right side): shoulder 12, elbow 14, wrist 16, hip 24, left hip 23, left knee 25, left ankle 27
    const shoulder = landmarks[12] || {x:0,y:0};
    const elbow    = landmarks[14] || {x:0,y:0};
    const wrist    = landmarks[16] || {x:0,y:0};
    const hip      = landmarks[24] || {x:0,y:0};

    // (2) elbow-up proxy: elbow y should not be far below shoulder y in overhead moments
    // (y smaller = higher in normalized coordinates)
    if (elbow.y > shoulder.y + 0.08) s -= 2;

    // (3) reach-up proxy: wrist should get above shoulder sometimes; if not, penalize lightly
    if (wrist.y > shoulder.y - 0.02) s -= 2;

    // (4) rotation proxy: shoulder-hip line angle extreme suggests leaning rather than rotation
    const torsoAngle = Math.atan2(shoulder.y - hip.y, shoulder.x - hip.x) * 180 / Math.PI;
    if (Math.abs(torsoAngle) > 25) s -= 2;

    // (5) lower body: left leg angle too straight (risk / less power transfer)
    const hipL   = landmarks[23] || {x:0,y:0};
    const kneeL  = landmarks[25] || {x:0,y:0};
    const ankleL = landmarks[27] || {x:0,y:0};
    const legAngle = calculateAngle(hipL, kneeL, ankleL);
    if (legAngle > 160) s -= 2;

    // (1) prep/balance: if shoulder and hip are too close vertically (crouched/unclear), slight penalty
    const torsoLen = Math.abs(shoulder.y - hip.y);
    if (torsoLen < 0.10) s -= 2;

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
    const lang = currentLang;
    if (lang === "ko"){
      let fb =
        score >= 8 ? "우수한 자세예요. " :
        score >= 5 ? "전반적으로 양호하지만 몇 포인트만 다듬으면 더 좋아요. " :
        "교정 포인트가 있어요. ";
      fb += `최고 추정 손목 속도는 ${speed}km/h예요. `;
      fb += "팁: 팔꿈치를 더 높게, 타점을 더 위로, 무릎 굽힘과 중심을 유지해보세요.";
      return fb;
    }
    if (lang === "zh"){
      let fb =
        score >= 8 ? "动作很棒。 " :
        score >= 5 ? "整体不错，但还有提升空间。 " :
        "需要一些动作调整。 ";
      fb += `最高估计手腕速度：${speed} km/h。 `;
      fb += "建议：抬高肘部、提高击球点、屈膝并保持重心稳定。";
      return fb;
    }
    // en
    let fb =
      score >= 8 ? "Great form. " :
      score >= 5 ? "Good overall, but there’s room to improve. " :
      "Needs some form adjustment. ";
    fb += `Peak estimated wrist speed: ${speed} km/h. `;
    fb += "Tip: keep elbow higher, reach up at contact, and maintain knee bend + balance.";
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
      setStatus("err", i18n[currentLang].status_initfail);
      alert(i18n[currentLang].alerts.initfail);
      return false;
    }
  }

  async function prepareAnalysis(){
    const file = $("videoUpload").files[0];
    if (!file){ alert(i18n[currentLang].alerts.upload); return; }
    if (file.size > 100 * 1024 * 1024){ alert(i18n[currentLang].alerts.max); return; }

    // reset
    prepared = false;
    isAnalyzing = false;
    lastVideoTime = -1;
    scoreSamples = [];
    finalScore = null;
    lastWrist3d = null;
    lastTimeSec = null;
    maxSpeedKmh = 0;
    finalSpeed = null;
    finalPoseFrameUrl = null;

    $("results").style.display = "none";
    $("userForm").style.display = "none";
    playOverlay.classList.remove("hidden");

    setStatus("live", i18n[currentLang].status_preparing);
    setHUD(i18n[currentLang].hud_wait);
    setLoading(true);

    safeGtag("event","analysis_prepare");

    if (!poseLandmarker){
      const ok = await initPoseLandmarker();
      if (!ok){ setLoading(false); return; }
    }

    // load video (no autoplay; user presses play)
    const src = URL.createObjectURL(file);
    videoEl.src = src;
    videoEl.load();

    videoEl.onloadedmetadata = () => {
      canvasEl.width = videoEl.videoWidth;
      canvasEl.height = videoEl.videoHeight;

      prepared = true;
      setLoading(false);
      setStatus("live", i18n[currentLang].status_ready);
      setHUD(i18n[currentLang].hud_ready);

      prepareBtn.disabled = true;
      stopBtn.disabled = false;
      resetBtn.disabled = false;

      safeGtag("event","analysis_ready");
    };
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

        // scoring samples
        scoreSamples.push(calculateScore(lm));

        // wrist speed samples (world)
        sampleWristSpeedFromWorld(results);
      }
    }catch(e){
      console.error("Detection failed:", e);
    }

    ctx.restore();
    requestAnimationFrame(analyzeLoop);
  }

  async function startPlaybackAndAnalysis(){
    if (!prepared){
      // if user didn't press prepare, guide
      return;
    }
    try{
      await videoEl.play();
      playOverlay.classList.add("hidden");

      isAnalyzing = true;
      setStatus("live", i18n[currentLang].status_analyzing);
      setHUD(i18n[currentLang].hud_analyzing);
      setLoading(true);

      safeGtag("event","analysis_start");
      requestAnimationFrame(analyzeLoop);
    }catch(e){
      console.error(e);
      alert(i18n[currentLang].alerts.playfail);
    }
  }

  function stopAnalysis(){
    isAnalyzing = false;
    setLoading(false);
    setStatus("err", i18n[currentLang].status_stopped);
    setHUD(i18n[currentLang].hud_wait);
    playOverlay.classList.remove("hidden");
    safeGtag("event","analysis_stop");
  }

  function resetAll(){
    stopAnalysis();
    prepared = false;

    ctx.clearRect(0,0,canvasEl.width, canvasEl.height);
    videoEl.pause();
    videoEl.src = "";

    $("videoUpload").value = "";
    $("results").style.display = "none";
    $("userForm").style.display = "none";

    prepareBtn.disabled = false;
    stopBtn.disabled = true;
    resetBtn.disabled = true;

    setStatus("", i18n[currentLang].status_idle);
    setHUD(i18n[currentLang].hud_wait);
  }

  function finishAnalysis(){
    if (!prepared) return;
    isAnalyzing = false;
    setLoading(false);

    finalScore = computeFinalScore();
    finalSpeed = maxSpeedKmh || 0;

    // capture the current overlay frame (video + landmarks already drawn on canvas)
    finalPoseFrameUrl = canvasEl.toDataURL("image/png");

    const feedback = getFeedback(finalScore, finalSpeed);
    renderResults(finalScore, finalSpeed, feedback, finalPoseFrameUrl);

    $("results").style.display = "block";
    $("userForm").style.display = "block";

    setStatus("live", i18n[currentLang].status_done);
    setHUD(i18n[currentLang].hud_done);

    // scroll to results for mobile delight
    $("results").scrollIntoView({ behavior:"smooth", block:"start" });

    safeGtag("event","analysis_complete",{ score: finalScore, speed: finalSpeed });
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
      await downloadStoryImage({
        poseFrameUrl,
        score,
        speed,
        feedback
      });
    });
  }

  // --- Story image (1080x1920) generation ---
  async function downloadStoryImage({ poseFrameUrl, score, speed, feedback }){
    const W = 1080, H = 1920;
    const out = document.createElement("canvas");
    out.width = W; out.height = H;
    const c = out.getContext("2d");

    // background gradient
    const g = c.createLinearGradient(0,0,W,H);
    g.addColorStop(0, "#0b1020");
    g.addColorStop(1, "#1b1450");
    c.fillStyle = g;
    c.fillRect(0,0,W,H);

    // title
    c.fillStyle = "rgba(255,255,255,0.92)";
    c.font = "900 54px system-ui, -apple-system, Segoe UI, Roboto";
    c.fillText(currentLang === "ko" ? "🏸 스매싱 분석 결과" :
               currentLang === "zh" ? "🏸 扣杀分析结果" :
               "🏸 Smash Analysis", 70, 140);

    // frame card (9:16 area)
    const img = await loadImage(poseFrameUrl);
    const cardX = 70, cardY = 210, cardW = 940, cardH = 1040;

    c.save();
    roundRectPath(c, cardX, cardY, cardW, cardH, 36);
    c.clip();
    const crop = coverCrop(img.width, img.height, cardW, cardH);
    c.drawImage(img, crop.sx, crop.sy, crop.sw, crop.sh, cardX, cardY, cardW, cardH);
    c.restore();

    // pill boxes
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

  // Ranking submit
  function saveScore(){
    const nickname = $("nickname").value.trim();
    const instagram = $("instagram").value.trim();
    if (!nickname || !instagram){
      alert(i18n[currentLang].alerts.need_profile);
      return;
    }
    if (finalScore === null){
      alert(i18n[currentLang].alerts.need_finish);
      return;
    }

    const data = {
      score: finalScore,
      speed: finalSpeed ?? maxSpeedKmh ?? 0,
      nickname,
      instagram,
      timestamp: Date.now()
    };

    push(ref(db, "scores"), data)
      .then(() => {
        alert(i18n[currentLang].alerts.saved);
        $("userForm").style.display = "none";
        safeGtag("event","save_score",{ score: finalScore, speed: data.speed, lang: currentLang });
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
    ranks.sort((a,b)=> (b.score - a.score) || ((b.speed||0) - (a.speed||0)) || (b.timestamp - a.timestamp));
    ranks.slice(0,10).forEach((r, i) => {
      const li = document.createElement("li");
      li.textContent = `${i+1}위: ${r.nickname} (${r.instagram}) - 점수: ${r.score}${r.speed ? ` / 속도: ${r.speed}km/h` : ""}`;
      list.appendChild(li);
    });
  }, err => console.error("Failed to fetch rankings:", err));

  // Events
  $("videoUpload").addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (file && file.size > 100 * 1024 * 1024){
      alert(i18n[currentLang].alerts.max);
      e.target.value = "";
      return;
    }
    resetBtn.disabled = !e.target.files[0];
    safeGtag("event","video_upload",{ lang: currentLang });
  });

  prepareBtn.addEventListener("click", prepareAnalysis);
  stopBtn.addEventListener("click", stopAnalysis);
  resetBtn.addEventListener("click", resetAll);
  $("saveScoreButton").addEventListener("click", saveScore);

  playOverlay.addEventListener("click", startPlaybackAndAnalysis);

  // video end triggers finish
  videoEl.addEventListener("ended", () => {
    // capture final overlay frame already drawn on canvas at last loop tick
    finalSpeed = maxSpeedKmh || 0;
    finishAnalysis();
    playOverlay.classList.remove("hidden");
  });

  videoEl.addEventListener("pause", () => {
    if (isAnalyzing){
      isAnalyzing = false;
      setLoading(false);
      setStatus("live", i18n[currentLang].status_ready);
      setHUD(i18n[currentLang].hud_ready);
    }
    playOverlay.classList.remove("hidden");
  });

  videoEl.addEventListener("play", () => {
    // keep overlay hidden during play
    playOverlay.classList.add("hidden");
  });

  // Initial UI
  setLang("ko");
  setStatus("", i18n[currentLang].status_idle);
  setHUD(i18n[currentLang].hud_wait);
  setLoading(false);
  stopBtn.disabled = true;
  resetBtn.disabled = true;

</script>
</body>
</html>
