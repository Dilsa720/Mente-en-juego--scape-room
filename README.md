[index.html.html](https://github.com/user-attachments/files/31033589/index.html.html)
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mente en Juego — Escape Room</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root{
      --bg1:#120d2d;
      --bg2:#1d1b4b;
      --cyan:#48e5ff;
      --pink:#ff4fa3;
      --lime:#d7ff63;
      --amber:#ffd166;
    }
    body{
      background:
        radial-gradient(circle at 20% 20%, rgba(72,229,255,.12), transparent 28%),
        radial-gradient(circle at 80% 15%, rgba(255,79,163,.12), transparent 24%),
        linear-gradient(135deg,var(--bg1),var(--bg2));
      min-height:100vh;
      color:#fff;
      overflow-x:hidden;
    }
    .glass{
      background:rgba(255,255,255,.08);
      border:1px solid rgba(255,255,255,.13);
      box-shadow:0 20px 60px rgba(0,0,0,.28);
      backdrop-filter:blur(16px);
    }
    .pulse-ring{animation:pulseRing 1.6s infinite}
    @keyframes pulseRing{
      0%{box-shadow:0 0 0 0 rgba(72,229,255,.45)}
      70%{box-shadow:0 0 0 18px rgba(72,229,255,0)}
      100%{box-shadow:0 0 0 0 rgba(72,229,255,0)}
    }
    .shake{animation:shake .35s}
    @keyframes shake{
      0%,100%{transform:translateX(0)}
      25%{transform:translateX(-8px)}
      50%{transform:translateX(8px)}
      75%{transform:translateX(-5px)}
    }
    .success-pop{animation:successPop .55s ease}
    @keyframes successPop{
      0%{transform:scale(.85);opacity:.4}
      60%{transform:scale(1.04);opacity:1}
      100%{transform:scale(1)}
    }
    .fade-in{animation:fadeIn .45s ease}
    @keyframes fadeIn{
      from{opacity:0;transform:translateY(10px)}
      to{opacity:1;transform:translateY(0)}
    }
    .choice:hover{transform:translateY(-2px)}
    .progress-glow{box-shadow:0 0 18px rgba(72,229,255,.5)}
  </style>
</head>
<body class="font-sans">
  <audio id="sndOk" preload="auto">
    <source src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAESsAACJWAAACABAAZGF0YQAAAAA=" type="audio/wav">
  </audio>
  <audio id="sndBad" preload="auto">
    <source src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAESsAACJWAAACABAAZGF0YQAAAAA=" type="audio/wav">
  </audio>

  <div class="max-w-6xl mx-auto px-4 py-5 md:py-8">
    <header class="glass rounded-3xl p-4 md:p-6 mb-5">
      <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
        <div>
          <div class="text-xs uppercase tracking-[.3em] text-cyan-300 mb-1">Escape Room Educativo</div>
          <h1 class="text-3xl md:text-5xl font-black">🎮 MENTE EN JUEGO</h1>
          <p class="text-white/70 mt-2">Decisiones bajo presión · Nivel adolescente</p>
        </div>

        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-center">
          <div class="glass rounded-2xl px-4 py-3">
            <div class="text-xs text-white/60">Tiempo</div>
            <div id="timer" class="text-xl font-black text-yellow-300">15:00</div>
          </div>
          <div class="glass rounded-2xl px-4 py-3">
            <div class="text-xs text-white/60">Puntos</div>
            <div id="score" class="text-xl font-black text-lime-300">100</div>
          </div>
          <div class="glass rounded-2xl px-4 py-3">
            <div class="text-xs text-white/60">Pistas</div>
            <div id="hintsUsed" class="text-xl font-black text-pink-300">0</div>
          </div>
          <div class="glass rounded-2xl px-4 py-3">
            <div class="text-xs text-white/60">Escena</div>
            <div id="roomCounter" class="text-xl font-black text-cyan-300">1/6</div>
          </div>
        </div>
      </div>

      <div class="mt-5">
        <div class="flex justify-between text-xs text-white/60 mb-2">
          <span>Progreso</span>
          <span id="progressText">0%</span>
        </div>
        <div class="w-full bg-white/10 rounded-full h-3 overflow-hidden">
          <div id="progressBar" class="h-3 rounded-full bg-cyan-300 progress-glow transition-all duration-500" style="width:0%"></div>
        </div>
      </div>
    </header>

    <main id="app" class="glass rounded-3xl p-5 md:p-8 min-h-[560px] fade-in"></main>

    <footer class="text-center text-xs text-white/40 mt-5">
      Experiencia educativa de reflexión. No realiza diagnósticos psicológicos.
    </footer>
  </div>

<script>
const rooms = [
{
  title:"Habitación 1 · ¿Quién soy cuando nadie mira?",
  icon:"🪞",
  narrative:"Una pared de espejos se enciende. Cada espejo muestra una versión distinta de ti: la que ven tus amigos, la de redes sociales y la que solo tú conoces.",
  question:"¿Qué opción refleja mejor un autoconcepto saludable?",
  options:[
    "Valgo solo si otros me aprueban.",
    "Puedo reconocer fortalezas y aspectos que aún estoy desarrollando.",
    "Debo ser igual al grupo para sentirme seguro.",
    "Si fallo en algo, significa que soy un fracaso."
  ],
  correct:1,
  concept:"Autoconcepto: una imagen equilibrada de uno mismo integra fortalezas, límites y posibilidad de cambio.",
  hint:"Busca la opción que no dependa totalmente de la aprobación externa."
},
{
  title:"Habitación 2 · Todos lo hacen",
  icon:"👥",
  narrative:"Tu teléfono vibra. El grupo insiste en que hagas algo que no te convence. La puerta solo se abrirá si identificas una respuesta asertiva.",
  question:"¿Cuál respuesta protege mejor tus límites sin atacar a los demás?",
  options:[
    "Voy aunque no quiera.",
    "Me invento una excusa para evitar problemas.",
    "No quiero hacerlo. Gracias, pero prefiero no participar.",
    "Los bloqueo a todos y no vuelvo a hablarles."
  ],
  correct:2,
  concept:"Asertividad: expresar una decisión con claridad y respeto, sin someterse ni atacar.",
  hint:"La respuesta más fuerte no siempre es la más agresiva; busca claridad + respeto."
},
{
  title:"Habitación 3 · Mi vida vs. Instagram",
  icon:"📱",
  narrative:"Una pantalla gigante proyecta vidas perfectas: viajes, cuerpos ideales, éxitos instantáneos. El sistema comienza a comparar tu vida con esas imágenes.",
  question:"¿Qué pensamiento ayuda más a reducir una comparación social dañina?",
  options:[
    "Si no vivo así, mi vida está mal.",
    "En redes veo una selección de momentos, no la historia completa.",
    "Tengo que competir con todos.",
    "La cantidad de likes define mi valor."
  ],
  correct:1,
  concept:"Comparación social: las redes suelen mostrar versiones seleccionadas de la realidad. Cuestionar esa comparación protege la autoestima.",
  hint:"Recuerda: lo que ves publicado no equivale a toda la vida de una persona."
},
{
  title:"Habitación 4 · ¿Exploto o respondo?",
  icon:"🔥",
  narrative:"Una alarma roja empieza a sonar. Recibes un mensaje que te molesta mucho. Solo tienes unos segundos antes de responder.",
  question:"¿Qué secuencia ayuda más a regular una reacción impulsiva?",
  options:[
    "Leer → responder inmediatamente → pensar después.",
    "Pausar → identificar lo que siento → decidir cómo responder.",
    "Ignorar todo lo que siento.",
    "Responder más fuerte para ganar."
  ],
  correct:1,
  concept:"Regulación emocional: sentir una emoción no obliga a actuar inmediatamente. La pausa crea espacio para elegir una respuesta.",
  hint:"Busca una secuencia donde primero aparezca una pausa consciente."
},
{
  title:"Habitación 5 · Demasiado encima",
  icon:"⏳",
  narrative:"Tareas, mensajes, responsabilidades y compromisos comienzan a aparecer en todas las pantallas. El sistema marca: SOBRECARGA.",
  question:"¿Qué estrategia suele ser más útil frente a una carga intensa de pendientes?",
  options:[
    "Intentar resolverlo todo al mismo tiempo.",
    "No dormir para terminar más rápido.",
    "Priorizar, dividir tareas y pedir apoyo cuando sea necesario.",
    "Evitar revisar lo pendiente."
  ],
  correct:2,
  concept:"Manejo del estrés: priorizar, dividir tareas y buscar apoyo puede reducir la sensación de descontrol.",
  hint:"La respuesta más efectiva convierte un problema grande en partes manejables."
},
{
  title:"Habitación 6 · El Espejo Final",
  icon:"🧠",
  narrative:"Has llegado al centro de Mente en Juego. El espejo final no pregunta quién eres. Pregunta cómo decides cuando existe presión.",
  question:"¿Cuál principio resume mejor lo aprendido?",
  options:[
    "La mejor decisión siempre complace a todos.",
    "Decidir bien significa no sentir miedo ni dudas.",
    "No controlo todas las situaciones, pero puedo elegir cómo responder.",
    "Una buena decisión nunca tiene consecuencias difíciles."
  ],
  correct:2,
  concept:"Toma de decisiones: decidir conscientemente implica reconocer emociones, consecuencias, valores y límites, incluso cuando no existe una opción perfecta.",
  hint:"Piensa en lo único que realmente permanece bajo tu control: tu respuesta."
}
];

let state = JSON.parse(localStorage.getItem("menteJuegoState")) || {
  current:0, score:100, hints:0, solved:Array(rooms.length).fill(false), remaining:900, started:false, finished:false, answers:[]
};

let timerInterval;

function save(){
  localStorage.setItem("menteJuegoState", JSON.stringify(state));
}

function beep(ok=true){
  try{
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain); gain.connect(ctx.destination);
    osc.frequency.value = ok ? 660 : 180;
    gain.gain.setValueAtTime(.12, ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(.001, ctx.currentTime + .18);
    osc.start(); osc.stop(ctx.currentTime + .18);
  }catch(e){}
}

function updateHUD(){
  document.getElementById("timer").textContent = formatTime(state.remaining);
  document.getElementById("score").textContent = state.score;
  document.getElementById("hintsUsed").textContent = state.hints;
  document.getElementById("roomCounter").textContent = `${Math.min(state.current+1,rooms.length)}/${rooms.length}`;
  const done = state.solved.filter(Boolean).length;
  const pct = Math.round(done/rooms.length*100);
  document.getElementById("progressBar").style.width = pct+"%";
  document.getElementById("progressText").textContent = pct+"%";
}

function formatTime(s){
  const m=Math.floor(s/60).toString().padStart(2,"0");
  const sec=(s%60).toString().padStart(2,"0");
  return `${m}:${sec}`;
}

function startTimer(){
  if(timerInterval) clearInterval(timerInterval);
  timerInterval=setInterval(()=>{
    if(!state.started || state.finished) return;
    state.remaining--;
    if(state.remaining<=0){
      state.remaining=0;
      clearInterval(timerInterval);
      save();
      renderTimeOut();
      return;
    }
    save(); updateHUD();
  },1000);
}

function startGame(){
  state.started=true;
  save();
  startTimer();
  renderRoom();
}

function renderIntro(){
  document.getElementById("app").innerHTML = `
    <section class="max-w-3xl mx-auto text-center py-8">
      <div class="text-6xl md:text-8xl mb-5 pulse-ring inline-block rounded-full p-5">🧠</div>
      <div class="text-cyan-300 uppercase tracking-[.25em] text-xs mb-3">Acceso al sistema</div>
      <h2 class="text-3xl md:text-5xl font-black mb-5">Cada decisión deja una huella.</h2>
      <p class="text-base md:text-xl text-white/75 leading-relaxed">
        Has entrado a <strong>Mente en Juego</strong>, un espacio donde tus elecciones alteran lo que ocurre después.
        Tendrás 15 minutos para superar seis habitaciones. No buscas respuestas perfectas: buscas comprender cómo pensar bajo presión.
      </p>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3 mt-8 text-sm">
        <div class="glass p-4 rounded-2xl">🧠 Bienestar</div>
        <div class="glass p-4 rounded-2xl">🤝 Relaciones</div>
        <div class="glass p-4 rounded-2xl">🎯 Metas</div>
        <div class="glass p-4 rounded-2xl">💬 Comunicación</div>
      </div>
      <button onclick="startGame()" class="mt-8 bg-cyan-300 text-slate-950 font-black px-7 py-4 rounded-2xl hover:scale-105 transition">
        ENTRAR A MENTE EN JUEGO
      </button>
      ${state.started ? `<button onclick="resetGame()" class="block mx-auto mt-4 text-sm text-white/50 underline">Reiniciar progreso</button>`:""}
    </section>`;
}

function renderRoom(){
  if(state.finished){renderFinal();return;}
  const r=rooms[state.current];
  const used = state.solved[state.current];
  document.getElementById("app").innerHTML = `
    <section class="max-w-4xl mx-auto fade-in">
      <div class="flex items-start gap-4 mb-6">
        <div class="text-5xl bg-white/10 rounded-2xl p-4">${r.icon}</div>
        <div>
          <div class="text-xs uppercase tracking-[.2em] text-pink-300">Escena desbloqueada</div>
          <h2 class="text-2xl md:text-4xl font-black">${r.title}</h2>
        </div>
      </div>

      <div class="glass rounded-2xl p-5 mb-6 border-l-4 border-cyan-300">
        <p class="text-white/80 leading-relaxed">${r.narrative}</p>
      </div>

      <h3 class="text-lg md:text-2xl font-bold mb-4">${r.question}</h3>
      <div id="choices" class="space-y-3">
        ${r.options.map((o,i)=>`
          <button onclick="answer(${i})" class="choice w-full text-left glass rounded-2xl p-4 md:p-5 transition border border-white/10 hover:border-cyan-300">
            <span class="font-black text-cyan-300 mr-2">${String.fromCharCode(65+i)}.</span>${o}
          </button>`).join("")}
      </div>

      <div id="feedback" class="mt-5"></div>

      <div class="mt-6 flex flex-col sm:flex-row gap-3 justify-between items-center">
        <button onclick="useHint()" class="w-full sm:w-auto bg-pink-500/20 border border-pink-300/30 text-pink-200 font-bold px-5 py-3 rounded-2xl hover:bg-pink-500/30 transition">
          💡 Usar pista (-5 puntos)
        </button>
        <div id="hintBox" class="text-sm text-yellow-200"></div>
      </div>
    </section>`;
}

function answer(i){
  const r=rooms[state.current];
  const fb=document.getElementById("feedback");
  if(i===r.correct){
    beep(true);
    if(!state.solved[state.current]){
      state.solved[state.current]=true;
      state.answers[state.current]=i;
    }
    fb.innerHTML=`
      <div class="success-pop rounded-2xl p-5 bg-lime-300/15 border border-lime-300/40">
        <div class="text-lime-300 font-black text-lg">✓ Has desbloqueado la siguiente escena</div>
        <p class="mt-2 text-white/80">${r.concept}</p>
        <button onclick="nextRoom()" class="mt-4 bg-lime-300 text-slate-950 font-black px-5 py-3 rounded-xl">Continuar →</button>
      </div>`;
    save(); updateHUD();
  } else {
    beep(false);
    state.score=Math.max(0,state.score-3);
    state.answers[state.current]=i;
    fb.innerHTML=`
      <div class="shake rounded-2xl p-5 bg-red-500/15 border border-red-400/30">
        <div class="text-red-300 font-black">Esa decisión tiene riesgos. Intenta analizarla otra vez.</div>
        <p class="mt-2 text-white/70">Piensa en límites, consecuencias y capacidad de responder sin actuar por impulso.</p>
      </div>`;
    save(); updateHUD();
  }
}

function useHint(){
  const r=rooms[state.current];
  state.hints++;
  state.score=Math.max(0,state.score-5);
  document.getElementById("hintBox").textContent="Pista: "+r.hint;
  beep(true);
  save(); updateHUD();
}

function nextRoom(){
  if(state.current < rooms.length-1){
    state.current++;
    save();
    renderRoom();
    updateHUD();
  } else {
    state.finished=true;
    clearInterval(timerInterval);
    save();
    renderFinal();
  }
}

function profile(){
  const score=state.score;
  if(score>=90) return ["Estratega Reflexivo","Analizas consecuencias, mantienes límites y tomas decisiones con intención.","Seguir practicando decisiones rápidas sin perder reflexión."];
  if(score>=75) return ["Comunicador","Tiendes a reconocer el valor de expresar límites y buscar respuestas claras.","Fortalecer la regulación emocional en momentos de alta presión."];
  if(score>=60) return ["Mediador","Buscas equilibrio entre tus necesidades y las relaciones con otros.","Evitar sacrificar demasiado tus propios límites por mantener armonía."];
  return ["Explorador","Aprendes probando opciones y observando sus consecuencias.","Detenerte un poco más antes de actuar y revisar posibles impactos."];
}

function renderFinal(){
  state.finished=true; save(); clearInterval(timerInterval);
  const elapsed=900-state.remaining;
  const p=profile();
  document.getElementById("app").innerHTML=`
    <section class="max-w-4xl mx-auto text-center py-7 fade-in">
      <div class="text-7xl mb-4">🏆</div>
      <div class="uppercase tracking-[.25em] text-xs text-lime-300 mb-2">Escape completado</div>
      <h2 class="text-3xl md:text-5xl font-black">Has salido de Mente en Juego</h2>
      <p class="mt-4 text-white/70 max-w-2xl mx-auto">
        No controlas todas las situaciones que aparecerán en tu vida, pero puedes aprender a elegir cómo responder ante ellas.
      </p>

      <div class="grid md:grid-cols-3 gap-4 mt-8">
        <div class="glass p-5 rounded-2xl"><div class="text-white/50 text-sm">Puntuación</div><div class="text-3xl font-black text-lime-300">${state.score}/100</div></div>
        <div class="glass p-5 rounded-2xl"><div class="text-white/50 text-sm">Tiempo usado</div><div class="text-3xl font-black text-cyan-300">${formatTime(elapsed)}</div></div>
        <div class="glass p-5 rounded-2xl"><div class="text-white/50 text-sm">Pistas utilizadas</div><div class="text-3xl font-black text-pink-300">${state.hints}</div></div>
      </div>

      <div class="glass rounded-3xl p-6 mt-7 text-left">
        <div class="text-xs uppercase tracking-[.2em] text-yellow-300">Tu perfil de juego</div>
        <h3 class="text-3xl font-black mt-1">${p[0]}</h3>
        <p class="mt-3 text-white/80"><strong>Fortaleza observada:</strong> ${p[1]}</p>
        <p class="mt-2 text-white/70"><strong>Reto para seguir creciendo:</strong> ${p[2]}</p>
        <p class="mt-4 text-xs text-white/40">Este perfil es una dinámica educativa y no constituye una evaluación o diagnóstico psicológico.</p>
      </div>

      <button onclick="resetGame()" class="mt-7 bg-cyan-300 text-slate-950 font-black px-6 py-3 rounded-2xl">Jugar nuevamente</button>
    </section>`;
  updateHUD();
}

function renderTimeOut(){
  document.getElementById("app").innerHTML=`
    <section class="max-w-2xl mx-auto text-center py-12">
      <div class="text-7xl mb-5">⏰</div>
      <h2 class="text-4xl font-black">El tiempo terminó</h2>
      <p class="mt-4 text-white/70">Mente en Juego ha cerrado sus puertas por ahora. Puedes reiniciar y volver a intentar el recorrido.</p>
      <button onclick="resetGame()" class="mt-7 bg-yellow-300 text-slate-950 font-black px-6 py-3 rounded-2xl">Reiniciar misión</button>
    </section>`;
}

function resetGame(){
  state={current:0, score:100, hints:0, solved:Array(rooms.length).fill(false), remaining:900, started:false, finished:false, answers:[]};
  localStorage.removeItem("menteJuegoState");
  clearInterval(timerInterval);
  updateHUD();
  renderIntro();
}

updateHUD();
if(state.finished) renderFinal();
else if(state.started){ renderRoom(); startTimer(); }
else renderIntro();
</script>
</body>
</html>
