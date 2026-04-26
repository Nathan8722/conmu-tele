# Parte conceptual #

## Punto 1 ##

NetFlow es una tecnología de estado. El dispositivo (router o switch) observa los paquetes que pasan y los agrupa en "flujos". Un flujo se define generalmente por la 5-tupla (IP origen/destino, puerto origen/destino y protocolo).

- **Proceso**: El dispositivo mantiene una caché de flujos en su memoria. Cuando llega un paquete, el dispositivo verifica si pertenece a un flujo existente para actualizar sus contadores o si debe crear uno nuevo.
- **Muestreo**: En versiones modernas (Sampled NetFlow), no se analizan todos los paquetes para crear el flujo, pero una vez que un paquete es seleccionado, se utiliza para actualizar un registro de flujo completo en la memoria del equipo.
- **Exportación**: Los datos se envían al colector solo cuando el flujo expira (por tiempo de inactividad o porque el flujo terminó).

sFlow es una tecnología sin estado. No intenta reconstruir flujos en la memoria del dispositivo de red.

- **Proceso**: El hardware toma un paquete de forma aleatoria (por ejemplo, 1 de cada 1000) y copia la cabecera (header) de ese paquete.
- **Exportación Inmediata**: Esa cabecera, junto con información de la interfaz, se empaqueta y se envía inmediatamente al colector sFlow. El switch no guarda memoria de lo que vio hace un segundo.
- **Visibilidad**: Proporciona una "fotografía" estadística en tiempo real de todo lo que atraviesa el dispositivo, incluyendo tráfico de Capa 2 (Ethernet).

Se elegiria sFlow en un enlace de 100 Gbps cuando la prioridad es la escalabilidad y la visibilidad en tiempo real sin arriesgar la estabilidad del dispositivo de red, aceptando que la precisión será estadística pero suficiente para identificar a los principales consumidores de tráfico.

**5-tuple en un flujo NetFlow**

Es el conjunto de campos clave que definen un flujo único. Si un solo bit cambia en cualquiera de estos campos, el dispositivo de red lo considera un flujo totalmente distinto.

Para que un paquete sea agrupado dentro del mismo flujo, debe coincidir exactamente en:

- **Dirección IP de Origen**: La identidad de quien envía los datos.
- **Dirección IP de Destino**: La identidad del receptor.
- **Puerto de Origen**: El puerto efímero o de servicio desde el cual se inicia la conexión.
- **Puerto de Destino**: El puerto que identifica el servicio que se está accediendo.
- **Protocolo IP**: El número que identifica el protocolo de la capa de transporte (por ejemplo, 6 para TCP o 17 para UDP).

Los puertos a utilizar son:

- HTTP: Puerto 80 (TCP).
- Para tráfico web seguro (HTTPS), se debe usar el puerto 443 (TCP).
- SSH: Puerto 22 (TCP).

**IP Accounting**

En la imagen se observa asimetría: por cada 30 paquetes enviados por el host de origen, solo regresa 1 paquete del destino (1500 / 50 = 30). 

Esta asimetría extrema suele indicar uno de los siguientes tres escenarios:

**Enrutamiento Asimétrico**: 
Es posible que los paquetes de ida (192.168.1.10 → 10.0.0.5) pasen por este router, pero los paquetes de regreso estén tomando una ruta distinta (otro router o enlace) para volver al origen. En este caso, el tráfico es normal, pero el router solo "ve" una parte de la conversación.
  
**Naturaleza del Tráfico**:

- Exfiltración de datos o Backup
- Streaming de Video (UDP)
- Ataque de Denegación de Servicio (DoS)

**Problemas de conectividad o seguridad**

- Bloqueo por Firewall/ACL
- Packet Loss (Pérdida de paquetes)

# Parte de diseño #

Contenedor Docker
Descripcion:
 
Esquema de la Arquitectura
La arquitectura se divide en tres capas principales:

- Capa de Host / Frontend (Navegador): Captura el flujo de video local mediante la API getUserMedia y lo envía al entorno de ejecución.
- Contenedor Docker (Backend de Procesamiento):
- Script Python: Actúa como el orquestador.
- Motor YOLOv8: Carga los pesos del modelo (ej. yolov8n.pt) en memoria (CPU o GPU).
- OpenCV: Procesa los frames, dibuja las cajas delimitadoras (bounding boxes) y añade las etiquetas.
- Capa de Salida: Muestra el video procesado en tiempo real o guarda los metadatos de las detecciones.

<img width="1387" height="747" alt="image" src="https://github.com/user-attachments/assets/7bd75d6b-d1b8-48d7-ab79-752a0d884849" />

Maquina Virtual

Descripcion:

Esquema de la arquitectura

El Sensor (Captura y Pre-proceso)

**Origen**: El navegador captura el video de la cámara local (JS) y lo inyecta al entorno de ejecución.
**Ingesta**: El script de Python en el contenedor Docker recibe los datos y usa OpenCV para preparar los cuadros (frames) para la IA.

El Cerebro (Detección YOLOv8)

**Inferencia**: El motor YOLOv8 analiza cada frame buscando patrones de peatones y vehículos.
**Acción**: El script genera el video anotado y envía los resultados (alertas o video) hacia un servidor externo a través de la red.

El Auditor (Observabilidad NetFlow)

**Exportación**: Una VM con softflowd "escucha" el tráfico que sale del contenedor sin interrumpirlo.

**Análisis**: Agrupa los paquetes en flujos (NetFlow) y envía esa telemetría a un colector para monitorear el consumo de ancho de banda y el rendimiento del enlace.

<img width="1382" height="745" alt="image" src="https://github.com/user-attachments/assets/58721e41-2496-409f-b51c-5842a784dc4f" />

Descripcion:

Esquema de la arquitectura:

Generación (IA)
Entrada: Video de la webcam (vía JS/Colab).

**Proceso**: Contenedor Docker con YOLOv8 detecta peatones y vehículos.

**Salida**: Resultados de detección enviados por red.

Telemetría (Red)
**Captura**: La VM con softflowd intercepta el tráfico del contenedor.
**Exportación**: Convierte los paquetes en registros NetFlow y los envía al colector.

Visualización (Dashboard)
Recolección: Un script de Python (Colector) recibe y organiza los flujos.

Dashboard: Streamlit/Matplotlib muestra en tiempo real los "Top Talkers" y el consumo de ancho de banda.

<img width="1377" height="740" alt="image" src="https://github.com/user-attachments/assets/603955a7-9a1c-4ec9-a663-1e0545ab51dd" />


Colector netflow

Descripcion:

Esquema de la arquitectura

En un entorno real, el flujo de datos sigue este orden:

Exportador (Router/Switch): Envía paquetes UDP (puerto 2055) con registros de tráfico.

    Colector (Colab): Un script de Python abre un socket UDP, decodifica los encabezados de NetFlow y almacena los datos en un DataFrame     de Pandas.

    Visualización: Un hilo secundario toma los datos del colector y actualiza un gráfico de barras o líneas.

Implementación del Colector y Dashboard

Dado que en Colab no solemos tener un router físico enviando datos reales a la IP efímera de Google, incluiremos un Generador de Tráfico Sintético para que puedas ver el dashboard en funcionamiento inmediatamente.

Explicación de los Componentes

Socket UDP vs. Hilo de Simulación: En una red local, usarías socket.socket(socket.AF_INET, socket.SOCK_DGRAM) para recibir datos de un dispositivo Cisco o Huawei. Aquí usamos un hilo (threading) para poblar una lista compartida y no bloquear la ejecución del dashboard.

Pandas Integration: Es la mejor forma de procesar NetFlow, ya que permite agrupar datos por IP de origen, destino o puertos de forma casi instantánea.

