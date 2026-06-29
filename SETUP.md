# Puesta en marcha — Cheat Sheets en Claude Code (WSL)

## 1. Por qué tus skills de claude.ai no aparecen en WSL
Son **dos sistemas distintos con el mismo formato**. Las skills de claude.ai viven en tu cuenta
(Settings ▸ Skills, en la nube). Claude Code lee skills del **disco**, en dos sitios:

| Alcance     | Ruta                              | Para qué |
|-------------|-----------------------------------|----------|
| Personal    | `~/.claude/skills/<skill>/SKILL.md`  | disponible en todos tus proyectos |
| Proyecto    | `<proyecto>/.claude/skills/<skill>/SKILL.md` | viaja con el repo (git) |

Si una skill de proyecto y una personal tienen el mismo nombre, **gana la de proyecto**.
Por eso no ves aquí lo que subiste a claude.ai: hay que **copiar las carpetas `SKILL.md` al disco de WSL**.

> Importante en WSL: pon el proyecto en el **sistema de archivos de Linux** (p. ej.
> `~/proyectos/cheatsheets`), **no** en `/mnt/c/...` (más lento y da problemas de permisos).

## 2. Estructura de carpetas
```
~/proyectos/cheatsheets/
├── CLAUDE.md                         # constitución del proyecto (Claude la lee siempre)
├── .claude/
│   └── skills/
│       └── cheatsheet-builder/
│           ├── SKILL.md
│           └── references/
│               ├── default-system.md
│               ├── design-playbook.md
│               └── tool-profiles.md   # ← la "esencia" por herramienta
├── curriculum/                       # un JSON por herramienta (lo actualizas tú)
│   ├── python.json
│   ├── powerbi.json
│   ├── excel.json
│   ├── sql.json
│   ├── git.json
│   ├── css.json
│   └── docker.json
├── sheets/                           # salida: las hojas .html
│   └── cheatsheet_python.html
└── index.html                        # selector maestro (al final)
```

## 3. Instalar la skill (una vez)
```bash
mkdir -p ~/proyectos/cheatsheets/.claude/skills
cp -r cheatsheet-builder ~/proyectos/cheatsheets/.claude/skills/
cd ~/proyectos/cheatsheets
claude            # abre Claude Code en la carpeta
/skills           # verifica que aparece "cheatsheet-builder"
```
- Si **creas** la carpeta `.claude/skills` con la sesión ya abierta, **reinicia** Claude Code.
- Editar un `SKILL.md` con la sesión abierta **se aplica al instante** (hot-reload).
- Para usarla en todos tus proyectos, cópiala también a `~/.claude/skills/`.

## 4. Qué skills tener activas
- **`cheatsheet-builder`** — la principal (esta). Es autosuficiente: su sistema de diseño está en
  `references/`. No necesitas docx/pptx/pdf/xlsx para HTML.
- **`frontend-design`** *(opcional)* — si la tienes, cópiala también a `~/.claude/skills/`; aporta
  gusto tipográfico. No es obligatoria porque `default-system.md` ya trae los tokens de diseño.
- El resto de skills de claude.ai no hacen falta aquí.

## 5. El curriculum (actualizar por nombre)
Un archivo por herramienta. Esquema mínimo:
```json
{
  "course": "The Complete Python Bootcamp From Zero to Hero",
  "tool": "python",
  "sections": [
    { "title": "Python Object and Data Structure Basics",
      "lessons": ["Numbers", "Strings", "Lists", "Dictionaries", "Tuples", "Sets", "Booleans"] }
  ]
}
```
Flujo de actualización: editas `curriculum/python.json` (añades temas nuevos) y pides:
> *"Actualiza la hoja de python."*
Claude compara el curriculum con `sheets/cheatsheet_python.html`, **añade solo lo que falta** y
conserva el resto. (Regla 5 de `CLAUDE.md`.)

## 6. Paso a paso para crear/afinar UNA hoja
1. `cd ~/proyectos/cheatsheets && claude`
2. Pega el **prompt de construcción** (ver `PROMPT.md` o el chat) cambiando `<tool>`.
3. Revisa la hoja en el navegador. Anota lo que no encaje (una caja desalineada, scroll horizontal…).
4. Pide los ajustes concretos; la skill conserva lo demás.
5. Cuando te guste, esa hoja queda como **referencia de estilo** para las siguientes.
6. Repite por herramienta. **Al final**, construye `index.html` como portada que enlaza a todas.

## 7. Modelo y effort (recomendación)
Como actualizas **muy de vez en cuando** (1 vez al año, o cada 6-8 meses), el costo es irrelevante:
usa **Opus 4.8 con effort alto/Max para todo** — diseñar el sistema, crear cada hoja y las raras
actualizaciones. No vale la pena optimizar con Sonnet para algo que corres un par de veces al año;
quédate con la máxima calidad.

## 8. Sobre la rigidez (cajas) y la esencia
- **Rigidez = está en el CSS de la skill** (`default-system.md` §8.5): filas de cajas de **igual alto**
  (`align-items:stretch`), la última caja impar ocupa **ancho completo** (`.tile.wide`), y el código
  **nunca** hace scroll horizontal (envuelve). Por eso no necesitas un script aparte: cada hoja se
  escribe libre, pero las cajas siempre cuadran.
- **Esencia = está en `tool-profiles.md`**: color, cita y motivo propios de cada herramienta. Regla
  dura: **nunca** reutilizar el color de otra (el amarillo es solo de Power BI). Los colores **B/I/A**
  sí son los mismos en todas (verde/amarillo/coral), para que "nivel" se lea igual siempre.
