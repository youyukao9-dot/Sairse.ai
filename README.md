```html
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>airp · 实时聊天</title>
<style>
:root{
  --bg:#dededb;--screen:#f3f3f0;--surface:#f7f7f4;--glass:rgba(255,255,255,.76);--glass2:rgba(255,255,255,.50);
  --line:rgba(46,46,48,.12);--ink:#2f2f32;--muted:#8b8b91;--mine:#ddddda;--other:#fbfbf8;
  --primary:#2b2b2d;--danger:#9e4a4a;--accent:#6366f1;--wallpaper:none;
}
.night{
  --bg:#111214;--screen:#17181a;--surface:#1e1f23;--glass:rgba(32,33,37,.82);--glass2:rgba(38,39,44,.65);
  --line:rgba(255,255,255,.09);--ink:#eaeaea;--muted:#929299;--mine:#2c2d32;--other:#212226;
  --primary:#ededed;--danger:#cf6666;--accent:#818cf8;
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;padding:0;width:100%;height:100%;height:100dvh;overflow:hidden;font-family:-apple-system,BlinkMacSystemFont,"PingFang SC","Microsoft YaHei",sans-serif;color:var(--ink);background:var(--screen)}
button,input,textarea,select{font:inherit}button{border:0;background:none;color:inherit;cursor:pointer}button:active{transform:scale(.96)}

/* 全屏手机视图，无外壳、无多余外层边框 */
.screen{width:100vw;height:100vh;height:100dvh;position:relative;display:flex;flex-direction:column;overflow:hidden;background-color:var(--screen);background-image:var(--wallpaper),radial-gradient(circle at 80% 8%,rgba(255,255,255,.7),transparent 32%),linear-gradient(145deg,var(--screen),var(--bg));background-size:cover;background-position:center}
.night .screen{background-image:var(--wallpaper),radial-gradient(circle at 80% 8%,rgba(255,255,255,.04),transparent 32%),linear-gradient(145deg,#191a1d,#121215)}
.screen:before{content:"";position:absolute;inset:0;pointer-events:none;opacity:.18;background-image:radial-gradient(rgba(50,50,50,.12) .6px,transparent .8px);background-size:6px 6px;mix-blend-mode:multiply;z-index:1}

/* 顶部状态栏 */
.status{height:calc(36px + env(safe-area-inset-top,0px));padding-top:env(safe-area-inset-top,0px);padding-left:18px;padding-right:18px;display:flex;justify-content:space-between;align-items:center;font-size:11px;letter-spacing:.04em;position:relative;z-index:10;user-select:none}
.statusRight{display:flex;gap:8px;align-items:center;font-size:10px}
.connDot{width:6px;height:6px;border-radius:50%;background:#10b981;display:inline-block}.connDot.offline{background:#f59e0b}
.battery{width:21px;height:10px;border:1px solid currentColor;border-radius:3px;padding:1px;position:relative;display:flex;align-items:center}
.battery i{display:block;height:100%;width:80%;background:currentColor;border-radius:1px}
.battery:after{content:"";display:block;width:2px;height:4px;background:currentColor;position:absolute;right:-4px;top:2px;border-radius:0 1px 1px 0}

/* 页面容器与切换 */
.viewContainer{flex:1;min-height:0;position:relative;display:flex;flex-direction:column;z-index:2}
.view{position:absolute;inset:0;display:none;flex-direction:column;overflow:hidden}.view.active{display:flex}

/* 主页 */
.homeContent{flex:1;overflow-y:auto;padding:14px 18px calc(18px + env(safe-area-inset-bottom,0px));display:flex;flex-direction:column;gap:14px;scrollbar-width:none}
.homeContent::-webkit-scrollbar{display:none}
.dateCard{padding:16px 18px;border:1px solid var(--line);background:var(--glass);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border-radius:24px;box-shadow:0 8px 30px rgba(0,0,0,.04)}
.dateLine{display:flex;justify-content:space-between;align-items:flex-end}
.day{font-size:38px;font-weight:300;line-height:1}.month{font-size:11px;color:var(--muted);letter-spacing:.18em;margin-top:4px}
.homeQuote{margin-top:10px;font-size:12px;color:var(--muted);line-height:1.5}
.homeApps{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
.appIcon{aspect-ratio:1;border:1px solid var(--line);background:var(--glass);backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);border-radius:20px;display:flex;flex-direction:column;align-items:center;justify-content:center;position:relative;box-shadow:0 6px 20px rgba(0,0,0,.03)}
.appIcon:before{content:"";position:absolute;inset:7px;border:1px solid var(--line);border-radius:14px;pointer-events:none}
.iconMark{font-size:20px;font-weight:300;line-height:1}.iconLabel{font-size:9px;color:var(--muted);margin-top:5px}

/* 黑胶留声机 */
.turntable{height:106px;border:1px solid var(--line);border-radius:26px;background:var(--glass2);backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);display:flex;align-items:center;gap:16px;padding:14px 16px;box-shadow:0 8px 24px rgba(0,0,0,.04)}
.disc{width:74px;height:74px;flex:0 0 auto;border-radius:50%;background:radial-gradient(circle,#e9e9e7 0 8px,#777 9px 10px,#29292b 11px 31px,#b7b7b5 32px 33px,#353537 34px 37px,#d6d6d2 38px);box-shadow:0 6px 14px rgba(0,0,0,.16);animation:spin 5s linear infinite;animation-play-state:paused}
.disc.play{animation-play-state:running}@keyframes spin{to{transform:rotate(360deg)}}
.turnInfo{min-width:0;flex:1}.turnInfo b{display:block;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}.turnInfo small{display:block;color:var(--muted);font-size:10px;margin-top:4px}
.wave{display:flex;gap:3px;height:18px;align-items:center;margin-top:8px}
.wave i{width:2px;height:5px;background:#888;border-radius:2px;transition:height .2s ease}.wave.play i{animation:soundWave .7s ease-in-out infinite alternate}
.wave i:nth-child(2){animation-delay:.1s}.wave i:nth-child(3){animation-delay:.2s}.wave i:nth-child(4){animation-delay:.3s}.wave i:nth-child(5){animation-delay:.4s}
@keyframes soundWave{to{height:16px}}

/* 主页聊天卡片 */
.homeChatCard{margin-top:auto;padding:14px 16px;border:1px solid var(--line);border-radius:24px;background:var(--glass);backdrop-filter:blur(18px);-webkit-backdrop-filter:blur(18px);display:flex;align-items:center;gap:12px;cursor:pointer;box-shadow:0 8px 24px rgba(0,0,0,.04)}
.avatar{width:38px;height:38px;border-radius:14px;object-fit:cover;background:#d6d6d2;border:1px solid var(--line);flex-shrink:0}
.homeChatTxt{flex:1;min-width:0;text-align:left}.homeChatTxt b{font-size:13px;display:block}.homeChatTxt small{display:block;color:var(--muted);font-size:10px;margin-top:3px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.arrowRight{font-size:18px;color:var(--muted)}

/* 聊天页面 */
.chatHead{height:56px;padding:6px 14px;display:flex;align-items:center;gap:10px;border-bottom:1px solid var(--line);background:var(--glass);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);z-index:5;flex-shrink:0}
.headBack{width:32px;height:32px;display:grid;place-items:center;font-size:24px;color:var(--ink);cursor:pointer;border-radius:10px}
.headInfo{flex:1;min-width:0;text-align:left;cursor:pointer}.headInfo b{font-size:13px;display:block}.headInfo small{display:block;color:var(--muted);font-size:9px;margin-top:2px}
.relTag{font-size:9px;padding:3px 8px;border:1px solid var(--line);border-radius:99px;color:var(--muted);white-space:nowrap;background:rgba(255,255,255,.4)}
.dots{font-size:20px;letter-spacing:2px;cursor:pointer;padding:4px 8px;color:var(--ink)}

.messages{flex:1;min-height:0;overflow-y:auto;padding:14px 14px 10px;scrollbar-width:none;display:flex;flex-direction:column}
.messages::-webkit-scrollbar{display:none}
.msg{display:flex;gap:8px;margin:8px 0;animation:pop .2s ease both}
.msg.mine{justify-content:flex-end}.msg.mine .bubbleWrap{align-items:flex-end}.msg.mine .avatar{order:2}
.bubbleWrap{max-width:82%;display:flex;flex-direction:column;align-items:flex-start}
.bubble{border:1px solid var(--line);padding:10px 13px;border-radius:18px;background:var(--other);font-size:13px;line-height:1.6;box-shadow:0 3px 12px rgba(0,0,0,.03);word-break:break-word;white-space:pre-wrap;position:relative}
.mine .bubble{background:var(--mine);border-top-right-radius:4px}
.other .bubble{border-top-left-radius:4px}
.time{font-size:9px;color:var(--muted);margin:3px 5px}
.quoteBox{border-left:2px solid #888;padding:5px 8px;margin-bottom:7px;color:var(--muted);background:rgba(0,0,0,.04);border-radius:6px;font-size:11px}
.systemMsg{text-align:center;font-size:10px;color:var(--muted);margin:12px auto;padding:4px 12px;border-radius:99px;background:rgba(0,0,0,.04);max-width:85%}

/* 打字机光标 */
.typingCursor{display:inline-block;width:2px;height:1.15em;background:currentColor;vertical-align:-.15em;margin-left:3px;animation:cursorBlink .7s infinite}
@keyframes cursorBlink{0%,100%{opacity:1}50%{opacity:0}}

/* 快捷标点栏 */
.quick{display:flex;gap:6px;overflow-x:auto;padding:6px 12px;scrollbar-width:none;background:rgba(247,247,244,.4);border-top:1px solid var(--line);flex-shrink:0}
.night .quick{background:rgba(20,21,24,.4)}
.quick button{white-space:nowrap;border:1px solid var(--line);background:var(--glass);border-radius:99px;padding:4px 10px;font-size:11px;color:var(--ink);cursor:pointer}

/* 引用提示条 */
.quotingBanner{display:flex;justify-content:space-between;align-items:center;padding:6px 14px;background:var(--mine);border-top:1px solid var(--line);font-size:10px;color:var(--ink)}

/* 输入区域 */
.composer{padding:8px 12px calc(10px + env(safe-area-inset-bottom,0px));display:flex;gap:8px;align-items:flex-end;background:var(--glass);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border-top:1px solid var(--line);flex-shrink:0}
.toolBtn{width:34px;height:34px;border:1px solid var(--line);border-radius:12px;background:var(--glass);display:grid;place-items:center;font-size:15px;color:var(--ink);cursor:pointer;flex-shrink:0}
.inputField{flex:1;min-width:0;border:1px solid var(--line);background:var(--glass);border-radius:16px;min-height:34px;max-height:96px;padding:8px 12px;font-size:13px;line-height:1.45;resize:none;outline:none;color:var(--ink);font-family:inherit}
.sendBtn{height:34px;padding:0 14px;border-radius:12px;background:var(--primary);color:var(--screen);font-size:12px;font-weight:500;cursor:pointer;display:grid;place-items:center;flex-shrink:0}
.stopBtn{height:34px;padding:0 12px;border-radius:12px;background:var(--danger);color:#fff;font-size:11px;font-weight:500;cursor:pointer;display:flex;align-items:center;gap:4px;flex-shrink:0}

/* 空状态引导 */
.chatEmptyState{margin:auto;text-align:center;padding:32px 20px;max-width:280px}
.emptyLogo{width:52px;height:52px;border-radius:20px;border:1px solid var(--line);background:var(--glass);display:grid;place-items:center;font-size:22px;font-weight:300;margin:0 auto 14px}
.emptyTitle{font-size:13px;font-weight:500;margin-bottom:6px}
.emptyDesc{font-size:11px;color:var(--muted);line-height:1.6;margin-bottom:16px}
.emptyBtn{display:inline-block;padding:8px 16px;border-radius:12px;background:var(--primary);color:var(--screen);font-size:11px;font-weight:500;cursor:pointer}

/* 子页面 (朋友圈/好友/记录) */
.subHead{height:56px;display:flex;align-items:center;justify-content:space-between;padding:0 16px;border-bottom:1px solid var(--line);background:var(--glass);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);z-index:5;flex-shrink:0}
.subHead b{font-size:14px}
.subHead button{width:32px;height:32px;display:grid;place-items:center;font-size:20px;color:var(--ink);cursor:pointer}
.subBody{flex:1;min-height:0;overflow-y:auto;padding:14px 16px;scrollbar-width:none}
.subBody::-webkit-scrollbar{display:none}
.bottomNav{height:calc(50px + env(safe-area-inset-bottom,0px));padding-bottom:env(safe-area-inset-bottom,0px);display:flex;border-top:1px solid var(--line);background:var(--glass);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);flex-shrink:0}
.navBtn{flex:1;font-size:10px;color:var(--muted);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;cursor:pointer}
.navBtn.active{color:var(--ink);font-weight:600}

.momentCard{padding:14px;border:1px solid var(--line);background:var(--glass);border-radius:20px;margin-bottom:12px;box-shadow:0 4px 16px rgba(0,0,0,.03)}
.momentHead{display:flex;gap:10px;align-items:center;margin-bottom:8px}.momentHead b{font-size:12px;display:block}.momentHead small{display:block;color:var(--muted);font-size:9px;margin-top:2px}
.momentBody{font-size:12px;line-height:1.6;white-space:pre-wrap;word-break:break-word}
.momentActions{display:flex;gap:14px;margin-top:10px;font-size:11px;color:var(--muted);border-top:1px solid var(--line);padding-top:8px}

.friendRow{display:flex;align-items:center;gap:12px;padding:12px 14px;border:1px solid var(--line);background:var(--glass);border-radius:18px;margin-bottom:10px}
.friendRow .grow{flex:1;min-width:0}.friendRow b{font-size:13px;display:block}.friendRow small{display:block;color:var(--muted);font-size:10px;margin-top:3px}
.miniBtn{padding:6px 12px;border:1px solid var(--line);border-radius:12px;font-size:10px;background:var(--glass);color:var(--ink);cursor:pointer;white-space:nowrap}
.miniBtn.primary{background:var(--primary);color:var(--screen)}

.logItem{padding:11px 13px;border-left:3px solid #888;background:var(--glass);border-radius:0 14px 14px 0;margin-bottom:9px;font-size:11px;line-height:1.5}
.logHeader{display:flex;justify-content:space-between;color:var(--muted);font-size:9px;margin-bottom:4px}

/* 弹窗抽屉 */
.overlay{position:absolute;inset:0;background:rgba(15,15,18,.45);backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px);z-index:50;display:none;align-items:flex-end;padding:12px 12px calc(12px + env(safe-area-inset-bottom,0px));animation:fadeIn .2s ease}
.overlay.show{display:flex}
.modalSheet{width:100%;max-height:88%;overflow-y:auto;background:var(--surface);border:1px solid var(--line);border-radius:28px;box-shadow:0 24px 60px rgba(0,0,0,.25);padding:20px;animation:up .22s cubic-bezier(.16,1,.3,1)}
.modalHead{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px}.modalHead h3{margin:0;font-size:15px;font-weight:600}
.closeBtn{font-size:22px;color:var(--muted);cursor:pointer;padding:2px 6px}
.field{margin:12px 0}.field label{display:block;font-size:11px;color:var(--muted);margin-bottom:6px;font-weight:500}
.field input,.field textarea,.field select{width:100%;border:1px solid var(--line);background:var(--glass);border-radius:14px;padding:10px 12px;font-size:12px;outline:none;color:var(--ink);font-family:inherit}
.field textarea{min-height:76px;resize:vertical}
.fieldHint{font-size:10px;color:var(--muted);margin-top:4px;line-height:1.4}
.modalBtns{display:flex;gap:10px;margin-top:18px}
.modalBtns button{flex:1;padding:11px;border-radius:14px;border:1px solid var(--line);font-size:12px;font-weight:500;background:var(--glass);color:var(--ink);cursor:pointer}
.modalBtns .primary{background:var(--primary);color:var(--screen)}
.divider{height:1px;background:var(--line);margin:16px 0}
.menuGrid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}
.menuGrid button{padding:14px 8px;border:1px solid var(--line);border-radius:16px;background:var(--glass);font-size:11px;color:var(--ink);cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:6px}

/* 轻提示 */
.toast{position:absolute;left:50%;bottom:calc(75px + env(safe-area-inset-bottom,0px));transform:translate(-50%,15px);z-index:90;background:rgba(30,30,32,.94);color:#fff;padding:8px 16px;border-radius:99px;font-size:11px;opacity:0;pointer-events:none;transition:opacity .25s ease,transform .25s ease;white-space:nowrap;max-width:88%;overflow:hidden;text-overflow:ellipsis}
.toast.show{opacity:1;transform:translate(-50%,0)}

@keyframes pop{from{opacity:0;transform:translateY(6px) scale(.98)}to{opacity:1;transform:none}}
@keyframes up{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:none}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
</style>
</head>
<body>
<div class="screen" id="screen">
  <!-- 顶部状态栏 -->
  <header class="status">
    <span id="statusClock">12:00</span>
    <div class="statusRight">
      <span class="connDot" id="connIndicator"></span>
      <span>5G</span>
      <span id="batteryText">85%</span>
      <span class="battery"><i id="batteryBar"></i></span>
    </div>
  </header>

  <div class="viewContainer">
    <!-- 主页 -->
    <section class="view active" id="homeView">
      <div class="homeContent">
        <div class="dateCard">
          <div class="dateLine">
            <div>
              <div class="day" id="homeDay">09</div>
              <div class="month" id="homeMonth">SEP / WEDNESDAY</div>
            </div>
            <div class="iconMark">+</div>
          </div>
          <div class="homeQuote">今天也把一点点温柔留给自己。</div>
        </div>

        <div class="homeApps">
          <button class="appIcon" data-go="chat" type="button">
            <div class="iconMark">+</div>
            <div class="iconLabel">chat</div>
          </button>
          <button class="appIcon" data-go="moments" type="button">
            <div class="iconMark">×</div>
            <div class="iconLabel">moments</div>
          </button>
          <button class="appIcon" data-go="friends" type="button">
            <div class="iconMark">□</div>
            <div class="iconLabel">friends</div>
          </button>
          <button class="appIcon" id="homeSettingsBtn" type="button">
            <div class="iconMark">○</div>
            <div class="iconLabel">settings</div>
          </button>
        </div>

        <div class="turntable">
          <div class="disc" id="homeDisc"></div>
          <div class="turnInfo" id="homeMusicInfo">
            <b id="homeSong">little grey song</b>
            <small id="homeArtist">airp local player</small>
            <div class="wave" id="homeWave"><i></i><i></i><i></i><i></i><i></i></div>
          </div>
          <button class="toolBtn" id="homePlayBtn" type="button">▶</button>
        </div>

        <button class="homeChatCard" data-go="chat" type="button">
          <img class="avatar" id="homeAvatar" alt="avatar">
          <div class="homeChatTxt">
            <b id="homeChatName">小熊</b>
            <small id="homeChatPreview">轻触开启与 airp 的实时对话。</small>
          </div>
          <span class="arrowRight">›</span>
        </button>
      </div>
    </section>

    <!-- 实时聊天界面 -->
    <section class="view" id="chatView">
      <header class="chatHead">
        <button class="headBack" id="chatBackBtn" type="button">‹</button>
        <img class="avatar" id="peerAvatar" alt="avatar" style="width:34px;height:34px;cursor:pointer">
        <div class="headInfo" id="chatTitleInfo">
          <b id="peerName">小熊</b>
          <small id="peerSub">在线 · 实时对话就绪</small>
        </div>
        <span class="relTag" id="relTag" style="display:none">恋人</span>
        <button class="dots" id="chatMenuBtn" type="button">···</button>
      </header>

      <div class="messages" id="messages"></div>

      <div class="quotingBanner" id="quotingBanner" style="display:none">
        <span id="quotingText">引用消息</span>
        <button type="button" id="cancelQuoteBtn">×</button>
      </div>

      <div class="quick" id="quickBar">
        <button data-ins="，">，</button>
        <button data-ins="。">。</button>
        <button data-ins="？">？</button>
        <button data-ins="！">！</button>
        <button data-ins="……">……</button>
        <button data-ins="「」">「」</button>
        <button data-ins="（ ）">（ ）</button>
        <button data-ins="【 】">【 】</button>
      </div>

      <div class="composer">
        <button class="toolBtn" id="chatToolBtn" type="button">＋</button>
        <textarea class="inputField" id="chatInput" rows="1" placeholder="写点什么..."></textarea>
        <button class="sendBtn" id="sendBtn" type="button">发送</button>
        <button class="stopBtn" id="stopBtn" type="button" style="display:none">■ 停止</button>
      </div>
    </section>

    <!-- 朋友圈 -->
    <section class="view" id="momentsView">
      <header class="subHead">
        <button data-go="home" type="button">‹</button>
        <b>朋友圈</b>
        <button id="addMomentBtn" type="button">＋</button>
      </header>
      <div class="subBody" id="momentsList"></div>
      <nav class="bottomNav">
        <button class="navBtn" data-go="chat" type="button"><span>＋</span><span>聊天</span></button>
        <button class="navBtn active" type="button"><span>×</span><span>朋友圈</span></button>
        <button class="navBtn" data-go="friends" type="button"><span>□</span><span>好友</span></button>
      </nav>
    </section>

    <!-- 好友列表 -->
    <section class="view" id="friendsView">
      <header class="subHead">
        <button data-go="home" type="button">‹</button>
        <b>好友</b>
        <button id="addFriendBtn" type="button">＋</button>
      </header>
      <div class="subBody" id="friendsList"></div>
      <nav class="bottomNav">
        <button class="navBtn" data-go="chat" type="button"><span>＋</span><span>聊天</span></button>
        <button class="navBtn" data-go="moments" type="button"><span>×</span><span>朋友圈</span></button>
        <button class="navBtn active" type="button"><span>□</span><span>好友</span></button>
      </nav>
    </section>

    <!-- 查岗 / 后台记录 -->
    <section class="view" id="logsView">
      <header class="subHead">
        <button data-go="chat" type="button">‹</button>
        <b>查岗 / 后台记录</b>
        <button id="clearLogsBtn" type="button" style="font-size:12px">清空</button>
      </header>
      <div class="subBody" id="logsList"></div>
    </section>
  </div>

  <!-- 弹窗抽屉 -->
  <div class="overlay" id="overlay">
    <div class="modalSheet" id="modalSheet"></div>
  </div>

  <!-- Toast 提示 -->
  <div class="toast" id="toast"></div>
</div>

<script>
(()=>{
'use strict';

const $ = s => document.querySelector(s);
const $$ = s => [...document.querySelectorAll(s)];

// 本地存储键
const STORE_KEY = 'airp_mobile_app_v3';
const API_KEY_STORE = 'airp_api_config_v3';

// 默认配置 (纯字符，无任何 emoji)
const defaultState = {
  theme: 'light',
  peerName: '小熊',
  peerAvatar: '',
  myAvatar: '',
  wallpaper: '',
  blocked: false,
  relation: '恋人',
  relationSpace: {
    days: 3,
    lastCheckin: '',
    moods: ['平静沉着'],
    diaries: [{ id: '1', date: '昨天 23:45', text: '只要你在，这里就是属于我们的安静小站。' }]
  },
  music: [
    { id: '1', title: 'little grey song', artist: 'airp local player', url: 'https://cdn.pixabay.com/download/audio/2022/05/27/audio_1808fbf07a.mp3?filename=lofi-study-112191.mp3' }
  ],
  musicIndex: 0,
  musicPlaying: false,
  moments: [
    { id: '1', author: '小熊', time: '今天 00:12', text: '凌晨的风很轻，适合在静默中聆听自己的呼吸。', likes: 8, comments: 1, visibility: '公开' },
    { id: '2', author: '小熊', time: '昨天 22:31', text: '把今天妥帖地折叠起来，留一点空白给明日。', likes: 12, comments: 3, visibility: '公开' }
  ],
  friends: [
    { id: '1', name: '小熊', note: 'airp 核心伴侣' },
    { id: '2', name: '纸飞机', note: '偶尔说晚安的人' },
    { id: '3', name: '白噪音', note: '安静列表里的朋友' }
  ],
  chats: [], // 初始完全清空！不提前做完假对话，等待真实 API 流式输出
  logs: [
    { id: '1', time: '系统就绪', tag: 'BOOT', content: 'airp 全屏手机端界面已加载完成' },
    { id: '2', time: '连接就绪', tag: 'READY', content: '实时流式 API 等待用户发送首条输入' }
  ]
};

const defaultApi = {
  endpoint: 'https://api.deepseek.com/v1/chat/completions',
  apiKey: '',
  model: 'deepseek-chat',
  systemPrompt: '你是 airp，一个克制、敏锐、温和的对话伴侣。你的文字精简而有温度，不用修饰过度的词藻，绝对不使用任何 emoji 表情符号。',
  temperature: 0.7
};

function safeParse(s, fallback) {
  try { return JSON.parse(s) || fallback; } catch { return fallback; }
}

let state = { ...defaultState, ...safeParse(localStorage.getItem(STORE_KEY), {}) };
let apiConfig = { ...defaultApi, ...safeParse(localStorage.getItem(API_KEY_STORE), {}) };

let activeAbortController = null;
let currentQuoting = null;
let audio = new Audio();
audio.preload = 'metadata';

function saveState() {
  localStorage.setItem(STORE_KEY, JSON.stringify(state));
}
function saveApi() {
  localStorage.setItem(API_KEY_STORE, JSON.stringify(apiConfig));
}

function toast(msg) {
  const el = $('#toast');
  el.textContent = msg;
  el.classList.add('show');
  clearTimeout(toast.timer);
  toast.timer = setTimeout(() => el.classList.remove('show'), 2200);
}

function addLog(tag, content) {
  const now = new Date();
  const time = `${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}:${String(now.getSeconds()).padStart(2,'0')}`;
  state.logs.unshift({ id: String(Date.now()), time, tag, content });
  saveState();
  if ($('#logsView').classList.contains('active')) renderLogs();
}

function esc(s='') {
  return String(s).replace(/[&<>"']/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

function avatarSvg(text='我') {
  return 'data:image/svg+xml;charset=UTF-8,' + encodeURIComponent(
    `<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><rect width="100" height="100" rx="28" fill="#d6d6d2"/><circle cx="50" cy="43" r="20" fill="#f5f5f1"/><path d="M25 82c7-20 43-20 50 0" fill="#f5f5f1"/><text x="50" y="92" text-anchor="middle" font-size="9" fill="#777">${esc(text).slice(0,2)}</text></svg>`
  );
}

function applyTheme() {
  document.documentElement.classList.toggle('night', state.theme === 'dark');
  document.documentElement.style.setProperty('--wallpaper', state.wallpaper ? `url("${state.wallpaper.replaceAll('"','\\"')}")` : 'none');
  
  $('#peerName').textContent = state.peerName + (state.blocked ? ' (已拉黑)' : '');
  $('#homeChatName').textContent = state.peerName;
  $('#peerAvatar').src = state.peerAvatar || avatarSvg(state.peerName);
  $('#homeAvatar').src = state.peerAvatar || avatarSvg(state.peerName);
  
  if (state.relation) {
    $('#relTag').style.display = 'inline-block';
    $('#relTag').textContent = state.relation;
  } else {
    $('#relTag').style.display = 'none';
  }

  const track = state.music[state.musicIndex] || defaultState.music[0];
  $('#homeSong').textContent = track.title;
  $('#homeArtist').textContent = track.artist;
  $('#homeDisc').classList.toggle('play', !!state.musicPlaying);
  $('#homeWave').classList.toggle('play', !!state.musicPlaying);
  $('#homePlayBtn').textContent = state.musicPlaying ? '⏸' : '▶';

  const hasApi = Boolean(apiConfig.endpoint && apiConfig.apiKey);
  $('#connIndicator').classList.toggle('offline', !hasApi);
  $('#peerSub').textContent = hasApi ? '在线 · 实时对话就绪' : '在线 · 点击配置 API';
}

function go(viewName) {
  $$('.view').forEach(v => v.classList.remove('active'));
  const target = $('#' + viewName + 'View');
  if (target) target.classList.add('active');
  if (viewName === 'chat') renderChat();
  if (viewName === 'moments') renderMoments();
  if (viewName === 'friends') renderFriends();
  if (viewName === 'logs') renderLogs();
}

$$('[data-go]').forEach(b => b.addEventListener('click', () => go(b.dataset.go)));
$('#chatBackBtn').onclick = () => go('home');

/* 渲染聊天界面 */
function renderChat() {
  const box = $('#messages');
  box.innerHTML = '';

  if (state.chats.length === 0) {
    const hasApi = Boolean(apiConfig.endpoint && apiConfig.apiKey);
    box.innerHTML = `
      <div class="chatEmptyState">
        <div class="emptyLogo">+</div>
        <div class="emptyTitle">airp 实时连接已就绪</div>
        <div class="emptyDesc">${hasApi ? '在下方输入框中输入消息，将通过实时 API 流式输出生成回复。' : '当前尚未配置 API 端点或密钥，请先配置后开始实时对话。'}</div>
        ${!hasApi ? '<button class="emptyBtn" id="emptyConfigBtn" type="button">前往配置 API 端点与密钥</button>' : ''}
      </div>
    `;
    const btn = $('#emptyConfigBtn');
    if (btn) btn.onclick = openApiModal;
    return;
  }

  state.chats.forEach(m => {
    if (m.type === 'system') {
      const el = document.createElement('div');
      el.className = 'systemMsg';
      el.textContent = m.text;
      box.appendChild(el);
      return;
    }

    const isMine = m.sender === 'me';
    const el = document.createElement('div');
    el.className = `msg ${isMine ? 'mine' : 'other'}`;
    el.dataset.id = m.id;

    const av = document.createElement('img');
    av.className = 'avatar';
    av.style.width = '32px';
    av.style.height = '32px';
    av.src = isMine ? (state.myAvatar || avatarSvg('我')) : (state.peerAvatar || avatarSvg(state.peerName));

    const wrap = document.createElement('div');
    wrap.className = 'bubbleWrap';

    let quoteHtml = m.quote ? `<div class="quoteBox">「${esc(m.quote.sender === 'me' ? '我' : state.peerName)}」：${esc(m.quote.text)}</div>` : '';
    let cursorHtml = m.streaming ? `<span class="typingCursor"></span>` : '';

    wrap.innerHTML = `
      <div class="bubble">${quoteHtml}<span>${esc(m.text)}</span>${cursorHtml}</div>
      <div class="time">${esc(m.time || '')}</div>
    `;

    el.append(av, wrap);

    // 双击引用
    el.addEventListener('dblclick', () => {
      currentQuoting = { sender: m.sender, text: m.text.slice(0, 30) };
      $('#quotingText').textContent = `引用「${m.sender === 'me' ? '我' : state.peerName}」：${currentQuoting.text}`;
      $('#quotingBanner').style.display = 'flex';
      $('#chatInput').focus();
    });

    box.appendChild(el);
  });

  box.scrollTop = box.scrollHeight;
}

$('#cancelQuoteBtn').onclick = () => {
  currentQuoting = null;
  $('#quotingBanner').style.display = 'none';
};

/* 快捷标点 */
$$('#quickBar button').forEach(btn => {
  btn.onclick = () => {
    const char = btn.dataset.ins;
    $('#chatInput').value += char;
    $('#chatInput').focus();
  };
});

/* 弹窗抽屉管理 */
function openModal(html) {
  $('#modalSheet').innerHTML = html;
  $('#overlay').classList.add('show');
}
function closeModal() {
  $('#overlay').classList.remove('show');
}
$('#overlay').addEventListener('click', e => {
  if (e.target === $('#overlay')) closeModal();
});

/* API 设置弹窗 */
function openApiModal() {
  openModal(`
    <div class="modalHead">
      <h3>API 接口设置</h3>
      <button class="closeBtn" data-close type="button">×</button>
    </div>
    <div class="field">
      <label>快速填入服务商规范</label>
      <select id="apiPresetSelect">
        <option value="" disabled selected>选择预设配置...</option>
        <option value="deepseek">DeepSeek 官方 (api.deepseek.com)</option>
        <option value="openai">OpenAI 官方 (api.openai.com)</option>
        <option value="silicon">SiliconFlow 硅基流动</option>
        <option value="moonshot">Moonshot 月之暗面</option>
        <option value="ollama">Ollama 本地运行 (localhost:11434)</option>
      </select>
    </div>
    <div class="field">
      <label>API 端点地址 (Endpoint URL)</label>
      <input id="apiEndpointInput" value="${esc(apiConfig.endpoint)}" placeholder="https://api.deepseek.com/v1/chat/completions">
      <div class="fieldHint">兼容 OpenAI 规范的 /chat/completions 接口</div>
    </div>
    <div class="field">
      <label>API 密钥 (API Key)</label>
      <input id="apiKeyInput" type="password" value="${esc(apiConfig.apiKey)}" placeholder="输入 API Key (如 sk-...)">
      <div class="fieldHint">保存在当前设备浏览器本地，绝对不会上传到第三方服务器</div>
    </div>
    <div class="field">
      <label>模型名称 (Model)</label>
      <input id="apiModelInput" value="${esc(apiConfig.model)}" placeholder="例如: deepseek-chat, gpt-4o-mini">
    </div>
    <div class="field">
      <label>系统提示词 (System Prompt)</label>
      <textarea id="apiSystemInput" rows="3">${esc(apiConfig.systemPrompt)}</textarea>
    </div>
    <div style="margin:12px 0">
      <button class="miniBtn" id="testApiBtn" type="button" style="width:100%;padding:9px">测试接口连通性</button>
      <div id="testResultBox" style="margin-top:8px;font-size:11px;display:none"></div>
    </div>
    <div class="modalBtns">
      <button data-close type="button">取消</button>
      <button class="primary" id="saveApiBtn" type="button">保存配置</button>
    </div>
  `);

  $('#apiPresetSelect').onchange = e => {
    const val = e.target.value;
    if (val === 'deepseek') {
      $('#apiEndpointInput').value = 'https://api.deepseek.com/v1/chat/completions';
      $('#apiModelInput').value = 'deepseek-chat';
    } else if (val === 'openai') {
      $('#apiEndpointInput').value = 'https://api.openai.com/v1/chat/completions';
      $('#apiModelInput').value = 'gpt-4o-mini';
    } else if (val === 'silicon') {
      $('#apiEndpointInput').value = 'https://api.siliconflow.cn/v1/chat/completions';
      $('#apiModelInput').value = 'deepseek-ai/DeepSeek-V3';
    } else if (val === 'moonshot') {
      $('#apiEndpointInput').value = 'https://api.moonshot.cn/v1/chat/completions';
      $('#apiModelInput').value = 'moonshot-v1-8k';
    } else if (val === 'ollama') {
      $('#apiEndpointInput').value = 'http://localhost:11434/v1/chat/completions';
      $('#apiModelInput').value = 'llama3.2';
    }
    toast('已应用预设');
  };

  $('#saveApiBtn').onclick = () => {
    apiConfig.endpoint = $('#apiEndpointInput').value.trim();
    apiConfig.apiKey = $('#apiKeyInput').value.trim();
    apiConfig.model = $('#apiModelInput').value.trim();
    apiConfig.systemPrompt = $('#apiSystemInput').value;
    saveApi();
    applyTheme();
    closeModal();
    addLog('CONFIG', `更新 API 配置: ${apiConfig.endpoint} [${apiConfig.model}]`);
    toast('API 配置已保存');
  };

  $('#testApiBtn').onclick = async () => {
    const btn = $('#testApiBtn');
    const resBox = $('#testResultBox');
    btn.disabled = true;
    btn.textContent = '测试中...';
    resBox.style.display = 'none';

    try {
      const endpoint = $('#apiEndpointInput').value.trim();
      const key = $('#apiKeyInput').value.trim();
      const model = $('#apiModelInput').value.trim();
      const startTime = Date.now();

      const headers = { 'Content-Type': 'application/json' };
      if (key) headers['Authorization'] = `Bearer ${key}`;

      const res = await fetch(endpoint, {
        method: 'POST',
        headers,
        body: JSON.stringify({
          model,
          messages: [{ role: 'user', content: 'Ping' }],
          max_tokens: 5,
          stream: false
        })
      });

      const latency = Date.now() - startTime;
      if (res.ok) {
        resBox.style.display = 'block';
        resBox.style.color = '#047857';
        resBox.textContent = `连通成功 (${latency}ms)，端点响应正常。`;
      } else {
        const errText = await res.text();
        resBox.style.display = 'block';
        resBox.style.color = '#b91c1c';
        resBox.textContent = `HTTP ${res.status}: ${errText.slice(0, 100)}`;
      }
    } catch (err) {
      resBox.style.display = 'block';
      resBox.style.color = '#b91c1c';
      resBox.textContent = `连接失败: ${err.message} (若跨域受限，请确认服务端已允许 CORS)`;
    } finally {
      btn.disabled = false;
      btn.textContent = '测试接口连通性';
    }
  };
}

$('#homeSettingsBtn').onclick = openApiModal;

/* 菜单与各功能弹窗 */
$('#chatMenuBtn').onclick = () => {
  openModal(`
    <div class="modalHead">
      <h3>快捷功能与设置</h3>
      <button class="closeBtn" data-close type="button">×</button>
    </div>
    <div class="menuGrid">
      <button data-action="api"><span style="font-size:16px">⚙</span><span>API 设置</span></button>
      <button data-action="music"><span style="font-size:16px">♪</span><span>留声机</span></button>
      <button data-action="moments"><span style="font-size:16px">×</span><span>朋友圈</span></button>
      <button data-action="friends"><span style="font-size:16px">□</span><span>好友列表</span></button>
      <button data-action="logs"><span style="font-size:16px">▤</span><span>查岗记录</span></button>
      <button data-action="relation"><span style="font-size:16px">△</span><span>建立关系</span></button>
      <button data-action="space"><span style="font-size:16px">○</span><span>关系空间</span></button>
      <button data-action="remark"><span style="font-size:16px">✎</span><span>修改备注</span></button>
      <button data-action="avatar"><span style="font-size:16px">◉</span><span>更换头像</span></button>
      <button data-action="wallpaper"><span style="font-size:16px">▦</span><span>更换壁纸</span></button>
      <button data-action="theme"><span style="font-size:16px">◑</span><span>${state.theme === 'dark' ? '白间模式' : '夜间模式'}</span></button>
      <button data-action="clearChat"><span style="font-size:16px">⟳</span><span>清空聊天</span></button>
    </div>
    <div class="divider"></div>
    <button class="miniBtn" data-action="block" style="width:100%;color:${state.blocked ? 'var(--ink)' : 'var(--danger)'};padding:10px">
      ${state.blocked ? '解除拉黑' : '拉黑联系人'}
    </button>
  `);
};

$('#modalSheet').addEventListener('click', e => {
  if (e.target.closest('[data-close]')) {
    closeModal();
    return;
  }
  const btn = e.target.closest('[data-action]');
  if (!btn) return;
  const action = btn.dataset.action;
  closeModal();

  if (action === 'api') openApiModal();
  else if (action === 'music') openMusicModal();
  else if (action === 'moments') go('moments');
  else if (action === 'friends') go('friends');
  else if (action === 'logs') go('logs');
  else if (action === 'relation') openRelationModal();
  else if (action === 'space') openSpaceModal();
  else if (action === 'remark') openRemarkModal();
  else if (action === 'avatar') openAvatarModal();
  else if (action === 'wallpaper') openWallpaperModal();
  else if (action === 'theme') {
    state.theme = state.theme === 'dark' ? 'light' : 'dark';
    saveState();
    applyTheme();
    toast(state.theme === 'dark' ? '已切换至夜间模式' : '已切换至白间模式');
  } else if (action === 'clearChat') {
    state.chats = [];
    saveState();
    renderChat();
    addLog('CLEAR', '清空本地聊天记录');
    toast('聊天记录已清空');
  } else if (action === 'block') {
    state.blocked = !state.blocked;
    saveState();
    applyTheme();
    toast(state.blocked ? '已拉黑联系人' : '已解除拉黑');
  }
});

$('#chatToolBtn').onclick = () => $('#chatMenuBtn').click();
$('#chatTitleInfo').onclick = () => $('#chatMenuBtn').click();

/* 备注修改 */
function openRemarkModal() {
  openModal(`
    <div class="modalHead"><h3>修改备注名</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field"><label>联系人备注</label><input id="remarkInput" value="${esc(state.peerName)}"></div>
    <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="saveRemarkBtn" type="button">保存</button></div>
  `);
  $('#saveRemarkBtn').onclick = () => {
    state.peerName = $('#remarkInput').value.trim() || '小熊';
    saveState();
    applyTheme();
    closeModal();
    toast(`备注已修改为 ${state.peerName}`);
  };
}

/* 更换头像 */
function openAvatarModal() {
  openModal(`
    <div class="modalHead"><h3>头像设置</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field"><label>选择对象</label><select id="avatarWho"><option value="peer">对方头像</option><option value="me">我的头像</option></select></div>
    <div class="field"><label>图片 URL</label><input id="avatarUrl" placeholder="https://..."></div>
    <div class="field"><label>本地相册图片</label><input id="avatarFile" type="file" accept="image/*"></div>
    <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="saveAvatarBtn" type="button">保存</button></div>
  `);
  $('#saveAvatarBtn').onclick = () => {
    const who = $('#avatarWho').value;
    const url = $('#avatarUrl').value.trim();
    const file = $('#avatarFile').files[0];
    const done = src => {
      if (who === 'peer') state.peerAvatar = src;
      else state.myAvatar = src;
      saveState();
      applyTheme();
      closeModal();
      toast('头像已保存');
    };
    if (file) {
      const reader = new FileReader();
      reader.onload = () => done(reader.result);
      reader.readAsDataURL(file);
    } else done(url);
  };
}

/* 更换壁纸 */
function openWallpaperModal() {
  openModal(`
    <div class="modalHead"><h3>聊天背景壁纸</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field"><label>图片网络直链 URL</label><input id="wallUrl" value="${esc(state.wallpaper)}" placeholder="https://..."></div>
    <div class="field"><label>或选取本地相册图片</label><input id="wallFile" type="file" accept="image/*"></div>
    <div class="modalBtns">
      <button id="clearWallBtn" type="button">清除壁纸</button>
      <button class="primary" id="saveWallBtn" type="button">应用壁纸</button>
    </div>
  `);
  $('#clearWallBtn').onclick = () => {
    state.wallpaper = '';
    saveState();
    applyTheme();
    closeModal();
    toast('已清除壁纸');
  };
  $('#saveWallBtn').onclick = () => {
    const url = $('#wallUrl').value.trim();
    const file = $('#wallFile').files[0];
    const done = src => {
      state.wallpaper = src;
      saveState();
      applyTheme();
      closeModal();
      toast('壁纸已应用');
    };
    if (file) {
      const reader = new FileReader();
      reader.onload = () => done(reader.result);
      reader.readAsDataURL(file);
    } else done(url);
  };
}

/* 关系空间 */
function openRelationModal() {
  openModal(`
    <div class="modalHead"><h3>建立关系申请</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field">
      <label>选择关系类型</label>
      <select id="relationSelect">
        <option>恋人</option><option>朋友</option><option>基友</option><option>闺蜜</option><option>家人</option><option>知己</option>
      </select>
    </div>
    <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="sendRelBtn" type="button">建立关系</button></div>
  `);
  $('#sendRelBtn').onclick = () => {
    const rel = $('#relationSelect').value;
    state.relation = rel;
    state.relationSpace.relation = rel;
    state.chats.push({
      id: String(Date.now()),
      sender: 'system',
      type: 'system',
      text: `你们已正式建立「${rel}」关系，已开启专属空间。`
    });
    saveState();
    applyTheme();
    closeModal();
    renderChat();
    toast(`已建立「${rel}」关系`);
  };
}

function openSpaceModal() {
  const space = state.relationSpace;
  const mood = space.moods[0] || '暂无心情记录';
  openModal(`
    <div class="modalHead"><h3>关系空间 · ${esc(state.relation || '恋人')}</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="dateCard" style="margin-bottom:14px">
      <b>已连续打卡 ${space.days} 天</b>
      <div style="font-size:11px;color:var(--muted);margin:6px 0 12px">今日心情：${esc(mood)}</div>
      <button class="sendBtn" id="checkinBtn" type="button" style="width:100%;height:36px">今日打卡记录</button>
    </div>
    <div class="modalBtns" style="margin-top:0;margin-bottom:14px">
      <button id="randomMoodBtn" type="button">随机心情标签</button>
      <button id="writeDiaryBtn" type="button">写日记</button>
    </div>
    <div style="max-height:160px;overflow-y:auto">
      ${space.diaries.map((d,i)=>`
        <div class="logItem">
          <div class="logHeader"><span>${esc(d.date)}</span><button data-del-diary="${i}" style="font-size:10px;color:var(--muted)">删除</button></div>
          <div>${esc(d.text)}</div>
        </div>
      `).join('')}
    </div>
    <div class="divider"></div>
    <button class="miniBtn" id="clearRelBtn" style="width:100%;color:var(--danger);padding:9px" type="button">解除关系并重置空间</button>
  `);

  $('#checkinBtn').onclick = () => {
    const today = new Date().toISOString().slice(0,10);
    if (space.lastCheckin === today) {
      toast('今日已完成打卡');
    } else {
      space.days++;
      space.lastCheckin = today;
      saveState();
      toast('今日打卡成功');
      openSpaceModal();
    }
  };

  $('#randomMoodBtn').onclick = () => {
    const list = ['平静沉着','适度独处','慢节奏','心怀期待','微风清朗','静心阅读','轻盈安宁'];
    const pick = list[Math.floor(Math.random()*list.length)];
    space.moods.unshift(pick);
    saveState();
    toast(`已记录心情：${pick}`);
    openSpaceModal();
  };

  $('#writeDiaryBtn').onclick = () => {
    openModal(`
      <div class="modalHead"><h3>写日记</h3><button class="closeBtn" data-close type="button">×</button></div>
      <div class="field"><textarea id="diaryInput" placeholder="写下属于彼此的话语..."></textarea></div>
      <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="saveDiaryBtn" type="button">发布日记</button></div>
    `);
    $('#saveDiaryBtn').onclick = () => {
      const txt = $('#diaryInput').value.trim();
      if (!txt) return;
      const now = new Date();
      const dateStr = `${now.getMonth()+1}月${now.getDate()}日 ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
      space.diaries.unshift({ id: String(Date.now()), date: dateStr, text: txt });
      saveState();
      toast('日记已存入关系空间');
      openSpaceModal();
    };
  };

  $$('[data-del-diary]').forEach(btn => {
    btn.onclick = () => {
      const idx = Number(btn.dataset.delDiary);
      space.diaries.splice(idx, 1);
      saveState();
      toast('日记已删除');
      openSpaceModal();
    };
  });

  $('#clearRelBtn').onclick = () => {
    state.relation = '';
    state.relationSpace = { days: 0, lastCheckin: '', moods: [], diaries: [] };
    saveState();
    applyTheme();
    closeModal();
    toast('关系已解除');
  };
}

