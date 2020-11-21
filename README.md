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
    

# Creación y Configuración REST-API

## Despliegue Endpoint de SageMaker<a name="despliegue-enpoint"></a>
En esta sección se creará un Endpoint de inferencia de SageMaker en base a un modelo exportado previamente entrenado. La creación se la realizará mediante un Notebook con código Python.

 1. Entrar a la consola de Amazon SageMaker y en el menú Instancias de
    bloc de notas seleccionar la opción Crear instancia de bloc de
    notas.
 2. Colocar un nombre representativo y en el campo Tipo de instancia de
    bloc de notas seleccionar la opción ml.t2.medium. Deja el resto de
    configuración con la información que viene por defecto y al final da
    clic en Crear instancia de bloc de notas
 3. Espera hasta que el estatus cambie a ‘InService’ y abre el Notebook
    en JupyterLab 
 4. Importar el notebook deploy_end_point.ipynb que está
        en esta misma carpeta seleccionando el Kernel conda_tensorflow2_p36.
 5. Ejecutar el notebook siguiendo las instrucciones.
 6. En el menú lateral, seleccione la opción Inferencia, Puntos de
    Enlace se debería haber creado un end point nuevo, copie el nombre
    para luego configurar las variables de entorno de la función Lambda.

## Creación Función Lambda

En esta sección se creará la función lambda que recibirá una imágen, realizará el preprocesamiento correspondiente y llamará al Endpoint de inferencia de SageMaker. Luego retornará el resultado de dicha inferencia.

 1. Ingresar a la consola de AWS Lambda y seleccionar la opción Crear
    una función
 2. Dentro de la información básica, colocar el nombre y en lenguaje
    seleccionar Python 3.6
 3. Hacer click en el botón Crear función
 4. Dentro del combo Acciones, seleccionar la opción Cargar un archivo
    .zip y seleccionar el archivo lambda_function.zip que está en el
    drive.
 5. En la sección Variables de entorno hacer click en el botón Editar y
    luego en Agregar Variable de Entorno
 6. Agregar una variable con clave ENDPOINT_NAME y cómo valor colocar el
    nombre del enpoint de SageMaker obtenido en el punto 6 de la sección
    anterior.

## Asignación de permisos

En esta sección se asignan los permisos necesarios para que la función Lambda pueda invocar al Endpoint de inferencia.

 1. Ingresar a la consola administrativa IAM e ir a la opción
    Administración del acceso, Roles Seleccionar el rol asociado a la
    función lambda creada. El nombre del rol comienza con el nombre de
    la función y en el campo Entidades de Confianza dice Servicio de
    AWS: lambda
 2. Seleccionar la opción Añadir una política insertada
 3. En servicio seleccionar la opción SageMaker, en acciones seleccionar
    la opción InvokeEndpoint y en Recursos seleccionar Todos los
    recursos
 4. Hacer click en la opción Revisar la política
 5. Agregar el nombre correspondiente y hacer click en el botón Crear
    una política

## Creación Api Gateway
En esta sección se creará el Api Gateway que será el Api REST que recibirá las solicitudes HTTP con la imagen a clasificar y retornará en formato json la clasificación correspondiente y el porcentaje de certeza de dicha clasificaión. 

 1. Ingresar a la consola Api Gateway y seleccionar la opción Crear API.
    Seleccionar la opción Crear del tipo API REST.
 2. Ingresar el nombre elegido y presionar el botón Crear API.
 3. En el combo Acciones seleccionar la opción Crear	método.
 4. Seleccionar la opción POST y hacer click en el botón con tilde.
 5. En Tipo de integración seleccionar la opción Función Lambda.
 6. En el campo Función Lambda elegir la función creada en el paso
    anterior y hacer click en el botón Guardar.
 7. Hacer click en la sección Solicitud de integración y en Plantillas
    de mapeo.
 8. Hacer click en Agregar Plantilla de mapeo y colocar el Content-Type
    image/jpeg.
 9. Al hacer click en el botón con el tilde, aparecerá un popup con un
    alerta, seleccionar la opción No, usar configuración actual.
 10. En el cuerpo de la plantilla agregar el siguiente contenido:

    #set($inputRoot = $input.path('$'))
    {
        "json" : $input.json('$'),
    	"body" : "$util.escapeJavaScript($input.body).replaceAll("\\'", "'")",
    }

 11. Seleccionar la opción Configuración del menú izquierdo e ir a la
     sección Tipos de medios binarios.
 12. Hacer click en el botón Agregar tipo de medios binarios y colocar
     el valor image/jpeg.
 13. Seleccionar la opción Recursos del menú izquierdo y en el combo
     Acciones seleccionar la opción Implementar la API.
 14. En el combo Etapa de implementación colocar el nombre que se desee
     y hacer click en el botón Implementación.
 15. Con este paso el API REST está desplegada y la url para invocar
     aparece a continuación del texto Invocar URL.

## Invocación API REST 
En esta sección se indicará el formato del request para invocar el API REST.

 - Protocolo: HTTPS
- Método: POST
- Headers: Content-Type, image/jpeg
- Data-binary: archivo de imágen.

Ejemplo:

    curl --location --request POST 'https://4b4cj4hei4.execute-api.us-east-2.amazonaws.com/beta' \
    --header 'Content-Type: image/jpeg' \
    --data-binary '@/C:/sai/waste-sorting/imagenes/O4.JPG'
    
## Referencias
[1] Moyer, E. (2018). Día del Reciclaje: ¿Qué tanto se recicla en América Latina?. [En línea]. Recuperado el 9 de Octubre del 2020 de https://www.nrdc.org/es/experts/erika-moyer/dia-reciclaje-tanto-recicla-america-latina#:~:text=Seg%C3%BAn%20estad%C3%ADsticas%20del%20Banco%20Mundial,menos%20430%2C000%20toneladas%20de%20basura.&text=Si%20la%20basura%20se%20separara,posible%20reciclar%20el%2030%20porciento.

[2] Bircanoğlu, K. et. al. (2018). RecycleNet: Intelligent Waste Sorting Using Deep Neural Networks. Recuperado el 13 de Octubre del 2020 de https://www.researchgate.net/publication/325626219_RecycleNet_Intelligent_Waste_Sorting_Using_Deep_Neural_Networks