Matplotlib/Seaborn: Se encargan de la capa de presentación. Usamos clear_output(wait=True) para simular una actualización en vivo, lo cual es la forma más eficiente de crear dashboards dentro de una celda de Jupyter sin necesidad de desplegar un servidor web externo.

<img width="1054" height="544" alt="imagen" src="https://github.com/user-attachments/assets/433f4d14-c890-4987-92e3-04b71d1d0cae" />

# Preguntas de Diseño #

Para lograr que el tráfico de detecciones de un contenedor YOLO sea capturado y muestreado por NetFlow, necesitamos que los paquetes atraviesen un punto de control (un switch virtual o el stack de red del host) donde un "Exportador" de NetFlow pueda observar el flujo.

Arquitectura de Red: El "Punto de Observación"

Para que NetFlow funcione, el tráfico no puede ser interno del contenedor (IPC); debe salir hacia la red de la VM. La mejor forma de estructurarlo es tratar al contenedor como un host de red independiente conectado a un Bridge Virtual en la VM.
Componentes clave:

- Contenedor (YOLO): Envía las detecciones (vía JSON, MQTT, o gRPC) hacia una IP de destino.

- Interfaz Virtual (veth): El "cable" que une al contenedor con el stack de la VM.

- Exportador NetFlow: Un software en la VM (como ipt-netflow, Softflowd o Open vSwitch) que escucha en esa interfaz.

Estrategias de Implementación

Usar Softflowd

softflowd es un exportador de NetFlow por software que puede escuchar en una interfaz específica de la VM y enviar los datos a un colector.

    Identificar la interfaz: Docker suele usar docker0.

    Ejecutar el sensor:
    Bash

    sudo softflowd -i docker0 -n 127.0.0.1:2055

    Aquí, softflowd captura el tráfico que sale de YOLO por la interfaz bridge y lo envía al puerto 2055 (donde estaría tu colector NetFlow).

Para medir el tráfico específicamente entre la subred del contenedor (donde vive YOLO) y la interfaz de la VM, lo ideal es utilizar nftables. Es el sucesor moderno de iptables y permite definir contadores de forma mucho más limpia y eficiente.

Asumiremos que la subred de tus contenedores es 172.17.0.0/16 (la habitual de Docker) y que la IP de la VM en el bridge es 172.17.0.1.
1. Propuesta con nftables (Recomendado)

Esta regla crea un "contador" específico para el tráfico que entra y sale del contenedor hacia la VM. La ventaja es que puedes consultar solo estos contadores sin navegar por todo el log del sistema.
Configuración de la regla:
Bash

1. Crear una tabla para contabilidad
nft add table inet accounting

2. Crear una cadena que intercepte el tráfico forward y local
nft add chain inet accounting detections { type filter hook input priority 0 \; }
nft add chain inet accounting output_det { type filter hook output priority 0 \; }

3. Agregar los contadores específicos para la subred de YOLO
nft add rule inet accounting detections ip saddr 172.17.0.0/16 counter
nft add rule inet accounting output_det ip daddr 172.17.0.0/16 counter

Cómo consultar los datos:

Para ver cuántos bytes y paquetes han generado las detecciones, simplemente ejecuta:
Bash

nft list table inet accounting

Esto te devolverá algo como: ip saddr 172.17.0.0/16 counter packets 1542 bytes 124043.
2. Propuesta con iptables (Legacy)

Si prefieres el método clásico, puedes insertar reglas en las cadenas de INPUT y OUTPUT. Al no definir un "target" (como ACCEPT o DROP), la regla actúa puramente como un contador de paquetes que pasan por ella.
Configuración de la regla:
Bash

Contar tráfico entrante desde el contenedor YOLO a la VM
iptables -A INPUT -s 172.17.0.0/16 -d 172.17.0.1 -m comment --comment "CONTADOR_YOLO_IN"

