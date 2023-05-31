# Ejercicio CiberKillChain - Defensa

## Alumno

Gustavo Hernan Siciliano
#
## Enunciado

Desarrollar la defensa en función del ataque planteado en orden inverso.

Para cada etapa elegir una sola defensa, la más importante, considerar recursos limitados.

## Resolución

### Ataque realizado

Se dañaron los huertos lentamente. Se inundaron o se secaron diferentes cultivos sin que el sistema note que esto se produce por acciones externas a los componentes del proyecto.

### Cyberkillchain al inverso para definir mitigaciones posibles

### 7. Actions on Objectives.
De manera física se hacen vistas a los cultivos de la huerta. A medida que pasa el tiempo se nota que el sistema no funciona bien (ya sea porque los cultivos estan secos o inundados). Se asume que los dispositivos no funcionan correctamente. En este punto hay dos opciones:
1- Se tiene conocimiento del desarrollo del sistema.
2- No se tiene conocimiento del desarrollo, pero se tiene un contacto de alguien que si lo posee (por ejemplo, con un mail del equipo de desarrollo en la web del sistema).

A través de conocer el desarrollo (o tener el contacto de alguien que lo conoce) se hacen pruebas sobre los sensores para valdiar sus mediciones. Como andan correctamente, se llega a la conclusión de que anda mal la placa o la transferencia de información.

Antes de continuar, y debido a que no se encontró la solución, se quita el dispositivo de la huerta para dejar de dañar los cultivos.

Se procede a validar el buen funcionamiento del ESP32 y de sus datos enviados, llegando a la conclusión de que andan correctamente.

Lógicamente quedan dos opciones:
1- La capa lógica del sistema IoT tiene bugs y no anda correctamente.
2- Estamos teniendo ataques externos sobre el sistema IoT.

La primer opción se descarta rápidamente porque se cuenta con una buena covertura de casos de pruebas y los tests cubren todos los endpoints y casos de la capa lógica. Solo para terminar de descartar la opción se prueba el sistema en un servidor replica, llegando a la conclusión de que funciona correctamente. Por lo tanto, se presumen que el sistema tiene vulnerabilidades no conocidas que están siendo explotadas por uno o varios agentes externos.

A pesar de que no se sabe realmente si se tiene un ataque, ni de que tipo es, se comienza a analizar las vulnerabilidades del sistema IoT, para esto se pone foco en la capa de trasferencia de información para ver que el trafico de datos del sistema sea el correcto. Cuando se encuentra que hay mediciones que vienen de fuentes que no son dispositivos del sistema, y que dispositivos bajo observación no mandan los datos correctos, se analiza esa capa.

### 6. Command & Control y 5. Installation.
> Uso de M1043 Credential Access Protection

Para evitar que al sistema lleguen mensajes de agentes externos se agregan credenciales.
De esta forma se regulariza la información que llega al sistema

### 4. Exploit.
> Uso de M1036 Account Use Policies

Para evitar el acceso indevido a credenciales se mejoras las politicas de guardado de passwords y de accesos por parte de los dispositivos.
Además se cambia la arquitectura para, en conjunto con las credenciales, se canalice la información un Broker MQTT que use certificados.

### 3. Delivery, 2. Weaponization y 1. Reconnaissance.
> Uso de M1047 Audit

Para evitar futuros ataques se implementan diferentes auditorias sobre el sistema y sobre su documentación pública para prevenir de otras vulnerabilidades.