/* 留声机播放器 */
function openMusicModal() {
  const current = state.music[state.musicIndex] || defaultState.music[0];
  openModal(`
    <div class="modalHead"><h3>留声机 · 播放器</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="turntable" style="margin-bottom:16px">
      <div class="disc ${state.musicPlaying ? 'play' : ''}"></div>
      <div class="turnInfo">
        <b>${esc(current.title)}</b>
        <small>${esc(current.artist)}</small>
        <div class="wave ${state.musicPlaying ? 'play' : ''}"><i></i><i></i><i></i><i></i><i></i></div>
      </div>
    </div>
    <div style="display:flex;justify-content:center;gap:16px;margin:12px 0">
      <button class="toolBtn" id="mPrevBtn" type="button">‹‹</button>
      <button class="miniBtn primary" id="mPlayBtn" style="padding:8px 24px;font-size:13px" type="button">${state.musicPlaying ? '暂停' : '播放'}</button>
      <button class="toolBtn" id="mNextBtn" type="button">››</button>
    </div>
    <div class="field"><label>导入网络音频直链 (.mp3 / .m4a)</label><input id="importMusicUrl" placeholder="https://..."></div>
    <div class="field"><label>歌曲名</label><input id="importMusicTitle" placeholder="歌曲名"></div>
    <button class="miniBtn" id="importMusicBtn" type="button" style="width:100%;padding:9px;margin-bottom:14px">加入列表</button>
    <div style="max-height:140px;overflow-y:auto">
      ${state.music.map((m,i)=>`
        <div style="display:flex;justify-content:space-between;align-items:center;padding:8px 4px;border-bottom:1px solid var(--line);font-size:12px">
          <div style="flex:1;cursor:pointer" data-pick-music="${i}">
            <b>${esc(m.title)}</b> <small style="color:var(--muted)">${esc(m.artist)}</small>
          </div>
          <button data-del-music="${i}" style="color:var(--muted);font-size:11px">删</button>
        </div>
      `).join('')}
    </div>
  `);

  $('#mPlayBtn').onclick = () => {
    toggleMusic();
    openMusicModal();
  };
  $('#mPrevBtn').onclick = () => {
    state.musicIndex = (state.musicIndex - 1 + state.music.length) % state.music.length;
    playMusicTrack(state.musicIndex);
    openMusicModal();
  };
  $('#mNextBtn').onclick = () => {
    state.musicIndex = (state.musicIndex + 1) % state.music.length;
    playMusicTrack(state.musicIndex);
    openMusicModal();
  };
  $('#importMusicBtn').onclick = () => {
    const url = $('#importMusicUrl').value.trim();
    const title = $('#importMusicTitle').value.trim() || '未命名音乐';
    if (!url) { toast('请输入直链地址'); return; }
    state.music.push({ id: String(Date.now()), title, artist: '网络音乐', url });
    saveState();
    toast('已添加歌曲');
    openMusicModal();
  };
  $$('[data-pick-music]').forEach(b => {
    b.onclick = () => {
      playMusicTrack(Number(b.dataset.pickMusic));
      openMusicModal();
    };
  });
  $$('[data-del-music]').forEach(b => {
    b.onclick = () => {
      if (state.music.length <= 1) { toast('列表至少保留一首'); return; }
      state.music.splice(Number(b.dataset.delMusic), 1);
      state.musicIndex = 0;
      saveState();
      openMusicModal();
    };
  });
}

