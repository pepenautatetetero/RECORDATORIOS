<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Reciclatorios</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<style>
html{ scroll-behavior: smooth; }

body{
    margin: 0;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    background-color: #3f6b3f;
}

#map{
    height: 400px;
    width: 100%;
    border-radius: 15px;
    border: 3px solid white;
    margin-top: 20px;
}

.header-top{
    background-color: #a8c3a0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 30px;
    flex-wrap: wrap;
}

.header-top h1{ margin: 0; }

.logos{
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
}

.logos img{
    width: 100px;
    border-radius: 10px;
}

.menu{
    background-color: #6c8f5e;
    display: flex;
    justify-content: center;
    gap: 20px;
    padding: 15px;
    flex-wrap: wrap;
}

.menu a{
    text-decoration: none;
    color: black;
    font-weight: bold;
}

.principal{ padding: 50px; }

.grid{
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px;
}

.item-grande{ grid-row: span 2; }

.item-grande img{
    width: 100%;
    height: 415px;
    object-fit: cover;
}

.grid img{
    width: 100%;
    height: 200px;
    object-fit: cover;
}

section{
    padding: 80px 40px;
    color: white;
}

.zona-juego{
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
    flex-wrap: wrap;
    gap: 20px;
}

.objetos, .botes{
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.item{
    background-color: white;
    color: black;
    padding: 10px;
    cursor: grab;
    text-align: center;
    border-radius: 8px;
}

.bote{
    background-color: #a8c3a0;
    padding: 20px;
    text-align: center;
    min-width: 120px;
    border-radius: 10px;
}

button{
    margin: 5px;
    padding: 10px 15px;
    cursor: pointer;
    border: none;
    border-radius: 8px;
    font-weight: bold;
}

#notificacion{
    position: fixed;
    top: 20px;
    right: 20px;
    background: white;
    color: black;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0,0,0,0.3);
    display: none;
    z-index: 9999;
}

#racha{
    font-size: 22px;
    font-weight: bold;
    margin-top: 10px;
}

@media(max-width:768px){
    .grid{
        grid-template-columns: 1fr;
    }

    .item-grande{
        grid-row: span 1;
    }

    .item-grande img{
        height: auto;
    }
}
</style>
</head>

<body>

<div id="notificacion"></div>

<!-- BLOQUE 1 -->
<div class="header-top">
    <h1>RECICLATORIOS</h1>

    <div class="logos">
        <img src="https://tse3.mm.bing.net/th/id/OIP.JNYfxsVM5y-gSFeQdWdvngHaHa?rs=1&pid=ImgDetMain&o=7&rm=3">
        <img src="https://tse3.mm.bing.net/th/id/OIP.YrmhDxy7KanKF2sCm--b2QHaIu?rs=1&pid=ImgDetMain&o=7&rm=3">
    </div>
</div>

<!-- BLOQUE 2 -->
<div class="menu">
    <a href="#aprende">Aprende a reciclar</a>
    <a href="#donde">Dónde reciclar</a>
    <a href="#mapa">Mapa</a>
    <a href="#objetivo">Nuestro objetivo</a>
    <a href="#conocenos">Conócenos</a>
    <a href="#juegos">Juegos</a>
</div>

<!-- BLOQUE 3 -->
<div class="principal">
    <div class="grid">

        <div class="item-grande">
            <img src="https://tse3.mm.bing.net/th/id/OIP.Ld0ALVeHBe08o-T9SRUs5AHaE7?rs=1&pid=ImgDetMain&o=7&rm=3">
            <p>Aprende a reciclar y ayuda al planeta con pequeñas acciones.</p>
        </div>

        <img src="https://tse2.mm.bing.net/th/id/OIP.KJrVSaeZ5UEDqn-Ak5e67gHaE8?rs=1&pid=ImgDetMain&o=7&rm=3">
        <img src="https://th.bing.com/th/id/R.25e599134090c3340f86e24701401ce1?rik=KUwtahbccyCVYw&pid=ImgRaw&r=0">

    </div>
</div>

<!-- JUEGOS -->
<section id="juegos">
    <h2>Juegos</h2>

    <p id="racha">Racha: 0 🔥</p>

    <h3>Quiz ♻️</h3>
    <p id="pregunta"></p>
    <div id="opciones"></div>
    <p id="resultado"></p>
    <button onclick="siguientePregunta()">Siguiente</button>

    <hr>

    <h3>Arrastra el residuo al bote correcto ♻️</h3>

    <div class="zona-juego">
        <div class="objetos">
            <div class="item" draggable="true" id="plastico1">Botella</div>
            <div class="item" draggable="true" id="organico1">Cáscara</div>
            <div class="item" draggable="true" id="vidrio1">Vidrio</div>
            <div class="item" draggable="true" id="papel1">Periódico</div>
        </div>

        <div class="botes">
            <div class="bote" data-tipo="plastico">Plástico</div>
            <div class="bote" data-tipo="organico">Orgánico</div>
            <div class="bote" data-tipo="vidrio">Vidrio</div>
            <div class="bote" data-tipo="papel">Papel</div>
        </div>
    </div>

    <p id="resultadoDrag"></p>
</section>

<!-- SECCIONES -->
<section id="aprende">
    <h2>Aprende a reciclar</h2>
    <p>Aquí irá la información sobre reciclaje.</p>
