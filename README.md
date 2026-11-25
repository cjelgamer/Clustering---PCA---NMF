# Actividad 05: Clustering, PCA y NMF

Universidad Nacional del Altiplano · Ingeniería de Sistemas · Aprendizaje de Máquina (IX ciclo, grupo B)
Docente: Ing. Mayenka Fernandez Chambi

**Integrantes:** Cunurana Calderon Cristian Junior, Llusca Surco Miguel Angel y Yucra Mendoza Maria Vanesa.

## Objetivo
Aplicar clustering jerárquico a textos, reconocimiento Eigenfaces mediante PCA y recomendación temática usando NMF.

## Casos

1. **Clustering jerárquico:** 20 Newsgroups, TF-IDF, TruncatedSVD y comparación de Ward, complete, average y single. Complete obtuvo el ARI más alto (0.136); Ward obtuvo mayor silhouette (0.071).
2. **Eigenfaces:** Olivetti Faces, PCA y 1-NN; 25 componentes elegidos en validación y accuracy de prueba 0.912.
3. **Recomendación:** NMF sobre TF-IDF de 20 Newsgroups, con 10 temas elegidos por MAP@5 (0.226). TF-IDF directo logró MAP@5 0.361 en las mismas consultas.

## Datasets
Se descargan por `sklearn.datasets.fetch_20newsgroups` y `fetch_olivetti_faces`; no se almacenan en el repositorio. 20 Newsgroups es un corpus de foros de discusión usado como aproximación temática a artículos; Olivetti contiene 400 imágenes de 40 identidades.

## Tecnologías
Python 3.14, NumPy, Pandas, Matplotlib, Seaborn, SciPy, scikit-learn, Jupyter y nbconvert.

## Ejecución local

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=1800 Actividad_05_Clustering_PCA_NMF.ipynb
```

La primera ejecución descarga los datasets públicos. En Google Colab, abra el notebook con el enlace **Open in Colab**; el notebook usa rutas relativas y no depende de Drive.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://github.com/cjelgamer/Clustering---PCA---NMF/blob/main/Actividad_05_Clustering_PCA_NMF.ipynb)

## Resultados e informe

- [Informe PDF](main.pdf)
- [Notebook ejecutado](Actividad_05_Clustering_PCA_NMF.ipynb)

Las figuras y métricas CSV/JSON por caso se encuentran en `resultados/`. Los tiempos se incluyen en los CSV y varían con el equipo.

## Estructura

```text
Actividad_05_Clustering_PCA_NMF.ipynb
main.pdf
README.md
requirements.txt
resultados/
  01_clustering_jerarquico/ (métricas, contingencia, palabras y figuras)
  02_pca_eigenfaces/       (métricas y figuras)
  03_nmf_recomendacion/    (métricas, temas y figuras)
```
