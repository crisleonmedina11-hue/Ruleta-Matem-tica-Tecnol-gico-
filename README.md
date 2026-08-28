<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🎡 Ruleta Matemática</title>

<style>
*{box-sizing:border-box}

body{
    margin:0;
    min-height:100vh;
    font-family:Arial,sans-serif;
    background:linear-gradient(135deg,#172554,#2563eb);
    display:flex;
    justify-content:center;
    align-items:center;
    padding:20px
}

.pantalla{
    width:95%;
    max-width:1100px;
    background:white;
    padding:28px;
    border-radius:25px;
    text-align:center;
    box-shadow:0 15px 45px rgba(0,0,0,.35)
}

.oculto{display:none!important}

h1{
    color:#172554;
    font-size:38px;
    margin:10px
}

h2{color:#2563eb}

.grados{
    display:grid;
    grid-template-columns:repeat(5,1fr);
    gap:15px;
    margin-top:30px
}

.grado{
    border:none;
    padding:25px 10px;
    border-radius:18px;
    background:#2563eb;
    color:white;
    font-size:18px;
    font-weight:bold;
    cursor:pointer;
    transition:.2s
}

.grado:hover{
    transform:scale(1.06);
    background:#1d4ed8
}

.info{
    font-size:20px;
    font-weight:bold
}

#puntaje{color:#16a34a}

.nivel-facil{color:#16a34a}
.nivel-medio{color:#ca8a04}
.nivel-extremo{color:#dc2626}

.nivel-dios{
    color:#7e22ce;
    font-weight:900;
    text-shadow:0 0 8px #c084fc
}

.dios-box{
    background:linear-gradient(135deg,#581c87,#7e22ce,#4c1d95);
    color:white;
    padding:12px;
    border-radius:15px;
    margin:15px 0;
    font-size:20px;
    font-weight:bold
}

.ruleta-area{
    display:flex;
    justify-content:center;
    align-items:center;
    gap:35px;
    flex-wrap:wrap;
    margin-top:20px
}

.ruleta-contenedor{
    position:relative;
    width:420px;
    height:420px
}

canvas{
    width:420px;
    height:420px
}

.flecha{
    position:absolute;
    top:-17px;
    left:50%;
    transform:translateX(-50%);
    width:0;
    height:0;
    border-left:20px solid transparent;
    border-right:20px solid transparent;
    border-top:45px solid red;
    z-index:10
}

.botones{
    display:flex;
    flex-direction:column;
    gap:12px;
    min-width:280px
}

.boton{
    border:none;
    padding:15px 25px;
    border-radius:30px;
    color:white;
    font-size:18px;
    font-weight:bold;
    cursor:pointer;
    transition:.2s
}

.boton:hover{transform:scale(1.04)}

.girar{background:#f97316}
.continuar{background:#16a34a}
.volver{background:#64748b}

.boton:disabled{
    background:#999;
    cursor:not-allowed
}

#resultado{
    display:none;
    margin-top:25px;
    padding:25px;
    border-radius:18px;
    background:#eff6ff
}

#ejercicioResultado{
    font-size:24px;
    font-weight:bold;
    line-height:1.5;
    margin:15px auto;
    max-width:850px
}

#respuestaUsuario{
    width:85%;
    max-width:700px;
    padding:15px;
    border:3px solid #2563eb;
    border-radius:12px;
    font-size:19px;
    text-align:center
}

#mensajeRespuesta{
    font-size:21px;
    font-weight:bold
}

.oportunidades{
    font-size:22px;
    font-weight:bold;
    margin:10px
}

#tiempoTexto{
    font-size:23px;
    font-weight:bold;
    margin:10px
}

.barra-tiempo{
    width:90%;
    max-width:700px;
    height:18px;
    background:#dbeafe;
    border-radius:20px;
    margin:10px auto;
    overflow:hidden
}

#barraTiempo{
    height:100%;
    width:100%;
    background:#2563eb;
    transition:width 1s linear
}

#resumenErrores{
    margin-top:30px;
    text-align:left
}

.error-card{
    background:#fee2e2;
    border-left:6px solid #dc2626;
    padding:18px;
    border-radius:12px;
    margin:12px 0
}

.error-card h3{
    color:#b91c1c;
    margin-top:0
}

.mi-respuesta{
    color:#dc2626;
    font-weight:bold
}

.respuesta-correcta{
    color:#15803d;
    font-weight:bold
}

#felicitacion{
    display:none;
    background:#dcfce7;
    color:#15803d;
    padding:18px;
    border-radius:18px;
    margin-bottom:20px;
    font-size:24px;
    font-weight:bold
}

#gameOver{
    position:fixed;
    inset:0;
    z-index:9999;
    display:none;
    justify-content:center;
    align-items:center;
    flex-direction:column;
    background:radial-gradient(
        circle,
        #450a0a 0%,
        #1a0000 45%,
        #000 100%
    );
    color:white
}

