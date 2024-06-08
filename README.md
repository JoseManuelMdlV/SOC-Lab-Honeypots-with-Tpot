# SOC Lab: Honeypots with Tpot

## Objetivo
En este proyecto se busca crear un honeypot, una trampa para hackers que intentan entrar en una red privada, y desplegarlo de tal manera que se pueda ver el tráfico entrante del exterior de la red y poder monitorizar los movimientos de los instrusos. Esto, trasladado a un entorno empresarial, añade una capa más de defensa y permite estudiar patrones de comportamiento y las tácticas de los hackers, ayudado a entrenar al Blue Team para poder detectar posibles fallos en la seguridad de la red y ver cómo mitigarlos. 

## Habilidades desarrolladas
- Uso de Docker
- Virtualiización con Oracle VM VirtualBox
- Uso de la consola de comandos de sistemas Linux (Bash)
- Instalación y despligue de Honeypots
- Resolución de problemas en el ámbito de la IT y ciberseguridad

## Desarrollo del proyecto
### Consideraciones previas
- Este proyecto se realizará siguiendo las instrucciones de los propios creadores de Tpot (https://github.com/telekom-security/tpotce).
- El proyecto no está terminado y se presentarán todos los errores que he cometido.


1. Instalando T-Pot

Siguiendo las instrucciones presentes en la página de github de Telekom, empezamos viendo la posibilidad de instalar Tpot en una máquina virtual. En concreto, se elige Oracle VM VirtualBox al ya estar familiarizado con la herramienta. Acto seguido elegimos una distribución que esté soportada, que será Debian al estar basada en Ubuntu y ya estar familiarizado con este al haber montado una máquina virtual con una distrirbución Kali para hacer los ejercicios de PicoCTF y el curso de Docker. 

Una vez creamos la máquina con las especificaciones de hardware que nos indican (el hardware real del host cumple con los requisitos), instalamos Debian con los paquetes y servicios mínimos indispensables e instalamos curl con `$ sudo apt install curl` e instalamos tpot 

`env bash -c "$(curl -sL https://github.com/telekom-security/tpotce/raw/master/install.sh)"`

Usar este método, sin embargo, acaba haciendo que tengamos un error, no encontrando el archivo tpot.yml. Se initenta encontrar el origen del error para poder solventarlo, pero tras no encontrar nada, se opta por la instalacón a través de la copia de todo el repositorio haciendo uso de la instrucción git clone previa instalación de git.

`$ git clone https://github.com/telekom-security/tpotce -> $ cd tpotce -> $ ./install.sh`

Seguimos las instrucciones del instalador e instalamos la versión Standard/HIVE de T-pot, creando al final un usuario que nos servirá para acceder al entorno web de T-pot a través del puerto correspondiente. Terminada la instalación, hacemos un reboot y comprobamos si se han iniciado los contenedores pertinentes.

2. Comprobando el funcionamiento de T-pot

Antes de hacer nada con T-pot, tenemos que asegurarnos que todo el tráfico entrante estará bloqueado para no comprometer la red, pues una vez comencemos a trastear con el pot, es probable que permitamos el paso de cualquier agente malicioso sin darnos cuenta.

T-pot permite conectarte a los honeypots de diferentes formas, como a través de conexiones ssh `ssh -l <OS_USERNAME> -p 64295 <your.ip>` o a través de un servidor web `https:\\<your.ip>:64297`. Si nos conectamos al servidor web usando la ip 172.0.0.1 o localhost, podremos ver la landing page de T-pot, la cual se muestra en la figura 1. Desde aquí, podemos entrar a las diferentes herramientas que nos proporciona T-pot con simplemente clickar los botones. Por ejemplo, podemos clickar el botón "Attack Map" para ver un mapa mundi como el que se muestra en la figura 2, en el cual podremos ver desde dónde nos atacan, además de recibir información tan importante como la dirección IPv4 del atacante.

![image](https://github.com/JoseManuelMdlV/SOC-Lab-Honeypots-with-Tpot/assets/83475119/d4ef9fd0-c3b0-4268-a2fe-53a077b6a3ea)

<b>Fig.1:</b> Landing Page de T-pot

Si clickamos en Kibana, lo que obtendemos será la posibilidad de ver una serie de gráficos y visualizaciones muy detallados con la información referente a los ataques que nos van intentando hacer, como lo que se puede ver en la figura 3.

![image](https://github.com/JoseManuelMdlV/SOC-Lab-Honeypots-with-Tpot/assets/83475119/026f56b7-9f30-4a9d-a115-a0a5dab709ef)

<b>Fig.2:</b> Attack Map de T-pot

En mi caso particular, nada más instalar T-pot y aislar el ordenador de la red, intento iniciar el Attack Map para comprobar que todo funciona bien, pero soy incapaz de conectarme. Lo mismo ocurre con el resto de los servicios. 

Tras mirar todo y volver a revisar que no ha habido problemas con la instalación y que todo está en orden, realizo un docker run `$ docker run <ID del Web Map>` para comprobar si el problema está en que no se haya iniciado el container. Una vez termina el run, vuelvo a intentar iniciar el Attack Map, consiguiendo esta vez que aparezca el mapa. Cyberchef y Spiderfoot también son accesibles, no así Kibana y Elasticvue.

![image](https://github.com/JoseManuelMdlV/SOC-Lab-Honeypots-with-Tpot/assets/83475119/933db5d4-b01b-4700-932a-af149a2a614b)

<b>Fig.2:</b> Visualizaciones de Kibana

Realizar un docker run de la imagen de Kibana termina arrojando un error relacionado con elasticvue. El error de este paso aún estoy investigando cómo solucionarlo. No tengo el suficiente conocimiento técnico como para ser capaz de entender lo que está pasando con solo leer el mensaje de la consola y solucionarlo (o buscar la información precisa). Si a lo largo de los días soy incapaz de resolver el problema por mí mismo, el siguiente paso será preguntar directamente a Telekom para ver si es posible que me indiquen cómo solucionarlo. 