function toggleMusic() {
  const track = state.music[state.musicIndex];
  if (!track || !track.url) {
    openMusicModal();
    toast('当前歌曲无音频直链');
    return;
  }
  if (state.musicPlaying) {
    audio.pause();
    state.musicPlaying = false;
    applyTheme();
    toast('已暂停');
  } else {
    audio.src = track.url;
    audio.play().then(() => {
      state.musicPlaying = true;
      applyTheme();
      toast('开始播放');
    }).catch(() => {
      state.musicPlaying = false;
      applyTheme();
      toast('播放失败，请检查直链有效性');
    });
  }
}

function playMusicTrack(idx) {
  state.musicIndex = idx;
  const track = state.music[idx];
  if (track && track.url) {
    audio.src = track.url;
    audio.play().then(() => {
      state.musicPlaying = true;
      applyTheme();
    }).catch(() => {
      state.musicPlaying = false;
      applyTheme();
    });
  }
}

$('#homePlayBtn').onclick = toggleMusic;
$('#homeMusicInfo').onclick = openMusicModal;
$('#homeDisc').onclick = openMusicModal;

/* 朋友圈与好友 */
function renderMoments() {
  $('#momentsList').innerHTML = state.moments.map(m => `
    <article class="momentCard">
      <div class="momentHead">
        <img class="avatar" src="${m.author === '我' ? (state.myAvatar || avatarSvg('我')) : (state.peerAvatar || avatarSvg(m.author))}" style="width:34px;height:34px">
        <div><b>${esc(m.author)}</b><small>${esc(m.time)} · ${esc(m.visibility || '公开')}</small></div>
      </div>
      <p class="momentBody">${esc(m.text)}</p>
      <div class="momentActions">
        <button data-like="${m.id}" type="button">赞 ${m.likes}</button>
        <button type="button">评论 ${m.comments}</button>
      </div>
    </article>
  `).join('');

  $$('[data-like]').forEach(b => {
    b.onclick = () => {
      const id = b.dataset.like;
      const m = state.moments.find(x => x.id === id);
      if (m) {
        m.likes++;
        saveState();
        renderMoments();
        toast('已点赞');
      }
    };
  });
}