#gameOver h1{
    color:#ef4444;
    font-size:clamp(48px,15vw,100px);
    margin:0;
    letter-spacing:4px;
    line-height:1.05;
    text-align:center;
    text-shadow:
        0 0 10px #ef4444,
        0 0 25px #dc2626,
        0 0 50px #991b1b
}

#gameOver p{
    font-size:clamp(18px,5vw,25px);
    text-align:center;
    margin:18px 20px
}

#contadorGameOver{
    font-size:clamp(17px,4.5vw,22px);
    color:#fca5a5;
    text-align:center
}

.correcto{
    background:#dcfce7!important;
    border:3px solid #16a34a
}

.incorrecto{
    background:#fee2e2!important;
    border:3px solid #dc2626
}

.finalizado{
    background:linear-gradient(135deg,#fef3c7,#dcfce7);
    border:4px solid #16a34a;
    padding:25px;
    border-radius:20px;
    margin-top:20px;
}

.finalizado h2{
    color:#15803d;
    font-size:30px;
}

@media(max-width:700px){

    body{
        padding:10px
    }

    .pantalla{
        width:100%;
        padding:18px
    }

    .grados{
        grid-template-columns:repeat(2,1fr)
    }

    .ruleta-contenedor,
    canvas{
        width:300px;
        height:300px
    }

    h1{
        font-size:30px
    }

    #ejercicioResultado{
        font-size:21px
    }

    .botones{
        width:100%;
        min-width:0
    }

    .boton{
        width:100%
    }
}
</style>
</head>

<body>

<!-- =========================
     GAME OVER
========================= -->

<div id="gameOver">

    <h1>GAME OVER</h1>

    <p>
        💀 Se acabaron tus oportunidades
    </p>

    <div id="contadorGameOver">
        🔄 Reiniciando en 5 segundos...
    </div>

</div>


<!-- =========================
     SELECCIÓN DE GRADO
========================= -->

<div class="pantalla" id="pantallaGrados">

    <h1>🎡 RULETA MATEMÁTICA</h1>

    <h2>📚 Selecciona tu grado</h2>

    <p>
        Responde correctamente para avanzar:
        <b>Fácil → Medio → Extremo → Dios</b>.
    </p>

    <div class="grados">

        <button class="grado" onclick="seleccionarGrado(1)">
            1.º Secundaria
        </button>

        <button class="grado" onclick="seleccionarGrado(2)">
            2.º Secundaria
        </button>

        <button class="grado" onclick="seleccionarGrado(3)">
            3.º Secundaria 🔥
        </button>

        <button class="grado" onclick="seleccionarGrado(4)">
            4.º Secundaria 🔥🔥
        </button>

        <button class="grado" onclick="seleccionarGrado(5)">
            5.º Secundaria 💀
        </button>

    </div>

</div>


<!-- =========================
     JUEGO
========================= -->

<div class="pantalla oculto" id="pantallaRuleta">

    <div id="felicitacion">
        🎉 ¡CORRECTO! 🎉
        <br>
        ¡Muy bien! Subes al siguiente nivel. 🧠👏
    </div>

    <h1>🎡 RULETA MATEMÁTICA</h1>

    <h2 id="tituloGrado"></h2>

    <div id="cajaDios" class="dios-box oculto">
        ☠️👑 NIVEL DIOS ACTIVADO 👑☠️
    </div>

    <p class="info">
        Nivel:
        <span id="nivelActual"></span>
    </p>

    <p class="info">
        ⭐ Puntaje:
        <span id="puntaje">0</span>
    </p>

    <div class="ruleta-area">

        <div class="ruleta-contenedor">

            <div class="flecha"></div>

            <canvas
                id="ruleta"
                width="420"
                height="420">
            </canvas>

        </div>

        <div class="botones">

            <button
                class="boton girar"
                id="botonGirar"
                onclick="girarRuleta()">

                🎲 GIRAR RULETA

            </button>

            <button
                class="boton continuar"
                id="botonContinuar"
                onclick="continuarNivel()"
                style="display:none">

                ➡️ SIGUIENTE NIVEL

            </button>

            <button
                class="boton volver"
                onclick="volverGrados()">

                ↩️ CAMBIAR GRADO

            </button>

        </div>

    </div>


    <!-- =========================
         PREGUNTA
    ========================= -->

    <div id="resultado">

        <h2 id="tituloProblema"></h2>

        <div
            class="oportunidades"
            id="oportunidades">
            ❤️❤️❤️
        </div>

        <div id="tiempoTexto">
            ⏱️ Tiempo:
            <span id="segundos">20</span> s
        </div>

        <div class="barra-tiempo">
            <div id="barraTiempo"></div>
        </div>

        <p id="ejercicioResultado"></p>

        <input
            type="text"
            id="respuestaUsuario"
            placeholder="✏️ Escribe tu respuesta">

        <br>
        <br>

        <button
            class="boton girar"
            id="botonComprobar"
            onclick="comprobarRespuesta()">

            ✅ COMPROBAR

        </button>

        <p id="mensajeRespuesta"></p>

    </div>


    <!-- =========================
         ERRORES
    ========================= -->

    <div
        id="resumenErrores"
        class="oculto">

        <h2>
            📚 REPASO DE TUS ERRORES
        </h2>

        <p>
            Estas son las preguntas que
            respondiste incorrectamente:
        </p>

        <div id="listaErrores"></div>

    </div>


    <!-- =========================
         FINAL
    ========================= -->

    <div
        id="finalNivel"
        class="finalizado oculto">

        <h2>🏆 ¡FELICIDADES! 🏆</h2>

        <p>
            Has completado correctamente
            todos los niveles.
        </p>

        <p>
            ☠️👑 ¡DERROTASTE EL NIVEL DIOS! 👑☠️
        </p>

        <h3>
            ⭐ Puntaje final:
            <span id="puntajeFinal">0</span>
        </h3>

        <button
            class="boton continuar"
            onclick="volverGrados()">

            🎮 JUGAR DE NUEVO

        </button>

    </div>

