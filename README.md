# agenda<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Cartelera Escolar</title>
<meta name="description" content="Avisos, circulares, cambios de horario, reuniones y fechas importantes del colegio, organizados por grado y por fecha.">
<!--
  Cartelera Escolar — código abierto, licencia MIT.
  Es un solo archivo: no necesita servidor ni instalación.
  Para cambiar los avisos, edita la lista AVISOS más abajo (sección "DATOS").
-->
<style>
:root {
  --bg: #F3F5F8; --surface: #FFFFFF; --fg: #14202E; --muted: #5A6B80; --line: #D8DFE8;
  --accent: #0A5C9E; --accent-soft: #E1EEF8; --on-accent: #FFFFFF;
  --c-horario: #B83F0A; --c-circular: #0A5C9E; --c-reunion: #6B3FA0; --c-actividad: #1F7A4D; --c-entrega: #9A6A00;
  --font: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0E1620; --surface: #16222F; --fg: #E6EDF5; --muted: #9DB0C4; --line: #2A3B4D;
    --accent: #6DB4F0; --accent-soft: #14304A; --on-accent: #0E1620;
    --c-horario: #F59A6B; --c-circular: #6DB4F0; --c-reunion: #C3A0F0; --c-actividad: #6FCF9B; --c-entrega: #E8C060;
    color-scheme: dark;
  }
}
* { box-sizing: border-box; }
body { margin: 0; background: var(--bg); color: var(--fg); font: 16px/1.55 var(--font); }
.wrap { max-width: 1040px; margin: 0 auto; padding: 0 16px 56px; }
:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; border-radius: 4px; }

header { padding: 32px 0 20px; }
header .colegio { margin: 0 0 4px; font-size: 13px; letter-spacing: .08em; text-transform: uppercase; color: var(--muted); font-weight: 600; }
h1 { margin: 0; font-size: clamp(28px, 6vw, 42px); line-height: 1.1; letter-spacing: -0.02em; }
header p.sub { margin: 8px 0 0; color: var(--muted); max-width: 60ch; }

.herramientas { display: grid; gap: 12px; padding: 16px 0 20px; border-top: 2px solid var(--fg); }
.fila { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; }
.fila .etq { font-size: 13px; font-weight: 600; color: var(--muted); min-width: 72px; }
.pill {
  font: inherit; font-size: 14px; font-weight: 600; padding: 5px 13px; border-radius: 999px; cursor: pointer;
  border: 1px solid var(--line); background: var(--surface); color: var(--fg);
}
.pill[aria-pressed="true"] { background: var(--accent); border-color: var(--accent); color: var(--on-accent); }
.buscar { flex: 1 1 220px; font: inherit; padding: 8px 12px; border-radius: 8px; border: 1px solid var(--line); background: var(--surface); color: var(--fg); min-width: 0; }
.check { font-size: 14px; color: var(--muted); display: inline-flex; gap: 6px; align-items: center; cursor: pointer; }

.cuerpo { display: grid; grid-template-columns: minmax(0, 1fr) 320px; gap: 24px; align-items: start; }
.resumen { display: flex; flex-wrap: wrap; justify-content: space-between; gap: 8px; margin: 0 0 12px; font-size: 14px; color: var(--muted); }
.resumen strong { color: var(--fg); }

.grupo { margin: 0 0 20px; }
.grupo h2 { margin: 0 0 8px; font-size: 14px; letter-spacing: .06em; text-transform: uppercase; color: var(--muted); font-weight: 700; }
.aviso { display: grid; grid-template-columns: 56px minmax(0, 1fr); gap: 4px 14px; background: var(--surface); border: 1px solid var(--line); border-left: 5px solid var(--cat, var(--accent)); border-radius: 10px; padding: 14px 16px; margin-bottom: 10px; }
.aviso.pasado { opacity: .6; }
.aviso.leido h3 { color: var(--muted); }
.fecha { text-align: center; line-height: 1.1; }
.fecha .mes { font-size: 12px; font-weight: 700; text-transform: uppercase; color: var(--muted); letter-spacing: .05em; }
.fecha .dia { font-size: 28px; font-weight: 700; letter-spacing: -0.02em; }
.aviso h3 { margin: 0 0 2px; font-size: 18px; line-height: 1.3; }
.aviso p { margin: 0; color: var(--muted); font-size: 15px; }
.meta { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 8px; align-items: center; }
.chip { font-size: 12px; font-weight: 600; padding: 2px 8px; border-radius: 6px; background: var(--accent-soft); color: var(--accent); }
.chip.cat { background: transparent; border: 1px solid var(--cat); color: var(--cat); }
.acciones { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }
.btn { font: inherit; font-size: 14px; font-weight: 600; padding: 6px 12px; border-radius: 8px; cursor: pointer; border: 1px solid var(--accent); background: transparent; color: var(--accent); }
.btn[aria-pressed="true"] { background: var(--accent); color: var(--on-accent); }
.btn.sec { border-color: var(--line); color: var(--muted); }
.vacio { padding: 28px 16px; text-align: center; color: var(--muted); background: var(--surface); border: 1px dashed var(--line); border-radius: 10px; }

