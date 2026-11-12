# Laboratorio 14: comunidades en grafos KNN de imágenes H&M

Detección de comunidades con Louvain sobre grafos KNN construidos a partir de 2000 imágenes del catálogo H&M (diez tipos de prenda, 200 por tipo). Se comparan dos representaciones (píxeles de 32 × 32 × 3 y embeddings de ResNet-50), se contrastan las comunidades con el catálogo y con K-Means, y se proyecta el grafo con Node2Vec. Curso Data Mining, UTEC, semana 14.

**Visualización interactiva:** https://ricardoamiel.github.io/Grafo-KNN/

## Integrantes (Grupo 4)

1. Ricardo Amiel Acuña Villogas.
2. Juan Leibniz Aquino Espinoza.
3. Camilo Ernesto Soto Cristóbal.

## Resultados principales

| Representación | k (codo) | Comunidades | Q | ARI tipo | NMI tipo | NMI color |
|:--|:-:|:-:|:-:|:-:|:-:|:-:|
| CNN ResNet-50 | 10 | 12 | 0.836 | 0.739 | 0.796 | 0.094 |
| Píxeles | 3 | 25 | 0.813 | 0.164 | 0.347 | 0.168 |

1. Las comunidades CNN reproducen el tipo de prenda (pureza ponderada de 0.888) y casi no dependen del color. Las de píxeles se ordenan por luminosidad y encuadre.
2. La CNN también agrupa por material y punto de vista: aparece una comunidad de denim que junta shorts y jeans, y Sneakers se divide entre fotos de perfil y fotos desde arriba.
3. El k se elige con el método del codo sobre el NMI con el ground truth (umbral de 2 % de ganancia relativa), no con el máximo.

## Contenido

| Ruta | Descripción |
|:--|:--|
| Lab14_Grafo_KNN_HM.ipynb | Notebook ejecutado con las Partes 1 a 4 y las respuestas |
| Informe_Lab14_Grafo_KNN_HM.docx | Informe de 6 páginas con figuras, tablas y análisis |
| docs/index.html | Visualización D3 v7 publicada en GitHub Pages (paneles enlazados, tooltip con imagen y selección de región) |
| viz/index.html | La misma visualización como entregable autocontenido |
| figs | Nueve figuras en PNG y PDF |

## Reproducir

Entorno: Python 3.12 con torch y torchvision (CPU), networkx, scikit-learn, gensim y huggingface_hub.

1. Descargar el catálogo en esta carpeta:

        python -c "from huggingface_hub import hf_hub_download; import shutil; shutil.copy(hf_hub_download('einrafh/hnm-fashion-recommendations-data', 'data/raw/articles.csv', repo_type='dataset'), 'articles.csv')"

2. Ejecutar el notebook de principio a fin. La primera ejecución descarga unas 2400 imágenes en data; cada ejecución calcula los embeddings, los layouts y Node2Vec (unos 8 minutos en CPU) y vuelve a escribir las figuras en figs. Todas las semillas están fijas, así que los resultados se reproducen exactamente.

La visualización (viz/index.html y su copia publicada docs/index.html) y el informe ya están construidos en el repositorio con los resultados de esta ejecución.

## Publicar en GitHub Pages

En el repositorio, ir a Settings, luego Pages, elegir Deploy from a branch, rama main y carpeta /docs. El archivo docs/.nojekyll hace que Pages sirva el HTML sin procesarlo. La página carga D3 desde cdnjs y lleva los datos y las miniaturas incrustados, así que no necesita nada más.

## Datos

H&M Personalized Fashion Recommendations, versión de Hugging Face einrafh/hnm-fashion-recommendations-data. Las imágenes y el CSV del catálogo no se versionan: se descargan con los pasos anteriores.