</div>


<script>

/* =========================================================
   EJERCICIOS CORREGIDOS
========================================================= */

const ejercicios={

1:{

facil:[
"Resuelve: 7 + 8 × 2.",
"Resuelve: 3x + 4 = 19.",
"Calcula: 144 ÷ 12 + 6.",
"Resuelve: 5x - 7 = 28.",
"Calcula: 25% de 200.",
"Resuelve: 2x + 9 = 25."
],

dificil:[
"Resuelve: 4x + 7 = 35.",
"Calcula: 18 + 6 × 4 - 10.",
"Resuelve: 7x - 9 = 40.",
"Calcula: 3² + 4² × 2.",
"Resuelve: 5(x + 2) = 35.",
"Calcula: 360 ÷ 9 + 17."
],

extremo:[
"Resuelve: 3(x + 4) - 5 = 25.",
"Resuelve: 4(2x - 3) = 36.",
"Calcula: 15% de 360.",
"Resuelve: 2(3x - 5) + 4 = 30.",
"Calcula: 5² + 3³ - 10.",
"Resuelve: 6x - 2(x + 3) = 18."
],

dios:[
"Resuelve: 3[2(x - 4) + 5] - 2(x + 7) = 21.",
"Resuelve: 5{2x - [3(x - 4) - 7]} = 55.",
"Resuelve: 4(3x - 2) - 2(5x - 7) = 18.",
"Calcula P(2), si P(x)=2x³-3x²+5x+4.",
"Resuelve: 2[3(x+1)-4] = 28.",
"Resuelve: 7x - 3(2x - 5) = 20."
]

},


2:{

facil:[
"Resuelve: 4x + 5 = 29.",
"Resuelve: 2x - 9 = 15.",
"Calcula: 35% de 200.",
"Calcula: 3² + 4².",
"Resuelve: 5x = 45.",
"Calcula: 18 × 7 - 20."
],

dificil:[
"Resuelve: 3x - 7 = 2x + 11.",
"Resuelve: 2(x - 3) + 5 = 17.",
"Calcula: 15% de 600.",
"Desarrolla (x + 3)². ¿Cuál es el coeficiente de x?",
"Resuelve: 4(x + 2) = 40.",
"Calcula: 2⁶ - 20."
],

extremo:[
"Resuelve: 5(x - 2) = 3x + 14.",
"Calcula: 25% de 480.",
"Resuelve: 3(x + 5) - 4 = 32.",
"Calcula: 18 × 15 - 72.",
"Resuelve: 7x - 3 = 4x + 21.",
"Calcula: 12² - 5³."
],

dios:[
"Resuelve: 4(2x-3)-3(x+5)+2(4-x)=19.",
"Resuelve: 5[2x-3(x-4)]+7=3x+35.",
"Desarrolla: 3(x+2)²-2(x-3)². ¿Cuál es el coeficiente de x²?",
"Resuelve: 4[2x-(x-7)] = 44.",
"Resuelve: 3[4x-2(x+1)] = 42.",
"Calcula: 35% de 1240."
]

},


3:{

facil:[
"Resuelve: 5x - 13 = 32.",
"Resuelve: 3x + 5 = 26.",
"Calcula: 2⁵ - 3³.",
"Calcula: 20% de 450.",
"Resuelve: 2(x + 5) = 30.",
"Calcula: 7² + 6²."
],

dificil:[
"Resuelve: 3(x - 4) = 2x + 9.",
"Calcula: 25% de 640.",
"Resuelve: 4x - 7 = 2x + 15.",
"Calcula: 3⁴ - 50.",
"Resuelve: 5(x + 3) = 2x + 27.",
"Calcula: 1440 ÷ 12 + 35."
],

extremo:[
"Resuelve: 2x² - 5x - 3 = 0. Escribe la solución positiva.",
"Resuelve: 3x² - 10x + 7 = 0. Escribe la solución mayor.",
"Resuelve: 3(x+2)-2(x-5)=27.",
"Calcula: 40% de 750.",
"Resuelve: 5(x-3)=2x+21.",
"Resuelve: x² - 6x + 8 = 0. Escribe la solución mayor."
],

dios:[
"Resuelve: 3(2x²-5x+4)-2(x²+3x-7)=4x²-19x+20.",
"Resuelve: 2[3x-4(x-5)]+5[2x-(x+3)]=47.",
"Resuelve: 3x²-12x+9=0. Escribe la solución mayor.",
"Calcula P(3), si P(x)=2x³-5x²+3x-8.",
"Resuelve: 4(x²-3x+2)-3(x²-5x+6)=x²+7.",
"Resuelve: 2x²-9x+10=0. Escribe la solución mayor."
]

},


4:{

facil:[
"Resuelve: 3x + 7 = 25.",
"Resuelve: 4x - 9 = 27.",
"Calcula: 2⁶ ÷ 2³.",
"Calcula: 35% de 400.",
"Resuelve: x² - 9x + 20 = 0. Escribe la solución mayor.",
"Simplifica: 7x² - 3x² + 2x. ¿Cuál es el coeficiente de x²?"
],

dificil:[
"Resuelve: 5x - 17 = 38.",
"Resuelve: 2(x+5)=34.",
"Calcula la suma de los primeros 10 números naturales.",
"Calcula: 15% de 800.",
"Resuelve: x² - 7x + 12 = 0. Escribe la solución mayor.",
"Resuelve: 3(x-2)+4=25."
],

extremo:[
"Resuelve: 3x² - 7x - 6 = 0. Escribe la solución positiva.",
"Resuelve: x² - 5x + 6 = 0. Escribe la solución mayor.",
"Resuelve: √(x+4)=6.",
"Calcula: 20% de 1250.",
"Resuelve: 4(x-3)-2(x+5)=10.",
"Encuentra el número positivo cuya suma con 12 sea 17 y cuyo producto con 12 sea 60."
],

dios:[
"Resuelve: 3(2x²-7x+5)-2(3x²-5x+8)=x-1.",
"Resuelve: 4[2x-(3x-7)]-3[x-2(x-5)]=31.",
"Factoriza: x⁴-5x²+4. Escribe la raíz positiva mayor.",
"Resuelve: x²-7x+10=0. Escribe la solución mayor.",
"Resuelve: 2x²-8x+6=0. Escribe la solución mayor.",
"Calcula: 25% de 2400."
]

},


5:{

facil:[
"Resuelve: x² - 5x + 6 = 0. Escribe la solución mayor.",
"Calcula: log₂(8).",
"Resuelve: 2ˣ = 32.",
"Calcula: 3⁴ - 2⁵.",
"Resuelve: 4x - 13 = 27.",
"Calcula: 18% de 1000."
],

dificil:[
"Resuelve: 5x - 17 = 38.",
"Resuelve: log₃(x)=4.",
"Resuelve: 3x² - 12x + 9 = 0. Escribe la solución mayor.",
"Calcula la suma de los primeros 10 términos: 5, 12, 19...",
"Resuelve: x² - 9x + 20 = 0. Escribe la solución mayor.",
"Calcula: 8% de 2500."
],

extremo:[
"Resuelve: x³ - 6x² + 11x - 6 = 0. Escribe la solución mayor.",
"Resuelve: 2ˣ + 2ˣ⁺¹ = 48.",
"Resuelve: x² - 7x + 10 = 0. Escribe la solución mayor.",
"Calcula: 15% de 3600.",
"Resuelve: 4x² - 20x + 24 = 0. Escribe la solución mayor.",
"Resuelve: 3x² - 15x + 18 = 0. Escribe la solución mayor."
],

dios:[
"Resuelve: 3x² - 8x + 4 = 0. Escribe la solución mayor.",
"Resuelve: 2[3x²-4(x-5)]-3[x²-2(x+1)]=4x+43.",
"Resuelve: 3^(2x)-10(3^x)+9=0. Escribe la solución mayor.",
"Resuelve: x³-6x²+11x-6=0. Escribe la solución mayor.",
"Resuelve: 2^(x+2)-5(2^x)+8=0. Escribe la solución mayor.",
"Resuelve: 2x²-10x+12=0. Escribe la solución mayor."
]

}

};


