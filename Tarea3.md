## Resumen de capitulos ##

### Modulo 4

Conceptos Básicos de Redes Domésticas

La mayoría de las redes domésticas constan de al menos dos redes separadas. La red pública procedente del proveedor de servicios. El router está conectado a Internet. Lo más probable es que el enrutador doméstico esté equipado con funcionalidades tanto cableadas como inalámbricas.

Tecnologías de Red en el Hogar

Las tecnologías inalámbricas utilizan ondas electromagnéticas para transportar información entre dispositivos. El espectro electromagnético incluye bandas de transmisión de radio y televisión, luz visible, rayos X y rayos gama. Algunos tipos de ondas electromagnéticas no son adecuados para transportar datos.

Estándares Inalámbricos

El estándar IEEE 802.11 rige el entorno WLAN. Los estándares inalámbricos para redes LAN usan las bandas de frecuencia de 2.4 GHz y 5 GHz. En conjunto, estas tecnologías se conocen como Wi-Fi. La Alianza Wi-Fi (Wi-Fi Alliance) es responsable de probar los dispositivos LAN inalámbricos de diferentes fabricantes.

Configurar un Enrutador Doméstico

Muchos enrutadores inalámbricos diseñados para el uso doméstico tienen una utilidad de configuración automática que se puede usar para configurar los ajustes básicos del enrutador. Para conectarse al enrutador mediante una conexión cableada, conecte un cable de conexión Ethernet al puerto de red de la computadora. Conecte el otro extremo a un puerto LAN del router.

### Modulo 5

Protocolo de comunicación

Los protocolos son necesarios para que las computadoras se comuniquen correctamente a través de la red. Estos incluyen el formato del mensaje, el tamaño del mensaje, el tiempo, la codificación, la encapsulación y los patrones del mensaje.

Estándares de Comunicación

Las topologías nos permiten ver la red mediante la representación de dispositivos finales y dispositivos intermediarios. ¿Cómo ve una red un dispositivo? Piense en un dispositivo en una burbuja. Lo único que ve un dispositivo es su propia información de direccionamiento. ¿Cómo sabe el dispositivo que está en la misma red que otro dispositivo? La respuesta son los protocolos de red. La mayoría de las comunicaciones de red se divide en unidades de datos o paquetes más pequeños.
Modelos de Comunicación de Red

Descripción de la capa del modelo OSI

Los protocolos son reglas que rigen las comunicaciones. La comunicación correcta entre hosts requiere la interacción entre una serie de protocolos. Los protocolos incluyen HTTP, TCP, IP y Ethernet. Estos protocolos se implementan en el software y el hardware que están instalados en cada host y dispositivo de red.

Modulo 6: Medios de red

Tipos de medios de red
La comunicación se transmite a través de una red en los medios. El medio proporciona el canal por el cual viaja el mensaje desde el origen hasta el destino.

Las redes modernas utilizan principalmente tres tipos de medios para interconectar dispositivos:

- Hilos metálicos dentro de cables – Los datos se codifican en impulsos eléctricos.

- Fibras de vidrio o plástico (cable de fibra óptica) – Los datos se codifican como pulsos de luz.

- Transmisión inalámbrica – Los datos se codifican a través de la modulación de frecuencias específicas de ondas electromagnéticas.

### Modulo 7

Encapsulación y la Trama de Ethernet

El proceso que consiste en colocar un mensaje dentro de otro formato de mensaje se denomina encapsulamiento. Cuando el destinatario revierte este proceso y quita la carta del sobre se produce la desencapsulación del mensaje. De la misma manera en la que una carta se encapsula en un sobre para la entrega, los mensajes de las computadoras también deben encapsularse. 

La Capa de Acceso

La capa de acceso es la parte de la red que permite a las personas obtener acceso a otros hosts y a archivos e impresoras compartidos. La capa de acceso proporciona la primera línea de dispositivos de red que conectan hosts a la red Ethernet cableada. Dentro de una red Ethernet, cada host puede conectarse directamente a un dispositivo de red de capa de acceso mediante un cable Ethernet.

### Modulo 8

Propósito de la Dirección IPv4

La dirección IPv4 es una dirección de red lógica que identifica a un host en particular Debe configurarse correctamente y ser única dentro de la red LAN, para posibilitar la comunicación local. También debe configurarse correctamente y ser única en el mundo, para posibilitar la comunicación remota.

Propósito de la Dirección IPv4

La dirección IPv4 es una dirección de red lógica que identifica a un host en particular Debe configurarse correctamente y ser única dentro de la red LAN, para posibilitar la comunicación local. También debe configurarse correctamente y ser única en el mundo, para posibilitar la comunicación remota.

### Modulo 9

Unidifusión, difusión y multidifusión de IPv4

La transmisión unidifusión se refiere a un dispositivo que envía un mensaje a otro dispositivo en comunicaciones uno a uno. Un paquete de unidifusión tiene una dirección IP de destino que es una dirección de unidifusión que va a un único destinatario. Una dirección IP de origen sólo puede ser una dirección de unidifusión, ya que el paquete sólo puede originarse de un único origen.

Tipos de direcciones IPv4 (Públicas vs. Privadas)

Las direcciones IPv4 públicas son direcciones en las que se realiza routing globalmente entre los routers ISP. Sin embargo, no todas las direcciones IPv4 disponibles pueden usarse en Internet. Existen bloques de direcciones denominadas direcciones privadas que la mayoría de las organizaciones usan para asignar direcciones IPv4 a los hosts internos.

Segmentación de la red y protocolos de descubrimiento

