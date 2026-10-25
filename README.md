# 🕹️ Juegos Retro

Colección de juegos arcade clásicos reimplementados desde cero con **HTML5, CSS3 y JavaScript puro** (Canvas 2D), sin dependencias externas ni frameworks.

## 🌐 Demo en GitHub Pages

**▶️ [Abrir la colección en GitHub Pages](https://dianri.github.io/juegos-retro/)**

La portada principal ([`index.html`](index.html)) reúne todos los juegos y enlaza directamente con cada uno. GitHub Pages publica la rama `main` desde la raíz del repositorio.

## 🎮 Juegos

| Juego | Carpeta | Estado |
|---|---|---|
| Arkanoid | [`Arkanoid/`](Arkanoid/) | ✅ Disponible |
| Asteroids | [`Asteroids/`](Asteroids/) | ✅ Disponible |
| Pac-Man | [`Pac-Man/`](Pac-Man/) | ✅ Disponible |
| Pong | [`Pong/`](Pong/) | ✅ Disponible |
| Puzzle Bobble | [`Puzzle-bobble/`](Puzzle-bobble/) | ✅ Disponible |
| Space Invaders | [`Space-Invaders/`](Space-Invaders/) | ✅ Disponible |

## 📁 Estructura

```
juegos-retro/
├── index.html         # Portada de la colección para GitHub Pages
├── Arkanoid/          # Juego completo (HTML5 + Canvas 2D)
├── Asteroids/         # Juego completo (HTML5 + Canvas 2D)
├── Pac-Man/           # Juego completo (HTML5 + Canvas 2D)
├── Pong/              # Juego completo (HTML5 + Canvas 2D)
├── Puzzle-bobble/     # Juego completo (HTML5 + Canvas 2D)
└── Space-Invaders/    # Juego completo (HTML5 + Canvas 2D)
```

Cada juego es autónomo: se ejecuta abriendo su `index.html` en cualquier navegador moderno, sin instalación ni servidor.

## 🎯 Objetivo

Reunir en un solo lugar los distintos juegos retro del proyecto, manteniendo cada uno aislado en su carpeta para poder desarrollarlos y desplegarlos por separado (por ejemplo, con GitHub Pages).