/* =========================================================
   RESPUESTAS VERIFICADAS
========================================================= */

const respuestas={

1:{
facil:[23,5,18,7,50,8],
dificil:[7,18,7,41,5,57],
extremo:[6,6,54,6,42,12],
dios:[11,8,6,18,5,5]
},

2:{
facil:[6,12,70,25,9,106],
dificil:[18,11,90,6,10,44],
extremo:[12,120,7,198,8,19],
dios:[38/3,4,1,4,8,434]
},

3:{
facil:[9,7,5,90,10,85],
dificil:[21,160,11,31,6,155],
extremo:[3,7/3,17,300,12,4],
dios:[3,22/3,3,10,17/3,2]
},

4:{
facil:[6,9,8,140,5,4],
dificil:[11,12,55,120,4,7],
extremo:[3,-2/3,32,250,12,5],
dios:[0,-33,2,5,3,600]
},

5:{
facil:[3,3,5,49,10,180],
dificil:[11,81,3,365,5,200],
extremo:[3,4,5,540,3,3],
dios:[2,1,2,3,2,3]
}

};


/* =========================================================
   VARIABLES
========================================================= */

let gradoActual=0;
let nivelActual="facil";
let problemasActuales=[];
let respuestasActuales=[];
let indiceActual=0;

let angulo=0;
let girando=false;

