# Sistema de Diseño — Liderar desde la Serenidad

Landing page de Tránsito Aracil (psicóloga integral / programa ejecutivo de regulación del sistema nervioso). Construida con **Astro + Tailwind CSS**. Este documento describe el sistema de diseño tal como está implementado hoy en el código (`tailwind.config.mjs`, `src/styles/global.css`, `src/components/*`).

La estética busca transmitir **calidez, profesionalismo y espiritualidad sobria** — nada corporativo-frío ni new-age genérico. Referencia clínica de alto nivel ("high-end executive"), con acentos dorados y formas orgánicas.

---

## 1. Paleta de color

Definida en `tailwind.config.mjs`:

| Token | Hex | Uso |
| :--- | :--- | :--- |
| `primary` | `#7A3EFE` | Violeta principal. CTAs, links activos, acentos de "transformación". |
| `primary-soft` | `#EBE4FF` | Fondos suaves sobre `primary` (badges, hover states). |
| `gold` | `#C6A87C` | Acento cálido. Iconos secundarios, bullets, líneas decorativas, estrellas. |
| `gold-light` | `#E5D4B8` | Bordes y scrollbar, variante clara del dorado. |
| `cream` | `#FDFBF7` | Fondo base del `<body>`. |
| `cream-dark` | `#F2EFE9` | Fondo de secciones alternas (para crear ritmo visual sin usar blanco puro). |
| `text-main` | `#2D2A32` | Texto principal (casi negro, con tinte cálido). |
| `text-light` | `#6B6671` | Texto secundario / párrafos de apoyo. |

**Reglas de uso:**
- El violeta (`primary`) es el único color de acción — se reserva para CTAs y elementos interactivos clave. No se usa como color decorativo de fondo salvo en opacidades muy bajas (`primary/5`, `primary/10`).
- El dorado (`gold`) nunca lleva acción; es puramente ornamental (bordes finos, círculos concéntricos, subrayados tipo "swoosh", bullets de listas).
- Los fondos alternan entre `cream` (secciones default) y `white` / `cream-dark` para separar bloques sin usar líneas divisorias duras.
- Las sombras de color (`shadow-primary/20`, `shadow-primary/30`) se usan bajo botones y tarjetas para dar profundidad "suave" en vez de sombras grises planas.

> Nota histórica: el brief original (`agen.md`) especificaba `#FFC350` (naranja/dorado vivo) y fondo `#FAFAFA`. La implementación actual evolucionó hacia una paleta más apagada y cálida (`gold` `#C6A87C`, `cream` `#FDFBF7`), que es la que debe tomarse como fuente de verdad.

---

## 2. Tipografía

```js
fontFamily: {
  serif: ["Playfair Display", "serif"],
  sans:  ["Lato", "sans-serif"],
}
```

Cargadas vía Google Fonts en `Layout.astro` (pesos: Playfair 400/500/600/700 + itálica 400; Lato 300/400/700).

| Elemento | Fuente | Notas |
| :--- | :--- | :--- |
| H1 / H2 / H3 | `font-serif` (Playfair Display) | Tamaños grandes (`text-4xl` a `text-7xl`), a veces con una palabra clave en `italic text-primary` para dar énfasis emocional. |
| Cuerpo de texto | `font-sans` (Lato), normalmente `font-light` | `text-text-light`, `leading-relaxed`. |
| Botones / CTAs | `font-serif`, `uppercase`, `tracking-wider`/`tracking-widest` | Los botones son la única excepción donde el serif se usa en mayúsculas — refuerza la sensación "editorial/boutique" en vez de "app SaaS". |
| Eyebrows / labels | `font-sans`, `text-xs`, `uppercase`, `tracking-[0.2em]` o `tracking-widest`, a menudo `text-gold` o `text-primary` | Ej. "Los Fundamentos", "Programa Ejecutivo High-End". |
| Iconografía | Material Symbols Outlined | `font-variation-settings: 'FILL' 0, 'wght' 300` — trazo fino, no relleno. |

**Jerarquía tipo:**
- Hero H1: `text-5xl md:text-6xl lg:text-7xl font-serif font-medium leading-[1.15] tracking-tight`
- H2 de sección: `text-4xl md:text-5xl` (o `md:text-6xl` en CTAs finales), `font-serif`
- H3 de tarjeta: `text-2xl font-serif`
- Body: `text-lg`/`text-xl` para intros, `text-base` para contenido normal, siempre `font-light` cuando es texto largo.

---

## 3. Componentes UI

### Botones
- **Forma:** completamente redondeados (`rounded-full`), nunca esquinas rectas.
- **CTA primario:** `bg-primary hover:bg-primary/90 text-white`, `font-serif`, `uppercase`, `tracking-widest`, padding generoso (`px-10 py-4` o `px-12 py-5` en CTAs hero/finales), `shadow-xl shadow-primary/25-30`.
- **Micro-interacción:** `hover:scale-105 duration-300` en CTAs grandes; en botones con flecha (`arrow_forward`), la flecha se traslada con `group-hover:translate-x-1`.
- No existe actualmente un estilo de botón "secundario" formal en el código (a diferencia del brief original que pedía un botón dorado) — todas las acciones visibles usan el primario violeta.

