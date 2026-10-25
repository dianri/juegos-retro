# Space Invaders Arcade

Un clon arcade retro-futurista de **Space Invaders** desarrollado en HTML5, CSS3 y JavaScript puro (Canvas 2D), con audio procedural generado mediante la Web Audio API y soporte para integración en aplicaciones contenedor mediante `postMessage`.

El juego conserva la esencia del clásico (naves invasoras en formación, búnkeres destructibles y disparos descendentes) y la combina con la progresión de un arcade moderno: **3 oleadas, power-ups, jefe final, vidas, récords y pantalla de victoria**.

---

## 🚀 Características

- **Sin dependencias externas**: todo el juego vive en un único `index.html` ejecutable en cualquier navegador moderno.
- **Audio procedural**: disparos, impactos, explosiones, oleadas y fanfarria generados en tiempo real con osciladores y ruido de la Web Audio API, sin archivos de audio.
- **Motor con paso de tiempo fijo (60 Hz)**, independiente de la tasa de refresco del monitor.
- **Sub-stepping en proyectiles** para evitar el efecto túnel contra búnkeres y naves.
- **Sprites pixel-art dibujados en canvas**: tres tipos de invasor (11 × 8 píxeles) con animación de patas y una nave nodriza final construida con primitivas luminosas.
- **Formación clásica**: la flota se desplaza en bloque, rebota en los bordes y desciende un escalón cada vez; la velocidad aumenta a medida que quedan menos invasores.
- **Búnkeres destructibles**: 4 refugios de 13 bloques cada uno que protegen al jugador y que pueden destruir tanto los disparos enemigos como los tuyos.
- **Power-ups deterministas (PRNG Mulberry32)**: cada oleada reparte siempre los mismos ítems entre sus invasores.
- **4 niveles progresivos** (3 oleadas + combate final contra la nave madre **DOH**).
- **Soporte multiplataforma**: teclado, ratón y pantallas táctiles (móviles y tablets).
- **Persistencia**: guarda el récord de puntuación y el nivel máximo alcanzado en `localStorage`.
- **Integración con rankings externos**: emite eventos `postMessage` hacia aplicaciones contenedoras (`iframe`) al finalizar la partida.

---

## 🎮 Controles

| Acción | Control |
|---|---|
| **Mover la nave** | Flechas `←` / `→`, `A` / `D`, ratón o deslizamiento táctil |
| **Disparar** | Mantén `ESPACIO`, el clic izquierdo o la pulsación táctil |
| **Empezar / despegar** | `ESPACIO`, clic o toque |
| **Pausa** | `P` o `ESC` |
| **Continuar nivel (tras Game Over)** | `C` |
| **Reiniciar desde el nivel 1** | `R` |

El botón **REINICIAR NIVEL** vuelve a empezar la oleada actual conservando la puntuación. En la pantalla de victoria se convierte en **JUGAR DE NUEVO**.

---

## 🧱 Niveles y puntuación

| Nivel | Nombre | Contenido |
|---|---|---|
| 1 | Primer contacto | 32 invasores (8 × 4), 1 impacto cada uno |
| 2 | Enjambre | 32 invasores; las dos filas superiores resisten 2 impactos |
| 3 | Asalto | 40 invasores (8 × 5); las filas altas resisten 2 impactos |
| 4 | Nave madre | Combate contra **DOH**, 45 puntos de integridad |

- Cada invasor destruido otorga **30 / 20 / 20 / 10 / 10** puntos según su fila.
- Cada impacto sobre la nave madre suma **50 puntos** (con láser, **100**).
- Superar una oleada concede **+250 puntos**.
- Si la formación alcanza la **línea de defensa**, pierdes una vida y la flota vuelve a la parte superior conservando las naves ya destruidas.
- Recibir un disparo también cuesta una vida. La nave reaparece en el centro con 2 segundos de invulnerabilidad y se pierden los power-ups activos.
- La partida comienza con **3 vidas**.

En el **Nivel 4** no hay formación: debes destruir la nave nodriza **DOH** mientras esquivas sus ráfagas de proyectiles. La nave nodriza se mueve más rápido y dispara abanicos de 5 proyectiles cuando está por debajo del 50 % de integridad. Al morir, se desencadena una secuencia de explosiones y la pantalla de victoria.

---

## ✨ Power-ups

| Icono | Nombre | Efecto | Duración |
|---|---|---|---|
| ≡ | Disparo triple | Dispara tres proyectiles en abanico | 10 s |
| ⚡ | Láser | Proyectiles perforantes que atraviesan invasores y dañan al jefe x2 | 10 s |
| ◉ | Escudo | Absorbe un impacto enemigo (disparo o invasión) | 12 s o hasta recibir un golpe |
| » | Cadencia rápida | Reduce el enfriamiento de disparo de 13 a 7 fotogramas y permite más proyectiles en pantalla | 10 s |

Las cápsulas caen desde los invasores destruidos con un movimiento ondulante. Si no las atrapas con la nave, se pierden. Durante el combate contra DOH aparecen cápsulas de apoyo periódicamente.

---

## 📡 Integración en Apps Contenedoras (`postMessage`)

Al finalizar la partida (**Game Over** o **Victoria**), el juego emite un mensaje a la ventana padre con la puntuación final:

### 1. Incrustación en HTML
```html
<iframe
  src="Space-Invaders/index.html"
  width="860"
  height="740"
  frameborder="0"
  allow="autoplay">
</iframe>
```

### 2. Captura del evento en la aplicación contenedora
```javascript
window.addEventListener('message', (event) => {
  if (event.data?.type === 'SPACE_INVADERS_SCORE_SUBMISSION') {
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
| `type` | `string` | Identificador del evento: `'SPACE_INVADERS_SCORE_SUBMISSION'` |
| `score` | `number` | Puntuación final obtenida por el jugador |
| `game` | `string` | Identificador del juego: `'space-invaders'` |

---

## 💾 Persistencia

El juego guarda de forma local (sin servidor ni cuentas):

| Clave | Contenido |
|---|---|
| `si_high` | Récord de puntuación |
| `si_maxlevel` | Nivel máximo alcanzado |

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

- **Bucle principal**: acumulador de tiempo con `requestAnimationFrame` y actualizaciones a 60 Hz.
- **Colisiones**: AABB para proyectiles, naves y búnkeres, con sub-pasos para proyectiles rápidos.
- **Formación**: offsets de rejilla (`col`, `row`) y un origen compartido que se desplaza, rebota y desciende.
- **Aleatoriedad reproducible**: PRNG Mulberry32 con semilla por nivel para repartir los power-ups.
- **Render**: estrellas con parpadeo, efectos de resplandor (`shadowBlur`), partículas, anillos expansivos y sacudida de pantalla.
- **Audio**: osciladores, barridos de frecuencia, ruido filtrado y secuencias de notas generadas al vuelo.
