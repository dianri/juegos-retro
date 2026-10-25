# Pong Arcade

Un clon arcade retro-futurista de **Pong** desarrollado en HTML5, CSS3 y JavaScript puro (Canvas 2D) con audio procedural generado mediante la Web Audio API y soporte para integración en aplicaciones contenedor mediante `postMessage`.

El juego mantiene la esencia del Pong clásico (dos palas, una bola, saques y sets) y le añade la progresión de un arcade moderno: **4 niveles, power-ups, bloques destructibles y un jefe final**.

---

## 🚀 Características

- **Sin dependencias externas**: todo el juego vive en un único `index.html` ejecutable directamente en el navegador.
- **Audio procedural**: efectos de sonido y fanfarrias generados en tiempo real mediante osciladores Web Audio API, sin archivos de audio.
- **Física sólida y calibrada**:
  - Motor con paso de tiempo fijo (*Fixed Timestep* a 60 Hz), independiente de la tasa de refresco del monitor.
  - Sub-stepping (4 pasos por frame) para evitar el efecto túnel.
  - Rebote angular en ambas palas según el punto de impacto y la inercia del movimiento.
  - Detección precisa de caras y esquinas en los bloques.
  - Mecanismo anti-bloqueo automático para evitar rallies infinitos.
- **Estética retro-neón**: palas y bloques luminosos con halo, bloques metálicos blindados con remaches, línea central de pista, partículas de impacto y estelas de bola.
- **Power-ups deterministas (PRNG Mulberry32)**: cada nivel reparte siempre los mismos ítems.
  - ✦ **Multibola**: triplica las bolas en juego.
  - ↕ **Pala ancha**: incrementa el tamaño de la pala.
  - ⚡ **Láser**: dispara proyectiles que destruyen bloques y aturden a la CPU.
  - 🔥 **Bola de fuego**: atraviesa los bloques destructibles sin rebotar y daña el doble al jefe.
- **4 niveles progresivos** (sets a 3, 4 y 5 puntos + combate final):
  1. *Red Central*: toma de contacto, primera red de bloques y CPU lenta.
  2. *Fortaleza*: bloques reforzados de 2 golpes y acero blindado indestructible.
  3. *Laberinto*: red densa, CPU rápida y precisión exigente.
  4. *Enfrentamiento Final*: combate contra el núcleo **DOH**, con barra de vida, disparos defensivos, señuelos y secuencia de explosión cinemática.
- **Soporte multiplataforma**: teclado, ratón y pantallas táctiles (móviles y tablets).
- **Persistencia**: guarda el récord de puntuación y el nivel máximo en `localStorage`.
- **Integración con rankings externos**: emite eventos `postMessage` hacia aplicaciones contenedoras (`iframe`) al finalizar la partida.

---

## 🎮 Controles

| Acción | Control |
|---|---|
| **Mover pala** | Flechas `↑` / `↓`, `W` / `S`, ratón o deslizamiento táctil |
| **Sacar / Disparar láser** | `ESPACIO`, clic izquierdo o pulsación táctil |
| **Pausa** | `P` o `ESC` |
| **Continuar nivel (tras Game Over)** | `C` |
| **Reiniciar partida desde el Nivel 1** | `R` |

---

## 🧱 Niveles y marcador

Cada nivel es un **set**: gana quien llegue primero a la puntuación objetivo (3, 4 o 5 puntos).

- La bola que supera la pala de la CPU suma **1 punto** y **+100** de puntuación.
- La bola que supera tu pala suma un punto para la CPU.
- Si la CPU gana el set, **pierdes una vida** y el marcador vuelve a 0-0.
- Los bloques del centro otorgan puntos extra y pueden soltar power-ups.

En el **Nivel 4** no hay sets: debes destruir el núcleo DOH (26 impactos) mientras esquivas sus proyectiles. Cada bola perdida cuesta una vida.

| Nivel | Nombre | Objetivo |
|---|---|---|
| 1 | Red Central | Set a 3 puntos |
| 2 | Fortaleza | Set a 4 puntos |
| 3 | Laberinto | Set a 5 puntos |
| 4 | Enfrentamiento Final | Derrotar a DOH |

---

## ✨ Power-ups

| Icono | Nombre | Efecto | Duración |
|---|---|---|---|
| ✦ | Multibola | Añade 2 bolas con ángulos distintos | Instantáneo |
| ↕ | Pala ancha | Pala un 55 % más grande | 10 s |
| ⚡ | Láser | Dos haces que destruyen bloques y aturden a la CPU | 10 s |
| 🔥 | Bola de fuego | Atraviesa bloques y daña al jefe x2 | 10 s |

Las cápsulas caen al romper ciertos bloques y planean hacia tu pala. Si no las atrapas, se pierden.

---

## 📡 Integración en Apps Contenedoras (`postMessage`)

Al finalizar la partida (**Game Over** o **Victoria**), el juego emite un mensaje a la ventana padre con la puntuación final:

### 1. Incrustación en HTML
```html
<iframe
  src="Pong/index.html"
  width="860"
  height="740"
  frameborder="0"
  allow="autoplay">
</iframe>
```

### 2. Captura del evento en la aplicación contenedora
```javascript
window.addEventListener('message', (event) => {
  if (event.data?.type === 'PONG_SCORE_SUBMISSION') {
    const { score, game } = event.data;
    console.log(`Puntuación recibida de ${game}: ${score}`);

    // Ejemplo: registrar puntuación en el ranking de la app
    guardarPuntuacionRanking(score);
  }
});
```

### 3. Esquema del payload
| Propiedad | Tipo | Descripción |
|---|---|---|
| `type` | `string` | Identificador del evento: `'PONG_SCORE_SUBMISSION'` |
| `score` | `number` | Puntuación final obtenida por el jugador |
| `game` | `string` | Identificador del juego: `'pong'` |

---

## 🛠️ Cómo ejecutar en local

Abre el archivo `index.html` en cualquier navegador web moderno:

```bash
open index.html # En macOS
```

O haz doble clic sobre el archivo `index.html`.
