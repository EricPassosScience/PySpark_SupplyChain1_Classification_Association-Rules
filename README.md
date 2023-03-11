# Predecir el riesgo de demorar la entrega de una compra usando reglas de asociación y clasificación con PySpark.
## ¿Qué es "Supply Chain?
Es un término inglés que significa "cadena de suplementos". Se refiere a los procesos y los diferentes caminos por los que pasan los productos, desde el retiro de la materia prima hasta la entrega al consumidor final.

En la práctica, esto representa todas las actividades de compra de insumos o productos, transporte, almacenamiento, transformación, embalaje, gestión interna, venta y distribución a los clientes.

Agregue a todos estos pasos la infraestructura física y de procesos necesaria para respaldar las operaciones.

En el proceso de la cadena de suministro, también podemos incluir algunas actividades adicionales, como las relacionadas con la creación de nuevos productos, servicio al cliente, control financiero y marketing.

Esto implica la interacción entre varias empresas y profesionales, como productores, proveedores, distribuidores y tenderos, además de toda la logística necesaria que permita el movimiento de productos e insumos.

## Supply Chain and Logistics
La logística y las cadenas de suministro siempre han ido de la mano. Si bien la “cadena de suministro” es un concepto nuevo, su origen se remonta al surgimiento de la logística como objeto de estudio en 1950.

Es una relación tan dependiente que no es posible decir con certeza si la logística es un instrumento de la cadena de suministro o al contrario, ya que hay autores que defienden ambos conceptos.

Brevemente, podemos decir que la logística se encarga de mover los productos de un lugar a otro y de toda la documentación relacionada con estos procesos.

Estas actividades engloban no solo el propio transporte sino también el análisis estratégico para definir los medios más ágiles y adecuados para cada tipo de producto, el uso de tecnologías de movimiento y seguimiento, gestión de riesgos y control de carga y descarga.

Todos estos procesos logísticos tienen como objetivo que los productos lleguen a su destino a tiempo y con total seguridad.

## ¿Qué es el "Supply Chain Analytics"?
En Supply Chain Analytics nos interesa aplicar Data Science a los datos generados por la cadena de suministro para detectar patrones, anomalías o realizar predicciones.

## Definición del Problema Empresarial
En este trabajo, predeciremos el riesgo de un retraso en la entrega. Para ello, utilizaremos técnicas de extracción de reglas de asociación y modelado predictivo con PySpark.

Imagine que una empresa quiere predecir el riesgo de retraso en la entrega de productos en su cadena de suministro. Un cliente realiza una compra en línea, completa todos los datos relacionados con la compra y recibe un tiempo estimado de entrega de los productos. La empresa podría entonces utilizar un modelo predictivo para predecir el riesgo de retraso en la entrega. ¿Porque? Tomar medidas proactivas para evitar demoras, evitar costos adicionales de envío o devolución y aumentar la satisfacción del cliente.

## Fuente de datos

Los datos están disponibles públicamente en -> https://data.mendeley.com/datasets/8gx2fvg2k6/5

## ¿Qué son las Reglas de Asociación?
¿Qué son las Reglas de Asociación?
Las reglas de asociación representan uno de los conceptos de aprendizaje automático más importantes en el análisis de la cesta de la compra.

Invertir tiempo y recursos en la colocación deliberada de productos no solo acorta el tiempo de compra del cliente, sino que también le recuerda qué artículos pueden interesarle y comprar, lo que ayuda a las tiendas a realizar ventas cruzadas en el proceso. Otra ventaja está en la cadena de suministro, ya que los sistemas de entrega se pueden adaptar y personalizar al patrón de compra de los clientes.

Las reglas de asociación ayudan a descubrir todas estas relaciones entre grandes elementos de la base de datos. Una cosa importante a tener en cuenta es que las Reglas de Asociación no extraen la preferencia de un individuo, sino que encuentran relaciones entre un conjunto de elementos de cada transacción distinta. Esto es lo que los diferencia del filtrado colaborativo, por ejemplo.

En nuestro problema, utilizaremos la extracción de reglas de asociación para comprender mejor los datos y luego seleccionaremos las mejores variables para crear un modelo predictivo. Vamos a crear un marco de datos que asocie al cliente con todos los productos que ha comprado.
Para ello utilizaremos el algoritmo principal de extracción de Reglas de Asociación con PySpark el "Frequent Pattern Mining".

Documentación -> https://spark.apache.org/docs/latest/ml-frequent-pattern-mining.html
## Métricas de evaluación para el algoritmo "Frequent Pattern Mining" de PySpark
- Support (Apoyo): Esta medida da una idea de la frecuencia de un conjunto de artículos en todas las transacciones. Ejemplos: Considere itemset1 = {Pan, Mantequilla} y itemset2 = {Pan, Champú}. Habrá muchas más transacciones que contengan pan y mantequilla que que contengan pan y champú. Por lo tanto, itemset1 generalmente tendrá mayor soporte que itemset2.
Matemáticamente, el soporte es la fracción del número total de transacciones en las que se produce el conjunto de elementos. El valor de soporte nos ayuda a identificar lo que vale la pena considerar para un análisis más detallado.
Por ejemplo, es posible que solo desee considerar conjuntos de elementos que ocurren al menos 50 veces de un total de 10 000 transacciones, es decir, en este caso soporte = 0,005. Si un conjunto de elementos tiene un soporte muy bajo, no tenemos suficiente información sobre la relación entre sus elementos y, por lo tanto, no se pueden sacar conclusiones de dicha regla.
- Confidence (Confianza): Esta medida define la probabilidad de ocurrencia de consecuentes en el carro, ya que el carro ya tiene los antecedentes.
Esta medida se utiliza para responder a la pregunta: De todas las transacciones que contienen {Mantequilla}, ¿cuántas cremas también tenían {Pan}? Podemos decir que es de conocimiento común que {Mantequilla} -> {Pan} debe ser una regla de confianza alta.
Técnicamente, la confianza es la probabilidad condicional de ocurrencia del consecuente dado el antecedente.
No importa lo que tengas en el antecedente para un consecuente tan frecuente. La confianza para una regla de asociación con un consecuente muy frecuente siempre será alta.
- Lift (Elevación): El ascensor es el control para el soporte (frecuencia) del consecuente mientras se calcula la probabilidad condicional de ocurrencia de {Y} dado {X}. Piense en ello como el impulso que proporciona {X} a nuestra confianza al tener {Y} en el carrito.
Para reformular, la elevación es el aumento en la probabilidad de tener {Y} en el carrito con el conocimiento de que {X} está presente sobre la probabilidad de tener {Y} en el carrito sin ningún conocimiento de la presencia de {X}.
En los casos en que X realmente lleva a Y en el carrito, el valor de elevación será mayor que 1. Un valor de elevación menor que 1 muestra que tener X en el carrito no aumenta las posibilidades de que Y ocurra en el carrito, a pesar de que la regla muestra un alto valor de confianza.
Un valor de elevación superior a 1 atestigua la alta asociación entre Y y X. Cuanto mayor sea el valor de elevación, mayores serán las posibilidades de comprar Y si el cliente ya ha comprado X. La elevación es la medida que ayudará a los gerentes a decidir sobre la colocación del producto. en el pasillo o en el sitio de comercio electrónico.

## Modelo Predictivo
Finalmente, usamos el algoritmo "Decision Tree Classifier" de PySpark para hacer las predicciones.

Documentación -> https://spark.apache.org/docs/latest/ml-classification-regression.html#decision-tree-classifier

Documentación de métricas de evaluación -> https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.evaluation.MulticlassClassificationEvaluator.html