let puntaje=0;
let oportunidades=3;

let errores=[];

let timer=null;
let segundos=20;

let usados=new Set();

let juegoPerdido=false;


/* =========================================================
   CANVAS
========================================================= */

const canvas=document.getElementById("ruleta");
const ctx=canvas.getContext("2d");

const colores=[
"#ef4444",
"#3b82f6",
"#22c55e",
"#eab308",
"#a855f7",
"#f97316"
];


/* =========================================================
   SELECCIONAR GRADO
========================================================= */

function seleccionarGrado(g){

    gradoActual=g;

    nivelActual="facil";

    puntaje=0;

    oportunidades=3;

    errores=[];

    usados=new Set();

    juegoPerdido=false;

    angulo=0;

    cargarNivel();

    document
    .getElementById("pantallaGrados")
    .classList.add("oculto");

    document
    .getElementById("pantallaRuleta")
    .classList.remove("oculto");

    document
    .getElementById("tituloGrado")
    .textContent=
        g+".º de Secundaria";

    document
    .getElementById("puntaje")
    .textContent="0";

    document
    .getElementById("finalNivel")
    .classList.add("oculto");

    document
    .getElementById("botonGirar")
    .style.display="inline-block";

    document
    .getElementById("botonGirar")
    .disabled=false;

    ocultarPregunta();

    mostrarNombreNivel();

    dibujarRuleta();

    window.scrollTo({
        top:0,
        behavior:"smooth"
    });
}


/* =========================================================
   CARGAR NIVEL
========================================================= */

function cargarNivel(){

    problemasActuales=
        ejercicios[gradoActual][nivelActual];

    respuestasActuales=
        respuestas[gradoActual][nivelActual];

    indiceActual=0;
}


/* =========================================================
   MOSTRAR NIVEL
========================================================= */

function mostrarNombreNivel(){

    const e=
        document.getElementById("nivelActual");

    e.className="";

    const nombres={

        facil:[
            "🟢 FÁCIL",
            "nivel-facil"
        ],

        dificil:[
            "🟡 MEDIO",
            "nivel-medio"
        ],

        extremo:[
            "🔴 EXTREMO",
            "nivel-extremo"
        ],

        dios:[
            "☠️👑 DIOS",
            "nivel-dios"
        ]

    };

    e.textContent=
        nombres[nivelActual][0];

    e.className=
        nombres[nivelActual][1];

    document
    .getElementById("cajaDios")
    .classList.toggle(
        "oculto",
        nivelActual!=="dios"
    );
}


/* =========================================================
   SIGUIENTE NIVEL
========================================================= */

function continuarNivel(){

    const orden=[
        "facil",
        "dificil",
        "extremo",
        "dios"
    ];

    const i=
        orden.indexOf(nivelActual);

    if(i>=3){

        finalizarJuego();

        return;
    }

    nivelActual=
        orden[i+1];

    cargarNivel();

    usados=new Set();

    oportunidades=3;

    document
    .getElementById("felicitacion")
    .style.display="none";

    ocultarPregunta();

    mostrarNombreNivel();

    document
    .getElementById("botonGirar")
    .disabled=false;

    document
    .getElementById("botonGirar")
    .style.display="inline-block";

    dibujarRuleta();

    window.scrollTo({
        top:0,
        behavior:"smooth"
    });
}


/* =========================================================
   DIBUJAR RULETA
========================================================= */