.cal { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 14px; position: sticky; top: 12px; }
.cal-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
.cal-top h2 { margin: 0; font-size: 17px; text-transform: capitalize; }
.cal-top button { font: inherit; font-size: 18px; width: 36px; height: 36px; border-radius: 8px; border: 1px solid var(--line); background: var(--bg); color: var(--fg); cursor: pointer; }
.sem, .dias { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; text-align: center; }
.sem span { font-size: 12px; font-weight: 700; color: var(--muted); padding: 4px 0; }
.dias button { font: inherit; font-size: 14px; aspect-ratio: 1; border: 1px solid transparent; background: transparent; color: var(--fg); border-radius: 8px; cursor: pointer; position: relative; padding: 0; }
.dias button:hover { background: var(--bg); }
.dias button.hoy { border-color: var(--accent); font-weight: 700; }
.dias button.sel { background: var(--accent); color: var(--on-accent); }
.dias button.tiene::after { content: ""; position: absolute; left: 50%; bottom: 4px; width: 5px; height: 5px; margin-left: -2.5px; border-radius: 50%; background: var(--accent); }
.dias button.sel.tiene::after { background: var(--on-accent); }
.dias .fuera { visibility: hidden; }
.leyenda { display: flex; flex-wrap: wrap; gap: 6px 12px; margin-top: 12px; font-size: 12px; color: var(--muted); }
.leyenda i { display: inline-block; width: 10px; height: 10px; border-radius: 3px; background: var(--cat); margin-right: 5px; vertical-align: -1px; }

footer { margin-top: 32px; padding-top: 16px; border-top: 1px solid var(--line); font-size: 13px; color: var(--muted); }

@media (max-width: 800px) {
  .cuerpo { grid-template-columns: minmax(0, 1fr); }
  .cal { position: static; order: -1; }
}
@media (max-width: 480px) {
  .fila .etq { width: 100%; }
}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <p class="colegio" id="colegio">Nombre del colegio</p>
    <h1>Cartelera escolar</h1>
    <p class="sub">Circulares, cambios de horario, reuniones y fechas importantes, en un solo lugar y por grado.</p>
  </header>

  <section class="herramientas" aria-label="Filtros">
    <div class="fila" role="group" aria-label="Grado"><span class="etq">Grado</span><span id="f-grado" style="display:contents"></span></div>
    <div class="fila" role="group" aria-label="Tipo de aviso"><span class="etq">Tipo</span><span id="f-cat" style="display:contents"></span></div>
    <div class="fila">
      <input id="buscar" class="buscar" type="search" placeholder="Buscar un aviso" aria-label="Buscar un aviso">
      <label class="check"><input type="checkbox" id="pasados"> Mostrar avisos pasados</label>
    </div>
  </section>

  <div class="cuerpo">
    <main>
      <p class="resumen" id="resumen" aria-live="polite"></p>
      <div id="lista"></div>
    </main>
    <aside class="cal" aria-label="Calendario">
      <div class="cal-top">
        <button type="button" id="prev" aria-label="Mes anterior">‹</button>
        <h2 id="mes"></h2>
        <button type="button" id="next" aria-label="Mes siguiente">›</button>
      </div>
      <div class="sem" aria-hidden="true"><span>L</span><span>M</span><span>X</span><span>J</span><span>V</span><span>S</span><span>D</span></div>
      <div class="dias" id="dias"></div>
      <div class="leyenda" id="leyenda"></div>
    </aside>
  </div>

  <footer>
    Cartelera Escolar es software libre (licencia MIT). Los avisos se guardan como leídos solo en este dispositivo.
  </footer>
</div>

<script>
/* ============================ DATOS ============================
   Edita esta sección para publicar avisos reales.
   - fecha: "AAAA-MM-DD"
   - hora: opcional, "HH:MM" (24 horas)
   - categoria: horario | circular | reunion | actividad | entrega
   - grados: "todos" o una lista, por ejemplo ["6", "7"]
   Los avisos de abajo son de ejemplo.
================================================================ */
var COLEGIO = "Nombre del colegio";

