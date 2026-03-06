<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Banco 1 - Quiz Interactivo</title>
  <style>
    :root {
      --bg1: #0f172a;
      --bg2: #1e293b;
      --card: rgba(255,255,255,0.08);
      --text: #e5eefc;
      --muted: #b7c6e0;
      --accent: #38bdf8;
      --ok: #22c55e;
      --bad: #ef4444;
      --warn: #f59e0b;
      --shadow: 0 20px 50px rgba(0,0,0,.25);
      --radius: 22px;
    }

    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Inter, system-ui, Arial, sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at top left, #1d4ed8 0%, transparent 28%),
        radial-gradient(circle at top right, #0ea5e9 0%, transparent 22%),
        linear-gradient(135deg, var(--bg1), var(--bg2));
      min-height: 100vh;
    }

    .wrap {
      max-width: 980px;
      margin: 0 auto;
      padding: 24px;
    }

    .hero, .panel, .quiz-card, .summary {
      background: var(--card);
      border: 1px solid rgba(255,255,255,.12);
      backdrop-filter: blur(12px);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }

    .hero {
      padding: 28px;
      margin-bottom: 18px;
    }

    h1 {
      margin: 0 0 10px;
      font-size: clamp(2rem, 3vw, 2.8rem);
      line-height: 1.05;
    }

    .subtitle {
      color: var(--muted);
      font-size: 1.05rem;
      line-height: 1.5;
    }

    .stats {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin-top: 18px;
    }

    .stat {
      padding: 14px;
      border-radius: 18px;
      background: rgba(255,255,255,.06);
    }

    .stat small {
      display: block;
      color: var(--muted);
      margin-bottom: 6px;
    }

    .stat strong {
      font-size: 1.25rem;
    }

    .panel {
      padding: 20px;
      margin-bottom: 18px;
    }

    .config-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 14px;
      align-items: end;
    }

    label {
      display: block;
      font-size: .95rem;
      color: var(--muted);
      margin-bottom: 8px;
    }

    select, button {
      width: 100%;
      border: none;
      border-radius: 14px;
      padding: 14px 16px;
      font-size: 1rem;
    }

    select {
      background: rgba(255,255,255,.12);
      color: var(--text);
      outline: none;
      border: 1px solid rgba(255,255,255,.12);
    }

    button {
      cursor: pointer;
      font-weight: 700;
      transition: transform .15s ease, filter .15s ease;
    }

    button:hover { transform: translateY(-1px); filter: brightness(1.05); }
    button:active { transform: translateY(1px); }

    .primary { background: linear-gradient(135deg, #38bdf8, #2563eb); color: white; }
    .secondary { background: rgba(255,255,255,.11); color: white; }
    .danger { background: rgba(239,68,68,.14); color: #fecaca; }

    .quiz-card {
      padding: 24px;
      display: none;
    }

    .topbar {
      display: flex;
      gap: 14px;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      margin-bottom: 18px;
    }

    .progress-wrap {
      flex: 1;
      min-width: 240px;
    }

    .progress-text {
      display: flex;
      justify-content: space-between;
      color: var(--muted);
      font-size: .95rem;
      margin-bottom: 8px;
    }

    .progress {
      width: 100%;
      height: 12px;
      background: rgba(255,255,255,.08);
      border-radius: 999px;
      overflow: hidden;
    }

    .bar {
      height: 100%;
      width: 0%;
      border-radius: 999px;
      background: linear-gradient(90deg, #22c55e, #38bdf8);
      transition: width .35s ease;
    }

    .pill {
      padding: 10px 14px;
      border-radius: 999px;
      font-weight: 700;
      font-size: .95rem;
      background: rgba(255,255,255,.08);
    }

    .question-head {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      align-items: center;
      margin-bottom: 8px;
    }

    .topic {
      background: rgba(56,189,248,.15);
      color: #bae6fd;
      padding: 8px 12px;
      border-radius: 999px;
      font-size: .9rem;
      font-weight: 700;
    }

    .difficulty {
      background: rgba(245,158,11,.13);
      color: #fde68a;
      padding: 8px 12px;
      border-radius: 999px;
      font-size: .9rem;
      font-weight: 700;
    }

    .question {
      font-size: clamp(1.2rem, 2vw, 1.5rem);
      line-height: 1.4;
      margin: 14px 0 18px;
    }

    .options {
      display: grid;
      gap: 12px;
      margin-bottom: 18px;
    }

    .option {
      text-align: left;
      background: rgba(255,255,255,.07);
      color: var(--text);
      border: 1px solid rgba(255,255,255,.1);
      padding: 16px;
      border-radius: 18px;
      font-weight: 600;
    }

    .option.selected {
      outline: 2px solid rgba(56,189,248,.8);
      background: rgba(56,189,248,.14);
    }

    .option.correct {
      background: rgba(34,197,94,.16);
      border-color: rgba(34,197,94,.5);
    }

    .option.wrong {
      background: rgba(239,68,68,.15);
      border-color: rgba(239,68,68,.5);
    }

    .explain {
      display: none;
      margin-top: 10px;
      border-radius: 18px;
      padding: 16px;
      line-height: 1.6;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.1);
    }

    .explain.ok { border-color: rgba(34,197,94,.35); }
    .explain.bad { border-color: rgba(239,68,68,.35); }

    .actions {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
    }

    .summary {
      display: none;
      padding: 24px;
      margin-top: 18px;
    }

    .summary-grid {
      display: grid;
      grid-template-columns: 1.2fr .8fr;
      gap: 18px;
    }

    .review-list {
      display: grid;
      gap: 12px;
      margin-top: 16px;
    }

    .review-item {
      padding: 14px;
      border-radius: 18px;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.1);
    }

    .mini {
      color: var(--muted);
      font-size: .95rem;
    }

    .celebrate {
      font-size: 1.15rem;
      font-weight: 800;
      margin: 10px 0 14px;
    }

    .hidden { display: none !important; }

    @media (max-width: 780px) {
      .stats, .config-grid, .summary-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <section class="hero">
      <h1>Banco 1 · Quiz interactivo</h1>
      <p class="subtitle">
        Practica Buck-Boost, PFC, AC/AC, cicloconvertidor, SVC y AC/DC con PWM.
        Hay preguntas con respuestas correctas en distintas alternativas para que no se vuelva predecible.
        Si fallas, el sistema te explica qué era lo correcto y por qué.
      </p>
      <div class="stats">
        <div class="stat"><small>Modo</small><strong>Dinámico</strong></div>
        <div class="stat"><small>Preguntas</small><strong id="heroQuestions">20</strong></div>
        <div class="stat"><small>Temas</small><strong>Banco 1</strong></div>
        <div class="stat"><small>Feedback</small><strong>Inmediato</strong></div>
      </div>
    </section>

    <section class="panel" id="setupPanel">
      <div class="config-grid">
        <div>
          <label for="topicSelect">Tema</label>
          <select id="topicSelect">
            <option value="todos">Todos los temas</option>
            <option value="Buck-Boost">Buck-Boost</option>
            <option value="PFC">PFC</option>
            <option value="AC/AC">Reguladores AC/AC</option>
            <option value="Cicloconvertidor">Cicloconvertidor</option>
            <option value="SVC">SVC</option>
            <option value="AC/DC PWM">AC/DC con PWM</option>
          </select>
        </div>
        <div>
          <label for="countSelect">Cantidad</label>
          <select id="countSelect">
            <option value="10">10 preguntas</option>
            <option value="15">15 preguntas</option>
            <option value="20" selected>20 preguntas</option>
            <option value="25">25 preguntas</option>
          </select>
        </div>
        <div>
          <button class="primary" id="startBtn">Empezar quiz</button>
        </div>
      </div>
    </section>

    <section class="quiz-card" id="quizCard">
      <div class="topbar">
        <div class="progress-wrap">
          <div class="progress-text">
            <span id="progressLabel">Pregunta 1 de 20</span>
            <span id="scoreLabel">Puntaje: 0</span>
          </div>
          <div class="progress"><div class="bar" id="progressBar"></div></div>
        </div>
        <div class="pill" id="streakLabel">Racha: 0</div>
      </div>

      <div class="question-head">
        <span class="topic" id="topicBadge">Tema</span>
        <span class="difficulty" id="difficultyBadge">Nivel</span>
      </div>

      <div class="question" id="questionText"></div>
      <div class="options" id="optionsBox"></div>

      <div class="explain" id="feedbackBox"></div>

      <div class="actions">
        <button class="primary" id="checkBtn">Revisar respuesta</button>
        <button class="secondary hidden" id="nextBtn">Siguiente</button>
        <button class="danger" id="restartBtn">Reiniciar</button>
      </div>
    </section>

    <section class="summary" id="summaryBox">
      <div class="summary-grid">
        <div>
          <h2 style="margin-top:0">Resultado final</h2>
          <div class="celebrate" id="finalMessage"></div>
          <p class="mini" id="finalStats"></p>
          <div class="review-list" id="reviewList"></div>
        </div>
        <div>
          <div class="panel" style="margin:0; background: rgba(255,255,255,.04)">
            <h3 style="margin-top:0">Recomendación</h3>
            <p class="mini" id="recommendation"></p>
            <button class="primary" id="retryWrongBtn">Practicar mis errores</button>
            <div style="height:10px"></div>
            <button class="secondary" id="playAgainBtn">Jugar otra vez</button>
          </div>
        </div>
      </div>
    </section>
  </div>

  <script>
    const questionBank = [
      {
        topic: 'Buck-Boost', difficulty: 'Básico',
        q: '¿Cuál es la variable principal de control en un convertidor Buck-Boost?',
        options: ['La resistencia de carga', 'El ciclo de trabajo D', 'La frecuencia de red', 'El factor de potencia'],
        correct: 1,
        explain: 'La variable principal es el ciclo de trabajo D. Al variarlo, la tensión de salida puede subir o bajar.'
      },
      {
        topic: 'Buck-Boost', difficulty: 'Básico',
        q: 'En el estado ON del Buck-Boost, ¿qué sucede?',
        options: ['El inductor almacena energía y el diodo bloquea', 'El inductor entrega energía y el diodo bloquea', 'El capacitor se descarga por completo', 'La corriente del inductor siempre llega a cero'],
        correct: 0,
        explain: 'En ON, el interruptor conduce, el inductor almacena energía en su campo magnético y el diodo permanece bloqueado.'
      },
      {
        topic: 'Buck-Boost', difficulty: 'Básico',
        q: 'En el estado OFF del Buck-Boost, la opción correcta es:',
        options: ['El diodo bloquea y el inductor sigue cargándose', 'El inductor deja de actuar', 'El inductor entrega energía a la carga y el diodo conduce', 'Solo conduce el capacitor'],
        correct: 2,
        explain: 'En OFF, el inductor libera la energía almacenada hacia la carga y el diodo conduce para permitir ese camino.'
      },
      {
        topic: 'Buck-Boost', difficulty: 'Intermedio',
        q: 'La diferencia entre CCM y DCM es que:',
        options: ['En CCM la corriente del inductor nunca llega a cero; en DCM sí llega a cero', 'En DCM la corriente siempre es mayor', 'En CCM el diodo nunca conduce', 'En DCM no existe ciclo de trabajo'],
        correct: 0,
        explain: 'CCM es conducción continua: la corriente del inductor no llega a cero. En DCM sí aparece un intervalo con corriente cero.'
      },
      {
        topic: 'Buck-Boost', difficulty: 'Intermedio',
        q: 'En modo DCM, la relación de conversión del Buck-Boost:',
        options: ['Depende solo de D', 'No depende de la carga', 'No depende de la inductancia', 'No depende solo de D; también influyen carga e inductancia'],
        correct: 3,
        explain: 'En DCM la conversión ya no depende solo del duty cycle. También intervienen la inductancia, la carga y otras condiciones de operación.'
      },
      {
        topic: 'PFC', difficulty: 'Básico',
        q: 'La función principal del PFC es:',
        options: ['Solo regular una tensión DC', 'Mejorar el factor de potencia y reducir la distorsión de corriente', 'Aumentar siempre la frecuencia', 'Eliminar todo uso del rectificador'],
        correct: 1,
        explain: 'El PFC busca que la corriente de entrada tenga mejor calidad: más senoidal, en fase con la tensión y con menor distorsión.'
      },
      {
        topic: 'PFC', difficulty: 'Básico',
        q: 'Idealmente, la corriente de entrada en una etapa PFC debe ser:',
        options: ['Cuadrada y adelantada', 'Pulsante y desfasada', 'Senoidal y en fase con la tensión', 'Constante en todo instante'],
        correct: 2,
        explain: 'La meta del PFC es que la corriente siga a la tensión de entrada: forma senoidal y en fase.'
      },
      {
        topic: 'PFC', difficulty: 'Intermedio',
        q: 'THD significa:',
        options: ['Tiempo de Histéresis Dinámica', 'Distorsión Armónica Total', 'Tensión Homogénea Directa', 'Transferencia de Hardware Digital'],
        correct: 1,
        explain: 'THD es Distorsión Armónica Total, una medida de cuánto se deforma una señal respecto a una senoide ideal.'
      },
      {
        topic: 'PFC', difficulty: 'Intermedio',
        q: 'Si el THD es alto, una consecuencia típica es:',
        options: ['Menor distorsión', 'Más pérdidas y calentamiento', 'Factor de potencia perfecto', 'Menos armónicos'],
        correct: 1,
        explain: 'Un THD alto implica más armónicos, lo que suele traer pérdidas extra, calentamiento y peor factor de potencia.'
      },
      {
        topic: 'PFC', difficulty: 'Básico',
        q: '¿Cuál de estas NO es la idea central del PFC?',
        options: ['Mejorar la forma de la corriente de entrada', 'Lograr corriente en fase con la tensión', 'Reducir la distorsión armónica', 'Ser únicamente un regulador DC'],
        correct: 3,
        explain: 'La etapa PFC no se define solo por regular DC. Su meta central es mejorar la calidad de la corriente de entrada.'
      },
      {
        topic: 'AC/AC', difficulty: 'Básico',
        q: 'Un regulador AC/AC controla principalmente:',
        options: ['La frecuencia de salida manteniendo fijo el RMS', 'El valor RMS sin cambiar la frecuencia', 'Solo la potencia reactiva', 'La corriente DC del bus'],
        correct: 1,
        explain: 'La idea central del regulador AC/AC es variar el valor eficaz de la tensión aplicada a la carga sin modificar la frecuencia de la red.'
      },
      {
        topic: 'AC/AC', difficulty: 'Básico',
        q: 'Decir que el regulador AC/AC es una conversión directa significa que:',
        options: ['Usa un enlace DC intermedio', 'Convierte AC a AC sin enlace DC intermedio', 'Siempre eleva tensión', 'Solo funciona con cargas resistivas'],
        correct: 1,
        explain: 'Es conversión directa porque no pasa primero por una etapa DC intermedia.'
      },
      {
        topic: 'AC/AC', difficulty: 'Intermedio',
        q: 'Cuando el ángulo de disparo α aumenta, normalmente ocurre que:',
        options: ['Aumenta el RMS y baja el THD', 'Baja el RMS, sube el THD y empeora el factor de potencia', 'No cambia nada relevante', 'La frecuencia aumenta'],
        correct: 1,
        explain: 'Un α mayor retrasa más la conducción, disminuye el RMS y empeora la calidad de onda.'
      },
      {
        topic: 'AC/AC', difficulty: 'Intermedio',
        q: 'En una carga RL conectada a un regulador AC/AC, cerca del cruce por cero:',
        options: ['La corriente se anula exactamente con la tensión', 'La conducción se prolonga por efecto del inductor', 'No existe desfase', 'Solo manda la resistencia'],
        correct: 1,
        explain: 'El inductor hace que la corriente no se corte instantáneamente; por eso la conducción continúa más allá del cruce por cero.'
      },
      {
        topic: 'AC/AC', difficulty: 'Intermedio',
        q: 'El burst control o control por ciclos completos consiste en:',
        options: ['Variar solo el ancho de pulso', 'Dejar pasar ciclos completos y bloquear otros', 'Cambiar la frecuencia de la red', 'Disparar siempre a 180°'],
        correct: 1,
        explain: 'Burst control no es PWM. Se basa en aplicar ciclos enteros y suprimir otros para regular la potencia entregada.'
      },
      {
        topic: 'Cicloconvertidor', difficulty: 'Básico',
        q: 'Un cicloconvertidor es:',
        options: ['Un convertidor DC/AC con filtro LCL', 'Un AC/AC directo con frecuencia de salida normalmente menor', 'Un rectificador no controlado', 'Un inversor monofásico'],
        correct: 1,
        explain: 'El cicloconvertidor convierte AC a AC directamente y suele entregar una frecuencia de salida menor que la de entrada.'
      },
      {
        topic: 'Cicloconvertidor', difficulty: 'Intermedio',
        q: 'Una aplicación típica del cicloconvertidor es:',
        options: ['Equipos de muy alta potencia y baja velocidad', 'Cargadores USB', 'Circuitos lógicos digitales', 'Solo iluminación LED'],
        correct: 0,
        explain: 'Se usa en grandes accionamientos industriales, como molinos o propulsión, donde se requiere alta potencia y baja velocidad.'
      },
      {
        topic: 'Cicloconvertidor', difficulty: 'Intermedio',
        q: 'La principal desventaja del cicloconvertidor suele ser:',
        options: ['No puede trabajar con tiristores', 'No permite control', 'Alta distorsión armónica y forma de onda no senoidal', 'Que siempre requiere batería'],
        correct: 2,
        explain: 'El cicloconvertidor maneja bien alta potencia, pero a costa de una forma de onda más distorsionada.'
      },
      {
        topic: 'SVC', difficulty: 'Básico',
        q: 'La función principal de un SVC es:',
        options: ['Compensar potencia reactiva para regular tensión y mejorar el factor de potencia', 'Rectificar AC a DC', 'Cambiar la frecuencia de la red', 'Sustituir todos los transformadores'],
        correct: 0,
        explain: 'El SVC actúa sobre la potencia reactiva del sistema para sostener tensión y mejorar el factor de potencia.'
      },
      {
        topic: 'SVC', difficulty: 'Básico',
        q: 'TCR significa:',
        options: ['Transformador de Corriente Rápida', 'Thyristor Controlled Reactor', 'Terminal de Control Reactivo', 'Total Compensation Regulator'],
        correct: 1,
        explain: 'TCR significa reactor controlado por tiristores: una bobina cuya conducción se ajusta con el ángulo de disparo.'
      },
      {
        topic: 'SVC', difficulty: 'Intermedio',
        q: 'Si en un TCR el ángulo α se acerca a 180°, entonces:',
        options: ['Conduce más corriente', 'El reactor casi no conduce y absorbe muy poca reactiva', 'Se vuelve un capacitor', 'La frecuencia de red disminuye'],
        correct: 1,
        explain: 'A medida que α se acerca a 180°, el TCR reduce su conducción casi hasta cero.'
      },
      {
        topic: 'SVC', difficulty: 'Intermedio',
        q: 'Los filtros LC en un SVC se utilizan principalmente para:',
        options: ['Elevar el voltaje de línea', 'Eliminar el ángulo de disparo', 'Reducir armónicos hacia la red', 'Medir frecuencia'],
        correct: 2,
        explain: 'Los TCR generan armónicos; por eso se añaden filtros LC para evitar que esos armónicos se inyecten a la red.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Básico',
        q: 'En un sistema AC/DC moderno con PWM, el lazo externo se encarga de:',
        options: ['Regular Vdc', 'Definir la frecuencia de red', 'Activar el transformador', 'Eliminar totalmente el rectificador'],
        correct: 0,
        explain: 'El lazo externo regula la tensión del bus DC y genera la referencia necesaria para el control interno.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Básico',
        q: 'En ese mismo sistema, el lazo interno se encarga de:',
        options: ['Moldear o regular la corriente', 'Subir la tensión de red', 'Disparar SCR con α', 'Cambiar la frecuencia de la red'],
        correct: 0,
        explain: 'El lazo interno actúa sobre la corriente y lo hace ajustando la señal de control del convertidor.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Básico',
        q: 'La variable manipulada típica en un convertidor PWM es:',
        options: ['El duty cycle d', 'El factor de potencia', 'La impedancia de red', 'La inductancia física'],
        correct: 0,
        explain: 'En PWM la variable manipulada es el duty cycle, es decir, el tiempo de encendido relativo dentro del período.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Intermedio',
        q: 'PWM significa:',
        options: ['Pulse Width Modulation', 'Power Wave Method', 'Phase Wiring Mode', 'Pulse Watt Meter'],
        correct: 0,
        explain: 'PWM significa modulación por ancho de pulso: se varía el ancho del pulso para controlar la salida.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Intermedio',
        q: 'El objetivo principal de un rectificador PWM trifásico es:',
        options: ['Tener Vdc estable, corrientes senoidales y FP cercano a 1', 'Generar solo corriente DC pura sin control', 'Trabajar únicamente con SCR', 'Eliminar toda electrónica de control'],
        correct: 0,
        explain: 'El rectificador PWM trifásico busca tomar energía de la red con buena calidad: bajo THD, FP alto y bus DC estable.'
      },
      {
        topic: 'AC/DC PWM', difficulty: 'Intermedio',
        q: 'Interleaving significa usar:',
        options: ['Una sola rama con frecuencia cero', 'Ramas en paralelo con PWM desfasadas', 'Un filtro RC únicamente', 'Solo control por ángulo α'],
        correct: 1,
        explain: 'Interleaving usa varias ramas en paralelo desfasadas entre sí para repartir corriente y reducir rizado.'
      }
    ];

    const topicSelect = document.getElementById('topicSelect');
    const countSelect = document.getElementById('countSelect');
    const startBtn = document.getElementById('startBtn');
    const heroQuestions = document.getElementById('heroQuestions');

    const quizCard = document.getElementById('quizCard');
    const summaryBox = document.getElementById('summaryBox');
    const setupPanel = document.getElementById('setupPanel');
    const questionText = document.getElementById('questionText');
    const optionsBox = document.getElementById('optionsBox');
    const topicBadge = document.getElementById('topicBadge');
    const difficultyBadge = document.getElementById('difficultyBadge');
    const checkBtn = document.getElementById('checkBtn');
    const nextBtn = document.getElementById('nextBtn');
    const restartBtn = document.getElementById('restartBtn');
    const feedbackBox = document.getElementById('feedbackBox');
    const progressBar = document.getElementById('progressBar');
    const progressLabel = document.getElementById('progressLabel');
    const scoreLabel = document.getElementById('scoreLabel');
    const streakLabel = document.getElementById('streakLabel');
    const finalMessage = document.getElementById('finalMessage');
    const finalStats = document.getElementById('finalStats');
    const reviewList = document.getElementById('reviewList');
    const recommendation = document.getElementById('recommendation');
    const retryWrongBtn = document.getElementById('retryWrongBtn');
    const playAgainBtn = document.getElementById('playAgainBtn');

    let selectedQuestions = [];
    let currentIndex = 0;
    let score = 0;
    let streak = 0;
    let selectedOption = null;
    let answered = false;
    let wrongAnswers = [];

    function shuffle(array) {
      const arr = [...array];
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function getPool() {
      const topic = topicSelect.value;
      if (topic === 'todos') return questionBank;
      return questionBank.filter(q => q.topic === topic);
    }

    function startQuiz(customPool = null) {
      const pool = customPool || getPool();
      const count = Math.min(Number(countSelect.value), pool.length);
      selectedQuestions = shuffle(pool).slice(0, count).map(q => ({ ...q }));
      currentIndex = 0;
      score = 0;
      streak = 0;
      wrongAnswers = [];
      selectedOption = null;
      answered = false;
      summaryBox.style.display = 'none';
      quizCard.style.display = 'block';
      setupPanel.classList.add('hidden');
      renderQuestion();
    }

    function renderQuestion() {
      const item = selectedQuestions[currentIndex];
      selectedOption = null;
      answered = false;
      feedbackBox.style.display = 'none';
      feedbackBox.className = 'explain';
      nextBtn.classList.add('hidden');
      checkBtn.classList.remove('hidden');

      questionText.textContent = item.q;
      topicBadge.textContent = item.topic;
      difficultyBadge.textContent = item.difficulty;
      progressLabel.textContent = `Pregunta ${currentIndex + 1} de ${selectedQuestions.length}`;
      scoreLabel.textContent = `Puntaje: ${score}`;
      streakLabel.textContent = `Racha: ${streak}`;
      progressBar.style.width = `${(currentIndex / selectedQuestions.length) * 100}%`;

      optionsBox.innerHTML = '';
      item.options.forEach((opt, idx) => {
        const btn = document.createElement('button');
        btn.className = 'option';
        btn.textContent = `${String.fromCharCode(65 + idx)}. ${opt}`;
        btn.onclick = () => {
          if (answered) return;
          selectedOption = idx;
          [...optionsBox.children].forEach(el => el.classList.remove('selected'));
          btn.classList.add('selected');
        };
        optionsBox.appendChild(btn);
      });
    }

    function explainResult(isCorrect, item) {
      const correctLabel = String.fromCharCode(65 + item.correct);
      const chosen = selectedOption === null ? 'Sin respuesta' : `${String.fromCharCode(65 + selectedOption)}. ${item.options[selectedOption]}`;
      feedbackBox.style.display = 'block';
      if (isCorrect) {
        feedbackBox.classList.add('ok');
        feedbackBox.innerHTML = `<strong>✅ Correcto.</strong><br><br><strong>Tu respuesta:</strong> ${chosen}<br><strong>Por qué está bien:</strong> ${item.explain}`;
      } else {
        feedbackBox.classList.add('bad');
        feedbackBox.innerHTML = `<strong>❌ No fue correcta.</strong><br><br><strong>Tu respuesta:</strong> ${chosen}<br><strong>Respuesta correcta:</strong> ${correctLabel}. ${item.options[item.correct]}<br><strong>Explicación:</strong> ${item.explain}`;
      }
    }

    checkBtn.onclick = () => {
      if (answered) return;
      const item = selectedQuestions[currentIndex];
      answered = true;

      if (selectedOption === null) {
        streak = 0;
        wrongAnswers.push(item);
        explainResult(false, item);
      } else {
        const children = [...optionsBox.children];
        children[item.correct].classList.add('correct');
        if (selectedOption !== item.correct) {
          children[selectedOption].classList.add('wrong');
        }

        const isCorrect = selectedOption === item.correct;
        if (isCorrect) {
          score++;
          streak++;
        } else {
          streak = 0;
          wrongAnswers.push(item);
        }
        explainResult(isCorrect, item);
      }

      scoreLabel.textContent = `Puntaje: ${score}`;
      streakLabel.textContent = `Racha: ${streak}`;
      checkBtn.classList.add('hidden');
      nextBtn.classList.remove('hidden');
      progressBar.style.width = `${((currentIndex + 1) / selectedQuestions.length) * 100}%`;
    };

    nextBtn.onclick = () => {
      currentIndex++;
      if (currentIndex >= selectedQuestions.length) {
        showSummary();
      } else {
        renderQuestion();
      }
    };

    function showSummary() {
      quizCard.style.display = 'none';
      summaryBox.style.display = 'block';
      const percent = Math.round((score / selectedQuestions.length) * 100);

      let message = 'Buen trabajo.';
      if (percent >= 90) message = 'Excelente. Ya estás fuerte para el examen.';
      else if (percent >= 75) message = 'Muy bien. Ya tienes buena base, solo pule detalles.';
      else if (percent >= 60) message = 'Vas avanzando. Conviene reforzar algunos conceptos.';
      else message = 'Toca repasar, pero ya detectaste exactamente qué mejorar.';

      finalMessage.textContent = message;
      finalStats.textContent = `Acertaste ${score} de ${selectedQuestions.length} preguntas (${percent}%). Errores detectados: ${wrongAnswers.length}.`;

      reviewList.innerHTML = '';
      if (wrongAnswers.length === 0) {
        const div = document.createElement('div');
        div.className = 'review-item';
        div.innerHTML = '<strong>Sin errores.</strong><br><span class="mini">Puedes subir la dificultad repitiendo con otro tema o más preguntas.</span>';
        reviewList.appendChild(div);
      } else {
        wrongAnswers.slice(0, 8).forEach(item => {
          const div = document.createElement('div');
          div.className = 'review-item';
          div.innerHTML = `<strong>${item.topic}</strong><br>${item.q}<br><span class="mini">Clave: ${item.options[item.correct]}. ${item.explain}</span>`;
          reviewList.appendChild(div);
        });
      }

      recommendation.textContent = wrongAnswers.length === 0
        ? 'Haz otra ronda cambiando de tema o aumentando la cantidad para mantener la velocidad mental.'
        : 'Practica tus errores primero. El modo de repaso te lanzará solo las preguntas que fallaste para consolidar memoria.';
    }

    retryWrongBtn.onclick = () => {
      if (wrongAnswers.length === 0) {
        alert('No tienes errores para repasar. Prueba otro tema o más preguntas.');
        return;
      }
      countSelect.value = String(Math.min(wrongAnswers.length, 20));
      startQuiz(shuffle(wrongAnswers));
    };

    playAgainBtn.onclick = () => {
      summaryBox.style.display = 'none';
      setupPanel.classList.remove('hidden');
      heroQuestions.textContent = countSelect.value;
    };

    restartBtn.onclick = () => {
      if (confirm('¿Quieres reiniciar el quiz?')) {
        summaryBox.style.display = 'none';
        quizCard.style.display = 'none';
        setupPanel.classList.remove('hidden');
      }
    };

    startBtn.onclick = () => startQuiz();
    countSelect.onchange = () => { heroQuestions.textContent = countSelect.value; };
    heroQuestions.textContent = countSelect.value;
  </script>
</body>
</html>
