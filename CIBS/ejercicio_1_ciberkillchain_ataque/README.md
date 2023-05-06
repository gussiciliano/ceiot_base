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

### 2. Weaponization.
> Descripción: Definición y diseño del plan para atacar a la orgnización.

Uso de "Acquire Infrastructure" [T1583](https://attack.mitre.org/techniques/T1583/).

Pasos:
  - Diseño un software (nombre: s1) que pueda simular se una placas ESP del sistema.
  - Diseño un software (nombre: s2) que reciba el mensaje de una placa correcta y pueda retrasmitirlo. Se espera que se haga pasar por cualquier conjunto de huertas.
  
### 3. Delivery.
> Descripción: Se pone en marcha el inicio del plan. Se envia los recursos necesario para comenzar con el ataque.

Uso de "Compromise Software Supply Chain" [T1195.002](https://attack.mitre.org/techniques/T1195/002/).

Pasos:
  - Envío mensajes al servidor simulando ser una dispositivo correcto por parte de s1.
  
### 4. Exploit.
> Descripción: El o los recursos iniciales del ataque logran entrar en la organización de forma controlada (sin ser detectados como una potencial amenaza).

Uso de "Account Discovery" [T1087](https://attack.mitre.org/techniques/T1087/).

Pasos:
  - Una vez que se acepten los mensajes simulando ser una placa (s1). En el siguiente mensaje se establece la conexión con el software de simulación de un conjunto de dispositivos (s2). Se espera poder hacerse pasar por todos los dispositivos que manden al menos un mensaje real.
  
### 5. Installation.
> Descripción: El o los recursos lograr instalarse y/o acomodarse dentro de la organización, también de forma controlada.

Uso de "Access Token Manipulation" [T1134](https://attack.mitre.org/techniques/T1134/).

Pasos:
  - El software (s2) logra hacerse pasar por un gran número de dispositivos y comienza a redireccionar los mensajes de las placas (sin alteraciones) para evaluar la reacción del software sin levantar sospechas.

### 6. Command & Control.
> Descripción: Se establece una mecanismo de comunicación con el o los recursos que lograron instalarse dentro de la organización. Esto también se hace sin generar sospechas.

Uso de "Proxy" [T1090](https://attack.mitre.org/techniques/T1090/).

Pasos:
  - Se utiliza un software intemedio que podrá conectarse con el malware de simulación (s2).
  
### 7. Actions on Objectives.
> Descripción: Se llega al punto final donde se concreta el objetivo del ataque.

Uso de "Data Manipulation" [T1565](https://attack.mitre.org/techniques/T1565/).

Pasos:
  - Se comienzan a cambiar las mediciones para que se sequen los cultivos (simulando tener buenas condiciones de humedad) o bien inundando los cultivos (simulando que tienen malas condiciones de humedad).
