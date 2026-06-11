<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Schatkaart — 12 maanden, 12 dates</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IM+Fell+English+SC&family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&display=swap" rel="stylesheet">
<style>
  :root{
    --parchment:#ead9b4;
    --parchment-deep:#dcc28e;
    --ink:#3b2a1a;
    --ink-soft:#6b5538;
    --rood:#9c3322;        /* lakzegel-rood */
    --goud:#b58a3c;
    --route:#5a4226;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html,body{height:100%;}
  body{
    font-family:'EB Garamond',serif;
    color:var(--ink);
    background:
      radial-gradient(ellipse at 20% 10%, rgba(255,250,235,.55), transparent 55%),
      radial-gradient(ellipse at 85% 90%, rgba(120,84,40,.28), transparent 60%),
      radial-gradient(ellipse at 50% 50%, var(--parchment), var(--parchment-deep) 85%);
    min-height:100vh;
  }
  /* subtle paper grain */
  body::before{
    content:"";position:fixed;inset:0;pointer-events:none;opacity:.5;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2'/%3E%3CfeColorMatrix values='0 0 0 0 0.27 0 0 0 0 0.2 0 0 0 0 0.12 0 0 0 0.05 0'/%3E%3C/filter%3E%3Crect width='160' height='160' filter='url(%23n)'/%3E%3C/svg%3E");
  }
  .wrap{max-width:560px;margin:0 auto;padding:28px 18px 90px;position:relative;}

  header{text-align:center;margin-bottom:6px;}
  .eyebrow{
    font-size:13px;letter-spacing:.22em;text-transform:uppercase;color:var(--ink-soft);
  }
  h1{
    font-family:'IM Fell English SC',serif;font-weight:400;
    font-size:clamp(34px,9vw,46px);line-height:1.05;margin:6px 0 4px;
  }
  .names{
    font-size:20px;font-style:italic;color:var(--rood);
  }
  .intro{
    margin:14px auto 4px;max-width:46ch;font-size:17px;line-height:1.5;color:var(--ink-soft);
  }
  .intro b{color:var(--ink);font-weight:600;}

  .map{display:block;width:100%;height:auto;margin-top:6px;}

  /* route */
  .route{
    fill:none;stroke:var(--route);stroke-width:3.5;
    stroke-dasharray:1 11;stroke-linecap:round;opacity:.85;
  }

  /* waypoints — wax-seal medallions */
  .dot{cursor:pointer;}
  .dot circle.seal{
    fill:var(--rood);
    stroke:#6e2014;stroke-width:1.5;
    filter:drop-shadow(1px 2px 1.5px rgba(40,20,5,.45));
    transition:transform .15s ease;
    transform-box:fill-box;transform-origin:center;
  }
  .dot:hover circle.seal,.dot:focus-visible circle.seal{transform:scale(1.12);}
  .dot:focus-visible{outline:none;}
  .dot text{
    font-family:'IM Fell English SC',serif;font-size:21px;fill:#f6e7c8;
    text-anchor:middle;dominant-baseline:central;pointer-events:none;
  }
  .dot.done circle.seal{fill:var(--goud);stroke:#7c5c22;}
  .dot .tick{display:none;}
  .dot.done .tick{display:block;}
  .dot.done .num{display:none;}

  .xspot text{
    font-family:'IM Fell English SC',serif;fill:var(--rood);
  }
  .deco{fill:none;stroke:var(--ink-soft);stroke-width:1.6;opacity:.8;}
  .deco-fill{fill:var(--ink-soft);opacity:.8;}
  .maplabel{
    font-family:'IM Fell English SC',serif;fill:var(--ink-soft);font-size:15px;
  }

  /* date card (bottom sheet) */
  .scrim{
    position:fixed;inset:0;background:rgba(35,22,8,.45);
    opacity:0;pointer-events:none;transition:opacity .25s ease;z-index:9;
  }
  .card{
    position:fixed;left:50%;bottom:0;transform:translate(-50%,105%);
    width:min(560px,100%);z-index:10;
    background:
      radial-gradient(ellipse at 50% 0%, #f7ecd2, #eeddb6 75%);
    border-radius:18px 18px 0 0;
    border:1.5px solid var(--goud);border-bottom:none;
    box-shadow:0 -14px 40px rgba(40,20,5,.35);
    padding:26px 24px 30px;text-align:center;
    transition:transform .3s cubic-bezier(.2,.9,.3,1);
  }
  .open .scrim{opacity:1;pointer-events:auto;}
  .open .card{transform:translate(-50%,0);}
  .card .maand{
    font-size:13px;letter-spacing:.22em;text-transform:uppercase;color:var(--ink-soft);
  }
  .card h2{
    font-family:'IM Fell English SC',serif;font-weight:400;
    font-size:30px;margin:8px 0 10px;line-height:1.1;
  }
  .card p{font-size:17.5px;line-height:1.55;color:var(--ink-soft);max-width:42ch;margin:0 auto;}
  .envelop{
    display:inline-block;margin-top:16px;padding:10px 18px;
    border:1.5px dashed var(--rood);border-radius:10px;
    color:var(--rood);font-weight:600;font-size:16px;
  }
  .acties{display:flex;gap:10px;justify-content:center;margin-top:18px;flex-wrap:wrap;}
  .knop{
    font-family:'EB Garamond',serif;font-size:16px;font-weight:600;
    padding:10px 20px;border-radius:999px;cursor:pointer;
    border:1.5px solid var(--ink-soft);background:transparent;color:var(--ink);
    transition:background .15s ease;
  }
  .knop:hover{background:rgba(107,85,56,.12);}
  .knop.primair{background:var(--rood);border-color:#6e2014;color:#f6e7c8;}
  .knop.primair:hover{background:#86291a;}
  .knop:focus-visible{outline:2px solid var(--rood);outline-offset:2px;}

  footer{
    text-align:center;margin-top:8px;font-size:14.5px;color:var(--ink-soft);font-style:italic;
  }
  @media (prefers-reduced-motion:reduce){
    .card,.scrim,.dot circle.seal{transition:none;}
  }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="eyebrow">Een cadeau voor jullie eerste huwelijksjaar</div>
    <h1>De Schatkaart</h1>
    <div class="names" id="namen"></div>
    <p class="intro">
      Hoera, jullie zijn getrouwd — en vandaag begint het grootste avontuur!
      Deze kaart wijst de weg. In de doos liggen twaalf envelopjes: één date voor
      iedere maand van jullie eerste huwelijksjaar. <b>Tik op een stip</b>, open het
      envelopje met hetzelfde nummer, en ga samen op schattenjacht. Veel plezier onderweg!
    </p>
  </header>

  <svg class="map" id="kaart" viewBox="0 0 600 1020" role="list" aria-label="Schatkaart met twaalf dates">
    <!-- decorations -->
    <g class="kompas" transform="translate(520,80)">
      <circle class="deco" r="30"/>
      <circle class="deco" r="4"/>
      <path class="deco-fill" d="M0,-26 L6,0 L0,8 L-6,0 Z"/>
      <path class="deco" d="M-21,-21 L21,21 M21,-21 L-21,21" stroke-width="1"/>
      <text class="maplabel" x="0" y="-38" text-anchor="middle">N</text>
    </g>
    <g transform="translate(70,60)">
      <text class="maplabel">Hier begint</text>
      <text class="maplabel" y="18">het avontuur →</text>
    </g>
    <!-- doodles in mapstijl -->
    <g class="deco-fill" opacity=".55">
      <path d="M538,372 c-5,-7 -16,-2 -12,6 c2,5 12,11 12,11 c0,0 10,-6 12,-11 c4,-8 -7,-13 -12,-6 Z"/>
    </g>

    <!-- scheepje met hartenzeil -->
    <g class="deco" transform="translate(350,58)">
      <path d="M-26,6 Q0,18 26,6 L18,-2 L-18,-2 Z"/>
      <path d="M0,-2 L0,-30"/>
      <path d="M2,-28 L2,-8 L20,-11 Z"/>
      <path class="deco-fill" d="M9,-21 c-2,-3 -7,-1 -5,3 c1,2 5,4 5,4 c0,0 4,-2 5,-4 c2,-4 -3,-6 -5,-3 Z" stroke="none"/>
      <path d="M-34,14 q6,-5 12,0 q6,5 12,0 q6,-5 12,0 q6,5 12,0"/>
    </g>
    <!-- meeuwen -->
    <g class="deco" transform="translate(420,92)" stroke-width="1.3">
      <path d="M0,0 q5,-6 10,0 q5,-6 10,0"/>
      <path d="M28,-12 q4,-5 8,0 q4,-5 8,0"/>
    </g>

    <!-- pijl door hart -->
    <g class="deco" transform="translate(78,282)">
      <path class="deco-fill" d="M0,-2 c-6,-9 -20,-2 -15,8 c3,6 15,13 15,13 c0,0 12,-7 15,-13 c5,-10 -9,-17 -15,-8 Z" stroke="none"/>
      <path d="M-26,16 L28,-13 M28,-13 l-8,1 M28,-13 l-2,8 M-26,16 l-2,7 M-26,16 l-7,2"/>
    </g>

    <!-- zeemonster -->
    <g class="deco" transform="translate(548,440)">
      <path d="M-34,0 q8,-18 16,0"/>
      <path d="M-10,0 q8,-18 16,0"/>
      <path d="M14,0 q2,-16 12,-14 q8,2 3,9"/>
      <circle class="deco-fill" cx="24" cy="-9" r="1.8" stroke="none"/>
      <path d="M-40,8 q6,-4 12,0 q6,4 12,0 q6,-4 12,0 q6,4 12,0"/>
    </g>
    <text class="maplabel" x="548" y="468" text-anchor="middle" font-size="12.5">de Zee van Vlinders</text>
    <text class="maplabel" x="548" y="482" text-anchor="middle" font-size="11">(in de buik)</text>

    <!-- moeras der vuile sokken -->
    <g class="deco" transform="translate(56,640)">
      <path d="M-18,12 l3,-9 M-14,12 l0,-11 M-10,12 l3,-9"/>
      <path d="M12,14 l3,-8 M16,14 l0,-10 M20,14 l-3,-8"/>
      <path d="M-3,-16 l8,0 0,9 q0,3 3,5 l6,4 q3,3 -1,5 l-7,0 q-9,-1 -9,-9 Z"/>
    </g>
    <text class="maplabel" x="56" y="672" text-anchor="middle" font-size="12.5">het Moeras der</text>
    <text class="maplabel" x="56" y="686" text-anchor="middle" font-size="12.5">Vuile Sokken</text>

    <!-- eiland van de eeuwige afwas -->
    <g class="deco" transform="translate(545,860)">
      <path d="M-24,12 Q0,0 24,12"/>
      <path d="M4,10 q-3,-14 -8,-22"/>
      <path d="M-4,-12 q-12,-6 -18,0 M-4,-12 q-2,-12 8,-14 M-4,-12 q12,-4 16,4"/>
      <circle class="deco-fill" cx="-1" cy="-8" r="2" stroke="none"/>
      <path d="M-8,18 a7,5 0 0 0 14,0 M-5,21 l1,5 M5,21 l-1,5"/>
    </g>
    <text class="maplabel" x="545" y="900" text-anchor="middle" font-size="12.5">het Eiland van</text>
    <text class="maplabel" x="545" y="914" text-anchor="middle" font-size="12.5">de Eeuwige Afwas</text>

    <!-- de schat: X en kist bij stip 12 -->
    <text class="xspot" x="174" y="918" font-size="40">✕</text>
    <g class="deco" transform="translate(232,916)">
      <rect x="-18" y="-6" width="36" height="20" rx="3"/>
      <path d="M-18,-6 q18,-18 36,0"/>
      <circle class="deco-fill" cy="2" r="2.5" stroke="none"/>
      <path d="M0,4 l0,4"/>
      <path d="M-27,-20 l5,5 M-22,-20 l-5,5 M24,-26 l5,5 M29,-26 l-5,5"/>
      <circle cx="-26" cy="16" r="2.5"/><circle cx="26" cy="13" r="2.5"/>
    </g>
    <text class="maplabel" x="225" y="948" text-anchor="middle">hier ligt de schat</text>

    <path class="route" id="route"/>
    <g id="stippen"></g>
  </svg>

  <footer>“De schat was niet het goud, maar de tijd samen.”</footer>
</div>

<!-- date card -->
<div class="scrim" id="scrim" onclick="sluitKaart()"></div>
<div class="card" id="dateskaart" role="dialog" aria-modal="true" aria-labelledby="kaart-titel">
  <div class="maand" id="kaart-maand"></div>
  <h2 id="kaart-titel"></h2>
  <p id="kaart-tekst"></p>
  <div class="envelop" id="kaart-envelop"></div>
  <div class="acties">
    <button class="knop primair" id="knop-gedaan" onclick="toggleGedaan()"></button>
    <button class="knop" onclick="sluitKaart()">Sluiten</button>
  </div>
</div>

<script>
/* ════════════════════════════════════════════════════════════════
   PAS HIER ALLES AAN ✏️
   Verander de namen en de teksten van de twaalf dates hieronder.
   ════════════════════════════════════════════════════════════════ */

const NAMEN = "Voor het bruidspaar";   // bijv. "Anna & Thomas — 14 juni 2026"

const DATES = [
  { titel:"IJsje halen",            tekst:"Loop samen naar de lekkerste ijssalon in de buurt en proef elkaars smaak. De zomer (of winter!) van jullie eerste maand als getrouwd stel." },
  { titel:"Picknick in het park",   tekst:"Pak een kleedje, wat lekkers en zoek een mooi plekje. Telefoon op stil — alleen jullie twee." },
  { titel:"Filmavond",              tekst:"Naar de bioscoop, of thuis met popcorn en dekentjes. Om de beurt mag iemand de film kiezen." },
  { titel:"Uit eten",               tekst:"Reserveer een tafel bij een restaurant waar jullie nog nooit zijn geweest. Proost op jullie kersverse huwelijk!" },
  { titel:"Wandeling & koffie",     tekst:"Een flinke wandeling door bos, duin of stad, met onderweg een warme koffie of chocolademelk." },
  { titel:"Museum of uitje",        tekst:"Kies samen een museum, tentoonstelling of bezienswaardigheid en speel toerist in eigen land." },
  { titel:"Dagje strand of water",  tekst:"Zon, zee, strand — of een dagje aan een meer. Vergeet de zonnebrand (of de warme chocolademelk) niet." },
  { titel:"Spelletjesavond",        tekst:"Haal snacks in huis en speel jullie favoriete spellen. De verliezer doet een week de afwas…" },
  { titel:"Ontbijtje buiten de deur", tekst:"Begin de dag goed: croissantjes, verse jus en alle tijd van de wereld." },
  { titel:"Fietstocht",             tekst:"Stippel een route uit en stap op de fiets. Onderweg ergens stoppen voor appelgebak is verplicht." },
  { titel:"Wellness of schaatsbaan", tekst:"Even helemaal ontspannen in de sauna — of juist samen zwieren op het ijs. Kies wat bij het seizoen past." },
  { titel:"Jubileumdiner ✕",        tekst:"Eén jaar getrouwd! Vier het groots met een feestelijk diner en blik samen terug op alle dates van dit jaar. Hier ligt de schat." },
];

/* ════════════════════════════════════════════════════════════════ */

document.getElementById('namen').textContent = NAMEN;

// waypoints along a winding route (x,y in the 600×1020 viewBox)
const PUNTEN = [
  [110,150],[280,185],[455,150],[510,300],[345,345],[150,395],
  [120,545],[315,580],[485,545],[460,710],[255,765],[130,905]
];

// smooth route through the points
const r = PUNTEN;
let d = `M ${r[0][0]},${r[0][1]}`;
for(let i=1;i<r.length;i++){
  const [px,py]=r[i-1],[x,y]=r[i];
  const mx=(px+x)/2, my=(py+y)/2;
  d += ` Q ${px},${py} ${mx},${my}`;
}
d += ` T ${r[r.length-1][0]},${r[r.length-1][1]}`;
document.getElementById('route').setAttribute('d', d);

// month names
const MAANDEN = ["eerste","tweede","derde","vierde","vijfde","zesde","zevende",
                 "achtste","negende","tiende","elfde","twaalfde"];

// progress remembered on this phone
let gedaan = [];
try { gedaan = JSON.parse(localStorage.getItem('schatkaart-gedaan')||'[]'); } catch(e){ gedaan = []; }
function bewaar(){ try{ localStorage.setItem('schatkaart-gedaan', JSON.stringify(gedaan)); }catch(e){} }

// build the dots
const svgNS = "http://www.w3.org/2000/svg";
const groep = document.getElementById('stippen');
PUNTEN.forEach(([x,y],i)=>{
  const g = document.createElementNS(svgNS,'g');
  g.setAttribute('class','dot'+(gedaan.includes(i)?' done':''));
  g.setAttribute('transform',`translate(${x},${y})`);
  g.setAttribute('tabindex','0');
  g.setAttribute('role','listitem');
  g.setAttribute('aria-label',`Date ${i+1}: ${DATES[i].titel}`);
  g.dataset.i = i;

  const c = document.createElementNS(svgNS,'circle');
  c.setAttribute('class','seal'); c.setAttribute('r','24');
  // slightly irregular wax-seal edge
  const rim = document.createElementNS(svgNS,'circle');
  rim.setAttribute('r','27.5'); rim.setAttribute('fill','none');
  rim.setAttribute('stroke','rgba(110,32,20,.35)'); rim.setAttribute('stroke-width','2');
  rim.setAttribute('stroke-dasharray','6 5 2 4 7 3');

  const t = document.createElementNS(svgNS,'text');
  t.setAttribute('class','num'); t.textContent = i+1;

  const tick = document.createElementNS(svgNS,'path');
  tick.setAttribute('class','tick');
  tick.setAttribute('d','M-9,1 L-3,8 L10,-8');
  tick.setAttribute('fill','none'); tick.setAttribute('stroke','#f6e7c8');
  tick.setAttribute('stroke-width','3.5'); tick.setAttribute('stroke-linecap','round');
  tick.setAttribute('stroke-linejoin','round');

  g.append(rim,c,t,tick);
  g.addEventListener('click',()=>openKaart(i));
  g.addEventListener('keydown',e=>{ if(e.key==='Enter'||e.key===' '){e.preventDefault();openKaart(i);} });
  groep.appendChild(g);
});

// card logic
let actief = null;
function openKaart(i){
  actief = i;
  document.getElementById('kaart-maand').textContent = `Maand ${i+1} · de ${MAANDEN[i]} maand`;
  document.getElementById('kaart-titel').textContent = DATES[i].titel;
  document.getElementById('kaart-tekst').textContent = DATES[i].tekst;
  document.getElementById('kaart-envelop').textContent = `✉ Pak envelopje nr. ${i+1} uit de doos`;
  document.getElementById('knop-gedaan').textContent =
    gedaan.includes(i) ? '↺ Toch nog niet gedaan' : '✓ Gedaan!';
  document.body.classList.add('open');
}
function sluitKaart(){ document.body.classList.remove('open'); actief=null; }
function toggleGedaan(){
  if(actief===null) return;
  const idx = gedaan.indexOf(actief);
  if(idx>=0) gedaan.splice(idx,1); else gedaan.push(actief);
  bewaar();
  document.querySelector(`.dot[data-i="${actief}"]`).classList.toggle('done', gedaan.includes(actief));
  document.getElementById('knop-gedaan').textContent =
    gedaan.includes(actief) ? '↺ Toch nog niet gedaan' : '✓ Gedaan!';
}
document.addEventListener('keydown',e=>{ if(e.key==='Escape') sluitKaart(); });
</script>
</body>
</html>
