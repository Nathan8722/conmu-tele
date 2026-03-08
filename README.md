# conmu-tele
conmutacion

Parcial 1

Punto 1

A. jitter es la diferencia de latencia entre el flujo de paquetes de un cliente a otro, el jitter se mide en milisegundos y es mas potente para transmitir servicios de audio y video y la latencia se usa para medir el tiempo que tardan los datos en viajar de un extremo a otro.

B. UDP es más eficiente en throughput para transmisión de video en tiempo real.
TCP ofrece mayor control de pérdida de paquetes gracias a su cabecera más compleja y mecanismos de confiabilidad, TCP usa un tamaño minimo de cabecera de 20 bytes y UDP 8 bytes.

C. el protocolo ARP llena la tabla que se muestra por el comando escrito y su funcion es traducir las direcciones ip a direcciones mac para que sea posible encapsularlos en tramas ethernet y transportarse dentro de la red local.

D. SNMPv2c es mas simple pero tiende a ser inseguro (community strings en texto plano)
SNMPv3 es mas complejo pero es mas seguro (autenticación, cifrado y control de acceso).

En cuanto a mensajes ambos soportan las mismas operaciones pero SNMPv3 añade un encabezado de seguridad que garantiza integridad y confidencialidad.

E. el OID identifica variables dentro de la MIB
Para saber la cantidad de bytes recibidos en una interfaz, se usa SNMP Get, porque permite consultar directamente el valor del OID.

Un Trap no es adecuado ya que solo reporta eventos espontáneos, no métricas bajo demanda.

Punto 2

A. la cabecera ethernet especifica quien envia, quien recibe y que protocolo se transporta

   en cuanto a la trama, va dirigida a una mac especifica, fue enviada por otra mac y contiene un paquete ipv4

B. el TTL es importante porque define la cantidad de tiempo que un paquete de datos debe existir en la red antes de ser descartado o revalidado y el protocolo indica que tecnologia se utiliza para transportar los paquetes de red segun la necesidad

C. ACK se usa para confirmar la recepcion de los datos y PSH sirve para indicar que los datos deben entregarse de inmediato a la aplicacion

  en cuanto al puerto 80 hace referencia a que esta intentando acceder a un servidor web mediante HTTP.

D. Al utilizar IPV6 la cabecera IPV4 seria reemplazada por la IPV6, el ethertype cambiaria a 0x86DD y se produciria un procesamiento mas rapido en los routers al ser una cabecera simplificada.

Punto 3

A. no se trata de un ping o un tracert, en realidad es una combinacion de ambas cosas, ya que inicialmente se realiza un proceso similar a tracert en el cual muestra la ruta hasta el destino, seguido de esto, realiza funciones similares a ping porque envia multiples paquetes a cada salto y de este modo calcula latencia y perdida de paquetes por cada nodo

Se usan dos fases para obtener el resultado:

en la primera fase envía paquetes ICMP echo Request y luego Va aumentando el valor del TTL progresivamente, y en la segunda fase envía múltiples paquetes ICMP a cada uno de los saltos detectados. De esta manera mide la latencia y el porcentaje de pérdida de paquetes para finalmente presentar:

Pérdida en el enlace.

Pérdida acumulada desde el origen.

Tiempo promedio por nodo.

B. El comando es snmpwalk -v 2c -c public 192.168.1.1 para recorrer el árbol MIB del router usando SNMPv2c.

El authenticationFailure trap ocurre cuando se intenta acceder al router mediante SNMP con credenciales o comunidad incorrecta.
La ventaja del Trap frente al polling es que el dispositivo notifica el evento automáticamente, reduciendo tráfico de red y permitiendo una detección más rápida del problema.

Punto 4

1. Las acciones de Git usan Capa 7 (Aplicación), mientras que el envío por red involucra principalmente Capa 4 (Transporte – TCP).

2. los protocolos y estructuras que intervienen son:

Datos Git

Segmento TCP

Paquete IP

Trama Ethernet/WiFi

Bits en el medio físico

3. Comandos de diagnóstico que se pueden usar:

nslookup github.com: Verifica la resolución DNS del servidor.

ping github.com: Comprueba la conectividad y latencia hacia el servidor.

tracert github.com: Permite identificar la ruta y los saltos de red hasta el destino.

pathping github.com: Analiza la ruta y la pérdida de paquetes en cada salto.

netstat -an: Muestra las conexiones TCP activas y el estado de los puertos.