function dibujarRuleta(){

    const c=canvas.width/2;

    const r=190;

    const n=
        problemasActuales.length;

    const t=
        2*Math.PI/n;

    ctx.clearRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    for(let i=0;i<n;i++){

        const a=
            angulo+i*t;

        const b=
            a+t;

        ctx.beginPath();

        ctx.moveTo(c,c);

        ctx.arc(
            c,
            c,
            r,
            a,
            b
        );

        ctx.closePath();

        ctx.fillStyle=
            colores[i%colores.length];

        ctx.fill();

        ctx.strokeStyle="white";

        ctx.lineWidth=3;

        ctx.stroke();

        ctx.save();

        ctx.translate(c,c);

        ctx.rotate(
            a+t/2
        );

        ctx.fillStyle="white";

        ctx.font=
            "bold 14px Arial";

        ctx.textAlign="center";

        ctx.fillText(
            "Problema "+(i+1),
            r*.62,
            5
        );

        ctx.restore();
    }

    ctx.beginPath();

    ctx.arc(
        c,
        c,
        38,
        0,
        2*Math.PI
    );

    ctx.fillStyle="white";

    ctx.fill();

    ctx.beginPath();

    ctx.arc(
        c,
        c,
        25,
        0,
        2*Math.PI
    );

    ctx.fillStyle="#172554";

    ctx.fill();
}


/* =========================================================
   GIRAR RULETA
========================================================= */

function girarRuleta(){

    if(
        girando ||
        juegoPerdido ||
        usados.size>=problemasActuales.length
    )
        return;

    girando=true;

    document
    .getElementById("botonGirar")
    .disabled=true;

    const disponibles=[];

    for(
        let i=0;
        i<problemasActuales.length;
        i++
    ){

        if(!usados.has(i))
            disponibles.push(i);
    }

    const elegido=
        disponibles[
            Math.floor(
                Math.random()*
                disponibles.length
            )
        ];

    const n=
        problemasActuales.length;

    const t=
        2*Math.PI/n;

    const centroObjetivo=
        Math.PI*1.5-
        (elegido*t+t/2);

    let delta=
        (centroObjetivo-angulo)
        %(2*Math.PI);

    if(delta<0)
        delta+=2*Math.PI;

    delta+=
        (5+
        Math.floor(
            Math.random()*3
        ))*2*Math.PI;

    const inicio=angulo;

    const duracion=3000;

    const tiempoInicio=
        performance.now();

    function animar(now){

        const p=
            Math.min(
                (now-tiempoInicio)/
                duracion,
                1
            );

        const suave=
            1-Math.pow(
                1-p,
                4
            );

        angulo=
            inicio+
            delta*suave;

        dibujarRuleta();

        if(p<1){

            requestAnimationFrame(animar);

        }else{

            indiceActual=elegido;

            usados.add(elegido);

            girando=false;

            mostrarProblema();
        }
    }

    requestAnimationFrame(animar);
}


/* =========================================================
   MOSTRAR PROBLEMA
========================================================= */

function mostrarProblema(){

    document
    .getElementById("tituloProblema")
    .textContent=
        "🎯 RESUELVE";

    document
    .getElementById("ejercicioResultado")
    .textContent=
        problemasActuales[indiceActual];

    document
    .getElementById("resultado")
    .style.display="block";

    document
    .getElementById("respuestaUsuario")
    .value="";

    document
    .getElementById("respuestaUsuario")
    .disabled=false;

    document
    .getElementById("botonComprobar")
    .disabled=false;

    document
    .getElementById("mensajeRespuesta")
    .textContent=
        "🧠 Piensa rápido y responde.";

    document
    .getElementById("mensajeRespuesta")
    .style.color="#2563eb";

    actualizarOportunidades();

    iniciarTemporizador();

    document
    .getElementById("resultado")
    .scrollIntoView({
        behavior:"smooth",
        block:"center"
    });

    document
    .getElementById("respuestaUsuario")
    .focus();
}


/* =========================================================
   TEMPORIZADOR
========================================================= */

function iniciarTemporizador(){

    detenerTemporizador();

    segundos=20;

    actualizarTiempo();

    timer=setInterval(

        ()=>{

            segundos--;

            actualizarTiempo();

            if(segundos<=0){

                detenerTemporizador();

                tiempoAgotado();

            }

        },

        1000
    );
}


function detenerTemporizador(){

    if(timer){

        clearInterval(timer);

        timer=null;

    }
}


function actualizarTiempo(){

    document
    .getElementById("segundos")
    .textContent=
        segundos;

    document
    .getElementById("barraTiempo")
    .style.width=
        (segundos/20*100)+"%";
}


/* =========================================================
   TIEMPO AGOTADO
========================================================= */

function tiempoAgotado(){

    if(juegoPerdido)
        return;

    const campo=
        document.getElementById(
            "respuestaUsuario"
        );

    campo.disabled=true;

    document
    .getElementById("botonComprobar")
    .disabled=true;

    oportunidades--;

    puntaje=
        Math.max(
            0,
            puntaje-2
        );

    document
    .getElementById("puntaje")
    .textContent=
        puntaje;

    errores.push({

        problema:
            problemasActuales[
                indiceActual
            ],

        usuario:
            "Sin respuesta (tiempo agotado)",

        correcta:
            respuestasActuales[
                indiceActual
            ]

    });

    actualizarOportunidades();

    const m=
        document.getElementById(
            "mensajeRespuesta"
        );

    m.textContent=
        "⏰ ¡TIEMPO AGOTADO! -2 PUNTOS";

    m.style.color="red";

    if(oportunidades<=0){

        juegoPerdido=true;

        mostrarGameOver();

        return;
    }

    setTimeout(
        volverARuleta,
        2000
    );
}


