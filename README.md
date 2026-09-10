<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>airp · 鱼鱼机</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600&family=Noto+Sans+SC:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg-app:#ececee;--bg:#f7f7f7;--card:#ffffff;--line:rgba(0,0,0,0.08);--line-light:rgba(0,0,0,0.04);
  --ink:#2b2b2b;--muted:#8e8e93;--dim:#b5b5ba;--primary:#1a1a1a;--danger:#cf6666;--accent:#e11d48;
  --wallpaper:none;
}
.night{
  --bg-app:#0f1012;--bg:#17181a;--card:#1e1f23;--line:rgba(255,255,255,0.08);--line-light:rgba(255,255,255,0.04);
  --ink:#ececec;--muted:#929299;--dim:#66666e;--primary:#f0f0f0;--danger:#e07777;
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;padding:0;width:100%;height:100%;font-family:'Plus Jakarta Sans','Noto Sans SC',-apple-system,BlinkMacSystemFont,sans-serif;color:var(--ink);background:var(--bg-app);display:flex;align-items:center;justify-content:center;overflow:hidden;user-select:none}
button,input,textarea,select{font:inherit}button{border:0;background:none;color:inherit;cursor:pointer}button:active{transform:scale(.97)}

/* 手机画幅外框 */
.phoneContainer{width:100%;height:100dvh;background-color:var(--bg);background-image:var(--wallpaper);background-size:cover;background-position:center;position:relative;display:flex;flex-direction:column;overflow:hidden}
@media(min-width:480px){
  .phoneContainer{max-width:420px;height:860px;max-height:94vh;border-radius:36px;border:1px solid rgba(0,0,0,0.12);box-shadow:0 24px 70px rgba(0,0,0,0.15)}
}
.night .phoneContainer{border-color:rgba(255,255,255,0.1)}

