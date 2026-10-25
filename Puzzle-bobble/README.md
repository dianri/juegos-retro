# Puzzle Bobble Arcade

Un homenaje arcade a **Puzzle Bobble / Bust-a-Move** desarrollado en HTML5, CSS3 y JavaScript puro (Canvas 2D), con audio procedural generado mediante la Web Audio API y soporte para integración en aplicaciones contenedor mediante `postMessage`.

El juego conserva la esencia del clásico (dispara burbujas, forma grupos de tres o más y haz caer la estructura) y la amplía con **cuatro niveles, burbujas de poder, piedras, filas que descienden y un jefe final**: **DOH, la burbuja madre**.

---

## 🚀 Características

- **Sin dependencias externas**: todo el juego vive en un único `index.html` ejecutable en cualquier navegador moderno.
- **Audio procedural**: disparos, rebotes, explosiones, cadenas y fanfarrias generados en tiempo real con osciladores y ruido de la Web Audio API, sin archivos de audio.
- **Motor con paso de tiempo fijo (60 Hz)**, independiente de la tasa de refresco del monitor.
- **Rejilla hexagonal real**: filas pares de 22 celdas y filas impares de 21, con vecinos correctos y ajuste a la celda vacía más cercana.
- **Física de disparo con rebotes en las paredes** y sub-pasos para evitar el efecto túnel.
- **Guía de puntería** que simula la trayectoria completa, incluidos los rebotes.
- **4 niveles progresivos**: tres tableros temáticos y un combate final contra DOH.
- **Burbujas de poder deterministas**: cada nivel reparte siempre los mismos tipos de poder.
- **Piedras**: bloques que no se pueden reventar y que solo caen cuando quedan desconectados del techo.
- **Soporte multiplataforma**: teclado, ratón y pantallas táctiles (móviles y tablets).
- **Interfaz en español** con HUD de puntuación, récord, vidas, nivel, burbujas restantes y poderes activos.
- **Persistencia**: guarda el récord y el nivel máximo alcanzado en `localStorage`.
- **Integración con rankings externos**: emite eventos `postMessage` hacia aplicaciones contenedoras (`iframe`) al terminar la partida.

---

## 🎮 Controles

| Acción | Control |
|---|---|
| **Apuntar** | Movimiento del ratón, arrastre táctil o flechas `←` / `→` (también `A` / `D`) |
| **Disparar** | Clic izquierdo, toque o `ESPACIO` |
| **Empezar** | Clic, toque o `ESPACIO` en la pantalla de inicio |
| **Pausa** | `P` o `ESC` |
| **Continuar (tras Game Over)** | `C` |
| **Reiniciar** | `R` |
| **Reiniciar nivel** | Botón **REINICIAR NIVEL** |

En **Game Over**, `C` continúa en el mismo nivel con **3 vidas** y la puntuación acumulada. En la pantalla de **victoria**, `R` o `ESPACIO` empiezan una partida nueva.

---

## 🧱 Niveles

| Nivel | Nombre | Contenido | Filas | Burbujas | Poderes | Bajada del techo |
|---|---|---|---|---|---|---|
| 1 | Primer burbujeo | Tablero de bandas de colores | 4 | 77 | 4 | Cada 8 disparos |
| 2 | Espiral | Formación en espiral con 5 colores | 5 | 93 | 5 | Cada 7 disparos |
| 3 | Fortaleza | Estructura con **7 piedras** | 5 | 100 (incl. 7 piedras) | 6 | Cada 6 disparos |
| 4 | DOH, la burbuja madre | Jefe final + tablero con corredor central | 5 | 60 + DOH | 4 | Cada 6 disparos |

- Los niveles y la posición de los poderes son **deterministas**: usan una semilla fija por nivel.
- Cada nivel utiliza una paleta de colores propia.
- En el **Nivel 4** el tablero baja su origen para dejar sitio a DOH y las nuevas filas del techo respetan un **corredor central** para que la burbuja pueda alcanzar al jefe.

---

## 🎯 Reglas y puntuación

- Forma un grupo de **3 o más burbujas del mismo color** para reventarlas.
- Las burbujas (y las piedras) que quedan **desconectadas del techo** caen y otorgan puntos extra.
- Tras cada disparo resuelto, el contador de bajada disminuye. Al llegar a cero, desciende una **nueva fila** desde el techo y el contador se reinicia.
- Si la burbuja más baja cruza la **línea de peligro** (`y = 500`), pierdes una vida. El tablero se alivia eliminando las filas inferiores hasta dejar la pila **dos filas por encima** de la línea, los bloques desconectados caen y los poderes activos se cancelan.
- La partida comienza con **3 vidas**. Al llegar a 0 se muestra **Game Over**.
- En el jefe, los impactos directos y los grupos también restan integridad a DOH.

| Acción | Puntos |
|---|---|
| Burbuja reventada en un grupo | **10** por burbuja |
| Burbujas extra del grupo (a partir de la cuarta) | **+15** por burbuja |
| Burbuja que cae desconectada | **20** |
| Piedra que cae desconectada | **25** |
| Burbuja de poder activada | **+25** |
| Objetivo alcanzado por la bomba | **15** |
| Daño a DOH | **50** por punto de daño |
| Nivel superado | **+250** |
| Victoria final | **+1000** |

Ejemplos de grupo: 3 burbujas = **30** puntos; 4 = **55**; 5 = **80**; 6 = **105**.

---

## ✨ Burbujas de poder

Los poderes están **integrados en el tablero** como burbujas especiales. Se activan al reventar la burbuja que los contiene y se muestran con un icono y un color propios.

