<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#f5f5f2">
<title>airp chat</title>
<style>
:root{
  --bg:#f3f3f0;--screen:#f6f6f3;--glass:rgba(255,255,255,.74);--glass2:rgba(255,255,255,.52);
  --line:rgba(45,45,48,.12);--ink:#292a2d;--muted:#85868b;--mine:#dfdfdb;--other:#ffffff;
  --panel:rgba(248,248,245,.96);--panelSolid:#f8f8f5;--shadow:0 18px 48px rgba(40,40,40,.13);
  --wallpaper:none;--accent:#4a4b4f;--danger:#8a7474;
}
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden}
body{font-family:Inter,"PingFang SC","Microsoft YaHei",system-ui,sans-serif;color:var(--ink);background:var(--screen)}
body:before{content:"";position:fixed;inset:0;pointer-events:none;opacity:.14;background-image:radial-gradient(rgba(80,80,80,.18) .5px,transparent .7px);background-size:6px 6px;z-index:0}
button,input,textarea,select{font:inherit}button{border:0;background:none;color:inherit;cursor:pointer}button:active{transform:scale(.97)}
#screen{position:relative;width:100vw;height:100dvh;min-height:100%;overflow:hidden;background-image:var(--wallpaper),radial-gradient(circle at 82% 10%,rgba(255,255,255,.72),transparent 28%),linear-gradient(145deg,#fafaf8,#ededeb 72%,#e3e3e0);background-size:cover;background-position:center}
#screen:before{content:"";position:absolute;inset:0;pointer-events:none;background:linear-gradient(125deg,rgba(255,255,255,.42),transparent 44%),radial-gradient(circle at 18% 82%,rgba(255,255,255,.26),transparent 32%);z-index:0}
.status{height:30px;display:flex;align-items:center;justify-content:space-between;padding:8px 18px 0;font-size:10px;letter-spacing:.05em;position:relative;z-index:8}.statusRight{display:flex;gap:8px;align-items:center}.battery{width:22px;height:9px;border:1px solid currentColor;border-radius:3px;padding:1px}.battery i{display:block;height:100%;width:74%;background:currentColor;border-radius:1px}.battery:after{content:"";display:block;width:2px;height:3px;background:currentColor;position:absolute;margin-left:21px;margin-top:-6px;border-radius:1px}
.view{position:absolute;inset:30px 0 0;display:none;flex-direction:column;z-index:2}.view.active{display:flex}
.chatHead,.subHead{height:57px;display:flex;align-items:center;gap:9px;padding:5px 13px;border-bottom:1px solid var(--line);background:rgba(247,247,244,.69);backdrop-filter:blur(18px);position:relative;z-index:4}.headInfo{flex:1;min-width:0;text-align:left}.headInfo b{font-size:13px;font-weight:650}.headInfo small{display:block;color:var(--muted);font-size:8px;margin-top:2px}.dots{font-size:18px;letter-spacing:2px}.relTag{font-size:8px;padding:3px 6px;border:1px solid var(--line);border-radius:99px;color:var(--muted)}
.avatar{width:34px;height:34px;flex:0 0 34px;border-radius:12px;object-fit:cover;background:#d9d9d6;border:1px solid var(--line)}
.messages{flex:1;overflow:auto;padding:14px 12px 8px;scrollbar-width:none;overscroll-behavior:contain}.messages::-webkit-scrollbar,.moments::-webkit-scrollbar,.friendsList::-webkit-scrollbar,.logs::-webkit-scrollbar{display:none}
.messageRow{display:flex;gap:7px;margin:8px 0;animation:pop .18s ease both}.messageRow.mine{justify-content:flex-end}.messageRow.mine .bubbleWrap{align-items:flex-end}.messageRow.mine .avatar{order:2}.bubbleWrap{max-width:min(80%,420px);display:flex;flex-direction:column;align-items:flex-start}.bubble{border:1px solid var(--line);padding:9px 11px;border-radius:17px;background:var(--other);font-size:12px;line-height:1.6;box-shadow:0 4px 14px rgba(0,0,0,.035);word-break:break-word;white-space:pre-wrap}.mine .bubble{background:var(--mine);border-top-right-radius:5px}.other .bubble{border-top-left-radius:5px}.time{font-size:7px;color:var(--muted);margin:3px 4px}.systemMsg{text-align:center;font-size:8px;color:var(--muted);margin:10px 0}.typingBubble{color:var(--muted);font-style:italic}.streamingCursor{display:inline-block;width:1px;height:12px;background:currentColor;vertical-align:-2px;margin-left:2px;animation:blink 1s steps(2,start) infinite}
.quoteBox{border-left:2px solid #999;padding:5px 7px;margin-bottom:6px;color:#777;background:rgba(0,0,0,.035);border-radius:7px;font-size:9px}.voice{min-width:110px;display:flex;align-items:center;gap:8px}.voiceBars{display:flex;gap:2px;align-items:center;height:18px}.voiceBars i{width:2px;height:7px;background:#777;border-radius:2px}.voiceBars.play i{animation:wave .6s infinite alternate}.sticker{max-width:110px;max-height:110px;border-radius:14px;object-fit:contain}.redpacket,.transfer{width:190px;padding:13px;background:#efefeb;border:1px solid var(--line);border-radius:16px;text-align:left}.rpTitle,.trTitle{font-size:11px;font-weight:650}.rpSub,.trSub{font-size:8px;color:#777;margin-top:5px}.linkCard{width:205px;padding:10px;display:flex;gap:8px;background:#f8f8f5;border:1px solid var(--line);border-radius:13px;text-align:left}.linkCard img{width:50px;height:50px;border-radius:9px;object-fit:cover;background:#ddd}.linkCard b{font-size:10px;display:block}.linkCard small{font-size:8px;color:#888}.relCard{padding:11px;background:#f8f8f5;border:1px solid var(--line);border-radius:14px}.relCard b{font-size:11px}.relBtns{display:flex;gap:6px;margin-top:9px}.miniBtn{flex:1;border:1px solid var(--line);border-radius:10px;padding:6px;font-size:9px;background:#fff}.miniBtn.primary{background:#333;color:#fff}
.quick{display:flex;gap:6px;overflow:auto;padding:5px 10px;scrollbar-width:none}.quick button{white-space:nowrap;border:1px solid var(--line);background:rgba(255,255,255,.62);border-radius:99px;padding:5px 8px;font-size:9px}.composer{padding:7px 9px max(10px,env(safe-area-inset-bottom));display:flex;gap:7px;align-items:flex-end;background:rgba(247,247,244,.77);backdrop-filter:blur(18px);border-top:1px solid var(--line)}.tool{width:30px;height:30px;flex:0 0 30px;border:1px solid var(--line);border-radius:10px;background:rgba(255,255,255,.64);display:grid;place-items:center;font-size:12px}.input{flex:1;min-width:0;border:1px solid var(--line);background:rgba(255,255,255,.78);border-radius:14px;min-height:31px;max-height:120px;padding:7px 9px;font-size:11px;line-height:1.45;resize:none;outline:none;overflow:auto}.send{padding:8px 12px;border-radius:12px;background:#333;color:#fff;font-size:10px;min-width:48px;height:31px}.send[disabled]{opacity:.45;cursor:not-allowed}
.subHead{justify-content:space-between}.subHead b{font-size:13px}.subBody{flex:1;overflow:auto;padding:13px}.moments,.friendsList,.logs{flex:1;overflow:auto;padding:12px}.moment{padding:12px;border:1px solid var(--line);background:rgba(255,255,255,.6);border-radius:19px;margin-bottom:10px}.momentHead{display:flex;gap:8px;align-items:center}.moment b{font-size:10px}.moment small{display:block;color:#999;font-size:8px}.moment p{font-size:10px;line-height:1.6;white-space:pre-wrap}.momentPics{display:grid;grid-template-columns:repeat(3,1fr);gap:5px}.momentPics img{width:100%;aspect-ratio:1;object-fit:cover;border-radius:9px}.momentActions{display:flex;gap:12px;margin-top:8px;font-size:9px;color:#777}.friend{display:flex;align-items:center;gap:10px;padding:11px;border-bottom:1px solid var(--line)}.friend .grow{flex:1}.friend b{font-size:11px}.friend small{display:block;color:#888;font-size:8px;margin-top:3px}.log{padding:10px 11px;border-left:2px solid #999;background:rgba(255,255,255,.48);border-radius:0 12px 12px 0;margin-bottom:7px;font-size:9px}.log b{font-size:9px}.bottomNav{height:43px;display:flex;border-top:1px solid var(--line);background:rgba(247,247,244,.72);backdrop-filter:blur(16px)}.navBtn{flex:1;font-size:8px;color:#777}.navBtn.active{color:#222;font-weight:650}
.overlay{position:absolute;inset:0;background:rgba(20,20,20,.18);backdrop-filter:blur(7px);z-index:20;display:none;align-items:flex-end;padding:12px}.overlay.show{display:flex}.modal{width:100%;max-height:86%;overflow:auto;background:var(--panel);border:1px solid rgba(255,255,255,.8);border-radius:25px;box-shadow:0 20px 55px rgba(0,0,0,.2);padding:16px;animation:up .2s ease}.modal.center{align-self:center}.modal h3{margin:0 0 12px;font-size:13px}.field{margin:9px 0}.field label{display:block;font-size:8px;color:#888;margin:0 0 5px}.field input,.field textarea,.field select{width:100%;border:1px solid var(--line);background:#fff;border-radius:12px;padding:8px;font-size:10px;outline:none;color:inherit}.field textarea{min-height:72px;resize:vertical}.modalBtns{display:flex;gap:7px;margin-top:13px}.modalBtns button{flex:1;padding:9px;border-radius:12px;border:1px solid var(--line);font-size:10px;background:#fff}.modalBtns .primary{background:#333;color:#fff}.closex{float:right;font-size:18px;color:#777}.menuGrid{display:grid;grid-template-columns:repeat(3,1fr);gap:7px}.menuGrid button{padding:10px 5px;border:1px solid var(--line);border-radius:13px;background:#fff;font-size:9px}.emojiGrid{display:grid;grid-template-columns:repeat(4,1fr);gap:6px}.emo{min-height:34px;border:1px solid var(--line);border-radius:9px;background:#fff;font-size:9px}.toast{position:absolute;left:50%;bottom:74px;transform:translate(-50%,15px);z-index:50;background:rgba(40,40,40,.88);color:#fff;padding:7px 11px;border-radius:99px;font-size:9px;opacity:0;pointer-events:none;transition:.25s;white-space:nowrap;max-width:86%;overflow:hidden;text-overflow:ellipsis}.toast.show{opacity:1;transform:translate(-50%,0)}
.turntable{border:1px solid var(--line);border-radius:20px;background:var(--glass2);padding:12px;display:flex;align-items:center;gap:12px}.disc{width:64px;height:64px;border-radius:50%;flex:0 0 64px;background:radial-gradient(circle,#e9e9e7 0 7px,#777 8px 9px,#29292b 10px 26px,#b7b7b5 27px 28px,#353537 29px 32px,#d6d6d2 33px);box-shadow:0 6px 12px rgba(0,0,0,.18);animation:spin 5s linear infinite;animation-play-state:paused}.disc.play{animation-play-state:running}.turnInfo{min-width:0}.turnInfo b{font-size:11px}.turnInfo small{display:block;color:var(--muted);font-size:8px;margin-top:4px}.wave{display:flex;gap:2px;height:16px;align-items:center;margin-top:6px}.wave i{width:2px;height:5px;background:#777;border-radius:2px}.wave.play i{animation:wave .7s ease-in-out infinite alternate}.wave i:nth-child(2){animation-delay:.08s}.wave i:nth-child(3){animation-delay:.16s}.wave i:nth-child(4){animation-delay:.24s}.wave i:nth-child(5){animation-delay:.32s}.musicRow{display:flex;align-items:center;gap:8px;padding:8px 0;border-bottom:1px solid var(--line)}.musicRow .grow{flex:1}.musicRow b{font-size:9px}.musicRow small{display:block;font-size:7px;color:#888}.playerBtns{display:flex;justify-content:center;gap:12px;margin:9px}.playerBtns button{width:34px;height:34px;border:1px solid var(--line);border-radius:50%;background:#fff}.range{width:100%}
.apiStatus{font-size:8px;color:#777;margin-top:7px;min-height:12px}.statusDot{display:inline-block;width:6px;height:6px;border-radius:50%;background:#777;margin-right:4px}
.night{--bg:#121315;--screen:#17181a;--glass:rgba(30,31,34,.78);--glass2:rgba(35,36,39,.64);--line:rgba(255,255,255,.1);--ink:#ecece9;--muted:#999aa0;--mine:#303134;--other:#232427;--panel:rgba(28,29,31,.97);--panelSolid:#222326}.night body{background:#17181a}.night #screen{background-image:var(--wallpaper),radial-gradient(circle at 80% 10%,rgba(255,255,255,.05),transparent 28%),linear-gradient(145deg,#1d1e20,#131416)}.night .chatHead,.night .subHead,.night .composer,.night .bottomNav{background:rgba(23,24,26,.74)}.night .field input,.night .field textarea,.night .field select,.night .miniBtn,.night .menuGrid button,.night .emo,.night .modalBtns button,.night .playerBtns button,.night .linkCard,.night .redpacket,.night .transfer,.night .relCard{background:#232427;color:#eee}
@keyframes pop{from{opacity:0;transform:translateY(6px) scale(.985)}to{opacity:1;transform:none}}@keyframes up{from{opacity:0;transform:translateY(15px)}to{opacity:1;transform:none}}@keyframes spin{to{transform:rotate(360deg)}}@keyframes wave{to{height:15px}}@keyframes blink{50%{opacity:0}}
</style>
</head>
<body>
<div id="screen">
  <div class="status"><span id="clock">00:00</span><span class="statusRight"><span id="signal">AIRP</span><span id="batteryText">100%</span><span class="battery"><i id="batteryBar"></i></span></span></div>

  <section class="view active" id="chatView">
    <header class="chatHead">
      <button id="backHome" aria-label="页面菜单">‹</button>
      <button id="avatarBtn"><img class="avatar" id="peerAvatar" alt=""></button>
      <button class="headInfo" id="nameBtn"><b id="peerName">小熊</b><small id="peerSub">AI 对话 · 等待你的第一句话</small></button>
      <span id="relTag" class="relTag" style="display:none"></span>
      <button class="dots" id="menuBtn" aria-label="更多">···</button>
    </header>
    <div class="messages" id="messages"></div>
    <div class="quick">
      <button data-ins="，">，</button><button data-ins="。">。</button><button data-ins="？">？</button><button data-ins="！">！</button><button data-ins="……">……</button><button data-ins="「」">「」</button><button data-ins="（ ）">（ ）</button><button data-ins="【 】">【 】</button>
    </div>
    <div class="composer">
      <button class="tool" id="emojiBtn" aria-label="表情">＋</button>
      <textarea class="input" id="input" rows="1" placeholder="写点什么……"></textarea>
      <button class="tool" id="musicBtn" aria-label="音乐">♪</button>
      <button class="send" id="sendBtn">发送</button>
    </div>
  </section>

  <section class="view" id="momentsView"><header class="subHead"><button data-go="chat">‹</button><b>朋友圈</b><button id="postMomentBtn">＋</button></header><div class="moments" id="moments"></div><div class="bottomNav"><button class="navBtn" data-go="chat">聊天</button><button class="navBtn active">朋友圈</button><button class="navBtn" data-go="friends">好友</button></div></section>
  <section class="view" id="friendsView"><header class="subHead"><button data-go="chat">‹</button><b>好友</b><button id="addFriendBtn">＋</button></header><div class="friendsList" id="friends"></div><div class="bottomNav"><button class="navBtn active" data-go="chat">聊天</button><button class="navBtn" data-go="moments">朋友圈</button><button class="navBtn active">好友</button></div></section>
  <section class="view" id="logsView"><header class="subHead"><button data-go="chat">‹</button><b>查岗 / 后台记录</b><button id="refreshLogs">↻</button></header><div class="logs" id="logs"></div></section>

  <div class="overlay" id="overlay"><div class="modal" id="modal"></div></div>
  <div class="toast" id="toast"></div>
  <audio id="audio"></audio>
</div>

<script type="text/template" id="chatTemplate">
[]
</script>
<script type="text/template" id="logsTemplate">
[]
</script>
<script type="text/template" id="momentsTemplate">
[]
</script>

<script>
(()=>{
'use strict';
const $=s=>document.querySelector(s), $$=s=>[...document.querySelectorAll(s)];
const STORE='airp_chat_v3', API_STORE='airp_chat_api_v3';
const DEFAULTS={peerName:'小熊',peerAvatar:'',myAvatar:'',wallpaper:'',theme:'light',blocked:false,relation:'',relationSpace:{days:0,last:'',moods:[],diary:[]},moments:[],openedPackets:[],messages:[]};
let state={...DEFAULTS,...safeParse(localStorage.getItem(STORE),{})};
state.relationSpace={...DEFAULTS.relationSpace,...(state.relationSpace||{})};
let api={endpoint:'https://api.openai.com/v1/chat/completions',key:'',model:'gpt-4o-mini',system:'你正在参与 airp 的实时聊天。你是聊天对象本人，不要解释自己是模型，不要一次输出大段内容，保持自然、简短、符合上下文的聊天口吻。'};
api={...api,...safeParse(localStorage.getItem(API_STORE),{})};
let messages=[];

function safeParse(v,f){try{return v?JSON.parse(v):f}catch(_){return f}}
function save(){localStorage.setItem(STORE,JSON.stringify(state))}
function saveApi(){localStorage.setItem(API_STORE,JSON.stringify(api))}
function esc(v=''){return String(v).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function nowTime(){return new Date().toLocaleTimeString('zh-CN',{hour:'2-digit',minute:'2-digit'})}
function toast(t){const e=$('#toast');e.textContent=t;e.classList.add('show');clearTimeout(toast.t);toast.t=setTimeout(()=>e.classList.remove('show'),1800)}
function openModal(html,center=false){$('#modal').innerHTML=html;$('#modal').classList.toggle('center',center);$('#overlay').classList.add('show')}
function closeModal(){$('#overlay').classList.remove('show')}
$('#overlay').addEventListener('click',e=>{if(e.target===$('#overlay'))closeModal()});

function avatarSvg(t){return 'data:image/svg+xml;charset=UTF-8,'+encodeURIComponent(`<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><rect width="100" height="100" rx="28" fill="#d9d9d5"/><circle cx="50" cy="43" r="20" fill="#f5f5f1"/><path d="M25 82c7-20 43-20 50 0" fill="#f5f5f1"/><text x="50" y="92" text-anchor="middle" font-size="9" fill="#777">${esc(t).slice(0,2)}</text></svg>`)}
function apply(){
  document.documentElement.classList.toggle('night',state.theme==='dark');
  document.documentElement.style.setProperty('--wallpaper',state.wallpaper?`url("${state.wallpaper.replace(/"/g,'&quot;')}")`:'none');
  $('#peerName').textContent=state.peerName+(state.blocked?' 已拉黑':'');
  $('#peerAvatar').src=state.peerAvatar||avatarSvg(state.peerName);$('#peerAvatar').alt=state.peerName;
  $('#relTag').style.display=state.relation?'inline-block':'none';$('#relTag').textContent=state.relation||'';
  $('#sendBtn').disabled=false;
}

function parseTemplate(id){try{return JSON.parse($('#'+id).textContent.trim())}catch(_){return[]}}
function loadMessages(){
  const saved=Array.isArray(state.messages)?state.messages:[];
  messages=saved.length?saved.slice():parseTemplate('chatTemplate');
}
function persistMessages(){state.messages=messages.slice(-200);save()}
function addMessage(m){messages.push(m);persistMessages();return m}

function renderMessage(m,index){
  const el=document.createElement('div');
  if(m.side==='system'){el.className='systemMsg';el.textContent=m.text||'';return el}
  el.className='messageRow '+(m.side==='me'?'mine':'other');
  const av=document.createElement('img');av.className='avatar';av.src=m.side==='me'?(state.myAvatar||avatarSvg('我')):(state.peerAvatar||avatarSvg(state.peerName));
  const wrap=document.createElement('div');wrap.className='bubbleWrap';
  let body='';
  if(m.type==='text')body=`<div class="bubble">${esc(m.text)}</div>`;
  else if(m.type==='quote')body=`<div class="bubble"><div class="quoteBox">${esc(m.quote||'')}</div>${esc(m.text||'')}</div>`;
  else if(m.type==='voice')body=`<button class="bubble voice" data-voice="${m.id}"><span>${esc(m.duration||0)}s</span><span class="voiceBars"><i></i><i></i><i></i><i></i><i></i></span><span>转文字</span></button><div class="time" data-voice-text="${m.id}" style="display:none">${esc(m.text||'')}</div>`;
  else if(m.type==='sticker')body=m.url?`<div class="bubble"><img class="sticker" src="${esc(m.url)}"></div>`:`<div class="bubble">${esc(m.text||'')}</div>`;
  else if(m.type==='redpacket')body=`<button class="redpacket" data-rp="${m.id}"><div class="rpTitle">${esc(m.title||'红包')}</div><div class="rpSub">${state.openedPackets.includes(m.id)?'已拆开':'点击拆开'}</div></button>`;
  else if(m.type==='transfer')body=`<button class="transfer" data-tr="${m.id}"><div class="trTitle">转账 ${esc(m.amount||'0.00')}</div><div class="trSub">${m.status==='paid'?'已收款':'待收款 · 点击处理'}</div></button>`;
  else if(m.type==='link')body=`<div class="linkCard"><img src="${m.thumb?esc(m.thumb):avatarSvg('链')}"><div><b>${esc(m.title||'链接分享')}</b><small>${esc(m.desc||'')}</small></div></div>`;
  else if(m.type==='relation')body=`<div class="relCard"><b>关系申请 · ${esc(m.relation||'朋友')}</b><div style="font-size:9px;margin-top:5px;color:#777">${esc(m.text||'')}</div><div class="relBtns"><button class="miniBtn" data-rel="no">拒绝</button><button class="miniBtn primary" data-rel="yes">同意</button></div></div>`;
  else if(m.type==='stream')body=`<div class="bubble">${esc(m.text||'')}<span class="streamingCursor"></span></div>`;
  else body=`<div class="bubble">${esc(m.text||'')}</div>`;
  wrap.innerHTML=body+`<div class="time">${esc(m.time||'')}</div>`;el.append(av,wrap);
  el.addEventListener('contextmenu',e=>{e.preventDefault();quoteMessage(m)});
  let timer=0;el.addEventListener('pointerdown',()=>{timer=setTimeout(()=>quoteMessage(m),620)});['pointerup','pointercancel','pointerleave'].forEach(x=>el.addEventListener(x,()=>clearTimeout(timer)));
  return el;
}
function renderChat(scroll=true){const box=$('#messages');box.innerHTML='';messages.forEach((m,i)=>box.appendChild(renderMessage(m,i)));if(scroll)requestAnimationFrame(()=>box.scrollTop=box.scrollHeight)}
function quoteMessage(m){if(m.side==='system')return;$('#input').value=`引用：${m.text||m.title||m.amount||'[消息]'}`;resizeInput();$('#input').focus();toast('已引用这条消息')}

async function sendMessage(){
  const text=$('#input').value.trim();if(!text||state.blocked)return;
  if(state.blocked){toast('当前联系人已拉黑');return}
  $('#input').value='';resizeInput();
  const user={id:'u_'+Date.now(),side:'me',type:'text',role:'user',time:nowTime(),text};addMessage(user);renderChat();
  $('#sendBtn').disabled=true;
  const pendingId='a_'+Date.now();
  const pending={id:pendingId,side:'other',type:'stream',role:'assistant',time:nowTime(),text:''};messages.push(pending);renderChat();
  const row=[...$('#messages').children].at(-1);const bubble=row?.querySelector('.bubble');
  try{
    const result=await streamToApi();
    if(!result.text){throw new Error('API 没有返回可显示的文本')}
    pending.text=result.text;pending.type='text';pending.streaming=false;persistMessages();
    renderChat();
  }catch(err){
    pending.text='请求失败：'+(err.message||'未知错误');pending.type='text';persistMessages();renderChat();toast('API 请求失败');
  }finally{$('#sendBtn').disabled=false;}
}

function chatForApi(){
  return messages.filter(m=>m.side!=='system'&&m.type!=='redpacket'&&m.type!=='transfer'&&m.type!=='relation').map(m=>({role:m.role||(m.side==='me'?'user':'assistant'),content:m.text||m.title||''})).slice(-30)
}
function headers(){const h={'Content-Type':'application/json'};if(api.key)h.Authorization='Bearer '+api.key;return h}
async function streamToApi(){
  if(!api.endpoint)throw new Error('未设置 API Endpoint');
  const payload={model:api.model||'gpt-4o-mini',messages:[{role:'system',content:api.system||''},...chatForApi()],stream:true};
  const res=await fetch(api.endpoint,{method:'POST',headers:headers(),body:JSON.stringify(payload)});
  if(!res.ok){let msg='HTTP '+res.status;try{const j=await res.json();msg=j?.error?.message||j?.message||msg}catch(_){}throw new Error(msg)}
  if(!res.body) return {text:await res.text()};
  const reader=res.body.getReader(),decoder=new TextDecoder('utf-8');let buffer='',full='';
  const update=(chunk)=>{full+=chunk;const p=messages[messages.length-1];if(p&&p.type==='stream'){p.text=full;const last=[...$('#messages').children].at(-1);const b=last?.querySelector('.bubble');if(b)b.innerHTML=esc(full)+'<span class="streamingCursor"></span>';$('#messages').scrollTop=$('#messages').scrollHeight;}};
  while(true){const {value,done}=await reader.read();if(done)break;buffer+=decoder.decode(value,{stream:true});const parts=buffer.split(/\n\n|\r\n\r\n/);buffer=parts.pop()||'';for(const block of parts){for(const line of block.split(/\r?\n/)){const s=line.trim();if(!s.startsWith('data:'))continue;const data=s.slice(5).trim();if(data==='[DONE]')continue;try{const j=JSON.parse(data);const delta=j?.choices?.[0]?.delta?.content??j?.choices?.[0]?.message?.content??j?.content??'';if(delta)update(delta)}catch(_){}}}}
  return {text:full};
}

$('#sendBtn').addEventListener('click',sendMessage);
$('#input').addEventListener('keydown',e=>{if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendMessage()}});
$('#input').addEventListener('input',resizeInput);function resizeInput(){const e=$('#input');e.style.height='auto';e.style.height=Math.min(120,Math.max(31,e.scrollHeight))+'px'}
$$('[data-ins]').forEach(b=>b.addEventListener('click',()=>{$('#input').value+=b.dataset.ins;resizeInput();$('#input').focus();toast('已注入快捷标点')}));

$('#emojiBtn').onclick=()=>{openModal(`<button class="closex" data-close>×</button><h3>表情指令</h3><div class="field"><input id="emoSearch" placeholder="搜索指令，例如 开心"></div><div class="emojiGrid" id="emoGrid"></div>`);renderEmos();};
function renderEmos(){const list=['开心','委屈','抱抱','想你','晚安','好困','无语','谢谢','生气','心碎','猫猫','小熊','贴贴','害羞','发呆','叹气','微笑','哭泣','加油','期待'];const q=($('#emoSearch')?.value||'').trim();$('#emoGrid').innerHTML=list.filter(x=>!q||x.includes(q)).map(x=>`<button class="emo" data-emo="${x}">[${x}]</button>`).join('');$$('[data-emo]').forEach(b=>b.onclick=()=>{$('#input').value+='['+b.dataset.emo+']';resizeInput();closeModal();$('#input').focus();toast('已填入表情指令')});if($('#emoSearch'))$('#emoSearch').oninput=renderEmos}

$('#menuBtn').onclick=menuModal;$('#backHome').onclick=menuModal;
function menuModal(){openModal(`<button class="closex" data-close>×</button><h3>airp</h3><div class="menuGrid"><button data-menu="api">API 设置</button><button data-menu="logs">查岗记录</button><button data-menu="moments">朋友圈</button><button data-menu="friends">好友列表</button><button data-menu="relation">建立关系</button><button data-menu="space">关系空间</button><button data-menu="avatar">更换头像</button><button data-menu="wallpaper">更换壁纸</button><button data-menu="theme">白间 / 夜间</button><button data-menu="block">${state.blocked?'解除拉黑':'拉黑'}</button><button data-menu="clear">清空聊天</button><button data-menu="reset">重置本地数据</button></div><div style="font-size:8px;color:#888;margin-top:12px">当前接口：${esc(api.endpoint||'未设置')}</div>`)}
$('#modal').addEventListener('click',e=>{
  if(e.target.closest('[data-close]'))return closeModal();
  const item=e.target.closest('[data-menu]');if(item){closeModal();const m=item.dataset.menu;({api:apiModal,logs:()=>go('logs'),moments:()=>go('moments'),friends:()=>go('friends'),relation:relationModal,space:spaceModal,avatar:avatarModal,wallpaper:wallpaperModal,theme:toggleTheme,block:blockModal,clear:clearChat,reset:resetAll}[m]||(()=>{}))()}
  if(e.target.closest('[data-open-rp]')){const id=e.target.closest('[data-open-rp]').dataset.openRp;if(!state.openedPackets.includes(id))state.openedPackets.push(id);save();closeModal();renderChat();toast('红包已拆开')}
  if(e.target.closest('[data-pay]')){const id=e.target.closest('[data-pay]').dataset.pay;const msg=messages.find(x=>String(x.id)===String(id));if(msg)msg.status='paid';persistMessages();closeModal();renderChat();toast('已收款')}
  if(e.target.closest('[data-rel]')){const yes=e.target.closest('[data-rel]').dataset.rel==='yes';const req=[...messages].reverse().find(x=>x.type==='relation');if(yes&&req){state.relation=req.relation||'朋友';save();apply();addMessage({id:'s_'+Date.now(),side:'system',type:'text',time:nowTime(),text:'你们已经建立 '+state.relation+' 关系。'});toast('关系已建立')}else toast('已拒绝关系申请');closeModal();renderChat()}
  if(e.target.closest('[data-voice]')){const id=e.target.closest('[data-voice]').dataset.voice;const tx=$(`[data-voice-text="${CSS.escape(id)}"]`);if(tx)tx.style.display=tx.style.display==='none'?'block':'none'}
});
$('#nameBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>修改备注</h3><div class="field"><label>备注名</label><input id="remark" value="${esc(state.peerName)}"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveRemark">保存</button></div>`);
$('#modal').addEventListener('click',e=>{if(e.target.id==='saveRemark'){state.peerName=$('#remark').value.trim()||'小熊';save();apply();closeModal();toast('备注已保存')}});

$('#avatarBtn').onclick=avatarModal;
function avatarModal(){openModal(`<button class="closex" data-close>×</button><h3>头像设置</h3><div class="field"><label>对象</label><select id="avatarWho"><option value="peer">对方头像</option><option value="me">我的头像</option></select></div><div class="field"><label>图片 URL</label><input id="avatarUrl" placeholder="https://..."></div><div class="field"><label>本地图片</label><input id="avatarFile" type="file" accept="image/*"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveAvatar">保存</button></div>`);$('#modal').addEventListener('click',saveAvatar,{once:true})}
function saveAvatar(e){if(e.target.id!=='saveAvatar')return;const who=$('#avatarWho').value,url=$('#avatarUrl').value.trim(),file=$('#avatarFile').files[0];const done=data=>{if(who==='peer')state.peerAvatar=data;else state.myAvatar=data;save();apply();closeModal();renderChat();toast('头像已保存')};if(file){const r=new FileReader();r.onload=()=>done(r.result);r.readAsDataURL(file)}else done(url||'')}

function wallpaperModal(){openModal(`<button class="closex" data-close>×</button><h3>聊天壁纸</h3><div class="field"><label>图片 URL</label><input id="wallUrl" value="${esc(state.wallpaper)}" placeholder="https://..."></div><div class="field"><label>本地图片</label><input id="wallFile" type="file" accept="image/*"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveWall">保存</button></div>`);$('#modal').addEventListener('click',saveWall,{once:true})}
function saveWall(e){if(e.target.id!=='saveWall')return;const url=$('#wallUrl').value.trim(),file=$('#wallFile').files[0];const done=data=>{state.wallpaper=data;save();apply();closeModal();toast('壁纸已更新')};if(file){const r=new FileReader();r.onload=()=>done(r.result);r.readAsDataURL(file)}else done(url)}

function apiModal(){openModal(`<button class="closex" data-close>×</button><h3>API 设置</h3><div class="field"><label>Endpoint</label><input id="apiEndpoint" value="${esc(api.endpoint||'')}"></div><div class="field"><label>API Key</label><input id="apiKey" type="password" value="${esc(api.key||'')}"></div><div class="field"><label>Model</label><input id="apiModel" value="${esc(api.model||'')}"></div><div class="field"><label>System Prompt</label><textarea id="apiSystem">${esc(api.system||'')}</textarea></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveApi">保存</button></div><div class="apiStatus" id="apiStatus">保存后发送消息即可实时请求接口。</div>`)}
function saveApiSettings(){api={endpoint:$('#apiEndpoint').value.trim(),key:$('#apiKey').value.trim(),model:$('#apiModel').value.trim(),system:$('#apiSystem').value};saveApi();closeModal();toast('API 配置已保存')}
$('#modal').addEventListener('click',e=>{if(e.target.id==='saveApi')saveApiSettings()});

function toggleTheme(){state.theme=state.theme==='light'?'dark':'light';save();apply();toast(state.theme==='dark'?'已切换夜间':'已切换白间')}
function clearChat(){openModal(`<button class="closex" data-close>×</button><h3>清空聊天</h3><p style="font-size:10px;color:#777">会清除当前聊天记录，本地配置不会被删除。</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="reallyClear">确认清空</button></div>`,true);$('#modal').addEventListener('click',e=>{if(e.target.id==='reallyClear'){messages=[];state.messages=[];save();closeModal();renderChat();toast('聊天记录已清空')}})}
function resetAll(){openModal(`<button class="closex" data-close>×</button><h3>重置本地数据</h3><p style="font-size:10px;color:#777">头像、壁纸、关系、日记、API 配置和聊天记录都会恢复默认。</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="reallyReset">确认重置</button></div>`,true);$('#modal').addEventListener('click',e=>{if(e.target.id==='reallyReset'){localStorage.removeItem(STORE);localStorage.removeItem(API_STORE);location.reload()}})}
function blockModal(){openModal(`<button class="closex" data-close>×</button><h3>${state.blocked?'解除拉黑':'拉黑联系人'}</h3><p style="font-size:10px;color:#777">${state.blocked?'解除后可以继续向 API 发送消息。':'拉黑后发送按钮会停止请求，历史记录保留。'}</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="confirmBlock">确认</button></div>`,true);$('#modal').addEventListener('click',e=>{if(e.target.id==='confirmBlock'){state.blocked=!state.blocked;save();apply();closeModal();addMessage({id:'s_'+Date.now(),side:'system',type:'text',time:nowTime(),text:state.blocked?'你已将对方拉黑':'你已解除拉黑'});renderChat();toast(state.blocked?'已拉黑':'已解除拉黑')}})}

function relationModal(){openModal(`<button class="closex" data-close>×</button><h3>建立关系</h3><div class="menuGrid">${['恋人','朋友','基友','闺蜜','家人'].map(x=>`<button data-relpick="${x}">${x}</button>`).join('')}</div><p style="font-size:8px;color:#888">选择后只发送一条本地申请消息，不会自动替你完成关系。</p>`);$('#modal').addEventListener('click',e=>{const r=e.target.closest('[data-relpick]')?.dataset.relpick;if(r){addMessage({id:'u_'+Date.now(),side:'me',type:'text',role:'user',time:nowTime(),text:'[关系申请] '+r});addMessage({id:'r_'+(Date.now()+1),side:'other',type:'relation',role:'assistant',time:nowTime(),relation:r,text:'想和你建立 '+r+' 关系。'});closeModal();renderChat();toast('关系申请已发送')}})}
function spaceModal(){if(!state.relation){toast('请先建立关系');return}openModal(`<button class="closex" data-close>×</button><h3>关系空间 · ${esc(state.relation)}</h3><div class="turntable"><div><b style="font-size:11px">连续打卡 ${state.relationSpace.days||0} 天</b><div style="font-size:9px;color:#888;margin-top:5px">今天：${esc(randomMood())}</div></div></div><div class="modalBtns"><button id="checkin">今日打卡</button><button id="writeDiary">写日记</button></div><div class="modalBtns"><button id="clearRelation">解除关系并清空空间</button></div><div style="margin-top:10px">${(state.relationSpace.diary||[]).map((d,i)=>`<div class="log"><b>${esc(d.date)}</b><div>${esc(d.text)}</div><button class="miniBtn" data-del-diary="${i}">删除</button></div>`).join('')}</div>`);$('#modal').addEventListener('click',e=>{if(e.target.id==='checkin'){const d=new Date().toISOString().slice(0,10);if(state.relationSpace.last!==d){state.relationSpace.days=(state.relationSpace.days||0)+1;state.relationSpace.last=d;save();toast('打卡成功')}else toast('今天已经打过卡了');spaceModal()}if(e.target.id==='writeDiary')diaryModal();if(e.target.id==='clearRelation'){state.relation='';state.relationSpace={days:0,last:'',moods:[],diary:[]};save();apply();closeModal();toast('关系已解除，空间已清空')}const d=e.target.closest('[data-del-diary]');if(d){state.relationSpace.diary.splice(+d.dataset.delDiary,1);save();spaceModal();toast('日记已删除')}})}
function randomMood(){return['安静','想念','疲惫','晴朗','慢慢好起来'][new Date().getDate()%5]}
function diaryModal(){openModal(`<button class="closex" data-close>×</button><h3>写日记</h3><div class="field"><textarea id="diaryText" placeholder="留一点字给未来的自己……"></textarea></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveDiary">发布</button></div>`);$('#modal').addEventListener('click',e=>{if(e.target.id==='saveDiary'){const t=$('#diaryText').value.trim();if(!t)return toast('还没有写内容');state.relationSpace.diary.unshift({date:new Date().toLocaleString('zh-CN'),text:t});save();closeModal();spaceModal();toast('日记已保存')}})}

function go(name){$$('.view').forEach(v=>v.classList.remove('active'));$('#'+name+'View')?.classList.add('active');if(name==='moments')renderMoments();if(name==='friends')renderFriends();if(name==='logs')renderLogs();if(name==='chat'){renderChat();$('#input').focus()}}
$$('[data-go]').forEach(b=>b.addEventListener('click',()=>go(b.dataset.go)));
function renderMoments(){const arr=[...parseTemplate('momentsTemplate'),...(state.moments||[])];$('#moments').innerHTML=arr.length?arr.map((m,i)=>`<article class="moment"><div class="momentHead"><img class="avatar" src="${state.peerAvatar||avatarSvg(m.name||'联系人')}" alt=""><div><b>${esc(m.name||'联系人')}</b><small>${esc(m.time||'')}</small><small>可见范围：${esc(m.range||'公开')}</small></div></div><p>${esc(m.text||'')}</p>${m.pics?.length?`<div class="momentPics">${m.pics.map(p=>`<img src="${esc(p)}" alt="">`).join('')}</div>`:''}<div class="momentActions"><button data-like-moment="${i}">赞 ${m.likes||0}</button><span>评论 ${m.comments||0}</span><span>转发</span></div></article>`).join(''):`<div style="padding:30px 10px;text-align:center;color:#999;font-size:10px">还没有朋友圈内容</div>`}
$('#moments').addEventListener('click',e=>{const i=e.target.closest('[data-like-moment]')?.dataset.likeMoment;if(i!==undefined){const base=parseTemplate('momentsTemplate').length;let all=[...parseTemplate('momentsTemplate'),...(state.moments||[])];all[i].likes=(all[i].likes||0)+1;if(i>=base)state.moments[i-base]=all[i];else $('#moments');save();renderMoments();toast('已点赞')}});
$('#postMomentBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>发布朋友圈</h3><div class="field"><textarea id="momentText" placeholder="写下此刻……"></textarea></div><div class="field"><label>配图 URL，多张用空格分隔</label><input id="momentPics"></div><div class="field"><label>可见范围</label><select id="momentRange"><option>公开</option><option>仅自己</option><option>仅好友</option><option>部分好友</option></select></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="publishMoment">发布</button></div>`);
$('#modal').addEventListener('click',e=>{if(e.target.id==='publishMoment'){const text=$('#momentText').value.trim();if(!text)return toast('请写一点内容');const m={name:'我',time:'刚刚',text,pics:$('#momentPics').value.trim().split(/\s+/).filter(Boolean),likes:0,comments:0,range:$('#momentRange').value};state.moments.unshift(m);save();$('#input').value=`[朋友圈] ${text}`;resizeInput();closeModal();go('moments');toast('朋友圈已发布，指令已填入聊天框')}});
function renderFriends(){$('#friends').innerHTML=[{name:state.peerName,note:state.relation||'聊天联系人'},{name:'纸飞机',note:'偶尔说晚安的人'},{name:'白噪音',note:'安静列表里的朋友'},{name:'小小房间',note:'共同群聊'}].map((f,i)=>`<div class="friend"><img class="avatar" src="${i===0?(state.peerAvatar||avatarSvg(f.name)):avatarSvg(f.name)}"><div class="grow"><b>${esc(f.name)}</b><small>${esc(f.note)}</small></div><button class="miniBtn" data-friend-chat="${i}">聊天</button></div>`).join('')}
$('#friends').addEventListener('click',e=>{if(e.target.closest('[data-friend-chat]'))go('chat')});
$('#addFriendBtn').onclick=()=>toast('好友添加暂为本地入口');
function renderLogs(){const a=[...parseTemplate('logsTemplate'),...messages.map(m=>`${m.time||''} ${m.side==='me'?'我':'AI'} ${m.type==='text'?'文本':m.type}`)];$('#logs').innerHTML=a.length ? a.map((x,i)=>`<div class="log"><b>record_${String(i+1).padStart(2,'0')}</b><div>${esc(x)}</div></div>`).join('') : `<div style="padding:30px 10px;text-align:center;color:#999;font-size:10px">还没有后台记录</div>`}
$('#refreshLogs').onclick=()=>{renderLogs();toast('记录已重新解析')};

const audio=$('#audio');
let music=[{title:'little grey song',artist:'airp local',url:''}],musicIndex=0,musicPlaying=false;
try{const ms=safeParse(localStorage.getItem('airp_music_v3'),null);if(ms){music=ms.list?.length?ms.list:music;musicIndex=ms.index||0;musicPlaying=!!ms.playing}}catch(_){ }
function saveMusic(){localStorage.setItem('airp_music_v3',JSON.stringify({list:music,index:musicIndex,playing:musicPlaying}))}
$('#musicBtn').onclick=musicModal;
function musicModal(){const cur=music[musicIndex]||music[0];openModal(`<button class="closex" data-close>×</button><h3>留声机</h3><div class="turntable"><div class="disc ${musicPlaying?'play':''}" id="modalDisc"></div><div class="turnInfo"><b>${esc(cur.title)}</b><small>${esc(cur.artist)}</small><div class="wave ${musicPlaying?'play':''}"><i></i><i></i><i></i><i></i><i></i></div></div></div><div class="playerBtns"><button data-prev>‹‹</button><button data-toggle>${musicPlaying?'暂停':'播放'}</button><button data-next>››</button></div><input class="range" id="musicRange" type="range" min="0" max="100" value="0"><div style="font-size:8px;color:#888;text-align:center;margin:4px" id="musicTime">00:00 / 00:00</div><div class="modalBtns"><button id="importMusicBtn">导入音乐</button><button id="clearMusicBtn">清空列表</button></div><div id="musicList"></div>`);renderMusicList();syncAudioUI()}
function renderMusicList(){const box=$('#musicList');if(!box)return;box.innerHTML=music.map((m,i)=>`<div class="musicRow"><div class="grow"><b>${esc(m.title)}</b><small>${esc(m.artist)}${m.url?' · 外部直链':''}</small></div><button class="miniBtn" data-pick="${i}">${i===musicIndex?'当前':'播放'}</button><button class="miniBtn" data-del="${i}">删</button></div>`).join('')}
$('#modal').addEventListener('click',e=>{if(e.target.matches('[data-toggle]'))toggleMusic();if(e.target.matches('[data-prev]')){musicIndex=(musicIndex-1+music.length)%music.length;loadSong(true);musicModal()}if(e.target.matches('[data-next]')){musicIndex=(musicIndex+1)%music.length;loadSong(true);musicModal()}if(e.target.matches('[data-pick]')){musicIndex=+e.target.dataset.pick;loadSong(true);musicModal();toast('已切换歌曲')}if(e.target.matches('[data-del]')){music.splice(+e.target.dataset.del,1);if(!music.length)music=[{title:'little grey song',artist:'airp local',url:''}];musicIndex=Math.min(musicIndex,music.length-1);saveMusic();musicModal();toast('已删除歌曲')}if(e.target.id==='importMusicBtn')importMusicModal();if(e.target.id==='clearMusicBtn'){music=[{title:'little grey song',artist:'airp local',url:''}];musicIndex=0;saveMusic();musicModal();toast('播放列表已清空')}});
function importMusicModal(){openModal(`<button class="closex" data-close>×</button><h3>导入音乐</h3><div class="field"><label>音乐直链</label><input id="musicUrl" placeholder="https://example.com/song.mp3"></div><div class="field"><label>歌曲名</label><input id="musicTitle" value="new song"></div><div class="field"><label>歌手</label><input id="musicArtist" value="unknown"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveMusic">保存</button></div>`)}
$('#modal').addEventListener('click',e=>{if(e.target.id==='saveMusic'){music.push({title:$('#musicTitle').value.trim()||'new song',artist:$('#musicArtist').value.trim()||'unknown',url:$('#musicUrl').value.trim()});musicIndex=music.length-1;saveMusic();closeModal();musicModal();toast('音乐已加入播放列表')}});
function loadSong(autoPlay=false){const cur=music[musicIndex];audio.src=cur?.url||'';audio.load();if(autoPlay&&cur?.url)audio.play().then(()=>{musicPlaying=true;saveMusic()}).catch(()=>{musicPlaying=false;saveMusic();toast('浏览器阻止了自动播放，请手动播放')})}
function toggleMusic(){const cur=music[musicIndex];if(!cur?.url){toast('当前歌曲没有外部直链');return}if(audio.paused){audio.play().then(()=>{musicPlaying=true;saveMusic();musicModal()}).catch(()=>toast('无法播放该直链'))}else{audio.pause();musicPlaying=false;saveMusic();musicModal()}}
audio.addEventListener('ended',()=>{musicIndex=(musicIndex+1)%music.length;loadSong(true)});audio.addEventListener('timeupdate',syncAudioUI);audio.addEventListener('loadedmetadata',syncAudioUI);
function syncAudioUI(){const r=$('#musicRange');const t=$('#musicTime');if(r&&audio.duration){r.value=(audio.currentTime/audio.duration)*100||0;if(t)t.textContent=formatSec(audio.currentTime)+' / '+formatSec(audio.duration)}else if(t)t.textContent='00:00 / 00:00'}
function formatSec(n){n=Math.floor(n||0);return String(Math.floor(n/60)).padStart(2,'0')+':'+String(n%60).padStart(2,'0')}
$('#modal').addEventListener('input',e=>{if(e.target.id==='musicRange'&&audio.duration)audio.currentTime=audio.duration*(+e.target.value/100)});

function clock(){const d=new Date();$('#clock').textContent=d.toLocaleTimeString('zh-CN',{hour:'2-digit',minute:'2-digit'});if(navigator.getBattery){}setTimeout(clock,1000)}clock();
if(navigator.getBattery){navigator.getBattery().then(b=>{const f=()=>{$('#batteryText').textContent=Math.round(b.level*100)+'%';$('#batteryBar').style.width=(b.level*100)+'%'};f();b.addEventListener('levelchange',f)}).catch(()=>{})}

apply();loadMessages();renderChat(false);
window.airp={state,api,messages,sendMessage,renderChat,save,saveApi};
})();
</script>
</body>
</html>