$('#addMomentBtn').onclick = () => {
  openModal(`
    <div class="modalHead"><h3>发布朋友圈</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field"><textarea id="momentInput" placeholder="写下这一刻的心境..."></textarea></div>
    <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="pubMomentBtn" type="button">发布</button></div>
  `);
  $('#pubMomentBtn').onclick = () => {
    const t = $('#momentInput').value.trim();
    if (!t) return;
    state.moments.unshift({ id: String(Date.now()), author: '我', time: '刚刚', text: t, likes: 0, comments: 0, visibility: '公开' });
    saveState();
    closeModal();
    renderMoments();
    toast('已发布朋友圈');
  };
};

function renderFriends() {
  $('#friendsList').innerHTML = state.friends.map(f => `
    <div class="friendRow">
      <img class="avatar" src="${avatarSvg(f.name)}" style="width:36px;height:36px">
      <div class="grow"><b>${esc(f.name)}</b><small>${esc(f.note)}</small></div>
      <button class="miniBtn primary" data-chat-friend="${esc(f.name)}" type="button">聊天</button>
    </div>
  `).join('');

  $$('[data-chat-friend]').forEach(b => {
    b.onclick = () => {
      state.peerName = b.dataset.chatFriend;
      saveState();
      applyTheme();
      go('chat');
      toast(`已切换与 ${state.peerName} 对话`);
    };
  });
}

