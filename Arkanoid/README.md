# Arkanoid Arcade

> 🕹️ **Juega online ahora mismo**: **[https://dianri.github.io/Arkanoid_Game/](https://dianri.github.io/Arkanoid_Game/)**

Un clon arcade retro-futurista de **Arkanoid** desarrollado en HTML5, CSS3 y JavaScript puro (Canvas 2D) con audio procedural generado mediante la Web Audio API y soporte para integración en aplicaciones contenedor mediante `postMessage`.

---

## 🌐 Jugar online

Puedes jugar directamente desde tu navegador en ordenador, móvil o tablet sin instalar nada:  
👉 **[https://dianri.github.io/Arkanoid_Game/](https://dianri.github.io/Arkanoid_Game/)**

---

## 🚀 Características

- **Sin dependencias externas**: Código completamente autónomo en un archivo ejecutable directamente en el navegador.
- **Audio procedural**: Efectos de sonido y fanfarrias generados en tiempo real mediante osciladores Web Audio API, sin necesidad de archivos de audio externos.
- **Física sólida y calibrada**:
  - Motor de física con paso de tiempo fijo (*Fixed Timestep* a 60 Hz) independiente de la tasa de refresco del monitor (compatible con pantallas de 60 Hz, 120 Hz ProMotion, etc.).
  - Sub-stepping (4 pasos por frame) para evitar el efecto túnel.
  - Rebote angular en la pala en función del punto de impacto y la inercia del movimiento.
  - Detección precisa de caras y esquinas en ladrillos.
  - Mecanismo anti-bloqueo automático.
- **Estética retro-neón**: Bloques luminosos con halo de resplandor, bloques metálicos blindados con remaches y partículas de impacto con estela.
- **Power-ups deterministas (PRNG Mulberry32)**:
  - ✦ **Multibola**: Triplica las bolas en juego.
  - ↔ **Pala ancha**: Incrementa la superficie de control de la pala.
  - ⚡ **Láser**: Permite disparar proyectiles a los bloques.
  - 🔥 **Bola de fuego**: Atraviesa los bloques destructibles sin rebotar.
- **4 Niveles progresivos**:
  1. *Muralla*: Fase introductoria con bloques estándar.
  2. *Fortaleza*: Incorpora bloques reforzados de 2 golpes y bloques de titanio indestructibles.
  3. *Laberinto Dorado*: Distribución laberíntica compleja.
  4. *Enfrentamiento Final*: Combate contra el jefe nodriza **DOH**, con barra de salud, disparos defensivos, señuelos y secuencia de explosión cinemática con partículas y ondas de choque.
- **Soporte multiplataforma**: Controles para teclado, ratón y pantallas táctiles (móviles y tablets).
- **Persistencia**: Guarda récord de puntuación y nivel máximo en `localStorage`.
- **Integración con rankings externos**: Emite eventos `postMessage` hacia aplicaciones contenedoras (`iframe`) al finalizar la partida.

---

## 🎮 Controles

| Acción | Control |
|---|---|
| **Mover pala** | Flechas `←` / `→`, ratón o deslizamiento táctil |
| **Lanzar bola / Disparar láser** | `ESPACIO`, clic izquierdo o pulsación táctil |
| **Pausa** | `P` o `ESC` |
| **Continuar nivel (tras Game Over)** | `C` |
| **Reiniciar partida desde el Nivel 1** | `R` |

---

## 📡 Integración en Apps Contenedoras (`postMessage`)

El juego está preparado para incrustarse en aplicaciones web externas mediante un `<iframe>`. Al finalizar la partida (**Game Over** o **Victoria**), el juego emite un mensaje a la ventana padre con la puntuación final:

### 1. Incrustación en HTML
```html
<iframe 
  src="https://dianri.github.io/Arkanoid_Game/" 
  width="860" 
  height="740" 
  frameborder="0"
  allow="autoplay">
</iframe>
```

### 2. Captura del evento en la aplicación contenedora
```javascript
window.addEventListener('message', (event) => {
  if (event.data?.type === 'ARKANOID_SCORE_SUBMISSION') {
    const { score, game } = event.data;
    console.log(`Puntuación recibida de ${game}: ${score}`);
    
    // Ejemplo: registrar puntuación en el ranking de Qué Comemos Hoy
    guardarPuntuacionRanking(score);
  }
});
```

### 3. Esquema del Payload
| Propiedad | Tipo | Descripción |
|---|---|---|
| `type` | `string` | Identificador del evento: `'ARKANOID_SCORE_SUBMISSION'` |
| `score` | `number` | Puntuación final obtenida por el jugador |
| `game` | `string` | Identificador del juego: `'arkanoid'` |

---

## 🛠️ Cómo ejecutar en local

Si prefieres jugar o desarrollar en local, abre el archivo `index.html` en cualquier navegador web moderno:

```bash
open index.html # En macOS
```
O haz doble clic sobre el archivo `index.html`.