var AVISOS = [
  { id: 1, fecha: "2026-10-30", hora: "13:00", categoria: "horario", grados: ["9"],
    titulo: "Cambio de horario", detalle: "Las clases de la tarde empiezan a la 1:00 p. m. por actividad de docentes." },
  { id: 2, fecha: "2026-11-02", categoria: "entrega", grados: ["7"],
    titulo: "Entrega de circular firmada", detalle: "Entregar la autorización de salida pedagógica firmada por el acudiente." },
  { id: 3, fecha: "2026-11-03", hora: "06:30", categoria: "reunion", grados: ["6", "7"],
    titulo: "Reunión de padres de familia", detalle: "Citación en el salón de cada director de grupo." },
  { id: 4, fecha: "2026-11-05", hora: "07:00", categoria: "circular", grados: "todos",
    titulo: "Entrega de boletines", detalle: "Las familias recogen el boletín del periodo con el director de grupo." },
  { id: 5, fecha: "2026-11-11", categoria: "actividad", grados: "todos",
    titulo: "Jornada deportiva", detalle: "Traer uniforme de educación física y botella de agua." },
  { id: 6, fecha: "2026-11-18", hora: "08:00", categoria: "reunion", grados: ["10", "11"],
    titulo: "Reunión de orientación", detalle: "Información sobre pruebas de Estado y proyecto de vida." }
];
/* ========================== FIN DE DATOS ===================== */

var CATEGORIAS = {
  horario:   "Cambio de horario",
  circular:  "Circular",
  reunion:   "Reunión",
  actividad: "Actividad",
  entrega:   "Entrega de documentos"
};
var GRADOS = ["todos", "6", "7", "8", "9", "10", "11"];
var MESES = ["enero","febrero","marzo","abril","mayo","junio","julio","agosto","septiembre","octubre","noviembre","diciembre"];
var MESES_CORTO = ["ene","feb","mar","abr","may","jun","jul","ago","sep","oct","nov","dic"];
var DIAS = ["domingo","lunes","martes","miércoles","jueves","viernes","sábado"];

var estado = { grado: "todos", cat: "todas", texto: "", pasados: false, dia: null, anio: 0, mes: 0 };
var leidos = {};
try { leidos = JSON.parse(localStorage.getItem("cartelera-leidos") || "{}"); } catch (e) { leidos = {}; }
function guardarLeidos() { try { localStorage.setItem("cartelera-leidos", JSON.stringify(leidos)); } catch (e) {} }

function $(id) { return document.getElementById(id); }
function parseFecha(s) { var p = s.split("-"); return new Date(+p[0], +p[1] - 1, +p[2]); }
function claveDia(d) { return d.getFullYear() + "-" + String(d.getMonth() + 1).padStart(2, "0") + "-" + String(d.getDate()).padStart(2, "0"); }
var hoy = new Date(); hoy.setHours(0, 0, 0, 0);
var hoyClave = claveDia(hoy);

function aplicaAlGrado(a) {
  if (estado.grado === "todos") return true;
  return a.grados === "todos" || a.grados.indexOf(estado.grado) !== -1;
}
function coincide(a) {
  if (!aplicaAlGrado(a)) return false;
  if (estado.cat !== "todas" && a.categoria !== estado.cat) return false;
  if (estado.texto) {
    var t = (a.titulo + " " + a.detalle + " " + CATEGORIAS[a.categoria]).toLowerCase();
    if (t.indexOf(estado.texto.toLowerCase()) === -1) return false;
  }
  return true;
}
function etiquetaGrados(a) {
  if (a.grados === "todos") return "Todos los grados";
  return (a.grados.length > 1 ? "Grados " : "Grado ") + a.grados.map(function (g) { return g + "°"; }).join(", ");
}
function el(tag, cls, txt) {
  var e = document.createElement(tag);
  if (cls) e.className = cls;
  if (txt != null) e.textContent = txt;
  return e;
}
function colorCat(c) { return "var(--c-" + c + ")"; }

