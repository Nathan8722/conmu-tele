# Taller 1 e Instalacion docker

Punto 1

1. Actualizar el sistema

  Primero abrimos la terminal y actualizamos los paquetes con el siguiente comando:

  sudo apt update

  sudo apt upgrade -y

  <img width="1280" height="661" alt="1" src="https://github.com/user-attachments/assets/bd46f6de-97dc-4d70-a585-056410bff21c" />
  

  <img width="1286" height="662" alt="2" src="https://github.com/user-attachments/assets/f0112fc7-9f9c-41aa-80bd-22f2a6959c9c" />


2. Instalar dependencias necesarias

  sudo apt install ca-certificates curl gnupg lsb-release -y

  Estas herramientas permiten agregar repositorios externos de forma segura.

  <img width="1279" height="661" alt="3" src="https://github.com/user-attachments/assets/ff209fd8-24ee-4634-88c1-eac0943744d7" />

  <img width="1275" height="658" alt="4" src="https://github.com/user-attachments/assets/b16a55fa-ace5-4036-9657-9c86dbf2bf9f" />

3. Añadir la clave GPG oficial de Docker

   <img width="1281" height="658" alt="7" src="https://github.com/user-attachments/assets/65ca0c80-12d1-4dc8-84e7-953a7ec9fa24" />

4. Instalar Docker

   <img width="1277" height="663" alt="8" src="https://github.com/user-attachments/assets/e37deeec-bdbc-454a-9983-2d548de9d7f8" />

5. Verificar la instalación

   <img width="1279" height="659" alt="10" src="https://github.com/user-attachments/assets/a7bbd598-e122-4798-a118-6b8c42f35200" />


A. Al ejecutar el comando se puede visualizar un video obtenido de docker en el que se muestra un conteo regresivo mediante el formato ascii

<img width="1284" height="666" alt="11" src="https://github.com/user-attachments/assets/7af60281-31ad-4f90-808a-a355714ba9be" />

B. Paso 1: 

<img width="917" height="546" alt="PASO 1" src="https://github.com/user-attachments/assets/1224be76-e22a-492b-9ebd-b762077149b7" />

Paso 2: 

Se crea el dockerfile y se ejecuta el comando para visualizar el robot

<img width="1340" height="692" alt="Captura" src="https://github.com/user-attachments/assets/085ec7a8-58fd-4927-9442-f4f72ce2f92c" />

<img width="1354" height="703" alt="gazebo" src="https://github.com/user-attachments/assets/6cca2cdf-2e47-44bd-8aac-5b6d7452ace4" />

Segundo punto:

<img width="1313" height="666" alt="123123" src="https://github.com/user-attachments/assets/334a03b1-16ae-40c7-b481-a2ffb143fb04" />

<img width="1361" height="708" alt="wireshark" src="https://github.com/user-attachments/assets/59c8b0a8-f951-4d45-ba34-22ce35197911" />

El contenedor que inicia la comunicación envía una trama ARP broadcast para descubrir la dirección MAC asociada a la IP destino. Luego el contenedor destino responde con su dirección MAC, permitiendo que se establezca la comunicación IP.

La solicitud ARP se envía a la dirección MAC broadcast ff:ff:ff:ff:ff:ff, para que todos los dispositivos de la red reciban la petición y el que posea la IP solicitada responda con su dirección MAC.

La respuesta ARP es unicast, porque se envía directamente a la dirección MAC del host que realizó la solicitud.

ARP es necesario para resolver la dirección MAC asociada a una dirección IP dentro de una red local, permitiendo que los dispositivos puedan enviarse tramas Ethernet. Después del intercambio, la caché ARP almacena la asociación entre la dirección IP y la dirección MAC del dispositivo destino, junto con la interfaz de red correspondiente.

Punto 3

Durante la prueba se enviaron 10 paquetes ICMP Echo Request desde contenedor1 hacia contenedor2. Cada solicitud fue respondida con un Echo Reply, permitiendo medir el RTT (Round Trip Time). El RTT representa el tiempo que tarda un paquete en ir desde el origen hasta el destino y regresar, y puede observarse en la columna Time de Wireshark.

El RTT promedio se calcula sumando los tiempos de ida y vuelta de los paquetes ICMP y dividiéndolos entre el número total de paquetes. En la prueba se observa una ligera variación entre los RTT debido a factores como el procesamiento del sistema y la virtualización, aunque los valores se mantienen muy bajos al tratarse de comunicación dentro de la misma red local.

La latencia es el tiempo que tarda un paquete de datos en viajar entre dos dispositivos en una red. Puede verse afectada por factores como la distancia, la congestión de la red, el procesamiento en dispositivos intermedios y el ancho de banda disponible. El jitter es la variación en el tiempo de llegada de los paquetes y es especialmente importante en aplicaciones en tiempo real como VoIP o videojuegos, ya que puede causar interrupciones o pérdida de calidad en la comunicación.








   





