# SafeCampus
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Juegos: Entornos Universitarios Seguros y Igualdad de Género</title>
  <style>
    :root{
      --bg:#f9fafb;
      --card:#ffffff;
      --accent:#0077cc;
      --muted:#6b7280;
      --success:#22c55e;
      --danger:#ef4444;
    }

    html,body{
      height:100%;
      margin:0;
      font-family:Inter,system-ui,Segoe UI,Roboto,Arial;
      background:#f3f4f6;
      color:#1f2937;
    }

    .wrap{max-width:1100px;margin:28px auto;padding:22px}
    header{display:flex;align-items:center;gap:16px;margin-bottom:16px}
    header h1{font-size:22px;margin:0;font-weight:800;background:linear-gradient(90deg,#d946ef,#0ea5e9);-webkit-background-clip:text;color:transparent;}

    .grid{display:grid;grid-template-columns:1fr 360px;gap:18px}
    .card{background:#ffffff;border-radius:12px;padding:16px;box-shadow:0 2px 10px rgba(0,0,0,0.08)}

    nav{display:flex;gap:8px;flex-wrap:wrap}
    button.tab{background:#e5e7eb;border:1px solid #cbd5e1;padding:8px 12px;border-radius:8px;color:#374151;cursor:pointer}
    button.tab.active{background:linear-gradient(90deg,#d946ef,#0ea5e9);color:white;border:none;}

    .game{padding:8px 4px}
    .hidden{display:none}

    .question{font-weight:600;margin-bottom:8px;color:#1f2937}
    .choices{display:flex;flex-direction:column;gap:8px}
    .choice{background:#f9fafb;border:1px solid #d1d5db;padding:10px;border-radius:8px;cursor:pointer;color:#1f2937}
    .choice.correct{border-color:var(--success);background:#ecfdf5}
    .choice.wrong{border-color:var(--danger);background:#fef2f2}

    footer{margin-top:16px;font-size:13px;color:var(--muted)}
    .score{font-size:18px;font-weight:700}

    aside .panel{display:flex;flex-direction:column;gap:12px}
    .scenario{background:#f9fafb;padding:12px;border-radius:8px;border:1px solid #e5e7eb}
    .option{display:block;padding:8px;margin-top:6px;border-radius:8px;border:1px solid #d1d5db;background:#ffffff;color:#1f2937}
    .result{padding:10px;border-radius:8px;background:#eef2ff;border:1px solid #c7d2fe;color:#1e3a8a}

    .badge{display:inline-block;padding:6px 8px;border-radius:999px;background:#e0f2fe;color:#0369a1;font-size:12px}
    .small{font-size:13px;color:#374151}
    .center{text-align:center}
    .btn{background:linear-gradient(90deg,#0ea5e9,#6366f1);border:none;color:white;padding:8px 12px;border-radius:10px;cursor:pointer;font-weight:700;box-shadow:0 2px 6px rgba(0,0,0,0.15);}
    .muted{color:var(--muted)}

    .progress{height:10px;background:#e5e7eb;border-radius:999px;overflow:hidden}
    .bar{height:100%;background:linear-gradient(90deg,#d946ef,#0ea5e9);width:0%;}

    @media (max-width:980px){.grid{grid-template-columns:1fr;}.wrap{padding:12px}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='48' height='48'><rect rx='10' width='48' height='48' fill='%2306b6d4'/><text x='24' y='30' font-size='20' text-anchor='middle' font-family='Arial' fill='white'>U</text></svg>" alt="logo" style="width:48px;height:48px;border-radius:10px;"/>
      <div>
        <h1>EN BUSCA DE UN CAMPUS SEGURO</h1>
      <p class="tagline" style="margin:0;font-size:14px;font-weight:600;background:linear-gradient(90deg,#f472b6,#38bdf8);-webkit-background-clip:text;color:transparent;">Aprende, juega y construye un campus seguro e igualitario</p>
      
        <div class="small">Safe Campus**.</div>
      </div>
    </header>

    <div class="grid">
      <main class="card">
        <nav>
          <button class="tab active" data-target="quiz">Quiz rápido</button>
          <button class="tab" data-target="scenario">Rol: Escenarios</button>
          <button class="tab" data-target="bystander">Intervención activa</button>
        </nav>

        <!-- Quiz -->
        <section id="quiz" class="game">
          <h2>Quiz: identifica comportamientos seguros</h2>
          <p class="small">Responde correctamente para aprender pautas prácticas.</p>
          <div id="quiz-area">
            <div id="q-text" class="question">Cargando...</div>
            <div id="q-choices" class="choices"></div>
            <div style="display:flex;gap:8px;align-items:center;margin-top:12px">
              <button id="next-q" class="btn" disabled>Siguiente</button>
              <div class="muted" id="q-feedback"></div>
              <div style="margin-left:auto" class="small">Puntaje: <span id="q-score">0</span></div>
            </div>
          </div>
        </section>

        <!-- Scenario -->
        <section id="scenario" class="game hidden">
          <h2>Rol: tomar decisiones en un escenario</h2>
          <p class="small">Lee la situación y elige la acción que tomarías. Luego verás retroalimentación educativa.</p>
          <div id="scenario-area"></div>
          <div style="margin-top:12px;display:flex;gap:8px;align-items:center">
            <button id="next-scenario" class="btn" style="display:none">Siguiente escenario</button>
            <div id="scenario-feedback" class="muted"></div>
          </div>
        </section>

        <!-- Bystander -->
        <section id="bystander" class="game hidden">
          <h2>Juego: Bystander rápido</h2>
          <p class="small">Entrena una intervención segura en 30 segundos. Selecciona una acción eficaz antes de que acabe el tiempo.</p>
          <div class="center">
            <div class="progress" style="margin:12px 0"><div id="timer-bar" class="bar"></div></div>
            <div id="bystander-situation" class="question">Preparando...</div>
            <div id="bystander-options" class="choices" style="margin-top:10px"></div>
            <div style="margin-top:12px"><button id="start-bystander" class="btn">Iniciar</button> <button id="reset-bystander" class="btn" style="display:none">Reintentar</button></div>
            <div id="bystander-result" style="margin-top:10px"></div>
          </div>
        </section>

        <footer>
          <div class="small">Objetivo: fomentar comportamientos seguros, prácticas de reporte, y habilidades de apoyo. Este material es educativo — adaptenlo a la normativa local de su institución.</div>
        </footer>
      </main>

      <aside class="card panel">
        <div>
          <div style="display:flex;gap:8px;align-items:center">
            <div class="badge">Guía rápida</div>
            <div class="small">Consejos clave</div>
          </div>
          <ul class="small" style="margin-top:8px">
            <li>Prioriza la seguridad física: aléjate si hay peligro inmediato.</li>
            <li>Apoya a la persona afectada: escucha, cree, ofrece acompañamiento.</li>
            <li>Reporta según la normativa institucional y ofrece pruebas (si existen).</li>
            <li>Usa lenguaje inclusivo y evita culpar a la víctima.</li>
          </ul>
        </div>

        <div class="scenario">
          <strong>Recursos</strong>
          <div class="small" style="margin-top:6px">Incluye: protocolos de reporte, números de contacto de apoyo, enlaces a oficinas de igualdad de género.</div>
        </div>

        <div class="scenario center">
          <div class="small">¿Quieres modificar preguntas o escenarios? Puedo personalizarlos para tu universidad.</div>
          <div style="margin-top:8px"><button id="customize" class="btn">Personalizar</button></div>
        </div>
      </aside>
    </div>
  </div>

  <script>
    // ======= DATA =======
    const quizQuestions = [
      {q: 'En tu entorno educativo, un estudiante insiste en seguir a una compañera por el campus haciendo comentarios sobre su cuerpo. Ella expresa incomodidad. ¿Qué harías?',
       choices: ['Ignorar porque “solo está bromeando”','Separarte del agresor, ofrecer apoyo a la compañera y sugerir acudir a la Oficina de Bienestar Universitario','Publicarlo en redes para denunciar','Hablarle al agresor de manera agresiva frente a todos'],
       answer: 1,
       explain: 'La tu entorno educativo cuenta con canales de apoyo institucional; acompañar y orientar a la víctima es clave.'},

      {q: 'Ves en un grupo de WhatsApp de tu aula un meme ofensivo dirigido a una estudiante. ¿Qué acción es más responsable?',
       choices: ['Reír para no quedar mal','Reportarlo al docente tutor o Bienestar Universitario y avisar a la afectada','Responder con otro meme','Ignorar el tema'],
       answer: 1,
       explain: 'Reportar y acompañar evita la normalización del acoso digital.'},

      {q: 'En un trabajo grupal, un estudiante hace comentarios de “broma” sobre el rol de las mujeres en el equipo. ¿Cuál es la mejor respuesta?',
       choices: ['Corregirlo con humor y recordarle que los comentarios sexistas no van','Reír para evitar tensión','Ignorar','Responder de forma agresiva'],
       answer: 0,
       explain: 'Corregir con firmeza pero sin agresión promueve un ambiente inclusivo y seguro.'}
    ];

    const scenarios = [
      {text: 'En tu entorno educativo, durante un cambio de clase, escuchas a un docente hacer comentarios sobre la ropa de una estudiante. Ella parece incómoda. ¿Qué harías?',
       options: [
         {t:'Hablar con la estudiante, validar lo que siente y acompañarla a Bienestar Universitario', score:2, feedback:'Excelente: prioriza apoyo y canal institucional.'},
         {t:'Ignorar porque “así es su manera de hablar”.', score:0, feedback:'Normalizar perpetúa la conducta.'},
         {t:'Confrontar al docente frente a todos.', score:1, feedback:'Puede exponer a la estudiante; mejor apoyo privado y reporte.'}
       ]},

      {text: 'En una práctica grupal, un compañero dice que una estudiante “exagera” cuando se queja de comentarios incómodos. ¿Qué respondes?',
       options: [
         {t:'Recordar que nadie exagera al sentirse incómodo y ofrecer recursos institucionales.', score:2, feedback:'Ayuda a desmontar prejuicios y apoyar a la víctima.'},
         {t:'Reír para encajar con el grupo.', score:0, feedback:'Refuerza la minimización del acoso.'},
         {t:'Cambiar de tema sin corregir.', score:1, feedback:'Mejor intervenir con una corrección breve.'}
       ]}
    ];

    const bystanderSituations = [
      {s: 'Ves a un estudiante sujetando a otra persona contra su voluntad en la biblioteca.', opts: ['Pedir ayuda a seguridad y acercarte para crear distracción','Grabarlos con el móvil y subirlo','No hacer nada'] , correct:0, explain:'Llamar a seguridad y crear una distracción es una intervención segura.'},
      {s: 'Una persona insulta constantemente a otra en el pasillo; la víctima se aleja triste.', opts: ['Acompañarla, preguntarle si está bien y ofrecer apoyo','Unirte a los insultos para no destacar','Publicar sobre el incidente en redes'] , correct:0, explain:'Acompañar y validar ayuda a la recuperación y a que la víctima reporte si quiere.'}
    ];

    // ======= TAB NAV =======
    document.querySelectorAll('.tab').forEach(btn=>btn.addEventListener('click', e=>{
      document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      const t = btn.dataset.target;
      document.querySelectorAll('.game').forEach(g=>g.classList.add('hidden'));
      document.getElementById(t).classList.remove('hidden');
    }));

    // ======= QUIZ LOGIC =======
    let qi=0; let qscore=0; let answered=false;
    const qText = document.getElementById('q-text');
    const qChoices = document.getElementById('q-choices');
    const qFeedback = document.getElementById('q-feedback');
    const qScore = document.getElementById('q-score');
    const nextQ = document.getElementById('next-q');

    function renderQuestion(){
      const q = quizQuestions[qi];
      qText.textContent = q.q;
      qChoices.innerHTML = '';
      q.choices.forEach((c,i)=>{
        const btn = document.createElement('button');
        btn.className='choice'; btn.textContent = c; btn.dataset.index = i;
        btn.addEventListener('click', onChoose);
        qChoices.appendChild(btn);
      });
      qFeedback.textContent=''; nextQ.disabled=true; answered=false;
    }
    function onChoose(e){
      if(answered) return; answered=true;
      const idx = Number(e.currentTarget.dataset.index);
      const q = quizQuestions[qi];
      Array.from(qChoices.children).forEach((ch,ci)=>{
        ch.classList.remove('correct','wrong');
        if(ci===q.answer) ch.classList.add('correct');
        if(ci===idx && ci!==q.answer) ch.classList.add('wrong');
        ch.disabled=true;
      });
      if(idx===q.answer){ qscore+=1; qFeedback.textContent = 'Correcto — ' + q.explain; }
      else { qFeedback.textContent = 'Incorrecto — ' + q.explain; }
      qScore.textContent = qscore;
      nextQ.disabled=false;
    }
    nextQ.addEventListener('click', ()=>{
      qi = (qi+1) % quizQuestions.length;
      if(qi===0){ // end of round: offer restart
        qFeedback.textContent = '¡Has completado el quiz! Puntaje: '+qscore+'/' + quizQuestions.length + '. Se reiniciará.';
        qscore=0; qScore.textContent=qscore;
      }
      renderQuestion();
    });
    renderQuestion();

    // ======= SCENARIO LOGIC =======
    let si=0; const scenarioArea = document.getElementById('scenario-area'); const scenarioFeedback = document.getElementById('scenario-feedback'); const nextScenario = document.getElementById('next-scenario');
    function renderScenario(){
      scenarioArea.innerHTML = '';
      const s = scenarios[si];
      const p = document.createElement('div'); p.className='question'; p.textContent = s.text; scenarioArea.appendChild(p);
      s.options.forEach((o,idx)=>{
        const b = document.createElement('button'); b.className='option'; b.textContent = o.t; b.dataset.idx = idx; b.addEventListener('click', ()=>{
          // show feedback
          scenarioFeedback.textContent = o.feedback;
          scenarioFeedback.className = o.score===2 ? 'result' : 'result';
          nextScenario.style.display='inline-block';
        });
        scenarioArea.appendChild(b);
      });
      nextScenario.style.display='none'; scenarioFeedback.textContent='';
    }
    nextScenario.addEventListener('click', ()=>{
      si = (si+1) % scenarios.length; renderScenario();
    });
    renderScenario();

    // ======= BYSTANDER GAME =======
    let bt = null; let timer = null; let bsIndex = 0;
    const byStart = document.getElementById('start-bystander'); const byReset = document.getElementById('reset-bystander'); const bySituation = document.getElementById('bystander-situation'); const byOptions = document.getElementById('bystander-options'); const timerBar = document.getElementById('timer-bar'); const byResult = document.getElementById('bystander-result');
    function prepBystander(){
      bsIndex = Math.floor(Math.random()*bystanderSituations.length);
      const s = bystanderSituations[bsIndex]; bySituation.textContent = s.s; byOptions.innerHTML='';
      s.opts.forEach((o,i)=>{ const b=document.createElement('button'); b.className='choice'; b.textContent=o; b.dataset.i=i; b.addEventListener('click', ()=>selectBystander(i)); byOptions.appendChild(b); });
      timerBar.style.width='0%'; byResult.textContent=''; byReset.style.display='none';
    }
    function startBystander(){
      let time=30; timerBar.style.width='100%';
      byStart.style.display='none';
      // progress shrink
      timer = setInterval(()=>{
        time-=0.5; const pct = Math.max(0, (time/30)*100); timerBar.style.width = pct+'%';
        if(time<=0){ clearInterval(timer); timeoutBystander(); }
      },500);
    }
    function selectBystander(choice){
      clearInterval(timer);
      const s = bystanderSituations[bsIndex];
      Array.from(byOptions.children).forEach((c,ci)=>c.disabled=true);
      if(choice===s.correct){ byResult.innerHTML = '<div class="result" style="background:rgba(16,185,129,0.08);border:1px solid rgba(16,185,129,0.12);">Acción eficaz: '+s.explain+'</div>'; }
      else { byResult.innerHTML = '<div class="result" style="background:rgba(239,68,68,0.06);border:1px solid rgba(239,68,68,0.08);">Acción no recomendada: '+s.explain+'</div>'; }
      byReset.style.display='inline-block';
    }
    function timeoutBystander(){
      Array.from(byOptions.children).forEach(c=>c.disabled=true);
      byResult.innerHTML = '<div class="result" style="background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.04);">Tiempo agotado. Recuerda: busca ayuda inmediata y prioriza seguridad.</div>';
      byReset.style.display='inline-block';
    }
    byStart.addEventListener('click', ()=>{ startBystander(); });
    byReset.addEventListener('click', ()=>{ prepBystander(); byStart.style.display='inline-block'; byReset.style.display='none'; });
    prepBystander();

    // ======= CUSTOMIZE BUTTON =======
    document.getElementById('customize').addEventListener('click', ()=>{
      const edit = prompt('Introduce un nuevo escenario breve (texto) que quieras añadir al juego:');
      if(edit && edit.trim().length>10){
        scenarios.push({text: edit.trim(), options:[{t:'Ofrecer apoyo y ayudar a reportar', score:2, feedback:'Muy bien — ofrecer apoyo y acompañamiento es efectivo.'},{t:'Ignorar', score:0, feedback:'No intervendr deja sola a la persona.'},{t:'Publicar en redes', score:0, feedback:'No es recomendable exponer a la víctima.'}]});
        alert('Escenario añadido. Cambia a la pestaña "Rol: Escenarios" y usa "Siguiente" para verlo.');
      }
    });

    // Accessibility: keyboard navigation for choices (simple)
    document.addEventListener('keydown', (e)=>{
      if(e.key==='Escape'){ // reset first game
        qi=0; qscore=0; qScore.textContent=0; renderQuestion();
      }
    });
  </script>
</body>
</html>