/* ---------- Filtros ---------- */
function pintarFiltros() {
  var g = $("f-grado"); g.textContent = "";
  GRADOS.forEach(function (v) {
    var b = el("button", "pill", v === "todos" ? "Todos" : v + "°");
    b.type = "button"; b.setAttribute("aria-pressed", String(estado.grado === v));
    b.onclick = function () { estado.grado = v; pintar(); };
    g.appendChild(b);
  });
  var c = $("f-cat"); c.textContent = "";
  var todas = el("button", "pill", "Todos"); todas.type = "button";
  todas.setAttribute("aria-pressed", String(estado.cat === "todas"));
  todas.onclick = function () { estado.cat = "todas"; pintar(); };
  c.appendChild(todas);
  Object.keys(CATEGORIAS).forEach(function (k) {
    var b = el("button", "pill", CATEGORIAS[k]); b.type = "button";
    b.setAttribute("aria-pressed", String(estado.cat === k));
    b.onclick = function () { estado.cat = k; pintar(); };
    c.appendChild(b);
  });
}

/* ---------- Lista ---------- */
function ics(a) {
  var d = a.fecha.replace(/-/g, "");
  var lineas = ["BEGIN:VCALENDAR", "VERSION:2.0", "PRODID:-//Cartelera Escolar//ES", "BEGIN:VEVENT",
    "UID:cartelera-" + a.id + "@cartelera-escolar", "DTSTAMP:" + d + "T000000Z"];
  if (a.hora) {
    var h = a.hora.replace(":", "") + "00";
    lineas.push("DTSTART:" + d + "T" + h);
  } else {
    lineas.push("DTSTART;VALUE=DATE:" + d);
  }
  lineas.push("SUMMARY:" + a.titulo.replace(/[,;]/g, " "), "DESCRIPTION:" + a.detalle.replace(/[,;\n]/g, " "), "END:VEVENT", "END:VCALENDAR");
  return lineas.join("\r\n");
}
function descargarIcs(a) {
  var blob = new Blob([ics(a)], { type: "text/calendar;charset=utf-8" });
  var url = URL.createObjectURL(blob);
  var l = document.createElement("a");
  l.href = url; l.download = "aviso-" + a.id + ".ics";
  document.body.appendChild(l); l.click(); document.body.removeChild(l);
  setTimeout(function () { URL.revokeObjectURL(url); }, 1000);
}

function tarjeta(a) {
  var f = parseFecha(a.fecha);
  var pasado = a.fecha < hoyClave;
  var card = el("article", "aviso" + (pasado ? " pasado" : "") + (leidos[a.id] ? " leido" : ""));
  card.style.setProperty("--cat", colorCat(a.categoria));

  var fe = el("div", "fecha");
  fe.appendChild(el("div", "mes", MESES_CORTO[f.getMonth()]));
  fe.appendChild(el("div", "dia", String(f.getDate())));

  var c = el("div");
  c.appendChild(el("h3", null, a.titulo));
  c.appendChild(el("p", null, a.detalle));

  var m = el("div", "meta");
  m.appendChild(el("span", "chip cat", CATEGORIAS[a.categoria]));
  m.appendChild(el("span", "chip", etiquetaGrados(a)));
  var cuando = DIAS[f.getDay()] + (a.hora ? ", " + a.hora : "");
  m.appendChild(el("span", "chip", cuando));
  if (a.fecha === hoyClave) m.appendChild(el("span", "chip", "Hoy"));
  c.appendChild(m);

  var ac = el("div", "acciones");
  var b = el("button", "btn", leidos[a.id] ? "Leído ✓" : "Marcar como leído");
  b.type = "button"; b.setAttribute("aria-pressed", String(!!leidos[a.id]));
  b.onclick = function () { if (leidos[a.id]) delete leidos[a.id]; else leidos[a.id] = true; guardarLeidos(); pintar(); };
  ac.appendChild(b);
  if (!pasado) {
    var cb = el("button", "btn sec", "Añadir a mi calendario"); cb.type = "button";
    cb.onclick = function () { descargarIcs(a); };
    ac.appendChild(cb);
  }
  c.appendChild(ac);

  card.appendChild(fe); card.appendChild(c);
  return card;
}

