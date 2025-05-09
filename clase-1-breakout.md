# Clase 1: Introducción al Desarrollo de Juegos con JavaScript y Canvas

## Detalles del Curso
- **Proyecto Final**: Juego Breakout completo
- **Duración Total**: 4 clases de 3 horas cada una
- **Nivel**: Principiante a intermedio

## Objetivos de la Clase 1
- Comprender los fundamentos del desarrollo de juegos con JavaScript
- Aprender a trabajar con el elemento Canvas de HTML5
- Configurar la estructura básica del juego Breakout
- Dibujar y animar los primeros elementos del juego

## 1. Introducción al Desarrollo de Juegos Web (30 minutos)

### 1.1 Presentación y visión general del curso
- Bienvenida y presentación del instructor
- Descripción general del proyecto Breakout
- Explicación de la metodología de las 4 clases

### 1.2 ¿Por qué JavaScript y Canvas para juegos?
- Accesibilidad (solo necesitas un navegador)
- No requiere bibliotecas o motores externos
- Rendimiento adecuado para juegos 2D
- Facilidad de distribución (web)

### 1.3 Estructura básica de un juego
- Loop de juego (Game Loop)
- Detección de colisiones
- Manejo de entrada del usuario
- Actualización de estado y renderizado

## 2. Configuración Inicial del Proyecto (30 minutos)

### 2.1 Estructura de archivos
```
breakout/
  ├── index.html
  ├── styles.css
  └── script.js
```

### 2.2 HTML básico para nuestro juego

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego Breakout</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="game-container">
        <div class="score-lives">Puntuación: <span id="score">0</span> | Vidas: <span id="lives">3</span></div>
        <canvas id="gameCanvas" width="800" height="600"></canvas>
        <div class="game-over" id="gameOver">JUEGO TERMINADO</div>
        <div class="controls">
            <button id="startButton">Iniciar Juego</button>
            <p>Usa las flechas izquierda y derecha o el mouse para mover la paleta</p>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

### 2.3 CSS básico

```css
body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #222;
    font-family: Arial, sans-serif;
}

canvas {
    background: #000;
    border: 2px solid #333;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.game-container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.controls {
    margin-top: 20px;
    color: #fff;
    text-align: center;
}

.score-lives {
    position: absolute;
    top: 10px;
    left: 10px;
    color: #fff;
    font-size: 16px;
    font-weight: bold;
}

button {
    background: #4CAF50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 10px 2px;
    cursor: pointer;
    border-radius: 4px;
}

button:hover {
    background-color: #45a049;
}

.game-over {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: #fff;
    font-size: 48px;
    font-weight: bold;
    text-shadow: 2px 2px #000;
    display: none;
}
```

## 3. Introducción a Canvas (45 minutos)

### 3.1 ¿Qué es Canvas?
- Elemento HTML5 para dibujo gráfico
- API para gráficos 2D
- Coordenadas y sistema de pixeles

### 3.2 Obtener el contexto de dibujo

```javascript
// Obtenemos referencias a los elementos del DOM
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
```

### 3.3 Formas básicas en Canvas
- Rectángulos
- Círculos/Arcos
- Líneas
- Textos

### 3.4 Colores, estilos y rellenos
- fillStyle vs strokeStyle
- Transparencia (alpha)
- Gradientes

## 4. Ejercicio Práctico: Dibujar Elementos Básicos (30 minutos)

```javascript
// Dibujar un rectángulo (paleta)
function dibujarPaleta() {
    ctx.beginPath();
    ctx.rect(350, 580, 100, 15);  // x, y, width, height
    ctx.fillStyle = '#4169E1';    // Color azul royal
    ctx.fill();
    ctx.closePath();
}

// Dibujar un círculo (pelota)
function dibujarPelota() {
    ctx.beginPath();
    ctx.arc(400, 570, 10, 0, Math.PI * 2); // x, y, radio, ángulo inicial, ángulo final
    ctx.fillStyle = '#FF6347';             // Color tomate
    ctx.fill();
    ctx.closePath();
}

// Dibujar un ladrillo
function dibujarLadrillo() {
    ctx.beginPath();
    ctx.rect(200, 100, 75, 20);   // x, y, width, height
    ctx.fillStyle = '#FFA500';    // Color naranja
    ctx.fill();
    ctx.strokeStyle = '#000';     // Borde negro
    ctx.strokeRect(200, 100, 75, 20);
    ctx.closePath();
}

// Llamar a las funciones de dibujo
dibujarPaleta();
dibujarPelota();
dibujarLadrillo();
```