Wireshark (filtro tcp.port == 443): Permite analizar el tráfico HTTPS, el handshake TCP/TLS y detectar fallos en la comunicación.

Estas herramientas permiten diagnosticar problemas de DNS, conectividad, enrutamiento, pérdida de paquetes y establecimiento de conexiones TCP durante el envío de datos a GitHub.

4. El éxito del git push depende de una red con latencia baja, mínima pérdida de paquetes y buen throughput, lo que garantiza una comunicación estable con los servidores de GitHub.

Paso 1: Verificación de conectividad básica y resolución de nombres

Mediante nslookup se verifica que el sistema pueda resolver el nombre de GitHub a una dirección IP (capa de aplicación – DNS).
Luego, con ping se comprueba la conectividad IP entre el equipo y el servidor, verificando la capa de red del modelo OSI mediante el protocolo ICMP, además de permitir medir latencia y pérdida de paquetes.

El equipo obtiene la IP de github.com mediante el protocolo DNS, que traduce nombres de dominio a direcciones IP mediante consultas a servidores DNS. Este protocolo pertenece a la capa de aplicación del modelo OSI. Si la resolución falla, se puede diagnosticar manualmente utilizando el comando nslookup.

Si el ping es exitoso pero con latencia alta y variable, las métricas afectadas son principalmente latencia y jitter. Esto puede provocar que operaciones como git push sean más lentas y menos eficientes, debido al aumento del tiempo de comunicación y posibles retransmisiones en el protocolo TCP.

Paso 2: Establecimiento de la conexión para el push

Antes de que git push envíe los datos, se establece una conexión mediante el protocolo TCP (capa 4 del modelo OSI). Esta conexión se crea usando el mecanismo Three-Way Handshake, que consiste en tres pasos: SYN-SYN-ACK-ACK, asegurando que cliente y servidor estén listos para intercambiar datos de manera confiable.

Para observar en tiempo real los segmentos TCP durante un git push, se puede usar Wireshark. Aplicando el filtro ip.addr == IP_de_GitHub (o ip.addr == IP_de_GitHub && tcp) se puede visualizar únicamente el tráfico entre el equipo y los servidores de GitHub.

En una conexión de git push hacia GitHub, la cabecera TCP normalmente contiene:

Puerto origen: un puerto efímero del cliente (ej. 52344)

Puerto destino: 443 (HTTPS) del servidor

Estos puertos son gestionados por la capa de transporte (capa 4 del modelo OSI), que permite identificar las aplicaciones que están comunicándose en la red.

Paso 3: Encapsulamiento y enrutamiento de los datos

Durante un git push, los datos del commit pasan por un proceso de encapsulamiento: primero son datos de aplicación, luego se convierten en segmentos TCP, después en paquetes IP, y finalmente en tramas Ethernet, que la tarjeta de red transmite como bits a través del medio físico hacia los servidores de GitHub.

Si un router congestionado empieza a descartar paquetes, el git push se vuelve más lento debido a retransmisiones y reducción de la velocidad de envío. TCP activa mecanismos de retransmisión y control de congestión (como Slow Start y Fast Retransmit) para mantener la comunicación. Para identificar en qué salto ocurre la pérdida de paquetes, se puede usar el comando pathping github.com.

El campo TTL de la cabecera IP evita que los paquetes circulen indefinidamente en la red. Cada router reduce su valor en 1 y, cuando llega a 0, el paquete se descarta y se envía un mensaje ICMP al origen, previniendo bucles de enrutamiento y congestionamiento de la red.

Paso 4: Confirmación y fin de la comunicación

Para confirmar la recepción de datos, TCP utiliza segmentos con la bandera ACK. Estos mensajes indican que los datos fueron recibidos correctamente. Si ocurre pérdida de paquetes, el cliente detecta la ausencia del ACK y retransmite los segmentos, lo que permite que TCP mantenga la fiabilidad en la comunicación, asegurando que el git push se complete correctamente

Cuando termina un git push, la conexión TCP se cierra mediante un four-way handshake, en el que cliente y servidor intercambian segmentos FIN y ACK para indicar que ya no enviarán más datos. Este proceso garantiza un cierre ordenado y confiable de la conexión.

Mediante SNMP en el router de salida se pueden monitorear métricas como bytes transmitidos/recibidos, paquetes enviados, paquetes descartados y errores de interfaz, lo que permite analizar el tráfico generado por un git push. Si se requiere que las consultas sean seguras y cifradas, se debe utilizar SNMPv3, que incorpora autenticación y cifrado en la comunicación