$('#addFriendBtn').onclick = () => {
  openModal(`
    <div class="modalHead"><h3>添加联系人</h3><button class="closeBtn" data-close type="button">×</button></div>
    <div class="field"><label>姓名</label><input id="friendNameInput" placeholder="例如: 纸飞机"></div>
    <div class="field"><label>备注</label><input id="friendNoteInput" placeholder="例如: 偶尔说晚安的人"></div>
    <div class="modalBtns"><button data-close type="button">取消</button><button class="primary" id="saveFriendBtn" type="button">添加</button></div>
  `);
  $('#saveFriendBtn').onclick = () => {
    const name = $('#friendNameInput').value.trim();
    const note = $('#friendNoteInput').value.trim() || '新朋友';
    if (!name) return;
    state.friends.push({ id: String(Date.now()), name, note });
    saveState();
    closeModal();
    renderFriends();
    toast(`已添加好友 ${name}`);
  };
};

function renderLogs() {
  $('#logsList').innerHTML = state.logs.map(l => `
    <div class="logItem">
      <div class="logHeader"><span>${esc(l.tag)}</span><span>${esc(l.time)}</span></div>
      <div>${esc(l.content)}</div>
    </div>
  `).join('');
}
$('#clearLogsBtn').onclick = () => {
  state.logs = [];
  saveState();
  renderLogs();
  toast('后台记录已清空');
};