## 5. Creación de la Estructura de Objetos del Juego (45 minutos)

### 5.1 Definir variables y objetos del juego

```javascript
// Variables del juego
let score = 0;
let lives = 3;
let gameRunning = false;

// Propiedades de la pelota
const ball = {
    x: canvas.width / 2,
    y: canvas.height - 30,
    radius: 10,
    dx: 4,  // velocidad en x
    dy: -4, // velocidad en y (negativo = hacia arriba)
    color: '#FF6347'
};

// Propiedades de la paleta
const paddle = {
    width: 100,
    height: 15,
    x: (canvas.width - 100) / 2,
    y: canvas.height - 20,
    speed: 8,
    color: '#4169E1'
};

// Propiedades de los ladrillos
const brickRowCount = 5;
const brickColumnCount = 9;
const brickWidth = 75;
const brickHeight = 20;
const brickPadding = 10;
const brickOffsetTop = 60;
const brickOffsetLeft = 45;
```

### 5.2 Crear la matriz de ladrillos

```javascript
// Crear el arreglo de ladrillos
const bricks = [];

function crearLadrillos() {
    for (let c = 0; c < brickColumnCount; c++) {
        bricks[c] = [];
        for (let r = 0; r < brickRowCount; r++) {
            const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
            const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
            
            // Generar un color según la fila
            let brickColor;
            switch(r) {
                case 0: brickColor = '#FF6347'; break; // Tomate
                case 1: brickColor = '#FFA500'; break; // Naranja
                case 2: brickColor = '#FFD700'; break; // Dorado
                case 3: brickColor = '#32CD32'; break; // Verde Lima
                case 4: brickColor = '#4169E1'; break; // Azul Royal
                default: brickColor = '#FFF'; break;
            }
            
            bricks[c][r] = { 
                x: brickX, 
                y: brickY, 
                status: 1,  // 1 = visible, 0 = destruido
                color: brickColor 
            };
        }
    }
}
```

### 5.3 Funciones para dibujar todos los elementos

```javascript
// Dibujar la pelota
function dibujarPelota() {
    ctx.beginPath();
    ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
    ctx.fillStyle = ball.color;
    ctx.fill();
    ctx.closePath();
}

// Dibujar la paleta
function dibujarPaleta() {
    ctx.beginPath();
    ctx.rect(paddle.x, paddle.y, paddle.width, paddle.height);
    ctx.fillStyle = paddle.color;
    ctx.fill();
    ctx.closePath();
}

// Dibujar los ladrillos
function dibujarLadrillos() {
    for (let c = 0; c < brickColumnCount; c++) {
        for (let r = 0; r < brickRowCount; r++) {
            if (bricks[c][r].status === 1) {
                ctx.beginPath();
                ctx.rect(bricks[c][r].x, bricks[c][r].y, brickWidth, brickHeight);
                ctx.fillStyle = bricks[c][r].color;
                ctx.fill();
                ctx.strokeStyle = '#000';
                ctx.strokeRect(bricks[c][r].x, bricks[c][r].y, brickWidth, brickHeight);
                ctx.closePath();
            }
        }
    }
}

// Dibujar puntuación y vidas
function dibujarPuntuacion() {
    document.getElementById('score').textContent = score;
    document.getElementById('lives').textContent = lives;
}
```

## 6. Implementación del Game Loop Básico (30 minutos)

### 6.1 Crear la función principal de dibujo

