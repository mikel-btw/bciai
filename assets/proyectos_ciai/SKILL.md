---
name: ciai-proyecto-latex
description: Genera el archivo .tex autocontenido en formato IEEEtran de dos columnas con el avance semanal de un proyecto para el Centro de Inteligencia Artificial e Innovacion (CIAI - UTS). Usa este skill cuando un investigador o equipo quiera redactar, formatear o actualizar el reporte LaTeX de su proyecto (titulo, integrantes, enlace del proyecto, descripcion, rubrica de avances y grafica de progreso 0% a 100%) para compilarlo directamente en Overleaf y entregarlo al editor del boletin.
---

# CIAI Proyecto LaTeX

Este skill guia a cualquier agente o chat de LLM para generar un archivo `.tex` completo y compilable directamente en Overleaf en formato `IEEEtran` de dos columnas (`conference`, `twocolumn`, `spanish`), listo para que el equipo del Centro de Inteligencia Artificial e Innovacion (CIAI - UTS) verifique su renderizado y lo entregue al editor del Boletin Semanal.

## Cuando usar este skill

- Cuando un miembro o equipo del CIAI necesite crear o actualizar su reporte semanal de proyecto en LaTeX.
- Cuando se pida convertir notas de avance, rubricas o porcentajes de progreso en un documento `.tex` compilable en Overleaf bajo la plantilla oficial del CIAI.
- Cuando se requiera generar la grafica `pgfplots` de avance de `0%` a `100%` usando la macro `\graficaavanceproyecto`.

## Estructura obligatoria del archivo `.tex`

El archivo generado debe ser un documento LaTeX completo y autocontenido (con preambulo `IEEEtran` y la macro `\graficaavanceproyecto` incluida) con los siguientes elementos en este orden exacto:

1. **Encabezado IEEE (`\title` y `\author`):** Titulo del proyecto, integrantes del equipo, afiliacion `Centro de Inteligencia Artificial e Innovación (CIAI) -- UTS` y enlace directo del proyecto (`\url{...}` a GitHub, pagina web, demo o documentacion). No se usa `referencias.bib`.
2. **Descripcion del proyecto (`\section{Descripción del proyecto}`):** Un parrafo breve (`80-120` palabras) con el objetivo, problema que resuelve, tecnologias o modelos de IA empleados e impacto esperado en la UTS o la region.
3. **Rubrica de avances (`\section{Avances y rúbrica de seguimiento}`):** Resumen breve del logro semanal seguido de una tabla `tabularx` de ancho `\columnwidth` con los hitos definidos por el propio equipo, evidencia observable de cumplimiento y estado.
4. **Grafica de avance (`0%` a `100%`):** Invocacion a la macro `\graficaavanceproyecto{Titulo corto}{HitoN,...,Hito1}{{(HitoN, %) ... (Hito1, %)}}`.

## Pasos de ejecucion

1. **Recopilar datos del proyecto:** Si el usuario no los ha dado completos, solicita de forma breve:
   - Nombre del proyecto e integrantes.
   - Enlace principal del proyecto (GitHub, web, Hugging Face o demo).
   - Descripcion breve de que hace el proyecto.
   - Hitos o criterios de su rubrica propia, evidencia actual y porcentaje de avance (`0` a `100`) de cada hito.
2. **Validar consistencia entre rubrica y grafica:**
   - Verifica que los nombres de los hitos en el segundo argumento de `\graficaavanceproyecto` coincidan letra por letra con los pares `(Hito, valor)` del tercer argumento.
   - Ordena los hitos de abajo hacia arriba en `\graficaavanceproyecto` (el ultimo hito del proyecto va primero en la lista para que el hito inicial aparezca arriba en el eje Y).
   - Asegura que los porcentajes esten en el rango entero o decimal `0` a `100` y correspondan coherentemente con el estado reportado en la tabla (`Completado` = `100`, `En progreso` = `1-99`, `Pendiente` = `0`).
3. **Formatear en LaTeX seguro para `IEEEtran`:**
   - Usa espacios ASCII estandar (nunca espacios duros no separables).
   - Escapa caracteres especiales de LaTeX en texto (`\%`, `\_`, `\&`, `\$`).
   - Usa una etiqueta de tabla unica por proyecto (`\label{tab:rubrica_nombre_proyecto}`).
