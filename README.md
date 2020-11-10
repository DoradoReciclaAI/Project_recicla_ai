# Projecto Saturday AI: Recicla-IA!

## Nombre detallado del proyecto:
Sistema para clasificación de Residuos Sólidos Urbanos

## Impacto social principal:
Medio ambiente

## Descripción del problema específico:
En América Latina, debido a la falta de cultura en la separación de los Residuos Sólidos Urbanos (RSU), solamente es posible reciclar el 30% de la basura generada [1].

## Hipótesis:
Al realizar un sistema de fácil implementación que permitiera clasificar de manera correcta los RSU, este representaría la base para la integración de sistemas automáticos de separación de basura, generando un incremento en el porcentaje de basura potencialmente reciclable de hasta el 92% [1].

## Población objetivo: 
Estudiantes y desarrolladores de tecnología interesados en la elaboración de sistemas robóticos que permitan una separación efectiva de RSU.

## Descripción de las fuentes de información:
1. TrashNet
    
    a. Fuente definitiva
    
    b. Datos abiertos: https://github.com/garythung/trashnet
  
    c. Set de datos de 2527 imágenes y 6 clasificaciones
      
      1. 501 vidrio
      2. 594 papel
      3. 403 cartón
      4. 482 plástico
      5. 410 metal
      6. 137 Basura
  
    d. Tiene un artículo de investigación anexo: http://cs229.stanford.edu/proj2016/poster/ThungYang-ClassificationOfTrashForRecyclabilityStatus-poster.pdf

2. Kaggle: Waste Classification data
    
    a. Fuente definitiva
    
    b. Datos abiertos: https://www.kaggle.com/techsash/waste-classification-data
    
    c. Set de datos de 22,500 imágenes con dos clasificaciones
        
      1. Orgánico
      2. Inorgánico
    
    d. Tiene Notebooks de referencia relacionados: https://www.kaggle.com/techsash/waste-classification-data/notebooks
    

## Descripción de la solución

Realización de una API desarrollada por medio de Amazon Web Services (AWS), la cual reciba como entrada una imágen de un Residuo Sólido Urbano, y genere como salida la clasificación de dicho residuo, así como su localización en la imágen, de tal manera que permita una fácil implementación en proyectos con sistemas robóticos enfocados en la separación automatizada de basura.
    Así mismo, con respecto al modelo de Machine Learning realizado, se planea que este se base en una arquitectura de Red Neuronal Convolucional, debido a los resultados favorables que estas han presentado en el procesamiento y clasificación de imágenes. Basándonos en el modelo presentado en [2], se planea desarrollar una estructura de Red Neuronal.
    
Finalmente para la implementación final del proyecto, se realizará una integración en un sistema robótico simple, controlado por Arduino y Raspberry Pi, el cual capture imágenes de la basura a separar, la procese generando la solicitud a la API desarrollada, y, dependiendo de la categoría obtenida, haga la separación correspondiente, posicionándolo en el bote adecuado.

## ¿Quiénes podrán utilizarlo?
Cualquier persona con conocimientos suficientes en desarrollo web y uso de APIs, que sean capaces de realizar solicitudes a la API con las imágenes que deseen clasificar, y procesar las respuestas obtenidas.
    Dicho proyecto está enfocado en facilitar el acceso a la población en general a tecnologías de separación automática de basura, esto brindando una herramienta de clasificación accesible por cualquier persona que se encuentre en el desarrollo de un proyecto tecnológico de este tipo, con lo cual se espera reducir la barrera que representa el desarrollo de modelos de Inteligencia Artificial, permitiendo que dichos entusiastas enfoquen sus esfuerzos en la integración del modelo ya entrenado con diversas arquitecturas de sistemas robóticos.
    
## Referencias
[1] Moyer, E. (2018). Día del Reciclaje: ¿Qué tanto se recicla en América Latina?. [En línea]. Recuperado el 9 de Octubre del 2020 de https://www.nrdc.org/es/experts/erika-moyer/dia-reciclaje-tanto-recicla-america-latina#:~:text=Seg%C3%BAn%20estad%C3%ADsticas%20del%20Banco%20Mundial,menos%20430%2C000%20toneladas%20de%20basura.&text=Si%20la%20basura%20se%20separara,posible%20reciclar%20el%2030%20porciento.

[2] Bircanoğlu, K. et. al. (2018). RecycleNet: Intelligent Waste Sorting Using Deep Neural Networks. Recuperado el 13 de Octubre del 2020 de https://www.researchgate.net/publication/325626219_RecycleNet_Intelligent_Waste_Sorting_Using_Deep_Neural_Networks