```javascript
// Función principal de dibujo (Game Loop)
function dibujar() {
    // Limpiar el canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Dibujar elementos del juego
    dibujarLadrillos();
    dibujarPelota();
    dibujarPaleta();
    dibujarPuntuacion();
    
    // Continuar la animación
    if (gameRunning) {
        requestAnimationFrame(dibujar);
    }
}
```

### 6.2 Iniciar el juego

```javascript
// Función para iniciar el juego
function iniciarJuego() {
    if (!gameRunning) {
        gameRunning = true;
        document.getElementById('gameOver').style.display = 'none';
        crearLadrillos();
        dibujar();
    }
}

// Asignar el evento al botón de inicio
document.getElementById('startButton').addEventListener('click', iniciarJuego);

// Configuración inicial
crearLadrillos();
dibujarLadrillos();
dibujarPelota();
dibujarPaleta();
dibujarPuntuacion();
```

## 7. Animación Básica: Moviendo la Pelota (30 minutos)

### 7.1 Actualizar la posición de la pelota

```javascript
// Actualizar la posición de la pelota
function actualizarPelota() {
    ball.x += ball.dx;
    ball.y += ball.dy;
    
    // Colisión con paredes (izquierda y derecha)
    if (ball.x + ball.radius > canvas.width || ball.x - ball.radius < 0) {
        ball.dx = -ball.dx;
    }
    
    // Colisión con techo
    if (ball.y - ball.radius < 0) {
        ball.dy = -ball.dy;
    }
    
    // Colisión con suelo (perder vida)
    if (ball.y + ball.radius > canvas.height) {
        // Por ahora, solo invertimos la dirección
        ball.dy = -ball.dy;
    }
}
```

### 7.2 Actualizar el Game Loop para mover la pelota

```javascript
// Función principal de dibujo (Game Loop) actualizada
function dibujar() {
    // Limpiar el canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Dibujar elementos del juego
    dibujarLadrillos();
    dibujarPelota();
    dibujarPaleta();
    dibujarPuntuacion();
    
    // Actualizar la posición de la pelota
    actualizarPelota();
    
    // Continuar la animación
    if (gameRunning) {
        requestAnimationFrame(dibujar);
    }
}
```

## 8. Ejercicio Final: Juntando Todo (30 minutos)

Combina todos los elementos anteriores en un archivo script.js completo y funcional que muestre:
- Los ladrillos de colores
- La paleta
- La pelota moviéndose y rebotando en las paredes

