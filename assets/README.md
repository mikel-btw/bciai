# Guía de Uso: Reporte de Avance de Proyectos CIAI (LaTeX)

Esta guía permite a cualquier integrante o semillero del **Centro de Inteligencia Artificial e Innovación (CIAI - UTS)** generar el archivo `.tex` con el avance semanal de su proyecto usando un chat de IA (ChatGPT, Gemini, Claude) y validarlo en **Overleaf** antes de entregarlo.

---

## Flujo de entrega (4 pasos)

1. **Pasa el contexto al chat de IA:** Abre un chat nuevo, adjunta el archivo `SKILL.md` de esta carpeta (o pega el prompt completo de abajo) junto con los datos de tu proyecto.
2. **Guarda el código generado:** Copia el bloque LaTeX que te entregue el chat y guárdalo como `nombre_proyecto.tex` (en minúsculas, sin espacios ni tildes, por ejemplo: `vision_artificial_uts.tex`).
3. **Verifica en Overleaf que compile sin errores:**
   - Abre el proyecto del boletín en Overleaf.
   - Sube tu archivo dentro de la carpeta `proyectos/` (ejemplo: `proyectos/vision_artificial_uts.tex`).
   - En `main.tex`, dentro de la sección `\section{Proyectos CIAI}`, agrega temporalmente `\input{proyectos/vision_artificial_uts}` y haz clic en **Recompile**.
   - Comprueba que la descripción, la tabla de rúbrica y la gráfica de `0%` a `100%` se vean bien en dos columnas y no haya errores rojos de compilación.
4. **Envía tu archivo:** Una vez verificado en Overleaf, envía únicamente tu archivo `nombre_proyecto.tex` al editor del boletín. **No modifiques `referencias.bib`.**

---

## Prompt listo para copiar y pegar en el chat de IA

Si no vas a adjuntar `SKILL.md` directamente, copia todo este bloque en tu chat de IA, rellena los datos de tu proyecto al final y envíalo:

```text
Actúa siguiendo estrictamente las reglas del skill `ciai-proyecto-latex` para el Boletín Semanal del CIAI (UTS).

Genera únicamente el bloque de código LaTeX autocontenido para guardar como `proyectos/nombre_proyecto.tex` (SIN `\documentclass`, SIN `\begin{document}`, SIN `\bibliography` y SIN agregar entradas a `referencias.bib`).

El archivo debe contener exactamente estos 4 elementos en orden:
1. `\subsection{Nombre del Proyecto}`, los integrantes y `\url{enlace_del_proyecto}` (GitHub, página web, demo o documentación).
2. `\paragraph{Descripción del proyecto}`: un párrafo breve (80 a 120 palabras) explicando el objetivo, el problema que resuelve, las tecnologías o modelos de IA empleados y su impacto esperado en la UTS o la región.
3. `\paragraph{Avances y rúbrica de seguimiento}`: un resumen breve del logro semanal seguido de una tabla `tabularx` de ancho `\columnwidth` (columnas: Criterio / Hito, Evidencia observable de cumplimiento, Estado) y una etiqueta única `\label{tab:rubrica_nombre_proyecto}`.
4. La llamada a la macro `\graficaavanceproyecto{Título corto}{Hito4,Hito3,Hito2,Hito1}{{(Hito4, %) (Hito3, %) (Hito2, %) (Hito1, %)}}` con los porcentajes de 0 a 100 exactos de mi rúbrica. Los nombres de los hitos deben coincidir letra por letra en ambos argumentos y ordenarse de abajo hacia arriba (el último hito va primero).

Usa espacios ASCII estándar y escapa caracteres especiales de LaTeX (`\%`, `\_`, `\&`, `\$`).

Aquí están los datos de mi proyecto esta semana:
- Nombre del proyecto: [ESCRIBE AQUÍ]
- Integrantes: [ESCRIBE AQUÍ]
- Enlace (GitHub / web / demo): [ESCRIBE AQUÍ]
- Descripción breve: [ESCRIBE AQUÍ]
- Hitos de mi rúbrica, evidencia y porcentaje de avance (0% a 100%):
  1. [Hito 1]: [Evidencia] -> [X]%
  2. [Hito 2]: [Evidencia] -> [X]%
  3. [Hito 3]: [Evidencia] -> [X]%
  4. [Hito 4]: [Evidencia] -> [X]%
```

---

## Checklist rápido antes de enviar tu `.tex`

- [ ] El archivo se llama `nombre_proyecto.tex` (todo en minúsculas, separado por guiones bajos).
- [ ] Incluye el enlace (`\url{...}`) de tu repositorio, página web o demo.
- [ ] Tiene los 3 bloques completos: **Descripción**, **Rúbrica de avances** y **Gráfica `0%` a `100%`**.
- [ ] Compiló limpio en **Overleaf** dentro de `main.tex` sin errores de `pgfplots` ni tablas desbordadas.