Contar tráfico saliente desde la VM al contenedor (ACKs, comandos)
iptables -A OUTPUT -d 172.17.0.0/16 -s 172.17.0.1 -m comment --comment "CONTADOR_YOLO_OUT"

Cómo consultar los datos:

Para visualizar el tráfico acumulado:
Bash

iptables -L -v -n | grep "CONTADOR_YOLO"

La columna pkts y bytes te dará la medición exacta del tráfico de detección.
3. Integración con NetFlow

Para que estas mediciones no se queden solo en la consola y se exporten como flujos (NetFlow/IPFIX), te sugiero usar el módulo ipt-netflow.

Si lo instalas, la "regla" de accounting se convierte en una regla de exportación:
Bash

iptables -A FORWARD -s 172.17.0.0/16 -j NETFLOW

Esto enviará automáticamente la información de accounting (IP origen, IP destino, puertos, bytes) hacia tu colector configurado en el /proc/net/stat/ipt_netflow.
Resumen de diferencias

    Contadores (iptables/nftables): Útiles para saber el "cuánto" (volumen total de datos).

    NetFlow: Útil para saber el "cómo" y "cuándo" (gráficas de ráfagas de detección, picos de latencia, destinos frecuentes).

Flujo de datos

<img width="1138" height="622" alt="imagen" src="https://github.com/user-attachments/assets/40dc4c97-3d01-4653-8fbb-7608fbda4d6a" />

2. b

Una estación de trenes requiere monitoreo con cinco cámaras, cada una especializada:

<img width="1131" height="609" alt="imagen" src="https://github.com/user-attachments/assets/d382e2cb-27e6-4423-b7c0-8f681d2a03f4" />

Diagrama de Arquitectura

<img width="1139" height="612" alt="imagen" src="https://github.com/user-attachments/assets/c544e94b-ff9b-4af5-80ed-a1c4038d05df" />

Para calcular el throughput, debemos definir los parámetros de calidad del video (input) y la densidad de los mensajes de detección (output). Basándonos en estándares de videovigilancia y comunicación gRPC/JSON, aquí tienes el desglose técnico:

1. Parámetros de Entrada (Video UDP)

Asumiremos un estándar de alta definición para el análisis de IA:

    Resolución: 1080p (Full HD).

    Codec: H.264 / H.265.

    FPS (Frames por segundo): 15 fps (suficiente para analítica).

    Bitrate estimado: 4 Mbps por flujo de cámara.

2. Parámetros de Salida (Metadata TCP)

La metadata varía según la complejidad de la escena:

    Payload promedio: 2 KB por detección (JSON con coordenadas, etiquetas y timestamps).

    Frecuencia: 5 detecciones por segundo (promedio).

    Cálculo: 2KBX8X5 DET/S=80Kbps=0.08Mbps

Tabla de Throughput por Contenedor

Contenedor,Función,Video (In),Metadata (Out),Total Unitario
C1,Placas (OCR),4 Mbps,0.10 Mbps*,4.10 Mbps
C2,Parqueadero,4 Mbps,0.05 Mbps,4.05 Mbps
C3,Aforo (Personas),4 Mbps,0.15 Mbps**,4.15 Mbps
C4,Animales,4 Mbps,0.05 Mbps,4.05 Mbps
C5,Objetos perdidos,4 Mbps,0.08 Mbps,4.08 Mbps

Cálculo del Throughput Total

Para obtener el impacto total en el Switch Virtual y las VMs, sumamos todos los flujos:

<img width="416" height="94" alt="imagen" src="https://github.com/user-attachments/assets/261a0899-9511-4a8e-9039-867473df0b8c" />

Total Video (UDP): 5x4Mbps= 20Mbps
Total Metadata (TCP): 0.10+0.05+0.15+0.05+0.08=0.43Mbps

Resumen de Resultados

    Throughput por Contenedor (Promedio): 4.086 Mbps

    Throughput Total del Sistema: 20.43 Mbps
    
Protocolo adecuado para Video: UDP

