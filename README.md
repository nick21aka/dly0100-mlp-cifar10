MLP para Clasificación de Imágenes en CIFAR-10
Evaluación Parcial N°1 — Fundamentos de Deep Learning

📋 Información del proyecto
Campo	Valor
Asignatura	DLY0100 — Deep Learning
Institución	Duoc UC
Estudiante	Nicolas (trabajo individual)
Dataset	CIFAR-10 (Universidad de Toronto)
Framework	TensorFlow 2.x / Keras
Tipo de problema	Clasificación multiclase de imágenes (10 categorías)
🎯 Resumen ejecutivo
Este proyecto implementa una red neuronal artificial multicapa (MLP) para clasificar imágenes 32×32 RGB del dataset CIFAR-10 en 10 categorías (avión, auto, pájaro, gato, ciervo, perro, rana, caballo, barco, camión).

Resultados principales
Configuración	Test accuracy	Notas
MLP Base (sin regularización)	48.13%	Línea base
MLP Pro (BN + Dropout + L2 + Noise + AdamW)	57.17%	+9.04 pts vs base
🌟 ENSEMBLE (3 modelos diversos, soft voting)	59.33%	+11.20 pts vs base
K-fold 5-fold CV	49.44% ± 0.55%	Robustez confirmada
Métricas finales del ensemble
Métrica	Valor
Accuracy	59.33%
Precision (macro)	59.43%
Recall (macro)	59.33%
F1-score (macro)	59.31%
Top-3 accuracy	87.27%
Top-5 accuracy	95.69%
🛠️ Cómo ejecutar el proyecto
Opción 1: Google Colab (recomendado)
Abre Google Colab.
Sube el archivo MLP_CIFAR10_DLY0100.ipynb.
Activa GPU: Entorno de ejecución → Cambiar tipo de entorno → T4 GPU.
Sube el dataset cifar-10-python.tar.gz al panel de archivos de Colab.
Ejecuta todas las celdas: Entorno de ejecución → Ejecutar todo.
Tiempo estimado: ~1.5-2 horas con GPU.
El dataset original se puede descargar desde: https://www.cs.toronto.edu/~kriz/cifar.html

Opción 2: Local
# Crear entorno virtual
python -m venv venv
source venv/bin/activate

# Instalar dependencias
pip install tensorflow numpy pandas matplotlib seaborn scikit-learn jupyter nbformat

# Ejecutar
jupyter notebook MLP_CIFAR10_DLY0100.ipynb
📁 Estructura del repositorio
.
├── MLP_CIFAR10_DLY0100.ipynb            # Cuaderno principal (informe técnico)
├── Presentacion_DLY0100_Nicolas.pptx    # Presentación de defensa (10 min)
└── README.md                             # Este archivo
🧪 Metodología
El cuaderno está organizado en 14 secciones que cubren todos los criterios de la rúbrica:

Introducción — descripción del problema y objetivos
Carga y preprocesamiento — método dual: pickle manual + Keras + verificación
Modelo MLP base — línea base sin regularización
Entrenamiento base — identificación de overfitting
Experimentos hiperparámetros — LR, batch, profundidad, anchura
Comparación de funciones — activación (4), error (3), salida (2)
Comparación de optimizadores — SGD+momentum, RMSprop, Adam, AdamW
Optimización combinada — BN + Dropout + L2 + GaussianNoise
Validación cruzada — 5-fold CV
Ensemble — 3 modelos diversos con soft voting
Métricas — accuracy, precision, recall, F1, Top-K
Análisis de errores — matriz de confusión normalizada
Comparación final — 6 configuraciones tabuladas
Conclusiones — hallazgos y limitaciones del MLP
Decisiones técnicas clave
Decisión	Valor elegido	Justificación
Framework	TensorFlow/Keras	Sintaxis legible, ideal para defensa oral
Optimizador final	AdamW (lr=0.001, wd=1e-4)	Weight decay desacoplado
Batch size	128	Balance velocidad/estabilidad
Activación oculta	ReLU + He-Normal init	Evita vanishing gradient
Activación salida	Softmax	Distribución de probabilidad multiclase
Loss	Categorical Crossentropy	Estándar para clasificación con one-hot
Arquitectura Pro	4 capas: 1024→512→256→128	Determinada empíricamente
Dropout	0.4 → 0.2 (decreciente)	Más regularización en capas iniciales
L2 lambda	0.0005	Calibrado para no sobre-restringir
Arquitecturas del ensemble
Los 3 modelos del ensemble usan arquitecturas distintas para inducir diversidad:

Modelo	Arquitectura	Parámetros	Test acc
1 — Profundo	1024-512-256-128	3.84M	57.17%
2 — Medio	1024-512-256	3.81M	58.09%
3 — Pirámide	2048-512-128	7.42M	57.39%
Ensemble	Soft voting (promedio de probs)	—	59.33%
📊 Hallazgos clave
El MLP base sufre overfitting evidente: la brecha train-val crece sin parar tras ~10 épocas.
La regularización combinada cierra esa brecha: BN + Dropout + L2 + Noise mejoran el accuracy en +9.04 puntos porcentuales.
El ensemble suma +2.16 pts adicionales gracias a la diversidad arquitectónica entre modelos.
AdamW supera a Adam consistentemente, gracias al weight decay desacoplado.
LeakyReLU > ReLU > Sigmoid > Tanh en este caso (confirmación empírica de la teoría).
Categorical crossentropy + softmax es la pareja correcta para clasificación mutuamente excluyente.
Las clases de animales se confunden entre sí (dog→cat 239 casos, cat→dog 178 casos) porque un MLP, al aplanar la imagen, pierde información espacial.
⚠️ Limitaciones del MLP en este problema
Un MLP trata cada pixel como una feature independiente, descartando:

Vecindad espacial (píxeles cercanos están más correlacionados)
Invariancia traslacional (un gato a la izquierda vs derecha "se ve diferente")
Patrones jerárquicos (bordes → texturas → partes → objetos)
Por esta razón, ningún MLP supera ~60% en CIFAR-10, mientras una CNN básica supera el 80%. Nuestro ensemble en 59.33% está cerca de ese límite teórico.

🚀 Próximos pasos sugeridos
Acción	Mejora esperada
Migrar a CNN (capas convolucionales 2D)	+30 pts
Data augmentation 2D	+5 pts
Transfer learning desde ResNet50/EfficientNet	+10 pts
Schedulers avanzados (CosineDecay, OneCycleLR)	+1-2 pts
📚 Referencias
TensorFlow / Keras — https://www.tensorflow.org/api_docs
CIFAR-10 dataset — https://www.cs.toronto.edu/~kriz/cifar.html
Krizhevsky, A. (2009). Learning Multiple Layers of Features from Tiny Images. University of Toronto.
Loshchilov, I. & Hutter, F. (2019). Decoupled Weight Decay Regularization (AdamW).
👤 Autor
Nicolas — Estudiante Duoc UC Fundamentos de Deep Learning (DLY0100) 2026
