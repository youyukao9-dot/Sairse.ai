<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>airp · little phone</title>
<style>
:root{
 --bg:#dededb;--phone:#f3f3f0;--glass:rgba(255,255,255,.70);--glass2:rgba(255,255,255,.46);
 --line:rgba(46,46,48,.13);--ink:#2f2f32;--muted:#8b8b91;--mine:#ddddda;--other:#fbfbf8;
 --danger:#897070;--shadow:0 30px 100px rgba(24,24,24,.22);--wallpaper:none;--phoneEdge:#242426;
}
*{box-sizing:border-box}html,body{margin:0;min-height:100%;font-family:Inter,"PingFang SC","Microsoft YaHei",system-ui,sans-serif;color:var(--ink);background:radial-gradient(circle at 30% 10%,#fff 0,#e7e7e3 38%,#d4d4d0 100%)}
body:before{content:"";position:fixed;inset:0;pointer-events:none;opacity:.28;background-image:radial-gradient(rgba(70,70,70,.12) .6px,transparent .8px);background-size:6px 6px;mix-blend-mode:multiply}
button,input,textarea,select{font:inherit}button{border:0;background:none;color:inherit;cursor:pointer}button:active{transform:scale(.97)}
.app{min-height:100vh;display:grid;place-items:center;padding:18px}
.phone{width:min(350px,calc(100vw - 24px));height:min(760px,calc(100vh - 24px));min-height:620px;border:10px solid var(--phoneEdge);border-radius:50px;background:#151516;box-shadow:var(--shadow),inset 0 0 0 1px rgba(255,255,255,.12);padding:6px;position:relative}
.phone:before{content:"";position:absolute;left:-13px;top:125px;width:4px;height:52px;border-radius:4px;background:#2c2c2d}.phone:after{content:"";position:absolute;right:-13px;top:178px;width:4px;height:72px;border-radius:4px;background:#2c2c2d}
.screen{height:100%;overflow:hidden;border-radius:37px;background:var(--phone);position:relative;background-image:var(--wallpaper),radial-gradient(circle at 80% 10%,rgba(255,255,255,.82),transparent 28%),linear-gradient(145deg,#f7f7f4,#e9e9e6)}
.screen:before{content:"";position:absolute;inset:0;pointer-events:none;background:linear-gradient(125deg,rgba(255,255,255,.48),transparent 45%),radial-gradient(circle at 20% 80%,rgba(255,255,255,.33),transparent 30%);z-index:1}
.status{height:28px;display:flex;justify-content:space-between;align-items:center;padding:7px 18px 0;font-size:10px;letter-spacing:.05em;position:relative;z-index:10}.statusRight{display:flex;gap:8px;align-items:center}.battery{width:21px;height:9px;border:1px solid currentColor;border-radius:3px;padding:1px;position:relative}.battery i{display:block;height:100%;width:72%;background:currentColor;border-radius:1px}.battery:after{content:"";display:block;width:2px;height:3px;background:currentColor;position:absolute;right:-4px;top:2px;border-radius:1px}
.view{position:absolute;inset:28px 0 0;display:none;flex-direction:column;z-index:2}.view.active{display:flex}
.homeTop{padding:18px 18px 12px}.dateCard{padding:14px 16px;border:1px solid var(--line);background:var(--glass);backdrop-filter:blur(18px);border-radius:22px;box-shadow:0 8px 25px rgba(0,0,0,.06)}.dateLine{display:flex;justify-content:space-between;align-items:end}.day{font-size:34px;font-weight:300;line-height:1}.month{font-size:11px;color:var(--muted);letter-spacing:.18em}.quote{margin-top:9px;font-size:11px;color:var(--muted)}
.homeApps{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;padding:12px 18px}.appIcon{aspect-ratio:1;border:1px solid var(--line);background:rgba(255,255,255,.48);border-radius:19px;display:grid;place-items:center;position:relative;box-shadow:0 7px 18px rgba(0,0,0,.05)}.appIcon:before{content:"";position:absolute;inset:7px;border:1px solid rgba(0,0,0,.07);border-radius:14px}.cross{font-size:19px;font-weight:200}.iconLabel{font-size:8px;color:var(--muted);margin-top:3px}
.turntable{margin:2px 18px 12px;height:104px;border:1px solid var(--line);border-radius:25px;background:var(--glass2);backdrop-filter:blur(15px);display:flex;align-items:center;gap:15px;padding:14px;box-shadow:0 8px 25px rgba(0,0,0,.05)}.disc{width:72px;height:72px;flex:0 0 auto;border-radius:50%;background:radial-gradient(circle,#e9e9e7 0 7px,#777 8px 9px,#29292b 10px 30px,#b7b7b5 31px 32px,#353537 33px 36px,#d6d6d2 37px);box-shadow:0 6px 12px rgba(0,0,0,.18);animation:spin 5s linear infinite;animation-play-state:paused}.disc.play{animation-play-state:running}@keyframes spin{to{transform:rotate(360deg)}}.turnInfo{min-width:0;flex:1}.turnInfo b{font-size:12px}.turnInfo small{display:block;color:var(--muted);font-size:9px;margin-top:5px}.wave{display:flex;gap:2px;height:17px;align-items:center;margin-top:7px}.wave i{width:2px;height:5px;background:#777;border-radius:2px}.wave.play i{animation:wave .7s ease-in-out infinite alternate}.wave i:nth-child(2){animation-delay:.08s}.wave i:nth-child(3){animation-delay:.16s}.wave i:nth-child(4){animation-delay:.24s}.wave i:nth-child(5){animation-delay:.32s}@keyframes wave{to{height:15px}}
.homeChat{margin:auto 18px 18px;padding:13px;border:1px solid var(--line);border-radius:24px;background:rgba(255,255,255,.62);display:flex;align-items:center;gap:10px}.avatar{width:34px;height:34px;border-radius:12px;object-fit:cover;background:#d9d9d6;border:1px solid var(--line)}.homeChat .txt{flex:1}.homeChat b{font-size:11px}.homeChat small{display:block;color:var(--muted);font-size:9px;margin-top:3px}.arrow{font-size:17px;color:#777}
.chatHead{height:57px;padding:5px 13px;display:flex;align-items:center;gap:9px;border-bottom:1px solid var(--line);background:rgba(247,247,244,.68);backdrop-filter:blur(18px);z-index:4}.headBack{width:24px;font-size:21px;color:#666}.headInfo{flex:1;min-width:0;text-align:left}.headInfo b{font-size:12px}.headInfo small{display:block;color:var(--muted);font-size:8px;margin-top:2px}.dots{font-size:20px;letter-spacing:2px}.relTag{font-size:8px;padding:3px 6px;border:1px solid var(--line);border-radius:99px;color:var(--muted);white-space:nowrap}
.messages{flex:1;overflow:auto;padding:14px 12px 8px;scrollbar-width:none}.messages::-webkit-scrollbar,.friendsList::-webkit-scrollbar,.moments::-webkit-scrollbar,.logs::-webkit-scrollbar{display:none}.msg{display:flex;gap:7px;margin:7px 0;animation:pop .22s ease both}.msg.mine{justify-content:flex-end}.msg.mine .bubbleWrap{align-items:flex-end}.msg.mine .avatar{order:2}.bubbleWrap{max-width:78%;display:flex;flex-direction:column;align-items:flex-start}.bubble{border:1px solid var(--line);padding:9px 11px;border-radius:17px;background:var(--other);font-size:12px;line-height:1.55;box-shadow:0 4px 12px rgba(0,0,0,.04);word-break:break-word}.mine .bubble{background:var(--mine);border-top-right-radius:5px}.other .bubble{border-top-left-radius:5px}.time{font-size:7px;color:var(--muted);margin:3px 4px}.quoteBox{border-left:2px solid #999;padding:5px 7px;margin-bottom:6px;color:#777;background:rgba(0,0,0,.035);border-radius:7px;font-size:9px}.voice{min-width:98px;display:flex;align-items:center;gap:8px}.voiceBars{display:flex;gap:2px;align-items:center;height:18px}.voiceBars i{width:2px;height:7px;background:#777;border-radius:2px}.voiceBars.play i{animation:wave .6s infinite alternate}.sticker{max-width:110px;max-height:110px;border-radius:14px;object-fit:contain}.redpacket,.transfer{width:190px;padding:13px;background:#efefeb;border:1px solid var(--line);border-radius:16px}.rpTitle,.trTitle{font-size:11px;font-weight:600}.rpSub,.trSub{font-size:8px;color:#777;margin-top:5px}.linkCard{width:205px;padding:10px;display:flex;gap:8px;background:#f8f8f5;border:1px solid var(--line);border-radius:13px}.linkCard img{width:50px;height:50px;border-radius:9px;object-fit:cover;background:#ddd}.linkCard b{font-size:10px;display:block}.linkCard small{font-size:8px;color:#888}.systemMsg{text-align:center;font-size:8px;color:#8c8c8c;margin:10px 0}.relCard{padding:11px;background:#f8f8f5;border:1px solid var(--line);border-radius:14px}.relCard b{font-size:11px}.relBtns{display:flex;gap:6px;margin-top:9px}.miniBtn{flex:1;border:1px solid var(--line);border-radius:10px;padding:6px;font-size:9px;background:#fff}.miniBtn.primary{background:#333;color:#fff}.retracted{font-style:italic;color:#999}
.quick{display:flex;gap:6px;overflow:auto;padding:5px 10px;scrollbar-width:none}.quick button{white-space:nowrap;border:1px solid var(--line);background:rgba(255,255,255,.6);border-radius:99px;padding:5px 8px;font-size:9px}.composer{padding:7px 9px 10px;display:flex;gap:7px;align-items:center;background:rgba(247,247,244,.75);backdrop-filter:blur(18px);border-top:1px solid var(--line)}.tool{width:29px;height:29px;border:1px solid var(--line);border-radius:10px;background:rgba(255,255,255,.6);display:grid;place-items:center;font-size:12px}.input{flex:1;min-width:0;border:1px solid var(--line);background:rgba(255,255,255,.72);border-radius:14px;min-height:31px;max-height:70px;padding:7px 9px;font-size:11px;resize:none;outline:none}.send{padding:8px 11px;border-radius:12px;background:#333;color:#fff;font-size:10px}
.subHead{height:55px;display:flex;align-items:center;justify-content:space-between;padding:0 15px;border-bottom:1px solid var(--line);background:rgba(247,247,244,.72);backdrop-filter:blur(16px);z-index:4}.subHead b{font-size:13px}.subBody{flex:1;overflow:auto;padding:13px}.friendsList{flex:1;overflow:auto;padding:10px}.friend{display:flex;align-items:center;gap:10px;padding:11px;border-bottom:1px solid var(--line)}.friend .grow{flex:1}.friend b{font-size:11px}.friend small{display:block;color:#888;font-size:8px;margin-top:3px}
.moments{flex:1;overflow:auto;padding:12px}.moment{padding:12px;border:1px solid var(--line);background:rgba(255,255,255,.6);border-radius:19px;margin-bottom:10px}.momentHead{display:flex;gap:8px;align-items:center}.moment b{font-size:10px}.moment small{display:block;color:#999;font-size:8px}.moment p{font-size:10px;line-height:1.6}.momentPics{display:grid;grid-template-columns:repeat(3,1fr);gap:5px}.momentPics img{width:100%;aspect-ratio:1;object-fit:cover;border-radius:9px}.momentActions{display:flex;gap:12px;margin-top:8px;font-size:9px;color:#777}
.logs{flex:1;overflow:auto;padding:12px}.log{padding:10px 11px;border-left:2px solid #999;background:rgba(255,255,255,.48);border-radius:0 12px 12px 0;margin-bottom:7px;font-size:9px}.log b{font-size:9px}.bottomNav{height:43px;display:flex;border-top:1px solid var(--line);background:rgba(247,247,244,.72);backdrop-filter:blur(16px)}.navBtn{flex:1;font-size:8px;color:#777}.navBtn.active{color:#222;font-weight:600}
.overlay{position:absolute;inset:0;background:rgba(20,20,20,.18);backdrop-filter:blur(7px);z-index:30;display:none;align-items:flex-end;padding:12px}.overlay.show{display:flex}.modal{width:100%;max-height:84%;overflow:auto;background:rgba(248,248,245,.96);border:1px solid rgba(255,255,255,.85);border-radius:25px;box-shadow:0 20px 55px rgba(0,0,0,.2);padding:16px;animation:up .2s ease}.modal.center{align-self:center}.modal h3{margin:0 0 12px;font-size:13px}.modal p{font-size:10px;line-height:1.5;color:#777}.field{margin:9px 0}.field label{display:block;font-size:8px;color:#888;margin:0 0 5px}.field input,.field textarea,.field select{width:100%;border:1px solid var(--line);background:#fff;border-radius:12px;padding:8px;font-size:10px;outline:none}.field textarea{min-height:72px;resize:vertical}.modalBtns{display:flex;gap:7px;margin-top:13px}.modalBtns button{flex:1;padding:9px;border-radius:12px;border:1px solid var(--line);font-size:10px;background:#fff}.modalBtns .primary{background:#333;color:#fff}.closex{float:right;font-size:18px;color:#777}.menuGrid{display:grid;grid-template-columns:repeat(3,1fr);gap:7px}.menuGrid button{padding:10px 5px;border:1px solid var(--line);border-radius:13px;background:#fff;font-size:9px}.emojiGrid{display:grid;grid-template-columns:repeat(5,1fr);gap:6px}.emo{height:33px;border:1px solid var(--line);border-radius:9px;background:#fff;font-size:9px}.toast{position:absolute;left:50%;bottom:68px;transform:translate(-50%,15px);z-index:60;background:rgba(40,40,40,.9);color:#fff;padding:7px 11px;border-radius:99px;font-size:9px;opacity:0;pointer-events:none;transition:.25s;white-space:nowrap;max-width:84%;overflow:hidden;text-overflow:ellipsis}.toast.show{opacity:1;transform:translate(-50%,0)}.musicRow{display:flex;align-items:center;gap:8px;padding:8px 0;border-bottom:1px solid var(--line)}.musicRow .grow{flex:1}.musicRow b{font-size:9px}.musicRow small{display:block;font-size:7px;color:#888}.range{width:100%}.playerBtns{display:flex;justify-content:center;gap:12px;margin:8px}.playerBtns button{width:33px;height:33px;border:1px solid var(--line);border-radius:50%;background:#fff}.tag{display:inline-block;padding:3px 7px;border:1px solid var(--line);border-radius:99px;font-size:8px;color:#777}.divider{height:1px;background:var(--line);margin:12px 0}.previewImg{width:100%;max-height:150px;object-fit:cover;border-radius:14px;border:1px solid var(--line)}.videoBox{padding:18px 10px;text-align:center}.videoLens{width:85px;height:85px;border-radius:50%;margin:0 auto 12px;border:1px solid var(--line);background:radial-gradient(circle,#222 0 8px,#d8d8d5 9px 25px,#555 26px 34px,#eee 35px)}
.night{--bg:#111214;--phone:#1a1b1d;--glass:rgba(35,36,39,.78);--glass2:rgba(40,41,44,.62);--line:rgba(255,255,255,.1);--ink:#e9e9e7;--muted:#99999e;--mine:#303134;--other:#242528;--phoneEdge:#0f0f10}.night body{}.night .screen{background-image:var(--wallpaper),radial-gradient(circle at 80% 10%,rgba(255,255,255,.04),transparent 28%),linear-gradient(145deg,#1d1e20,#131416)}.night .dateCard,.night .homeChat,.night .appIcon,.night .turntable,.night .composer,.night .chatHead,.night .subHead,.night .bottomNav{background:rgba(25,26,28,.72)}.night .modal{background:rgba(29,30,32,.97)}.night .field input,.night .field textarea,.night .field select,.night .miniBtn,.night .menuGrid button,.night .emo,.night .modalBtns button,.night .playerBtns button,.night .linkCard,.night .redpacket,.night .transfer,.night .relCard{background:#232427;color:#eee}
@keyframes pop{from{opacity:0;transform:translateY(6px) scale(.98)}to{opacity:1;transform:none}}@keyframes up{from{opacity:0;transform:translateY(15px)}to{opacity:1;transform:none}}
@media(max-width:480px){.app{padding:12px}.phone{width:min(350px,calc(100vw - 18px));height:min(760px,calc(100vh - 18px));min-height:610px}.screen{border-radius:37px}}
@media(max-height:660px){.phone{height:calc(100vh - 12px);min-height:0}.app{padding:6px}}
</style>
</head>
<body>
<div class="app"><div class="phone"><div class="screen" id="screen">
  <div class="status"><span id="clock">04:46</span><span class="statusRight"><span id="signal">LTE</span><span id="batteryText">72%</span><span class="battery"><i id="batteryBar"></i></span></span></div>

  <section class="view active" id="homeView">
    <div class="homeTop"><div class="dateCard"><div class="dateLine"><div><div class="day" id="homeDay">09</div><div class="month" id="homeMonth">SEP / WEDNESDAY</div></div><div class="cross">+</div></div><div class="quote">今天也把一点点温柔留给自己。</div></div></div>
    <div class="homeApps">
      <button class="appIcon" data-go="chat"><div><div class="cross">+</div><div class="iconLabel">chat</div></div></button>
      <button class="appIcon" data-go="moments"><div><div class="cross">x</div><div class="iconLabel">moments</div></div></button>
      <button class="appIcon" data-go="friends"><div><div class="cross">□</div><div class="iconLabel">friends</div></div></button>
      <button class="appIcon" id="homeSettings"><div><div class="cross">○</div><div class="iconLabel">settings</div></div></button>
    </div>
    <div class="turntable"><div class="disc" id="homeDisc"></div><div class="turnInfo"><b id="homeSong">little grey song</b><small id="homeArtist">airp local player</small><div class="wave" id="homeWave"><i></i><i></i><i></i><i></i><i></i></div></div><button class="tool" id="homePlay" aria-label="播放">></button></div>
    <button class="homeChat" data-go="chat"><img class="avatar" id="homeAvatar"><div class="txt"><b id="homeChatName">小熊</b><small>有些话，晚一点说也没关系。</small></div><span class="arrow">></span></button>
  </section>

  <section class="view" id="chatView">
    <header class="chatHead"><button id="backHome" class="headBack">‹</button><button id="avatarBtn"><img class="avatar" id="peerAvatar"></button><button class="headInfo" id="nameBtn"><b id="peerName">小熊</b><small id="peerSub">在线 · 点击修改备注</small></button><span id="relTag" class="relTag" style="display:none"></span><button class="dots" id="menuBtn">···</button></header>
    <div class="messages" id="messages"></div>
    <div class="quick" id="quick"><button data-ins="，">，</button><button data-ins="。">。</button><button data-ins="？">？</button><button data-ins="！">！</button><button data-ins="……">……</button><button data-ins="「」">「」</button><button data-ins="（ ）">（ ）</button><button data-ins="【 】">【 】</button></div>
    <div class="composer"><button class="tool" id="emojiBtn">＋</button><textarea class="input" id="input" rows="1" placeholder="写点什么……"></textarea><button class="tool" id="musicBtn">♪</button><button class="send" id="sendBtn">发送</button></div>
  </section>

  <section class="view" id="momentsView"><header class="subHead"><button data-go="home">‹</button><b>朋友圈</b><button id="postMomentBtn">＋</button></header><div class="moments" id="moments"></div><div class="bottomNav"><button class="navBtn" data-go="chat">聊天</button><button class="navBtn active">朋友圈</button><button class="navBtn" data-go="friends">好友</button></div></section>
  <section class="view" id="friendsView"><header class="subHead"><button data-go="home">‹</button><b>好友</b><button id="addFriendBtn">＋</button></header><div class="friendsList" id="friends"></div><div class="bottomNav"><button class="navBtn" data-go="chat">聊天</button><button class="navBtn" data-go="moments">朋友圈</button><button class="navBtn active">好友</button></div></section>
  <section class="view" id="logsView"><header class="subHead"><button data-go="chat">‹</button><b>查岗 / 后台记录</b><button id="refreshLogs">↻</button></header><div class="logs" id="logs"></div></section>

  <div class="overlay" id="overlay"><div class="modal" id="modal"></div></div><div class="toast" id="toast"></div>
</div></div></div>

<script type="text/template" id="chatTemplate">
[
 {"id":1,"side":"other","type":"text","time":"23:41","text":"今天有一点点想你。"},
 {"id":2,"side":"me","type":"text","time":"23:42","text":"我也没有睡。"},
 {"id":3,"side":"other","type":"quote","time":"23:43","quote":"我也没有睡。","text":"那就陪我聊一会儿。"},
 {"id":4,"side":"other","type":"voice","time":"23:44","duration":12,"text":"其实只是想听听你的声音。"},
 {"id":5,"side":"me","type":"sticker","time":"23:45","url":"","text":"[sticker: soft bear]"},
 {"id":6,"side":"other","type":"redpacket","time":"23:46","title":"给你留了一点好运","sub":"点击拆开","opened":false},
 {"id":7,"side":"me","type":"transfer","time":"23:47","amount":"52.00","status":"unpaid"},
 {"id":8,"side":"other","type":"link","time":"23:48","title":"凌晨四点的白","desc":"一份很安静的歌单","thumb":""},
 {"id":9,"side":"system","type":"text","time":"23:49","text":"对方撤回了一条消息"},
 {"id":10,"side":"system","type":"text","time":"23:50","text":"你们已经成为好友。"},
 {"id":11,"side":"other","type":"relation","time":"23:51","relation":"恋人","text":"想和你建立一段更亲密的关系。"}
]
</script>
<script type="text/template" id="logsTemplate">
["初始化聊天记录解析完成","23:41 对方发送文本","23:44 对方发送语音","23:46 对方发送红包","23:49 系统检测到撤回消息","23:51 收到关系申请","本地存储状态检查完成"]
</script>
<script type="text/template" id="momentsTemplate">
[
 {"name":"小熊","time":"今天 00:12","text":"凌晨的风很轻，适合想念。","pics":[],"likes":12,"comments":3,"range":"公开"},
 {"name":"小熊","time":"昨天 22:31","text":"把今天折叠起来，明天再打开。","pics":[],"likes":8,"comments":1,"range":"仅好友"},
 {"name":"小熊","time":"周一 19:20","text":"留一点空白给自己。","pics":[],"likes":21,"comments":5,"range":"公开"}
]
</script>

<script>
(()=>{
'use strict';
const $=s=>document.querySelector(s), $$=s=>[...document.querySelectorAll(s)];
const STORE='airp_little_phone_v2';
const API_STORE='airp_little_phone_api_v1';
const defaults={theme:'light',peerName:'小熊',peerAvatar:'',myAvatar:'',wallpaper:'',blocked:false,relation:'',relationPending:'',relationSpace:{days:0,last:'',moods:[],diary:[]},music:[{title:'little grey song',artist:'airp local player',url:''}],musicIndex:0,musicPlaying:false,musicTime:0,openedPackets:[],moments:[],customChats:[]};
let state={...defaults,...safeParse(localStorage.getItem(STORE),{})};
state.relationSpace=Object.assign(defaults.relationSpace,state.relationSpace||{});
state.music=Array.isArray(state.music)&&state.music.length?state.music:structuredClone(defaults.music);
state.moments=Array.isArray(state.moments)?state.moments:[];
state.customChats=Array.isArray(state.customChats)?state.customChats:[];
let chats=[...safeParse(document.getElementById('chatTemplate').textContent,[]),...state.customChats];
let audio=new Audio(); audio.preload='metadata';

function safeParse(s,fallback){try{return JSON.parse(s)}catch{return fallback}}
function save(){localStorage.setItem(STORE,JSON.stringify({...state,customChats:chats.filter(x=>x.id>1000000000000)}))}
function saveApi(v){localStorage.setItem(API_STORE,JSON.stringify(v))}
function loadApi(){return safeParse(localStorage.getItem(API_STORE),{endpoint:'https://api.openai.com/v1/chat/completions',key:'',model:'gpt-4o-mini',system:'你正在参与一个温柔、克制的模拟聊天。请用自然中文回复。'})}
function toast(t){const e=$('#toast');e.textContent=t;e.classList.add('show');clearTimeout(toast.t);toast.t=setTimeout(()=>e.classList.remove('show'),1700)}
function openModal(html,center=false){$('#modal').innerHTML=html;$('#modal').classList.toggle('center',center);$('#overlay').classList.add('show')}
function closeModal(){$('#overlay').classList.remove('show')}
function esc(s=''){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function avatarSvg(t='我'){return 'data:image/svg+xml;charset=UTF-8,'+encodeURIComponent(`<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><rect width="100" height="100" rx="28" fill="#d9d9d5"/><circle cx="50" cy="43" r="20" fill="#f5f5f1"/><path d="M25 82c7-20 43-20 50 0" fill="#f5f5f1"/><text x="50" y="92" text-anchor="middle" font-size="9" fill="#777">${esc(t).slice(0,2)}</text></svg>`) }
function apply(){
 document.documentElement.classList.toggle('night',state.theme==='dark');
 document.documentElement.style.setProperty('--wallpaper',state.wallpaper?`url("${state.wallpaper.replaceAll('"','\\"')}")`:'none');
 $('#peerName').textContent=state.peerName+(state.blocked?' 已拉黑':''); $('#homeChatName').textContent=state.peerName;
 $('#peerAvatar').src=state.peerAvatar||avatarSvg(state.peerName); $('#homeAvatar').src=state.peerAvatar||avatarSvg(state.peerName);
 $('#relTag').style.display=state.relation?'inline-block':'none'; $('#relTag').textContent=state.relation||'';
 const song=state.music[state.musicIndex]||defaults.music[0]; $('#homeSong').textContent=song.title; $('#homeArtist').textContent=song.artist;
 $('#homeDisc').classList.toggle('play',!!state.musicPlaying); $('#homeWave').classList.toggle('play',!!state.musicPlaying);
}
function go(name){$$('.view').forEach(v=>v.classList.remove('active'));const id=name==='home'?'homeView':name+'View';$('#'+id)?.classList.add('active');if(name==='chat')renderChat();if(name==='moments')renderMoments();if(name==='friends')renderFriends();if(name==='logs')renderLogs()}
$$('[data-go]').forEach(b=>b.addEventListener('click',()=>{go(b.dataset.go);toast('已切换页面')}));
$('#backHome').onclick=()=>go('home');
function renderChat(){
 const box=$('#messages'); box.innerHTML='';
 chats.forEach((m,i)=>{
  if(m.side==='system'){const s=document.createElement('div');s.className='systemMsg';s.textContent=m.text;box.appendChild(s);return}
  const el=document.createElement('div');el.className='msg '+(m.side==='me'?'mine':'other');el.dataset.id=m.id;
  const av=document.createElement('img');av.className='avatar';av.src=m.side==='me'?(state.myAvatar||avatarSvg('我')):(state.peerAvatar||avatarSvg(state.peerName));
  const wrap=document.createElement('div');wrap.className='bubbleWrap';let body='';
  if(m.type==='text')body=`<div class="bubble">${esc(m.text)}</div>`;
  else if(m.type==='quote')body=`<div class="bubble"><div class="quoteBox">${esc(m.quote||'')}</div>${esc(m.text)}</div>`;
  else if(m.type==='voice')body=`<button class="bubble voice" data-voice="${m.id}"><span>${m.duration||0}s</span><span class="voiceBars"><i></i><i></i><i></i><i></i><i></i></span><span data-voice-state="${m.id}">转文字</span></button><div class="time" data-voice-text="${m.id}" style="display:none">${esc(m.text||'')}</div>`;
  else if(m.type==='sticker')body=m.url?`<div class="bubble"><img class="sticker" src="${esc(m.url)}"></div>`:`<div class="bubble">${esc(m.text||'[贴纸]')}</div>`;
  else if(m.type==='redpacket')body=`<button class="redpacket" data-rp="${m.id}"><div class="rpTitle">${esc(m.title||'红包')}</div><div class="rpSub">${state.openedPackets.includes(m.id)?'已拆开':'点击拆开'}</div></button>`;
  else if(m.type==='transfer')body=`<button class="transfer" data-tr="${m.id}"><div class="trTitle">转账 ¥${esc(m.amount||'0.00')}</div><div class="trSub">${m.status==='paid'?'已收款':'待收款 · 点击处理'}</div></button>`;
  else if(m.type==='link')body=`<div class="linkCard"><img src="${m.thumb?esc(m.thumb):avatarSvg('链')}" onerror="this.src='${avatarSvg('链')}'"><div><b>${esc(m.title||'链接')}</b><small>${esc(m.desc||'')}</small></div></div>`;
  else if(m.type==='relation')body=`<div class="relCard"><b>关系申请 · ${esc(m.relation||'朋友')}</b><div style="font-size:9px;margin-top:5px;color:#777">${esc(m.text||'')}</div><div class="relBtns"><button class="miniBtn" data-rel="no" data-rel-id="${m.id}">拒绝</button><button class="miniBtn primary" data-rel="yes" data-rel-id="${m.id}">同意</button></div></div>`;
  wrap.innerHTML=body+`<div class="time">${esc(m.time||'')}</div>`;el.append(av,wrap);box.appendChild(el);
  let timer=null; const quote=()=>quoteMessage(m); el.addEventListener('contextmenu',e=>{e.preventDefault();quote()}); el.addEventListener('pointerdown',()=>{timer=setTimeout(quote,600)}); ['pointerup','pointercancel','pointerleave'].forEach(x=>el.addEventListener(x,()=>clearTimeout(timer)));
 }); box.scrollTop=box.scrollHeight;
}
function quoteMessage(m){if(m.side==='system')return;$('#input').value=`引用：${m.text||m.title||m.amount||'[消息]'}`;$('#input').focus();toast('已引用这条消息')}
$('#messages').addEventListener('click',e=>{
 const voice=e.target.closest('[data-voice]');if(voice){const id=voice.dataset.voice,tx=$(`[data-voice-text="${id}"]`),st=$(`[data-voice-state="${id}"]`),bars=voice.querySelector('.voiceBars');const show=tx.style.display==='none';tx.style.display=show?'block':'none';st.textContent=show?'收起':'转文字';bars.classList.toggle('play',show);toast(show?'已展开转文字':'已收起转文字');return}
 const rp=e.target.closest('[data-rp]');if(rp){openModal(`<button class="closex" data-close>×</button><h3>红包</h3><div class="videoBox"><div class="cross" style="font-size:34px">+</div><p>愿你今天也被一点好运轻轻接住。</p><button class="send" data-open-rp="${rp.dataset.rp}">拆开红包</button></div>`,true);return}
 const tr=e.target.closest('[data-tr]');if(tr){const m=chats.find(x=>String(x.id)===tr.dataset.tr);openModal(`<button class="closex" data-close>×</button><h3>转账</h3><p>金额：¥${esc(m?.amount||'0.00')}<br>${m?.status==='paid'?'这笔转账已经收款。':'确认收款后，本地记录会变更为已收款。'}</p><div class="modalBtns"><button data-close>取消</button><button class="primary" data-pay="${tr.dataset.tr}" ${m?.status==='paid'?'disabled':''}>${m?.status==='paid'?'已收款':'确认收款'}</button></div>`,true);return}
 const rel=e.target.closest('[data-rel]');if(rel){const yes=rel.dataset.rel==='yes';const m=chats.find(x=>String(x.id)===rel.dataset.relId);if(yes){state.relation=m?.relation||'朋友';state.relationPending='';save();apply();closeModal();chats.push({id:Date.now(),side:'system',type:'text',time:'刚刚',text:`你们已经建立${state.relation}关系。`});renderChat();toast('关系已建立')}else{state.relationPending='';save();closeModal();chats.push({id:Date.now(),side:'system',type:'text',time:'刚刚',text:'你拒绝了这次关系申请。'});renderChat();toast('已拒绝关系申请')}}
});
$('#overlay').addEventListener('click',e=>{if(e.target===$('#overlay'))closeModal()});
$('#modal').addEventListener('click',async e=>{
 if(e.target.closest('[data-close]')){closeModal();return}
 const openRp=e.target.closest('[data-open-rp]');if(openRp){const id=Number(openRp.dataset.openRp);if(!state.openedPackets.includes(id))state.openedPackets.push(id);save();closeModal();renderChat();toast('红包已拆开');return}
 const pay=e.target.closest('[data-pay]');if(pay){const m=chats.find(x=>String(x.id)===pay.dataset.pay);if(m){m.status='paid';save();closeModal();renderChat();toast('已收款')}return}
 const menu=e.target.closest('[data-menu]');if(menu){handleMenu(menu.dataset.menu);return}
 const emo=e.target.closest('[data-emo]');if(emo){$('#input').value+=emo.dataset.emo;closeModal();$('#input').focus();toast('已填入表情指令');return}
 const pick=e.target.closest('[data-pick]');if(pick){state.musicIndex=Number(pick.dataset.pick);state.musicPlaying=true;syncAudio();save();musicModal();toast('已切换歌曲');return}
 const del=e.target.closest('[data-del]');if(del){state.music.splice(Number(del.dataset.del),1);if(!state.music.length)state.music=structuredClone(defaults.music);state.musicIndex=Math.min(state.musicIndex,state.music.length-1);state.musicPlaying=false;syncAudio(true);save();musicModal();toast('已删除歌曲');return}
 if(e.target.id==='toggleMusicModal'){toggleMusic();musicModal();return}
 if(e.target.id==='prevMusic'){changeMusic(-1);musicModal();return}
 if(e.target.id==='nextMusic'){changeMusic(1);musicModal();return}
 if(e.target.id==='importMusic'){importMusic();return}
 if(e.target.id==='clearMusic'){state.music=structuredClone(defaults.music);state.musicIndex=0;state.musicPlaying=false;syncAudio(true);save();musicModal();toast('播放列表已清空');return}
 if(e.target.id==='saveRemark'){state.peerName=$('#remark').value.trim()||'小熊';save();apply();closeModal();toast('备注已保存');return}
 if(e.target.id==='saveAvatar'){await saveAvatar();return}
 if(e.target.id==='saveWall'){await saveWallpaper();return}
 if(e.target.id==='saveRelationPick'){sendRelation($('#relationSelect').value);return}
 if(e.target.id==='checkin'){checkin();return}
 if(e.target.id==='mood'){randomMood();return}
 if(e.target.id==='diary'){diaryModal();return}
 const dd=e.target.closest('[data-del-diary]');if(dd){state.relationSpace.diary.splice(Number(dd.dataset.delDiary),1);save();spaceModal();toast('日记已删除');return}
 if(e.target.id==='clearRelation'){confirmDanger('解除关系','解除后会清空关系空间里的打卡、心情与日记。','确认解除',()=>{state.relation='';state.relationSpace={days:0,last:'',moods:[],diary:[]};save();apply();closeModal();chats.push({id:Date.now(),side:'system',type:'text',time:'刚刚',text:'你们的关系已经解除。'});renderChat();toast('关系已解除，空间已清空')});return}
 if(e.target.id==='saveDiary'){const t=$('#diaryText').value.trim();if(!t){toast('还没有写内容');return}state.relationSpace.diary.unshift({date:new Date().toLocaleString('zh-CN'),text:t});save();closeModal();toast('日记已保存');return}
 if(e.target.id==='confirmBlock'){state.blocked=!state.blocked;save();apply();closeModal();chats.push({id:Date.now(),side:'system',type:'text',time:'刚刚',text:state.blocked?'你已将对方拉黑':'你已解除拉黑'});renderChat();toast(state.blocked?'已拉黑':'已解除拉黑');return}
 if(e.target.id==='saveApi'){const a={endpoint:$('#apiEndpoint').value.trim(),key:$('#apiKey').value.trim(),model:$('#apiModel').value.trim(),system:$('#apiSystem').value};saveApi(a);closeModal();toast('API 配置已保存');return}
 if(e.target.id==='sendApiTest'){await callApi($('#apiTestInput').value.trim()||'你好');return}
 if(e.target.id==='publishMoment'){publishMoment();return}
 if(e.target.id==='saveFriend'){const name=$('#friendName').value.trim()||'新朋友';chats.push({id:Date.now(),side:'system',type:'text',time:'刚刚',text:`已添加联系人：${name}`});closeModal();save();renderChat();toast('好友已加入本地列表');return}
 if(e.target.id==='saveVideo'){closeModal();$('#input').value+='[视频通话邀请]';$('#input').focus();toast('视频通话指令已填入');return}
 if(e.target.id==='saveMusicImport'){await saveImportedMusic();return}
});

function handleMenu(m){closeModal();if(m==='logs')go('logs');else if(m==='moments')go('moments');else if(m==='friends')go('friends');else if(m==='relation')relationModal();else if(m==='space')spaceModal();else if(m==='wallpaper')wallpaperModal();else if(m==='avatar')avatarModal();else if(m==='theme'){state.theme=state.theme==='light'?'dark':'light';save();apply();toast(state.theme==='dark'?'已切换夜间':'已切换白间')}else if(m==='api')apiModal();else if(m==='block')blockModal();else if(m==='video')videoModal()}
$('#menuBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>更多</h3><div class="menuGrid"><button data-menu="logs">查岗记录</button><button data-menu="moments">朋友圈</button><button data-menu="friends">好友列表</button><button data-menu="relation">建立关系</button><button data-menu="space">关系空间</button><button data-menu="wallpaper">更换壁纸</button><button data-menu="avatar">更换头像</button><button data-menu="theme">白间 / 夜间</button><button data-menu="api">API 设置</button><button data-menu="video">视频通话</button><button data-menu="block">${state.blocked?'解除拉黑':'拉黑'}</button></div>`);
$('#nameBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>修改备注</h3><div class="field"><label>备注名</label><input id="remark" value="${esc(state.peerName)}"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveRemark">保存</button></div>`);
$('#avatarBtn').onclick=avatarModal;
function avatarModal(){openModal(`<button class="closex" data-close>×</button><h3>头像设置</h3><div class="field"><label>选择对象</label><select id="avatarWho"><option value="peer">对方头像</option><option value="me">我的头像</option></select></div><div class="field"><label>图片 URL</label><input id="avatarUrl" placeholder="https://..."></div><div class="field"><label>本地图片</label><input id="avatarFile" type="file" accept="image/*"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveAvatar">保存</button></div>`)}
async function saveAvatar(){const who=$('#avatarWho').value,url=$('#avatarUrl').value.trim(),file=$('#avatarFile').files[0];const done=data=>{if(who==='peer')state.peerAvatar=data;else state.myAvatar=data;save();apply();closeModal();toast('头像已保存')};if(file){const r=new FileReader();r.onload=()=>done(r.result);r.readAsDataURL(file)}else done(url)}
function wallpaperModal(){openModal(`<button class="closex" data-close>×</button><h3>更换聊天壁纸</h3><div class="field"><label>图片 URL</label><input id="wallUrl" value="${esc(state.wallpaper)}" placeholder="https://..."></div><div class="field"><label>本地图片</label><input id="wallFile" type="file" accept="image/*"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveWall">保存</button></div>`)}
async function saveWallpaper(){const url=$('#wallUrl').value.trim(),file=$('#wallFile').files[0];const done=data=>{state.wallpaper=data;save();apply();closeModal();toast('壁纸已更新')};if(file){const r=new FileReader();r.onload=()=>done(r.result);r.readAsDataURL(file)}else done(url)}
function relationModal(){openModal(`<button class="closex" data-close>×</button><h3>建立关系</h3><div class="field"><label>关系类型</label><select id="relationSelect"><option>恋人</option><option>朋友</option><option>基友</option><option>闺蜜</option><option>家人</option></select></div><p>选择后会把关系申请写入聊天消息，对方可以点同意或拒绝。</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveRelationPick">发送申请</button></div>`)}
function sendRelation(r){chats.push({id:Date.now(),side:'me',type:'text',time:'刚刚',text:`[关系申请] ${r}`});chats.push({id:Date.now()+1,side:'other',type:'relation',time:'刚刚',relation:r,text:`想和你建立${r}关系。`});save();closeModal();go('chat');toast('关系申请已发送')}
function spaceModal(){if(!state.relation){toast('请先建立关系');return}const moods=state.relationSpace.moods||[];openModal(`<button class="closex" data-close>×</button><h3>关系空间 · ${esc(state.relation)}</h3><div class="dateCard"><b>连续打卡 ${state.relationSpace.days} 天</b><p>最近心情：${esc(moods[0]||'还没有记录')}</p><button class="send" id="checkin">今日打卡</button></div><div class="modalBtns"><button id="mood">随机心情</button><button id="diary">写日记</button></div><div class="divider"></div><div>${state.relationSpace.diary.map((d,i)=>`<div class="log"><b>${esc(d.date)}</b><div>${esc(d.text)}</div><button class="miniBtn" data-del-diary="${i}">删除</button></div>`).join('')}</div><div class="modalBtns"><button id="clearRelation">解除关系并清空空间</button></div>`)}
function checkin(){const d=new Date().toISOString().slice(0,10);if(state.relationSpace.last!==d){state.relationSpace.days++;state.relationSpace.last=d;save();toast('打卡成功')}else toast('今天已经打过卡了');spaceModal()}
function randomMood(){const arr=['安静','想念','疲惫','晴朗','慢慢好起来','需要一点独处','有一点期待'];const mood=arr[Math.floor(Math.random()*arr.length)];state.relationSpace.moods=state.relationSpace.moods||[];state.relationSpace.moods.unshift(mood);state.relationSpace.moods=state.relationSpace.moods.slice(0,20);save();toast('今日心情：'+mood);spaceModal()}
function diaryModal(){openModal(`<button class="closex" data-close>×</button><h3>写日记</h3><div class="field"><textarea id="diaryText" placeholder="留一点字给未来的自己……"></textarea></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveDiary">发布</button></div>`)}
function blockModal(){openModal(`<button class="closex" data-close>×</button><h3>${state.blocked?'解除拉黑':'拉黑联系人'}</h3><p>${state.blocked?'解除后可以恢复正常聊天。':'拉黑后会在聊天中显示拉黑提示，但不会删除历史记录。'}</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="confirmBlock">确认</button></div>`,true)}
function confirmDanger(title,text,ok,fn){openModal(`<button class="closex" data-close>×</button><h3>${esc(title)}</h3><p>${esc(text)}</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="dangerOk">${esc(ok)}</button></div>`,true);const old=$('#dangerOk');old.onclick=fn}

function musicModal(){const s=state.music[state.musicIndex]||defaults.music[0];openModal(`<button class="closex" data-close>×</button><h3>留声机</h3><div class="turntable" style="margin:0"><div class="disc ${state.musicPlaying?'play':''}" id="modalDisc"></div><div class="turnInfo"><b>${esc(s.title)}</b><small>${esc(s.artist)}</small><div class="wave ${state.musicPlaying?'play':''}"><i></i><i></i><i></i><i></i><i></i></div></div></div><div class="playerBtns"><button id="prevMusic">‹‹</button><button id="toggleMusicModal">${state.musicPlaying?'暂停':'播放'}</button><button id="nextMusic">››</button></div><input class="range" id="musicRange" type="range" min="0" max="100" value="0"><div style="font-size:8px;color:#888;text-align:center;margin:4px" id="musicTime">00:00 / 00:00</div><div class="modalBtns"><button id="importMusic">导入音乐</button><button id="clearMusic">清空列表</button></div><div id="musicList"></div>`);renderMusicList();updateMusicRange()}
function renderMusicList(){const box=$('#musicList');if(!box)return;box.innerHTML=state.music.map((m,i)=>`<div class="musicRow"><div class="grow"><b>${esc(m.title)}</b><small>${esc(m.artist)}${m.url?' · 外部直链':''}</small></div><button class="miniBtn" data-pick="${i}">${i===state.musicIndex?'当前':'播放'}</button><button class="miniBtn" data-del="${i}">删</button></div>`).join('')}
function importMusic(){openModal(`<button class="closex" data-close>×</button><h3>导入音乐</h3><div class="field"><label>音乐直链</label><input id="musicUrl" placeholder="https://example.com/song.mp3"></div><div class="field"><label>歌曲名</label><input id="musicTitle" value="new song"></div><div class="field"><label>歌手</label><input id="musicArtist" value="unknown"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveMusicImport">保存</button></div>`)}
async function saveImportedMusic(){const u=$('#musicUrl').value.trim(),t=$('#musicTitle').value.trim()||'new song',a=$('#musicArtist').value.trim()||'unknown';if(!u){toast('请输入音乐直链');return}state.music.push({title:t,artist:a,url:u});state.musicIndex=state.music.length-1;state.musicPlaying=false;save();closeModal();musicModal();toast('音乐已加入播放列表')}
function syncAudio(stop=false){audio.pause();if(stop){audio.currentTime=0;return}const s=state.music[state.musicIndex];if(!s||!s.url){state.musicPlaying=false;return}audio.src=s.url;audio.currentTime=Math.max(0,state.musicTime||0);if(state.musicPlaying)audio.play().catch(()=>{state.musicPlaying=false;apply();toast('该音乐直链无法直接播放')});apply()}
function toggleMusic(){if(state.musicPlaying){state.musicPlaying=false;audio.pause();save();apply();toast('已暂停')}else{const s=state.music[state.musicIndex];if(!s?.url){musicModal();toast('当前歌曲没有外部直链');return}state.musicPlaying=true;audio.play().then(()=>{save();apply();toast('开始播放')}).catch(()=>{state.musicPlaying=false;save();apply();toast('音乐播放失败，请检查直链')})}}
function changeMusic(d){state.musicIndex=(state.musicIndex+d+state.music.length)%state.music.length;state.musicTime=0;state.musicPlaying=!!state.music[state.musicIndex]?.url;save();syncAudio();apply()}
$('#musicBtn').onclick=musicModal;$('#homePlay').onclick=()=>{musicModal();};
audio.addEventListener('timeupdate',()=>{state.musicTime=audio.currentTime;updateMusicRange()});audio.addEventListener('loadedmetadata',updateMusicRange);audio.addEventListener('ended',()=>changeMusic(1));
function updateMusicRange(){const r=$('#musicRange');if(!r)return;const dur=audio.duration||0;r.max=dur||100;r.value=Math.min(audio.currentTime||0,dur||100);const fmt=t=>{t=Math.max(0,Math.floor(t));return String(Math.floor(t/60)).padStart(2,'0')+':'+String(t%60).padStart(2,'0')};const mt=$('#musicTime');if(mt)mt.textContent=fmt(audio.currentTime||0)+' / '+fmt(dur)}
document.addEventListener('input',e=>{if(e.target.id==='musicRange'&&audio.duration){audio.currentTime=Number(e.target.value)}});

function emojiModal(){openModal(`<button class="closex" data-close>×</button><h3>表情指令</h3><div class="field"><input id="emoSearch" placeholder="搜索表情指令，例如：想你"></div><div class="emojiGrid" id="emoGrid"></div>`);fillEmos('');setTimeout(()=>$('#emoSearch')?.focus(),0)}
function fillEmos(q){const arr=['开心','委屈','抱抱','想你','晚安','好困','无语','谢谢','生气','心碎','猫猫','小熊','贴贴','害羞','发呆','叹气','微笑','哭泣','加油','冷淡','沉默','喜欢','依赖','困倦','难过'];const grid=$('#emoGrid');if(!grid)return;grid.innerHTML=arr.filter(x=>x.includes(q)).map(x=>`<button class="emo" data-emo="[${x}]">${x}</button>`).join('')}
$('#emojiBtn').onclick=emojiModal;$('#modal').addEventListener('input',e=>{if(e.target.id==='emoSearch')fillEmos(e.target.value.trim())});

$('#postMomentBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>发布朋友圈</h3><div class="field"><textarea id="momentText" placeholder="写下此刻……"></textarea></div><div class="field"><label>配图 URL，多张用空格分隔</label><input id="momentPics"></div><div class="field"><label>可见范围</label><select id="momentRange"><option>公开</option><option>仅自己</option><option>仅好友</option><option>部分好友</option></select></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="publishMoment">发布</button></div>`);
function publishMoment(){const text=$('#momentText').value.trim();if(!text){toast('请写一点内容');return}const m={name:'我',time:'刚刚',text,pics:$('#momentPics').value.trim().split(/\s+/).filter(Boolean),likes:0,comments:0,range:$('#momentRange').value};state.moments.unshift(m);save();$('#input').value=`[朋友圈] ${text}`;closeModal();renderMoments();toast('朋友圈已发布，指令已填入聊天框')}
function renderMoments(){const arr=[...safeParse($('#momentsTemplate').textContent,[]),...(state.moments||[])];$('#moments').innerHTML=arr.map(m=>`<article class="moment"><div class="momentHead"><img class="avatar" src="${m.name==='我'?(state.myAvatar||avatarSvg('我')):(state.peerAvatar||avatarSvg(m.name))}"><div><b>${esc(m.name)}</b><small>${esc(m.time)} · ${esc(m.range||'公开')}</small></div></div><p>${esc(m.text||'')}</p>${m.pics?.length?`<div class="momentPics">${m.pics.map(p=>`<img src="${esc(p)}" onerror="this.style.display='none'">`).join('')}</div>`:''}<div class="momentActions"><button data-like-moment="${encodeURIComponent(m.text||'')}">赞 ${m.likes||0}</button><button>评论 ${m.comments||0}</button><button>转发</button></div></article>`).join('')}
$('#moments').addEventListener('click',e=>{const b=e.target.closest('[data-like-moment]');if(b){toast('已点赞');b.textContent=b.textContent.replace(/^赞 \d+/,'赞 '+(Number((b.textContent.match(/\d+/)||['0'])[0])+1))}});
$('#addFriendBtn').onclick=()=>openModal(`<button class="closex" data-close>×</button><h3>添加本地联系人</h3><div class="field"><label>姓名 / 网名</label><input id="friendName" placeholder="例如：纸飞机"></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveFriend">添加</button></div>`);
function renderFriends(){$('#friends').innerHTML=[{name:state.peerName,note:state.relation||'聊天联系人',avatar:state.peerAvatar||avatarSvg(state.peerName)},{name:'纸飞机',note:'偶尔说晚安的人'},{name:'白噪音',note:'安静列表里的朋友'},{name:'小小房间',note:'共同群聊'}].map(f=>`<div class="friend"><img class="avatar" src="${f.avatar||avatarSvg(f.name)}"><div class="grow"><b>${esc(f.name)}</b><small>${esc(f.note)}</small></div><button class="miniBtn" data-friend-chat="${esc(f.name)}">聊天</button></div>`).join('')}
$('#friends').addEventListener('click',e=>{const b=e.target.closest('[data-friend-chat]');if(b){go('chat');toast('已打开联系人')}});

function renderLogs(){const a=safeParse($('#logsTemplate').textContent,[]);$('#logs').innerHTML=a.map((x,i)=>`<div class="log"><b>record_${String(i+1).padStart(2,'0')}</b><div>${esc(x)}</div></div>`).join('')}
$('#refreshLogs').onclick=()=>{renderLogs();toast('记录已重新解析')};
$('#homeSettings').onclick=apiModal;
function apiModal(){const api=loadApi();openModal(`<button class="closex" data-close>×</button><h3>API 设置</h3><div class="field"><label>Endpoint</label><input id="apiEndpoint" value="${esc(api.endpoint)}"></div><div class="field"><label>API Key</label><input id="apiKey" type="password" value="${esc(api.key)}"></div><div class="field"><label>Model</label><input id="apiModel" value="${esc(api.model)}"></div><div class="field"><label>System Prompt</label><textarea id="apiSystem">${esc(api.system)}</textarea></div><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveApi">保存</button></div><div class="divider"></div><div class="field"><label>快速测试</label><input id="apiTestInput" value="你好，今天还醒着吗？"></div><button class="send" id="sendApiTest">发送测试请求</button><p>纯前端直连：浏览器会直接请求 Endpoint。API Key 会保存在当前浏览器 localStorage，请不要把长期密钥放在公开网页。</p>`)}
async function callApi(text){const api=loadApi();if(!api.endpoint||!api.key){toast('请先填写 Endpoint 和 API Key');return}toast('正在请求 API');try{const history=chats.filter(x=>x.type==='text').slice(-14).map(x=>({role:x.side==='me'?'user':'assistant',content:x.text}));const fixed={model:api.model||'gpt-4o-mini',messages:[{role:'system',content:api.system||''},...history,{role:'user',content:text}]};
 const headers={'Content-Type':'application/json','Authorization':'Bearer '+api.key};const res=await fetch(api.endpoint,{method:'POST',headers,body:JSON.stringify(fixed)});if(!res.ok)throw new Error('HTTP '+res.status);const data=await res.json();const reply=data.choices?.[0]?.message?.content||data.output_text||JSON.stringify(data);chats.push({id:Date.now(),side:'me',type:'text',time:nowHM(),text:text});chats.push({id:Date.now()+1,side:'other',type:'text',time:nowHM(),text:reply});save();closeModal();go('chat');toast('API 回复已写入聊天')}catch(err){toast('API 请求失败：'+err.message)}}
function videoModal(){openModal(`<button class="closex" data-close>×</button><h3>视频通话</h3><div class="videoBox"><div class="videoLens"></div><p>${esc(state.peerName)} 正在等待接通。</p><div class="modalBtns"><button data-close>取消</button><button class="primary" id="saveVideo">发起通话</button></div></div>`,true)}

function confirmShortcut(){toast('当前操作需要在弹窗中确认')}
function nowHM(){return new Date().toLocaleTimeString('zh-CN',{hour:'2-digit',minute:'2-digit'})}
$('#sendBtn').onclick=()=>sendLocal();
function sendLocal(){const val=$('#input').value.trim();if(!val){toast('输入框还是空的');return}chats.push({id:Date.now(),side:'me',type:'text',time:nowHM(),text:val});$('#input').value='';save();renderChat();toast('消息已发送（本地模拟）');}
$('#input').addEventListener('keydown',e=>{if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendLocal()}});
$$('[data-ins]').forEach(b=>b.onclick=()=>{$('#input').value+=b.dataset.ins;$('#input').focus();toast('已注入快捷标点')});

// 拦截普通聊天卡片上的关闭及拉黑等高危操作，统一二次确认
$('#modal').addEventListener('click',e=>{const menuBlock=e.target.closest('[data-menu="block"]');if(menuBlock){closeModal();confirmDanger(state.blocked?'解除拉黑':'拉黑联系人',state.blocked?'确认恢复正常聊天？':'拉黑后仍保留聊天记录，但对方会被标记为已拉黑。', '确认',()=>{$('#modal').dispatchEvent(new Event('noop'));$('#confirmBlock')?.click()})}});

// 运行时修正“确认拉黑”按钮：主处理器依然生效
// 初始状态
function clock(){const d=new Date();$('#clock').textContent=d.toLocaleTimeString('zh-CN',{hour:'2-digit',minute:'2-digit'});$('#homeDay').textContent=String(d.getDate()).padStart(2,'0');$('#homeMonth').textContent=d.toLocaleDateString('en-US',{month:'short',weekday:'long'}).toUpperCase()}
setInterval(clock,1000);clock();
if(navigator.getBattery){navigator.getBattery().then(b=>{const f=()=>{$('#batteryText').textContent=Math.round(b.level*100)+'%';$('#batteryBar').style.width=(b.level*100)+'%'};f();b.addEventListener('levelchange',f)}).catch(()=>{})}
apply();renderChat();renderMoments();renderFriends();renderLogs();
window.airp={state,chats,save,go,openModal,toast,renderChat};
})();
</script>
</body>
</html>
