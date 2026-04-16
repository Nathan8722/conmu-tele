## Ejercicio en  clase

## Segundo corte

## Evidencias ejecucion de codigo

## Linea 1

<img width="1418" height="842" alt="image" src="https://github.com/user-attachments/assets/11701ee1-a341-45e3-8e2a-a61c89fbae1d" />

## Linea 2

<img width="1113" height="137" alt="image" src="https://github.com/user-attachments/assets/fe19a45e-e3ba-400f-99aa-48721210820a" />

## Linea 3

<img width="886" height="361" alt="image" src="https://github.com/user-attachments/assets/c9ed705e-b787-4274-8ce3-d42547528741" />

## Linea 4

<img width="856" height="162" alt="image" src="https://github.com/user-attachments/assets/d539fe34-bf60-4fc0-b22f-f4ea1733661e" />

## Linea 5

<img width="747" height="135" alt="image" src="https://github.com/user-attachments/assets/bb2a0f31-5a5c-4542-915e-d4533ec02e41" />

## Linea 6

<img width="883" height="382" alt="image" src="https://github.com/user-attachments/assets/9a45e589-1051-4103-909c-0b9fa3a91182" />

## Linea 7

<img width="487" height="107" alt="image" src="https://github.com/user-attachments/assets/ce54e8bb-cb2d-4d2d-b307-b8c25e515e2a" />

## Analisis forense con wireshark

Análisis del archivo descarga_tcp.pcap

Filtrado de trafico

tcp.flags.syn == 1

<img width="1912" height="625" alt="image" src="https://github.com/user-attachments/assets/e1b3d759-0f28-4889-958a-4aa2c3480510" />

tcp.analysis.retransmission

<img width="1915" height="660" alt="image" src="https://github.com/user-attachments/assets/4eca9773-3105-4ac3-8511-49b04bec32d5" />

Preguntas:

¿Qué es YOLO?, ¿cúales son sus características principales? Y ¿Qué arquitectura tiene?

YOLO es un modelo de detección de objetos que, a diferencia de enfoques anteriores que escaneaban una imagen múltiples veces, procesa la imagen completa en una sola pasada por la red neuronal. Esto lo hace extremadamente rápido y eficiente.

Características principales

Velocidad: Al procesar la imagen una sola vez, puede operar en tiempo real (30+ FPS), lo que lo hace ideal para video en vivo y aplicaciones embebidas.

Detección simultánea: Predice simultáneamente las cajas delimitadoras (bounding boxes) y las probabilidades de clase, tratando la detección como un problema de regresión.

Visión global: Al ver toda la imagen de una vez, YOLO tiene contexto global, lo que reduce falsos positivos en comparación con métodos de ventana deslizante.

Generalización: Aprende representaciones muy generalizables de los objetos, funcionando bien incluso en dominios distintos al entrenamiento.

Versiones evolutivas: Ha tenido muchas iteraciones (YOLOv1 al YOLOv11+, YOLO-NAS, etc.), cada una mejorando precisión y velocidad.
Arquitectura

La arquitectura general de YOLO se divide en tres grandes bloques:

1. Backbone — Es la red convolucional base que extrae características de la imagen a diferentes escalas. Genera mapas de características (feature maps) en tres resoluciones distintas: 52×52 para objetos pequeños, 26×26 para medianos y 13×13 para grandes.
   
2. Neck — Combina características de múltiples escalas usando dos mecanismos: la Feature Pyramid Network (FPN, top-down) que propaga información semántica de capas profundas hacia arriba, y la Path Aggregation Network (PAN, bottom-up) que refuerza la información de localización. El resultado son representaciones ricas en las tres escalas.
  
3. Head (cabeza de detección) — En versiones modernas (YOLOv8+) usa cabezas desacopladas (decoupled heads): una rama para clasificar el objeto y otra para predecir la bounding box. Esto mejora la precisión respecto a versiones anteriores que lo hacían en una sola rama.
Finalmente, se aplica NMS (Non-Maximum Suppression) para eliminar detecciones duplicadas y quedarse solo con las más confiables.

Protocolo vs. Aplicación

TCP es ideal por sus características fundamentales:

Orientado a conexión: Establece un canal seguro mediante el three-way handshake (SYN-SYN-ACK-ACK) antes de enviar cualquier dato.

Entrega garantizada: Cada paquete enviado debe ser confirmado (ACK). Si no llega confirmación, TCP retransmite automáticamente el paquete perdido.

Orden garantizado: Los segmentos se reensamblan en el orden correcto, sin importar cómo lleguen.

Control de flujo y congestión: Ajusta la velocidad de envío según la capacidad del receptor y de la red.


En el ejercicio esto se observa con el filtro tcp.analysis.retransmission: si aparece algún paquete retransmitido, TCP lo detectó y lo reenvió automáticamente sin que el usuario lo notara, garantizando la integridad del archivo.


¿Por qué la transmisión de video simulada usa UDP?

El script envía 100 paquetes a 20 FPS simulando fotogramas de cámara. Aquí la velocidad y la latencia baja son más importantes que la perfección. 

UDP es ideal porque:

Sin conexión previa: Envía los datos directamente, sin negociación inicial, lo que elimina el retardo del handshake.

Sin confirmaciones (ACK): No espera que el receptor confirme la llegada de cada paquete, lo que permite un flujo continuo y veloz.

Sin retransmisión: Si un paquete se pierde, simplemente se descarta. 

En video en vivo, un fotograma perdido se percibe como un pequeño glitch momentáneo, mucho menos molesto que un video congelado esperando retransmisiones.

Cabecera ligera: UDP tiene solo 8 bytes de cabecera frente a los 20+ bytes de TCP, reduciendo la sobrecarga.

La elección del protocolo depende de la naturaleza de la tarea: cuando los datos deben ser perfectos (como un modelo de IA que no funciona si está incompleto), se sacrifica velocidad por fiabilidad usando TCP. 

Cuando el tiempo real es prioritario y una pérdida ocasional es tolerable (como un fotograma de video), se sacrifica fiabilidad por velocidad usando UDP.

























