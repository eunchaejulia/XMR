<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>PROTOCOL: LINGUA | STATION_77</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');
        body { background: #000; color: #00FF41; font-family: 'VT323', monospace; overflow: hidden; }
        .crt::before { content: " "; display: block; position: absolute; top: 0; left: 0; bottom: 0; right: 0; background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06)); z-index: 100; background-size: 100% 2px, 3px 100%; pointer-events: none; }
        .scanline { width: 100%; height: 2px; background: rgba(0, 255, 65, 0.1); position: absolute; top: 0; z-index: 101; animation: scanline 8s linear infinite; }
        @keyframes scanline { 0% { top: 0; } 100% { top: 100%; } }
        .glitch-red { animation: shake 0.2s infinite; color: #ff0000 !important; }
        @keyframes shake { 0% { transform: translate(0,0); } 20% { transform: translate(-2px, 1px); } 100% { transform: translate(2px, -1px); } }
    </style>
</head>
<body class="crt p-4">
    <div class="scanline"></div>
    <header class="border border-[#00FF41] p-3 mb-4 flex justify-between items-center">
        <div>XMR: $<span id="xmr-price">240.00</span> | OXYGEN: <span id="oxy">100</span>%</div>
        <div class="text-right">CASH: <span id="cash">500</span>$ | XMR: <span id="xmr-val">0</span></div>
    </header>
    <div id="logs" class="h-[60vh] border border-[#00FF41] p-4 overflow-y-auto mb-4 bg-black/50 text-xl space-y-2"></div>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-2">
        <input type="text" id="in" class="md:col-span-2 bg-transparent border border-[#00FF41] p-2 outline-none text-[#00FF41]" placeholder="메시지 입력..." autofocus>
        <button onclick="trade('buy')" class="border border-[#00FF41] hover:bg-[#00FF41] hover:text-black">BUY XMR (100$)</button>
        <button onclick="trade('sell')" class="border border-[#00FF41] hover:bg-[#00FF41] hover:text-black">SELL ALL</button>
    </div>
    <script>
        let cash=500, xmr=0, oxy=100, price=240;
        const input=document.getElementById('in'), logBox=document.getElementById('logs');
        function tr(t){ return t.split('').map(c=>String.fromCharCode(c.charCodeAt(0)+500)).join('')+"on"; }
        input.onkeypress=(e)=>{
            if(e.key==='Enter'&&input.value){
                const div=document.createElement('div');
                const origin=input.value;
                div.innerHTML=`<span class="opacity-30">[${new Date().toLocaleTimeString()}]</span> <span class="cursor-pointer" onclick="dec(this,'${origin}')">${tr(origin)}</span>`;
                logBox.appendChild(div); logBox.scrollTop=logBox.scrollHeight;
                input.value=''; oxy=Math.min(100,oxy+5); update();
            }
        };
        function dec(el,o){ if(xmr>=0.01){ xmr-=0.01; el.innerText=">> "+o; el.style.color="white"; update(); } else alert("XMR 부족!"); }
        function trade(t){ if(t==='buy'&&cash>=100){ cash-=100; xmr+=100/price; } else if(t==='sell'){ cash+=xmr*price; xmr=0; } update(); }
        function update(){ document.getElementById('cash').innerText=cash.toFixed(0); document.getElementById('xmr-val').innerText=xmr.toFixed(3); document.getElementById('oxy').innerText=oxy.toFixed(1); document.getElementById('xmr-price').innerText=price.toFixed(2); }
        setInterval(()=>{ oxy=Math.max(0,oxy-0.2); price+=(Math.random()-0.5)*2; if(oxy<20)document.body.classList.add('glitch-red'); else document.body.classList.remove('glitch-red'); update(); },2000);
    </script>
</body>
</html>