### Tarjetas
- Fondo blanco sobre secciones `cream`/`cream-dark` para destacar.
- Radio de borde muy grande y asimétrico: `rounded-[2rem]` en tarjetas de contenido; las imágenes hero usan `rounded-t-[10rem] rounded-b-[2rem]` (forma de "arco", evita cajas rígidas).
- Sombra sutil por defecto (`shadow-sm`), que crece a `shadow-2xl shadow-primary/5-10` en hover, con `transition-all duration-500`.
- La tarjeta "destacada" en una grilla de 3 (columna central) se eleva con `md:-translate-y-6` y lleva un borde interior extra (`border-2 border-primary/5`) para jerarquía visual sin cambiar de forma.
- Iconos dentro de tarjetas: círculo (`rounded-full`) de `w-16 h-16`, fondo `primary/5` (o `bg-primary` sólido para la tarjeta destacada), con `group-hover:scale-110`.

### Header / Footer
- Header fijo (`fixed top-0`), fondo semitransparente con blur (`bg-cream/80 backdrop-blur-md`), borde inferior fino dorado (`border-gold/10`).
- Footer sobre fondo blanco, enlaces en mayúsculas con `tracking-widest`, iconos sociales en gris que pasan a `primary` en hover.

### Badges / eyebrows
`inline-flex` con `px-4 py-1.5`, fondo `primary/5`, borde `primary/10`, `rounded-full`, texto `text-xs uppercase tracking-widest`.

---

## 4. Formas y motivos gráficos

- **Nada de esquinas vivas.** Toda superficie relevante usa `rounded-full`, `rounded-[2rem]` o combinaciones asimétricas tipo arco (`rounded-t-[10rem] rounded-b-[2rem]`).
- **Círculos concéntricos decorativos:** pares de `border border-gold/10 rounded-full` de gran tamaño (600–800px), centrados absolutamente detrás del contenido, muy usados en secciones de CTA.
- **SVGs orgánicos de fondo:** formas tipo "mariposa"/curvas en `text-primary` u `text-gold` con opacidad muy baja (`opacity-[0.03]` a `opacity-[0.05]`) — decoración ambiental, nunca compiten con el contenido.
- **Subrayados manuscritos:** un `<svg>` con un path curvo (`Q 50 12 100 5`) posicionado bajo una palabra clave en el H1, color `gold/60` — sustituye al subrayado tradicional.
- **Fotografía:** filtro cálido consistente vía la clase utilitaria `.warm-filter` (`sepia(0.15) contrast(0.95) brightness(1.05) saturate(0.9)`), aplicado a toda imagen de persona/retrato para unificar tono.

---

## 5. Layout y espaciado

- Ancho máximo de contenido: `max-w-[1280px]` (secciones normales) o `max-w-[1400px]` (header/footer), siempre `mx-auto` con padding lateral `px-6` (`md:px-12` en header).
- Padding vertical de sección generoso y consistente: `py-24` a `py-32`. El hero usa `pt-32 pb-20 md:pt-48 md:pb-32` para compensar el header fijo.
- Grillas de 3 columnas (`md:grid-cols-3`) con `gap-8 lg:gap-12`.
- El whitespace es deliberadamente amplio — es parte del mensaje de marca ("calma", "espacio para pensar"), no un descuido.

---

## 6. Interacción y motion

- Transiciones largas y suaves: `duration-300` a `duration-700`, casi nunca instantáneas.
- `hover:scale-105`/`110` en botones e iconos, nunca en bloques de texto.
- `group`/`group-hover` es el patrón estándar para coordinar múltiples cambios (icono + texto + flecha) en un solo hover de tarjeta o botón.
- Scrollbar personalizada (`::-webkit-scrollbar`) en tono `gold-light` sobre fondo `cream`, coherente con la paleta incluso en detalles de sistema.

---

## 7. Voz de marca aplicada a UI

- CTAs siempre en primera persona o imperativo cercano: *"Reservar Sesión Gratuita"*, *"Reservar mi Sesión Gratuita"* — nunca "Click aquí" o "Enviar".
- Micro-copy de refuerzo bajo los CTAs (`text-xs`/`text-sm text-text-light`) elimina fricción: *"100% gratis · Sin compromiso · 15 min por videollamada"*.
- Headlines mezclan autoridad ejecutiva ("El Activo más Crítico de tu Empresa...") con vocabulario somático/clínico ("Sistema Nervioso", "Regulación Vagal"), reflejando el posicionamiento dual del programa: negocio + cuerpo.

---

## 8. Referencia rápida (cheatsheet de clases)

```
Sección estándar:      px-6 py-24 md:py-32
Contenedor:            max-w-[1280px] mx-auto
Eyebrow:               text-xs font-bold uppercase tracking-[0.2em] text-gold
H2:                    text-4xl md:text-5xl font-serif text-text-main
Body:                  text-text-light font-light leading-relaxed
CTA primario:          bg-primary hover:bg-primary/90 text-white font-serif
                       uppercase tracking-widest rounded-full px-10 py-4
                       shadow-xl shadow-primary/25
Card:                  bg-white rounded-[2rem] border border-gray-100 shadow-sm
                       hover:shadow-2xl hover:shadow-primary/5 transition-all duration-500
Icon badge:            w-16 h-16 rounded-full bg-primary/5 text-primary
Decorative ring:       border border-gold/10 rounded-full (absolute, centrado)
```