4. **Entregar el archivo listo para Overleaf:** Presenta el bloque `.tex` completo indicando el nombre exacto de archivo (`nombre_proyecto.tex`) e instruye al usuario para que lo compile en Overleaf antes de enviarlo al editor del boletin.

## Plantilla base de salida

```latex
\documentclass[conference,twocolumn,spanish]{IEEEtran}

\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[spanish,es-tabla]{babel}
\usepackage{booktabs}
\usepackage{tabularx}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usepackage{hyperref}

\newcommand{\graficaavanceproyecto}[3]{%
\begin{figure}[htbp]
\centering
\begin{tikzpicture}
\begin{axis}[
    xbar,
    width=0.92\columnwidth,
    height=3.6cm,
    bar width=7pt,
    xlabel={Porcentaje de avance (\%)},
    xmin=0,
    xmax=114,
    xtick={0,25,50,75,100},
    symbolic y coords={#2},
    ytick=data,
    tick label style={font=\scriptsize},
    label style={font=\scriptsize},
    xmajorgrids=true,
    grid style={dashed, gray!35},
    nodes near coords={\pgfmathprintnumber{\pgfplotspointmeta}\%},
    every node near coord/.append style={font=\tiny, anchor=west}
]
\addplot[fill=green!55!black, draw=green!70!black] coordinates {#3};
\end{axis}
\end{tikzpicture}
\caption{Porcentaje de avance por hitos: #1.}
\end{figure}
}

\title{Nombre del Proyecto CIAI}

\author{
  \IEEEauthorblockN{Nombre Apellido 1, Nombre Apellido 2}
  \IEEEauthorblockA{
    Centro de Inteligencia Artificial e Innovación (CIAI) -- UTS\\
    Enlace del proyecto: \url{https://github.com/usuario/proyecto-o-pagina}
  }
}

\begin{document}

\maketitle

\section{Descripción del proyecto}
Redacte aquí en un párrafo breve (80--120 palabras) el objetivo del proyecto, el problema que resuelve, las tecnologías o modelos de IA empleados y su impacto esperado en la UTS o la región.

\section{Avances y rúbrica de seguimiento}
Resuma el avance logrado durante la semana y complete la tabla con los criterios definidos por el equipo:

\begin{table}[htbp]
\centering
\caption{Rúbrica de avance del proyecto.}
\label{tab:rubrica_nombre_proyecto}
\scriptsize
\begin{tabularx}{\columnwidth}{>{\raggedright\arraybackslash}p{2.2cm} >{\raggedright\arraybackslash}X c}
\toprule
\textbf{Criterio / Hito} & \textbf{Evidencia observable de cumplimiento} & \textbf{Estado} \\
\midrule
Datos & Dataset limpio, etiquetado y verificado al 100\,\%. & Completado \\
Modelo & Prototipo entrenado y evaluado sobre conjunto de prueba. & En progreso \\
Interfaz & API o aplicación web funcional para demostración. & Pendiente \\
Validación & Pruebas con usuarios piloto y documentación final. & Pendiente \\
\bottomrule
\end{tabularx}
\end{table}

\graficaavanceproyecto{Nombre Corto del Proyecto}{Validación,Interfaz,Modelo,Datos}{
    (Validación, 0)
    (Interfaz, 20)
    (Modelo, 75)
    (Datos, 100)
}

\end{document}
```

## Errores comunes a evitar

- **No agregar entradas a `referencias.bib` ni `\bibliography`:** La unica referencia del proyecto es su enlace `\url{...}` dentro del bloque `\author`.
- **No desordenar los ejes de `\graficaavanceproyecto`:** Si un hito en `{Hito4,Hito3,Hito2,Hito1}` no aparece exactamente igual en `{(Hito4,0) ...}`, `pgfplots` lanzara un error de coordenada simbolica no definida.
- **No repetir `\label{tab:rubrica}`:** Siempre personaliza el `\label` con el identificador del proyecto (`\label{tab:rubrica_slug_proyecto}`).
