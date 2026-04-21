Parte conceptual

1

NetFlow es una tecnología de estado. El dispositivo (router o switch) observa los paquetes que pasan y los agrupa en "flujos". Un flujo se define generalmente por la 5-tupla (IP origen/destino, puerto origen/destino y protocolo).

Proceso: El dispositivo mantiene una caché de flujos en su memoria. Cuando llega un paquete, el dispositivo verifica si pertenece a un flujo existente para actualizar sus contadores o si debe crear uno nuevo.

Muestreo: En versiones modernas (Sampled NetFlow), no se analizan todos los paquetes para crear el flujo, pero una vez que un paquete es seleccionado, se utiliza para actualizar un registro de flujo completo en la memoria del equipo.

Exportación: Los datos se envían al colector solo cuando el flujo expira (por tiempo de inactividad o porque el flujo terminó).

sFlow es una tecnología sin estado. No intenta reconstruir flujos en la memoria del dispositivo de red.

Proceso: El hardware toma un paquete de forma aleatoria (por ejemplo, 1 de cada 1000) y copia la cabecera (header) de ese paquete.

Exportación Inmediata: Esa cabecera, junto con información de la interfaz, se empaqueta y se envía inmediatamente al colector sFlow. El switch no guarda memoria de lo que vio hace un segundo.

Visibilidad: Proporciona una "fotografía" estadística en tiempo real de todo lo que atraviesa el dispositivo, incluyendo tráfico de Capa 2 (Ethernet).

Se elegiria sFlow en un enlace de 100 Gbps cuando la prioridad es la escalabilidad y la visibilidad en tiempo real sin arriesgar la estabilidad del dispositivo de red, aceptando que la precisión será estadística pero suficiente para identificar a los principales consumidores de tráfico.

Es el conjunto de campos clave que definen un flujo único. Si un solo bit cambia en cualquiera de estos campos, el dispositivo de red lo considera un flujo totalmente distinto.

Para que un paquete sea agrupado dentro del mismo flujo, debe coincidir exactamente en:

Dirección IP de Origen: La identidad de quien envía los datos.

Dirección IP de Destino: La identidad del receptor.

Puerto de Origen: El puerto efímero o de servicio desde el cual se inicia la conexión.

Puerto de Destino: El puerto que identifica el servicio que se está accediendo.
Protocolo IP: El número que identifica el protocolo de la capa de transporte (por ejemplo, 6 para TCP o 17 para UDP).

los puertos a utilizar son:

HTTP: Puerto 80 (TCP).

Para tráfico web seguro (HTTPS), se debe usar el puerto 443 (TCP).

SSH: Puerto 22 (TCP).

