# Actividad 05: Clustering jerárquico, PCA y NMF

**Universidad Nacional del Altiplano · Ingeniería de Sistemas · Aprendizaje de Máquina, IX ciclo, grupo B**
**Docente:** Ing. Mayenka Fernandez Chambi
**Integrantes:** Cunurana Calderon Cristian Junior, Llusca Surco Miguel Angel y Yucra Mendoza Maria Vanesa.

## Propósito

Aplicamos tres técnicas de aprendizaje no supervisado a problemas distintos: descubrir agrupaciones temáticas en textos, reconocer identidades faciales conocidas y recomendar documentos de contenido similar. El proyecto busca comparar las técnicas con experimentos reproducibles y discutir qué indican —y qué no indican— sus resultados.

## Qué hicimos y por qué

### 1. Clustering jerárquico: 20 Newsgroups

- Usamos `fetch_20newsgroups` con encabezados, firmas y citas removidos para que la agrupación dependiera más del contenido que del formato del mensaje.
- Transformamos los textos en TF-IDF (vocabulario limitado a 3 000 términos), reducimos a 80 dimensiones con TruncatedSVD y normalizamos los vectores. El clustering jerárquico de todo el train sería costoso; por eso comparamos los cuatro linkage en una muestra estratificada reproducible de 12 documentos por categoría (240 en total), manteniendo 20 clusters.
- Comparamos Ward, complete, average y single con distancia euclidiana. Las etiquetas no se usaron para ajustar los modelos: se usaron después para ARI y NMI. Silhouette, Calinski–Harabasz y Davies–Bouldin describen estructura interna.
- **Conclusión:** complete tuvo el mayor ARI (**0.135930**), apenas por encima de Ward (**0.135616**). Ward logró mejor NMI (**0.470**) y silhouette (**0.071**). La diferencia entre complete y Ward es estrecha, así que el enlace preferido depende del criterio; single tuvo el desempeño más débil en ARI y NMI.

### 2. Reconocimiento facial: PCA y Eigenfaces

- Usamos Olivetti Faces: 400 imágenes en escala de grises, 40 personas, 10 imágenes por identidad y resolución de 64 × 64.
- Separamos por identidad seis imágenes para entrenamiento, dos para validación y dos para prueba. Ajustamos PCA solo con train; elegimos componentes en validación y reservamos test para el resultado final. Clasificamos con 1-NN euclidiano, sin redes neuronales.
- **Conclusión:** 25 componentes dieron el mejor resultado de validación entre las opciones probadas. En prueba obtuvimos accuracy **0.9125**, precision macro **0.9167**, recall macro **0.9125** y F1 macro **0.9083**. El sistema reconoce entre identidades conocidas; no tiene un mecanismo para rechazar una persona desconocida.

### 3. Recomendación temática: NMF

- Utilizamos los documentos de train de 20 Newsgroups como candidatos y consultas del split test. Es un corpus de foros de discusión usado como aproximación experimental a artículos, no un conjunto de noticias editoriales.
- Ajustamos TF-IDF en train y NMF sobre la matriz no negativa, explorando 5, 10, 15, 20 y 30 temas. La función recomienda documentos por coseno en el espacio documento-tema; evaluamos 60 consultas. La coincidencia de categoría se tomó como proxy de relevancia, y comparamos con coseno directo sobre TF-IDF en las mismas consultas.
- **Conclusión:** NMF con 10 temas obtuvo la mejor MAP@5 entre las configuraciones (**0.2257**; precision@5 **0.3033**). El baseline TF-IDF fue superior (**MAP@5 0.3611**, precision@5 **0.4267**). NMF facilita interpretar temas, pero en este experimento la reducción no mejoró la recuperación temática. El menor error de reconstrucción, por sí solo, tampoco seleccionó el mejor recomendador.

## Conclusión general

Cada modelo responde a un objetivo diferente. El clustering no produjo un ganador indiscutible entre todas las métricas; Eigenfaces obtuvo buen reconocimiento en una colección pequeña y controlada de personas conocidas; NMF ofreció temas interpretables, pero TF-IDF recuperó mejor documentos de la misma categoría. Las métricas de recomendación son aproximaciones temáticas, no valoraciones de usuarios o editores. Los resultados deben interpretarse con esas condiciones en mente.

## Datasets y herramientas

- **20 Newsgroups:** `sklearn.datasets.fetch_20newsgroups` (scikit-learn).
- **Olivetti Faces:** `sklearn.datasets.fetch_olivetti_faces` (scikit-learn).
- Python 3.14, NumPy, Pandas, Matplotlib, Seaborn, SciPy, scikit-learn, Jupyter y nbconvert.

Los datasets se descargan durante la ejecución y no se guardan en este repositorio. El notebook no usa rutas absolutas, Google Drive ni archivos privados.

## Ejecución

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=1800 Actividad_05_Clustering_PCA_NMF.ipynb
```

Al ejecutarlo por primera vez, scikit-learn descarga los datasets. También se puede abrir el notebook directamente en Google Colab:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://github.com/cjelgamer/Clustering---PCA---NMF/blob/main/Actividad_05_Clustering_PCA_NMF.ipynb)

## Contenido del repositorio

```text
Actividad_05_Clustering_PCA_NMF.ipynb   # notebook ejecutado con salidas
README.md                                # decisiones, resultados y conclusiones
requirements.txt
resultados/
  01_clustering_jerarquico/              # métricas, contingencia y palabras por cluster
  02_pca_eigenfaces/                     # métricas por componentes y evaluación final
  03_nmf_recomendacion/                  # métricas, temas y comparación TF-IDF
  resumen_general.json
recursos/qr_github.png
```

Las figuras PNG de alta resolución, `main.tex` y el informe PDF se conservan localmente; el PDF no forma parte de este repositorio.