```javascript
// Script.js completo para la Clase 1
document.addEventListener('DOMContentLoaded', () => {
    // Obtener elementos del DOM
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const startButton = document.getElementById('startButton');
    const scoreElement = document.getElementById('score');
    const livesElement = document.getElementById('lives');
    const gameOverElement = document.getElementById('gameOver');

    // Variables del juego
    let score = 0;
    let lives = 3;
    let gameRunning = false;
    let animationId;

    // Pelota
    const ball = {
        x: canvas.width / 2,
        y: canvas.height - 30,
        radius: 10,
        dx: 4,
        dy: -4,
        color: '#FF6347'
    };

    // Paleta
    const paddle = {
        width: 100,
        height: 15,
        x: (canvas.width - 100) / 2,
        y: canvas.height - 20,
        speed: 8,
        color: '#4169E1'
    };

    // Ladrillos
    const brickRowCount = 5;
    const brickColumnCount = 9;
    const brickWidth = 75;
    const brickHeight = 20;
    const brickPadding = 10;
    const brickOffsetTop = 60;
    const brickOffsetLeft = 45;
    const bricks = [];

    // Crear ladrillos
    function crearLadrillos() {
        for (let c = 0; c < brickColumnCount; c++) {
            bricks[c] = [];
            for (let r = 0; r < brickRowCount; r++) {
                const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
                const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
                
                // Generar color según la fila
                let brickColor;
                switch(r) {
                    case 0: brickColor = '#FF6347'; break; // Tomate
                    case 1: brickColor = '#FFA500'; break; // Naranja
                    case 2: brickColor = '#FFD700'; break; // Dorado
                    case 3: brickColor = '#32CD32'; break; // Verde Lima
                    case 4: brickColor = '#4169E1'; break; // Azul Royal
                    default: brickColor = '#FFF'; break;
                }
                
                bricks[c][r] = { 
                    x: brickX, 
                    y: brickY, 
                    status: 1, 
                    color: brickColor 
                };
            }
        }
    }

    // Dibujar pelota
    function dibujarPelota() {
        ctx.beginPath();
        ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
        ctx.fillStyle = ball.color;
        ctx.fill();
        ctx.closePath();
    }

    // Dibujar paleta
    function dibujarPaleta() {
        ctx.beginPath();
        ctx.rect(paddle.x, paddle.y, paddle.width, paddle.height);
        ctx.fillStyle = paddle.color;
        ctx.fill();
        ctx.closePath();
    }

    // Dibujar ladrillos
    function dibujarLadrillos() {
        for (let c = 0; c < brickColumnCount; c++) {
            for (let r = 0; r < brickRowCount; r++) {
                if (bricks[c][r].status === 1) {
                    ctx.beginPath();
                    ctx.rect(bricks[c][r].x, bricks[c][r].y, brickWidth, brickHeight);
                    ctx.fillStyle = bricks[c][r].color;
                    ctx.fill();
                    ctx.strokeStyle = '#000';
                    ctx.strokeRect(bricks[c][r].x, bricks[c][r].y, brickWidth, brickHeight);
                    ctx.closePath();
                }
            }
        }
    }

    // Dibujar puntuación
    function dibujarPuntuacion() {
        scoreElement.textContent = score;
        livesElement.textContent = lives;
    }

    // Actualizar posición de la pelota
    function actualizarPelota() {
        ball.x += ball.dx;
        ball.y += ball.dy;
        
        // Colisión con paredes (izquierda y derecha)
        if (ball.x + ball.radius > canvas.width || ball.x - ball.radius < 0) {
            ball.dx = -ball.dx;
        }
        
        // Colisión con techo
        if (ball.y - ball.radius < 0) {
            ball.dy = -ball.dy;
        }
        
        // Colisión con suelo (perder vida)
        if (ball.y + ball.radius > canvas.height) {
            // Por ahora, solo invertimos la dirección
            ball.dy = -ball.dy;
        }
    }

    // Game Loop principal
    function dibujar() {
        // Limpiar canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        // Dibujar elementos del juego
        dibujarLadrillos();
        dibujarPelota();
        dibujarPaleta();
        dibujarPuntuacion();
        
        // Actualizar estado del juego
        actualizarPelota();
        
        // Continuar animación
        if (gameRunning) {
            animationId = requestAnimationFrame(dibujar);
        }
    }

    // Iniciar juego
    function iniciarJuego() {
        if (!gameRunning) {
            gameRunning = true;
            gameOverElement.style.display = 'none';
            crearLadrillos();
            dibujar();
        }
    }

    // Event listeners
    startButton.addEventListener('click', iniciarJuego);

    // Configuración inicial
    crearLadrillos();
    dibujarLadrillos();
    dibujarPelota();
    dibujarPaleta();
    dibujarPuntuacion();
});
```

## Tareas para la Próxima Clase
1. Revisar el código de la clase 1
2. Experimentar con diferentes colores y tamaños para los elementos
3. Intentar responder: ¿Cómo podríamos implementar el control de la paleta con el teclado?
4. Opcional: Agregar más estilos al CSS para personalizar la apariencia del juego

## Recursos Adicionales
- [MDN Web Docs: Canvas API](https://developer.mozilla.org/es/docs/Web/API/Canvas_API)
- [Tutorial de Canvas en W3Schools](https://www.w3schools.com/html/html5_canvas.asp)
- [Documentación sobre requestAnimationFrame](https://developer.mozilla.org/es/docs/Web/API/window/requestAnimationFrame)