</section>

<section id="donde">
    <h2>Dónde reciclar</h2>
    <p>Aquí pondrás lugares para reciclar.</p>
</section>

<section id="mapa">
    <h2>Mapa de reciclaje 🗺️</h2>
    <p>Ubicación de puntos de reciclaje.</p>
    <div id="map"></div>
</section>

<section id="objetivo">
    <h2>Nuestro objetivo</h2>
    <p>Concientizar sobre el reciclaje.</p>
</section>

<section id="conocenos">
    <h2>Conócenos</h2>
    <p>Somos estudiantes del CETIS 71 en Reynosa.</p>
</section>

<script>
// ===============================
// NOTIFICACIONES
// ===============================
function mostrarNotificacion(mensaje){
    const noti = document.getElementById("notificacion");
    noti.textContent = mensaje;
    noti.style.display = "block";

    setTimeout(()=>{
        noti.style.display = "none";
    }, 2000);
}

// ===============================
// MAPA
// ===============================
document.addEventListener("DOMContentLoaded", function(){

    var map = L.map('map').setView([26.0806, -98.2884], 12);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    L.marker([26.0806, -98.2884]).addTo(map)
        .bindPopup('Reynosa 📍')
        .openPopup();

    var lugares = [
        {
            nombre: "FYMERSA - Centro de metales ♻️",
            coords: [26.084526395888336, -98.29755461861764],
            info: "Recibe metales"
        },
        {
            nombre: "CORSA Recicladora ♻️",
            coords: [26.08932324746204, -98.31992874226934],
            info: "Materiales ferrosos y no ferrosos"
        },
        {
            nombre: "PLASTIREYSA ♻️",
            coords: [26.051331045072637, -98.3733933937469],
            info: "Centro de reciclaje de plástico"
        }
    ];

    let marcadores = [];

    lugares.forEach(lugar => {
        let marcador = L.marker(lugar.coords)
            .addTo(map)
            .bindPopup(`<b>${lugar.nombre}</b><br>${lugar.info}`);

        marcadores.push(marcador);
    });

    var grupo = L.featureGroup(marcadores);
    map.fitBounds(grupo.getBounds());

    setTimeout(() => {
        map.invalidateSize();
    }, 500);

});

// ===============================
// QUIZ + RACHAS
// ===============================
const preguntas = [
{
    pregunta: "¿Qué significan las 3R?",
    opciones: ["Reciclar, Reducir, Reutilizar", "Reubicar, Reducir, Reciclar", "Reutilizar, Rapidez, Reubicar"],
    correcta: 0
},
{
    pregunta: "¿Qué es el PET?",
    opciones: ["Polímero de Energía Térmica", "Polietileno Tereftalato", "Plástico transparente"],
    correcta: 1
}
];

let indice = 0;
let puntaje = 0;
let racha = 0;

function actualizarRacha(){
    document.getElementById("racha").textContent = `Racha: ${racha} 🔥`;
}

function cargarPregunta(){
    const p = document.getElementById("pregunta");
    const op = document.getElementById("opciones");
    const r = document.getElementById("resultado");

    r.textContent = "";
    op.innerHTML = "";

    p.textContent = preguntas[indice].pregunta;

    preguntas[indice].opciones.forEach((o,i)=>{
        const btn = document.createElement("button");
        btn.textContent = o;
        btn.onclick = ()=>verificar(i);
        op.appendChild(btn);
    });
}

function verificar(i){
    const r = document.getElementById("resultado");

    if(i===preguntas[indice].correcta){
        r.textContent = "¡Correcto! ✅";
        puntaje++;
        racha++;
        mostrarNotificacion("¡Bien hecho! +1 racha 🔥");
    } else {
        r.textContent = "Incorrecto ❌";
        racha = 0;
        mostrarNotificacion("Racha reiniciada ❌");
    }

    actualizarRacha();
}

function siguientePregunta(){
    indice++;

    if(indice < preguntas.length){
        cargarPregunta();
    } else {
        mostrarNotificacion("Juego terminado 🎉");
        document.getElementById("resultado").innerHTML = `Terminaste 🎉 Puntaje: ${puntaje}`;
    }
}

cargarPregunta();

// ===============================
// DRAG & DROP
// ===============================
const items = document.querySelectorAll(".item");
const botes = document.querySelectorAll(".bote");
let arrastrado = null;

items.forEach(item => {
    item.addEventListener("dragstart", () => {
        arrastrado = item;
    });
});

botes.forEach(bote => {
    bote.addEventListener("dragover", (e) => e.preventDefault());

    bote.addEventListener("drop", () => {
        const tipoBote = bote.getAttribute("data-tipo");
        const idItem = arrastrado.id;

        let correcto = idItem.includes(tipoBote);
        const resultado = document.getElementById("resultadoDrag");

        if(correcto){
            resultado.textContent = "¡Correcto! ✅";
            arrastrado.style.display = "none";
            racha++;
            mostrarNotificacion("¡Excelente! Sigue así 🔥");
        } else {
            resultado.textContent = "Incorrecto ❌";
            racha = 0;
            mostrarNotificacion("Ups... Racha perdida ❌");
        }

        actualizarRacha();
    });
});
</script>

</body>
</html>