/* =========================================================
   VIDAS
========================================================= */

function actualizarOportunidades(){

    let s="";

    for(let i=0;i<3;i++){

        s+=
            i<oportunidades
            ?"❤️"
            :"🖤";
    }

    document
    .getElementById("oportunidades")
    .innerHTML=
        s+
        "<br><small>"+
        oportunidades+
        " oportunidades restantes"+
        "</small>";
}


/* =========================================================
   CONVERTIR FRACCIÓN
========================================================= */

function convertirFraccion(texto){

    const partes=
        texto.split("/");

    if(partes.length!==2)
        return null;

    const a=
        Number(partes[0]);

    const b=
        Number(partes[1]);

    if(
        !Number.isFinite(a) ||
        !Number.isFinite(b) ||
        b===0
    )
        return null;

    return a/b;
}


/* =========================================================
   NORMALIZAR RESPUESTA
========================================================= */

function obtenerNumero(valor){

    let v=
        String(valor)
        .trim()
        .replace(",", ".")
        .replace(/\s+/g,"");

    if(v.includes("/")){

        const f=
            convertirFraccion(v);

        if(f!==null)
            return f;
    }

    const n=Number(v);

    if(Number.isFinite(n))
        return n;

    return null;
}


/* =========================================================
   COMPARAR RESPUESTAS
========================================================= */

function respuestasIguales(usuario, correcta){

    const a=
        obtenerNumero(usuario);

    const b=
        obtenerNumero(correcta);

    if(
        a!==null &&
        b!==null
    ){

        return Math.abs(a-b)<0.0001;
    }

    return String(usuario)
        .trim()
        .toLowerCase()===
        String(correcta)
        .trim()
        .toLowerCase();
}


/* =========================================================
   FORMATO DE RESPUESTA
========================================================= */

function mostrarRespuesta(r){

    if(
        typeof r==="number" &&
        !Number.isInteger(r)
    ){

        const max=1000;

        for(
            let d=1;
            d<=max;
            d++
        ){

            const n=
                Math.round(r*d);

            if(
                Math.abs(
                    n/d-r
                )<0.000001
            ){

                return n+"/"+d;
            }
        }
    }

    return String(r);
}


/* =========================================================
   COMPROBAR
========================================================= */

function comprobarRespuesta(){

    if(juegoPerdido)
        return;

    const campo=
        document.getElementById(
            "respuestaUsuario"
        );

    const u=
        campo.value.trim();

    if(!u){

        document
        .getElementById(
            "mensajeRespuesta"
        )
        .textContent=
            "⚠️ Escribe una respuesta.";

        return;
    }

    detenerTemporizador();

    const correcta=
        respuestasActuales[
            indiceActual
        ];

    const m=
        document.getElementById(
            "mensajeRespuesta"
        );


    /* =========================
       CORRECTO
    ========================= */

    if(
        respuestasIguales(
            u,
            correcta
        )
    ){

        puntaje+=15;

        document
        .getElementById("puntaje")
        .textContent=
            puntaje;

        campo.disabled=true;

        document
        .getElementById(
            "botonComprobar"
        )
        .disabled=true;

        document
        .getElementById(
            "felicitacion"
        )
        .style.display="block";

        m.textContent=
            "🎉 ¡CORRECTO! +15 PUNTOS";

        m.style.color="green";

        document
        .getElementById(
            "resultado"
        )
        .classList.add("correcto");


        setTimeout(

            ()=>{

                document
                .getElementById(
                    "resultado"
                )
                .classList.remove(
                    "correcto"
                );

                document
                .getElementById(
                    "felicitacion"
                )
                .style.display="none";

                const orden=[
                    "facil",
                    "dificil",
                    "extremo",
                    "dios"
                ];

                const i=
                    orden.indexOf(
                        nivelActual
                    );

                if(i<3){

                    continuarNivel();

                }else{

                    finalizarJuego();
                }

            },

            2000
        );

    }

    /* =========================
       INCORRECTO
    ========================= */

    else{

        oportunidades--;

        puntaje=
            Math.max(
                0,
                puntaje-5
            );

        document
        .getElementById("puntaje")
        .textContent=
            puntaje;

        errores.push({

            problema:
                problemasActuales[
                    indiceActual
                ],

            usuario:u,

            correcta:correcta

        });

        actualizarOportunidades();

        campo.disabled=true;

        document
        .getElementById(
            "botonComprobar"
        )
        .disabled=true;

        m.innerHTML=
            "❌ INCORRECTO. La respuesta correcta es: <b>"+
            mostrarRespuesta(correcta)+
            "</b>";

        m.style.color="red";

        document
        .getElementById(
            "resultado"
        )
        .classList.add("incorrecto");


        if(oportunidades<=0){

            juegoPerdido=true;

            setTimeout(
                ()=>mostrarGameOver(),
                2000
            );

            return;
        }


        setTimeout(

            ()=>{

                document
                .getElementById(
                    "resultado"
                )
                .classList.remove(
                    "incorrecto"
                );

                volverARuleta();

            },

            2000
        );
    }
}


