# Asteroids Arcade

Un remake arcade de **Asteroids** construido con HTML5, CSS3 y JavaScript puro (Canvas 2D), con audio procedural generado en tiempo real mediante la Web Audio API y soporte para aplicaciones contenedoras mediante `postMessage`.

El juego conserva la esencia del clásico —nave inercial, pantalla envolvente, asteroides que se fragmentan y OVNIs hostiles— y la amplía con **4 niveles, power-ups, jefe final, vidas, récords y pantalla de victoria**.

---

## 🚀 Características

- **Sin dependencias externas**: todo el juego vive en un único `index.html` que funciona en cualquier navegador moderno.
- **Audio procedural**: disparos, impactos, explosiones, propulsión, power-ups y fanfarria generados con osciladores, ruido y filtros de la Web Audio API, sin archivos de sonido.
- **Motor de paso fijo a 60 Hz**, independiente de la tasa de refresco del monitor.
- **Sub-stepping de proyectiles** para que las balas rápidas no atraviesen asteroides ni al jefe.
- **Mundo envolvente**: nave, asteroides y disparos reaparecen por el lado opuesto de la pantalla; las colisiones también consideran ese «wrapping».
- **Asteroides procedurales**: cada roca tiene una silueta irregular, rotación y trayectoria propias; las grandes se dividen en medianas y estas en pequeñas.
- **Efectos visuales**: campo de estrellas parpadeante, nebulosa, estelas de partículas, anillos de explosión, resplandores neón y sacudida de pantalla.
- **Controles de teclado, ratón y táctiles**: giro y propulsión con teclado, apuntado con ratón o arrastre, y botones táctiles dedicados.
- **Persistencia local** del récord y del nivel máximo alcanzado.
- **Integración con apps contenedoras** mediante `postMessage` al terminar la partida.

---

## 🎮 Controles

### Teclado

| Tecla | Acción |
|---|---|
| `←` / `A` | Girar a la izquierda |
| `→` / `D` | Girar a la derecha |
| `↑` / `W` | Propulsar |
| `Espacio` | Disparar (mantener para fuego continuo) |
| `P` / `Esc` | Pausar / continuar |
| `C` | Continuar desde el nivel actual tras perder todas las vidas |
| `R` | Reiniciar la partida desde el nivel 1 |

### Ratón

- Mueve el cursor sobre el canvas para orientar la nave hacia él.
- Mantén pulsado el botón izquierdo para disparar.
- Haz clic en las pantallas de inicio, fin o victoria para avanzar.

### Táctil

- **Arrastra** sobre el canvas para apuntar la nave.
- Usa los botones inferiores: `⟲` girar a la izquierda, `▲` propulsar, `●` disparar y `⟳` girar a la derecha.
- Toca la pantalla de inicio para comenzar.

---

## 🕹️ Mecánicas principales

### Nave y física

La nave no se detiene sola: acelera al propulsar y conserva la inercia. La velocidad está limitada y se aplica una resistencia suave para evitar que el control se vuelva inmanejable. Al recibir un impacto se pierde una vida, se reinicia la nave en la zona inferior y se concede un breve periodo de invulnerabilidad.

### Asteroides

| Tamaño | Radio aproximado | Puntos | Al destruirse |
|---|---:|---:|---|
| Grande | 42 px | 20 | Se divide en 2 medianos |
| Mediano | 26 px | 50 | Se divide en 2 pequeños |
| Pequeño | 14 px | 100 | Desaparece |

Todos los asteroides se mueven en línea recta con rebote envolvente. Algunos transportan una cápsula de power-up que cae lentamente y puede recogerse con la nave.

### OVNIs

Aparecen en los niveles 2 y 3, cruzan la pantalla con un movimiento ondulante y disparan proyectiles dirigidos al jugador. Tienen 2 puntos de resistencia, otorgan **150 puntos** y pueden soltar una cápsula al ser destruidos.

### Power-ups

| Power-up | Icono | Efecto | Duración |
|---|---|---|---:|
| Disparo triple | `3` | Tres balas en abanico por cada disparo | 10 s |
| Escudo | `S` | Absorbe un impacto; el escudo se desactiva al usarlo | 15 s |
| Cadencia | `R` | Reduce el enfriamiento de disparo a 4 fotogramas | 10 s |
| Láser | `L` | Balas perforantes y con el doble de daño | 10 s |

