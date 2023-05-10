# Ejercicio CiberKillChain - Ataque

## Alumno

Gustavo Hernan Siciliano
#
## Enunciado

Armar una cyberkillchain usando técnicas de la matriz de Att&ck para un escenario relacionado al trabajo práctico de la carrera.
#
## Datos trabajo práctico

[Documento de GdP sobre el proyecto](https://github.com/gussiciliano/Plantilla-planificacion/blob/master/charter.pdf)

Proyecto: sistema de riego y control de huertas.

Breve descripción:
El proyecto consiste de un modulo IoT que sensará la información de humedad ambiente y del sustrato de una huerta. Enviará esa información a un Backend para su centralización y posterior disponilización en un Fronent. En las vistas se podrá configurar rangos de aceptación de humedad, para que el sistema manipule de forma automatica una válvula que accionará la apertura o cierre de una maneguera de riego. Se espera que el proyecto genere monitoreo y cuidado (tanto manual como automático) de los cultivos en una huerta.
#
## Resolución

### Objetivo del ataque

Dañar los huertos lentamente. Se quiere inundar o secar los cultivos sin que el sistema note que esto se produce por acciones externas a los componentes del proyecto.
  
### Cyberkillchain.

### 1. Reconnaissance.
> Descripción: El atacante junta información de la organización a la cual quiere hacer un daño. Se arma un grafo de información donde el nodo central es la victima.

Uso de "Gather Victim Host Information" [T1592](https://attack.mitre.org/techniques/T1592).

Pasos:
  - Lectura de la documentación del proyecto (capa física y lógica).
  - Replicar el modelo de las placas y sensores.
  - Analizar qué información se manda a la capa lógica.
  - Analizar cómo se envían los datos desde las placas ESP al servidor.
  - En este análisis se descrubre que los mensajes no tiene encriptación y se presume una poca validación de los datos enviados al servidor.

### 2. Weaponization.
> Descripción: Definición y diseño del plan para atacar a la orgnización.

Uso de "Acquire Infrastructure" [T1583](https://attack.mitre.org/techniques/T1583/).

Pasos:
  - Diseño un software de replicación e inflitración, que pueda simular se una placas ESP del sistema.
  - Diseño un software recepción y redirección de datos, que reciba el mensaje de una placa correcta y pueda retrasmitirlo. Se espera que se haga pasar por cualquier conjunto de huertas.
  
### 3. Delivery.
> Descripción: Se pone en marcha el inicio del plan. Se envia los recursos necesario para comenzar con el ataque.

Uso de "Exploit Public-Facing Application-APT39" [T1190.APT39](https://attack.mitre.org/groups/G0087/).

Pasos:
  - Envío mensajes al servidor simulando ser una dispositivo correcto por parte de replicación.
  - Parte de los mensajes tienen scripts de infiltración, usando técnicas de SQL injection.
  - Se espera a que una de estas técnicas penetre en la red y pueda devolver información de credenciales.
  
### 4. Exploit.
> Descripción: El o los recursos iniciales del ataque logran entrar en la organización de forma controlada (sin ser detectados como una potencial amenaza).

Uso de "Account Discovery" [T1087](https://attack.mitre.org/techniques/T1087/).

Pasos:
  - Los mensajes del software de replicación son aceptados y se con la infiltración se obtienen las credenciales de los dispositivos, del server y las URLs de obtención de datos de los sensores.
  
### 5. Installation.
> Descripción: El o los recursos lograr instalarse y/o acomodarse dentro de la organización, también de forma controlada.

Uso de "Access Token Manipulation" [T1134](https://attack.mitre.org/techniques/T1134/).

Pasos:
  - El software de recepción logra hacerse pasar por un gran número de credenciales de los dispositivos. De esta forma comienza a recibir las métricas de estos, y su lugar comienza a redireccionar los mensajes de las placas (sin alteraciones) para evaluar la reacción del software sin levantar sospechas.

### 6. Command & Control.
> Descripción: Se establece una mecanismo de comunicación con el o los recursos que lograron instalarse dentro de la organización. Esto también se hace sin generar sospechas.

Uso de "Proxy" [T1090](https://attack.mitre.org/techniques/T1090/).

Pasos:
  - Cuando se llega a la conclusión de que el servidor toma los mensajes redireccionados, sin alteraciones, como mensajes validos, se procede a iniciar el ataque.
  
### 7. Actions on Objectives.
> Descripción: Se llega al punto final donde se concreta el objetivo del ataque.

Uso de "Data Manipulation" [T1565](https://attack.mitre.org/techniques/T1565/).

Pasos:
  - Se comienzan a cambiar las mediciones para que se sequen los cultivos (simulando tener buenas condiciones de humedad) o bien inundando los cultivos (simulando que tienen malas condiciones de humedad).