/* =========================================================
   VOLVER A RULETA
========================================================= */

function volverARuleta(){

    ocultarPregunta();

    document
    .getElementById(
        "botonGirar"
    )
    .style.display=
        "inline-block";

    document
    .getElementById(
        "botonGirar"
    )
    .disabled=false;

    window.scrollTo({
        top:0,
        behavior:"smooth"
    });
}


function ocultarPregunta(){

    detenerTemporizador();

    document
    .getElementById(
        "resultado"
    )
    .style.display=
        "none";
}


/* =========================================================
   GAME OVER
========================================================= */

function mostrarGameOver(){

    mostrarErrores();

    const p=
        document.getElementById(
            "gameOver"
        );

    const c=
        document.getElementById(
            "contadorGameOver"
        );

    p.style.display="flex";

    let s=5;

    c.textContent=
        "🔄 Reiniciando en "+
        s+
        " segundos...";

    const it=setInterval(

        ()=>{

            s--;

            if(s>0){

                c.textContent=
                    "🔄 Reiniciando en "+
                    s+
                    " segundos...";

            }else{

                clearInterval(it);

                reiniciarJuego();

            }

        },

        1000
    );
}


/* =========================================================
   ERRORES
========================================================= */

function mostrarErrores(){

    const c=
        document.getElementById(
            "listaErrores"
        );

    c.innerHTML="";

    errores.forEach(

        (e,i)=>{

            const d=
                document.createElement(
                    "div"
                );

            d.className=
                "error-card";

            d.innerHTML=`

                <h3>
                    ❌ Error ${i+1}
                </h3>

                <p>
                    <strong>
                    📚 Problema:
                    </strong>
                    ${e.problema}
                </p>

                <p class="mi-respuesta">
                    ❌ Tu respuesta:
                    ${e.usuario}
                </p>

                <p class="respuesta-correcta">
                    ✅ Respuesta correcta:
                    ${mostrarRespuesta(e.correcta)}
                </p>

            `;

            c.appendChild(d);
        }
    );

    document
    .getElementById(
        "resumenErrores"
    )
    .classList.remove("oculto");
}


/* =========================================================
   FINALIZAR JUEGO
========================================================= */

function finalizarJuego(){

    detenerTemporizador();

    document
    .getElementById(
        "resultado"
    )
    .style.display="none";

    document
    .getElementById(
        "felicitacion"
    )
    .style.display="none";

    document
    .getElementById(
        "botonGirar"
    )
    .style.display="none";

    document
    .getElementById(
        "finalNivel"
    )
    .classList.remove("oculto");

    document
    .getElementById(
        "puntajeFinal"
    )
    .textContent=
        puntaje;

    mostrarErrores();
}


/* =========================================================
   REINICIAR
========================================================= */

function reiniciarJuego(){

    document
    .getElementById(
        "gameOver"
    )
    .style.display="none";

    document
    .getElementById(
        "pantallaRuleta"
    )
    .classList.add("oculto");

    document
    .getElementById(
        "pantallaGrados"
    )
    .classList.remove("oculto");

    gradoActual=0;

    nivelActual="facil";

    puntaje=0;

    oportunidades=3;

    errores=[];

    usados=new Set();

    juegoPerdido=false;

    ocultarPregunta();

    document
    .getElementById(
        "resumenErrores"
    )
    .classList.add("oculto");

    document
    .getElementById(
        "listaErrores"
    )
    .innerHTML="";

    document
    .getElementById(
        "finalNivel"
    )
    .classList.add("oculto");
}


/* =========================================================
   CAMBIAR GRADO
========================================================= */

function volverGrados(){

    detenerTemporizador();

    document
    .getElementById(
        "pantallaRuleta"
    )
    .classList.add("oculto");

    document
    .getElementById(
        "pantallaGrados"
    )
    .classList.remove("oculto");

    document
    .getElementById(
        "finalNivel"
    )
    .classList.add("oculto");

    juegoPerdido=false;

    gradoActual=0;

    nivelActual="facil";

    puntaje=0;

    oportunidades=3;
}


/* =========================================================
   ENTER PARA RESPONDER
========================================================= */

document
.getElementById(
    "respuestaUsuario"
)
.addEventListener(
    "keydown",
    e=>{

        if(e.key==="Enter")
            comprobarRespuesta();

    }
);

</script>

</body>
</html>
