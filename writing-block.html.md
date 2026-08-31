```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">

<title>JARVIS // Neural Interface</title>

<style>
*{
    box-sizing:border-box;
    margin:0;
    padding:0;
}

html,body{
    width:100%;
    height:100%;
    overflow:hidden;
    background:#02050b;
    color:#dff7ff;
    font-family:Arial,Helvetica,sans-serif;
}

body{
    user-select:none;
}

/* ================= BACKGROUND ================= */

#space{
    position:fixed;
    inset:0;
    width:100%;
    height:100%;
}

.scanlines{
    position:fixed;
    inset:0;
    pointer-events:none;
    z-index:20;
    opacity:.12;
    background:
        repeating-linear-gradient(
            to bottom,
            transparent 0px,
            transparent 3px,
            rgba(100,210,255,.08) 4px
        );
}

.vignette{
    position:fixed;
    inset:0;
    pointer-events:none;
    z-index:19;
    background:
        radial-gradient(
            circle at center,
            transparent 35%,
            rgba(0,0,0,.65) 100%
        );
}

/* ================= HUD ================= */

.hud{
    position:fixed;
    inset:0;
    z-index:10;
    pointer-events:none;
}

.top{
    position:absolute;
    top:20px;
    left:24px;
    right:24px;

    display:flex;
    justify-content:space-between;
    align-items:flex-start;
}

.brand{
    letter-spacing:7px;
    font-size:14px;
    font-weight:bold;
    color:#9deaff;
}

.sub{
    margin-top:7px;
    font-size:9px;
    letter-spacing:3px;
    color:#47788d;
}

.status{
    text-align:right;
    font-size:9px;
    letter-spacing:2px;
    color:#70dfff;
}

.status-dot{
    display:inline-block;
    width:7px;
    height:7px;
    border-radius:50%;
    background:#48eaff;
    box-shadow:0 0 15px #48eaff;
    margin-right:7px;
}

/* ================= SIDE HUD ================= */

.left-hud,
.right-hud{
    position:absolute;
    top:50%;
    transform:translateY(-50%);
    width:130px;
}

.left-hud{
    left:20px;
}

.right-hud{
    right:20px;
    text-align:right;
}

.hud-label{
    font-size:8px;
    letter-spacing:2px;
    color:#47788d;
    margin-bottom:7px;
}

.hud-value{
    font-size:10px;
    color:#8feaff;
    margin-bottom:20px;
}

.bar{
    width:100%;
    height:2px;
    background:#0a202c;
    margin-top:8px;
    overflow:hidden;
}

.bar i{
    display:block;
    height:100%;
    width:72%;
    background:#48dfff;
    box-shadow:0 0 10px #48dfff;
}

/* ================= CORE ================= */

#core{
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    width:min(75vw,75vh);
    height:min(75vw,75vh);

    transition:transform .15s ease;
}

.core-glow{
    position:absolute;
    inset:18%;
    border-radius:50%;

    background:
        radial-gradient(
            circle,
            rgba(70,220,255,.28),
            rgba(0,120,255,.10) 35%,
            transparent 70%
        );

    filter:blur(18px);

    animation:
        breathing 3s ease-in-out infinite;
}

@keyframes breathing{
    0%,100%{
        transform:scale(.92);
        opacity:.65;
    }
    50%{
        transform:scale(1.08);
        opacity:1;
    }
}

/* Rings */

.ring{
    position:absolute;
    left:50%;
    top:50%;
    border-radius:50%;
    transform:translate(-50%,-50%);
    pointer-events:none;
}

.r1{
    width:100%;
    height:100%;
    border:1px solid rgba(60,210,255,.28);
    animation:spin 18s linear infinite;
}

.r2{
    width:82%;
    height:82%;
    border:1px solid rgba(70,180,255,.20);
    border-left-color:#5ceaff;
    animation:spinReverse 11s linear infinite;
}

.r3{
    width:64%;
    height:64%;
    border:1px dashed rgba(70,220,255,.28);
    animation:spin 8s linear infinite;
}

.r4{
    width:48%;
    height:48%;
    border:2px solid rgba(60,230,255,.18);
    border-top-color:#a5f4ff;
    animation:spinReverse 5s linear infinite;
}

@keyframes spin{
    to{transform:translate(-50%,-50%) rotate(360deg)}
}

@keyframes spinReverse{
    to{transform:translate(-50%,-50%) rotate(-360deg)}
}

/* ================= FACE ================= */

.face{
    position:absolute;
    left:50%;
    top:50%;
    width:43%;
    height:43%;
    transform:translate(-50%,-50%);

    border-radius:50%;

    background:
        radial-gradient(
            circle at 50% 45%,
            rgba(120,240,255,.20),
            rgba(5,25,40,.95) 55%,
            rgba(0,10,18,1)
        );

    border:1px solid rgba(100,230,255,.55);

    box-shadow:
        inset 0 0 35px rgba(50,220,255,.22),
        0 0 35px rgba(30,180,255,.25);

    overflow:hidden;
}

.face::before{
    content:"";
    position:absolute;
    inset:10%;
    border-radius:50%;
    border:1px solid rgba(80,220,255,.2);
    animation:faceRotate 9s linear infinite;
}

@keyframes faceRotate{
    to{transform:rotate(360deg)}
}

/* eyes */

.eye{
    position:absolute;
    top:37%;
    width:25%;
    height:8%;
    background:#a9f5ff;

    box-shadow:
        0 0 8px #75eaff,
        0 0 25px rgba(50,220,255,.8);

    border-radius:100%;

    transition:
        transform .15s ease,
        height .15s ease;
}

.eye.left{
    left:19%;
}

.eye.right{
    right:19%;
}

/* eye pupils */

.eye::after{
    content:"";
    position:absolute;
    left:45%;
    top:25%;
    width:12%;
    height:50%;
    background:#032331;
    border-radius:50%;
}

/* mouth */

.mouth{
    position:absolute;
    left:50%;
    bottom:25%;
    transform:translateX(-50%);

    width:28%;
    height:8px;

    border-bottom:2px solid #78eaff;
    border-radius:50%;

    box-shadow:0 5px 15px rgba(50,220,255,.6);

    animation:talk 2s ease-in-out infinite;
}

@keyframes talk{
    0%,100%{
        height:5px;
    }
    50%{
        height:11px;
    }
}

/* center orb */

.orb{
    position:absolute;
    left:50%;
    top:50%;
    width:15%;
    height:15%;
    transform:translate(-50%,-50%);
    border-radius:50%;

    background:
        radial-gradient(
            circle,
            white 0%,
            #b7f7ff 12%,
            #35dfff 35%,
            #0577b5 60%,
            transparent 72%
        );

    box-shadow:
        0 0 15px #54eaff,
        0 0 50px #009dff,
        0 0 100px rgba(0,150,255,.55);

    animation:orbPulse 1.8s ease-in-out infinite;
}

@keyframes orbPulse{
    50%{
        transform:translate(-50%,-50%) scale(1.2);
    }
}

/* ================= BOTTOM ================= */

.bottom{
    position:absolute;
    left:50%;
    bottom:18px;
    transform:translateX(-50%);
    text-align:center;
    width:90%;
}

.command{
    font-size:9px;
    letter-spacing:3px;
    color:#47788d;
    margin-bottom:9px;
}

.response{
    font-size:11px;
    color:#a6edff;
    min-height:18px;
}

/* ================= CONTROLS ================= */

.controls{
    position:fixed;
    z-index:30;
    right:20px;
    bottom:55px;

    display:flex;
    gap:8px;
}

button{
    border:1px solid rgba(90,220,255,.35);
    background:rgba(3,15,25,.75);
    color:#9deeff;

    padding:10px 13px;

    font-size:9px;
    letter-spacing:1px;

    border-radius:4px;

    backdrop-filter:blur(10px);

    cursor:pointer;

    box-shadow:
        inset 0 0 15px rgba(50,200,255,.04),
        0 0 15px rgba(20,160,255,.08);
}

button:hover{
    background:rgba(20,80,110,.4);
}

/* ================= CHAT ================= */

.chat{
    position:fixed;
    z-index:40;

    right:20px;
    top:70px;

    width:min(330px,85vw);
    height:420px;

    display:none;
    flex-direction:column;

    background:
        linear-gradient(
            145deg,
            rgba(3,18,29,.96),
            rgba(1,7,13,.96)
        );

    border:1px solid rgba(90,220,255,.25);

    box-shadow:
        0 0 40px rgba(0,150,255,.12);

    backdrop-filter:blur(18px);
}

.chat.show{
    display:flex;
}

.chat-head{
    padding:14px;
    border-bottom:1px solid rgba(100,220,255,.12);

    font-size:9px;
    letter-spacing:2px;
}

.messages{
    flex:1;
    overflow:auto;
    padding:12px;
}

.msg{
    margin-bottom:12px;
    font-size:11px;
    line-height:1.5;
}

.msg.ai{
    color:#83eaff;
}

.msg.user{
    color:#fff;
    text-align:right;
}

.chat-input{
    display:flex;
    border-top:1px solid rgba(100,220,255,.12);
}

.chat-input input{
    flex:1;
    border:0;
    outline:0;
    background:transparent;
    color:white;
    padding:13px;
    font-size:11px;
}

.chat-input button{
    border:0;
    border-left:1px solid rgba(100,220,255,.15);
}

/* ================= CAMERA ================= */

.camera{
    position:fixed;
    z-index:35;

    left:20px;
    bottom:55px;

    width:180px;
    height:130px;

    display:none;

    overflow:hidden;

    border:1px solid rgba(80,220,255,.25);
    background:#02070c;
}

.camera.show{
    display:block;
}

.camera video{
    width:100%;
    height:100%;
    object-fit:cover;
    transform:scaleX(-1);
}

.camera-label{
    position:absolute;
    left:7px;
    top:7px;
    font-size:8px;
    letter-spacing:2px;
    color:#8feaff;
}

/* mobile */

@media(max-width:600px){

    .top{
        top:14px;
        left:15px;
        right:15px;
    }

    .brand{
        font-size:11px;
        letter-spacing:4px;
    }

    .left-hud,
    .right-hud{
        display:none;
    }

    #core{
        width:90vw;
        height:90vw;
    }

    .controls{
        right:10px;
        bottom:65px;
    }

    button{
        padding:9px 10px;
    }

    .camera{
        width:130px;
        height:95px;
        left:10px;
        bottom:65px;
    }
}
</style>
</head>

<body>

<canvas id="space"></canvas>

<div class="scanlines"></div>
<div class="vignette"></div>

<div class="hud">

    <div class="top">

        <div>
            <div class="brand">J.A.R.V.I.S</div>
            <div class="sub">NEURAL INTERFACE // MK-X</div>
        </div>

        <div class="status">
            <span class="status-dot"></span>
            SYSTEM ONLINE<br>
            <span id="clock">00:00:00</span>
        </div>

    </div>

    <div class="left-hud">

        <div class="hud-label">NEURAL LOAD</div>
        <div class="hud-value">72%</div>
        <div class="bar"><i></i></div>

        <div class="hud-label" style="margin-top:20px">
            VISION
        </div>

        <div class="hud-value" id="visionState">
            STANDBY
        </div>

        <div class="hud-label">
            VOICE
        </div>

        <div class="hud-value" id="voiceState">
            READY
        </div>

    </div>

    <div class="right-hud">

        <div class="hud-label">CORE TEMP</div>
        <div class="hud-value">36.7°C</div>

        <div class="hud-label">MEMORY</div>
        <div class="hud-value">ONLINE</div>

        <div class="hud-label">GESTURE</div>
        <div class="hud-value" id="gestureState">
            WAITING
        </div>

    </div>

</div>

<!-- CORE -->

<div id="core">

    <div class="core-glow"></div>

    <div class="ring r1"></div>
    <div class="ring r2"></div>
    <div class="ring r3"></div>
    <div class="ring r4"></div>

    <div class="face">

        <div class="eye left"></div>
        <div class="eye right"></div>

        <div class="mouth"></div>

        <div class="orb"></div>

    </div>

</div>

<!-- BOTTOM -->

<div class="bottom">

    <div class="command">
        JARVIS NEURAL CORE
    </div>

    <div class="response" id="response">
        Awaiting command...
    </div>

</div>

<!-- BUTTONS -->

<div class="controls">

    <button id="voiceBtn">
        🎙 VOICE
    </button>

    <button id="visionBtn">
        👁 VISION
    </button>

    <button id="chatBtn">
        CHAT
    </button>

</div>

<!-- CAMERA -->

<div class="camera" id="cameraBox">

    <video id="video"
           autoplay
           playsinline>
    </video>

    <div class="camera-label">
        VISION // LIVE
    </div>

</div>

<!-- CHAT -->

<div class="chat" id="chat">

    <div class="chat-head">
        J.A.R.V.I.S // NEURAL CHANNEL
    </div>

    <div class="messages" id="messages">

        <div class="msg ai">
            JARVIS online. Awaiting your command.
        </div>

    </div>

    <div class="chat-input">

        <input
            id="input"
            placeholder="Talk to JARVIS..."
            autocomplete="off"
        >

        <button id="send">
            SEND
        </button>

    </div>

</div>


<script>

/* ========================================================
   PARTICLE SPACE
======================================================== */

const canvas =
    document.getElementById("space");

const ctx =
    canvas.getContext("2d");

let W,H;

let particles=[];

function resize(){

    W=canvas.width=innerWidth;
    H=canvas.height=innerHeight;

}

resize();

window.addEventListener(
    "resize",
    resize
);

for(let i=0;i<180;i++){

    particles.push({

        x:Math.random(),
        y:Math.random(),

        z:Math.random(),

        speed:
            .00015+
            Math.random()*.0006,

        size:
            .5+
            Math.random()*1.8

    });

}

function drawSpace(){

    ctx.clearRect(
        0,
        0,
        W,
        H
    );

    for(const p of particles){

        p.y -= p.speed;

        if(p.y<0)
            p.y=1;

        const x=p.x*W;
        const y=p.y*H;

        const a=.15+
            p.z*.55;

        ctx.beginPath();

        ctx.arc(
            x,
            y,
            p.size,
            0,
            Math.PI*2
        );

        ctx.fillStyle=
            `rgba(90,210,255,${a})`;

        ctx.fill();

    }

    requestAnimationFrame(
        drawSpace
    );
}

drawSpace();


/* ========================================================
   CLOCK
======================================================== */

function updateClock(){

    const d=new Date();

    document.getElementById(
        "clock"
    ).textContent=
        d.toLocaleTimeString();

}

setInterval(
    updateClock,
    1000
);

updateClock();


/* ========================================================
   CORE ZOOM
======================================================== */

const core=
    document.getElementById("core");

let zoom=1;

let startDistance=null;

function distance(a,b){

    return Math.hypot(
        a.clientX-b.clientX,
        a.clientY-b.clientY
    );

}

document.addEventListener(
    "touchmove",
    e=>{

        if(e.touches.length===2){

            const d=
                distance(
                    e.touches[0],
                    e.touches[1]
                );

            if(startDistance===null)
                startDistance=d;

            const diff=
                (d-startDistance)/400;

            zoom+=diff;

            zoom=
                Math.max(
                    .75,
                    Math.min(
                        1.7,
                        zoom
                    )
                );

            core.style.transform=
                `translate(-50%,-50%) scale(${zoom})`;

            startDistance=d;

        }

    },
    {passive:true}
);

document.addEventListener(
    "touchend",
    ()=>{

        startDistance=null;

    }
);


/* ========================================================
   VOICE RECOGNITION
======================================================== */

const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition=null;

if(SpeechRecognition){

    recognition=
        new SpeechRecognition();

    recognition.continuous=false;

    recognition.interimResults=false;

    recognition.lang="en-US";

    recognition.onstart=()=>{

        document.getElementById(
            "voiceState"
        ).textContent="LISTENING";

        setResponse(
            "Listening..."
        );

    };

    recognition.onresult=e=>{

        const text=
            e.results[0][0].transcript;

        document.getElementById(
            "input"
        ).value=text;

        sendMessage(text);

    };

    recognition.onend=()=>{

        document.getElementById(
            "voiceState"
        ).textContent="READY";

    };

    recognition.onerror=()=>{

        document.getElementById(
            "voiceState"
        ).textContent="ERROR";

    };

}


/* ========================================================
   VOICE BUTTON
======================================================== */

document
.getElementById("voiceBtn")
.addEventListener(
    "click",
    ()=>{

        if(!recognition){

            setResponse(
                "Speech recognition is not supported by this browser."
            );

            return;

        }

        recognition.start();

    }
);


/* ========================================================
   SPEECH OUTPUT
======================================================== */

function speak(text){

    if(!("speechSynthesis" in window))
        return;

    speechSynthesis.cancel();

    const u=
        new SpeechSynthesisUtterance(
            text
        );

    u.rate=.95;
    u.pitch=.8;
    u.volume=1;

    speechSynthesis.speak(u);

}


/* ========================================================
   RESPONSE
======================================================== */

function setResponse(text){

    document.getElementById(
        "response"
    ).textContent=text;

}


/* ========================================================
   CHAT
======================================================== */

const chat=
    document.getElementById("chat");

document
.getElementById("chatBtn")
.addEventListener(
    "click",
    ()=>{

        chat.classList.toggle(
            "show"
        );

    }
);

function addMessage(
    text,
    type
){

    const div=
        document.createElement(
            "div"
        );

    div.className=
        "msg "+type;

    div.textContent=text;

    document
        .getElementById("messages")
        .appendChild(div);

    const box=
        document.getElementById(
            "messages"
        );

    box.scrollTop=
        box.scrollHeight;

}


/* ========================================================
   REAL AI CONNECTION
======================================================== */

/*
   IMPORTANT:

   Put your own AI endpoint in this function.

   Example:

   const response =
       await fetch("YOUR_AI_ENDPOINT", {
           method:"POST",
           headers:{
               "Content-Type":"application/json"
           },
           body:JSON.stringify({
               message:text
           })
       });

   const data=await response.json();

   return data.answer;

   DO NOT put a secret API key directly into
   a public HTML website.
*/

async function askRealAI(text){

    /*
       PLACE YOUR BACKEND/AI ENDPOINT HERE.

       The UI works without it.

       Returning this message keeps the interface
       functional until a real AI provider is connected.
    */

    return (
        "I received your command: " +
        text +
        ". My neural AI connection is ready to be connected."
    );

}


/* ========================================================
   SEND MESSAGE
======================================================== */

async function sendMessage(text){

    text=text.trim();

    if(!text)
        return;

    addMessage(
        text,
        "user"
    );

    setResponse(
        "Processing neural command..."
    );

    const answer=
        await askRealAI(text);

    addMessage(
        answer,
        "ai"
    );

    setResponse(
        answer
    );

    speak(answer);

}


/* ========================================================
   SEND BUTTON
======================================================== */

document
.getElementById("send")
.addEventListener(
    "click",
    ()=>{

        sendMessage(
            document
            .getElementById("input")
            .value
        );

        document
            .getElementById("input")
            .value="";

    }
);


/* ========================================================
   ENTER KEY
======================================================== */

document
.getElementById("input")
.addEventListener(
    "keydown",
    e=>{

        if(e.key==="Enter"){

            sendMessage(
                e.target.value
            );

            e.target.value="";

        }

    }
);


/* ========================================================
   CAMERA / VISION
======================================================== */

const video=
    document.getElementById("video");

const cameraBox=
    document.getElementById(
        "cameraBox"
    );

let cameraStream=null;

document
.getElementById("visionBtn")
.addEventListener(
    "click",
    async()=>{

        if(cameraStream){

            cameraStream
                .getTracks()
                .forEach(
                    track=>
                        track.stop()
                );

            cameraStream=null;

            cameraBox
                .classList
                .remove("show");

            document.getElementById(
                "visionState"
            ).textContent="STANDBY";

            return;

        }

        try{

            cameraStream=
                await navigator
                .mediaDevices
                .getUserMedia({

                    video:{
                        facingMode:
                            "user"
                    },

                    audio:false

                });

            video.srcObject=
                cameraStream;

            cameraBox
                .classList
                .add("show");

            document.getElementById(
                "visionState"
            ).textContent="LIVE";

            setResponse(
                "Vision system online."
            );

        }

        catch(err){

            setResponse(
                "Camera permission was denied or unavailable."
            );

        }

    }
);


/* ========================================================
   POINTER GESTURES
======================================================== */

let pointerStart=null;

document.addEventListener(
    "pointerdown",
    e=>{

        pointerStart={
            x:e.clientX,
            y:e.clientY
        };

    }
);

document.addEventListener(
    "pointerup",
    e=>{

        if(!pointerStart)
            return;

        const dy=
            e.clientY-
            pointerStart.y;

        if(Math.abs(dy)>100){

            if(dy<0){

                document.getElementById(
                    "gestureState"
                ).textContent=
                    "SWIPE UP";

                setResponse(
                    "Gesture detected: swipe up."
                );

            }else{

                document.getElementById(
                    "gestureState"
                ).textContent=
                    "SWIPE DOWN";

                setResponse(
                    "Gesture detected: swipe down."
                );

            }

            setTimeout(
                ()=>{
                    document.getElementById(
                        "gestureState"
                    ).textContent=
                        "WAITING";
                },
                1200
            );

        }

        pointerStart=null;

    }
);


/* ========================================================
   EYE REACTION
======================================================== */

document.addEventListener(
    "pointermove",
    e=>{

        const x=
            (e.clientX/
             innerWidth-
             .5)*20;

        const y=
            (e.clientY/
             innerHeight-
             .5)*12;

        document
            .querySelectorAll(".eye")
            .forEach(
                eye=>{

                    eye.style.transform=
                        `translate(${x}px,${y}px)`;

                }
            );

    }
);


/* ========================================================
   STARTUP
======================================================== */

setTimeout(
    ()=>{

        setResponse(
            "Neural core online. Awaiting command."
        );

        speak(
            "Jarvis systems online."
        );

    },
    1200
);

</script>

</body>
</html>
```