/* 核心：真实实时流式发送与响应 */
function nowHM() {
  const d = new Date();
  return `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}`;
}

async function sendMessage() {
  const input = $('#chatInput');
  const text = input.value.trim();
  if (!text) return;
  if (state.blocked) { toast('已拉黑联系人，无法发送消息'); return; }

  // 1. 压入用户消息
  const userMsg = {
    id: String(Date.now()),
    sender: 'me',
    type: 'text',
    text,
    time: nowHM(),
    quote: currentQuoting ? { ...currentQuoting } : null
  };
  state.chats.push(userMsg);
  input.value = '';
  currentQuoting = null;
  $('#quotingBanner').style.display = 'none';
  renderChat();
  addLog('USER_MSG', `用户发送: "${text.slice(0,24)}"`);

  // 检查 API 配置
  if (!apiConfig.endpoint || !apiConfig.apiKey) {
    state.chats.push({
      id: String(Date.now()+1),
      sender: 'system',
      type: 'system',
      text: '提示：尚未设置 API 端点或密钥。请点击右上角菜单进入「API 设置」填入密钥后开启实时对话。'
    });
    renderChat();
    openApiModal();
    return;
  }

  // 2. 准备实时流式 AI 占位消息
  const aiMsgId = String(Date.now() + 2);
  const aiMsg = {
    id: aiMsgId,
    sender: 'ai',
    type: 'text',
    text: '',
    time: nowHM(),
    streaming: true
  };
  state.chats.push(aiMsg);
  renderChat();

  // 切换按钮至停止状态
  $('#sendBtn').style.display = 'none';
  $('#stopBtn').style.display = 'flex';
  $('#chatInput').disabled = true;

  activeAbortController = new AbortController();
  addLog('API_STREAM', `发起流式请求 -> ${apiConfig.endpoint}`);

  try {
    // 构造上下文
    const history = state.chats
      .filter(m => m.type === 'text' && m.id !== aiMsgId && m.text.trim())
      .slice(-14)
      .map(m => ({ role: m.sender === 'me' ? 'user' : 'assistant', content: m.text }));

    const messagesPayload = [
      { role: 'system', content: (apiConfig.systemPrompt || '') + '\n【指令】：绝对禁止在输出中使用任何 emoji 表情符号。' },
      ...history
    ];

    const headers = {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiConfig.apiKey.trim()}`
    };

    const res = await fetch(apiConfig.endpoint.trim(), {
      method: 'POST',
      headers,
      body: JSON.stringify({
        model: apiConfig.model.trim() || 'deepseek-chat',
        messages: messagesPayload,
        temperature: Number(apiConfig.temperature) || 0.7,
        stream: true
      }),
      signal: activeAbortController.signal
    });

    if (!res.ok) {
      const errDetail = await res.text();
      throw new Error(`HTTP ${res.status}: ${errDetail.slice(0, 100)}`);
    }

    if (!res.body) throw new Error('未接收到流式输出体');

    const reader = res.body.getReader();
    const decoder = new TextDecoder('utf-8');
    let accumulated = '';
    let buffer = '';

    while (true) {
      const { done, value } = await reader.read();
      if (done) break;

      buffer += decoder.decode(value, { stream: true });
      const lines = buffer.split('\n');
      buffer = lines.pop() || '';

      for (const line of lines) {
        const trimmed = line.trim();
        if (!trimmed || trimmed.startsWith(':')) continue;

        if (trimmed.startsWith('data:')) {
          const dataStr = trimmed.slice(5).trim();
          if (dataStr === '[DONE]') continue;

          try {
            const parsed = JSON.parse(dataStr);
            const delta = parsed.choices?.[0]?.delta?.content || '';
            if (delta) {
              accumulated += delta;
              aiMsg.text = accumulated;
              updateAiBubble(aiMsgId, accumulated, true);
            }
          } catch {}
        }
      }
    }

    // 完成
    aiMsg.streaming = false;
    updateAiBubble(aiMsgId, accumulated, false);
    saveState();
    addLog('API_DONE', `完成流式接收 (${accumulated.length} 字)`);
  } catch (err) {
    if (err.name === 'AbortError') {
      aiMsg.streaming = false;
      aiMsg.text = aiMsg.text || '已手动中止';
      updateAiBubble(aiMsgId, aiMsg.text, false);
      addLog('API_STOP', '用户手动中止流式输出');
    } else {
      aiMsg.streaming = false;
      aiMsg.text = aiMsg.text ? (aiMsg.text + `\n[连接异常: ${err.message}]`) : `[连接失败: ${err.message}]`;
      updateAiBubble(aiMsgId, aiMsg.text, false);
      addLog('API_ERROR', err.message);
      toast(`API 错误: ${err.message}`);
    }
  } finally {
    $('#sendBtn').style.display = 'grid';
    $('#stopBtn').style.display = 'none';
    $('#chatInput').disabled = false;
    $('#chatInput').focus();
    activeAbortController = null;
    saveState();
  }
}

function updateAiBubble(id, text, isStreaming) {
  const el = document.querySelector(`.msg[data-id="${id}"] .bubble span`);
  const cursor = document.querySelector(`.msg[data-id="${id}"] .bubble .typingCursor`);
  if (el) el.textContent = text;
  if (!isStreaming && cursor) cursor.remove();
  const box = $('#messages');
  if (box) box.scrollTop = box.scrollHeight;
}

$('#stopBtn').onclick = () => {
  if (activeAbortController) activeAbortController.abort();
};

$('#sendBtn').onclick = sendMessage;
$('#chatInput').addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    sendMessage();
  }
});

/* 时钟与电量 */
function clock() {
  const d = new Date();
  const time = `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}`;
  $('#statusClock').textContent = time;
  $('#homeDay').textContent = String(d.getDate()).padStart(2,'0');
  $('#homeMonth').textContent = d.toLocaleDateString('en-US', { month: 'short', weekday: 'long' }).toUpperCase();
}
setInterval(clock, 1000);
clock();

if (navigator.getBattery) {
  navigator.getBattery().then(b => {
    const upd = () => {
      const p = Math.round(b.level * 100) + '%';
      $('#batteryText').textContent = p;
      $('#batteryBar').style.width = p;
    };
    upd();
    b.addEventListener('levelchange', upd);
  }).catch(()=>{});
}

// 初始化
applyTheme();
renderChat();
renderMoments();
renderFriends();
renderLogs();
})();
</script>
</body>
</html>
```
