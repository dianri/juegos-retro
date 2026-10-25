# Pac-Man Arcade

Un remake arcade de **Pac-Man** construido con HTML5, CSS3 y JavaScript puro (Canvas 2D), con audio procedural generado en tiempo real mediante la Web Audio API y soporte para aplicaciones contenedoras mediante `postMessage`.

El juego conserva la esencia del clásico —laberintos, puntos, power pellets y fantasmas— y la amplía con **4 niveles generados proceduralmente, frutas con power-ups, un jefe final, vidas, récords y pantalla de victoria**.

---

## 🟡 Características

- **Sin dependencias externas**: todo el juego vive en un único `index.html` que funciona en cualquier navegador moderno.
- **Laberintos procedurales**: cada nivel se genera con una semilla propia. El juego conserva la componente transitable más grande, conecta la salida de la casa de fantasmas y coloca los puntos solo en zonas alcanzables.
- **Casa de fantasmas con anillo transitable** y salida vertical conectada al laberinto.
- **Audio procedural**: pasos, power pellets, frutas, fantasmas devorados, escudo, muerte y fanfarria generados con osciladores, ruido y filtros de la Web Audio API, sin archivos de sonido.
- **Motor de paso fijo a 60 Hz**, independiente de la tasa de refresco del monitor.
- **Tres fantasmas clásicos** con IA de persecución basada en BFS, modo disperso y regreso a la casa al ser devorados.
- **Jefe final DOH**: en el nivel 4 aparece un cuarto fantasma gigante con corona, **4 puntos de vida** y barra de vida propia.
- **Power-ups mediante fruta**: velocidad, escudo, congelación y doble puntuación.
- **Efectos visuales**: laberinto neón, resplandores, partículas, anillos de energía, parpadeo de fantasmas asustados y sacudida de pantalla.
- **Controles de teclado, ratón y táctiles**: flechas o WASD, deslizamiento sobre el canvas, botones direccionales y botón de reinicio.
- **Persistencia local** del récord y del nivel máximo alcanzado.
- **Integración con apps contenedoras** mediante `postMessage` al terminar la partida.
- **Diseño responsive** que conserva la proporción 4:3 del canvas.

---

## 🎮 Controles

### Teclado

| Tecla | Acción |
|---|---|
| `←` / `A` | Mover a la izquierda |
| `→` / `D` | Mover a la derecha |
| `↑` / `W` | Mover arriba |
| `↓` / `S` | Mover abajo |
| `Espacio` / `Enter` | Comenzar la partida o volver a jugar |
| `P` / `Esc` | Pausar / continuar |
| `C` | Continuar desde el nivel actual tras perder todas las vidas |
| `R` | Reiniciar el nivel actual; reinicia la partida completa en fin de partida o victoria |

### Ratón y táctil

- **Desliza** sobre el canvas en la dirección deseada para mover a Pac-Man.
- Usa los botones `◀`, `▲`, `▼` y `▶` en dispositivos táctiles.
- Toca el canvas para comenzar o para volver a jugar después de una derrota o victoria.
- El botón **REINICIAR NIVEL** vuelve a cargar el nivel actual.

Pac-Man se mueve automáticamente por el pasillo actual y cambia de dirección en la siguiente intersección compatible. Los giros se pueden anticipar pulsando una dirección antes de llegar a la intersección.

---

## 🕹️ Mecánicas principales

### Laberinto y puntos

Cada nivel genera un laberinto de **25 × 23 celdas** con pasillos de 24 px. El juego busca la componente transitable más grande, conecta la salida de la casa de fantasmas y rellena únicamente esas celdas con puntos.

- **Punto normal**: 10 puntos.
- **Power pellet**: 50 puntos y activa el modo asustado de los fantasmas.
- **Nivel normal completado**: 250 puntos.
- Al comer un power pellet, los fantasmas activos se vuelven vulnerables durante unos **7,5 segundos** (la duración varía ligeramente por nivel).

### Fantasmas

Hay tres fantasmas normales con colores rojo, rosa y azul. Se mueven por el laberinto con una IA que calcula el camino más corto hacia Pac-Man mediante BFS, alternando periodos de persecución y dispersión.

- Al ser devorados en modo asustado, se convierten en ojos y regresan a la casa para regenerarse.
- La puntuación por fantasmas devorados en cadena es **200, 400, 800 y 1600**.
- La casa de fantasmas está rodeada por un anillo transitable; los fantasmas salen por la puerta superior y se incorporan al laberinto.

### Power-ups

Las frutas aparecen periódicamente en una celda del laberinto y desaparecen si no se recogen a tiempo. Cada fruta otorga **50 puntos** y activa uno de estos efectos:

| Power-up | Icono | Efecto | Duración |
|---|---|---|---:|
| Velocidad | `»` | Aumenta la velocidad de Pac-Man un 32 % | 10 s |
| Escudo | `S` | Absorbe un impacto y manda al fantasma a casa | 15 s |
| Congelar | `❄` | Detiene a los fantasmas mientras esté activo | 10 s |
| Doble puntos | `2X` | Duplica todos los puntos obtenidos | 10 s |