function pintarLista() {
  var cont = $("lista"); cont.textContent = "";
  var lista = AVISOS.filter(coincide);
  if (estado.dia) lista = lista.filter(function (a) { return a.fecha === estado.dia; });
  else if (!estado.pasados) lista = lista.filter(function (a) { return a.fecha >= hoyClave; });
  lista.sort(function (a, b) { return a.fecha < b.fecha ? -1 : a.fecha > b.fecha ? 1 : (a.hora || "") < (b.hora || "") ? -1 : 1; });

  var pendientes = lista.filter(function (a) { return a.fecha >= hoyClave && !leidos[a.id]; }).length;
  var r = $("resumen"); r.textContent = "";
  var s = el("span"); s.appendChild(el("strong", null, String(lista.length)));
  s.appendChild(document.createTextNode(lista.length === 1 ? " aviso" : " avisos"));
  if (estado.dia) {
    var f = parseFecha(estado.dia);
    s.appendChild(document.createTextNode(" el " + f.getDate() + " de " + MESES[f.getMonth()]));
  }
  r.appendChild(s);
  var p = el("span", null, pendientes === 0 ? "Todo al día" : pendientes + (pendientes === 1 ? " sin leer" : " sin leer"));
  r.appendChild(p);
  if (estado.dia) {
    var q = el("button", "btn sec", "Quitar fecha"); q.type = "button";
    q.onclick = function () { estado.dia = null; pintar(); };
    r.appendChild(q);
  }

  if (!lista.length) {
    cont.appendChild(el("div", "vacio", estado.dia ? "No hay avisos para esta fecha con los filtros elegidos." : "No hay avisos con los filtros elegidos."));
    return;
  }
  var actual = null, grupo = null;
  lista.forEach(function (a) {
    var f2 = parseFecha(a.fecha);
    var titulo = DIAS[f2.getDay()] + " " + f2.getDate() + " de " + MESES[f2.getMonth()];
    if (a.fecha === hoyClave) titulo = "Hoy · " + titulo;
    if (titulo !== actual) {
      actual = titulo;
      grupo = el("section", "grupo");
      grupo.appendChild(el("h2", null, titulo));
      cont.appendChild(grupo);
    }
    grupo.appendChild(tarjeta(a));
  });
}

/* ---------- Calendario ---------- */
function pintarCalendario() {
  $("mes").textContent = MESES[estado.mes] + " " + estado.anio;
  var cont = $("dias"); cont.textContent = "";
  var primero = new Date(estado.anio, estado.mes, 1);
  var offset = (primero.getDay() + 6) % 7;
  var total = new Date(estado.anio, estado.mes + 1, 0).getDate();
  var conAviso = {};
  AVISOS.filter(coincide).forEach(function (a) { conAviso[a.fecha] = true; });

  for (var i = 0; i < offset; i++) { var v = el("button", "fuera"); v.disabled = true; v.tabIndex = -1; cont.appendChild(v); }
  for (var d = 1; d <= total; d++) {
    var clave = claveDia(new Date(estado.anio, estado.mes, d));
    var b = el("button", null, String(d)); b.type = "button";
    if (clave === hoyClave) b.className += " hoy";
    if (conAviso[clave]) b.className += " tiene";
    if (estado.dia === clave) b.className += " sel";
    b.setAttribute("aria-label", d + " de " + MESES[estado.mes] + (conAviso[clave] ? ", con avisos" : ""));
    (function (c) { b.onclick = function () { estado.dia = (estado.dia === c) ? null : c; pintar(); }; })(clave);
    cont.appendChild(b);
  }
  var ley = $("leyenda"); ley.textContent = "";
  Object.keys(CATEGORIAS).forEach(function (k) {
    var s = el("span"); s.style.setProperty("--cat", colorCat(k));
    s.appendChild(el("i")); s.appendChild(document.createTextNode(CATEGORIAS[k]));
    ley.appendChild(s);
  });
}

function pintar() { pintarFiltros(); pintarLista(); pintarCalendario(); }

/* ---------- Inicio ---------- */
$("colegio").textContent = COLEGIO;
document.title = "Cartelera Escolar" + (COLEGIO && COLEGIO !== "Nombre del colegio" ? " · " + COLEGIO : "");
$("buscar").addEventListener("input", function (e) { estado.texto = e.target.value.trim(); pintar(); });
$("pasados").addEventListener("change", function (e) { estado.pasados = e.target.checked; pintar(); });
$("prev").onclick = function () { estado.mes--; if (estado.mes < 0) { estado.mes = 11; estado.anio--; } pintarCalendario(); };
$("next").onclick = function () { estado.mes++; if (estado.mes > 11) { estado.mes = 0; estado.anio++; } pintarCalendario(); };

(function iniciarMes() {
  var proximo = AVISOS.map(function (a) { return a.fecha; }).filter(function (f) { return f >= hoyClave; }).sort()[0];
  var base = proximo ? parseFecha(proximo) : hoy;
  estado.anio = base.getFullYear(); estado.mes = base.getMonth();
})();
pintar();
</script>
</body>
</html>
