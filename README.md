# GAAS---the-game
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Gods of Ash and Silence</title>
  <style>
    :root{
      --bg:#0b0f1a;
      --panel:#111827cc;
      --ink:#e5e7eb;
      --muted:#9ca3af;
      --accent:#fbbf24;
      --danger:#ef4444;
      --ok:#22c55e;
      --shadow: 0 12px 40px rgba(0,0,0,.55);
      --radius: 18px;
      --pixel: 2px;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      background: radial-gradient(1200px 700px at 35% 30%, #1d2a4a 0%, var(--bg) 55%, #070a12 100%);
      color:var(--ink);
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial;
      overflow:hidden;
    }
    button, input{
      font:inherit;
    }
    .ui{
      position:fixed;
      inset:0;
      display:grid;
      place-items:center;
      padding:24px;
      pointer-events:none;
    }
    .screen{
      width:min(980px, 95vw);
      background: linear-gradient(180deg, rgba(17,24,39,.92), rgba(17,24,39,.72));
      border: 1px solid rgba(255,255,255,.08);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 22px;
      pointer-events:auto;
      display:none;
    }
    .screen.active{display:block}
    .topline{
      display:flex;
      align-items:flex-start;
      justify-content:space-between;
      gap:14px;
    }
    .title{
      font-weight:800;
      letter-spacing: .6px;
      font-size: clamp(26px, 3.4vw, 42px);
      margin:0;
      line-height:1.05;
    }
    .subtitle{
      margin:10px 0 0 0;
      color:var(--muted);
      max-width: 62ch;
      line-height:1.35;
    }
    .row{
      display:flex;
      gap:12px;
      flex-wrap:wrap;
      margin-top:16px;
      align-items:center;
    }
    .btn{
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.06);
      color: var(--ink);
      padding: 10px 14px;
      border-radius: 14px;
      cursor:pointer;
      transition: transform .06s ease, background .15s ease, border-color .15s ease;
    }
    .btn:hover{
      background: rgba(255,255,255,.10);
      border-color: rgba(255,255,255,.18);
    }
    .btn:active{transform: translateY(1px)}
    .btn.primary{
      background: linear-gradient(180deg, rgba(251,191,36,.95), rgba(251,191,36,.70));
      color:#0b1020;
      border-color: rgba(251,191,36,.55);
      font-weight:700;
    }
    .btn.danger{
      background: rgba(239,68,68,.18);
      border-color: rgba(239,68,68,.35);
    }
    .pill{
      display:inline-flex;
      gap:8px;
      align-items:center;
      padding: 8px 12px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.06);
      color: var(--muted);
      font-size: 13px;
      user-select:none;
    }
    .grid{
      display:grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 12px;
      margin-top: 16px;
    }
    @media (max-width: 860px){
      .grid{grid-template-columns: 1fr}
    }
    .card{
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.10);
      border-radius: var(--radius);
      padding: 14px;
    }
    .card h3{
      margin:0 0 6px 0;
      font-size: 16px;
      letter-spacing:.2px;
    }
    .card p{
      margin:0;
      color:var(--muted);
      line-height:1.35;
      font-size: 14px;
    }

    canvas{
      display:block;
      width:100vw;
      height:100vh;
    }

    .hud{
      position:fixed;
      left:14px;
      top:14px;
      display:flex;
      gap:10px;
      align-items:center;
      pointer-events:none;
    }
    .hud > *{pointer-events:auto}
    .hud .mini{
      display:flex;
      flex-direction:column;
      gap:6px;
      padding: 10px 12px;
      border-radius: 16px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(17,24,39,.55);
      box-shadow: 0 10px 26px rgba(0,0,0,.35);
      min-width: 260px;
    }
    .bar{
      height:10px;
      background: rgba(255,255,255,.08);
      border-radius: 999px;
      overflow:hidden;
    }
    .bar > div{
      height:100%;
      width:0%;
      background: linear-gradient(90deg, rgba(251,191,36,.95), rgba(34,197,94,.85));
      transition: width .2s ease;
    }
    .hudline{
      display:flex;
      justify-content:space-between;
      gap:10px;
      font-size: 13px;
      color: var(--muted);
    }
    .kbd{
      position:fixed;
      right:14px;
      top:14px;
      display:flex;
      flex-direction:column;
      gap:8px;
      pointer-events:none;
    }
    .kbd .tip{
      pointer-events:auto;
      padding: 10px 12px;
      border-radius: 16px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(17,24,39,.55);
      color: var(--muted);
      font-size: 13px;
      box-shadow: 0 10px 26px rgba(0,0,0,.35);
      max-width: 320px;
      line-height:1.3;
    }

    .modalOverlay{
      position:fixed;
      inset:0;
      background: rgba(0,0,0,.55);
      display:none;
      place-items:center;
      padding:18px;
      z-index: 50;
    }
    .modalOverlay.active{display:grid}
    .modal{
      width:min(980px, 96vw);
      background: rgba(17,24,39,.92);
      border:1px solid rgba(255,255,255,.10);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 16px;
    }
    .modalHeader{
      display:flex;
      justify-content:space-between;
      gap:12px;
      align-items:flex-start;
    }
    .modalHeader h2{
      margin:0;
      font-size: 18px;
      letter-spacing:.2px;
    }
    .modalHeader p{
      margin:6px 0 0 0;
      color: var(--muted);
      line-height:1.35;
      max-width: 80ch;
      font-size: 14px;
    }
    .modalBody{
      display:grid;
      grid-template-columns: 1.15fr .85fr;
      gap: 12px;
      margin-top: 12px;
    }
    @media (max-width: 880px){
      .modalBody{grid-template-columns: 1fr}
    }
    .box{
      background: rgba(255,255,255,.06);
      border:1px solid rgba(255,255,255,.10);
      border-radius: var(--radius);
      padding: 14px;
    }
    .box h3{margin:0 0 8px 0; font-size: 15px}
    .box .small{color:var(--muted); font-size: 13px; line-height:1.35}
    .answers{
      display:flex;
      flex-direction:column;
      gap:8px;
      margin-top: 10px;
    }
    .answerBtn{
      text-align:left;
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.06);
      color: var(--ink);
      cursor:pointer;
    }
    .answerBtn:hover{background: rgba(255,255,255,.10)}
    .feedback{
      margin-top: 10px;
      padding: 10px 12px;
      border-radius: 14px;
      border:1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.06);
      color: var(--muted);
      line-height:1.35;
      font-size: 14px;
      display:none;
    }
    .feedback.ok{
      display:block;
      border-color: rgba(34,197,94,.35);
      background: rgba(34,197,94,.12);
      color: rgba(229,231,235,.95);
    }
    .feedback.bad{
      display:block;
      border-color: rgba(239,68,68,.35);
      background: rgba(239,68,68,.12);
      color: rgba(229,231,235,.95);
    }

    .ytWrap{
      position:fixed;
      left:-9999px;
      top:-9999px;
      width:1px;height:1px;
      overflow:hidden;
    }
    .soundBtn{
      position:fixed;
      bottom:14px;
      right:14px;
      z-index:60;
      display:flex;
      gap:10px;
      align-items:center;
    }
    .soundBtn .btn{box-shadow: 0 10px 26px rgba(0,0,0,.35)}
    .tag{
      font-size:12px;
      color:var(--muted);
      background: rgba(17,24,39,.55);
      border:1px solid rgba(255,255,255,.10);
      padding:6px 10px;
      border-radius:999px;
      box-shadow: 0 10px 26px rgba(0,0,0,.35);
    }
  </style>
