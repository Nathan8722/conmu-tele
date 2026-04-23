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

