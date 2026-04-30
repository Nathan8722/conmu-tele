Pagina principal: NetopsAssistant

<img width="1903" height="936" alt="image" src="https://github.com/user-attachments/assets/6d962d87-8f84-4b8f-8bb4-9aae10dc18ed" />

VLAN Voice

<img width="1907" height="763" alt="image" src="https://github.com/user-attachments/assets/a712031e-2c5c-44d1-941f-7e25a3cf545e" />

ACLs

<img width="1900" height="823" alt="image" src="https://github.com/user-attachments/assets/7513009e-a3bb-43cf-839c-d81678fee5c4" />

Analizador RTP

<img width="1902" height="942" alt="image" src="https://github.com/user-attachments/assets/d161252f-88ef-4716-b95b-aa5682fbb038" />

Historial de configuraciones

<img width="1912" height="802" alt="image" src="https://github.com/user-attachments/assets/418c4a05-0b26-4697-a5de-858a29d76848" />

Diseñar e implementar un software a partir de un prompt base, que permita:

### Generar plantillas de configuración para switches y routers Cisco y Huawei.

Esta funcionalidad se encuentra principalmente en el Panel: CMD Generator y el objeto JavaScript que maneja la lógica de renderizado.

Interfaz (HTML): Las líneas 111-133 definen los selectores de plataforma (Cisco IOS / Huawei VRP) y los módulos de configuración.

Lógica (JavaScript): La función generateCmd() (líneas 378-436) contiene las plantillas literales. Por ejemplo:

Cisco: Usa comandos como switchport voice vlan y service-policy.

Huawei: Implementa vlan batch, voice-vlan enable y traffic-policy.

### Visualizar dashboards de disponibilidad, rendimiento y eventos.

El código utiliza la librería Chart.js para renderizar datos en tiempo real dentro del Panel: Dashboard.

Interfaz (HTML): Las líneas 245-263 definen los contenedores de métricas (uptime, jitter, loss) y los elementos <canvas> para los gráficos de disponibilidad y ancho de banda.

Lógica (JavaScript): * initDashCharts() (líneas 541-569) inicializa los gráficos con datos simulados.

El bucle setInterval (líneas 643-661) actualiza las métricas visuales cada 2.5 segundos para simular un monitoreo en vivo.

### Analizar tráfico de voz y video en tiempo real, incluyendo métricas de QoS.
Esta es una de las partes más detalladas del software, ubicada en el Analizador de Tráfico RTP.

Interfaz (HTML): Las líneas 267-310 presentan tarjetas específicas para flujos de Voz (RTP) y Video (RTSP/RTP).

Métricas: El código calcula y muestra visualmente el MOS Score, Jitter, Delay y Packet Loss mediante barras de progreso dinámicas.

Lógica (JavaScript): La función dentro del setInterval (líneas 664-706) calcula dinámicamente el valor del MOS basándose en el Jitter y el Delay simulados, cambiando el color de la interfaz según la calidad (Excellent, Good, Fair, Poor).

### Gestionar VLAN Voice y tráfico sensible al retardo.

El software dedica un módulo exclusivo para la automatización de redes de voz.

Interfaz (HTML): El Panel: VLAN Voice (líneas 193-224) permite ingresar rangos de interfaces y IDs de VLAN de datos y voz.

Lógica (JavaScript): La función generateVlan() (líneas 493-513) automatiza la configuración de puertos de acceso, habilitando comandos críticos para la sensibilidad al retardo como spanning-tree portfast, trust dscp y trust cos.

### Aplicar ACLs para control de tráfico.

El cumplimiento de este ítem se divide en una versión rápida y una avanzada.

ACL Rápida: Dentro del generador de comandos (líneas 164-205), permite crear reglas SIP/RTP básicas.

ACL Avanzada (Panel: ACL): Las líneas 227-264 permiten la creación dinámica de filas de reglas.

Lógica (JavaScript): La función generateAcl() (líneas 523-538) construye la lista de control de acceso extendida (Cisco) o avanzada (Huawei), permitiendo filtrar por IP, protocolo, puerto y valor DSCP.