La secuencia de frutas es **velocidad → escudo → congelar → doble puntos** y se repite.

### Vidas y progresión

- La partida comienza con **3 vidas**.
- Al perder una vida se conserva la puntuación y se reinician las posiciones de Pac-Man y los fantasmas con un breve periodo de invulnerabilidad.
- Al quedarse sin vidas aparece **FIN DE LA PARTIDA**; `C` permite continuar desde el nivel actual con 3 vidas nuevas.
- Completar los niveles 1, 2 y 3 carga el siguiente nivel tras un breve interludio.
- El nivel 4 es el enfrentamiento contra **DOH**.

---

## 🌈 Niveles

### Nivel 1 — Laberinto neón
Laberinto de calentamiento con tres fantasmas y cuatro power pellets. Ideal para aprender los giros, la casa de fantasmas y el ritmo de la persecución.

### Nivel 2 — Pasillos sin salida
Los fantasmas son algo más rápidos y las rutas se vuelven más laberínticas. El modo asustado dura un poco menos.

### Nivel 3 — Fantasmas en celo
Mayor velocidad de Pac-Man y de los fantasmas, con apariciones más tempranas y menos margen para los errores.

### Nivel 4 — DOH, el rey fantasma
El jefe final **DOH** aparece en la casa junto a los tres fantasmas normales. Es más grande, lleva corona y tiene **4 puntos de vida**.

- Su barra de vida se muestra en la parte inferior del canvas.
- Cada impacto de Pac-Man en modo asustado le quita **1 punto de vida** y otorga **1000 puntos**.
- Al recibir un impacto vuelve a la casa y se regenera durante un tiempo.
- Si no quedan power pellets en el laberinto, reaparece uno periódicamente para que siempre haya una forma de atacarlo.
- Al derrotarlo se obtienen **1500 puntos** adicionales y se muestra la pantalla de victoria.

---

## 🏆 Puntuación

| Acción | Puntos |
|---|---:|
| Punto normal | 10 |
| Power pellet | 50 |
| Fruta | 50 |
| Primer fantasma devorado | 200 |
| Segundo fantasma devorado | 400 |
| Tercer fantasma devorado | 800 |
| Cuarto fantasma devorado | 1600 |
| Impacto al jefe DOH | 1000 |
| Completar un nivel normal | 250 |
| Derrotar a DOH | 1500 |

El récord y el nivel máximo se guardan en `localStorage` con las claves `pm_high` y `pm_maxlevel`. El HUD muestra la puntuación, el récord, el nivel máximo, el nivel actual, el objetivo y las vidas restantes.

---

## 🔌 Integración con una app contenedora

Al terminar la partida (derrota o victoria) el juego envía un mensaje a la ventana padre:

```js
window.parent.postMessage({
  type: 'PAC_MAN_SCORE_SUBMISSION',
  game: 'pac-man',
  score: 12450,
  level: 4,
  highScore: 18400
}, '*');
```

Ejemplo de escucha en la aplicación contenedora:

```js
window.addEventListener('message', (event) => {
  if (event.data && event.data.type === 'PAC_MAN_SCORE_SUBMISSION') {
    console.log('Puntuación recibida:', event.data.score);
    console.log('Nivel alcanzado:', event.data.level);
    console.log('Récord local:', event.data.highScore);
  }
});
```

El mensaje solo se envía cuando el juego está dentro de un `iframe` o ventana contenedora; al abrirlo directamente no se genera tráfico de mensajes.

---

## 🛠️ Detalles técnicos

- **Canvas interno**: 800 × 600 px, escalado de forma responsive conservando la proporción 4:3.
- **Laberinto**: 25 columnas × 23 filas; celdas de 24 px; offset `MX = 100`, `MY = 30`.
- **Bucle**: `requestAnimationFrame` con acumulador y paso fijo de `1000 / 60` ms.
- **Movimiento**: interpolación entre celdas con giros anticipados y reversión a mitad de tile.
- **IA**: BFS por nivel para persecución, camino de regreso a casa y cálculo de distancias; modo disperso por temporizador.
- **Audio**: Web Audio API; el contexto se crea o reanuda con la primera interacción del usuario.
- **Persistencia**: `localStorage` para récord y nivel máximo.
- **Sin frameworks, sin dependencias, sin imágenes ni audio externos**.
- **Controles adaptativos**: los botones táctiles solo se muestran en dispositivos sin hover o pantallas estrechas.

---

## ▶️ Cómo jugar

Abre directamente `Pac-Man/index.html` en el navegador o sirve el repositorio con cualquier servidor estático:

```bash
# Desde la raíz del repositorio
python -m http.server 8000
```

Después visita:

```text
http://localhost:8000/Pac-Man/
```

También puedes abrir el archivo con doble clic; el juego no necesita red ni recursos externos.

---

## 📁 Estructura

```text
Pac-Man/
├── index.html   # Juego completo: HTML, CSS, Canvas, audio y lógica
└── README.md    # Este documento
```
