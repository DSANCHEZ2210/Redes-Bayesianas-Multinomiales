# Redes-Bayesianas-Multinomiales

# Artículo 1: Redes Bayesianas Multinomiales (EOD INEGI)

> Análisis de la Encuesta Origen–Destino en Hogares (INEGI, ZMVM 2017) mediante **redes bayesianas** (DAG), con limpieza de datos en **R** y **Python**, selección de modelos por **BIC** y aprendizaje estructural con **Hill Climbing**.

---

## Objetivo

- Integrar y depurar tablas de la EOD (vivienda, hogar, sociodemográficos, viajes y transporte).
- Modelar dependencias entre variables con **DAGs** manuales y con estructura aprendida.
- Comparar estructuras con **BIC** y responder **queries** de probabilidad condicional relevantes para movilidad.

---

## Estructura del repositorio (sugerida)

.
├── Articulo.qmd                 # Documento principal (Quarto)
├── Articulo.pdf                 # Render PDF del artículo
├── Limpieza_Selección.qmd       # Limpieza y ensamblado de dataset
├── df_final_r.csv               # Dataset intermedio (R)
├── df_final_py.csv              # Dataset final (Python)
├── img/                         # Figuras (modelo relacional, etc.)
├── .gitignore                   # Ignora artefactos de build/datos locales
└── .venv/                       # (opcional) entorno virtual de Python local

> Si no deseas versionar `df_final_*.csv` o `.venv/`, mantenlos en `.gitignore`.

---

## Requisitos

### R
- `R >= 4.2`
- Paquetes: `readr`, `dplyr`, `tidyr`, `bnlearn`, `here`  
  *(opcional para graficar DAGs)* `Rgraphviz`
- Para PDF (opcional): `tinytex`

Instalación en R:
r
install.packages(c("readr","dplyr","tidyr","bnlearn","here"))
# PDF:
install.packages("tinytex")
tinytex::install_tinytex()
# Graficar DAGs:
install.packages("BiocManager")
BiocManager::install("Rgraphviz")

Python
	•	Python 3.10+
	•	Paquetes: pandas (y opcionalmente numpy)

Con entorno local:

python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install pandas numpy

Si usas reticulate:

library(reticulate)
use_python(normalizePath(".venv/bin/python"), required = TRUE)
py_config()


⸻

 Cómo reproducir
	1.	Clonar:

git clone https://github.com/DSANCHEZ2210/Redes-Bayesianas-Multinomiales.git
cd Redes-Bayesianas-Multinomiales


	2.	(Opcional) Preparar Python:

python3 -m venv .venv
source .venv/bin/activate
python -m pip install pandas numpy


	3.	Abrir en RStudio (ideal con .Rproj).
	4.	Renderizar:
	•	HTML:

quarto render Articulo.qmd --to html


	•	PDF (con LaTeX instalado):

quarto render Articulo.qmd --to pdf

Si hay errores de fuentes con XeLaTeX:

tinytex::tlmgr_install(c("tex-gyre","inconsolata"))



⸻

 Flujo de trabajo
	1.	Limpieza y ensamblado (R)
Selección de columnas por tabla (tvivienda, thogar, tsdem, tviaje, ttransporte), renombrado legible, joins (left_join) por claves (id_soc, id_hog, id_viv, id_via), exportación df_final_r.csv.
	2.	Transformaciones (Python)
Cálculo de duracion_viaje_min a partir de horas/minutos de inicio y fin; eliminación de columnas originales; exportación df_final_py.csv.
	3.	Preparación para BN (R)
Segunda limpieza (remover variables redundantes e IDs), renombrado corto NP, O, D, S, E, NE, T, DV; discretización de DV a binaria (DV60 → DV).
	4.	Modelado BN (R)
Tres DAGs manuales; aprendizaje estructural con Hill Climbing; comparación por BIC; ajuste de parámetros con bn.fit; queries con cpquery.

⸻

 Resultados (resumen)
	•	BIC: la DAG 2 mejor que DAG 1 y 3; el Hill Climbing mejora aún más el score (estructura aprendida).
	•	Queries (ejemplos con cpquery):
	1.	Prob. de usar transporte público (metro/autobús) con hogares de 2–5 personas → baja.
	2.	Prob. de viaje hogar→trabajo > 60 min si usa coche → casi nula.
	3.	Prob. de mujer use colectivo/micro dado estrato alto → baja.
	4.	Prob. de que personas con educación alta usen transporte público → ~37% (la mayor entre las 4 consultas).

Interpretaciones dependen de codificaciones, niveles y discretización aplicada.

⸻

 Datos
	•	Fuente: INEGI — Encuesta Origen-Destino en Hogares (ZMVM 2017).
	•	Tablas: tvivienda.csv, thogar.csv, tsdem.csv, tviaje.csv, ttransporte.csv.

Los CSV pueden no estar incluidos por tamaño/licencia. Ajusta rutas o agrega instrucciones de descarga si es necesario.

⸻

 .gitignore recomendado

# Sistemas / IDE
.DS_Store
.RDataTmp
.Rhistory
.Rproj.user/
*.Rproj

# Quarto / R Markdown / LaTeX
*_files/
*.html
*.md
*.log
*.aux
*.out
*.toc
*.tex

# Datos locales
*.csv

# Python
.venv/
__pycache__/

Si desean versionar algún CSV (p. ej., df_final_py.csv), quiten *.csv y listen explícitamente cuáles no subir.

⸻

 Contribuir
	1.	Crear rama:

git switch -c tu-usuario/feature-descripcion


	2.	Commits pequeños y claros.
	3.	Pull Request a main con descripción y evidencia (figuras/outputs si aplica).

⸻

 Referencias
	•	Koller, D. & Friedman, N. Probabilistic Graphical Models: Principles and Techniques. MIT Press, 2009.
	•	Pearl, J. “The Do-Calculus Revisited.” UAI Keynote, 2012.
	•	Ma, T.Y. et al. “Bayesian Networks for Multimodal Mode Choice Behavior.” Transportation Research Procedia, 2015.

⸻

 Autores
	•	Jose Angel Govea Garcia
	•	Diego Vértiz Padilla
	•	Daniel Sánchez Fortiz
	•	Augusto Ley Rodriguez

