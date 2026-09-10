## 1.1 Los cuatro defectos. Para cada uno: qué está mal, en qué archivo y en qué líneas se manifiesta, y qué consecuencia tiene. Un defecto no es "falta una línea": es qué garantía se pierde por no tenerla.

1. La publicación no depende de la validación.
   - Archivos/líneas: pipeline.yml, jobs validar y publicar; desde la línea 41, donde publicar no tiene dependencia.
   - Garantía que se pierde: No se garantiza que un artefacto solo sea publicado después de superar las pruebas y el análisis
   - Consecuencia: publicar puede terminar correctamente aunque validar falle
2. El Quality Gate de SonarCloud no actúa realmente como gate del pipeline
   Archivos/líneas: pipeline.yml, líneas 31–39
   Garantía que se pierde: No se garantiza que un análisis de calidad fallido bloquee el flujo
   Consecuencia: El scanner puede enviar el análisis a SonarCloud, pero un Quality Gate Failed no necesariamente detiene el workflow
3. Las dependencias se instalan desde cero en cada ejecución y además en ambos jobs
   Archivos/líneas: pipeline.yml, líneas 23–26 y 53–56
   Garantía que se pierde: No se garantiza reutilización del trabajo ya realizado entre ejecuciones
   Consecuencia: Cada run vuelve a descargar/instalar dependencias, aumentando innecesariamente la duración
4. Todo push dispara también el job de publicación, sin restricción por rama
   Archivos/líneas: pipeline.yml, líneas 1–5
   Garantía que se pierde: No se garantiza que la generación/publicación de artefactos ocurra únicamente desde una rama autorizada
   Consecuencia: Un push a una rama de trabajo puede generar artefactos igual que un push a main

## 1.2 El defecto que explica la duración. De los cuatro, cuál explica el tiempo que registraron en docs/linea-base.md. Sustenten con el número que midieron.

- El defecto que mejor explica la duración del workflow es la falta de reutilización de dependencias entre ejecuciones, ya que ambos jobs vuelven a instalar las dependencias desde cero. Las tres ejecuciones de la línea base duraron 27 s, 37 s y 22 s, con un promedio de 28.7 s. Por ello, la instalación repetitiva de dependencias es un punto susceptible de optimización y su efecto podrá comprobarse comparando las nuevas ejecuciones contra esta línea base.

## 1.3 El vínculo con su caso. Cuál de los cuatro defectos ataca la restricción del caso transversal de su grupo. Citen un dato del value stream map que levantaron en la Sesión 1.

- Este punto no puede completarse correctamente solo con linea-base.md, porque la consigna exige citar un dato del Value Stream Map levantado por el grupo en la Sesión 1, y dicho dato no aparece en el archivo proporcionado. Para responderlo sin inventar información, necesito el VSM o al menos los valores de PT, LT y %C&A del proceso que identificaron como restricción.

## 1.4 La métrica DORA. Qué métrica DORA esperan mover con la intervención y por qué. Solo dos son alcanzables sin despliegue: identifiquen cuáles y elijan una.

- La métrica DORA que esperamos impactar es Lead Time for Changes, ya que la intervención busca disminuir el tiempo necesario para que un cambio atraviese el pipeline y obtenga retroalimentación. De las métricas DORA, Lead Time for Changes y Change Failure Rate son las dos que pueden aproximarse en este ejercicio sin disponer de un despliegue real. Elegimos Lead Time for Changes porque la intervención propuesta actúa directamente sobre la duración del flujo de integración continua.

## 1.5 El proxy. Qué número concreto van a medir para sustentar que la métrica se movió. Decláralo antes de intervenir.

- El proxy será la duración total de una ejecución exitosa del workflow, medida en segundos. Antes de intervenir, la línea base registrada es de 27 s, 37 s y 22 s, cuyo promedio es 28.7 s. Después de la intervención se volverá a ejecutar el workflow y se comparará su duración promedio con los 28.7 s iniciales. Una reducción de este valor será la evidencia cuantitativa utilizada para sustentar una mejora en el tiempo de retroalimentación asociado a Lead Time for Changes.
