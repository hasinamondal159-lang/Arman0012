<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#f8faf4">
<title>Attendly — Student Attendance</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script src="https://unpkg.com/html5-qrcode"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
:root{--lime:#d7ff58;--ink:#18231b;--green:#203b2a;--bg:#f8faf4;--muted:#818a7f;--line:#e8ecdf;--white:#fff;--shadow:0 14px 40px #23351e0c}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",sans-serif;background:#edf0e7;color:var(--ink);min-height:100vh}
button,input,select{font:inherit}button{cursor:pointer;-webkit-tap-highlight-color:transparent}
button:focus-visible,input:focus-visible,select:focus-visible{outline:3px solid #b8dd45;outline-offset:3px}
.app{width:100%;max-width:480px;min-height:100dvh;margin:auto;background:var(--bg);padding-bottom:calc(94px + env(safe-area-inset-bottom));position:relative;overflow:hidden;box-shadow:0 0 70px #18231b0b}
header{height:70px;display:flex;align-items:center;justify-content:space-between;padding:0 22px;border-bottom:1px solid var(--line);background:#f8faf4ed;backdrop-filter:blur(16px);position:sticky;top:0;z-index:8}
.brand{display:flex;align-items:center;gap:11px;font-size:18px;font-weight:850;letter-spacing:-.6px}
.logo{width:34px;height:34px;background:var(--lime);border-radius:11px 11px 11px 4px;display:grid;place-items:center;box-shadow:0 5px 12px #b9d84c40}
.logo svg{width:21px;height:21px}
.header-actions{display:flex;gap:9px}
.round{height:41px;width:41px;border:1px solid var(--line);background:white;border-radius:14px;display:grid;place-items:center;color:var(--green);transition:transform .2s,background .2s}
.round:hover{transform:translateY(-2px);background:#f1f7e5}
.round svg{width:19px;height:19px}
main{padding:25px 20px 18px}
.view{display:none;animation:pageIn .35s cubic-bezier(.2,.8,.2,1)}
.view.active{display:block}
@keyframes pageIn{from{opacity:0;transform:translateY(9px)}to{opacity:1;transform:translateY(0)}}
.eyebrow{font-size:10px;letter-spacing:1.8px;font-weight:850;color:#758640}
h1{font-size:clamp(27px,7vw,33px);line-height:1.12;letter-spacing:-1.4px;font-weight:720;margin:10px 0}
h2{font-size:21px;font-weight:750;letter-spacing:-.7px}
.sub{font-size:13px;color:var(--muted);line-height:1.65}
.hero-row{display:flex;justify-content:space-between;align-items:center;gap:12px}
.date-chip{flex-shrink:0;background:#eef3e5;border-radius:100px;padding:8px 11px;color:#53633c;font-size:10px;font-weight:800}
.card{position:relative;isolation:isolate;margin-top:23px;padding:23px 19px;background:linear-gradient(135deg,#e6ff9c 0%,#d7ff58 60%,#c9f248 100%);border-radius:26px;min-height:258px;overflow:hidden;box-shadow:0 13px 28px #badc4c24}
.card:after{content:"";position:absolute;width:170px;height:170px;border:1px solid #ffffff70;border-radius:50%;right:-70px;top:-65px;box-shadow:0 0 0 20px #ffffff19,0 0 0 42px #ffffff13;z-index:-1}
.pill{display:inline-flex;align-items:center;gap:7px;background:#f6ffdfb8;padding:8px 11px;border-radius:30px;font-size:9px;font-weight:900;letter-spacing:.6px}
.live-dot{height:7px;width:7px;background:#4c7a26;border-radius:50%;box-shadow:0 0 0 4px #4c7a2618;animation:pulse 1.7s infinite}
@keyframes pulse{50%{box-shadow:0 0 0 7px #4c7a2600}}
.class-title{font-size:clamp(25px,6vw,30px);line-height:1.08;letter-spacing:-1.1px;font-weight:700;margin:18px 0 12px;max-width:290px}
.meta{font-size:11px;color:#52613b;display:flex;gap:7px;align-items:center;flex-wrap:wrap}
.meta svg{width:14px;height:14px}
.dark-btn{margin-top:21px;display:flex;align-items:center;gap:10px;background:var(--green);color:white;border:0;border-radius:14px;padding:7px 14px 7px 7px;min-height:49px;font-size:12px;font-weight:800;box-shadow:0 5px 0 #17201912;transition:transform .2s,box-shadow .2s}
.dark-btn:hover{transform:translateY(-3px);box-shadow:0 8px 15px #17201920}
.qr-icon{height:35px;width:35px;display:grid;place-items:center;background:var(--lime);color:var(--green);border-radius:10px}
.qr-icon svg{width:22px;height:22px}
.real-qr-mark{shape-rendering:crispEdges}
.qr-icon .real-qr-mark{filter:drop-shadow(0 1px 0 #ffffff30)}
.qr-symbol .real-qr-mark{width:21px;height:21px}
.qr-icon{transition:transform .22s ease,box-shadow .22s ease}
.dark-btn:hover .qr-icon{transform:rotate(-4deg) scale(1.06);box-shadow:0 0 0 4px #d7ff5824}
nav button[data-view="qrView"] .ico svg{shape-rendering:crispEdges}
nav button[data-view="qrView"].active .ico svg{filter:drop-shadow(0 2px 2px #a4c84b45)}
.arrow{margin-left:auto;font-size:20px}
.note{font-size:10px;color:#66783f;margin-top:11px}
.section-head{display:flex;justify-content:space-between;align-items:end;gap:12px;margin:32px 0 14px}
.section-head h2{margin-top:7px}
.link{border:0;background:none;font-size:12px;font-weight:750;color:#63704e;padding:8px 0}
.schedule{display:grid;gap:10px}
.item{display:flex;align-items:center;gap:10px;padding:13px 10px;background:#fff;border:1px solid var(--line);border-radius:17px;box-shadow:0 3px 12px #24341b04;transition:transform .2s,border-color .2s}
.item:hover{transform:translateY(-2px);border-color:#d6e5b7}
.time{min-width:48px;font-size:9px;line-height:1.6;color:var(--muted)}
.time strong{display:block;font-size:12px;color:var(--ink)}
.symbol{width:40px;height:40px;display:grid;place-items:center;flex-shrink:0;border-radius:13px;background:#f0f5e6;color:#435b30}
.symbol svg{width:20px;height:20px}
.details{flex:1;min-width:0}
.details strong{font-size:11px;line-height:1.4;display:block}
.details small{display:block;color:var(--muted);font-size:9px;margin-top:5px;line-height:1.4}
.status{font-size:8px;font-weight:900;padding:7px 8px;border-radius:20px;background:#eaffae;white-space:nowrap;color:#465c2a}
.streak{display:flex;align-items:center;gap:11px;background:#f0f3e8;padding:15px;border-radius:19px;margin-top:22px}
.streak-icon{width:45px;height:45px;border-radius:14px;background:white;display:grid;place-items:center;color:#617b2e}
.streak-icon svg{width:22px;height:22px}
.streak-info{flex:1}.streak-info strong{font-size:12px}.streak-info p{font-size:10px;color:var(--muted);margin-top:5px;line-height:1.4}
.count{font-size:24px;font-weight:850;text-align:right;letter-spacing:-1px}.count small{display:block;font-size:8px;letter-spacing:1.2px;color:var(--muted)}
nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:480px;min-height:78px;padding:8px 8px calc(8px + env(safe-area-inset-bottom));background:#fffffff0;border-top:1px solid var(--line);display:flex;align-items:center;justify-content:space-around;z-index:10;backdrop-filter:blur(18px)}
nav button{min-width:62px;border:0;background:transparent;color:#8b9487;border-radius:15px;padding:8px 10px;display:flex;flex-direction:column;align-items:center;gap:5px;font-size:9px;font-weight:800;transition:background .2s,color .2s,transform .2s}
nav button.active{background:#edf7d9;color:#304321}
nav button:active{transform:scale(.94)}
nav .ico{height:21px;display:grid;place-items:center}
nav .ico svg{width:20px;height:20px;stroke-width:1.9}
.page-head{text-align:left;margin:8px 0 23px}
.page-head h1{font-size:29px}
.panel{background:#fff;border:1px solid var(--line);border-radius:20px;padding:17px;margin-top:16px;box-shadow:var(--shadow)}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px 10px}
.field.full{grid-column:1/-1}
label{display:block;font-size:9px;font-weight:850;letter-spacing:.7px;color:#758071;margin-bottom:8px}
input,select{width:100%;height:45px;border:1px solid #e4e9dc;border-radius:12px;background:#fbfcf8;padding:0 12px;font-size:12px;color:var(--ink);outline:none;min-width:0;transition:border .2s,box-shadow .2s}
input:focus,select:focus{border-color:#b3d34e;box-shadow:0 0 0 3px #d7ff5838}
.center-status{display:table;margin:18px auto 13px;padding:9px 13px;border-radius:30px;background:#fff4d1;color:#8b7222;font-size:10px;font-weight:800}
.student-card{display:flex;align-items:center;gap:11px;padding:13px;background:white;border:1px solid var(--line);border-radius:16px;margin:0 4px -5px;position:relative;z-index:1}
.avatar{height:39px;width:39px;display:grid;place-items:center;border-radius:13px;background:#f0e8d5;color:#715b31;font-size:11px;font-weight:900;flex-shrink:0}
.student-info{min-width:0}.student-info strong{display:block;font-size:12px;overflow-wrap:anywhere}.student-info small{display:block;margin-top:5px;color:var(--muted);font-size:10px;overflow-wrap:anywhere}
.saved-student-item{display:grid;grid-template-columns:76px minmax(0,1fr);gap:12px;align-items:center;padding:12px;background:#fff;border:1px solid var(--line);border-radius:14px;margin-bottom:10px}.saved-student-qr{width:76px;height:76px;display:grid;place-items:center;background:#fff;border:1px solid #e7eadf;border-radius:9px;overflow:hidden}.saved-student-qr canvas,.saved-student-qr img{width:68px!important;height:68px!important;image-rendering:pixelated}.saved-student-actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:8px}.saved-student-actions button{font-size:10px!important;padding:7px 9px!important}
.qr-frame{width:min(100%,250px);min-height:250px;margin:0 auto 18px;padding:13px;border:5px solid var(--green);border-radius:18px 18px 27px 27px;background:white;box-shadow:0 5px 0 var(--lime);display:flex;align-items:center;justify-content:center}
#qrcode{display:flex;justify-content:center;align-items:center;width:100%;min-height:190px}
#qrcode img,#qrcode canvas{max-width:100%;height:auto!important;image-rendering:pixelated}
.primary{display:flex;align-items:center;justify-content:center;gap:9px;width:100%;margin:0 auto;padding:14px 18px;border:0;border-radius:14px;background:var(--lime);color:#26331d;font-size:12px;font-weight:850;transition:transform .2s,filter .2s}
.primary:hover{transform:translateY(-2px);filter:saturate(1.1)}
.primary svg{width:17px;height:17px}
.footnote{text-align:center;margin-top:14px;color:var(--muted);font-size:10px}
.notice{text-align:center;font-size:10px;color:var(--muted);line-height:1.65;margin-top:14px}
.camera-box{height:270px;background:#10251b;border-radius:19px;position:relative;overflow:hidden}
#reader{width:100%!important;height:100%!important;min-height:270px;border:0!important;background:#10251b!important;position:relative}
#reader__scan_region{width:100%!important;height:270px!important;min-height:270px!important;display:flex!important;align-items:center!important;justify-content:center!important;background:#10251b!important}
#reader__scan_region video,#reader video{display:block!important;width:100%!important;height:270px!important;min-height:270px!important;object-fit:cover!important;background:#10251b!important}
#reader__scan_region img{display:none!important}
#reader__dashboard{display:none!important}#reader__header_message{display:none!important}
.scan-frame{position:absolute;inset:17%;pointer-events:none;z-index:2}
.corner{position:absolute;width:26px;height:26px;border-color:var(--lime);border-style:solid}
.tl{top:0;left:0;border-width:3px 0 0 3px}.tr{top:0;right:0;border-width:3px 3px 0 0}.bl{bottom:0;left:0;border-width:0 0 3px 3px}.br{bottom:0;right:0;border-width:0 3px 3px 0}
.scan-line{position:absolute;left:3%;right:3%;height:2px;background:var(--lime);box-shadow:0 0 12px var(--lime);animation:scan 2.3s ease-in-out infinite}
@keyframes scan{0%,100%{top:8%}50%{top:90%}}
.camera-message{text-align:center;margin-top:13px;padding:12px 9px;border-radius:11px;background:#ffe8e2;color:#a64b3b;font-size:10px;font-weight:800}
.camera-message.ok{background:#e8f6ce;color:#486b21}
.controls{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:14px}
.control{border:0;border-radius:12px;padding:13px 7px;background:var(--lime);font-size:11px;font-weight:850;color:#26331d}
.control.secondary{background:#edf0e7;color:#53604d}
.result{display:none;margin-top:15px;background:white;border:1px solid var(--line);border-radius:16px;padding:15px;animation:pageIn .25s ease}
.result.show{display:block}.result h3{font-size:14px;margin-bottom:10px}.result p{font-size:12px;line-height:1.9;overflow-wrap:anywhere}.result strong{color:#29372b}
.profile-hero{background:linear-gradient(140deg,#e7ffb1,#d7ff58);border-radius:22px;padding:20px;display:flex;align-items:center;gap:14px;margin-bottom:18px}
.profile-hero .avatar{height:57px;width:57px;border-radius:18px;background:#ffffffa6;font-size:15px;color:var(--green)}
.profile-hero h2{font-size:18px}.profile-hero p{font-size:11px;color:#5c6b42;margin-top:5px}
.student-profile-card{margin-top:0;margin-bottom:15px;padding:16px}.profile-detail-row{display:flex;justify-content:space-between;align-items:flex-start;gap:12px;padding:11px 0;border-bottom:1px solid var(--line);font-size:11px}.profile-detail-row span{color:var(--muted)}.profile-detail-row strong{text-align:right;overflow-wrap:anywhere;max-width:62%}.profile-save-status{font-size:10px;color:var(--muted);line-height:1.5;margin-top:12px}
#reader,#reader__scan_region,#reader__scan_region video{width:100%!important}#reader__scan_region{min-height:100%!important}#reader__scan_region video{object-fit:cover!important}#reader__dashboard_section_csr button{border:0;border-radius:10px;padding:9px 12px;background:var(--lime);font-weight:800}
.profile-action{width:100%;display:flex;align-items:center;gap:12px;text-align:left;border:1px solid var(--line);border-radius:15px;background:white;padding:14px;margin-top:10px;color:var(--ink);font-size:12px;font-weight:800}
.profile-action .symbol{width:38px;height:38px}.profile-action span:last-child{margin-left:auto;color:#8a9384;font-size:20px}
.toast{position:fixed;top:82px;left:50%;transform:translate(-50%,-8px);background:#172019;color:white;padding:12px 16px;border-radius:13px;font-size:12px;opacity:0;pointer-events:none;transition:opacity .2s,transform .2s;z-index:30;width:max-content;max-width:90%;box-shadow:0 8px 25px #0002}
.toast.show{opacity:1;transform:translate(-50%,0)}

.attendance-panel{margin-top:22px;padding:16px;background:#fff;border:1px solid var(--line);border-radius:20px;box-shadow:var(--shadow)}
.attendance-head{display:flex;justify-content:space-between;align-items:flex-start;gap:10px}.attendance-head h2{font-size:18px;margin:7px 0}.attendance-head .sub{font-size:11px;max-width:210px}
.export-btn{border:0;background:var(--green);color:#fff;border-radius:11px;padding:10px 11px;font-size:10px;font-weight:850;white-space:nowrap}
.attendance-stats{display:flex;align-items:baseline;gap:8px;padding:12px 0;border-bottom:1px solid var(--line)}.attendance-stats strong{font-size:26px;letter-spacing:-1px}.attendance-stats span{font-size:10px;color:var(--muted)}
.attendance-list{display:grid;gap:9px;margin-top:12px}.attendance-record{padding:12px;border:1px solid var(--line);border-radius:13px;background:#fbfcf8;display:grid;grid-template-columns:1fr auto;gap:7px 10px}.attendance-record strong{font-size:11px;display:block;overflow-wrap:anywhere}.attendance-record small{font-size:9px;color:var(--muted);display:block;margin-top:4px;line-height:1.5;overflow-wrap:anywhere}.attendance-record time{font-size:9px;font-weight:800;color:#536b2e;text-align:right;line-height:1.5}.attendance-badge{display:inline-flex;margin-top:6px;padding:4px 7px;border-radius:20px;background:#eaffae;color:#476127;font-size:8px;font-weight:900}.empty-attendance{padding:17px 9px;text-align:center;color:var(--muted);font-size:10px;line-height:1.6;background:#f8faf4;border-radius:12px}
.about-overlay{display:none;position:fixed;inset:0;z-index:50;background:#1320178c;backdrop-filter:blur(5px);padding:22px;align-items:center;justify-content:center}.about-overlay.show{display:flex;animation:pageIn .2s ease}.about-dialog{position:relative;width:min(100%,410px);background:var(--bg);border-radius:25px;padding:25px;box-shadow:0 25px 90px #0003}.about-close{position:absolute;right:15px;top:13px;border:0;background:#edf0e7;border-radius:50%;width:34px;height:34px;font-size:24px;color:var(--green)}.about-brand{display:flex;align-items:center;gap:10px;font-size:17px;font-weight:900;margin-bottom:27px}.about-dialog h2{font-size:25px;margin:10px 0 12px;line-height:1.15}.about-dialog p{font-size:12px;color:#697466;line-height:1.75}.about-features{display:grid;gap:9px;margin:19px 0}.about-features div{display:flex;align-items:center;gap:12px;padding:12px;background:#fff;border:1px solid var(--line);border-radius:12px;font-size:11px;font-weight:750}.about-features strong{color:#728d32;font-size:10px}.about-dialog .about-foot{padding-top:12px;border-top:1px solid var(--line);font-size:10px}
@media(min-width:600px){body{padding:22px 0}.app{min-height:calc(100dvh - 44px);border-radius:28px}.app header{border-radius:28px 28px 0 0}nav{bottom:22px;border-radius:0 0 28px 28px}}
@media(max-width:360px){header{padding:0 15px}main{padding:22px 14px 16px}.card{padding:20px 15px}.class-title{font-size:24px}.status{display:none}nav button{min-width:55px;padding:8px 7px}.form-grid{gap:12px 8px}.camera-box{height:235px}}
@media(prefers-reduced-motion:reduce){*,*:before,*:after{animation-duration:.01ms!important;animation-iteration-count:1!important;scroll-behavior:auto!important;transition-duration:.01ms!important}}
</style>
</head>
<body>
<div class="app">
<header>
 <div class="brand"><div class="logo"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m5 12 4 4L19 6"/></svg></div>Attendly</div>
 <div class="header-actions">
  <button class="round" id="notifications" aria-label="Notifications" title="Notifications"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M18 8a6 6 0 0 0-12 0c0 7-3 7-3 9h18c0-2-3-2-3-9"/><path d="M10 21h4"/></svg></button>
 </div>
</header>
<main>
<section class="view active" id="homeView">
 <div class="hero-row"><div class="eyebrow" id="greeting">WELCOME BACK</div><span class="date-chip" id="todayDate">TODAY</span></div>
 <h1>Ready for your<br>next class?</h1>
 <p class="sub">Stay on track, check your schedule, and make every class count.</p>
 <section class="card">
  <div class="pill"><span class="live-dot"></span> NEXT CLASS · 11:30 AM</div>
  <h2 class="class-title">Human Computer<br>Interaction</h2>
  <div class="meta"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M3 21h18M5 21V5l7-3 7 3v16M9 9h1m4 0h1M9 13h1m4 0h1M10 21v-4h4v4"/></svg><span>Room B12</span><span>·</span><span>Dr. Maya Chen</span></div>
  <button class="dark-btn" id="showMyQR"><span class="qr-icon"><svg class="real-qr-mark" viewBox="0 0 24 24" aria-hidden="true"><path d="M2 2h8v8H2zM14 2h8v8h-8zM2 14h8v8H2z" fill="none" stroke="currentColor" stroke-width="2.2"/><path d="M4.5 4.5h3v3h-3zM16.5 4.5h3v3h-3zM4.5 16.5h3v3h-3zM14 14h3v3h-3zM19 14h3v2h-3zM19 18h3v4h-3zM14 19h3v3h-3z" fill="currentColor"/></svg></span>Show my QR <span class="arrow">→</span></button>
  <p class="note">Your attendance is recorded after a teacher scans your QR.</p>
 </section>
 <div class="section-head"><div><div class="eyebrow">YOUR DAY</div><h2>Today's schedule</h2></div><button class="link" id="viewAll">View schedule →</button></div>
 <div class="schedule">
  <article class="item"><div class="time"><strong>11:30</strong>AM – 12:30</div><div class="symbol"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 4h16v16H4zM8 8h8M8 12h8M8 16h5"/></svg></div><div class="details"><strong>Human Computer Interaction</strong><small>Room B12 · Dr. Maya Chen</small></div><span class="status">UP NEXT</span></article>
  <article class="item"><div class="time"><strong>1:00</strong>PM – 2:00</div><div class="symbol"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 4h16v16H4zM8 8h8v8H8z"/></svg></div><div class="details"><strong>Introduction to Psychology</strong><small>Room A04 · Prof. Liam Brooks</small></div></article>
  <article class="item"><div class="time"><strong>3:15</strong>PM – 4:15</div><div class="symbol"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="m4 16-.8 4.8L8 20l11-11-4-4zM13.5 6.5l4 4M4 4h6"/></svg></div><div class="details"><strong>Visual Design Studio</strong><small>Design Lab · Nina Patel</small></div></article>
 </div>
 <section class="streak"><div class="streak-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M12 3c2 4-2 5 1 8 1-2 2-3 2-3 4 4 4 8 1 11a7 7 0 0 1-11-6c0-3 2-5 4-7 0 3 1 4 2 4 2-2 1-5 1-7z"/></svg></div><div class="streak-info"><strong>You're building great habits!</strong><p>Keep showing up and grow your attendance streak.</p></div><div class="count">8<small>DAY STREAK</small></div></section>
</section>

<section class="view" id="qrView">
 <div class="page-head"><div class="eyebrow">STUDENT PASS</div><h1>Your attendance QR</h1><p class="sub">Add your student details and generate a QR pass to show your teacher.</p></div>
 <div class="panel"><div class="form-grid">
  <div class="field"><label for="studentName">STUDENT NAME</label><input id="studentName" value="Alex Kim" maxlength="60" autocomplete="name"></div>
  <div class="field"><label for="studentId">STUDENT ID</label><input id="studentId" value="20240184" maxlength="30"></div>
  <div class="field full"><label for="group">ATTENDANCE GROUP</label><select id="group"><option>Alpha</option><option>Bravo</option><option>Charlie</option><option>Delta</option></select></div>
  <div class="field"><label for="studentPhone">PHONE NUMBER</label><input id="studentPhone" type="tel" value="" maxlength="20" placeholder="Phone number"></div>
 </div></div>
 <div class="center-status" id="studentStatus">● Waiting for teacher scan</div>
 <div class="student-card"><div class="avatar" id="avatar">AK</div><div class="student-info"><strong id="cardName">Alex Kim</strong><small id="cardDetails">ID · 20240184 · Group Alpha</small></div></div>
 <div class="qr-frame"><div id="qrcode"></div></div>
 <button class="primary" id="downloadQR"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/></svg>Download QR as PNG</button>
 <button class="primary save-student-btn" id="saveStudentDetails" style="margin-top:10px;background:#203b2a;color:#fff"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 3h12l4 4v14H3V3zM7 3v6h10V3M7 21v-7h10v7"/></svg>Save Student Details</button>
 <p class="footnote" id="footerDetails">Student 20240184 · Group Alpha</p>
 <p class="notice">This QR is generated on this device. Shared attendance records need a secure school database and backend.</p>
</section>

<section class="view" id="scheduleView">
 <div class="page-head"><div class="eyebrow">PLAN YOUR DAY</div><h1>Class schedule</h1><p class="sub">Your classes and locations for today.</p></div>
 <div class="schedule">
  <article class="item"><div class="time"><strong>11:30</strong>AM – 12:30</div><div class="symbol">▤</div><div class="details"><strong>Human Computer Interaction</strong><small>Room B12 · Dr. Maya Chen</small></div><span class="status">UP NEXT</span></article>
  <article class="item"><div class="time"><strong>1:00</strong>PM – 2:00</div><div class="symbol">▣</div><div class="details"><strong>Introduction to Psychology</strong><small>Room A04 · Prof. Liam Brooks</small></div></article>
  <article class="item"><div class="time"><strong>3:15</strong>PM – 4:15</div><div class="symbol">✎</div><div class="details"><strong>Visual Design Studio</strong><small>Design Lab · Nina Patel</small></div></article>
 </div>
 <p class="notice">Sample schedule data — edit the class details in the HTML to match your institution.</p>
</section>

<section class="view" id="profileView">
 <div class="page-head"><div class="eyebrow">YOUR ACCOUNT</div><h1>Student profile</h1><p class="sub">Manage your student details and quick actions.</p></div>
 <div class="profile-hero"><div class="avatar" id="profileAvatar">AK</div><div><h2 id="profileName">Alex Kim</h2><p id="profileDetails">20240184 · Group Alpha</p></div></div>
 <div class="panel student-profile-card" id="studentProfileCard"><div class="eyebrow">CURRENT STUDENT DETAILS</div><div class="profile-detail-row"><span>Student name</span><strong id="profileStudentName">Alex Kim</strong></div><div class="profile-detail-row"><span>Student ID</span><strong id="profileStudentId">20240184</strong></div><div class="profile-detail-row"><span>Group</span><strong id="profileStudentGroup">Alpha</strong></div><div class="profile-detail-row"><span>Phone number</span><strong id="profileStudentPhone">Not added</strong></div><p class="profile-save-status" id="profileSaveStatus">Save student details from My QR.</p></div><div class="panel student-profile-card" style="margin-top:12px"><div class="eyebrow">ALL SAVED STUDENTS</div><div id="savedStudentsList" class="attendance-list"><div class="empty-attendance">No student profiles saved yet.</div></div></div>
 <button class="profile-action" id="editProfile"><span class="symbol">✎</span><span>Edit student details</span><span>›</span></button>
 <button class="profile-action" id="openTeacher"><span class="symbol qr-symbol"><svg class="real-qr-mark" viewBox="0 0 24 24" aria-hidden="true"><path d="M2 2h8v8H2zM14 2h8v8h-8zM2 14h8v8H2z" fill="none" stroke="currentColor" stroke-width="2.2"/><path d="M4.5 4.5h3v3h-3zM16.5 4.5h3v3h-3zM4.5 16.5h3v3h-3zM14 14h3v3h-3zM19 14h3v2h-3zM19 18h3v4h-3zM14 19h3v3h-3z" fill="currentColor"/></svg></span><span>Teacher QR scanner</span><span>›</span></button>
 <button class="profile-action" id="aboutApp"><span class="symbol">✦</span><span>About Attendance</span><span>›</span></button>
 <p class="notice">For a real school rollout, add verified teacher accounts, secure login, and a database before relying on attendance results.</p>
</section>

<section class="view" id="teacherView">
 <div class="page-head"><div class="eyebrow">TEACHER TOOLS</div><h1>Scan student QR</h1><p class="sub">Open your phone’s rear camera, allow permission, and point it at a student attendance QR.</p></div>
 <div class="camera-box"><div id="reader"></div><div class="scan-frame"><i class="corner tl"></i><i class="corner tr"></i><i class="corner bl"></i><i class="corner br"></i><i class="scan-line"></i></div></div>
 <div class="camera-message" id="cameraMessage">Camera is not started</div>
 <div class="controls"><button class="control" id="startScan">Start live camera</button><button class="control secondary" id="stopScan">Stop camera</button></div>
 <div class="result" id="scanResult"><h3>Student scan result</h3><p id="resultDetails"></p><button class="control" id="scanAgain" style="width:100%;margin-top:13px">Scan another student</button></div>
 <section class="attendance-panel" id="attendancePanel">
  <div class="attendance-head"><div><div class="eyebrow">ATTENDANCE REGISTER</div><h2>Scanned students</h2><p class="sub">Attendance saved on this device with date and time.</p></div><button class="export-btn" id="exportAttendance">Export PDF</button></div>
  <div class="attendance-stats"><strong id="attendanceCount">0</strong><span>Saved records</span></div>
  <div id="attendanceList" class="attendance-list"><div class="empty-attendance">No attendance recorded yet. Scan a student QR to add a record.</div></div>
 </section>
 <p class="notice">Attendance records are stored in this browser on this device. To share records across teacher/student devices, a secure backend database is required.</p>
</section>
</main>
<nav aria-label="Main navigation">
 <button class="active" data-view="homeView"><span class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="m3 10 9-7 9 7v10a1 1 0 0 1-1 1h-5v-7h-6v7H4a1 1 0 0 1-1-1z" stroke-linejoin="round"/></svg></span>Home</button>
 <button data-view="scheduleView"><span class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><rect x="3" y="5" width="18" height="16" rx="2"/><path d="M16 3v4M8 3v4M3 10h18M8 14h3"/></svg></span>Schedule</button>
 <button data-view="qrView"><span class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M3 3h7v7H3zM14 3h7v7h-7zM3 14h7v7H3zM14 14h3v3h-3zM20 14v2m-3 5h4v-3" stroke-linejoin="round"/></svg></span>My QR</button>
 <button data-view="profileView"><span class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><circle cx="12" cy="8" r="4"/><path d="M4 21a8 8 0 0 1 16 0" stroke-linecap="round"/></svg></span>Profile</button>
</nav>
</div>
<div class="about-overlay" id="aboutOverlay" aria-hidden="true">
 <section class="about-dialog" role="dialog" aria-modal="true" aria-labelledby="aboutTitle">
  <button class="about-close" id="closeAbout" aria-label="Close About Attendance">×</button>
  <div class="about-brand"><div class="logo"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m5 12 4 4L19 6"/></svg></div><span>Attendly</span></div>
  <div class="eyebrow">ABOUT ATTENDANCE</div><h2 id="aboutTitle">Attendance, made simple.</h2>
  <p>Attendly is a student attendance companion for creating student QR passes, scanning them with a teacher's camera, and keeping an organised attendance register.</p>
  <div class="about-features"><div><strong>01</strong><span>Student QR passes</span></div><div><strong>02</strong><span>Scan with date &amp; time</span></div><div><strong>03</strong><span>Local attendance register</span></div></div>
  <p class="about-foot">This version saves attendance in the current browser on this device. A secure school database is needed to synchronise records across devices.</p>
 </section>
</div>
<div class="toast" id="toast" role="status" aria-live="polite"></div>
<script>
const $=id=>document.getElementById(id);
let qrInstance=null,scanner=null,scannerRunning=false,lastScanned='',toastTimer;
function toast(message){const el=$('toast');el.textContent=message;el.classList.add('show');clearTimeout(toastTimer);toastTimer=setTimeout(()=>el.classList.remove('show'),2500)}
function go(viewId){
 if(viewId!=='teacherView')stopCamera();
 document.querySelectorAll('.view').forEach(v=>v.classList.toggle('active',v.id===viewId));
 document.querySelectorAll('nav button').forEach(b=>b.classList.toggle('active',b.dataset.view===viewId));
 window.scrollTo({top:0,behavior:'smooth'});
 if(viewId==='qrView')generateQR();
}
document.querySelectorAll('nav button').forEach(button=>button.addEventListener('click',()=>{go(button.dataset.view);if(button.dataset.view==='scheduleView')toast('Your class schedule');}));
$('showMyQR').onclick=()=>go('qrView');
$('viewAll').onclick=()=>go('scheduleView');
$('notifications').onclick=()=>toast('You are all caught up!');
$('editProfile').onclick=()=>{go('qrView');$('studentName').focus()};
$('openTeacher').onclick=()=>go('teacherView');
$('aboutApp').onclick=()=>{$('aboutOverlay').classList.add('show');$('aboutOverlay').setAttribute('aria-hidden','false')};
$('closeAbout').onclick=closeAbout;
$('aboutOverlay').addEventListener('click',e=>{if(e.target===$('aboutOverlay'))closeAbout()});
document.addEventListener('keydown',e=>{if(e.key==='Escape')closeAbout()});
function closeAbout(){$('aboutOverlay').classList.remove('show');$('aboutOverlay').setAttribute('aria-hidden','true')};
function generateQR(){
 const name=$('studentName').value.trim(),id=$('studentId').value.trim(),group=$('group').value,studentPhone=$('studentPhone').value.trim();
 if(!name||!id){$('qrcode').textContent='Enter a name and student ID to create your QR.';return}
 const payload=JSON.stringify({app:'Attendly',type:'student-attendance',studentName:name,studentId:id,group,studentPhone});
 if(typeof QRCode==='undefined'){$('qrcode').textContent='QR library could not load. Check your internet connection.';return}
 $('qrcode').innerHTML='';
 qrInstance=new QRCode($('qrcode'),{text:payload,width:200,height:200,colorDark:'#10251b',colorLight:'#ffffff',correctLevel:QRCode.CorrectLevel.H});
 const initials=name.split(/\s+/).slice(0,2).map(x=>x[0]?.toUpperCase()||'').join('');
 $('avatar').textContent=initials;$('cardName').textContent=name;$('cardDetails').textContent='ID · '+id+' · Group '+group;
 $('footerDetails').textContent='Student '+id+' · Group '+group;$('profileAvatar').textContent=initials;$('profileName').textContent=name;$('profileDetails').textContent=id+' · Group '+group;renderStudentProfile(currentStudentDetails(),!!readStudentDetails());
}
['studentName','studentId','group','studentPhone'].forEach(id=>{ $(id).addEventListener('input',generateQR);$(id).addEventListener('change',generateQR);});
const STUDENT_DATA_KEY='attendly_student_details_v1';
const STUDENT_LIST_KEY='attendly_all_student_profiles_v1';
function readStudentDetails(){try{return JSON.parse(localStorage.getItem(STUDENT_DATA_KEY)||'null')}catch(e){return null}}
function getSavedStudents(){try{const v=JSON.parse(localStorage.getItem(STUDENT_LIST_KEY)||'[]');return Array.isArray(v)?v:[]}catch(e){return []}}
function currentStudentDetails(){return {studentName:$('studentName').value.trim(),studentId:$('studentId').value.trim(),group:$('group').value,studentPhone:$('studentPhone').value.trim()}}
function applyStudentDetails(data){if(!data)return;if(data.studentName!==undefined)$('studentName').value=data.studentName;if(data.studentId!==undefined)$('studentId').value=data.studentId;if(data.group!==undefined)$('group').value=data.group;if(data.studentPhone!==undefined)$('studentPhone').value=data.studentPhone}
function renderSavedStudents(){const list=getSavedStudents();const el=$('savedStudentsList');if(!el)return;if(!list.length){el.innerHTML='<div class="empty-attendance">No student profiles saved yet. Add each student in My QR and tap Save Student Details.</div>';return}el.innerHTML=list.map((d,i)=>'<article class="saved-student-item"><div class="saved-student-qr" id="savedQr_'+i+'"></div><div><strong>'+escapeHTML(d.studentName)+'</strong><small>ID: '+escapeHTML(d.studentId)+' · Group: '+escapeHTML(d.group)+(d.studentPhone?'<br>Phone: '+escapeHTML(d.studentPhone):'')+'</small><div class="saved-student-actions"><button type="button" class="control secondary" data-load-student="'+escapeHTML(d.studentId)+'">Open profile</button><button type="button" class="control" data-download-student-qr="'+escapeHTML(d.studentId)+'">Download QR</button></div></div></article>').join('');list.forEach((d,i)=>{const target=$('savedQr_'+i);if(target&&typeof QRCode!=='undefined'){target.innerHTML='';new QRCode(target,{text:JSON.stringify({app:'Attendly',type:'student-attendance',studentName:d.studentName,studentId:d.studentId,group:d.group,studentPhone:d.studentPhone||''}),width:68,height:68,colorDark:'#10251b',colorLight:'#ffffff',correctLevel:QRCode.CorrectLevel.M})}});el.querySelectorAll('[data-load-student]').forEach(btn=>btn.addEventListener('click',()=>{const d=getSavedStudents().find(x=>String(x.studentId)===btn.dataset.loadStudent);if(d){applyStudentDetails(d);generateQR();go('qrView');toast('Student details loaded')}}));el.querySelectorAll('[data-download-student-qr]').forEach(btn=>btn.addEventListener('click',()=>{const d=getSavedStudents().find(x=>String(x.studentId)===btn.dataset.downloadStudentQr);const item=btn.closest('.saved-student-item');const canvas=item&&item.querySelector('canvas'),img=item&&item.querySelector('img');let url='';try{if(canvas)url=canvas.toDataURL('image/png');else if(img)url=img.src}catch(e){}if(!url){toast('QR library not ready. Check internet and retry.');return}const a=document.createElement('a');a.href=url;a.download='attendly-'+String(d.studentId).replace(/[^a-z0-9_-]/gi,'_')+'.png';document.body.appendChild(a);a.click();a.remove();toast('Student QR download started')}))}
function renderStudentProfile(data,saved=false){if(!data)return;$('profileStudentName').textContent=data.studentName||'Not added';$('profileStudentId').textContent=data.studentId||'Not added';$('profileStudentGroup').textContent=data.group||'Not added';$('profileStudentPhone').textContent=data.studentPhone||'Not added';$('profileSaveStatus').textContent=saved?'Student profile saved on this device.':'Unsaved changes — open My QR and tap Save Student Details.';renderSavedStudents()}
$('saveStudentDetails').onclick=()=>{const data=currentStudentDetails();if(!data.studentName||!data.studentId){toast('Enter student name and ID first');return}try{const list=getSavedStudents();const index=list.findIndex(x=>String(x.studentId).trim().toLowerCase()===data.studentId.trim().toLowerCase());if(index>=0)list[index]={...list[index],...data,updatedAt:new Date().toISOString()};else list.unshift({...data,createdAt:new Date().toISOString()});localStorage.setItem(STUDENT_LIST_KEY,JSON.stringify(list));localStorage.setItem(STUDENT_DATA_KEY,JSON.stringify(data));renderStudentProfile(data,true);generateQR();toast(index>=0?'Student profile updated':'Student profile saved separately')}catch(e){toast('Could not save student details in this browser')}};
['studentName','studentId','group','studentPhone'].forEach(id=>{$(id).addEventListener('change',()=>{const old=readStudentDetails();if(old&&String(old.studentId)===String($('studentId').value.trim())){try{localStorage.setItem(STUDENT_DATA_KEY,JSON.stringify(currentStudentDetails()))}catch(e){}}})});
$('downloadQR').onclick=()=>{
 const canvas=$('qrcode').querySelector('canvas'),img=$('qrcode').querySelector('img');let url='';
 try{if(canvas)url=canvas.toDataURL('image/png');else if(img)url=img.src}catch(e){}
 if(!url){toast('QR is not ready yet');return}
 const a=document.createElement('a');a.href=url;a.download='attendly-'+$('studentId').value.trim()+'.png';document.body.appendChild(a);a.click();a.remove();toast('QR download started');
};
function setCameraMessage(message,ok=false){$('cameraMessage').textContent=message;$('cameraMessage').className='camera-message'+(ok?' ok':'')}
async function startCamera(){
 if(scannerRunning)return;
 if(typeof Html5Qrcode==='undefined'){setCameraMessage('QR scanner library did not load. Connect to the internet and reload the app.');return}
 // Mobile browsers only expose getUserMedia camera access in a secure web origin.
 // A WhatsApp content:// preview is not a normal HTTPS website and cannot be granted camera access by app code.
 const isLocalHost=['localhost','127.0.0.1','[::1]'].includes(location.hostname);
 if(location.protocol!=='https:' && !isLocalHost){
  setCameraMessage('Live camera needs a secure HTTPS website. This file is open in a WhatsApp/content preview, where camera access is blocked. Open the HTML from an HTTPS website in Chrome, then allow Camera permission.');
  return;
 }
 if(!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia){
  setCameraMessage('This browser does not support live camera access. Open this app in the latest Chrome over HTTPS.');
  return;
 }
 try{
  setCameraMessage('Starting live camera… tap Allow if Chrome asks for camera permission.');
  $('reader').innerHTML='';
  if(!scanner)scanner=new Html5Qrcode('reader',{verbose:true,useBarCodeDetectorIfSupported:true});
  const config={fps:15,qrbox:(vw,vh)=>{const size=Math.max(160,Math.min(270,Math.floor(Math.min(vw*.78,vh*.78))));return {width:size,height:size}},aspectRatio:1,disableFlip:false,videoConstraints:{facingMode:{ideal:'environment'}}};
  try{
   await scanner.start({facingMode:'environment'},config,onScanSuccess,()=>{});
  }catch(firstError){
   const cameras=await Html5Qrcode.getCameras();
   if(!cameras||!cameras.length)throw firstError;
   const chosen=cameras.find(c=>/back|rear|environment|world/i.test(c.label))||cameras[cameras.length-1];
   await scanner.start(chosen.id,config,onScanSuccess,()=>{});
  }
  scannerRunning=true;
  const video=$('reader').querySelector('video');if(video){video.setAttribute('playsinline','true');video.muted=true;video.autoplay=true;try{await video.play()}catch(e){}}
  setCameraMessage('LIVE CAMERA ON · Place the student QR inside the square',true);
 }catch(error){
  console.error('Attendly live camera error:',error);
  const reason=error&&error.name?error.name:'';
  if(!window.isSecureContext)setCameraMessage('Live camera needs HTTPS or localhost. Host this HTML on a secure website, then allow camera access.');
  else if(reason==='NotAllowedError'||reason==='PermissionDeniedError')setCameraMessage('Chrome blocked camera permission. In Chrome, tap the lock/site settings icon → Permissions → Camera → Allow, reload this page, then tap Start live camera. Also close other apps using the camera.');
  else if(reason==='NotFoundError'||reason==='DevicesNotFoundError')setCameraMessage('No camera found on this device. Check that a camera is connected.');
  else setCameraMessage('Live camera could not start ('+(reason||'camera error')+'). Open this page in Chrome over HTTPS, allow Camera permission, close other apps using the camera, and retry.');
 }
}
async function stopCamera(){if(!scanner)return;try{if(scannerRunning)await scanner.stop()}catch(e){}try{scanner.clear()}catch(e){}scannerRunning=false}
function escapeHTML(value){return String(value).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function parseQR(text){try{return JSON.parse(text)}catch(e){return {studentName:'Not provided',studentId:text,group:'Not provided'}}}
const ATTENDANCE_KEY='attendly_attendance_records_v1';
function getAttendanceRecords(){try{const records=JSON.parse(localStorage.getItem(ATTENDANCE_KEY)||'[]');return Array.isArray(records)?records:[]}catch(e){return[]}}
function saveAttendanceRecord(data){
 const id=String(data.studentId||data.id||'Not provided').trim();
 if(!id||id==='Not provided')return {saved:false,reason:'Student ID is missing'};
 const now=new Date(),dateKey=now.getFullYear()+'-'+String(now.getMonth()+1).padStart(2,'0')+'-'+String(now.getDate()).padStart(2,'0');
 const records=getAttendanceRecords();
 if(records.some(r=>String(r.studentId)===id&&r.dateKey===dateKey))return {saved:false,duplicate:true,record:records.find(r=>String(r.studentId)===id&&r.dateKey===dateKey)};
 const record={studentName:String(data.studentName||data.name||'Not provided'),studentId:id,group:String(data.group||'Not provided'),studentPhone:String(data.studentPhone||data.phone||''),timestamp:now.toISOString(),dateKey,timeLabel:now.toLocaleTimeString([], {hour:'2-digit',minute:'2-digit',second:'2-digit'}),dateLabel:now.toLocaleDateString([], {year:'numeric',month:'short',day:'2-digit'})};
 records.unshift(record);localStorage.setItem(ATTENDANCE_KEY,JSON.stringify(records));renderAttendanceRecords();return {saved:true,record};
}
function renderAttendanceRecords(){
 const records=getAttendanceRecords();$('attendanceCount').textContent=records.length;
 if(!records.length){$('attendanceList').innerHTML='<div class="empty-attendance">No attendance recorded yet. Scan a student QR to add a record.</div>';return}
 $('attendanceList').innerHTML=records.map(r=>'<article class="attendance-record"><div><strong>'+escapeHTML(r.studentName)+'</strong><small>ID: '+escapeHTML(r.studentId)+' · Group: '+escapeHTML(r.group)+(r.studentPhone?'<br>Phone: '+escapeHTML(r.studentPhone):'')+'</small><span class="attendance-badge">PRESENT</span></div><time>'+escapeHTML(r.dateLabel||r.dateKey||'')+'<br>'+escapeHTML(r.timeLabel||'')+'</time></article>').join('');
}
$('exportAttendance').onclick=()=>{
 const records=getAttendanceRecords();
 if(!records.length){toast('No attendance records to export');return}
 if(!window.jspdf||!window.jspdf.jsPDF){toast('PDF library unavailable. Check internet and try again.');return}
 const {jsPDF}=window.jspdf;const doc=new jsPDF({orientation:'landscape',unit:'mm',format:'a4'});
 const pageW=doc.internal.pageSize.getWidth(),pageH=doc.internal.pageSize.getHeight();
 const cols=[{label:'Student Name',key:'studentName',w:48},{label:'Student ID',key:'studentId',w:35},{label:'Group',key:'group',w:35},{label:'Phone',key:'studentPhone',w:38},{label:'Date',key:'dateLabel',w:38},{label:'Time',key:'timeLabel',w:30}];
 const totalW=cols.reduce((n,c)=>n+c.w,0),left=(pageW-totalW)/2,rowH=9;let y=40;
 const drawHeader=()=>{doc.setFillColor(19,75,57);doc.rect(left,y,totalW,rowH,'F');doc.setFont('helvetica','bold');doc.setFontSize(9);doc.setTextColor(255,255,255);let x=left;cols.forEach(c=>{doc.text(c.label,x+2,y+5.8);x+=c.w});y+=rowH;};
 doc.setFont('helvetica','bold');doc.setFontSize(19);doc.setTextColor(19,75,57);doc.text('ATTENDLY — ATTENDANCE REGISTER',left,18);
 doc.setFont('helvetica','normal');doc.setFontSize(9);doc.setTextColor(80,80,80);doc.text('Generated: '+new Date().toLocaleString(),left,25);doc.text('Total attendance records: '+records.length,left,31);drawHeader();
 doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(35,35,35);
 records.forEach((r,i)=>{
  if(y+rowH>pageH-12){doc.addPage();y=15;drawHeader();doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(35,35,35)}
  if(i%2===1){doc.setFillColor(240,246,243);doc.rect(left,y,totalW,rowH,'F')}
  doc.setDrawColor(215,225,220);doc.rect(left,y,totalW,rowH,'S');let x=left;
  cols.forEach(c=>{const value=String(r[c.key]??(c.key==='dateLabel'?r.dateKey:'' )??'');const lines=doc.splitTextToSize(value, c.w-4);doc.text(lines[0]||'',x+2,y+5.8);x+=c.w;doc.setDrawColor(215,225,220);doc.line(x,y,x,y+rowH)});y+=rowH;
 });
 const pages=doc.internal.getNumberOfPages();for(let page=1;page<=pages;page++){doc.setPage(page);doc.setFontSize(8);doc.setTextColor(110,110,110);doc.text('Attendly • Page '+page+' of '+pages,pageW-12,pageH-5,{align:'right'})}
 doc.save('attendly-attendance-'+new Date().toISOString().slice(0,10)+'.pdf');toast('Attendance PDF exported');
};
async function onScanSuccess(text){
 if(text===lastScanned)return;lastScanned=text;const data=parseQR(text);
 const name=data.studentName||data.name||'Not provided',id=data.studentId||data.id||'Not provided',group=data.group||'Not provided';
 const attendance=saveAttendanceRecord({...data,studentName:name,studentId:id,group});
 let attendanceMessage=attendance.saved?'Attendance saved':attendance.duplicate?'Already marked present today':'Attendance not saved';
 let stamp=attendance.record?'<br><strong>Date:</strong> '+escapeHTML(attendance.record.dateLabel)+'<br><strong>Time:</strong> '+escapeHTML(attendance.record.timeLabel):'';
 $('resultDetails').innerHTML='<strong>Name:</strong> '+escapeHTML(name)+'<br><strong>Student ID:</strong> '+escapeHTML(id)+'<br><strong>Group:</strong> '+escapeHTML(group)+(data.studentPhone?'<br><strong>Phone:</strong> '+escapeHTML(data.studentPhone):'')+'<br><strong>Result:</strong> '+escapeHTML(attendanceMessage)+stamp;
 $('scanResult').classList.add('show');setCameraMessage(attendance.saved?'QR scanned · Attendance saved':attendance.duplicate?'Already marked present today':'QR scanned · '+attendance.reason,true);await stopCamera();
 if(id===$('studentId').value.trim())$('studentStatus').textContent=attendance.saved?'● Attendance saved':attendance.duplicate?'● Already marked present today':'● QR scanned';
 if(attendance.saved)toast('Attendance saved with date and time');else if(attendance.duplicate)toast('This student is already marked present today');
}
$('startScan').onclick=startCamera;
$('stopScan').onclick=async()=>{await stopCamera();setCameraMessage('Camera stopped')};
$('scanAgain').onclick=()=>{lastScanned='';$('scanResult').classList.remove('show');startCamera()};
const now=new Date(),hour=now.getHours();
$('greeting').textContent=hour<12?'GOOD MORNING':hour<17?'GOOD AFTERNOON':'GOOD EVENING';
$('todayDate').textContent=now.toLocaleDateString(undefined,{weekday:'short',month:'short',day:'numeric'}).toUpperCase();
applyStudentDetails(readStudentDetails());try{const legacy=readStudentDetails();const list=getSavedStudents();if(legacy&&legacy.studentId&&!list.some(x=>String(x.studentId)===String(legacy.studentId))){list.unshift(legacy);localStorage.setItem(STUDENT_LIST_KEY,JSON.stringify(list))}}catch(e){}generateQR();renderStudentProfile(currentStudentDetails(),!!readStudentDetails());renderSavedStudents();renderAttendanceRecords();window.addEventListener('pagehide',stopCamera);
</script>
</body>
</html>
