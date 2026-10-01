<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Boyacá Interactiva | Provincias e información territorial</title>
<style>
:root{--navy:#17324d;--blue:#1769aa;--light:#eef5fb;--border:#d7e0e8;--text:#183247;--muted:#657789;--green:#38704d}
*{box-sizing:border-box}
body{margin:0;background:#f4f7fb;color:var(--text);font-family:system-ui,-apple-system,Segoe UI,sans-serif}
header{background:linear-gradient(135deg,#17324d,#1d557d);color:white;padding:30px 20px}
header .wrap{max-width:1180px;margin:auto}
header h1{margin:0 0 8px;font-size:30px}
header p{margin:0;opacity:.9}
main{max-width:1180px;margin:22px auto;padding:0 16px}
.card{background:white;border:1px solid var(--border);border-radius:18px;padding:20px;margin-bottom:18px;box-shadow:0 5px 18px rgba(24,50,71,.05)}
.section-title{margin:0 0 6px;font-size:22px}
.section-subtitle{margin:0 0 16px;color:var(--muted)}
.map-layout{display:grid;grid-template-columns:minmax(0,1.65fr) minmax(280px,.8fr);gap:18px;align-items:stretch}
.map-box{background:#f7fafc;border:1px solid var(--border);border-radius:16px;min-height:560px;position:relative;overflow:hidden}
#map{width:100%;height:560px;display:block}
.map-loading{position:absolute;inset:0;display:grid;place-items:center;color:var(--muted);background:rgba(247,250,252,.92);z-index:3}
.map-note{font-size:12px;color:var(--muted);padding:9px 12px;border-top:1px solid var(--border);background:#fff}
.province-path{stroke:#fff;stroke-width:1.7;cursor:pointer;transition:opacity .15s,stroke-width .15s}
.province-path:hover{opacity:.82;stroke:#17324d;stroke-width:2.8}
.province-path.selected{stroke:#17324d;stroke-width:3.8}
.map-label{pointer-events:none;font-size:11px;font-weight:800;fill:#183247;text-anchor:middle;paint-order:stroke;stroke:#fff;stroke-width:4px;stroke-linejoin:round}
.fallback-map{display:none;position:absolute;inset:0;background:#edf3f7;border-radius:16px;overflow:hidden}
.fb-region{position:absolute;display:flex;align-items:center;justify-content:center;text-align:center;font-size:12px;font-weight:800;color:#183247;background:#cfe2ee;border:2px solid #fff;cursor:pointer;padding:8px;line-height:1.05;transition:.15s;box-shadow:0 1px 3px rgba(0,0,0,.12)}
.fb-region:hover,.fb-region.active{background:#8eb8d0;z-index:5;transform:scale(1.025);border-color:#17324d}
.r-centro{left:34%;top:32%;width:20%;height:17%;clip-path:polygon(8% 0,88% 8%,100% 48%,88% 100%,18% 90%,0 40%)}
.r-norte{left:17%;top:20%;width:20%;height:18%;clip-path:polygon(0 25%,48% 0,100% 18%,88% 82%,38% 100%,5% 70%)}
.r-gut{left:5%;top:27%;width:19%;height:24%;clip-path:polygon(15% 0,92% 8%,100% 65%,70% 100%,8% 82%,0 28%)}
.r-val{left:22%;top:7%;width:24%;height:19%;clip-path:polygon(8% 18%,70% 0,100% 35%,82% 95%,20% 100%,0 55%)}
.r-tund{left:48%;top:18%;width:22%;height:19%;clip-path:polygon(5% 18%,62% 0,100% 20%,90% 80%,45% 100%,0 65%)}
.r-sug{left:52%;top:33%;width:25%;height:23%;clip-path:polygon(10% 5%,80% 0,100% 35%,82% 100%,30% 92%,0 45%)}
.r-occ{left:20%;top:44%;width:20%;height:22%;clip-path:polygon(5% 10%,70% 0,100% 45%,78% 100%,20% 92%,0 45%)}
.r-mar{left:37%;top:48%;width:18%;height:18%;clip-path:polygon(5% 10%,75% 0,100% 45%,82% 100%,20% 92%,0 42%)}
.r-nei{left:27%;top:61%;width:20%;height:18%;clip-path:polygon(8% 5%,72% 0,100% 48%,78% 100%,18% 92%,0 38%)}
.r-or{left:51%;top:53%;width:21%;height:20%;clip-path:polygon(8% 0,88% 12%,100% 62%,62% 100%,10% 85%,0 30%)}
.r-lib{left:41%;top:69%;width:17%;height:18%;clip-path:polygon(10% 0,86% 12%,100% 60%,72% 100%,10% 88%,0 35%)}
.r-len{left:58%;top:70%;width:22%;height:22%;clip-path:polygon(5% 12%,70% 0,100% 38%,88% 88%,35% 100%,0 55%)}
.r-ric{left:24%;top:78%;width:21%;height:17%;clip-path:polygon(10% 0,76% 8%,100% 50%,82% 100%,18% 90%,0 40%)}

.legend{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-top:12px}
.legend button{border:1px solid var(--border);background:white;border-radius:9px;padding:9px;text-align:left;cursor:pointer;font-weight:700;color:var(--text)}
.legend button:hover,.legend button.active{border-color:var(--blue);background:#eef5fb}
.info-panel{background:#fff;border:1px solid var(--border);border-radius:16px;padding:18px;min-height:560px}
.badge{display:inline-block;background:#e8f2fb;color:#1769aa;border-radius:999px;padding:5px 10px;font-size:12px;font-weight:800;margin-bottom:9px}
.info-panel h3{font-size:27px;margin:0 0 8px}
.info-panel p{line-height:1.55}
.info-block{border-top:1px solid var(--border);padding-top:13px;margin-top:13px}
.info-block h4{margin:0 0 7px;font-size:14px}
.info-block .content{white-space:pre-line;color:#405466;font-size:14px}
.actions{display:flex;gap:10px;flex-wrap:wrap;margin-top:16px}
button{border:0;border-radius:10px;padding:11px 15px;font-weight:750;cursor:pointer}
.primary{background:var(--blue);color:#fff}.secondary{background:#e8eef4;color:var(--text)}.success{background:#e6f3ea;color:#28633e}.danger{background:#fce8e8;color:#9b2226}
.status{margin-top:10px;color:var(--green);font-weight:650}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:16px}
label{display:block;font-weight:750;margin:10px 0 7px}
select,input,textarea{width:100%;padding:12px;border:1px solid #bcc9d5;border-radius:9px;font:inherit;color:var(--text)}
textarea{min-height:130px;resize:vertical}
.share-box{background:#eef5fb;border:1px solid #d5e5f2;border-radius:13px;padding:15px}
.share-box strong{display:block;margin-bottom:5px}
small{color:var(--muted)}
footer{max-width:1180px;margin:0 auto 28px;padding:0 16px;color:var(--muted);font-size:12px}
@media(max-width:850px){.map-layout{grid-template-columns:1fr}.map-box,#map{min-height:430px;height:430px}.info-panel{min-height:auto}.grid{grid-template-columns:1fr}}
@media(max-width:520px){header h1{font-size:24px}.legend{grid-template-columns:1fr}.card{padding:15px}}
</style>
</head>
<body>
<header>
<div class="wrap">
<h1>Boyacá Interactiva</h1>
<p>Mapa de provincias con información territorial, educativa y estadística. Selecciona una región para consultar su ficha.</p>
</div>
</header>

<main>
<section class="card">
<h2 class="section-title">Mapa interactivo de las provincias de Boyacá</h2>
<p class="section-subtitle">Haz clic directamente sobre una provincia del mapa o selecciónala en la lista. La información aparecerá en el panel de la derecha.</p>
<div class="map-layout">
<div>
<div class="map-box">
<div id="map-loading" class="map-loading">Cargando mapa provincial…</div>
<div id="fallbackMap" class="fallback-map" aria-label="Vista interactiva alternativa de las provincias de Boyacá">
  <div class="fb-region r-centro" data-p="Centro">Centro</div>
  <div class="fb-region r-norte" data-p="Norte">Norte</div>
  <div class="fb-region r-gut" data-p="Gutiérrez">Gutiérrez</div>
  <div class="fb-region r-val" data-p="Valderrama">Valderrama</div>
  <div class="fb-region r-tund" data-p="Tundama">Tundama</div>
  <div class="fb-region r-sug" data-p="Sugamuxi">Sugamuxi</div>
  <div class="fb-region r-occ" data-p="Occidente">Occidente</div>
  <div class="fb-region r-mar" data-p="Márquez">Márquez</div>
  <div class="fb-region r-nei" data-p="Neira">Neira</div>
  <div class="fb-region r-or" data-p="Oriente">Oriente</div>
  <div class="fb-region r-lib" data-p="La Libertad">La Libertad</div>
  <div class="fb-region r-len" data-p="Lengupá">Lengupá</div>
  <div class="fb-region r-ric" data-p="Ricaurte">Ricaurte</div>
</div>
<svg id="map" viewBox="0 0 760 620" role="img" aria-label="Mapa interactivo de las provincias de Boyacá"></svg>
<div class="map-note">División provincial basada en el archivo GeoJSON/TopoJSON público del repositorio <b>geojson_boyaca</b>. Se muestran las 13 provincias; Cubará y Puerto Boyacá son tratamientos territoriales especiales y no se incluyen como provincias.</div>
</div>
<div id="legend" class="legend"></div>
</div>

<aside class="info-panel" id="publicInfo">
<span class="badge">PROVINCIA SELECCIONADA</span>
<h3 id="infoName">Centro</h3>
<p id="infoDescription">Selecciona una provincia para consultar su información.</p>
<div class="info-block"><h4>Colegios / instituciones</h4><div class="content" id="infoSchools">Sin información registrada.</div></div>
<div class="info-block"><h4>Estadísticas / resultados</h4><div class="content" id="infoStats">Sin información registrada.</div></div>
<div class="actions"><button class="primary" id="goEdit" type="button">Editar esta provincia</button></div>
</aside>
</div>
</section>

<section class="card" id="adminSection">
<h2 class="section-title">Administrar información</h2>
<p class="section-subtitle">Aquí puedes completar las fichas que luego verá cualquier persona al hacer clic en el mapa.</p>
<label for="province">Provincia</label>
<select id="province"></select>
<div class="grid">
<div>
<label for="descripcion">Descripción de la provincia</label>
<textarea id="descripcion" placeholder="Escribe aquí la información general de la provincia…"></textarea>
</div>
<div>
<label for="colegios">Colegios / instituciones</label>
<textarea id="colegios" placeholder="Escribe los colegios. Puedes poner uno por línea…"></textarea>
</div>
<div>
<label for="estadisticas">Estadísticas / resultados</label>
<textarea id="estadisticas" placeholder="Resultados de Pruebas Saber, indicadores u otros datos…"></textarea>
</div>
<div>
<label for="enlace">Enlace externo (opcional)</label>
<input id="enlace" type="url" placeholder="https://…">
<label for="imagen">Imagen (opcional)</label>
<input id="imagen" type="text" placeholder="Nombre o dirección de la imagen">
</div>
</div>
<div class="actions">
<button class="primary" id="save" type="button">Guardar provincia</button>
<button class="secondary" id="export" type="button">Exportar información</button>
<button class="secondary" id="importBtn" type="button">Importar información</button>
<button class="success" id="makeShare" type="button">Generar versión para compartir</button>
<button class="secondary" id="sharePage" type="button">Compartir plataforma</button>
<button class="danger" id="clear" type="button">Borrar información</button>
<input id="file" type="file" accept=".json" hidden>
</div>
<div class="status" id="status" aria-live="polite"></div>
<div class="share-box" style="margin-top:16px">
<strong>¿Cómo compartirla con otras personas?</strong>
<span>Usa “Generar versión para compartir” para descargar un HTML con la información actual incorporada. Después puedes subir ese archivo a GitHub Pages, Netlify, Vercel o un servidor institucional y compartir el enlace. Así las demás personas podrán abrir el mapa y consultar las fichas sin tener tu navegador ni tus datos locales.</span>
</div>
</section>
</main>

<footer>
Fuentes cartográficas consultadas: Secretaría de Planeación de Boyacá y repositorio público geojson_boyaca. La plataforma está preparada para agregar posteriormente indicadores, gráficos, enlaces y fichas municipales.
</footer>

<script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3.1.0/dist/topojson-client.min.js"></script>
<script>
const PROVINCIAS=["Centro","Gutiérrez","La Libertad","Lengupá","Márquez","Neira","Norte","Occidente","Oriente","Ricaurte","Sugamuxi","Tundama","Valderrama"];
const KEY="boyaca_provincias_v2";
const TOPO_URL="https://raw.githubusercontent.com/mwlware/geojson_boyaca/main/provincias.json";
const INITIAL_SHARED_DATA=window.BOYACA_SHARED_DATA||{};
const empty=()=>({descripcion:"",colegios:"",estadisticas:"",enlace:"",imagen:""});
let data={};
try{data={...Object.fromEntries(PROVINCIAS.map(p=>[p,empty()])),...INITIAL_SHARED_DATA,...JSON.parse(localStorage.getItem(KEY)||"{}")};}catch(e){data=Object.fromEntries(PROVINCIAS.map(p=>[p,empty()]));}
let selected="Centro";

const $=id=>document.getElementById(id);
PROVINCIAS.forEach(p=>{const o=document.createElement("option");o.value=p;o.textContent=p;$("province").appendChild(o);});

const colors=["#d9e8f5","#dcefdc","#f6e6c9","#eadcf4","#f4d9df","#e3efd1","#dce7f4","#f3e0c9","#d8ece9","#f0ddeb","#dce4f2","#f3e6cf","#e1dceb"];
const norm=s=>String(s||"").normalize("NFD").replace(/[\u0300-\u036f]/g,"").toUpperCase().replace("GUTIERREZ","GUTIERREZ");
const mapNames={"CENTRO":"Centro","GUTIERREZ":"Gutiérrez","LA LIBERTAD":"La Libertad","LENGUPA":"Lengupá","MARQUEZ":"Márquez","NEIRA":"Neira","NORTE":"Norte","OCCIDENTE":"Occidente","ORIENTE":"Oriente","RICAURTE":"Ricaurte","SUGAMUXI":"Sugamuxi","TUNDAMA":"Tundama","VALDERRAMA":"Valderrama"};

function loadForm(){
 const d=data[selected]||empty();
 $("province").value=selected;$("descripcion").value=d.descripcion||"";$("colegios").value=d.colegios||"";$("estadisticas").value=d.estadisticas||"";$("enlace").value=d.enlace||"";$("imagen").value=d.imagen||"";
 renderInfo();
}
function renderInfo(){
 const d=data[selected]||empty();
 $("infoName").textContent=selected;
 $("infoDescription").textContent=d.descripcion||"Aún no has agregado una descripción para esta provincia.";
 $("infoSchools").textContent=d.colegios||"Sin información registrada.";
 $("infoStats").textContent=d.estadisticas||"Sin información registrada.";
 document.querySelectorAll(".legend button").forEach(b=>b.classList.toggle("active",b.dataset.province===selected));
 document.querySelectorAll(".province-path").forEach(p=>p.classList.toggle("selected",mapNames[p.dataset.province]===selected));
}
function selectProvince(p){
 if(!PROVINCIAS.includes(p))return;
 selected=p;loadForm();
 document.getElementById("publicInfo").scrollIntoView({behavior:"smooth",block:"nearest"});
}
$("province").addEventListener("change",e=>{selected=e.target.value;loadForm();renderInfo();});
$("save").addEventListener("click",()=>{
 data[selected]={descripcion:$("descripcion").value,colegios:$("colegios").value,estadisticas:$("estadisticas").value,enlace:$("enlace").value,imagen:$("imagen").value};
 localStorage.setItem(KEY,JSON.stringify(data));renderInfo();$("status").textContent="✓ Información de "+selected+" guardada correctamente.";
});
$("goEdit").addEventListener("click",()=>{$("adminSection").scrollIntoView({behavior:"smooth"});});
$("export").addEventListener("click",()=>{
 const blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"});const a=document.createElement("a");
 a.href=URL.createObjectURL(blob);a.download="datos_provincias_boyaca.json";a.click();URL.revokeObjectURL(a.href);
});
$("importBtn").addEventListener("click",()=>$("file").click());
$("file").addEventListener("change",async e=>{
 const f=e.target.files[0];if(!f)return;
 try{const imported=JSON.parse(await f.text());data={...data,...imported};localStorage.setItem(KEY,JSON.stringify(data));loadForm();$("status").textContent="✓ Información importada correctamente.";}
 catch(err){alert("El archivo no tiene un formato JSON válido.");}
});
$("clear").addEventListener("click",()=>{
 if(!confirm("¿Borrar toda la información de "+selected+"?"))return;
 data[selected]=empty();localStorage.setItem(KEY,JSON.stringify(data));loadForm();$("status").textContent="Información borrada.";
});

function buildLegend(){
 const wrap=$("legend");wrap.innerHTML="";
 PROVINCIAS.forEach(p=>{const b=document.createElement("button");b.type="button";b.dataset.province=p;b.textContent=p;b.addEventListener("click",()=>selectProvince(p));wrap.appendChild(b);});
}

async function drawMap(){
 const loading=$("map-loading");
 const fallback=$("fallbackMap");
 try{
   if(!window.d3||!window.topojson)throw new Error("Motor cartográfico no disponible");
   const topo=await fetch(TOPO_URL,{mode:"cors"}).then(r=>{if(!r.ok)throw new Error("No se pudo cargar el mapa");return r.json();});
   if(!topo.objects||!topo.objects.provincias_topo)throw new Error("Formato cartográfico no compatible");
   const features=topojson.feature(topo,topo.objects.provincias_topo).features.filter(f=>mapNames[f.properties.provincia]);
   if(!features.length)throw new Error("Sin provincias");
   const svg=d3.select("#map"),w=760,h=620;
   const projection=d3.geoMercator().fitExtent([[28,28],[w-28,h-28]],{type:"FeatureCollection",features});
   const path=d3.geoPath(projection);svg.selectAll("*").remove();
   const g=svg.append("g");
   g.selectAll("path").data(features).join("path").attr("class","province-path").attr("data-province",d=>d.properties.provincia).attr("fill",d=>colors[PROVINCIAS.indexOf(mapNames[d.properties.provincia])%colors.length]).attr("d",path).attr("aria-label",d=>mapNames[d.properties.provincia]).on("click",(event,d)=>selectProvince(mapNames[d.properties.provincia]));
   g.selectAll("text").data(features).join("text").attr("class","map-label").attr("x",d=>path.centroid(d)[0]).attr("y",d=>path.centroid(d)[1]).text(d=>mapNames[d.properties.provincia]);
   fallback.style.display="none";svg.style.display="block";loading.style.display="none";renderInfo();
 }catch(e){
   svg=document.getElementById("map");svg.style.display="none";fallback.style.display="block";loading.style.display="none";
   fallback.querySelectorAll(".fb-region").forEach(el=>el.addEventListener("click",()=>selectProvince(el.dataset.p)));
   renderInfo();
   $("status").textContent="✓ Se activó la vista alternativa interactiva porque el mapa geográfico externo no respondió. Las provincias siguen siendo seleccionables.";
 }
}


$("makeShare").addEventListener("click",()=>{
 const safe=JSON.stringify(data).replace(/</g,"\\u003c");
 const source=document.documentElement.outerHTML.replace(
   '<script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js"></script>',
   '<script>window.BOYACA_SHARED_DATA='+safe+';</script><script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js"></script>'
 );
 const blob=new Blob(["<!doctype html>\n"+source],{type:"text/html;charset=utf-8"});
 const a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download="boyaca_interactiva_para_compartir.html";a.click();URL.revokeObjectURL(a.href);
 $("status").textContent="✓ Se generó una copia con las fichas actuales incorporadas. Ya puedes subirla a un servicio de alojamiento web.";
});
$("sharePage").addEventListener("click",async()=>{
 if(location.protocol!=="file:" && navigator.share){
   try{await navigator.share({title:"Boyacá Interactiva",text:"Consulta el mapa interactivo de las provincias de Boyacá.",url:location.href});return;}catch(e){}
 }
 $("status").textContent="Para compartirla mediante un enlace público, primero genera la versión para compartir y súbela a GitHub Pages, Netlify, Vercel o al servidor de tu institución.";
});

buildLegend();loadForm();drawMap();
</script>
</body>
</html>
