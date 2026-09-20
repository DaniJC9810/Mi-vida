# Mi vida — app personal de Dani

Web app personal (PWA) para registrar el día a día con muy poco esfuerzo. Se usa sobre todo desde un iPhone, añadida a la pantalla de inicio. Publicada con GitHub Pages en https://danijc9810.github.io/Mi-vida/ (con M mayúscula, la ruta respeta las mayúsculas del nombre del repo) a partir de la rama `main`.

## Principios (no romperlos)
- **5 minutos al día como máximo.** El check-in diario debe ser casi todo toques. Cualquier función nueva no puede hacer más lento el registro diario.
- **Sin sobreingeniería.** Un solo `index.html` con CSS y JS dentro, sin frameworks, sin build, sin dependencias. Solo Google Fonts (Manrope y Young Serif).
- **Nunca perder datos.** Los datos viven en `localStorage` con la clave `mivida.v1`. Si cambias la estructura, migra los datos existentes al cargar; nunca borres ni renombres campos sin migración. La exportación/importación de copias debe seguir funcionando con copias antiguas.
- **Diseño oscuro y directo:** fondo negro con acento rosa fucsia por defecto (Hoy, Stats, Dinero, Planes, Hitos). La pestaña Trabajo cambia a azul y blanco, y Emocional a rojo y blanco; el cambio de paleta se hace con el atributo `data-section` en `<body>` (`trabajo`/`emocional`/sin atributo = por defecto) y bloques `[data-section="..."]` en `:root` que sobrescriben los tokens CSS. Sin ilustración de mar ni barco: la cabecera es una franja de color lisa (`.topbar`) con el título. Un único aspecto (sin modo claro ni `prefers-color-scheme`). Se mantienen detalles rojiblancos minimalistas por el Atleti (rayas en el indicador de pestaña activa, rachas, chip de Fútbol), constantes en las tres paletas. Sin escudos ni logos de clubes.
- Textos en español, tono cercano y claro.

## Estructura
- `index.html`: toda la app.
- `manifest.webmanifest`, `icon-*.png`: instalación en pantalla de inicio.
- `sw.js`: service worker, red primero con caché de respaldo (funciona sin conexión y siempre carga la última versión publicada). Si cambias la lista de archivos base, sube la versión de `CACHE`.

## Pestañas
- **Hoy:** entreno, dieta por comida (desayuno/comida/merienda/cena, cada una con check y nota libre si no se cumple), pasos (manual), deporte extra (Fútbol, Pádel, Correr…), social (amigos y con quién), sitios especiales (varios por día: nombre, tipo, estrellas, ¿volverías?, comentario — engloba restaurante, escapada, concierto, etc.) y películas/series (varias por día: título, nota 1-10, comentario), ambos con botón "+ Añadir" para ir sumando entradas y lista editable/borrable, nota del día 1-5 y una frase. Los domingos aparece la revisión semanal.
- **Stats:** semana/mes/año, anillos de cumplimiento, rachas, calendario (cada día en 4 franjas de arriba abajo: entreno, dieta, pasos, amigos), mapa del año, pasos, deporte, resumen tipo Wrapped (incluye pelis/series vistas, nota media y favorita), lista de "Películas y series" del periodo con su nota, y "Hace cuánto que no…".
- **Dinero:** ingresos netos, gastos fijos y ahorro (categoría, periodicidad en meses y meses de inicio/fin opcionales), dinero libre para ocio, proyección a 5 años y objetivos de ahorro. No registra transacciones sueltas (a propósito).
- **Planes:** tareas a corto plazo y ideas a largo plazo con estados (Idea, Valorando, En marcha, Hecho, Descartado).
- **Hitos:** cuentas atrás editables (manuales y automáticas desde Dinero), ajustes y copia de seguridad.
- **Trabajo:** imputación diaria de horas por tarea y proyecto, con objetivo de horas del día (8,5h L-J y 7h V; 7h L-V en julio y agosto) y del mes; y tareas de trabajo con proyecto, fecha límite y prioridad (0-5) opcionales.
- **Emocional:** aprendizajes, lemas y frases que recordar, con filtro por tipo.

Los proyectos de Trabajo (imputación y tareas) son una lista fija: JAKE, Rossellimac, Gavilán, KALMA, Emuasa, Preventa, Tiempos internos, Formación, Reuniones equipo, Vacaciones.

## Modelo de datos (localStorage `mivida.v1`)
```
{ days:   { "YYYY-MM-DD": {gym, diet:{desayuno,comida,merienda,cena: {ok,note}}, steps, sports[], friends, people[], social, places[{id,name,kind,rating,again,note}], watches[{id,name,rating,note}], mood, note} },
  finance:{ income, items[{id,name,amount,kind:"fijo"|"ahorro",cat,every,start:"YYYY-MM",end:"YYYY-MM"}], goals[{id,name,target,saved,date}] },
  plans:  { tasks[{id,text,due,done,doneAt}], ideas[{id,text,cat,status,note,createdAt}] },
  work:   { entries[{id,date:"YYYY-MM-DD",text,project,hours}], tasks[{id,text,due,priority,project,done,doneAt,createdAt}] },
  emotional: { entries[{id,text,type,createdAt}] },
  meta:   { stepGoal, gymWeek, milestones[{id,name,date}], reviews{}, lastBackup } }
```
`every` en gastos fijos/ahorro es la periodicidad en meses: 0 = no se repite (solo el mes de "start"), 1 = cada mes (por defecto), 2+ = cada N meses desde "start". Las fechas de hitos y objetivos aceptan "YYYY", "YYYY-MM" o "YYYY-MM-DD". `days[].diet` migra automáticamente al cargar si algún día antiguo tiene `diet` como booleano; "dieta cumplida" (rachas, anillos, franja del calendario) exige las 4 comidas marcadas. `days[].social` ya no tiene campo propio en Hoy (se fusionó con "Sitio especial"), pero se conserva para no perder datos antiguos y sigue contando en Stats como señal de día social junto a `friends`. `days[].places`/`days[].watches` migran automáticamente al cargar si un día antiguo tiene `place`/`watch` en formato singular (un único objeto en vez de lista).

## Al hacer cambios
- Prueba que las siete pestañas cargan sin errores y que los datos existentes se siguen viendo.
- Mantén el diseño responsive para iPhone (zonas seguras con `env(safe-area-inset-*)`).
- Haz commit y push a `main` con un mensaje claro en español; GitHub Pages publica solo.