En una LAN Ethernet, los dispositivos usan transmisiones y ARP para ubicar otros dispositivos. ARP envía transmisiones de capa 2 a una dirección IPv4 conocida en la red local para descubrir la dirección MAC asociada. Los dispositivos de LAN Ethernet también localizan otros dispositivos que utilizan servicios. Un host generalmente adquiere su configuración de dirección IPv4 mediante DHCP, que envía transmisiones en la red local para ubicar un servidor DHCP.

### Modulo 10

Problemas con IPv4 y la migración

El agotamiento del espacio de direcciones IPv4 fue el factor que motivó la migración a IPv6. IPv6 tiene un mayor espacio de direcciones de 128 bits, lo que proporciona 340 sextillones de direcciones. Cuando el IETF comenzó a desarrollar un sucesor de IPv4, aprovechó esta oportunidad para corregir las limitaciones de IPv4 e incluir mejoras.

Asignación y Estructura de direcciones IPv6

Las direcciones IPv6 tienen una longitud de 128 bits y se escriben como una cadena de valores hexadecimales. Cada cuatro bits está representado por un solo dígito hexadecimal; para un total de 32 valores hexadecimales. Las direcciones IPv6 no distinguen entre mayúsculas y minúsculas, y pueden escribirse en minúsculas o en mayúsculas. En IPv6, un hexteto que se refiere a un segmento de 16 bits o cuatro valores hexadecimales.

### Modulo 11

Direccionamiento Estático y Dinámico

Con una asignación estática, el administrador de red debe configurar manualmente la información de red para un host. Como mínimo, esto incluye la dirección IPv4 del host, la máscara de subred y la puerta de enlace predeterminada. La asignación estática de la información de direccionamiento puede proporcionar un mayor control de los recursos de red, pero introducir la información en cada host puede ser muy lento.

Configuración de DHCPv4

El servidor DHCP está configurado con un rango (o pool) de direcciones IPv4 que pueden ser asignadas a clientes DHCP. El cliente que necesite una dirección IPv4 enviará un mensaje Descubrir DHCP (DHCP Discover), que es una difusión con dirección IPv4 de destino 255.255.255.255 (32 unos) y dirección MAC de destino FF-FF-FF-FF-FF-FF (48 unos). Todos los hosts de la red recibirán esta trama DHCP de difusión, pero solo un servidor DHCP responderá.

### Modulo 12

Límites de la Red

Cada host de una red debe utilizar el router como gateway hacia otras redes. Por lo tanto, cada host debe conocer la dirección IPv4 de la interfaz del enrutador conectada a la red donde el host se encuentra. Esta dirección se conoce como dirección de puerta de enlace predeterminada.

Funcionamiento de NAT

El enrutador inalámbrico recibe una dirección pública del ISP, lo que le permite enviar y recibir paquetes en el Internet. Este, a su vez, proporciona direcciones privadas a los clientes de la red local.

### Modulo 13

MAC e IP

A veces, un host debe enviar un mensaje, pero solo conoce la dirección IP del dispositivo de destino. El host necesita saber la dirección MAC de ese dispositivo. La dirección MAC se puede descubrir mediante la resolución de direcciones.

Contención de Difusiones

Un mensaje puede contener solo una dirección MAC de destino. La resolución de direcciones permite que un host envíe un mensaje de difusión a una dirección MAC única que es reconocida por todos los hosts. La dirección MAC de transmisión es una dirección de 48 bits compuesta por todos unos. Las direcciones MAC generalmente se representan en notación hexadecimal. La dirección MAC de difusión en notación hexadecimal es FFFF.FFFF.FFFF.

### Modulo 14

La Necesidad del Enrutamiento

A medida que las redes crecen, a menudo es necesario dividir una red de capa de acceso en varias redes de capa de acceso.

La Tabla de Enrutamiento

Cada puerto o interfaz de un enrutador se conecta a una red local diferente. Cada router tiene una tabla de todas las redes conectadas de manera local y las interfaces que se conectan a ellas.

Crear una LAN

LAN se refiere a una red local o un grupo de redes locales interconectadas que están bajo el mismo control administrativo. Todas las redes locales dentro de una LAN están bajo un control administrativo. Otras características comunes de las LAN son que suelen usar protocolos Ethernet o inalámbricos, y que admiten velocidades de transmisión de datos altas.

### Modulo 15

TCP y UDP

UDP es un sistema de entrega de "mejor esfuerzo" que no requiere confirmación de recepción. UDP es el protocolo aconsejable para aplicaciones como transmisiones de audio y de voz por IP (VoIP). Las confirmaciones de recepción reducirían la velocidad de la entrega, y las retransmisiones no son recomendables.

Números de Puerto

Cuando se envía un mensaje utilizando TCP o UDP, los protocolos y servicios solicitados se identifican con un número de puerto. Un puerto es un identificador numérico dentro de cada segmento, que se usa para llevar un seguimiento de las conversaciones específicas entre un cliente y un servidor.

### Modulo 16

- La Relación Cliente Servidor

- Servicios de Aplicaciones de Red

- Sistema de Nombres de Dominios

- Clientes y Servidores Web

- Clientes y Servidores FTP

- Terminales Virtuales

- Correo Electrónico y Mensajería

### Modulo 17

Comandos de solución de problemas

Hay disponibles varios programas de utilidad de software que pueden ayudar a identificar problemas de red. El sistema operativo proporciona la mayoría de estas utilidades como comandos CLI.

Éstas son algunas de las utilidades disponibles:

- ipconfig - Muestra información de la configuración IP

- ping - Prueba las conexiones con otros hosts IP

- netstat - Muestra las conexiones de red

- tracert - Muestra la ruta exacta recorrida hasta el destino

- nslookup - Directamente solicita al servidor de nombres información sobre un dominio de destino

