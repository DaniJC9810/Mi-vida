# Mi vida — app personal de Dani

Web app personal (PWA) para registrar el día a día con muy poco esfuerzo. Se usa sobre todo desde un iPhone, añadida a la pantalla de inicio. Publicada con GitHub Pages en https://danijc9810.github.io/mi-vida/ a partir de la rama `main`.

## Principios (no romperlos)
- **5 minutos al día como máximo.** El check-in diario debe ser casi todo toques. Cualquier función nueva no puede hacer más lento el registro diario.
- **Sin sobreingeniería.** Un solo `index.html` con CSS y JS dentro, sin frameworks, sin build, sin dependencias. Solo Google Fonts (Manrope y Young Serif).
- **Nunca perder datos.** Los datos viven en `localStorage` con la clave `mivida.v1`. Si cambias la estructura, migra los datos existentes al cargar; nunca borres ni renombres campos sin migración. La exportación/importación de copias debe seguir funcionando con copias antiguas.
- **Diseño tranquilo:** el mar de Benidorm con el Peñón (isla) en la cabecera, paleta azul mar, y detalles rojiblancos minimalistas (rayas rojas y blancas en la vela del velero, indicador de pestaña activa, rachas, chip de Fútbol). Sin escudos ni logos de clubes. Modo claro y oscuro con tokens CSS en `:root`.
- Textos en español, tono cercano y claro.

## Estructura
- `index.html`: toda la app.
- `manifest.webmanifest`, `icon-*.png`: instalación en pantalla de inicio.
- `sw.js`: service worker, red primero con caché de respaldo (funciona sin conexión y siempre carga la última versión publicada). Si cambias la lista de archivos base, sube la versión de `CACHE`.

## Pestañas
- **Hoy:** entreno, dieta, pasos (manual), deporte extra (Fútbol, Pádel, Correr…), social (amigos y con quién), sitio especial (nombre, tipo, estrellas, ¿volverías?), nota del día 1-5 y una frase. Los domingos aparece la revisión semanal.
- **Stats:** semana/mes/año, anillos de cumplimiento, rachas, calendario, pasos, deporte, resumen tipo Wrapped y "Hace cuánto que no…".
- **Dinero:** ingresos netos, gastos fijos y ahorro con meses de inicio y fin, dinero libre para ocio, proyección a 5 años y objetivos de ahorro. No registra transacciones sueltas (a propósito).
- **Planes:** tareas a corto plazo y ideas a largo plazo con estados (Idea, Valorando, En marcha, Hecho, Descartado).
- **Hitos:** cuentas atrás (manuales y automáticas desde Dinero), ajustes y copia de seguridad.

## Modelo de datos (localStorage `mivida.v1`)
```
{ days:   { "YYYY-MM-DD": {gym, diet, steps, sports[], friends, people[], social, place{name,kind,rating,again}, mood, note} },
  finance:{ income, items[{id,name,amount,kind:"fijo"|"ahorro",cat,start:"YYYY-MM",end:"YYYY-MM"}], goals[{id,name,target,saved,date}] },
  plans:  { tasks[{id,text,due,done,doneAt}], ideas[{id,text,cat,status,note,createdAt}] },
  meta:   { stepGoal, gymWeek, milestones[{id,name,date}], reviews{}, lastBackup } }
```
Las fechas de hitos y objetivos aceptan "YYYY", "YYYY-MM" o "YYYY-MM-DD".

## Al hacer cambios
- Prueba que las cinco pestañas cargan sin errores y que los datos existentes se siguen viendo.
- Mantén el diseño responsive para iPhone (zonas seguras con `env(safe-area-inset-*)`).
- Haz commit y push a `main` con un mensaje claro en español; GitHub Pages publica solo.