| Icono | Poder | Efecto | Duración |
|---|---|---|---|
| **B** | Bomba | El **siguiente disparo** explota en un radio de 4 radios de burbuja (~72 px) y revienta las burbujas que alcance (las piedras no se destruyen con la explosión; si quedan desconectadas, caen) | Un disparo |
| **A** | Arcoíris | El disparo se vuelve **comodín**: coincide con cualquier color | 12 s |
| **G** | Guía láser | Muestra la **trayectoria completa con rebotes** hasta el punto de impacto | 12 s |
| **C** | Techo congelado | Impide que desciendan nuevas filas del techo | 10 s |

- El HUD muestra los poderes activos y los segundos restantes.
- La burbuja actual se dibuja como arcoíris cuando el poder está activo.
- La bomba no se acumula: el siguiente disparo la consume.

---

## 👑 Jefe final: DOH

En el Nivel 4 aparece **DOH**, una burbuja madre con **30 puntos de integridad** y un tablero de 60 burbujas a su alrededor.

- **Impacto de color correcto** (o disparo arcoíris): **4 de daño**.
- **Impacto de color incorrecto**: **1 de daño**.
- **Bomba**: **5 de daño**, independientemente del color.
- **Grupo reventado**: **1 + ⌊(tamaño − 3) / 3⌋** de daño.
- DOH **cambia de color cada 6 disparos resueltos**, mostrando un aviso en pantalla.
- Al llegar a 0 de integridad comienza una secuencia de explosiones y la **pantalla de victoria**.

---

## 📡 Integración en apps contenedoras (`postMessage`)

Al terminar una partida (**Game Over** o **Victoria**), el juego emite un mensaje a la ventana padre con la puntuación final. Solo se envía **una vez por partida**; al continuar o empezar de nuevo se vuelve a habilitar.

### 1. Incrustación en HTML

```html
<iframe
  src="Puzzle-bobble/index.html"
  width="800"
  height="700"
  frameborder="0"
  allow="autoplay">
</iframe>
```

### 2. Captura del evento en la aplicación contenedora

```javascript
window.addEventListener('message', (event) => {
  if (event.data?.type === 'PUZZLE_BOBBLE_SCORE_SUBMISSION') {
    const { game, score, level, highScore } = event.data;
    console.log(`Puntuación recibida de ${game}: ${score} (nivel ${level})`);

    // Ejemplo: registrar la puntuación en el ranking de la app
    guardarPuntuacionRanking(score);
  }
});
```

### 3. Esquema del payload

| Propiedad | Tipo | Descripción |
|---|---|---|
| `type` | `string` | Identificador del evento: `'PUZZLE_BOBBLE_SCORE_SUBMISSION'` |
| `game` | `string` | Identificador del juego: `'puzzle-bobble'` |
| `score` | `number` | Puntuación final de la partida |
| `level` | `number` | Nivel alcanzado (1–4) |
| `highScore` | `number` | Récord local del jugador |

---

## 💾 Persistencia

El juego guarda de forma local (sin servidor ni cuentas):

| Clave | Contenido |
|---|---|
| `pb_high` | Récord de puntuación |
| `pb_maxlevel` | Nivel máximo alcanzado |

---

## 🛠️ Cómo ejecutar en local

Abre el archivo `index.html` en cualquier navegador web moderno:

```bash
open index.html # En macOS
```

O haz doble clic sobre el archivo `index.html`.

También puedes servirlo con cualquier servidor estático, por ejemplo:

```bash
npx serve .
```

---

## 🧠 Detalles técnicos

- **Bucle principal**: acumulador de tiempo con `requestAnimationFrame` y actualizaciones a 60 Hz, con un máximo de 5 pasos por fotograma para evitar saltos tras una pausa.
- **Rejilla hexagonal**: cada fila guarda su paridad; las filas pares tienen 22 celdas y las impares 21. El desplazamiento horizontal de las filas impares es de medio diámetro.
- **Disparo**: velocidad de 13 px por fotograma, rebote en las paredes y cuatro sub-pasos por fotograma para una detección de colisión fiable.
- **Conexión y caída**: búsqueda en anchura (BFS) para localizar el grupo del mismo color y recorrido desde la fila superior para detectar bloques desconectados que deben caer.
- **Ajuste de impacto**: la burbuja se coloca en la celda vacía válida más cercana al punto de impacto (con al menos una vecina ocupada); si no se encuentra ninguna, se usa la celda vacía más cercana.
- **Bajada del techo**: se inserta una fila nueva en la parte superior, se alterna la paridad y se desplazan las filas existentes; en el jefe se omite el corredor central.
- **Alivio por desbordamiento**: elimina filas inferiores hasta que la pila quede dos filas por encima de la línea de peligro, después recalcula los bloques desconectados y reinicia los poderes.
- **Render**: burbujas con degradados radiales precalculados en sprites, brillos, sombras, chispas, estrellas, burbujas ambientales y sacudida de pantalla.
- **Audio**: osciladores, barridos de frecuencia, ruido filtrado y secuencias de notas generadas al vuelo; se desbloquea con la primera interacción del usuario.
- **Entrada**: ratón, táctil y teclado; se normaliza `event.key` cuando el navegador no proporciona `event.code`, lo que mejora la compatibilidad con teclados virtuales y automatización.
- **Accesibilidad**: el canvas es enfocable y tiene una etiqueta ARIA; el HUD usa texto real para puntuación, vidas y nivel.

---

## 📁 Estructura

```
Puzzle-bobble/
├── index.html   # Juego completo (HTML5 + Canvas 2D + Web Audio API)
└── README.md    # Este documento
```

---

Hecho como homenaje fan a la saga **Puzzle Bobble / Bust-a-Move**. No está afiliado a Taito.
