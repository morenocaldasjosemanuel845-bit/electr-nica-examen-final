<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Banco 1 - Quiz de 20 preguntas</title>
  <style>
    :root{
      --bg:#0f172a;
      --card:#111827;
      --card2:#1f2937;
      --text:#e5e7eb;
      --muted:#94a3b8;
      --accent:#38bdf8;
      --ok:#22c55e;
      --bad:#ef4444;
      --warn:#f59e0b;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:Arial, Helvetica, sans-serif;
      background:linear-gradient(135deg,#0f172a,#1e293b);
      color:var(--text);
      min-height:100vh;
    }
    .container{
      max-width:1000px;
      margin:0 auto;
      padding:24px;
    }
    .card{
      background:rgba(17,24,39,.92);
      border:1px solid rgba(255,255,255,.08);
      border-radius:20px;
      padding:24px;
      box-shadow:0 20px 40px rgba(0,0,0,.25);
    }
    h1,h2,h3,p{margin-top:0}
    .top{
      display:flex;
      gap:16px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:space-between;
      margin-bottom:18px;
    }
    .badge{
      background:#0ea5e915;
      border:1px solid #38bdf840;
      color:#bae6fd;
      padding:8px 12px;
      border-radius:999px;
      font-weight:bold;
      font-size:14px;
    }
    .stats{
      display:grid;
      grid-template-columns:repeat(4,1fr);
      gap:12px;
      margin:18px 0 24px;
    }
    .stat{
      background:var(--card2);
      border-radius:16px;
      padding:14px;
      border:1px solid rgba(255,255,255,.07);
    }
    .stat span{
      display:block;
      color:var(--muted);
      font-size:13px;
      margin-bottom:6px;
    }
    .progress-box{
      margin:20px 0;
    }
    .progress{
      width:100%;
      height:12px;
      background:#334155;
      border-radius:999px;
      overflow:hidden;
    }
    .progress-bar{
      width:0%;
      height:100%;
      background:linear-gradient(90deg,var(--ok),var(--accent));
      transition:width .3s ease;
    }
    .question-box{
      margin-top:20px;
      display:none;
    }
    .question-number{
      color:var(--accent);
      font-weight:bold;
      margin-bottom:10px;
    }
    .question{
      font-size:24px;
      line-height:1.4;
      margin-bottom:20px;
    }
    .options{
      display:grid;
      gap:12px;
    }
    .option{
      width:100%;
      text-align:left;
      background:var(--card2);
      border:1px solid rgba(255,255,255,.08);
      color:var(--text);
      padding:16px;
      border-radius:14px;
      cursor:pointer;
      font-size:16px;
      transition:.2s;
    }
    .option:hover{
      transform:translateY(-1px);
      border-color:#38bdf880;
    }
    .option.selected{
      border:2px solid var(--accent);
      background:#0ea5e920;
    }
    .option.correct{
      background:#15803d30;
      border:2px solid var(--ok);
    }
    .option.wrong{
      background:#b91c1c30;
      border:2px solid var(--bad);
    }
    .actions{
      display:flex;
      gap:12px;
      flex-wrap:wrap;
      margin-top:20px;
    }
    button.main-btn{
      background:linear-gradient(135deg,#0ea5e9,#2563eb);
      color:white;
      border:none;
      padding:14px 18px;
      border-radius:12px;
      cursor:pointer;
      font-weight:bold;
      font-size:15px;
    }
    button.secondary-btn{
      background:#334155;
      color:white;
      border:none;
      padding:14px 18px;
      border-radius:12px;
      cursor:pointer;
      font-weight:bold;
      font-size:15px;
    }
    .feedback{
      margin-top:18px;
      padding:16px;
      border-radius:14px;
      display:none;
      line-height:1.6;
    }
    .feedback.ok{
      background:#14532d55;
      border:1px solid #22c55e88;
    }
    .feedback.bad{
      background:#7f1d1d55;
      border:1px solid #ef444488;
    }
    .start-screen, .result-screen{
      display:block;
    }
    .result-screen{
      display:none;
      margin-top:20px;
    }
    .review-item{
      margin-top:12px;
      background:var(--card2);
      border:1px solid rgba(255,255,255,.08);
      border-radius:14px;
      padding:14px;
    }
    .small{
      color:var(--muted);
      font-size:14px;
    }
    @media (max-width: 700px){
      .stats{
        grid-template-columns:repeat(2,1fr);
      }
      .question{
        font-size:20px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <div class="start-screen" id="startScreen">
        <div class="top">
          <div>
            <h1>Banco 1 · Quiz interactivo</h1>
            <p>Estas son las 20 preguntas basadas en lo que resolvimos juntos. El sistema te dirá si acertaste y te explicará la respuesta correcta cuando te equivoques.</p>
          </div>
          <div class="badge">20 preguntas reales</div>
        </div>

        <div class="stats">
          <div class="stat"><span>Temas</span><strong>Buck-Boost, PFC, AC/AC, SVC, PWM</strong></div>
          <div class="stat"><span>Modo</span><strong>Interactivo</strong></div>
          <div class="stat"><span>Feedback</span><strong>Inmediato</strong></div>
          <div class="stat"><span>Objetivo</span><strong>Repasar Banco 1</strong></div>
        </div>

        <button class="main-btn" onclick="startQuiz()">Empezar quiz</button>
      </div>

      <div class="question-box" id="questionBox">
        <div class="progress-box">
          <div class="small" id="progressText">Pregunta 1 de 20</div>
          <div class="progress">
            <div class="progress-bar" id="progressBar"></div>
          </div>
        </div>

        <div class="top">
          <div class="badge" id="topicBadge">Tema</div>
          <div class="badge" id="scoreBadge">Puntaje: 0</div>
        </div>

        <div class="question-number" id="questionNumber">Pregunta 1</div>
        <div class="question" id="questionText"></div>
        <div class="options" id="optionsContainer"></div>

        <div class="feedback" id="feedbackBox"></div>

        <div class="actions">
          <button class="main-btn" id="checkBtn" onclick="checkAnswer()">Revisar respuesta</button>
          <button class="secondary-btn" id="nextBtn" onclick="nextQuestion()" style="display:none;">Siguiente</button>
          <button class="secondary-btn" onclick="restartQuiz()">Reiniciar</button>
        </div>
      </div>

      <div class="result-screen" id="resultScreen">
        <h2>Resultado final</h2>
        <p id="finalScore"></p>
        <p id="finalMessage"></p>
        <h3>Preguntas que fallaste</h3>
        <div id="reviewContainer"></div>
        <div class="actions">
          <button class="main-btn" onclick="restartQuiz()">Volver a jugar</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    const questions = [
      {
        topic: "Buck-Boost",
        question: "En un convertidor Buck-Boost, ¿qué ocurre en el estado ON y qué ocurre en el estado OFF?",
        options: [
          "En ON el inductor almacena energía y el diodo bloquea; en OFF el inductor entrega energía a la carga y el diodo conduce.",
          "En ON el capacitor se descarga y en OFF el inductor deja de trabajar.",
          "En ON el diodo conduce y en OFF el inductor desaparece.",
          "En ON y OFF ocurre exactamente lo mismo."
        ],
        answer: 0,
        explanation: "La idea correcta es: en ON el inductor almacena energía y el diodo queda bloqueado; en OFF el inductor libera energía hacia la carga y el diodo conduce."
      },
      {
        topic: "Buck-Boost",
        question: "¿Cuál es la diferencia entre CCM y DCM en el Buck-Boost?",
        options: [
          "CCM y DCM son lo mismo.",
          "En CCM la corriente del inductor nunca llega a cero; en DCM sí llega a cero.",
          "En DCM la corriente siempre es mayor.",
          "En CCM no existe inductor."
        ],
        answer: 1,
        explanation: "CCM es conducción continua: la corriente del inductor no llega a cero. DCM es conducción discontinua: sí llega a cero durante parte del período."
      },
      {
        topic: "Buck-Boost",
        question: "¿Cuál es la variable principal de control que permite subir o bajar la tensión de salida en el Buck-Boost?",
        options: [
          "La temperatura",
          "La frecuencia de la red",
          "El ciclo de trabajo D",
          "La resistencia interna del diodo"
        ],
        answer: 2,
        explanation: "La variable principal es el duty cycle o ciclo de trabajo D. Variándolo, el convertidor puede elevar o reducir la tensión de salida."
      },
      {
        topic: "PFC",
        question: "¿Cuál es la función principal del PFC y qué forma debe tener idealmente la corriente de entrada?",
        options: [
          "Solo regular una tensión DC y tener corriente cuadrada.",
          "Aumentar la frecuencia y tener corriente triangular.",
          "Apagar armónicos sin importar la fase.",
          "Mejorar el factor de potencia; la corriente debe ser senoidal y estar en fase con la tensión."
        ],
        answer: 3,
        explanation: "La etapa PFC busca mejorar el factor de potencia y reducir la distorsión. Idealmente la corriente de entrada debe ser senoidal y en fase con la tensión."
      },
      {
        topic: "PFC",
        question: "¿Qué es el THD y qué consecuencias tiene que sea alto?",
        options: [
          "Es la Distorsión Armónica Total; si es alto produce más pérdidas, calentamiento y peor factor de potencia.",
          "Es un error de temperatura que no afecta al sistema.",
          "Es una tensión de referencia positiva.",
          "Es un tipo de transformador."
        ],
        answer: 0,
        explanation: "THD significa Distorsión Armónica Total. Un THD alto implica más armónicos, más pérdidas, calentamiento y menor calidad de energía."
      },
      {
        topic: "AC/AC",
        question: "En un regulador AC/AC, ¿qué pasa con el valor RMS de salida cuando el ángulo de disparo α aumenta?",
        options: [
          "Sube el RMS y mejora el factor de potencia.",
          "Baja el RMS, sube el THD y empeora el factor de potencia.",
          "La frecuencia aumenta automáticamente.",
          "No ocurre ningún cambio."
        ],
        answer: 1,
        explanation: "Más ángulo de disparo implica menos tiempo de conducción, menor RMS, más distorsión armónica y peor factor de potencia."
      },
      {
        topic: "AC/AC",
        question: "En un regulador AC/AC, ¿qué significa que sea una conversión directa?",
        options: [
          "Que convierte AC a AC sin enlace DC intermedio.",
          "Que siempre aumenta el voltaje.",
          "Que necesita batería externa.",
          "Que usa un motor para controlar la señal."
        ],
        answer: 0,
        explanation: "Conversión directa significa que pasa de AC a AC sin etapa DC intermedia."
      },
      {
        topic: "AC/AC",
        question: "En una carga RL conectada a un regulador AC/AC, ¿qué pasa con la conducción de corriente cerca del cruce por cero?",
        options: [
          "La corriente siempre se corta exactamente en cero.",
          "La conducción se prolonga más allá del cruce por cero por efecto del inductor.",
          "La resistencia elimina la corriente por completo.",
          "La corriente se vuelve DC."
        ],
        answer: 1,
        explanation: "En cargas RL la inductancia hace que la corriente no se anule instantáneamente, así que la conducción continúa después del cruce por cero."
      },
      {
        topic: "AC/AC",
        question: "¿Qué es el burst control o control por ciclos completos en un regulador AC/AC?",
        options: [
          "Disparar siempre a 180°.",
          "Cambiar la frecuencia de la red.",
          "Dejar pasar ciclos completos y bloquear otros ciclos completos.",
          "Variar solo el ancho de pulso como en PWM."
        ],
        answer: 2,
        explanation: "El burst control consiste en permitir ciclos completos y bloquear otros, regulando la potencia de forma distinta al ángulo de fase."
      },
      {
        topic: "Cicloconvertidor",
        question: "¿Qué es un cicloconvertidor y cuál es su principal característica respecto a la frecuencia de salida?",
        options: [
          "Es un rectificador DC/AC con frecuencia infinita.",
          "Es un AC/AC directo cuya frecuencia de salida normalmente es menor que la de entrada.",
          "Es un convertidor que solo trabaja con batería.",
          "Es un sistema que siempre duplica la frecuencia."
        ],
        answer: 1,
        explanation: "El cicloconvertidor es un AC/AC directo que construye una salida de frecuencia variable, normalmente menor que la de entrada."
      },
      {
        topic: "Cicloconvertidor",
        question: "¿En qué tipo de aplicaciones se usa típicamente el cicloconvertidor?",
        options: [
          "En equipos de muy alta potencia y baja velocidad, como grandes accionamientos industriales.",
          "En calculadoras escolares.",
          "Solo en luces LED domésticas.",
          "Únicamente en teléfonos móviles."
        ],
        answer: 0,
        explanation: "Se usa en aplicaciones de muy alta potencia y baja velocidad, por ejemplo grandes motores, molinos o propulsión."
      },
      {
        topic: "SVC",
        question: "En un SVC, ¿cuál es su función principal dentro del sistema eléctrico?",
        options: [
          "Cambiar la frecuencia del sistema.",
          "Rectificar AC a DC.",
          "Compensar potencia reactiva para regular tensión y mejorar el factor de potencia.",
          "Eliminar transformadores."
        ],
        answer: 2,
        explanation: "El SVC compensa potencia reactiva para sostener tensión y mejorar el factor de potencia del sistema."
      },
      {
        topic: "SVC",
        question: "¿Qué significa TCR dentro de un SVC?",
        options: [
          "Transformador de Corriente Reactiva",
          "Terminal de Control Rápido",
          "Total Compensation Reactor",
          "Thyristor Controlled Reactor"
        ],
        answer: 3,
        explanation: "TCR significa Thyristor Controlled Reactor, es decir, reactor controlado por tiristores."
      },
      {
        topic: "SVC",
        question: "En un TCR, ¿qué ocurre cuando el ángulo de disparo α se acerca a 180°?",
        options: [
          "El reactor casi no conduce y absorbe muy poca reactiva.",
          "La corriente se vuelve infinita.",
          "Se convierte en capacitor.",
          "La red cambia de frecuencia."
        ],
        answer: 0,
        explanation: "Cuando α se acerca a 180°, la conducción del reactor se reduce casi a cero."
      },
      {
        topic: "AC/DC PWM",
        question: "En un sistema AC/DC moderno con PWM, ¿qué hace el lazo externo y qué hace el lazo interno?",
        options: [
          "El externo regula Vdc y el interno regula o moldea la corriente.",
          "El externo controla el ángulo alfa y el interno la temperatura.",
          "Ambos hacen exactamente lo mismo.",
          "El externo apaga la red y el interno prende el capacitor."
        ],
        answer: 0,
        explanation: "En el control de doble lazo, el lazo externo regula la tensión del bus DC y el lazo interno controla la corriente."
      },
      {
        topic: "AC/DC PWM",
        question: "En ese mismo sistema, ¿cuál es la variable manipulada por el controlador?",
        options: [
          "La reactancia de línea",
          "El duty cycle d del PWM",
          "El ángulo de disparo de SCR",
          "La temperatura del cable"
        ],
        answer: 1,
        explanation: "En un sistema PWM moderno, la variable manipulada es el duty cycle, no el ángulo de disparo de SCR."
      },
      {
        topic: "AC/DC PWM",
        question: "¿Qué significa PWM y qué idea básica hay detrás de este método de control?",
        options: [
          "Power Wave Method; mide ondas de potencia.",
          "Pulse Width Modulation; variar el ancho de pulso para controlar la salida.",
          "Phase Wiring Mode; cambiar fases manualmente.",
          "Pulse Watt Meter; medir vatios."
        ],
        answer: 1,
        explanation: "PWM significa modulación por ancho de pulso. Consiste en variar el tiempo de encendido dentro de un período fijo."
      },
      {
        topic: "AC/DC PWM",
        question: "¿Cuál es el objetivo principal de un rectificador PWM trifásico respecto a la red de entrada?",
        options: [
          "Solo producir calor en la red.",
          "Tener Vdc estable, corrientes senoidales y factor de potencia cercano a 1.",
          "Trabajar únicamente con SCR.",
          "Eliminar cualquier control."
        ],
        answer: 1,
        explanation: "El rectificador PWM trifásico busca tomar energía con buena calidad: bajo THD, FP alto y bus DC estable."
      },
      {
        topic: "AC/DC PWM",
        question: "¿Qué ventaja tiene usar interleaving en convertidores de potencia?",
        options: [
          "Aumentar el ruido y el rizado.",
          "Usar una sola rama sin control.",
          "Reducir el rizado y repartir mejor la corriente entre varias ramas en paralelo desfasadas.",
          "Eliminar la modulación PWM."
        ],
        answer: 2,
        explanation: "El interleaving utiliza ramas en paralelo con PWM desfasadas para repartir corriente y reducir el rizado."
      },
      {
        topic: "Control",
        question: "¿Cuál es la diferencia entre control por ángulo de disparo y control PWM?",
        options: [
          "No existe diferencia.",
          "Ángulo de disparo varía el instante de encendido en AC; PWM varía el ancho del pulso o tiempo de encendido.",
          "PWM usa siempre tiristores y alfa usa solo baterías.",
          "Ambos cambian únicamente la temperatura."
        ],
        answer: 1,
        explanation: "El control por ángulo de disparo cambia el instante en que conduce el dispositivo dentro de la onda AC. El PWM cambia el ancho del pulso o el tiempo de encendido."
      }
    ];

    let currentQuestion = 0;
    let score = 0;
    let selectedOption = null;
    let answered = false;
    let wrongQuestions = [];

    function startQuiz() {
      document.getElementById("startScreen").style.display = "none";
      document.getElementById("resultScreen").style.display = "none";
      document.getElementById("questionBox").style.display = "block";
      currentQuestion = 0;
      score = 0;
      selectedOption = null;
      answered = false;
      wrongQuestions = [];
      renderQuestion();
    }

    function renderQuestion() {
      const q = questions[currentQuestion];
      selectedOption = null;
      answered = false;

      document.getElementById("questionNumber").textContent = "Pregunta " + (currentQuestion + 1);
      document.getElementById("questionText").textContent = q.question;
      document.getElementById("topicBadge").textContent = q.topic;
      document.getElementById("scoreBadge").textContent = "Puntaje: " + score;
      document.getElementById("progressText").textContent = "Pregunta " + (currentQuestion + 1) + " de " + questions.length;
      document.getElementById("progressBar").style.width = ((currentQuestion) / questions.length) * 100 + "%";

      const container = document.getElementById("optionsContainer");
      container.innerHTML = "";

      q.options.forEach((option, index) => {
        const btn = document.createElement("button");
        btn.className = "option";
        btn.innerHTML = "<strong>" + String.fromCharCode(65 + index) + ".</strong> " + option;
        btn.onclick = () => selectOption(index, btn);
        container.appendChild(btn);
      });

      const feedback = document.getElementById("feedbackBox");
      feedback.style.display = "none";
      feedback.className = "feedback";
      feedback.innerHTML = "";

      document.getElementById("checkBtn").style.display = "inline-block";
      document.getElementById("nextBtn").style.display = "none";
    }

    function selectOption(index, element) {
      if (answered) return;
      selectedOption = index;
      const options = document.querySelectorAll(".option");
      options.forEach(opt => opt.classList.remove("selected"));
      element.classList.add("selected");
    }

    function checkAnswer() {
      if (answered || selectedOption === null) {
        alert("Primero selecciona una alternativa.");
        return;
      }

      answered = true;
      const q = questions[currentQuestion];
      const options = document.querySelectorAll(".option");
      const feedback = document.getElementById("feedbackBox");

      options[q.answer].classList.add("correct");

      if (selectedOption === q.answer) {
        score++;
        feedback.classList.add("ok");
        feedback.innerHTML = "<strong>✅ Correcto.</strong><br><br>" + q.explanation;
      } else {
        options[selectedOption].classList.add("wrong");
        wrongQuestions.push(q);
        feedback.classList.add("bad");
        feedback.innerHTML = "<strong>❌ Incorrecto.</strong><br><br><strong>Respuesta correcta:</strong> " +
          String.fromCharCode(65 + q.answer) + ". " + q.options[q.answer] +
          "<br><br><strong>Explicación:</strong> " + q.explanation;
      }

      feedback.style.display = "block";
      document.getElementById("scoreBadge").textContent = "Puntaje: " + score;
      document.getElementById("progressBar").style.width = ((currentQuestion + 1) / questions.length) * 100 + "%";
      document.getElementById("checkBtn").style.display = "none";
      document.getElementById("nextBtn").style.display = "inline-block";
    }

    function nextQuestion() {
      currentQuestion++;
      if (currentQuestion < questions.length) {
        renderQuestion();
      } else {
        showResults();
      }
    }

    function showResults() {
      document.getElementById("questionBox").style.display = "none";
      document.getElementById("resultScreen").style.display = "block";

      const total = questions.length;
      const percent = Math.round((score / total) * 100);

      document.getElementById("finalScore").textContent =
        "Obtuviste " + score + " de " + total + " respuestas correctas (" + percent + "%).";

      let message = "";
      if (percent >= 90) {
        message = "Excelente. Ya dominas muy bien el Banco 1.";
      } else if (percent >= 75) {
        message = "Muy bien. Tienes buena base, solo te falta pulir algunos detalles.";
      } else if (percent >= 60) {
        message = "Vas avanzando. Conviene reforzar los temas donde fallaste.";
      } else {
        message = "Necesitas repasar más, pero este resultado te ayuda a detectar tus puntos débiles.";
      }

      document.getElementById("finalMessage").textContent = message;

      const review = document.getElementById("reviewContainer");
      review.innerHTML = "";

      if (wrongQuestions.length === 0) {
        review.innerHTML = "<div class='review-item'>No fallaste ninguna pregunta. Muy buen trabajo.</div>";
      } else {
        wrongQuestions.forEach((q, i) => {
          const item = document.createElement("div");
          item.className = "review-item";
          item.innerHTML = `
            <strong>${i + 1}. ${q.question}</strong>
            <div class="small" style="margin-top:8px;"><strong>Respuesta correcta:</strong> ${q.options[q.answer]}</div>
            <div class="small" style="margin-top:6px;"><strong>Explicación:</strong> ${q.explanation}</div>
          `;
          review.appendChild(item);
        });
      }
    }

    function restartQuiz() {
      document.getElementById("startScreen").style.display = "block";
      document.getElementById("questionBox").style.display = "none";
      document.getElementById("resultScreen").style.display = "none";
    }
  </script>
</body>
</html>