</head>
<body>
  <canvas id="game"></canvas>

  <div class="hud" id="hud" style="display:none;">
    <div class="mini">
      <div class="hudline">
        <div id="hudChar">Char: -</div>
        <div id="hudScore">Score: 0</div>
      </div>
      <div class="bar"><div id="hudProgress"></div></div>
      <div class="hudline">
        <div id="hudRegion">Region: -</div>
        <div id="hudQuest">Quest: 0/0</div>
      </div>
    </div>
    <button class="btn" id="btnPause">Pause</button>
  </div>

  <div class="kbd" id="kbd" style="display:none;">
    <div class="tip">
      Steuerung: WASD oder Pfeiltasten. <br>
      Gehe zu den leuchtenden Checkpoints. <br>
      Jede Station erklärt einen Teil deines Schreibprozesses und stellt eine Frage.
    </div>
  </div>

  <div class="ui">
    <div class="screen active" id="screenMenu">
      <div class="topline">
        <div>
          <h1 class="title">Gods of Ash and Silence</h1>
          <p class="subtitle">
            Willkommen in Althrea. Ein interaktiver Weg durch mein Schreibprojekt, meine Blockaden, meine Planung und das, was ich daraus gelernt habe.
          </p>
          <div class="row">
            <span class="pill">2D Movement</span>
            <span class="pill">Story + Trivia</span>
            <span class="pill">Deutsch mit englischen “hard parts”</span>
          </div>
        </div>
        <div class="pill">Seminarfach: Meine Herausforderung</div>
      </div>

      <div class="row">
        <button class="btn primary" id="btnPlay">Play</button>
        <button class="btn" id="btnRules">Rules</button>
        <button class="btn" id="btnSettings">Settings</button>
      </div>

      <div class="grid">
        <div class="card">
          <h3>Story Setup</h3>
          <p>
            Du startest in Virellia, der strahlenden Hauptstadt. Alles wirkt perfekt, aber darunter liegt ein älteres System, das nicht gesehen werden soll.
            Danach geht es nach Blackwater Hold, Veloras Heimat, wo Schatten mehr Wahrheit sagen als Licht.
          </p>
        </div>
        <div class="card">
          <h3>Ziel</h3>
          <p>
            Sammle alle 12 Siegel der Reise. Jedes Siegel ist ein Checkpoint mit Erklärung und Multiple Choice Frage.
            Am Ende hast du die ganze Projektstory von Anfang bis Ende erlebt.
          </p>
        </div>
      </div>
    </div>

    <div class="screen" id="screenRules">
      <div class="topline">
        <div>
          <h1 class="title">Rules</h1>
          <p class="subtitle">
            Kurze Anleitung, damit niemand lost ist.
          </p>
        </div>
      </div>
      <div class="grid">
        <div class="card">
          <h3>Bewegung</h3>
          <p>WASD oder Pfeiltasten. Du bewegst deinen Charakter über die Karte.</p>
        </div>
        <div class="card">
          <h3>Checkpoints</h3>
          <p>Gehe in einen leuchtenden Kreis. Dann kommt Erklärung + Frage. Richtig lösen öffnet den Weg weiter.</p>
        </div>
        <div class="card">
          <h3>Sprache</h3>
          <p>Fragen sind auf Deutsch. Schwierige Reflexionsstellen und Stilfragen stehen teilweise auf Englisch.</p>
        </div>
        <div class="card">
          <h3>Sound</h3>
          <p>Browser blockieren Autoplay mit Ton. Musik startet deshalb meist stumm. Unten rechts Sound einschalten.</p>
        </div>
      </div>
      <div class="row">
        <button class="btn" id="btnBackFromRules">Back</button>
      </div>
    </div>

    <div class="screen" id="screenSettings">
      <div class="topline">
        <div>
          <h1 class="title">Settings</h1>
          <p class="subtitle">Ein paar simple Optionen.</p>
        </div>
      </div>

      <div class="grid">
        <div class="card">
          <h3>Hints</h3>
          <p>Zeigt eine kurze Hilfe an: ein Hinweis auf Deutsch und eine Mini-Übersetzung, falls nötig.</p>
          <div class="row">
            <button class="btn" id="btnToggleHints">Hints: ON</button>
          </div>
        </div>
        <div class="card">
          <h3>Difficulty</h3>
          <p>Einfach: du bekommst nach falscher Antwort direkt einen besseren Hinweis. Normal: weniger Hilfe.</p>
          <div class="row">
            <button class="btn" id="btnDiff">Difficulty: Easy</button>
          </div>
        </div>
      </div>

      <div class="row">
        <button class="btn" id="btnBackFromSettings">Back</button>
      </div>
    </div>

    <div class="screen" id="screenCharacter">
      <div class="topline">
        <div>
          <h1 class="title">Pick your character</h1>
          <p class="subtitle">
            Du spielst denselben Weg, aber mit anderem Look. (In dieser Version: stylisierte Pixel-Avatare, keine echten Portraits.)
          </p>
        </div>
      </div>

      <div class="grid" id="charGrid"></div>

      <div class="row">
        <button class="btn" id="btnBackFromChar">Back</button>
        <button class="btn primary" id="btnStartGame" disabled>Start</button>
      </div>
    </div>

    <div class="screen" id="screenPause">
      <div class="topline">
        <div>
          <h1 class="title">Pause</h1>
          <p class="subtitle">Atme. Dann weiter.</p>
        </div>
      </div>
      <div class="row">
        <button class="btn primary" id="btnResume">Resume</button>
        <button class="btn danger" id="btnQuit">Quit to Menu</button>
      </div>
    </div>

    <div class="screen" id="screenEnd">
      <div class="topline">
        <div>
          <h1 class="title">Journey complete</h1>
          <p class="subtitle">
            Du hast alle Siegel gesammelt. Jetzt kannst du die 2 Minuten Fazit mündlich machen.
            Diese Game-Version ersetzt nicht deine Reflexion, sie macht sie erlebbar.
          </p>
        </div>
        <div class="pill" id="endScorePill">Score: 0</div>
      </div>

      <div class="grid">
        <div class="card">
          <h3>Was die Klasse jetzt wissen sollte</h3>
          <p>
            Dein Projekt war nicht nur “ein Buch schreiben”. Es war Sprache, Struktur, Weltbau, Emotionsführung, Zeitmanagement und Durchhalten.
          </p>
        </div>
        <div class="card">
          <h3>Optionaler Schluss Satz</h3>
          <p>
            “Ich habe gelernt, dass Kreativität keine Stimmung ist, sondern ein System, das man sich aufbauen muss.”
          </p>
        </div>
      </div>

      <div class="row">
        <button class="btn" id="btnRestart">Play Again</button>
        <button class="btn" id="btnEndMenu">Menu</button>
      </div>
    </div>
  </div>

  <div class="modalOverlay" id="modalOverlay">
    <div class="modal">
      <div class="modalHeader">
        <div>
          <h2 id="modalTitle">Checkpoint</h2>
          <p id="modalText"></p>
        </div>
        <div class="row" style="margin-top:0;">
          <span class="pill" id="modalTag">Quest</span>
          <button class="btn" id="btnCloseModal">Close</button>
        </div>
      </div>

      <div class="modalBody">
        <div class="box">
          <h3>Explain</h3>
          <div class="small" id="modalExplain"></div>
        </div>

        <div class="box">
          <h3 id="qTitle">Frage</h3>
          <div class="small" id="qPrompt"></div>
          <div class="answers" id="answers"></div>
          <div class="feedback" id="feedback"></div>

          <div class="row" style="margin-top:12px;">
            <button class="btn" id="btnHint">Hint</button>
            <button class="btn primary" id="btnContinue" disabled>Continue</button>
          </div>
          <div class="small" id="hintBox" style="display:none; margin-top:10px;"></div>
        </div>
      </div>
    </div>
  </div>

  <div class="soundBtn" id="soundControls" style="display:none;">
    <span class="tag" id="soundStatus">Music: muted autoplay</span>
    <button class="btn primary" id="btnSoundOn">Sound on</button>
    <button class="btn" id="btnSoundOff">Mute</button>
  </div>

  <div class="ytWrap">
    <div id="ytPlayer"></div>
  </div>

  <script>
    // =========================
    // YouTube music (autoplay muted; user can unmute)
    // =========================
    // Your link: https://youtu.be/ai6Thls3EAE?...
    // Video id is "ai6Thls3EAE"
    const YT_VIDEO_ID = "ai6Thls3EAE";
    let ytPlayer = null;
    let ytReady = false;

    function loadYouTubeAPI(){
      const tag = document.createElement('script');
      tag.src = "https://www.youtube.com/iframe_api";
      document.body.appendChild(tag);
    }

    window.onYouTubeIframeAPIReady = function(){
      ytPlayer = new YT.Player('ytPlayer', {
        height: '1',
        width: '1',
        videoId: YT_VIDEO_ID,
        playerVars: {
          autoplay: 1,
          controls: 0,
          disablekb: 1,
          fs: 0,
          modestbranding: 1,
          rel: 0,
          loop: 1,
          playlist: YT_VIDEO_ID
        },
        events: {
          onReady: (ev) => {
            ytReady = true;
            try{
              ytPlayer.mute();
              ytPlayer.setVolume(60);
              ytPlayer.playVideo();
            }catch(e){}
          }
        }
      });
    };

    function musicUnmute(){
      if(!ytReady) return;
      try{
        ytPlayer.unMute();
        ytPlayer.playVideo();
      }catch(e){}
    }
    function musicMute(){
      if(!ytReady) return;
      try{
        ytPlayer.mute();
      }catch(e){}
    }

    // =========================
    // Game Data
    // =========================
    const settings = {
      hints: true,
      difficulty: "easy" // easy | normal
    };

    const characters = [
      {
        id:"velora",
        name:"Velora",
        label:"FMC",
        desc:"Black woman, romfantasy lead. Elegant medieval silhouette, black and white palette, pearls and gold details, long black hair with white highlights.",
        color:"#fbbf24",
        badge:"V"
      },
      {
        id:"azrael",
        name:"Azrael",
        label:"MMC",
        desc:"Asian (Korean) inspired. Black wings, a scar, quiet intensity. He moves like a secret.",
        color:"#60a5fa",
        badge:"A"
      },
      {
        id:"cecilia",
        name:"Cecilia",
        label:"Witch",
        desc:"White woman with red hair, witch energy. Sharp mind, sharper choices.",
        color:"#f87171",
        badge:"C"
      },
      {
        id:"micah",
        name:"Micah",
        label:"Ally",
        desc:"Indian woman, green eyes, bindi, jewel details, confident presence. She reads people fast.",
        color:"#34d399",
        badge:"M"
      }
    ];

    // 12 checkpoints total to stretch to ~20 minutes with movement + reading + choices
    // Questions in German, some reflection lines in English.
    const checkpoints = [
      // Intro: Welcome to Althrea
      {
        id:"cp0",
        region:"Althrea Gate",
        palette:{bg1:"#a7f3d0", bg2:"#93c5fd"},
        x: 260, y: 220,
        title:"Welcome to Althrea",
        text:"Willkommen. Die New Gods sind zufrieden, dass du Althrea gewählt hast.",
        explain: "Althrea wirkt wie ein perfektes Reich: glänzende Türme, Ordnung, Religion. Aber unter der Oberfläche gibt es eine ältere Welt. Das ist auch mein Schreibprozess: außen wirkt es wie “ich schreibe einfach”, innen war es Planung, Zweifel, Stil und Durchhalten.\n\nHard part (English): Writing is not only inspiration. It is a system you build.",
        q: {
          prompt:"Was ist das Hauptziel dieser Game-Reise?",
          answers:[
            "Nur die Handlung zusammenfassen",
            "Meine Schreib-Herausforderung erlebbar machen, mit Erklärungen und Fragen",
            "Nur die Charaktere vorstellen",
            "Nur KI-Tools zeigen"
          ],
          correct:1,
          hintDE:"Achte auf Titel und Untertitel: “Meine Herausforderung” und “erlebbar”.",
          hintEN:"Focus: process + hardship, not only plot."
        }
      },

      // Virellia: Glass Keep
      {
        id:"cp1",
        region:"Virellia: Glass Keep",
        palette:{bg1:"#fef3c7", bg2:"#ffffff"},
        x: 520, y: 220,
        title:"Virellia: The Glass Keep",
        text:"Du stehst vor dem Glass Keep. Es sieht aus wie pures Licht. Aber Glas kann auch schneiden.",
        explain:"Diese Station steht für den Anfang: die Idee ist da, alles wirkt machbar. In echt beginnt hier die Struktur: Anfang, Konflikt, Entwicklung. Ohne Struktur zerfällt Fantasy schnell.\n\nHard part (English): Worldbuilding without structure becomes pretty noise.",
        q:{
          prompt:"Welche Struktur meint man meist mit “Anfang, Konflikt, Entwicklung”?",
          answers:[
            "Nur schöne Beschreibungen",
            "Ein Story-Gerüst mit Steigerung und Wendepunkt",
            "Nur Dialoge schreiben",
            "Nur Charakter-Outfits planen"
          ],
          correct:1,
          hintDE:"Es geht um Plot-Aufbau, nicht um Deko.",
          hintEN:"Think: narrative arc."
        }
      },

      // Virellia: Garden (Motivation)
      {
        id:"cp2",
        region:"Virellia: Garden",
        palette:{bg1:"#fde68a", bg2:"#ffffff"},
        x: 720, y: 340,
        title:"Garden of Motivation",
        text:"Im Garten wirkt alles friedlich. Hier beginnt deine Motivation zu sprechen.",
        explain:"Meine Motivation kam aus Lesen: Romantasy hat mich gepackt, aber mir fehlten schwarze weibliche Hauptfiguren. BookTok hat das verstärkt. Ich wollte eine Welt schreiben, in der die schwarze FMC nicht Nebensatz ist, sondern Zentrum.\n\nHard part (English): Representation changes how deeply a reader can attach to a story.",
        q:{
          prompt:"Was war ein konkreter Auslöser für die ersten Ideen (2025)?",
          answers:[
            "Ein Mathe-Test",
            "Assassin’s Blade im Praktikum gelesen",
            "Ein Fußballspiel",
            "Ein Kochvideo"
          ],
          correct:1,
          hintDE:"Du hast es selbst genannt: Praktikum, Pause, Buch.",
          hintEN:"Trigger: reading during internship."
        }
      },

      // Virellia: Courtyard (Language)
      {
        id:"cp3",
        region:"Virellia: Courtyard",
        palette:{bg1:"#f8fafc", bg2:"#fde68a"},
        x: 820, y: 520,
        title:"Courtyard of Language",
        text:"Hier zählt jedes Wort. Höfisch nach außen, schwierig im Inneren.",
        explain:"Eine große Hürde war Sprache: Englisch, aber formeller, teilweise altmodischer Fantasy-Ton. Das ist nicht “normales Englisch”. Es ist Stil, Rhythmus, Wortwahl, Atmosphäre.\n\nHard part (English): Style is a constraint, not decoration.",
        q:{
          prompt:"Warum war die Sprache eine besondere Herausforderung?",
          answers:[
            "Weil Englisch gar nicht erlaubt war",
            "Weil Fantasy-Englisch einen anderen Stil hat als Alltagssprache",
            "Weil man keine Adjektive nutzen darf",
            "Weil Dialoge verboten sind"
          ],
          correct:1,
          hintDE:"Alltagssprache vs literarischer Ton.",
          hintEN:"Genre voice is different."
        }
      },

      // Virellia: Marketplace (Time + Motivation)
      {
        id:"cp4",
        region:"Virellia: Marketplace",
        palette:{bg1:"#ffffff", bg2:"#fbbf24"},
        x: 680, y: 660,
        title:"Marketplace of Time",
        text:"Viele Stimmen. Viele Aufgaben. Du musst entscheiden, was du zuerst trägst.",
        explain:"Zeitmanagement war real: Schule, Hausaufgaben, Freizeit und Schreiben. Und Motivation schwankt, besonders wenn Schreiben wie Pflicht wirkt.\n\nHard part (English): Discipline is what carries you when inspiration drops.",
        q:{
          prompt:"Welche zwei Faktoren haben sich hier besonders gebissen?",
          answers:[
            "Zeitmanagement und Motivation",
            "Wetter und Sport",
            "Musik und Mode",
            "Mathe und Kunst"
          ],
          correct:0,
          hintDE:"Du hast es wörtlich genannt: Zeitmanagement + Motivationsschwankungen.",
          hintEN:"Time + motivation friction."
        }
      },

      // Transition: South Gate to Blackwater
      {
        id:"cp5",
        region:"Road to the South",
        palette:{bg1:"#93c5fd", bg2:"#a7f3d0"},
        x: 480, y: 820,
        title:"Map of Houses",
        text:"Die Karte öffnet sich. Häuser, Regionen, Machtlinien.",
        explain:"Althrea hat Regionen und Häuser: Süden House Van Zwaenepoel und House Krynsor, Mid South-West House Veyrean und House Altharion (Royal House), North House Eliraen, East House Faerel. Das ist nicht nur Lore: es ist ein System, das Konflikte trägt.",
        q:{
          prompt:"Welches Haus ist das Royal House?",
          answers:[
            "House Faerel",
            "House Eliraen",
            "House Altharion",
            "House Krynsor"
          ],
          correct:2,
          hintDE:"Du hast es direkt gesagt: Altharion = royal.",
          hintEN:"Royal house = Altharion."
        }
      },

      // Blackwater Hold: Gate (tone shift)
      {
        id:"cp6",
        region:"Blackwater Hold: Gate",
        palette:{bg1:"#0f172a", bg2:"#7f1d1d"},
        x: 260, y: 980,
        title:"Blackwater Hold",
        text:"Das Licht wird härter. Schönheit wird menacing. Willkommen zu Veloras Wahrheit.",
        explain:"Blackwater Hold steht für den zweiten Teil meines Projekts: Tiefe. Nicht mehr nur “cool”, sondern konsequent. Hier geht es um Schatten, Wissen, Einfluss. Und beim Schreiben: konsistent bleiben, nicht ausweichen.",
        q:{
          prompt:"Wofür steht Blackwater Hold in deiner Reflexion symbolisch?",
          answers:[
            "Für Oberflächenglanz",
            "Für Tiefe, Konsequenz und Schatten-Themen",
            "Für ein Ferienhotel",
            "Für Comedy"
          ],
          correct:1,
          hintDE:"Es ist das Gegenteil der glänzenden Hauptstadt.",
          hintEN:"Shift from shine to truth."
        }
      },

      // Blackwater: Castle interior (writer's block)
      {
        id:"cp7",
        region:"Blackwater: Great Hall",
        palette:{bg1:"#111827", bg2:"#991b1b"},
        x: 520, y: 980,
        title:"The Great Hall of Blocks",
        text:"Die Halle ist still. Und genau da kam die Blockade.",
        explain:"Ich hatte Schreibblockaden und Phasen, wo Motivation weg war. Ein großer Punkt war: Gefühle, Atmosphäre und Spannung passend auszudrücken. Ich habe dann nicht “Text erzwungen”, sondern die emotionale Absicht der Szene notiert.\n\nHard part (English): When stuck, define the emotional target before writing words.",
        q:{
          prompt:"Welche Strategie hat dir geholfen, wenn du festgesteckt hast?",
          answers:[
            "Nichts machen und hoffen",
            "Nur neue Figuren erfinden",
            "Emotion und Atmosphäre einer Szene zuerst definieren",
            "Alles löschen und aufgeben"
          ],
          correct:2,
          hintDE:"Du hast es beschrieben: nicht erzwingen, erst Emotionen klären.",
          hintEN:"Emotional target first."
        }
      },

      // Blackwater: Swan Lake (rewriting + resilience)
      {
        id:"cp8",
        region:"Swan Lake",
        palette:{bg1:"#0b1020", bg2:"#f5f5f4"},
        x: 760, y: 980,
        title:"Swan Lake of Rewriting",
        text:"Wasser ist ruhig. Aber du weißt, was es gekostet hat, zurückzukommen.",
        explain:"Rückschläge waren Teil des Prozesses. Rewriting hat Disziplin und Resilienz trainiert. Und ich habe meine Muttersprache neu gelernt, aber diesmal emotionaler: Körpersprache, Intention, Subtext.",
        q:{
          prompt:"Was hast du durch Rückschläge besonders gelernt?",
          answers:[
            "Dass Talent reicht",
            "Dass Pausen und Planung helfen und Rückschläge normal sind",
            "Dass man nie überarbeitet",
            "Dass Figuren keine Gefühle brauchen"
          ],
          correct:1,
          hintDE:"Kombination aus: Pausen, Planung, Rückschläge normal.",
          hintEN:"Perseverance > talent."
        }
      },

      // Blackwater: Sea view (AI as support)
      {
        id:"cp9",
        region:"Southern Sea",
        palette:{bg1:"#0b1020", bg2:"#1f2937"},
        x: 900, y: 840,
        title:"Sea of Tools (KI)",
        text:"Tools sind wie Wasser: nützlich, aber du musst steuern.",
        explain:"KI war Unterstützung, kein Ersatz: Timeline, Synonyme, Hintergrunddetails (z.B. mittelalterliche Speisen), erstes Cover. Aber Entscheidungen und Bedeutung kamen von mir.\n\nHard part (English): AI can accelerate drafts, but it cannot be the author of meaning.",
        q:{
          prompt:"Welche Aussage passt am besten zu deinem KI-Einsatz?",
          answers:[
            "KI hat die Geschichte komplett geschrieben",
            "KI hat mich unterstützt, aber ich habe kritisch geprüft",
            "KI war verboten",
            "KI ersetzt Kreativität"
          ],
          correct:1,
          hintDE:"Unterstützung, kein Ersatz.",
          hintEN:"Assist, not replace."
        }
      },

      // Blackwater: Harbor (critical reflection)
      {
        id:"cp10",
        region:"Blackwater Harbor",
        palette:{bg1:"#1f2937", bg2:"#7f1d1d"},
        x: 980, y: 660,
        title:"Harbor of Reflection",
        text:"Hier schaust du zurück. Nicht nur auf Plot, sondern auf Aussage.",
        explain:"Kritische Reflexion: Ich hätte die Repräsentation schwarzer weiblicher Hauptfiguren in der Facharbeit noch tiefer analysieren sollen. Repräsentation beeinflusst Identifikation. Diversität wächst, aber viele Geschichten bleiben bei einer dominanten Identität.",
        q:{
          prompt:"Was würdest du rückblickend in der Facharbeit stärker vertiefen?",
          answers:[
            "Nur die Schriftart",
            "Das Thema Repräsentation schwarzer weiblicher Hauptfiguren",
            "Mehr Memes",
            "Weniger Reflexion"
          ],
          correct:1,
          hintDE:"Das steht direkt in deiner kritischen Reflexion.",
          hintEN:"Go deeper on representation."
        }
      },

      // End: Outlook
      {
        id:"cp11",
        region:"Outlook Ridge",
        palette:{bg1:"#0b0f1a", bg2:"#fbbf24"},
        x: 980, y: 420,
        title:"Fazit als Ausblick",
        text:"Du hast es geschafft. Jetzt kommt dein echtes Fazit, live gesprochen.",
        explain:"Dieses Projekt war eine der bedeutendsten Lernerfahrungen: Struktur, Geduld, Selbstreflexion. Übertragbar auf Schule, akademische Arbeit, persönliche Challenges. Und: Geschichten, in denen mehr Menschen sich sehen können, sind wichtig.",
        q:{
          prompt:"Welche Kernaussage passt am besten zu deinem Fazit?",
          answers:[
            "Kreativität braucht keine Struktur",
            "Kreative Arbeit braucht Struktur, Geduld und Selbstreflexion",
            "Planung macht alles langweilig",
            "Repräsentation spielt keine Rolle"
          ],
          correct:1,
          hintDE:"Dein Fazit nennt genau diese drei Dinge.",
          hintEN:"Structure + patience + reflection."
        }
      }
    ];

    // =========================
    // UI helpers
    // =========================
    const $ = (id)=>document.getElementById(id);
    const screens = {
      menu: $("screenMenu"),
      rules: $("screenRules"),
      settings: $("screenSettings"),
      character: $("screenCharacter"),
      pause: $("screenPause"),
      end: $("screenEnd")
    };
    function showScreen(name){
      for(const k in screens) screens[k].classList.remove("active");
      screens[name].classList.add("active");
    }

    // Character UI
    const charGrid = $("charGrid");
    let selectedChar = null;
    function renderChars(){
      charGrid.innerHTML = "";
      for(const c of characters){
        const el = document.createElement("div");
        el.className = "card";
        el.style.cursor = "pointer";
        el.innerHTML = `
          <h3>${c.name} <span style="color:var(--muted); font-weight:600;">(${c.label})</span></h3>
          <p>${c.desc}</p>
          <div class="row" style="margin-top:10px;">
            <span class="pill" style="color:${c.color}; border-color: rgba(255,255,255,.10);">Avatar: ${c.badge}</span>
            <span class="pill">Theme: ${c.id}</span>
          </div>
        `;
        el.addEventListener("click", ()=>{
          selectedChar = c;
          for(const child of charGrid.children){
            child.style.outline = "none";
          }
          el.style.outline = "2px solid rgba(251,191,36,.65)";
          $("btnStartGame").disabled = false;
        });
        charGrid.appendChild(el);
      }
    }

    // =========================
    // Core game
    // =========================
    const canvas = $("game");
    const ctx = canvas.getContext("2d");

    function resize(){
      canvas.width = Math.floor(window.innerWidth * devicePixelRatio);
      canvas.height = Math.floor(window.innerHeight * devicePixelRatio);
      ctx.setTransform(devicePixelRatio,0,0,devicePixelRatio,0,0);
    }
    window.addEventListener("resize", resize);
    resize();

    const state = {
      running:false,
      paused:false,
      showHints:true,
      score:0,
      currentIndex:0,
      unlocked:0,
      visited:new Set(),
      player:{
        x: 140, y: 220, r: 16,
        vx:0, vy:0,
        speed: 2.2
      },
      camera:{
        x:0, y:0
      },
      world:{
        w: 1200,
        h: 1100
      },
      regionName:"-"
    };

    // Simple world layout "track"
    // Walls as rectangles (x,y,w,h). The idea is a guided path like a race route.
    const walls = [
      // Outer bounds
      {x:-40,y:-40,w:1280,h:40},{x:-40,y:1100,w:1280,h:40},
      {x:-40,y:-40,w:40,h:1180},{x:1200,y:-40,w:40,h:1180},

      // Path separators
      {x: 320, y: 0, w: 20, h: 480},
      {x: 640, y: 0, w: 20, h: 240},
      {x: 860, y: 360, w: 20, h: 260},
      {x: 640, y: 560, w: 20, h: 320},
      {x: 340, y: 760, w: 20, h: 380},
      {x: 680, y: 900, w: 20, h: 240}
    ];

    // Input
    const keys = new Set();
    window.addEventListener("keydown",(e)=>{
      if(["ArrowUp","ArrowDown","ArrowLeft","ArrowRight","w","a","s","d","W","A","S","D"].includes(e.key)){
        keys.add(e.key.toLowerCase());
        e.preventDefault();
      }
      if(e.key === "Escape" && state.running){
        togglePause(true);
      }
    });
    window.addEventListener("keyup",(e)=>{
      keys.delete(e.key.toLowerCase());
    });

    function clamp(v,min,max){return Math.max(min, Math.min(max, v));}

    function circleRectCollide(cx, cy, r, rect){
      const closestX = clamp(cx, rect.x, rect.x + rect.w);
      const closestY = clamp(cy, rect.y, rect.y + rect.h);
      const dx = cx - closestX;
      const dy = cy - closestY;
      return (dx*dx + dy*dy) < r*r;
    }

    function movePlayer(){
      const p = state.player;
      let dx = 0, dy = 0;
      if(keys.has("arrowup") || keys.has("w")) dy -= 1;
      if(keys.has("arrowdown") || keys.has("s")) dy += 1;
      if(keys.has("arrowleft") || keys.has("a")) dx -= 1;
      if(keys.has("arrowright") || keys.has("d")) dx += 1;

      const len = Math.hypot(dx,dy) || 1;
      dx = dx/len * p.speed;
      dy = dy/len * p.speed;

      // attempt X
      let nx = p.x + dx;
      let ny = p.y;
      for(const w of walls){
        if(circleRectCollide(nx, ny, p.r, w)){
          nx = p.x; break;
        }
      }
      // attempt Y
      ny = p.y + dy;
      for(const w of walls){
        if(circleRectCollide(nx, ny, p.r, w)){
          ny = p.y; break;
        }
      }
      p.x = clamp(nx, 0, state.world.w);
      p.y = clamp(ny, 0, state.world.h);
    }

    function currentPalette(){
      // palette of next checkpoint region for vibe
      const idx = Math.min(state.unlocked, checkpoints.length-1);
      return checkpoints[idx].palette;
    }

    function drawBackground(){
      const pal = currentPalette();
      const grad = ctx.createLinearGradient(0,0,window.innerWidth,window.innerHeight);
      grad.addColorStop(0, pal.bg1);
      grad.addColorStop(1, pal.bg2);
      ctx.fillStyle = grad;
      ctx.fillRect(0,0,window.innerWidth,window.innerHeight);

      // subtle noise dots
      ctx.globalAlpha = 0.08;
      ctx.fillStyle = "#000";
      for(let i=0;i<220;i++){
        const x = (Math.random()*window.innerWidth)|0;
        const y = (Math.random()*window.innerHeight)|0;
        ctx.fillRect(x,y,1,1);
      }
      ctx.globalAlpha = 1;
    }

    function drawWorld(){
      const cam = state.camera;
      const p = state.player;

      // camera follows player
      cam.x = clamp(p.x - window.innerWidth/2, 0, state.world.w - window.innerWidth);
      cam.y = clamp(p.y - window.innerHeight/2, 0, state.world.h - window.innerHeight);

      ctx.save();
      ctx.translate(-cam.x, -cam.y);

      // Track floor
      ctx.globalAlpha = 0.25;
      ctx.fillStyle = "#000";
      ctx.fillRect(0,0,state.world.w,state.world.h);
      ctx.globalAlpha = 1;

      // Walls
      ctx.fillStyle = "rgba(255,255,255,.10)";
      for(const w of walls){
        ctx.fillRect(w.x,w.y,w.w,w.h);
      }

      // Checkpoints
      for(let i=0;i<checkpoints.length;i++){
        const cp = checkpoints[i];
        const isUnlocked = (i <= state.unlocked);
        const isDone = state.visited.has(cp.id);

        const baseR = 22;
        const pulse = 2 + Math.sin(Date.now()/220) * 2;
        const r = baseR + pulse;

        // lock visual
        if(!isUnlocked){
          ctx.globalAlpha = 0.22;
          ctx.fillStyle = "#9ca3af";
          ctx.beginPath();
          ctx.arc(cp.x, cp.y, baseR, 0, Math.PI*2);
          ctx.fill();
          ctx.globalAlpha = 1;
          continue;
        }

        ctx.globalAlpha = isDone ? 0.35 : 1;
        ctx.strokeStyle = isDone ? "rgba(34,197,94,.65)" : "rgba(251,191,36,.9)";
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.arc(cp.x, cp.y, r, 0, Math.PI*2);
        ctx.stroke();

        ctx.fillStyle = isDone ? "rgba(34,197,94,.22)" : "rgba(251,191,36,.18)";
        ctx.beginPath();
        ctx.arc(cp.x, cp.y, baseR, 0, Math.PI*2);
        ctx.fill();

        ctx.fillStyle = "rgba(255,255,255,.8)";
        ctx.font = "12px ui-sans-serif, system-ui";
        ctx.fillText(`${i+1}`, cp.x-4, cp.y+4);
        ctx.globalAlpha = 1;
      }

      // Player sprite
      const c = selectedChar || characters[0];
      ctx.fillStyle = c.color;
      ctx.beginPath();
      ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
      ctx.fill();

      // Player badge
      ctx.fillStyle = "rgba(0,0,0,.65)";
      ctx.font = "bold 12px ui-sans-serif, system-ui";
      ctx.fillText(c.badge, p.x-5, p.y+4);

      ctx.restore();
    }

    function checkCheckpoint(){
      const p = state.player;
      for(let i=0;i<=state.unlocked;i++){
        const cp = checkpoints[i];
        if(state.visited.has(cp.id)) continue;
        const d = Math.hypot(p.x - cp.x, p.y - cp.y);
        if(d < 26){
          openCheckpoint(i);
          break;
        }
      }
    }

    // =========================
    // Modal checkpoint logic
    // =========================
    const overlay = $("modalOverlay");
    const modalTitle = $("modalTitle");
    const modalText = $("modalText");
    const modalExplain = $("modalExplain");
    const qPrompt = $("qPrompt");
    const answersBox = $("answers");
    const feedback = $("feedback");
    const hintBox = $("hintBox");
    const btnContinue = $("btnContinue");

    let activeCheckpointIndex = null;
    let answeredCorrect = false;
    let wrongCount = 0;

    function openCheckpoint(i){
      activeCheckpointIndex = i;
      answeredCorrect = false;
      wrongCount = 0;
      btnContinue.disabled = true;
      feedback.className = "feedback";
      feedback.style.display = "none";
      hintBox.style.display = "none";

      const cp = checkpoints[i];
      modalTitle.textContent = `${cp.title}`;
      modalText.textContent = cp.text;
      modalExplain.textContent = cp.explain;

      qPrompt.textContent = cp.q.prompt;
      answersBox.innerHTML = "";
      cp.q.answers.forEach((a, idx)=>{
        const b = document.createElement("button");
        b.className = "answerBtn";
        b.textContent = a;
        b.addEventListener("click", ()=>submitAnswer(idx));
        answersBox.appendChild(b);
      });

      $("modalTag").textContent = `${cp.region} • Quest ${i+1}/${checkpoints.length}`;

      overlay.classList.add("active");
      state.paused = true;
    }

    function submitAnswer(idx){
      const cp = checkpoints[activeCheckpointIndex];
      const correct = (idx === cp.q.correct);

      if(correct){
        answeredCorrect = true;
        state.score += 10;
        feedback.className = "feedback ok";
        feedback.style.display = "block";
        feedback.textContent = "Richtig. Siegel erhalten. Der Weg öffnet sich.";
        btnContinue.disabled = false;
        updateHUD();
        return;
      }

      wrongCount++;
      feedback.className = "feedback bad";
      feedback.style.display = "block";
      state.score = Math.max(0, state.score - 2);
      updateHUD();

      if(settings.difficulty === "easy" && wrongCount >= 1){
        feedback.textContent = "Nicht ganz. Tipp: Nutze den Hint Button. (Easy Mode gibt dir mehr Hilfe.)";
      }else{
        feedback.textContent = "Nicht ganz. Versuch es noch einmal.";
      }
    }

    function closeModal(force=false){
      if(!force && !answeredCorrect) return;
      overlay.classList.remove("active");
      state.paused = false;
    }

    $("btnCloseModal").addEventListener("click", ()=>closeModal(false));
    btnContinue.addEventListener("click", ()=>{
      const cp = checkpoints[activeCheckpointIndex];
      state.visited.add(cp.id);
      if(state.unlocked < checkpoints.length-1){
        state.unlocked = Math.max(state.unlocked, activeCheckpointIndex + 1);
      }
      closeModal(true);

      if(state.visited.size >= checkpoints.length){
        endGame();
      }
      updateHUD();
    });

    $("btnHint").addEventListener("click", ()=>{
      if(!settings.hints) return;
      const cp = checkpoints[activeCheckpointIndex];
      hintBox.style.display = "block";
      hintBox.textContent = `Hint (DE): ${cp.q.hintDE}\n\nHint (EN): ${cp.q.hintEN}`;
    });

    // =========================
    // HUD
    // =========================
    function updateHUD(){
      $("hudChar").textContent = `Char: ${(selectedChar && selectedChar.name) || "-"}`;
      $("hudScore").textContent = `Score: ${state.score}`;
      $("hudRegion").textContent = `Region: ${checkpoints[Math.min(state.unlocked, checkpoints.length-1)].region}`;
      $("hudQuest").textContent = `Quest: ${state.visited.size}/${checkpoints.length}`;
      $("hudProgress").style.width = `${(state.visited.size / checkpoints.length) * 100}%`;
      $("endScorePill").textContent = `Score: ${state.score}`;
    }

    // =========================
    // Flow: menu, pause, end
    // =========================
    function startGame(){
      state.running = true;
      state.paused = false;
      state.score = 0;
      state.unlocked = 0;
      state.visited = new Set();
      state.player.x = 140;
      state.player.y = 220;

      $("hud").style.display = "flex";
      $("kbd").style.display = "flex";
      $("soundControls").style.display = "flex";
      updateHUD();
    }

    function endGame(){
      state.running = false;
      $("hud").style.display = "none";
      $("kbd").style.display = "none";
      showScreen("end");
    }

    function togglePause(on){
      if(!state.running) return;
      state.paused = on;
      showScreen(on ? "pause" : "none");
      if(on){
        for(const k in screens) screens[k].classList.remove("active");
        screens.pause.classList.add("active");
      }else{
        for(const k in screens) screens[k].classList.remove("active");
        // no overlay screen while game running
      }
    }

    $("btnPause").addEventListener("click", ()=>togglePause(true));
    $("btnResume").addEventListener("click", ()=>{
      for(const k in screens) screens[k].classList.remove("active");
      state.paused = false;
    });
    $("btnQuit").addEventListener("click", ()=>{
      state.running = false;
      state.paused = false;
      $("hud").style.display = "none";
      $("kbd").style.display = "none";
      $("soundControls").style.display = "none";
      showScreen("menu");
    });

    $("btnRestart").addEventListener("click", ()=>{
      showScreen("character");
      renderChars();
    });
    $("btnEndMenu").addEventListener("click", ()=>{
      showScreen("menu");
    });

    // Menu buttons
    $("btnPlay").addEventListener("click", ()=>{
      showScreen("character");
      renderChars();
    });
    $("btnRules").addEventListener("click", ()=>showScreen("rules"));
    $("btnSettings").addEventListener("click", ()=>showScreen("settings"));
    $("btnBackFromRules").addEventListener("click", ()=>showScreen("menu"));
    $("btnBackFromSettings").addEventListener("click", ()=>showScreen("menu"));
    $("btnBackFromChar").addEventListener("click", ()=>showScreen("menu"));

    $("btnStartGame").addEventListener("click", ()=>{
      // remove screens
      for(const k in screens) screens[k].classList.remove("active");
      startGame();
    });

    // Settings buttons
    $("btnToggleHints").addEventListener("click", ()=>{
      settings.hints = !settings.hints;
      $("btnToggleHints").textContent = `Hints: ${settings.hints ? "ON" : "OFF"}`;
    });
    $("btnDiff").addEventListener("click", ()=>{
      settings.difficulty = (settings.difficulty === "easy") ? "normal" : "easy";
      $("btnDiff").textContent = `Difficulty: ${settings.difficulty === "easy" ? "Easy" : "Normal"}`;
    });

    // Sound controls
    $("btnSoundOn").addEventListener("click", ()=>{
      musicUnmute();
      $("soundStatus").textContent = "Music: on";
    });
    $("btnSoundOff").addEventListener("click", ()=>{
      musicMute();
      $("soundStatus").textContent = "Music: muted";
    });

    // Close modal by clicking outside (only if answered correct)
    overlay.addEventListener("click", (e)=>{
      if(e.target === overlay) closeModal(false);
    });

    // =========================
    // Game loop
    // =========================
    function loop(){
      requestAnimationFrame(loop);

      // if any UI screen is active (menu, rules, settings, character, pause, end), do not run gameplay
      const anyScreenActive = Object.values(screens).some(s=>s.classList.contains("active"));
      if(anyScreenActive){
        // render a subtle background anyway
        drawBackground();
        return;
      }

      drawBackground();

      if(state.running && !state.paused){
        movePlayer();
        checkCheckpoint();
      }

      drawWorld();
    }

    // =========================
    // Boot
    // =========================
    loadYouTubeAPI();
    loop();
  </script>
</body>
</html>
