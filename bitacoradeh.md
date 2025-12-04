# 🍦 Bitácora de Helados - Documento de Diseño UX/UI (MVP)

---

## 📋 Índice

1. [Resumen Ejecutivo](#resumen-ejecutivo)
2. [Persona y Contexto](#persona-y-contexto)
3. [Arquitectura de Información](#arquitectura-de-información)
4. [Mapa de Flujos](#mapa-de-flujos)
5. [Wireframes de Baja Fidelidad](#wireframes-de-baja-fidelidad)
6. [Sistema de Diseño](#sistema-de-diseño)
7. [Especificaciones por Pantalla](#especificaciones-por-pantalla)
8. [Microinteracciones](#microinteracciones)

---

## 🎯 Resumen Ejecutivo

### Propuesta de Valor
> *"Nunca más olvides cuál fue ese gusto increíble"*

### Principios de Diseño

| Principio | Descripción |
|-----------|-------------|
| **One-Thumb Interaction** | Todo accesible con el pulgar, diseño para uso con una mano |
| **Speed First** | Máximo 3 toques para calificar un helado |
| **Visual > Texto** | Iconografía clara, mínima lectura requerida |
| **Contexto Inteligente** | La app sabe dónde estás y qué sugerirte |

---

## 👤 Persona y Contexto

### Usuario Principal: "El Heladero Porteño"

```
┌─────────────────────────────────────────────────────────┐
│  MARTÍN, 32 años                                        │
│  Buenos Aires, Argentina                                │
│                                                         │
│  🎯 Motivación:                                         │
│     "Siempre pido lo mismo por miedo a equivocarme,    │
│      pero quiero descubrir nuevos sabores"             │
│                                                         │
│  😤 Frustración:                                        │
│     "Probé un DDL increíble hace meses pero no         │
│      recuerdo dónde ni cómo se llamaba"                │
│                                                         │
│  📱 Contexto de uso:                                    │
│     • Parado en la fila de la heladería                │
│     • Una mano ocupada (cartera, niño, helado)         │
│     • Luz solar intensa en pantalla                    │
│     • Máximo 60 segundos de atención                   │
└─────────────────────────────────────────────────────────┘
```

---

## 🗂️ Arquitectura de Información

### Jerarquía de Entidades

```
                    ┌─────────────┐
                    │   USUARIO   │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │EXPERIENCIA│ │HELADERÍA │ │ SABORES  │
        │  (Visita) │ │ (Lugar)  │ │FAVORITOS │
        └────┬─────┘ └──────────┘ └──────────┘
             │
    ┌────────┼────────┬────────┐
    ▼        ▼        ▼        ▼
┌───────┐┌───────┐┌───────┐┌───────┐
│Sabor 1││Sabor 2││Sabor 3││Sabor 4│  (Máx 4 por experiencia)
│ ★★★★☆││ ★★★☆☆││ ★★★★★││ ★★☆☆☆│
│ Nota  ││ Nota  ││ Nota  ││ Nota  │
└───────┘└───────┘└───────┘└───────┘
```

### Taxonomía de Familias de Sabores

```
┌──────────────────────────────────────────────────────────────────┐
│                    FAMILIAS DE HELADO                            │
├──────────────┬──────────────┬──────────────┬────────────────────┤
│  🍫 CHOCOLATES│  🍯 DDL      │  🍓 FRUTALES │  🍦 CREMAS         │
├──────────────┼──────────────┼──────────────┼────────────────────┤
│ • Amargo     │ • Clásico    │ • Frutilla   │ • Americana        │
│ • Con Leche  │ • Granizado  │ • Maracuyá   │ • Vainilla         │
│ • Blanco     │ • Con Brownie│ • Limón      │ • Tiramisú         │
│ • Suizo      │ • Con Nuez   │ • Frambuesa  │ • Mascarpone       │
│ • Bitter     │ • Tentación  │ • Mango      │ • Cheesecake       │
│ • Con Almend.│ • Colonial   │ • Ananá      │ • Tramontana       │
└──────────────┴──────────────┴──────────────┴────────────────────┘

                    🌿 ESPECIALES
                    ├── Pistacho
                    ├── Menta Granizada
                    ├── Sambayón
                    └── Crema del Cielo
```

---

## 🗺️ Mapa de Flujos

### Flujo Principal: "Calificar Helado"

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐          │
│   │          │     │          │     │          │     │          │          │
│   │  SPLASH  │────▶│ ONBOARD  │────▶│  LOGIN   │────▶│   HOME   │          │
│   │          │     │          │     │  SOCIAL  │     │          │          │
│   └──────────┘     └──────────┘     └──────────┘     └────┬─────┘          │
│                                                           │                 │
│                                          ┌────────────────┘                 │
│                                          ▼                                  │
│                                    ┌──────────┐                             │
│                                    │ DETECTAR │                             │
│                                    │HELADERÍA │                             │
│                                    │   GPS    │                             │
│                                    └────┬─────┘                             │
│                          ┌──────────────┼──────────────┐                    │
│                          ▼              ▼              ▼                    │
│                    ┌──────────┐   ┌──────────┐   ┌──────────┐              │
│                    │CONFIRMAR │   │  BUSCAR  │   │  CREAR   │              │
│                    │SUGERENCIA│   │  MANUAL  │   │  NUEVA   │              │
│                    └────┬─────┘   └────┬─────┘   └────┬─────┘              │
│                         └──────────────┼──────────────┘                    │
│                                        ▼                                    │
│                                  ┌──────────┐                               │
│                                  │ AGREGAR  │◀─────────────┐               │
│                                  │ SABORES  │              │               │
│                                  └────┬─────┘              │               │
│                                       │                    │               │
│                    ┌──────────────────┼─────────┐          │               │
│                    ▼                  ▼         ▼          │               │
│              ┌──────────┐      ┌──────────┐ ┌──────────┐   │               │
│              │ FAMILIA  │      │ FAMILIA  │ │ FAMILIA  │   │               │
│              │CHOCOLATES│      │   DDL    │ │ FRUTALES │   │               │
│              └────┬─────┘      └────┬─────┘ └────┬─────┘   │               │
│                   └─────────────────┼────────────┘         │               │
│                                     ▼                      │               │
│                              ┌──────────┐                  │               │
│                              │ SELEC.   │──────────────────┘               │
│                              │ SABOR    │ (repetir hasta 4)                │
│                              └────┬─────┘                                  │
│                                   ▼                                        │
│                            ┌──────────┐                                    │
│                            │CALIFICAR │                                    │
│                            │  SABORES │                                    │
│                            │  ★★★★☆  │                                    │
│                            └────┬─────┘                                    │
│                                 ▼                                          │
│                           ┌──────────┐                                     │
│                           │ GUARDAR  │                                     │
│                           │EXPERIENC.│                                     │
│                           └────┬─────┘                                     │
│                                ▼                                           │
│                          ┌──────────┐                                      │
│                          │ SUCCESS  │                                      │
│                          │   🎉     │                                      │
│                          └──────────┘                                      │
│                                                                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Flujo Secundario: "Consultar Bitácora"

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   ┌──────────┐     ┌───────────────────────────────────────┐   │
│   │          │     │                                       │   │
│   │   HOME   │────▶│            MI BITÁCORA                │   │
│   │          │     │                                       │   │
│   └──────────┘     │  ┌─────────────┬─────────────────┐   │   │
│                    │  │ Por Heladería│  Por Familia    │   │   │
│                    │  └──────┬──────┴────────┬────────┘   │   │
│                    │         │               │            │   │
│                    │         ▼               ▼            │   │
│                    │   ┌──────────┐   ┌──────────────┐   │   │
│                    │   │ Lista de │   │ Ranking de   │   │   │
│                    │   │ Visitas  │   │ Sabores DDL  │   │   │
│                    │   │ Cadore   │   │ (Todos)      │   │   │
│                    │   └────┬─────┘   └──────────────┘   │   │
│                    │        │                            │   │
│                    │        ▼                            │   │
│                    │  ┌───────────┐                      │   │
│                    │  │ Detalle   │                      │   │
│                    │  │ Experienc.│                      │   │
│                    │  └───────────┘                      │   │
│                    └────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📐 Wireframes de Baja Fidelidad

### 1. Splash & Onboarding

```
┌─────────────────────────┐    ┌─────────────────────────┐    ┌─────────────────────────┐
│                         │    │                         │    │                         │
│                         │    │    🍦 🍦 🍦 🍦 🍦      │    │                         │
│                         │    │                         │    │    Cada helado tiene    │
│         🍦              │    │   "Nunca más olvides   │    │    una historia.        │
│                         │    │    cuál fue ese gusto  │    │                         │
│      BITÁCORA           │    │      increíble"        │    │    📊 Calificá          │
│     DE HELADOS          │    │                         │    │    📍 Recordá           │
│                         │    │    ─────────────────   │    │    🏆 Descubrí          │
│                         │    │                         │    │                         │
│                         │    │   [SIGUIENTE ──────▶]  │    │   [EMPEZAR ──────▶]    │
│                         │    │                         │    │                         │
│    ● ○ ○                │    │    ○ ● ○                │    │    ○ ○ ●                │
└─────────────────────────┘    └─────────────────────────┘    └─────────────────────────┘
```

### 2. Login Social

```
┌─────────────────────────┐
│                         │
│         🍦              │
│                         │
│   Creá tu bitácora      │
│   personal de helados   │
│                         │
│                         │
│  ┌─────────────────────┐│
│  │  G  Continuar con   ││
│  │     Google          ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │  🍎 Continuar con   ││
│  │     Apple           ││
│  └─────────────────────┘│
│                         │
│                         │
│   ─────── o ───────     │
│                         │
│   Continuar con email   │
│                         │
│                         │
│  Al continuar aceptás   │
│  nuestros Términos      │
│                         │
└─────────────────────────┘
```

### 3. Home Principal

```
┌─────────────────────────┐
│  🍦 Bitácora     ⚙️     │
├─────────────────────────┤
│                         │
│  ┌─────────────────────┐│
│  │ 📍 ¿Estás en        ││
│  │                     ││
│  │ 🏪 Heladería Cadore ││
│  │    Av. Corrientes   ││
│  │                     ││
│  │  [✓ Sí]  [Buscar]  ││
│  └─────────────────────┘│
│                         │
│  ─────────────────────  │
│                         │
│  📊 Tu Resumen          │
│                         │
│  ┌───────┐ ┌───────┐   │
│  │  42   │ │  12   │   │
│  │sabores│ │helader│   │
│  │probado│ │visita │   │
│  └───────┘ └───────┘   │
│                         │
│  🏆 Mejor DDL: Cadore   │
│     DDL Granizado ★★★★★ │
│                         │
│                         │
│     ╭─────────────╮     │
│     │             │     │
│     │  CALIFICAR  │     │
│     │   HELADO    │     │
│     │     🍦      │     │
│     ╰─────────────╯     │
│                         │
├─────────────────────────┤
│  🏠    📖    👤         │
│ Home  Bitácora  Perfil  │
└─────────────────────────┘
```

### 4. Selección de Heladería (Búsqueda Manual)

```
┌─────────────────────────┐
│  ← Buscar Heladería     │
├─────────────────────────┤
│                         │
│  ┌─────────────────────┐│
│  │ 🔍 Buscar...        ││
│  └─────────────────────┘│
│                         │
│  📍 Cerca de vos        │
│                         │
│  ┌─────────────────────┐│
│  │ 🏪 Freddo           ││
│  │    200m • Palermo   ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🏪 Persicco         ││
│  │    350m • Palermo   ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🏪 Volta            ││
│  │    500m • Recoleta  ││
│  └─────────────────────┘│
│                         │
│  ─────────────────────  │
│                         │
│  🕐 Visitadas antes     │
│                         │
│  ┌─────────────────────┐│
│  │ 🏪 Cadore           ││
│  │    Última: 15/11    ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ + Agregar nueva     ││
│  │   heladería         ││
│  └─────────────────────┘│
│                         │
└─────────────────────────┘
```

### 5. Selector de Familia de Sabores (★ PANTALLA CLAVE)

```
┌─────────────────────────┐
│  ← Agregar Sabores      │
├─────────────────────────┤
│                         │
│  📍 Heladería Cadore    │
│                         │
│  Elegí la familia:      │
│                         │
│  ┌─────────┬─────────┐  │
│  │         │         │  │
│  │   🍫    │   🍯    │  │
│  │         │         │  │
│  │CHOCOLATE│  DDL    │  │
│  │         │         │  │
│  └─────────┴─────────┘  │
│  ┌─────────┬─────────┐  │
│  │         │         │  │
│  │   🍓    │   🍦    │  │
│  │         │         │  │
│  │FRUTALES │ CREMAS  │  │
│  │         │         │  │
│  └─────────┴─────────┘  │
│  ┌─────────────────────┐│
│  │         🌿          ││
│  │     ESPECIALES      ││
│  └─────────────────────┘│
│                         │
│  ─────────────────────  │
│                         │
│  🔍 O buscá directo:    │
│  ┌─────────────────────┐│
│  │ Escribí el sabor... ││
│  └─────────────────────┘│
│                         │
└─────────────────────────┘
```

### 6. Lista de Sabores dentro de Familia

```
┌─────────────────────────┐
│  ← 🍯 Dulce de Leche    │
├─────────────────────────┤
│                         │
│  ┌─────────────────────┐│
│  │ 🔍 Buscar en DDL... ││
│  └─────────────────────┘│
│                         │
│  Sabores conocidos:     │
│                         │
│  ┌─────────────────────┐│
│  │ ○ DDL Clásico       ││
│  │   ★★★★☆ (tu última) ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ ○ DDL Granizado     ││
│  │   🏆 Tu favorito    ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ ○ DDL con Brownie   ││
│  │                     ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ ○ DDL con Nuez      ││
│  │                     ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ ○ DDL Tentación     ││
│  │                     ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ + Crear nuevo sabor ││
│  │   de DDL            ││
│  └─────────────────────┘│
│                         │
│  ━━━━━━━━━━━━━━━━━━━━━  │
│  Sabores seleccionados: │
│  DDL Granizado (1/4)    │
│                         │
│     [CONTINUAR ▶]       │
│                         │
└─────────────────────────┘
```

### 7. Pantalla de Calificación (★ CORAZÓN DE LA APP)

```
┌─────────────────────────┐
│  ← Calificar Sabores    │
├─────────────────────────┤
│                         │
│  📍 Cadore • Hoy        │
│                         │
│  ┌─────────────────────┐│
│  │ 🍯 DDL Granizado    ││
│  │                     ││
│  │  ☆  ☆  ☆  ☆  ☆     ││
│  │                     ││
│  │ ┌─────────────────┐ ││
│  │ │Nota: (opcional) │ ││
│  │ └─────────────────┘ ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ 🍫 Choc. Amargo     ││
│  │                     ││
│  │  ☆  ☆  ☆  ☆  ☆     ││
│  │                     ││
│  │ ┌─────────────────┐ ││
│  │ │Nota: (opcional) │ ││
│  │ └─────────────────┘ ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ + Agregar otro sabor││
│  │   (2/4 gustos)      ││
│  └─────────────────────┘│
│                         │
│                         │
│  ╭─────────────────────╮│
│  │                     ││
│  │  GUARDAR EXPERIENCIA││
│  │         ✓           ││
│  ╰─────────────────────╯│
│                         │
└─────────────────────────┘
```

### 8. Calificación con Estrellas (Detalle de Interacción)

```
┌─────────────────────────┐
│                         │
│  🍯 DDL Granizado       │
│                         │
│     ★  ★  ★  ★  ☆      │
│                         │
│   Muy bueno             │
│                         │
│  ┌─────────────────────┐│
│  │ Muy cremoso, aunque ││
│  │ le faltaba un poco  ││
│  │ más de granizado... ││
│  │                     ││
│  └─────────────────────┘│
│                         │
└─────────────────────────┘

Estados de estrellas:
1 ★☆☆☆☆ - Malo
2 ★★☆☆☆ - Regular  
3 ★★★☆☆ - Bueno
4 ★★★★☆ - Muy bueno
5 ★★★★★ - Excelente
```

### 9. Confirmación de Éxito

```
┌─────────────────────────┐
│                         │
│                         │
│                         │
│           🎉            │
│                         │
│    ¡Experiencia        │
│      guardada!          │
│                         │
│  ─────────────────────  │
│                         │
│  📍 Cadore              │
│  🍦 2 sabores           │
│  📅 3 de Diciembre      │
│                         │
│                         │
│  ┌─────────────────────┐│
│  │   Ver en Bitácora   ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │   Volver al Home    ││
│  └─────────────────────┘│
│                         │
│                         │
│                         │
└─────────────────────────┘
```

### 10. Mi Bitácora - Vista por Heladería

```
┌─────────────────────────┐
│  📖 Mi Bitácora         │
├─────────────────────────┤
│                         │
│  ┌──────────┬──────────┐│
│  │▓▓▓▓▓▓▓▓▓▓│          ││
│  │Por Helad.│Por Famil.││
│  └──────────┴──────────┘│
│                         │
│  🔍 Buscar heladería... │
│                         │
│  ┌─────────────────────┐│
│  │ 🏪 Cadore           ││
│  │    5 visitas        ││
│  │    Última: hace 2h  ││
│  │    ▸                ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🏪 Freddo           ││
│  │    3 visitas        ││
│  │    Última: 15/11    ││
│  │    ▸                ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🏪 Persicco         ││
│  │    2 visitas        ││
│  │    Última: 02/11    ││
│  │    ▸                ││
│  └─────────────────────┘│
│                         │
│                         │
├─────────────────────────┤
│  🏠    📖    👤         │
│ Home  Bitácora  Perfil  │
└─────────────────────────┘
```

### 11. Mi Bitácora - Vista por Familia

```
┌─────────────────────────┐
│  📖 Mi Bitácora         │
├─────────────────────────┤
│                         │
│  ┌──────────┬──────────┐│
│  │          │▓▓▓▓▓▓▓▓▓▓││
│  │Por Helad.│Por Famil.││
│  └──────────┴──────────┘│
│                         │
│  🏆 TUS RANKINGS        │
│                         │
│  ┌─────────────────────┐│
│  │ 🍯 Dulce de Leche   ││
│  │    8 sabores        ││
│  │    probados         ││
│  │                     ││
│  │  1. DDL Granizado   ││
│  │     Cadore ★★★★★    ││
│  │                     ││
│  │  2. DDL con Nuez    ││
│  │     Freddo ★★★★☆    ││
│  │                     ││
│  │    [Ver todos ▸]    ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ 🍫 Chocolates       ││
│  │    5 sabores        ││
│  │    [Ver ranking ▸]  ││
│  └─────────────────────┘│
│                         │
│  ┌─────────────────────┐│
│  │ 🍓 Frutales         ││
│  │    3 sabores        ││
│  │    [Ver ranking ▸]  ││
│  └─────────────────────┘│
│                         │
├─────────────────────────┤
│  🏠    📖    👤         │
│ Home  Bitácora  Perfil  │
└─────────────────────────┘
```

### 12. Detalle de Visita a Heladería

```
┌─────────────────────────┐
│  ← Cadore               │
├─────────────────────────┤
│                         │
│  📍 Av. Corrientes 1695 │
│     Buenos Aires        │
│                         │
│  ━━━━━━━━━━━━━━━━━━━━━  │
│                         │
│  📅 3 de Diciembre      │
│                         │
│  ┌─────────────────────┐│
│  │ 🍯 DDL Granizado    ││
│  │    ★★★★★            ││
│  │    "Perfecto, mucho ││
│  │     granizado"      ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🍫 Choc. Amargo     ││
│  │    ★★★★☆            ││
│  │    "Muy intenso"    ││
│  └─────────────────────┘│
│                         │
│  ━━━━━━━━━━━━━━━━━━━━━  │
│                         │
│  📅 15 de Noviembre     │
│                         │
│  ┌─────────────────────┐│
│  │ 🍦 Americana        ││
│  │    ★★★☆☆            ││
│  └─────────────────────┘│
│  ┌─────────────────────┐│
│  │ 🍯 DDL Clásico      ││
│  │    ★★★★☆            ││
│  └─────────────────────┘│
│                         │
└─────────────────────────┘
```

---

## 🎨 Sistema de Diseño

### Paleta de Colores

```
┌─────────────────────────────────────────────────────────────────┐
│                     PALETA PRINCIPAL                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   PRIMARY                                                       │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐                          │
│   │ #F5E6D3 │ │ #E8D5C4 │ │ #C4A77D │                          │
│   │  Cream  │ │  Waffle │ │ Caramel │                          │
│   │  Base   │ │  Light  │ │  Accent │                          │
│   └─────────┘ └─────────┘ └─────────┘                          │
│                                                                 │
│   SECONDARY                                                     │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐                          │
│   │ #6B4423 │ │ #8B6914 │ │ #2D1810 │                          │
│   │Chocolate│ │  Dulce  │ │  Dark   │                          │
│   │  Brown  │ │de Leche │ │ Cocoa   │                          │
│   └─────────┘ └─────────┘ └─────────┘                          │
│                                                                 │
│   ACCENTS (FAMILIAS)                                           │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐             │
│   │ #4A3728 │ │ #D4A574 │ │ #E85A5A │ │ #F5F0E8 │             │
│   │Chocolate│ │  DDL    │ │ Frutal  │ │ Crema   │             │
│   └─────────┘ └─────────┘ └─────────┘ └─────────┘             │
│                                                                 │
│   ┌─────────┐                                                  │
│   │ #7CB342 │                                                  │
│   │Especial │                                                  │
│   └─────────┘                                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Colores por Familia de Sabor

```
┌─────────────────────────────────────────────────────────────────┐
│                    FAMILIA → COLOR                              │
├─────────────────┬───────────────┬───────────────────────────────┤
│ Familia         │ Color         │ Uso                           │
├─────────────────┼───────────────┼───────────────────────────────┤
│ 🍫 Chocolates   │ #4A3728       │ Badge, borde tarjeta          │
│ 🍯 Dulce Leche  │ #D4A574       │ Badge, borde tarjeta          │
│ 🍓 Frutales     │ #E85A5A       │ Badge, borde tarjeta          │
│ 🍦 Cremas       │ #F5F0E8       │ Badge con borde, tarjeta      │
│ 🌿 Especiales   │ #7CB342       │ Badge, borde tarjeta          │
└─────────────────┴───────────────┴───────────────────────────────┘
```

### Tipografía

```
┌─────────────────────────────────────────────────────────────────┐
│                      TIPOGRAFÍA                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   DISPLAY / TÍTULOS                                            │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │ Playfair Display                                        │  │
│   │                                                         │  │
│   │ Aa Bb Cc Dd Ee                                         │  │
│   │ BITÁCORA DE HELADOS                                    │  │
│   └─────────────────────────────────────────────────────────┘  │
│   Uso: Logo, títulos de sección, nombres de heladerías         │
│                                                                 │
│   BODY / CONTENIDO                                             │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │ DM Sans                                                 │  │
│   │                                                         │  │
│   │ Aa Bb Cc Dd Ee Ff Gg                                   │  │
│   │ El helado perfecto existe, solo hay que encontrarlo.   │  │
│   └─────────────────────────────────────────────────────────┘  │
│   Uso: Cuerpo, botones, labels, notas                          │
│                                                                 │
│   ESCALA                                                       │
│   ├── Display:     32px / Bold                                 │
│   ├── H1:          24px / Bold                                 │
│   ├── H2:          20px / SemiBold                             │
│   ├── Body:        16px / Regular                              │
│   ├── Body Small:  14px / Regular                              │
│   └── Caption:     12px / Regular                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Componentes UI

```
┌─────────────────────────────────────────────────────────────────┐
│                     COMPONENTES                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   BOTÓN PRIMARIO (FAB)                                         │
│   ┌───────────────────────────────────────┐                    │
│   │                                       │                    │
│   │  ╭───────────────────────────────╮   │  Background: #6B4423│
│   │  │      CALIFICAR HELADO 🍦      │   │  Text: #FFFFFF      │
│   │  ╰───────────────────────────────╯   │  Radius: 28px       │
│   │                                       │  Shadow: 0 4px 12px │
│   └───────────────────────────────────────┘  Padding: 16px 32px │
│                                                                 │
│   TARJETA DE SABOR                                             │
│   ┌───────────────────────────────────────┐                    │
│   │ ┌───────────────────────────────────┐ │  Background: #FFF   │
│   │ │ 🍯 DDL Granizado          ★★★★★ │ │  Border-left: 4px   │
│   │ │    "Perfecto"                     │ │  (color familia)    │
│   │ └───────────────────────────────────┘ │  Radius: 12px       │
│   └───────────────────────────────────────┘  Shadow: soft       │
│                                                                 │
│   SELECTOR DE FAMILIA                                          │
│   ┌───────────────────────────────────────┐                    │
│   │  ┌─────────┐                          │  Size: 80x80px      │
│   │  │         │                          │  Border-radius: 16px│
│   │  │   🍫    │  ← Icono grande          │  Background: color  │
│   │  │         │                          │  familia con 20%    │
│   │  │CHOCOLATE│  ← Label debajo          │  opacity            │
│   │  └─────────┘                          │                     │
│   └───────────────────────────────────────┘                    │
│                                                                 │
│   RATING STARS                                                 │
│   ┌───────────────────────────────────────┐                    │
│   │                                       │  Size: 32px (touch) │
│   │     ★  ★  ★  ★  ☆                   │  Color activo:      │
│   │                                       │  #D4A574 (dorado)   │
│   │     Touch area: 48x48px cada una     │  Color inactivo:    │
│   │                                       │  #E0E0E0            │
│   └───────────────────────────────────────┘                    │
│                                                                 │
│   CHIP / BADGE DE FAMILIA                                      │
│   ┌───────────────────────────────────────┐                    │
│   │  ┌───────────────┐                    │  Height: 24px       │
│   │  │ 🍯 DDL        │                    │  Padding: 4px 12px  │
│   │  └───────────────┘                    │  Radius: 12px       │
│   └───────────────────────────────────────┘  Font: 12px         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Iconografía de Familias

```
┌─────────────────────────────────────────────────────────────────┐
│                    ICONOS DE FAMILIA                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐          │
│   │         │  │         │  │         │  │         │          │
│   │   🍫    │  │   🍯    │  │   🍓    │  │   🍦    │          │
│   │         │  │         │  │         │  │         │          │
│   │Chocolate│  │  DDL    │  │ Frutal  │  │ Crema   │          │
│   └─────────┘  └─────────┘  └─────────┘  └─────────┘          │
│                                                                 │
│   ┌─────────┐                                                  │
│   │         │  Nota: Usar emojis nativos para MVP             │
│   │   🌿    │  En versión futura: iconografía custom          │
│   │         │                                                  │
│   │Especial │                                                  │
│   └─────────┘                                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📱 Especificaciones por Pantalla

### Spacing System

```
Base unit: 4px

┌──────────────────────────────────────┐
│ Space Token    │ Valor   │ Uso      │
├──────────────────────────────────────┤
│ xs             │ 4px     │ Interno  │
│ sm             │ 8px     │ Gaps     │
│ md             │ 16px    │ Padding  │
│ lg             │ 24px    │ Secciones│
│ xl             │ 32px    │ Bloques  │
│ xxl            │ 48px    │ Pantallas│
└──────────────────────────────────────┘
```

### Safe Areas y Touch Targets

```
┌─────────────────────────┐
│     Status Bar 44px     │ ← Safe area top
├─────────────────────────┤
│                         │
│   Mínimo touch target:  │
│   48x48 px              │
│                         │
│   FAB size: 56px        │
│   FAB margin: 16px      │
│                         │
│                         │
├─────────────────────────┤
│   Tab Bar: 83px         │ ← Safe area bottom
│   (49px + 34px home)    │
└─────────────────────────┘
```

---

## ✨ Microinteracciones

### 1. Calificación con Estrellas

```
┌─────────────────────────────────────────────────────────────────┐
│ INTERACCIÓN: Seleccionar Rating                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Estado Inicial:                                                 │
│     ☆  ☆  ☆  ☆  ☆                                              │
│                                                                 │
│ Al tocar estrella 4:                                           │
│     ★  ★  ★  ★  ☆   ← Animación: scale(1.2) + bounce          │
│         ↓                                                       │
│   "Muy bueno"        ← Texto aparece con fade-in               │
│                                                                 │
│ Feedback háptico:    Impacto ligero                            │
│ Duración animación:  200ms ease-out                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Agregar Sabor a la Lista

```
┌─────────────────────────────────────────────────────────────────┐
│ INTERACCIÓN: Seleccionar sabor de la lista                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Estado: Item no seleccionado                                   │
│ ┌─────────────────────────┐                                    │
│ │ ○ DDL Granizado         │                                    │
│ └─────────────────────────┘                                    │
│                                                                 │
│ Al tocar:                                                      │
│ ┌─────────────────────────┐                                    │
│ │ ✓ DDL Granizado    ✓   │  ← Check aparece con scale-in      │
│ │   Background: familia   │  ← Fondo cambia con transición     │
│ └─────────────────────────┘                                    │
│                                                                 │
│ Footer actualizado:                                            │
│ ━━━━━━━━━━━━━━━━━━━━━━━                                        │
│ "1 sabor seleccionado"    ← Slide-up animation                 │
│ [CONTINUAR ▶]                                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3. Confirmación de Guardado

```
┌─────────────────────────────────────────────────────────────────┐
│ INTERACCIÓN: Guardar experiencia exitosa                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Secuencia de animación:                                        │
│                                                                 │
│ 1. Botón presionado                                            │
│    ╭─────────────────────╮                                     │
│    │  GUARDANDO...  ⟳   │  ← Spinner aparece                  │
│    ╰─────────────────────╯                                     │
│                                                                 │
│ 2. Success (300ms después)                                     │
│                                                                 │
│              🎉           ← Confetti particles                 │
│           (scale 0→1)                                          │
│                                                                 │
│    ╭─────────────────────╮                                     │
│    │     ✓ GUARDADO     │  ← Verde, check animado             │
│    ╰─────────────────────╯                                     │
│                                                                 │
│ 3. Transición a pantalla de éxito                              │
│    (fade + slide-up, 400ms)                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4. Detección de Heladería

```
┌─────────────────────────────────────────────────────────────────┐
│ INTERACCIÓN: Geolocalización detecta heladería                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Estado: Buscando...                                            │
│ ┌─────────────────────────┐                                    │
│ │ 📍 Buscando...         │  ← Pulse animation en icono        │
│ │    ▓▓▓░░░░░░░          │  ← Progress bar                    │
│ └─────────────────────────┘                                    │
│                                                                 │
│ Estado: Encontrado                                             │
│ ┌─────────────────────────┐                                    │
│ │ 📍 ¿Estás en           │                                    │
│ │                        │  ← Card slide-in desde abajo       │
│ │ 🏪 Heladería Cadore    │  ← Nombre con highlight            │
│ │    Av. Corrientes      │                                    │
│ │                        │                                    │
│ │  [✓ Sí]  [Buscar]     │                                    │
│ └─────────────────────────┘                                    │
│                                                                 │
│ Feedback háptico al detectar: Notificación suave               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔍 Autocomplete Inteligente

### Lógica de Búsqueda

```
┌─────────────────────────────────────────────────────────────────┐
│ BÚSQUEDA: Usuario escribe "Granizado"                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Input:                                                         │
│ ┌─────────────────────────┐                                    │
│ │ 🔍 Granizado_           │                                    │
│ └─────────────────────────┘                                    │
│                                                                 │
│ Resultados (priorizados):                                      │
│                                                                 │
│ ┌─────────────────────────────────────────┐                    │
│ │ 🍯 DDL Granizado                        │ ← Familia DDL      │
│ │    Dulce de Leche • 127 calificaciones  │                    │
│ └─────────────────────────────────────────┘                    │
│ ┌─────────────────────────────────────────┐                    │
│ │ 🍫 Chocolate Granizado                  │ ← Familia Choc     │
│ │    Chocolates • 89 calificaciones       │                    │
│ └─────────────────────────────────────────┘                    │
│ ┌─────────────────────────────────────────┐                    │
│ │ 🌿 Menta Granizada                      │ ← Familia Esp      │
│ │    Especiales • 45 calificaciones       │                    │
│ └─────────────────────────────────────────┘                    │
│                                                                 │
│ ─────────────────────────────────────────                      │
│                                                                 │
│ ┌─────────────────────────────────────────┐                    │
│ │ + Crear "Granizado" como nuevo sabor    │                    │
│ │   (Elegir familia)                      │                    │
│ └─────────────────────────────────────────┘                    │
│                                                                 │
│ Orden de prioridad:                                            │
│ 1. Sabores que el usuario ya calificó                          │
│ 2. Sabores más populares globalmente                           │
│ 3. Opción de crear nuevo (siempre al final)                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 Resumen de Entregables

### Checklist de Diseño MVP

```
┌─────────────────────────────────────────────────────────────────┐
│                    ENTREGABLES                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ ✅ Wireframes de Baja Fidelidad                                │
│    ├── Onboarding (3 pantallas)                                │
│    ├── Login Social                                            │
│    ├── Home Principal                                          │
│    ├── Selector de Heladería                                   │
│    ├── Selector de Familia                                     │
│    ├── Lista de Sabores                                        │
│    ├── Pantalla de Calificación                                │
│    ├── Confirmación de Éxito                                   │
│    └── Mi Bitácora (2 vistas)                                  │
│                                                                 │
│ ✅ Sistema de Diseño                                           │
│    ├── Paleta de colores                                       │
│    ├── Colores por familia                                     │
│    ├── Tipografía                                              │
│    ├── Componentes UI                                          │
│    ├── Iconografía                                             │
│    └── Spacing system                                          │
│                                                                 │
│ ✅ Especificaciones de Interacción                             │
│    ├── Microinteracciones documentadas                         │
│    ├── Estados de componentes                                  │
│    └── Feedback háptico                                        │
│                                                                 │
│ 📋 Próximos Pasos                                              │
│    ├── [ ] Prototipo navegable en Figma                        │
│    ├── [ ] UI Kit de alta fidelidad                            │
│    ├── [ ] Pruebas de usabilidad con usuarios                  │
│    └── [ ] Iteración basada en feedback                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Métricas de Éxito UX

| Métrica | Objetivo MVP | Cómo Medir |
|---------|--------------|------------|
| Tiempo para calificar 1 helado | < 30 segundos | Analytics |
| Toques para completar flujo | ≤ 6 toques | User testing |
| Tasa de abandono en calificación | < 20% | Funnel analytics |
| NPS de la experiencia | > 40 | Encuesta in-app |

---

*Documento creado para el MVP de "Bitácora de Helados"*
*Versión 1.0 - Diciembre 2025*

