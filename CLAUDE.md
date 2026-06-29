# Proyecto: Cheat Sheets

Conjunto de hojas de repaso HTML, una por herramienta. **Misma estructura (sistema), distinta esencia.**

## Qué hay aquí
- `.claude/skills/cheatsheet-builder/` — la skill que define cómo se construye cada hoja.
- `curriculum/<tool>.json` — el temario de cada herramienta (lo actualizo cada cierto tiempo).
- `sheets/cheatsheet_<tool>.html` — las hojas generadas (la salida).
- `index.html` — selector maestro (se construye al final).

## Cómo trabajar (reglas que Claude debe seguir siempre)
1. **Usa la skill `cheatsheet-builder`** para crear o actualizar cualquier hoja. Lee primero su
   `SKILL.md` y sus `references/` (incluido `tool-profiles.md`).
2. **Sistema = igual para todas:** ventana de app con título grande, barra de navegación fija a la
   izquierda que hace scroll del contenido, stepper de secciones, etiquetas **B/I/A** con leyenda,
   **clic para copiar**, **tema claro** minimalista y profesional (fondo blanco/marfil).
3. **Esencia = única por herramienta:** color, cita de héroe y motivo salen de
   `references/tool-profiles.md`. Si la herramienta no está, añádela al registro.
4. **Pulido obligatorio:** cajas alineadas y de **igual alto por fila** (sin una caja más grande que
   otra: rellena, empareja o usa ancho completo) y **sin scroll horizontal** en el código (acorta o
   parte las líneas). Una barra de scroll en una caja de código es un bug, no una opción.
5. **Actualizar por nombre, sin rehacer:** si `sheets/cheatsheet_<tool>.html` ya existe, NO la
   regeneres. Compárala con `curriculum/<tool>.json`, añade **solo** los temas no cubiertos y conserva
   lo demás (orden, diseño, niveles).
6. **Valida antes de guardar:** todos los enlaces del índice resuelven, etiquetas balanceadas, sin
   overflow horizontal, B/I/A y cita presentes. Guarda en `sheets/`.

## Convenciones
- Español para el contenido de las hojas.
- Una hoja = un archivo `.html` autocontenido (CSS y JS dentro; fuentes por CDN).
- No `localStorage` ni dependencias externas más allá de Google Fonts.