Recoger una cápsula otorga **25 puntos**. Los power-ups aparecen en asteroides seleccionados de cada nivel, en OVNIs y en el jefe final.

### Vidas y progresión

- La partida comienza con **3 vidas**.
- Al perder una vida se conserva la puntuación y se reinicia la nave con invulnerabilidad temporal.
- Al quedarse sin vidas aparece **FIN DE LA PARTIDA**; `C` permite continuar desde el nivel actual con 3 vidas nuevas.
- Completar un nivel otorga **250 puntos** y un breve interludio antes de cargar el siguiente.

---

## 🌌 Niveles

### Nivel 1 — Cinturón exterior
4 asteroides grandes, sin OVNIs. Un nivel de calentamiento para dominar la inercia, el giro y el apuntado.

### Nivel 2 — Lluvia de escombros
6 asteroides grandes, 2 medianos y aparición periódica de un OVNI. El campo se llena rápidamente de fragmentos.

### Nivel 3 — Zona de guerra
7 grandes, 4 medianos, 2 pequeños y hasta 2 OVNIs simultáneos. Es el nivel más denso antes del enfrentamiento final.

### Nivel 4 — DOH-9, el núcleo planetario
El jefe final es un asteroide colosal con un núcleo de energía. Tiene **40 puntos de vida** y se mueve lateralmente por la zona superior.

- Dispara ráfagas radiales de proyectiles; al bajar del 50 % de vida se enfurece, dispara más proyectiles y con mayor frecuencia.
- Genera pequeños asteroides de apoyo mientras siga vivo.
- Suelta cápsulas de power-up periódicamente.
- Cada impacto otorgado por una bala vale **50 puntos**.
- Al destruirlo se obtienen **1000 puntos** adicionales y se muestra la pantalla de victoria.

---

## 🏆 Puntuación

| Acción | Puntos |
|---|---:|
| Asteroide grande | 20 |
| Asteroide mediano | 50 |
| Asteroide pequeño | 100 |
| OVNI | 150 |
| Impacto al jefe | 50 por punto de daño |
| Recoger power-up | 25 |
| Completar nivel | 250 |
| Destruir a DOH-9 | 1000 |

El récord y el nivel máximo se guardan en `localStorage` con las claves `as_high` y `as_maxlevel`. El HUD muestra siempre la mejor puntuación, el nivel máximo y las vidas restantes.

---

## 🔌 Integración con una app contenedora

Al terminar la partida (derrota o victoria) el juego envía un mensaje a la ventana padre:

```js
window.parent.postMessage({
  type: 'ASTEROIDS_SCORE_SUBMISSION',
  game: 'asteroids',
  score: 12450,
  level: 4,
  highScore: 18400
}, '*');
```

Ejemplo de escucha en la aplicación contenedora:

```js
window.addEventListener('message', (event) => {
  if (event.data && event.data.type === 'ASTEROIDS_SCORE_SUBMISSION') {
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
- **Bucle**: `requestAnimationFrame` con acumulador y paso fijo de `1000 / 60` ms.
- **Audio**: Web Audio API; el contexto se crea o reanuda con la primera interacción del usuario.
- **Persistencia**: `localStorage` para récord y nivel máximo.
- **Sin frameworks, sin dependencias, sin imágenes ni audio externos**.
- **Controles adaptativos**: los botones táctiles solo se muestran en dispositivos sin hover o pantallas estrechas.

---

## ▶️ Cómo jugar

Abre directamente `Asteroids/index.html` en el navegador o sirve el repositorio con cualquier servidor estático:

```bash
# Desde la raíz del repositorio
python -m http.server 8000
```

Después visita:

```text
http://localhost:8000/Asteroids/
```

También puedes abrir el archivo con doble clic; el juego no necesita red ni recursos externos.

---

## 📁 Estructura

```text
Asteroids/
├── index.html   # Juego completo: HTML, CSS, Canvas, audio y lógica
└── README.md    # Este documento
```