Para el flujo de video desde las cámaras hacia los contenedores YOLO, el protocolo más adecuado es UDP (generalmente a través de RTSP/RTP).

Justificación:

    Prioridad de la Latencia: En análisis de video en tiempo real, un retraso de pocos segundos puede significar que el objeto (o la persona) ya no esté en el cuadro cuando la IA procese la imagen. UDP no tiene retransmisiones; si un paquete se pierde, se descarta y se pasa al siguiente, manteniendo el flujo actualizado.

Evita el "Head-of-Line Blocking": TCP garantiza la entrega en orden. Si un paquete de video se pierde debido al jitter o congestión, TCP detendrá todo el flujo hasta recuperar ese paquete, causando un "congelamiento" visual que afecta el rendimiento de YOLO.

Naturaleza del Tráfico: El video es tolerante a pequeñas pérdidas (píxeles corruptos momentáneos), pero es extremadamente sensible al retardo (delay).

Mitigación del Jitter en el Receptor

El jitter de ±1 ms sobre una latencia de 2 ms significa que los paquetes pueden llegar en intervalos de entre 1 ms y 3 ms. Aunque parece poco, en flujos de alta tasa de bits esto puede causar desincronización.

Para mitigarlo en los contenedores o en la VM, se utilizan las siguientes técnicas:
A. Jitter Buffer (Búfer de De-jitter)

Es la técnica principal. El receptor (el contenedor que corre YOLO o el cliente RTSP) almacena temporalmente los paquetes entrantes durante unos milisegundos antes de entregarlos al motor de decodificación.

    Funcionamiento: Si la red entrega paquetes de forma irregular, el búfer los libera a una tasa constante (1/FPS)
    Configuración: En este escenario, un búfer de 5-10 ms sería suficiente para absorber el jitter de ±1 ms sin añadir una latencia          perceptible para la IA.

Timestamping (RTP Timestamps)

Al usar UDP sobre RTP, cada paquete lleva un sello de tiempo. El receptor utiliza estos timestamps para reconstruir la secuencia temporal original del video, independientemente de cuándo llegaron físicamente los paquetes a la interfaz de red.

Traffic Shaping en el Switch Virtual

Dado que usas Open vSwitch o un Linux Bridge, puedes aplicar políticas de QoS (Quality of Service) para priorizar los paquetes UDP de video sobre el tráfico de metadata TCP o tráfico de gestión.

Para medir el tráfico a nivel de contenedor utilizando NetFlow y técnicas de contabilidad IP, es fundamental entender que, en entornos virtualizados o de orquestación (como Docker o Kubernetes), cada contenedor suele identificarse por una IP única dentro de una red puente (bridge) o una superposición (overlay).

A continuación, se detalla la configuración de la regla y el método de detección.
1. Regla NetFlow (5-tuple) para Contenedores

En el estándar NetFlow, el flujo se define por una combinación de campos clave. Para medir el tráfico de un contenedor específico, la regla 5-tuple debe capturar la identidad de red del contenedor y la naturaleza de su conexión.
Definición de la Regla

Para monitorear un contenedor, la 5-tuple se compone de:

    Source IP: La dirección IP asignada al contenedor.

    Destination IP: La IP del servicio o host con el que se comunica.

    Source Port: El puerto de origen de la aplicación del contenedor.

    Destination Port: El puerto de destino (ej. 80 para HTTP, 443 para HTTPS).

    Protocol: El protocolo de transporte (TCP o UDP).

    Lógica de medición: Si el tráfico coincide con estos cinco campos, NetFlow lo agrupa en un "flow record" y suma los bytes y              paquetes. Al filtrar por la Source IP del contenedor, obtienes el tráfico de salida total.

2. Detección de Consumo con IP Accounting

El IP Accounting es una función más ligera que NetFlow, ideal para ver rápidamente quién está consumiendo más ancho de banda en una interfaz específica (como la interfaz virtual veth de un contenedor o el bridge principal).