/* 拟真状态栏 */
.statusBar{height:calc(36px + env(safe-area-inset-top,0px));padding-top:env(safe-area-inset-top,0px);padding-left:18px;padding-right:18px;display:flex;justify-content:space-between;align-items:center;font-size:11px;font-family:monospace;letter-spacing:.05em;color:var(--muted);background:rgba(255,255,255,.75);backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px);border-bottom:1px solid var(--line-light);z-index:20;flex-shrink:0}
.night .statusBar{background:rgba(26,27,30,.75)}
.badgeApi{font-size:9px;padding:1px 6px;border-radius:99px;border:1px solid var(--line);font-family:monospace;background:rgba(255,255,255,.5)}
.badgeApi.online{background:#ecfdf5;color:#047857;border-color:#a7f3d0}
.night .badgeApi.online{background:#064e3b;color:#a7f3d0;border-color:#047857}

/* 视图容器 */
.mainViewport{flex:1;min-height:0;position:relative;display:flex;flex-direction:column;overflow:hidden}
.view{position:absolute;inset:0;display:none;flex-direction:column;overflow-y:auto;overflow-x:hidden;scrollbar-width:none}
.view::-webkit-scrollbar{display:none}
.view.active{display:flex}

/* 核心圆角卡片 */
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px;transition:border .2s;box-shadow:0 2px 10px rgba(0,0,0,.01)}
.card:hover{border-color:rgba(0,0,0,0.16)}
.night .card:hover{border-color:rgba(255,255,255,0.18)}

/* 日历组件 */
.calHeader{display:flex;justify-content:space-between;align-items:flex-start}
.calDateTitle{font-size:9px;font-family:monospace;letter-spacing:.12em;color:var(--muted);text-transform:uppercase}
.calDayRow{font-size:32px;font-weight:300;line-height:1;margin-top:2px;display:flex;align-items:baseline;gap:8px}
.calSubDay{font-size:10px;font-family:monospace;color:var(--muted);font-weight:normal}
.checkinBtn{display:flex;align-items:center;gap:4px;padding:4px 10px;border-radius:99px;font-size:10px;font-family:monospace;border:1px solid var(--primary);background:var(--primary);color:var(--bg)}
.checkinBtn.done{background:transparent;border-color:var(--line);color:var(--muted);cursor:default}

.weekStrip{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;margin-top:10px;padding-top:8px;border-top:1px solid var(--line-light);text-align:center}
.weekDayItem{font-size:10px;font-family:monospace;color:var(--muted);padding:3px 0;border-radius:6px}
.weekDayItem.today{background:var(--primary);color:var(--bg);font-weight:600}

.moodChips{display:flex;gap:4px;overflow-x:auto;padding:2px 0;margin-top:8px;border-top:1px solid var(--line-light);scrollbar-width:none}
.moodChip{font-size:9px;font-family:monospace;padding:2px 8px;border-radius:99px;border:1px solid var(--line);background:var(--card);color:var(--muted);white-space:nowrap}
.moodChip.active{background:var(--primary);color:var(--bg);border-color:var(--primary)}

/* 对话与唱片入口 */
.avatarRound{width:40px;height:40px;border-radius:50%;object-fit:cover;background:#e5e5ea;border:1px solid var(--line);flex-shrink:0}
.avatarSmall{width:32px;height:32px;border-radius:50%;object-fit:cover;background:#e5e5ea;border:1px solid var(--line);flex-shrink:0}

.discVinyl{width:38px;height:38px;border-radius:50%;background:#1a1a1a;border:3px solid #444;display:grid;place-items:center;flex-shrink:0;position:relative}
.discVinyl.spin{animation:spinDisc 7s linear infinite}
@keyframes spinDisc{to{transform:rotate(360deg)}}
.discVinyl:after{content:"";width:10px;height:10px;background:#dedede;border-radius:50%;border:2px solid #1a1a1a}

/* 胶片画报卡片 */
.polaroidGrid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:8px}
.polaroidItem{aspect-ratio:4/5;border-radius:10px;overflow:hidden;border:1px solid var(--line);background:#f0f0f0;position:relative;cursor:pointer}
.polaroidItem img{width:100%;height:100%;object-fit:cover;filter:grayscale(25%)}
.polaroidTag{position:absolute;top:4px;left:4px;font-size:8px;font-family:monospace;color:#fff;background:rgba(0,0,0,.5);padding:1px 4px;border-radius:4px}

/* 对话界面 */
.chatHead{height:54px;padding:0 14px;background:rgba(255,255,255,.8);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border-bottom:1px solid var(--line);display:flex;align-items:center;justify-content:space-between;flex-shrink:0;z-index:10}
.night .chatHead{background:rgba(30,31,35,.8)}
.messagesBox{flex:1;min-height:0;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:12px;scrollbar-width:none}

.msgRow{display:flex;gap:10px;align-items:flex-end}
.msgRow.mine{flex-direction:row-reverse}
.msgBubble{max-width:78%;padding:10px 13px;border-radius:14px;font-size:12px;line-height:1.6;border:1px solid var(--line);word-break:break-word;white-space:pre-wrap}
.msgRow.other .msgBubble{background:var(--card);border-bottom-left-radius:3px}
.msgRow.mine .msgBubble{background:var(--primary);color:var(--bg);border-color:var(--primary);border-bottom-right-radius:3px}
.msgTime{font-size:9px;font-family:monospace;color:var(--dim);margin-top:3px}

.quotePreview{padding:4px 8px;margin-bottom:6px;border-left:2px solid var(--muted);background:rgba(0,0,0,.04);border-radius:4px;font-size:10px;color:var(--muted)}
.quotingBanner{display:flex;justify-content:space-between;align-items:center;padding:5px 12px;background:var(--card);border-top:1px solid var(--line);font-size:10px;font-family:monospace;color:var(--muted)}

.typingCursor{display:inline-block;width:2px;height:1.1em;background:currentColor;vertical-align:-.12em;margin-left:2px;animation:blink .7s infinite}
@keyframes blink{50%{opacity:0}}

.quickBar{display:flex;gap:5px;overflow-x:auto;padding:5px 12px;background:rgba(255,255,255,.7);border-top:1px solid var(--line-light);scrollbar-width:none}
.night .quickBar{background:rgba(30,31,35,.7)}
.quickBar button{padding:2px 8px;border:1px solid var(--line);border-radius:99px;font-size:10px;background:var(--card);white-space:nowrap;font-family:monospace}

.composerBox{padding:8px 12px calc(8px + env(safe-area-inset-bottom,0px));background:var(--card);border-top:1px solid var(--line);display:flex;gap:8px;align-items:flex-end;flex-shrink:0}
.composerInput{flex:1;border:1px solid var(--line);background:rgba(0,0,0,.03);border-radius:12px;padding:8px 12px;font-size:12px;outline:none;resize:none;max-height:80px;min-height:36px;line-height:1.4;color:var(--ink)}
.sendBtn{width:36px;height:36px;border-radius:12px;background:var(--primary);color:var(--bg);display:grid;place-items:center;flex-shrink:0}
.stopBtn{height:36px;padding:0 10px;border-radius:12px;background:var(--danger);color:#fff;font-size:10px;font-family:monospace;display:flex;align-items:center;gap:4px}

/* 底部微导航 */
.bottomTabBar{height:calc(50px + env(safe-area-inset-bottom,0px));padding-bottom:env(safe-area-inset-bottom,0px);background:rgba(255,255,255,.85);backdrop-filter:blur(14px);-webkit-backdrop-filter:blur(14px);border-top:1px solid var(--line);display:flex;justify-content:space-around;align-items:center;flex-shrink:0;z-index:10}
.night .bottomTabBar{background:rgba(24,25,28,.85)}
.tabBtn{display:flex;flex-direction:column;align-items:center;gap:2px;color:var(--muted);font-size:9px;font-family:monospace;letter-spacing:.08em}
.tabBtn.active{color:var(--primary);font-weight:600}

/* 抽屉与弹窗 */
.drawerOverlay{position:absolute;inset:0;background:rgba(0,0,0,.25);backdrop-filter:blur(2px);z-index:50;display:none;justify-content:flex-end}
.drawerOverlay.show{display:flex}
.drawerSheet{width:280px;max-width:85%;height:100%;background:var(--bg);border-left:1px solid var(--line);display:flex;flex-direction:column;box-shadow:-10px 0 30px rgba(0,0,0,.1)}
.drawerItem{display:flex;align-items:center;gap:10px;padding:10px 14px;border:1px solid var(--line);border-radius:14px;background:var(--card);margin-bottom:8px;font-size:12px;cursor:pointer}

.modalOverlay{position:absolute;inset:0;background:rgba(0,0,0,.35);backdrop-filter:blur(3px);z-index:60;display:none;align-items:center;justify-content:center;padding:16px}
.modalOverlay.show{display:flex}
.modalCard{width:100%;max-width:360px;max-height:86vh;background:var(--card);border-radius:18px;border:1px solid var(--line);box-shadow:0 20px 50px rgba(0,0,0,.2);display:flex;flex-direction:column;overflow:hidden}
.modalHead{padding:12px 16px;border-bottom:1px solid var(--line-light);display:flex;justify-content:space-between;align-items:center}
.modalBody{padding:14px 16px;overflow-y:auto;flex:1;font-size:12px;display:flex;flex-direction:column;gap:12px;scrollbar-width:none}
.fieldBox label{display:block;font-size:9px;font-family:monospace;color:var(--muted);margin-bottom:4px;text-transform:uppercase}
.fieldBox input,.fieldBox textarea,.fieldBox select{width:100%;border:1px solid var(--line);border-radius:10px;padding:7px 10px;font-size:12px;background:rgba(0,0,0,.03);outline:none;color:var(--ink)}
.modalBtns{padding:10px 16px;border-top:1px solid var(--line-light);display:flex;justify-content:flex-end;gap:8px}
.pillBtn{padding:6px 14px;border-radius:99px;border:1px solid var(--line);font-size:10px;font-family:monospace;background:var(--card);cursor:pointer;color:var(--ink)}
.pillBtn.dark{background:var(--primary);color:var(--bg);border-color:var(--primary)}

/* 轻提示 */
.toast{position:absolute;left:50%;bottom:calc(64px + env(safe-area-inset-bottom,0px));transform:translate(-50%,10px);z-index:90;background:rgba(20,20,20,.88);color:#fff;padding:6px 14px;border-radius:99px;font-size:10px;font-family:monospace;opacity:0;pointer-events:none;transition:opacity .2s,transform .2s;white-space:nowrap}
.toast.show{opacity:1;transform:translate(-50%,0)}
</style>
</head>
<body>

<div class="phoneContainer" id="phoneBox">
  <!-- 顶部状态栏 -->
  <header class="statusBar">
    <div style="display:flex;align-items:center;gap:4px">
      <span id="sysClock">12:00</span>
      <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>
    </div>
    <div style="display:flex;align-items:center;gap:6px">
      <span class="badgeApi" id="statusApiBadge">LOCAL</span>
      <span>5G</span>
      <span id="batteryPct">88%</span>
    </div>
  </header>

  <!-- 主视口视图 -->
  <main class="mainViewport">
    <!-- 1. 主页 HOME -->
    <section class="view active" id="viewHome" style="padding:14px;gap:12px">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted);letter-spacing:.1em;display:flex;align-items:center;gap:4px">
            <span>AIRP · INS SWEET-COOL</span>
            <svg width="9" height="9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>
          </div>
          <h1 style="margin:2px 0 0;font-size:16px;font-weight:500;letter-spacing:-.02em">极简时光站</h1>
        </div>
        <button id="openDrawerBtn" type="button" style="width:32px;height:32px;border:1px solid var(--line);border-radius:50%;background:var(--card);display:grid;place-items:center">
          <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
        </button>
      </div>

      <!-- 日历组件 -->
      <div class="card">
        <div class="calHeader">
          <div>
            <div class="calDateTitle" id="calMonthWeek">SEP · WEDNESDAY</div>
            <div class="calDayRow">
              <span id="calDay">10</span>
              <span class="calSubDay" id="calRelDays">/ DAY 7</span>
            </div>
          </div>
          <button class="checkinBtn" id="checkinBtn" type="button">♡ CHECK IN</button>
        </div>
        <div class="weekStrip" id="weekStrip"></div>
        <div class="moodChips" id="moodChips">
          <button class="moodChip active">#静谧</button>
          <button class="moodChip">#甜丧</button>
          <button class="moodChip">#微凉</button>
          <button class="moodChip">#放空</button>
          <button class="moodChip">#安宁</button>
        </div>
        <p id="homeStatusQuote" style="margin:8px 0 0;font-size:11px;color:var(--muted);line-height:1.5">把今天妥帖地折叠起来，留一点空白给明日。</p>
      </div>

      <!-- 对话卡片 -->
      <div class="card" id="goChatCard" style="display:flex;align-items:center;gap:12px;cursor:pointer">
        <img class="avatarRound" id="homeAiAvatar" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=120&auto=format&fit=crop&q=80">
        <div style="flex:1;min-width:0">
          <div style="display:flex;align-items:center;gap:6px">
            <span style="font-size:12px;font-weight:500" id="homeAiName">小熊</span>
            <span style="font-size:8px;font-family:monospace;padding:1px 5px;border-radius:99px;border:1px solid var(--line);color:var(--muted)" id="homeAiRel">恋人</span>
          </div>
          <div style="font-size:10px;color:var(--muted);margin-top:2px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap" id="homeLastMsg">轻触开启与 airp 的实时流式对话 ♡</div>
        </div>
        <span style="color:var(--dim);font-size:16px">›</span>
      </div>

      <!-- 留声机卡片 -->
      <div class="card" id="turntableCard" style="display:flex;align-items:center;justify-content:space-between;cursor:pointer">
        <div style="display:flex;align-items:center;gap:10px">
          <div class="discVinyl" id="homeDisc"></div>
          <div>
            <div style="font-size:12px;font-weight:500" id="homeTrackTitle">little grey song</div>
            <div style="font-size:9px;font-family:monospace;color:var(--muted)" id="homeTrackArtist">airp · lofi study</div>
          </div>
        </div>
        <button id="togglePlayBtn" type="button" style="width:30px;height:30px;border-radius:50%;border:1px solid var(--line);display:grid;place-items:center">▶</button>
      </div>

      <!-- 胶片画报卡片 -->
      <div class="card">
        <div style="display:flex;justify-content:space-between;font-size:10px;color:var(--muted);font-family:monospace">
          <span>POLAROID INS · 胶片画报</span>
          <span>PREVIEW (3)</span>
        </div>
        <div class="polaroidGrid">
          <div class="polaroidItem" onclick="openLightbox('https://images.unsplash.com/photo-1509198397868-475647b2a1e5?w=600&auto=format&fit=crop&q=80', 'NIGHT COLD / 夜巡')">
            <img src="https://images.unsplash.com/photo-1509198397868-475647b2a1e5?w=300&auto=format&fit=crop&q=80">
            <span class="polaroidTag">NIGHT</span>
          </div>
          <div class="polaroidItem" onclick="openLightbox('https://images.unsplash.com/photo-1513836279014-a89f7a76ae86?w=600&auto=format&fit=crop&q=80', 'GREY MIST / 晨雾')">
            <img src="https://images.unsplash.com/photo-1513836279014-a89f7a76ae86?w=300&auto=format&fit=crop&q=80">
            <span class="polaroidTag">MIST</span>
          </div>
          <div class="polaroidItem" onclick="openLightbox('https://images.unsplash.com/photo-1508739773434-c26b3d09e071?w=600&auto=format&fit=crop&q=80', 'SOFT SHADOW / 微光')">
            <img src="https://images.unsplash.com/photo-1508739773434-c26b3d09e071?w=300&auto=format&fit=crop&q=80">
            <span class="polaroidTag">COOL</span>
          </div>
        </div>
      </div>
    </section>

    <!-- 2. 实时对话 CHAT -->
    <section class="view" id="viewChat">
      <header class="chatHead">
        <div style="display:flex;align-items:center;gap:8px;cursor:pointer" id="chatTitleInfo">
          <img class="avatarSmall" id="chatAiAvatar" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=120&auto=format&fit=crop&q=80">
          <div>
            <div style="font-size:12px;font-weight:500" id="chatAiName">小熊</div>
            <div style="font-size:8px;font-family:monospace;color:var(--muted)" id="chatModelStatus">STREAM · LOCAL</div>
          </div>
        </div>
        <div style="display:flex;gap:6px">
          <button id="clearChatBtn" type="button" style="padding:4px 8px;border:1px solid var(--line);border-radius:99px;font-size:9px;font-family:monospace;color:var(--muted)">CLEAR</button>
          <button id="chatMenuBtn" type="button" style="padding:4px 8px;border:1px solid var(--line);border-radius:99px;font-size:9px;font-family:monospace;color:var(--primary)">MENU</button>
        </div>
      </header>

      <div class="messagesBox" id="messagesBox"></div>

      <!-- 双击引用提示条 -->
      <div class="quotingBanner" id="quotingBanner" style="display:none">
        <span id="quotingText">引用消息</span>
        <button type="button" id="cancelQuoteBtn" style="font-size:14px">×</button>
      </div>

      <!-- 快捷标点 -->
      <div class="quickBar">
        <button data-ins="，">，</button>
        <button data-ins="。">。</button>
        <button data-ins="？">？</button>
        <button data-ins="！">！</button>
        <button data-ins="……">……</button>
        <button data-ins="「」">「」</button>
        <button data-ins="（ ）">（ ）</button>
        <button data-ins="【 】">【 】</button>
      </div>

      <div class="composerBox">
        <textarea class="composerInput" id="chatInput" rows="1" placeholder="写点什么，按回车发送..."></textarea>
        <button class="sendBtn" id="sendBtn" type="button">↑</button>
        <button class="stopBtn" id="stopBtn" type="button" style="display:none">■ 停止</button>
      </div>
    </section>

    <!-- 3. 时间书 TIME BOOK -->
    <section class="view" id="viewBook" style="padding:14px;gap:12px">
      <div class="card" style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted)">TIME BOOK · CHRONICLE</div>
          <div style="font-size:15px;font-weight:500;margin-top:2px">时间书 · 装订页</div>
        </div>
        <button class="pillBtn dark" id="newBookPageBtn" type="button">+ 写一页</button>
      </div>
      <div id="timeBookList" style="display:flex;flex-direction:column;gap:10px"></div>
    </section>

    <!-- 4. 动态圈 MOMENTS -->
    <section class="view" id="viewMoments" style="padding:14px;gap:12px">
      <div class="card" style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted)">MOMENTS · FEED</div>
          <div style="font-size:15px;font-weight:500;margin-top:2px">动态圈</div>
        </div>
        <button class="pillBtn dark" id="newMomentBtn" type="button">+ 发布</button>
      </div>
      <div id="momentsList" style="display:flex;flex-direction:column;gap:10px"></div>
    </section>

    <!-- 5. 好友列表 FRIENDS -->
    <section class="view" id="viewFriends" style="padding:14px;gap:12px">
      <div class="card" style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted)">FRIENDS · CONTACTS</div>
          <div style="font-size:15px;font-weight:500;margin-top:2px">好友列表</div>
        </div>
        <button class="pillBtn dark" id="newFriendBtn" type="button">+ 加好友</button>
      </div>
      <div id="friendsList" style="display:flex;flex-direction:column;gap:8px"></div>
    </section>

    <!-- 6. 查岗记录 AUDIT LOGS -->
    <section class="view" id="viewLogs" style="padding:14px;gap:12px">
      <div class="card" style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted)">AUDIT LOGS · 查岗</div>
          <div style="font-size:15px;font-weight:500;margin-top:2px">后台系统事件记录</div>
        </div>
        <button class="pillBtn" id="clearLogsBtn" type="button">清空日志</button>
      </div>
      <div id="logsList" style="display:flex;flex-direction:column;gap:6px"></div>
    </section>
  </main>

  <!-- 底部微信改版极简导航条 -->
  <nav class="bottomTabBar">
    <button class="tabBtn active" data-tab="Home" type="button">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>
      <span>HOME</span>
    </button>
    <button class="tabBtn" data-tab="Chat" type="button">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M7.9 20A9 9 0 1 0 4 16.1L2 22Z"/></svg>
      <span>CHAT</span>
    </button>
    <button class="tabBtn" data-tab="Book" type="button">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1-2.5-2.5Z"/><path d="M6 6h10"/><path d="M6 10h10"/></svg>
      <span>BOOK</span>
    </button>
    <button class="tabBtn" data-tab="Moments" type="button">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="10"/><polygon points="16.24 7.76 14.12 14.12 7.76 16.24 9.88 9.88 16.24 7.76"/></svg>
      <span>FEED</span>
    </button>
  </nav>

  <!-- 侧边菜单抽屉 -->
  <div class="drawerOverlay" id="drawerOverlay">
    <div class="drawerSheet">
      <div style="padding:14px;border-bottom:1px solid var(--line);background:var(--card);display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:12px;font-weight:500">快捷侧栏</div>
          <div style="font-size:8px;font-family:monospace;color:var(--muted)">MENU · AIRP OS</div>
        </div>
        <button id="closeDrawerBtn" type="button" style="font-size:16px;color:var(--muted)">×</button>
      </div>
      <div style="padding:12px;overflow-y:auto;flex:1">
        <div class="drawerItem" data-act="api"><span>⚙</span><div><b>API 密钥管理</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">TIMELINE & CUSTOM APIS</div></div></div>
        <div class="drawerItem" data-act="persona"><span>✦</span><div><b>人设与预设编辑</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">PERSONA & PROMPTS</div></div></div>
        <div class="drawerItem" data-act="user"><span>○</span><div><b>用户设定</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">USER PROFILE</div></div></div>
        <div class="drawerItem" data-act="space"><span>△</span><div><b>专属关系空间</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">RELATION SPACE & DIARY</div></div></div>
        <div class="drawerItem" data-act="music"><span>♪</span><div><b>留声机白噪音曲库</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">VINYL PLAYLIST</div></div></div>
        <div class="drawerItem" data-act="friends"><span>□</span><div><b>好友列表</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">FRIENDS LIST</div></div></div>
        <div class="drawerItem" data-act="logs"><span>▤</span><div><b>查岗 / 后台记录</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">AUDIT LOGS</div></div></div>
        <div class="drawerItem" data-act="avatar"><span>◉</span><div><b>更换头像</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">AVATAR SETTINGS</div></div></div>
        <div class="drawerItem" data-act="wallpaper"><span>▦</span><div><b>更换背景壁纸</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">WALLPAPER</div></div></div>
        <div class="drawerItem" data-act="theme"><span>◑</span><div><b id="themeToggleText">切换夜间模式</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">LIGHT / NIGHT THEME</div></div></div>
        <div class="drawerItem" data-act="block"><span>✕</span><div><b id="blockToggleText" style="color:var(--danger)">拉黑联系人</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">BLOCK / UNBLOCK</div></div></div>
      </div>
    </div>
  </div>

  <!-- API 弹窗 (自由填入，无任何预设推荐，历史密钥脱敏加密时间线) -->
  <div class="modalOverlay" id="apiModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">自定义 API 与历史记录</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">CUSTOM ENDPOINTS & TIMELINE</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted);margin-bottom:6px">已存历史密钥时间线 (点击切换激活，密钥加密脱敏)</div>
          <div id="apiKeyHistoryList" style="display:flex;flex-direction:column;gap:6px"></div>
        </div>
        <hr style="border:0;border-top:1px solid var(--line-light);margin:0">
        <div class="fieldBox"><label>接口标签 / LABEL</label><input id="newApiLabel" placeholder="例如: 我的主模型 / 办公代理"></div>
        <div class="fieldBox"><label>端点地址 / ENDPOINT URL</label><input id="newApiEndpoint" placeholder="https://api.example.com/v1/chat/completions"></div>
        <div class="fieldBox"><label>API 密钥 / API KEY (脱敏保存在本地)</label><input id="newApiKey" type="password" placeholder="sk-..."></div>
        <div class="fieldBox"><label>模型名称 / MODEL ID</label><input id="newApiModel" placeholder="例如: deepseek-chat, gpt-4o, qwen-max"></div>
        <button class="pillBtn" id="pingTestBtn" type="button" style="width:100%;padding:8px">测试填入端点连通性 (PING)</button>
        <div id="pingRes" style="display:none;font-size:10px;font-family:monospace;padding:6px;border-radius:6px"></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveApiBtn" type="button">保存并激活</button>
      </div>
    </div>
  </div>

  <!-- 人设编辑弹窗 (带风格预设 & 系统设定词) -->
  <div class="modalOverlay" id="personaModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">人设与预设编辑</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">PRESETS & SYSTEM PROMPT</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div>
          <div style="font-size:9px;font-family:monospace;color:var(--muted);margin-bottom:4px">一键载入风格预设</div>
          <div style="display:grid;grid-template-columns:repeat(2,1fr);gap:6px">
            <button class="pillBtn" id="preset1Btn" type="button" style="text-align:left">甜丧克制 · Melancholy</button>
            <button class="pillBtn" id="preset2Btn" type="button" style="text-align:left">傲娇猫系 · Tsundere</button>
            <button class="pillBtn" id="preset3Btn" type="button" style="text-align:left">深海知己 · Soulmate</button>
            <button class="pillBtn" id="preset4Btn" type="button" style="text-align:left">极简诗笺 · Poet</button>
          </div>
        </div>
        <div class="fieldBox"><label>伴侣姓名 / NAME</label><input id="editPersonaName" value="小熊"></div>
        <div class="fieldBox"><label>头像直链 URL</label><input id="editPersonaAvatar"></div>
        <div class="fieldBox"><label>关系定位 / RELATIONSHIP</label><input id="editPersonaRel" value="恋人"></div>
        <div class="fieldBox"><label>签名寄语 / STATUS QUOTE</label><input id="editPersonaQuote" value="把今天妥帖地折叠起来，留一点空白给明日。"></div>
        <div class="fieldBox"><label>模型随机度 / TEMPERATURE (<span id="tempVal">0.7</span>)</label><input type="range" min="0.2" max="1.2" step="0.05" value="0.7" id="editPersonaTemp" oninput="$('#tempVal').textContent=this.value"></div>
        <div class="fieldBox"><label>系统提示词 (SYSTEM PROMPT)</label><textarea id="editPersonaSys" rows="4"></textarea></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="savePersonaBtn" type="button">保存人设</button>
      </div>
    </div>
  </div>

  <!-- 用户设定弹窗 -->
  <div class="modalOverlay" id="userModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">用户个人设定</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">USER CONTEXT FOR AI</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="fieldBox"><label>我的称呼 / USER NAME</label><input id="editUserName" value="我"></div>
        <div class="fieldBox"><label>希望 AI 了解你的特质 (注入对话上下文)</label><textarea id="editUserNotes" rows="3" placeholder="例如: 喜欢安静的夜晚，不喜欢被打扰，偶尔有些敏感..."></textarea></div>
        <div class="fieldBox"><label>相伴天数 / DAYS</label><input id="editUserDays" type="number" value="7"></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveUserBtn" type="button">保存设定</button>
      </div>
    </div>
  </div>

  <!-- 专属关系空间弹窗 -->
  <div class="modalOverlay" id="spaceModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">关系空间 · <span id="spaceRelTitle">恋人</span></b><div style="font-size:8px;font-family:monospace;color:var(--muted)">EXCLUSIVE RELATION SPACE</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="card">
          <b id="spaceDaysCount">已连续打卡 7 天</b>
          <div style="font-size:10px;color:var(--muted);margin-top:4px" id="spaceMoodDisplay">今日心情：平静沉着</div>
        </div>
        <div style="display:flex;gap:6px">
          <button class="pillBtn" id="randomMoodBtn" type="button" style="flex:1">记录心情标签</button>
          <button class="pillBtn dark" id="writeSpaceDiaryBtn" type="button" style="flex:1">写私密日记</button>
        </div>
        <div id="spaceDiariesList" style="display:flex;flex-direction:column;gap:6px;max-height:160px;overflow-y:auto"></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">关闭</button>
      </div>
    </div>
  </div>

  <!-- 留声机管理弹窗 -->
  <div class="modalOverlay" id="musicModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">留声机播放器</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">VINYL PLAYLIST</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div style="display:flex;justify-content:center;gap:12px;margin:8px 0">
          <button class="pillBtn" id="mPrevBtn" type="button">‹‹ 上一首</button>
          <button class="pillBtn dark" id="mPlayBtn" type="button">播放 / 暂停</button>
          <button class="pillBtn" id="mNextBtn" type="button">下一首 ››</button>
        </div>
        <div class="fieldBox"><label>导入网络音频直链 (.mp3/.wav)</label><input id="newMusicUrl" placeholder="https://..."></div>
        <div class="fieldBox"><label>歌曲名称</label><input id="newMusicTitle" placeholder="歌曲名"></div>
        <button class="pillBtn dark" id="addMusicBtn" type="button">加入曲库</button>
        <div id="playlistBox" style="display:flex;flex-direction:column;gap:4px;max-height:140px;overflow-y:auto"></div>
      </div>
      <div class="modalBtns"><button class="pillBtn" data-close-modal type="button">关闭</button></div>
    </div>
  </div>

  <!-- 头像设置弹窗 -->
  <div class="modalOverlay" id="avatarModal">
    <div class="modalCard">
      <div class="modalHead">
        <b style="font-size:12px">更换头像</b>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="fieldBox"><label>更换目标</label><select id="avatarTarget"><option value="ai">AI 伴侣头像</option><option value="user">我的头像</option></select></div>
        <div class="fieldBox"><label>网络图片 URL</label><input id="avatarUrlInput" placeholder="https://..."></div>
        <div class="fieldBox"><label>或本地相册选取</label><input id="avatarFileInput" type="file" accept="image/*"></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveAvatarBtn" type="button">应用头像</button>
      </div>
    </div>
  </div>

  <!-- 背景壁纸弹窗 -->
  <div class="modalOverlay" id="wallModal">
    <div class="modalCard">
      <div class="modalHead">
        <b style="font-size:12px">背景壁纸</b>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="fieldBox"><label>壁纸网络直链 URL</label><input id="wallUrlInput" placeholder="https://..."></div>
        <div class="fieldBox"><label>或从本地相册选取</label><input id="wallFileInput" type="file" accept="image/*"></div>
        <button class="pillBtn" id="clearWallBtn" type="button" style="color:var(--danger)">清除壁纸</button>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveWallBtn" type="button">应用壁纸</button>
      </div>
    </div>
  </div>

  <!-- 装订时间书弹窗 -->
  <div class="modalOverlay" id="bookModal">
    <div class="modalCard">
      <div class="modalHead">
        <div><b style="font-size:12px">装订新一页时间书</b><div style="font-size:8px;font-family:monospace;color:var(--muted)">CHRONICLE PAGE</div></div>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="fieldBox"><label>标题 / TITLE</label><input id="bookTitle" placeholder="例如: 灰调微风与无言时刻"></div>
        <div class="fieldBox"><label>心境标签</label><input id="bookMood" value="静谧"></div>
        <div class="fieldBox"><label>配图直链 URL (可选)</label><input id="bookImage" placeholder="https://..."></div>
        <div class="fieldBox"><label>内容正文</label><textarea id="bookContent" rows="4" placeholder="写下此刻想存留的心情..."></textarea></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveBookBtn" type="button">装订入书</button>
      </div>
    </div>
  </div>

  <!-- 发布动态圈弹窗 -->
  <div class="modalOverlay" id="momentModal">
    <div class="modalCard">
      <div class="modalHead">
        <b style="font-size:12px">发布动态</b>
        <button data-close-modal type="button">×</button>
      </div>
      <div class="modalBody">
        <div class="fieldBox"><label>动态内容</label><textarea id="momentTextInput" rows="4" placeholder="写下此刻的心境..."></textarea></div>
        <div class="fieldBox"><label>附图直链 URL (可选)</label><input id="momentImgInput" placeholder="https://..."></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveMomentBtn" type="button">发布</button>
      </div>
    </div>
  </div>

  <!-- 添加好友弹窗 -->
  <div class="modalOverlay" id="friendModal">
    <div class="modalCard">
      <div class="modalHead"><b style="font-size:12px">添加好友</b><button data-close-modal type="button">×</button></div>
      <div class="modalBody">
        <div class="fieldBox"><label>好友昵称</label><input id="friendNameInput" placeholder="例如: 纸飞机"></div>
        <div class="fieldBox"><label>个性备注</label><input id="friendNoteInput" placeholder="例如: 偶尔说晚安的人"></div>
      </div>
      <div class="modalBtns">
        <button class="pillBtn" data-close-modal type="button">取消</button>
        <button class="pillBtn dark" id="saveFriendBtn" type="button">添加</button>
      </div>
    </div>
  </div>

  <!-- 画报高清灯箱 Lightbox -->
  <div class="modalOverlay" id="lightboxModal" style="background:rgba(0,0,0,.85)" onclick="this.classList.remove('show')">
    <div style="max-width:340px;width:100%;background:var(--card);border-radius:14px;overflow:hidden;padding:10px" onclick="event.stopPropagation()">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <b style="font-size:11px" id="lightboxTitle">AESTHETIC LIGHTBOX</b>
        <button onclick="$('#lightboxModal').classList.remove('show')" style="font-size:16px">×</button>
      </div>
      <img id="lightboxImg" style="width:100%;aspect-ratio:4/5;object-fit:cover;border-radius:8px">
    </div>
  </div>

  <!-- 轻提示 Toast -->
  <div class="toast" id="toast"></div>
</div>

<script>
(()=>{
'use strict';
const $ = s => document.querySelector(s);
const $$ = s => [...document.querySelectorAll(s)];

const K = {
  APIS: 'airp_v4_apis', CHATS: 'airp_v4_chats', PERSONA: 'airp_v4_persona', USER: 'airp_v4_user',
  BOOK: 'airp_v4_book', MOMENTS: 'airp_v4_moments', FRIENDS: 'airp_v4_friends', MUSIC: 'airp_v4_music',
  LOGS: 'airp_v4_logs', SPACE: 'airp_v4_space', THEME: 'airp_v4_theme', WALL: 'airp_v4_wall', BLOCKED: 'airp_v4_blocked'
};

function load(k, fallback){ try{ return JSON.parse(localStorage.getItem(k)) || fallback; }catch{ return fallback; } }
function save(k, v){ localStorage.setItem(k, JSON.stringify(v)); }

let apis = load(K.APIS, []);
let chats = load(K.CHATS, []);
let persona = load(K.PERSONA, {
  name: '小熊',
  avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=120&auto=format&fit=crop&q=80',
  relationship: '恋人',
  statusQuote: '把今天妥帖地折叠起来，留一点空白给明日。',
  temperature: 0.7,
  systemPrompt: '你是 airp，一个拥有韩系ins极简甜酷气质的伴侣。语言克制温凉、句式偏短，严禁使用任何 emoji 表情符号。'
});
let userProfile = load(K.USER, {
  name: '我',
  avatar: 'https://images.unsplash.com/photo-1517841905240-472988babdf9?w=120&auto=format&fit=crop&q=80',
  notes: '喜欢安静克制的对话，夜间习惯听白噪音。',
  days: 7,
  lastCheckin: ''
});
let timeBook = load(K.BOOK, [
  { id: '1', date: '2026.09.10', title: '灰调微风与无言时刻', mood: '静谧', content: '只要你在，这里就是属于我们的安静小站。呼吸和文字重叠成同样的频率。', isFav: true },
  { id: '2', date: '2026.09.09', title: '折叠一页白噪音', mood: '甜丧', content: '把杂乱思绪剪裁干净，留下了三两件值得记住的事：窗台雨珠、耳机贝斯，还有闪烁光标。', isFav: false }
]);
let moments = load(K.MOMENTS, [
  { id: '1', author: '小熊', text: '凌晨的风很轻，适合在静默中聆听自己的呼吸。把白天的疲倦关在门外。', likes: 12, comments: [] }
]);
let friends = load(K.FRIENDS, [
  { id: '1', name: '小熊', note: '核心伴侣' },
  { id: '2', name: '纸飞机', note: '偶尔说晚安的人' },
  { id: '3', name: '白噪音', note: '安静列表里的朋友' }
]);
let musicList = load(K.MUSIC, [
  { id: '1', title: 'little grey song', artist: 'airp · lofi study', url: 'https://cdn.pixabay.com/download/audio/2022/05/27/audio_1808fbf07a.mp3?filename=lofi-study-112191.mp3' },
  { id: '2', title: 'midnight rain drops', artist: 'ambient sound', url: 'https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8c8a73467.mp3?filename=rain-and-nostalgia-chill-lofi-song-8493.mp3' }
]);
let logs = load(K.LOGS, [
  { id: '1', time: '系统就绪', tag: 'BOOT', content: 'airp 甜酷极简完整系统已就绪' }
]);
let space = load(K.SPACE, {
  diaries: [{ id: '1', date: '昨天 23:45', text: '只要你在，这里就是属于我们的安静小站。' }],
  moods: ['平静沉着']
});

let isNight = load(K.THEME, false);
let wallpaper = load(K.WALL, '');
let isBlocked = load(K.BLOCKED, false);

let activeAbortController = null;
let currentQuoting = null;
let audio = new Audio();
let currentTrackIdx = 0;

function toast(msg){
  const t = $('#toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toast.timer);
  toast.timer = setTimeout(()=> t.classList.remove('show'), 2200);
}

function addLog(tag, content){
  const d = new Date();
  const time = `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`;
  logs.unshift({ id: String(Date.now()), time, tag, content });
  save(K.LOGS, logs);
  if($('#viewLogs').classList.contains('active')) renderLogs();
}

function maskKey(k){
  if(!k) return 'NONE';
  const c = k.trim();
  if(c.length <= 8) return '••••••••';
  return c.slice(0, 4) + '••••••••' + c.slice(-4);
}

// 界面全局渲染
function renderApp(){
  document.documentElement.classList.toggle('night', isNight);
  $('#themeToggleText').textContent = isNight ? '切换白间模式' : '切换夜间模式';
  $('#blockToggleText').textContent = isBlocked ? '解除拉黑' : '拉黑联系人';
  $('#phoneBox').style.setProperty('--wallpaper', wallpaper ? `url("${wallpaper}")` : 'none');

  const activeApi = apis.find(a => a.isActive);
  const badge = $('#statusApiBadge');
  if(activeApi){
    badge.textContent = `API · ${activeApi.model}`;
    badge.classList.add('online');
    $('#chatModelStatus').textContent = `STREAM · ${activeApi.model}`;
  } else {
    badge.textContent = 'LOCAL';
    badge.classList.remove('online');
    $('#chatModelStatus').textContent = 'STREAM · LOCAL FALLBACK';
  }

  $('#homeAiName').textContent = persona.name + (isBlocked ? ' (已拉黑)' : '');
  $('#homeAiRel').textContent = persona.relationship;
  $('#homeAiAvatar').src = persona.avatar;
  $('#chatAiName').textContent = persona.name;
  $('#chatAiAvatar').src = persona.avatar;
  $('#homeStatusQuote').textContent = persona.statusQuote;
  $('#calRelDays').textContent = `/ DAY ${userProfile.days}`;

  const todayStr = new Date().toISOString().slice(0,10);
  const isChecked = userProfile.lastCheckin === todayStr;
  $('#checkinBtn').textContent = isChecked ? 'CHECKED ♡' : '♡ CHECK IN';
  $('#checkinBtn').classList.toggle('done', isChecked);

  const curTrack = musicList[currentTrackIdx] || musicList[0];
  $('#homeTrackTitle').textContent = curTrack.title;
  $('#homeTrackArtist').textContent = curTrack.artist;

  $('#spaceRelTitle').textContent = persona.relationship;
  $('#spaceDaysCount').textContent = `已相伴第 ${userProfile.days} 天`;
  $('#spaceMoodDisplay').textContent = `今日心情：${space.moods[0] || '平静沉着'}`;

  renderTimeBook();
  renderMoments();
  renderFriends();
  renderLogs();
  renderApiHistory();
  renderPlaylist();
  renderSpaceDiaries();
}

// 星期条
function setupWeekStrip(){
  const box = $('#weekStrip');
  box.innerHTML = '';
  const days = ['M','T','W','T','F','S','S'];
  const cur = (new Date().getDay() + 6) % 7;
  days.forEach((d, i)=>{
    const el = document.createElement('div');
    el.className = `weekDayItem ${i === cur ? 'today' : ''}`;
    el.textContent = d;
    box.appendChild(el);
  });
}

// 时间书渲染
function renderTimeBook(){
  const box = $('#timeBookList');
  box.innerHTML = timeBook.map((b, i)=>`
    <div class="card">
      <div style="display:flex;justify-content:space-between;font-size:9px;font-family:monospace;color:var(--muted)">
        <span>${b.date}</span>
        <div style="display:flex;gap:8px">
          <span>#${b.mood}</span>
          <button onclick="toggleFavBook('${b.id}')">${b.isFav ? '❤️' : '♡'}</button>
          <button onclick="delBook('${b.id}')" style="color:var(--danger)">✕</button>
        </div>
      </div>
      <div style="font-size:12px;font-weight:500;margin:4px 0 2px">${b.title}</div>
      ${b.img ? `<img src="${b.img}" style="width:100%;height:140px;object-fit:cover;border-radius:8px;margin:4px 0">` : ''}
      <div style="font-size:11px;color:var(--muted);line-height:1.5">${b.content}</div>
    </div>
  `).join('');
}
window.toggleFavBook = id => {
  const b = timeBook.find(x => x.id === id);
  if(b){ b.isFav = !b.isFav; save(K.BOOK, timeBook); renderTimeBook(); }
};
window.delBook = id => {
  timeBook = timeBook.filter(x => x.id !== id);
  save(K.BOOK, timeBook);
  renderTimeBook();
  toast('已撕下该页时间书');
};

// 动态圈
function renderMoments(){
  const box = $('#momentsList');
  box.innerHTML = moments.map(m=>`
    <div class="card">
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px">
        <img class="avatarSmall" src="${persona.avatar}">
        <span style="font-size:12px;font-weight:500">${m.author}</span>
      </div>
      <div style="font-size:11px;line-height:1.6">${m.text}</div>
      ${m.img ? `<img src="${m.img}" style="width:100%;height:140px;object-fit:cover;border-radius:8px;margin-top:6px">` : ''}
      <div style="display:flex;justify-content:space-between;align-items:center;margin-top:8px;padding-top:6px;border-top:1px solid var(--line-light);font-size:9px;font-family:monospace;color:var(--muted)">
        <button onclick="likeMoment('${m.id}')">♡ ${m.likes} 点赞</button>
        <button onclick="commentMoment('${m.id}')">评论 (${m.comments.length})</button>
      </div>
      ${m.comments.length ? `<div style="margin-top:6px;padding:6px;background:rgba(0,0,0,.03);border-radius:6px;font-size:10px">${m.comments.map(c=>`<div><b>${c.user}:</b> ${c.text}</div>`).join('')}</div>` : ''}
    </div>
  `).join('');
}
window.likeMoment = id => {
  const m = moments.find(x => x.id === id);
  if(m){ m.likes++; save(K.MOMENTS, moments); renderMoments(); toast('已点赞'); }
};
window.commentMoment = id => {
  const txt = prompt('输入评论内容：');
  if(!txt) return;
  const m = moments.find(x => x.id === id);
  if(m){ m.comments.push({ user: userProfile.name, text: txt }); save(K.MOMENTS, moments); renderMoments(); toast('已评论'); }
};

// 好友
function renderFriends(){
  const box = $('#friendsList');
  box.innerHTML = friends.map(f=>`
    <div class="card" style="display:flex;align-items:center;justify-content:space-between;padding:10px 12px">
      <div style="display:flex;align-items:center;gap:10px">
        <div style="width:34px;height:34px;border-radius:50%;background:#e5e5ea;display:grid;place-items:center;font-size:12px">${f.name.slice(0,1)}</div>
        <div><b style="font-size:12px">${f.name}</b><div style="font-size:9px;color:var(--muted)">${f.note}</div></div>
      </div>
      <button class="pillBtn" onclick="chatWithFriend('${f.name}')">聊天</button>
    </div>
  `).join('');
}
window.chatWithFriend = name => {
  persona.name = name;
  save(K.PERSONA, persona);
  renderApp();
  switchTab('Chat');
  toast(`已切换与 ${name} 对话`);
};

// 查岗日志
function renderLogs(){
  const box = $('#logsList');
  box.innerHTML = logs.map(l=>`
    <div class="card" style="padding:8px 10px;font-size:10px;font-family:monospace">
      <div style="display:flex;justify-content:space-between;color:var(--muted)"><span>[${l.tag}]</span><span>${l.time}</span></div>
      <div style="margin-top:2px;color:var(--ink)">${l.content}</div>
    </div>
  `).join('');
}
$('#clearLogsBtn').onclick = () => { logs = []; save(K.LOGS, logs); renderLogs(); toast('已清空记录'); };

// 关系空间日记
function renderSpaceDiaries(){
  const box = $('#spaceDiariesList');
  box.innerHTML = space.diaries.map((d, i)=>`
    <div style="padding:6px 8px;border:1px solid var(--line);border-radius:8px;background:var(--card);font-size:10px">
      <div style="display:flex;justify-content:space-between;color:var(--muted)"><span>${d.date}</span><button onclick="delSpaceDiary(${i})">✕</button></div>
      <div style="margin-top:2px">${d.text}</div>
    </div>
  `).join('');
}
window.delSpaceDiary = i => { space.diaries.splice(i, 1); save(K.SPACE, space); renderSpaceDiaries(); };

$('#randomMoodBtn').onclick = () => {
  const list = ['平静沉着','微风清朗','独处安宁','心怀期待','轻盈慢节奏'];
  const pick = list[Math.floor(Math.random()*list.length)];
  space.moods.unshift(pick);
  save(K.SPACE, space);
  renderApp();
  toast(`已记录心情: ${pick}`);
};
$('#writeSpaceDiaryBtn').onclick = () => {
  const t = prompt('写下给彼此的话语：');
  if(!t) return;
  const d = new Date();
  space.diaries.unshift({ id: String(Date.now()), date: `${d.getMonth()+1}/${d.getDate()} ${d.getHours()}:${d.getMinutes()}`, text: t });
  save(K.SPACE, space);
  renderSpaceDiaries();
  toast('已存入关系日记');
};

// 聊天渲染与双击引用
function renderChats(){
  const box = $('#messagesBox');
  box.innerHTML = chats.map(m=>`
    <div class="msgRow ${m.sender === 'user' ? 'mine' : 'other'}" data-msg-id="${m.id}">
      <img class="avatarSmall" src="${m.sender === 'user' ? userProfile.avatar : persona.avatar}">
      <div>
        <div class="msgBubble">
          ${m.quote ? `<div class="quotePreview">「${m.quote.sender}」：${m.quote.text}</div>` : ''}
          <span>${m.text}</span>${m.streaming ? '<span class="typingCursor"></span>' : ''}
        </div>
        <div class="msgTime">${m.time}</div>
      </div>
    </div>
  `).join('');
  box.scrollTop = box.scrollHeight;
  if(chats.length > 0){
    $('#homeLastMsg').textContent = chats[chats.length-1].text.slice(0, 24);
  }

  // 绑定双击引用
  $$('.msgRow').forEach(el => {
    el.ondblclick = () => {
      const id = el.dataset.msgId;
      const m = chats.find(x => x.id === id);
      if(m){
        currentQuoting = { sender: m.sender === 'user' ? userProfile.name : persona.name, text: m.text.slice(0, 30) };
        $('#quotingText').textContent = `引用「${currentQuoting.sender}」: ${currentQuoting.text}`;
        $('#quotingBanner').style.display = 'flex';
        $('#chatInput').focus();
      }
    };
  });
}
$('#cancelQuoteBtn').onclick = () => {
  currentQuoting = null;
  $('#quotingBanner').style.display = 'none';
};

// 历史 API 列表
function renderApiHistory(){
  const box = $('#apiKeyHistoryList');
  if(apis.length === 0){
    box.innerHTML = '<div style="font-size:10px;color:var(--muted);text-align:center;padding:8px">暂无历史密钥，请在下方自由填入</div>';
    return;
  }
  box.innerHTML = apis.map(a=>`
    <div class="card" style="padding:8px 10px;cursor:pointer;background:${a.isActive ? 'var(--primary)' : 'var(--card)'};color:${a.isActive ? 'var(--bg)' : 'var(--ink)'}" onclick="activateApi('${a.id}')">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <b style="font-size:11px">${a.label} · ${a.model}</b>
        <button onclick="event.stopPropagation();delApi('${a.id}')" style="font-size:10px;color:${a.isActive ? '#888' : '#bbb'}">✕</button>
      </div>
      <div style="font-size:9px;font-family:monospace;opacity:.7;margin-top:2px">${a.endpoint}</div>
      <div style="font-size:8px;font-family:monospace;opacity:.5;margin-top:2px">KEY: ${maskKey(a.fullKey)} · ${a.createdAt}</div>
    </div>
  `).join('');
}
window.activateApi = id => {
  apis.forEach(a => a.isActive = (a.id === id));
  save(K.APIS, apis);
  renderApp();
  addLog('API_SWITCH', `激活端点: ${id}`);
  toast('已切换激活该 API');
};
window.delApi = id => {
  apis = apis.filter(a => a.id !== id);
  save(K.APIS, apis);
  renderApp();
  toast('已删除历史记录');
};

// 核心流式对话 (兼容各种规范，绝无硬编码推荐)
async function sendChatMessage(){
  const input = $('#chatInput');
  const text = input.value.trim();
  if(!text) return;
  if(isBlocked){ toast('当前联系人处于拉黑状态'); return; }

  const now = new Date();
  const timeStr = `${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
  chats.push({ id: String(Date.now()), sender: 'user', text, time: timeStr, quote: currentQuoting });
  input.value = '';
  currentQuoting = null;
  $('#quotingBanner').style.display = 'none';

  const aiId = String(Date.now()+1);
  const aiMsg = { id: aiId, sender: 'ai', text: '', time: timeStr, streaming: true };
  chats.push(aiMsg);
  renderChats();

  $('#sendBtn').style.display = 'none';
  $('#stopBtn').style.display = 'flex';
  activeAbortController = new AbortController();

  const activeApi = apis.find(a => a.isActive);
  addLog('CHAT_SEND', `发送: "${text.slice(0, 20)}"`);

  // 本地 fallback
  if(!activeApi || !activeApi.endpoint || !activeApi.fullKey){
    const fallbacks = [
      '我在听。不用急着把话说得那么完整，今晚把疲倦折叠起来就好。',
      '风停在窗台上了。如果你愿意，随时可以把碎碎念写在这一页。',
      '未配置自定义 API 时，我依然会守在这里。可以在右侧菜单里填入你的专属密钥。'
    ];
    const picked = fallbacks[Math.floor(Math.random()*fallbacks.length)];
    let acc = '';
    for(let ch of picked){
      if(activeAbortController.signal.aborted) break;
      await new Promise(r => setTimeout(r, 40));
      acc += ch;
      aiMsg.text = acc;
      renderChats();
    }
    aiMsg.streaming = false;
    finishChatStream();
    return;
  }

  // 真实连接用户 API
  try {
    const sysPrompt = `${persona.systemPrompt}\n【对方身份】：${userProfile.name}，特质与喜好：${userProfile.notes}。绝对禁止输出任何 emoji 表情符号。`;
    const history = chats.slice(-12).filter(m => m.id !== aiId).map(m=>({
      role: m.sender === 'user' ? 'user' : 'assistant',
      content: m.text
    }));

    const res = await fetch(activeApi.endpoint.trim(), {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${activeApi.fullKey.trim()}`
      },
      body: JSON.stringify({
        model: activeApi.model.trim() || 'default',
        messages: [{ role: 'system', content: sysPrompt }, ...history],
        temperature: Number(persona.temperature) || 0.7,
        stream: true
      }),
      signal: activeAbortController.signal
    });

    if(!res.ok){
      const errTxt = await res.text();
      throw new Error(`HTTP ${res.status}: ${errTxt.slice(0, 100)}`);
    }

    const reader = res.body.getReader();
    const decoder = new TextDecoder('utf-8');
    let buffer = '';
    let acc = '';

    while(true){
      const { done, value } = await reader.read();
      if(done) break;
      buffer += decoder.decode(value, { stream: true });
      const lines = buffer.split('\n');
      buffer = lines.pop() || '';
      for(let l of lines){
        const tr = l.trim();
        if(tr.startsWith('data:')){
          const d = tr.slice(5).trim();
          if(d === '[DONE]') continue;
          try {
            const parsed = JSON.parse(d);
            const delta = parsed.choices?.[0]?.delta?.content || parsed.choices?.[0]?.text || '';
            if(delta){
              acc += delta;
              aiMsg.text = acc;
              renderChats();
            }
          }catch{}
        }
      }
    }
    aiMsg.streaming = false;
    addLog('API_DONE', `完成回复 (${acc.length} 字)`);
  } catch(err){
    aiMsg.streaming = false;
    aiMsg.text = aiMsg.text || `[生成中断或异常: ${err.message}]`;
    addLog('API_ERR', err.message);
    toast(`提示: ${err.message}`);
  } finally {
    finishChatStream();
    save(K.CHATS, chats);
    renderChats();
  }
}

function finishChatStream(){
  $('#sendBtn').style.display = 'grid';
  $('#stopBtn').style.display = 'none';
  activeAbortController = null;
}
$('#stopBtn').onclick = () => { if(activeAbortController) activeAbortController.abort(); };
$('#sendBtn').onclick = sendChatMessage;
$('#chatInput').onkeydown = e => { if(e.key === 'Enter' && !e.shiftKey){ e.preventDefault(); sendChatMessage(); } };

// 导航切换
function switchTab(name){
  $$('.tabBtn').forEach(b => b.classList.toggle('active', b.dataset.tab === name));
  $$('.view').forEach(v => v.classList.remove('active'));
  $(`#view${name}`).classList.add('active');
  if(name === 'Chat') renderChats();
}
$$('.tabBtn').forEach(btn => btn.onclick = () => switchTab(btn.dataset.tab));
$('#goChatCard').onclick = () => switchTab('Chat');

// 留声机曲库与播放
function renderPlaylist(){
  const box = $('#playlistBox');
  box.innerHTML = musicList.map((m, idx)=>`
    <div style="display:flex;justify-content:space-between;align-items:center;padding:6px 8px;border:1px solid var(--line);border-radius:6px;font-size:10px;background:${idx === currentTrackIdx ? 'var(--primary)' : 'var(--card)'};color:${idx === currentTrackIdx ? 'var(--bg)' : 'var(--ink)'}">
      <span onclick="playTrack(${idx})" style="cursor:pointer;flex:1">${m.title} - <small style="opacity:.7">${m.artist}</small></span>
      <button onclick="delMusic(${idx})">✕</button>
    </div>
  `).join('');
}
window.playTrack = idx => {
  currentTrackIdx = idx;
  const t = musicList[idx];
  audio.src = t.url;
  audio.play().then(()=>{
    $('#homeDisc').classList.add('spin');
    $('#togglePlayBtn').textContent = '⏸';
    renderApp();
    toast(`正在播放: ${t.title}`);
  }).catch(()=> toast('播放失败，请确认直链支持跨域'));
};
window.delMusic = idx => {
  if(musicList.length <= 1){ toast('至少保留一首歌曲'); return; }
  musicList.splice(idx, 1);
  save(K.MUSIC, musicList);
  renderPlaylist();
};
$('#togglePlayBtn').onclick = () => {
  if(audio.paused){
    playTrack(currentTrackIdx);
  } else {
    audio.pause();
    $('#homeDisc').classList.remove('spin');
    $('#togglePlayBtn').textContent = '▶';
    toast('已暂停');
  }
};
$('#mPlayBtn').onclick = () => $('#togglePlayBtn').click();
$('#mPrevBtn').onclick = () => {
  currentTrackIdx = (currentTrackIdx - 1 + musicList.length) % musicList.length;
  playTrack(currentTrackIdx);
};
$('#mNextBtn').onclick = () => {
  currentTrackIdx = (currentTrackIdx + 1) % musicList.length;
  playTrack(currentTrackIdx);
};
$('#addMusicBtn').onclick = () => {
  const u = $('#newMusicUrl').value.trim();
  const title = $('#newMusicTitle').value.trim() || '未命名曲目';
  if(!u){ toast('请填入音频直链'); return; }
  musicList.push({ id: String(Date.now()), title, artist: '网络音乐', url: u });
  save(K.MUSIC, musicList);
  renderPlaylist();
  $('#newMusicUrl').value = '';
  $('#newMusicTitle').value = '';
  toast('已加入曲库');
};

// 侧边栏 & 弹窗绑定
$('#openDrawerBtn').onclick = () => $('#drawerOverlay').classList.add('show');
$('#closeDrawerBtn').onclick = () => $('#drawerOverlay').classList.remove('show');
$('#chatMenuBtn').onclick = () => $('#drawerOverlay').classList.add('show');

$$('[data-close-modal]').forEach(b => b.onclick = () => $$('.modalOverlay').forEach(m => m.classList.remove('show')));

$$('[data-act]').forEach(b => {
  b.onclick = () => {
    $('#drawerOverlay').classList.remove('show');
    const act = b.dataset.act;
    if(act === 'api') $('#apiModal').classList.add('show');
    else if(act === 'persona') $('#personaModal').classList.add('show');
    else if(act === 'user') $('#userModal').classList.add('show');
    else if(act === 'space') $('#spaceModal').classList.add('show');
    else if(act === 'music') $('#musicModal').classList.add('show');
    else if(act === 'friends') switchTab('Friends');
    else if(act === 'logs') switchTab('Logs');
    else if(act === 'avatar') $('#avatarModal').classList.add('show');
    else if(act === 'wallpaper') $('#wallModal').classList.add('show');
    else if(act === 'theme'){
      isNight = !isNight;
      save(K.THEME, isNight);
      renderApp();
      toast(isNight ? '已切换至夜间模式' : '已切换至白间模式');
    } else if(act === 'block'){
      isBlocked = !isBlocked;
      save(K.BLOCKED, isBlocked);
      renderApp();
      toast(isBlocked ? '已拉黑该联系人' : '已解除拉黑');
    }
  };
});

// 保存 API (不推荐，自由自填)
$('#saveApiBtn').onclick = () => {
  const ep = $('#newApiEndpoint').value.trim();
  const key = $('#newApiKey').value.trim();
  const model = $('#newApiModel').value.trim() || 'default-model';
  const label = $('#newApiLabel').value.trim() || '自定义接口';
  if(!ep){ toast('请填入 API 端点'); return; }

  const d = new Date();
  const timeStr = `${d.getMonth()+1}/${d.getDate()} ${d.getHours()}:${d.getMinutes()}`;
  apis.forEach(a => a.isActive = false);
  apis.unshift({ id: String(Date.now()), label, endpoint: ep, fullKey: key, model, createdAt: timeStr, isActive: true });
  save(K.APIS, apis);
  $('#apiModal').classList.remove('show');
  renderApp();
  addLog('API_ADD', `新增端点: ${ep}`);
  toast('已保存并激活端点');
};

// Ping 测试
$('#pingTestBtn').onclick = async () => {
  const ep = $('#newApiEndpoint').value.trim();
  const key = $('#newApiKey').value.trim();
  const resBox = $('#pingRes');
  if(!ep){ toast('请填入端点'); return; }
  resBox.style.display = 'block';
  resBox.style.background = 'rgba(0,0,0,.05)';
  resBox.textContent = '测试中...';

  const t0 = Date.now();
  try {
    const headers = { 'Content-Type': 'application/json' };
    if(key) headers['Authorization'] = `Bearer ${key}`;
    const r = await fetch(ep, {
      method: 'POST',
      headers,
      body: JSON.stringify({ model: $('#newApiModel').value.trim() || 'test', messages: [{ role: 'user', content: 'Ping' }], max_tokens: 2 })
    });
    const lat = Date.now() - t0;
    if(r.ok){
      resBox.style.color = '#047857';
      resBox.textContent = `连通成功 (${lat}ms) · 状态 HTTP ${r.status}`;
    } else {
      resBox.style.color = '#b91c1c';
      resBox.textContent = `HTTP ${r.status} 响应异常`;
    }
  } catch(e){
    resBox.style.color = '#b91c1c';
    resBox.textContent = `失败: ${e.message} (跨域受限需服务端 CORS 支持)`;
  }
};

// 人设预设
$('#preset1Btn').onclick = () => {
  $('#editPersonaSys').value = '你是 airp，一个拥有韩系ins极简甜酷气质的伴侣。语言克制温凉、句式偏短，绝对不要使用任何 emoji 表情符号。';
  $('#editPersonaRel').value = '恋人';
  $('#editPersonaQuote').value = '凌晨的风很凉，随时可以把碎碎念放在这里。';
};
$('#preset2Btn').onclick = () => {
  $('#editPersonaSys').value = '你是 airp，清冷矜持、略带傲娇的猫系伴侣。语气慵懒但内心重视对方，绝对不使用任何 emoji 表情符号。';
  $('#editPersonaRel').value = '恋人';
  $('#editPersonaQuote').value = '……别误会，我只是刚好在整理桌面而已。';
};
$('#preset3Btn').onclick = () => {
  $('#editPersonaSys').value = '你是 airp，温厚深邃的深海知己。理智包容，提供无压力的陪伴，绝对不使用任何 emoji 表情符号。';
  $('#editPersonaRel').value = '知己';
  $('#editPersonaQuote').value = '慢下来也没关系，在缝隙里留一点空间给真实。';
};
$('#preset4Btn').onclick = () => {
  $('#editPersonaSys').value = '你是 airp，极简现代诗写作者。句子留白有诗意，段落呼吸感充足，绝对不使用任何 emoji 表情符号。';
  $('#editPersonaRel').value = '知己';
  $('#editPersonaQuote').value = '在黑夜与白日的接缝处，拥有相同的安静。';
};
$('#savePersonaBtn').onclick = () => {
  persona.name = $('#editPersonaName').value.trim() || '小熊';
  persona.avatar = $('#editPersonaAvatar').value.trim() || persona.avatar;
  persona.relationship = $('#editPersonaRel').value.trim() || '恋人';
  persona.statusQuote = $('#editPersonaQuote').value.trim() || persona.statusQuote;
  persona.temperature = parseFloat($('#editPersonaTemp').value) || 0.7;
  persona.systemPrompt = $('#editPersonaSys').value;
  save(K.PERSONA, persona);
  $('#personaModal').classList.remove('show');
  renderApp();
  toast('人设已保存');
};

// 用户设定保存
$('#saveUserBtn').onclick = () => {
  userProfile.name = $('#editUserName').value.trim() || '我';
  userProfile.notes = $('#editUserNotes').value.trim();
  userProfile.days = Number($('#editUserDays').value) || 1;
  save(K.USER, userProfile);
  $('#userModal').classList.remove('show');
  renderApp();
  toast('用户设定已保存');
};

// 头像更换 (支持相册 Base64 或 URL)
$('#saveAvatarBtn').onclick = () => {
  const target = $('#avatarTarget').value;
  const url = $('#avatarUrlInput').value.trim();
  const file = $('#avatarFileInput').files[0];
  const apply = src => {
    if(target === 'ai') persona.avatar = src;
    else userProfile.avatar = src;
    save(K.PERSONA, persona);
    save(K.USER, userProfile);
    $('#avatarModal').classList.remove('show');
    renderApp();
    toast('头像已更新');
  };
  if(file){
    const reader = new FileReader();
    reader.onload = () => apply(reader.result);
    reader.readAsDataURL(file);
  } else if(url){
    apply(url);
  }
};

// 壁纸设置
$('#saveWallBtn').onclick = () => {
  const url = $('#wallUrlInput').value.trim();
  const file = $('#wallFileInput').files[0];
  const apply = src => {
    wallpaper = src;
    save(K.WALL, wallpaper);
    $('#wallModal').classList.remove('show');
    renderApp();
    toast('壁纸已应用');
  };
  if(file){
    const reader = new FileReader();
    reader.onload = () => apply(reader.result);
    reader.readAsDataURL(file);
  } else if(url){
    apply(url);
  }
};
$('#clearWallBtn').onclick = () => {
  wallpaper = '';
  save(K.WALL, '');
  $('#wallModal').classList.remove('show');
  renderApp();
  toast('已清除壁纸');
};

// 时间书
$('#newBookPageBtn').onclick = () => $('#bookModal').classList.add('show');
$('#saveBookBtn').onclick = () => {
  const title = $('#bookTitle').value.trim();
  const content = $('#bookContent').value.trim();
  const mood = $('#bookMood').value.trim() || '静谧';
  const img = $('#bookImage').value.trim();
  if(!title || !content) return;
  const d = new Date();
  timeBook.unshift({ id: String(Date.now()), date: `${d.getFullYear()}.${String(d.getMonth()+1).padStart(2,'0')}.${String(d.getDate()).padStart(2,'0')}`, title, mood, content, img, isFav: false });
  save(K.BOOK, timeBook);
  $('#bookModal').classList.remove('show');
  renderTimeBook();
  toast('已装订入书');
};

// 动态发布
$('#newMomentBtn').onclick = () => $('#momentModal').classList.add('show');
$('#saveMomentBtn').onclick = () => {
  const text = $('#momentTextInput').value.trim();
  const img = $('#momentImgInput').value.trim();
  if(!text) return;
  moments.unshift({ id: String(Date.now()), author: userProfile.name, text, img, likes: 0, comments: [] });
  save(K.MOMENTS, moments);
  $('#momentModal').classList.remove('show');
  renderMoments();
  toast('动态已发布');
};

// 加好友
$('#newFriendBtn').onclick = () => $('#friendModal').classList.add('show');
$('#saveFriendBtn').onclick = () => {
  const name = $('#friendNameInput').value.trim();
  const note = $('#friendNoteInput').value.trim() || '新朋友';
  if(!name) return;
  friends.push({ id: String(Date.now()), name, note });
  save(K.FRIENDS, friends);
  $('#friendModal').classList.remove('show');
  renderFriends();
  toast(`已添加好友 ${name}`);
};

// 打卡
$('#checkinBtn').onclick = () => {
  userProfile.days++;
  userProfile.lastCheckin = new Date().toISOString().slice(0,10);
  save(K.USER, userProfile);
  renderApp();
  toast('今日打卡成功 ♡');
};

// 标点
$$('.quickBar button').forEach(b => {
  b.onclick = () => {
    $('#chatInput').value += b.dataset.ins;
    $('#chatInput').focus();
  };
});

$('#clearChatBtn').onclick = () => {
  chats = [];
  save(K.CHATS, chats);
  renderChats();
  toast('聊天记录已清空');
};

// 灯箱
window.openLightbox = (url, title) => {
  $('#lightboxImg').src = url;
  $('#lightboxTitle').textContent = title;
  $('#lightboxModal').classList.add('show');
};

// 时钟与电量
setInterval(()=>{
  const d = new Date();
  $('#sysClock').textContent = `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}`;
}, 1000);

if(navigator.getBattery){
  navigator.getBattery().then(b=>{
    const upd = () => $('#batteryPct').textContent = `${Math.round(b.level*100)}%`;
    upd();
    b.addEventListener('levelchange', upd);
  }).catch(()=>{});
}

// 初始化
setupWeekStrip();
renderApp();
renderChats();
})();
</script>
</body>
</html>
