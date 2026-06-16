**INSTITUTO TECNOLÓGICO DE MORELIA** 

![](Aspose.Words.25bcdd4d-1516-43c6-b103-973989486e70.001.png)

**SEGURIDAD EN LA NUBE** 

**PRACTICA II.  \
CONSTRUCCIÓN DE UNA MINI NUBE PRIVADA.** 

**ALUMNO: DIEGO HERNÁNDEZ VÁZQUEZ \
DOCENTE: ISC. FRANCISCO ALEJANDRO GAONA ROMERO** 

**MORELIA, MICH. A 02 DE FEBRERO DEL 2026** 

<a name="_page1_x82.00_y71.00"></a>INDICE 

Contenido 

[INDICE ................................................................................................................. 2 ](#_page1_x82.00_y71.00)[RESUMEN. ............................................................................................................ 3 ](#_page2_x82.00_y71.00)[OBJETIVOS............................................................................................................ 3 ](#_page2_x82.00_y232.00)[OBJETIVO GENERAL: .......................................................................................... 3 ](#_page2_x82.00_y274.00)[OBJETIVOS ESPECÍFICOS: .................................................................................. 3 ](#_page2_x82.00_y344.00)[MARCO TEÓRICO. ................................................................................................. 3 ](#_page2_x82.00_y483.00)[DESARROLLO Y METODOLOGÍA. ............................................................................ 5 ](#_page4_x82.00_y279.00)[ANÁLISIS Y DISCUSIÓN DE RESULTADOS. .............................................................. 7 ](#_page6_x82.00_y281.00)[PROBLEMAS ENCONTRADOS Y SOLUCIONES ........................................................ 8 ](#_page7_x82.00_y304.00)[CONCLUSIONES. .................................................................................................. 9 ](#_page8_x82.00_y71.00)[BIBLIOGRAFÍA....................................................................................................... 9 ](#_page8_x82.00_y248.00)

<a name="_page2_x82.00_y71.00"></a>RESUMEN. 

En esta práctica se implementó una infraestructura en la nube simulada utilizando contenedores de Docker y la herramienta docker-compose. Se desplegó una arquitectura de tres capas que incluye una capa de presentación (Proxy Inverso con Nginx), una capa de lógica de negocio (WordPress) y una capa de persistencia de datos (MariaDB). Además, se aplicaron políticas de seguridad con ufw, aislamiento de redes y pruebas de tolerancia a fallos. 

<a name="_page2_x82.00_y232.00"></a>OBJETIVOS. 

<a name="_page2_x82.00_y274.00"></a>OBJETIVO GENERAL: 

Desplegar una arquitectura de servicios de tres capas que simule el entorno de nube privada funcional. 

<a name="_page2_x82.00_y344.00"></a>OBJETIVOS ESPECÍFICOS: 

- Configurar un entorno de IaaS mediante VMware y Ubuntu Server. 
- Orquestar servicios PaaS/SaaS usando Docker Compose. 
- Implementar un Proxy Inverso (Nginx) para gestionar tráfico externo. 
- Resolver el mapeo de puertos en YAML para habilitar el acceso por un puerto distinto al estándar (Puerto 80). 

<a name="_page2_x82.00_y483.00"></a>MARCO TEÓRICO. 

- Modelos de servicio: 
- IaaS (Infraestructure as a Service): Esta capa estuvo representada por la máquina virtual con Ubuntu Server. Se realizó la gestión del sistema operativo base, las reglas de red, el firewall (UFW) y los recursos de hardware (memoria y disco). 
- PaaS (Platform as a Service): Esta capa estuvo representada por Docker y Docker Compose. La cual proporcionó un entorno estandarizado de ejecución, donde no hubo necesidad de instalar PHP, configurar librerías de Linux o compilar código a mano; simplemente se empleó el motor de Docker como plataforma para correr contenedores a partir del archivo YAML. 
- SaaS (Software as a Service): Es el producto final, el sitio web de WordPress. Desde la perspectiva del usuario final, ellos solo consumen una aplicación web lista para usar, sin tener que preocuparse en absoluto por los servidores, las bases de datos o el código que hay detrás. 
- Arquitectura de 3 capas: 
- Capa de presentación (Nginx): 

  Actuó como la puerta principal de la infraestructura. Su función fue recibir las peticiones HTTP del navegador, enrutarlas correctamente hacia la aplicación y devolver la página web al usuario. 

- Capa de aplicación (Wordpress): 

  Es el cerebro del sistema. Esta capa procesa el código PHP, ejecuta la lógica de negocio, gestiona las sesiones de los usuarios y dictamina cómo debe verse la página. Para lograrlo, le hace consultas continuas a la base de datos. 

- Capa de Datos (MariaDB): 

  Es la bóveda. Su única función es almacenar, consultar y proteger la información de manera persistente (como los usuarios, contraseñas y posts del blog). Trabaja en segundo plano dentro de una red interna aislada, sin conexión directa a Internet. 

- Proxy Inverso: 

  Es un servidor intermediario (en mi caso, Nginx) el cual se coloca *delante* de los servidores web internos. A diferencia de un proxy normal (que protege a los clientes que navegan hacia afuera), el proxy inverso protege a los servidores recibiendo las peticiones del exterior y reenviándolas hacia adentro. 

  Éste es de vital importancia en la seguridad del servidor debido a: 

1. Ocultamiento de topología: WordPress y MariaDB jamás quedan expuestos a Internet. Si un atacante escanea la red, solo ve a Nginx; por ello, no sabe cuántos servidores hay detrás ni qué tecnologías usan.  
1. Punto único de control: Con esto me permite centralizar políticas de seguridad, instalar certificados SSL/HTTPS en un solo lugar y bloquear ataques de denegación de servicio (DDoS) antes de que lleguen a la aplicación frágil. 
- Mapeo de puertos (Port Mapping): 

  Los contenedores de Docker viven en su propia burbuja de red y están completamente aislados del exterior por defecto. El mapeo de puertos es el túnel que perfora esa burbuja para permitir que el tráfico exterior entre a un contenedor específico. 

- Sintaxis de Docker: <Puerto\_del\_Host>:<Puerto\_del\_Contenedor> 
- Explicación en la práctica (7777:80): 
- El 7777 (Host) es el puerto físico del servidor Ubuntu Server. Fue el que se abrió en el firewall (UFW). 
- El 80 (Contenedor) es el puerto interno en el que Nginx está configurado para escuchar dentro de su burbuja. 
- Con esto buscamos que Cualquier petición que llegue al puerto 7777 del servidor Ubuntu, Docker la toma y la lanza ciegamente al puerto 80 del contenedor de Nginx. 

<a name="_page4_x82.00_y279.00"></a>DESARROLLO Y METODOLOGÍA. 

![](Aspose.Words.25bcdd4d-1516-43c6-b103-973989486e70.002.jpeg)

Figura 1. Verificación de servicios, donde se puede apreciar que las 3 capas están siendo levantadas correctamente. 

![](Aspose.Words.25bcdd4d-1516-43c6-b103-973989486e70.003.jpeg)

Figura 2. Resolución de mapeo de puertos, donde se puede apreciar que dentro del Docker-

Compose se realizó para nginx. 

![](Aspose.Words.25bcdd4d-1516-43c6-b103-973989486e70.004.jpeg)

Figura 3. Acceso Externo Exitoso, donde se puede apreciar claramente el acceso mediante Kali Linux a los servicios de Docker de Ubuntu Server por el puerto personalizado. 

![](Aspose.Words.25bcdd4d-1516-43c6-b103-973989486e70.005.png)

Figura 4. Aislamiento de capas, donde al intentar acceder al puerto 3306 (MariaDB) desde una máquina externa (Kali Linux), la conexión es rechazada. 

<a name="_page6_x82.00_y281.00"></a>ANÁLISIS Y DISCUSIÓN DE RESULTADOS. 

- Elasticidad y Escalabilidad: 

Al utilizar el comando de escalado, Docker crearía múltiples instancias idénticas del contenedor de WordPress. El Proxy Inverso (Nginx) se vería beneficiado y transformaría su rol: pasaría de ser un simple enrutador a convertirse en un Balanceador de Carga. Gracias al servidor DNS interno de Docker, cuando Nginx intente enviar tráfico al servicio app, Docker resolverá esa petición distribuyendo el tráfico en formato *Round-Robin* (por turnos) entre las tres réplicas. Esto aumentaría enormemente la capacidad de la página para recibir visitas simultáneas sin saturarse. 

- Seguridad y Puertos: 

La principal ventaja es la mitigación de ataques automatizados y ruido de fondo. En Internet, existen botnets y scripts maliciosos que escanean masiva y constantemente las IPs públicas buscando los puertos 80 (HTTP) y 443 (HTTPS) para explotar vulnerabilidades conocidas. Al mover el servicio a un puerto no estándar (como el 7777), se evade esta primera ola de escaneos ciegos. 

- Diagnóstico de Fallos: 

docker logs. Si el log muestra errores como *"Access denied for user"*, el problema es de credenciales (contraseña incorrecta o usuario sin privilegios). Si dice *"Connection refused"* o *"Host unreachable"*, apunta a un fallo de red o a que la base de datos está caída. 

- Comparativa de modelos: 

En el modelo IaaS (Infraestructura como Servicio) tradicional, el proveedor entrega una Máquina Virtual "cruda". El administrador debe gastar tiempo instalando el sistema operativo, las dependencias (PHP, librerías), configurando servicios web y gestionando actualizaciones manualmente. Docker Compose evoluciona esto acercándolo al PaaS (Plataforma como Servicio) porque actúa como una plataforma de *Infraestructura como Código (IaC)*. Mediante un simple archivo YAML, el desarrollador solo declara "qué necesita" (una base de datos, un proxy, una red) y el motor de Docker se encarga del "cómo". Abstrae la capa del sistema operativo subyacente, empaca la aplicación con todas sus dependencias y garantiza que el entorno de ejecución sea idéntico en cualquier máquina, permitiendo a los equipos enfocarse en el despliegue del producto y no en la gestión del hardware virtual. 

<a name="_page7_x82.00_y304.00"></a>PROBLEMAS ENCONTRADOS Y SOLUCIONES 

1. Conflicto de Puertos 

   Problema: Al intentar levantar la infraestructura, el Proxy Inverso (Nginx) falló con el error port is already allocated. El puerto 8080 estaba secuestrado por un contenedor residual de la práctica anterior (NextCloud). 

   Solución: En lugar de apagar el servicio anterior, aproveché la situación para cumplir el "Desafío Técnico". Modifiqué el archivo docker-compose.yml para mapear Nginx a un puerto completamente nuevo y personalizado: el 7777 (7777:80), esto con ayuda de un foro dentro de Reddit donde mostraban el funcionamiento del mapeo en Docker. 

2. Brecha de Divulgación de información: 

   Problema: Al realizar una petición con curl -v, el servidor revelaba en las cabeceras las versiones exactas de las tecnologías utilizadas (nginx/1.29.5 y PHP/8.3.30), lo cual es un riesgo crítico de seguridad. 

   Solución: Se aplicaron políticas de "Seguridad por Oscuridad" añadiendo las directivas server\_tokens off; y proxy\_hide\_header en la configuración de Nginx, ocultando las versiones y blindando el servidor contra escaneos de vulnerabilidades, esto con ayuda de la Inteligencia Artificial. 

<a name="_page8_x82.00_y71.00"></a>CONCLUSIONES. 

Comprobé de primera mano la enorme ventaja de usar Docker y docker-compose frente a la administración tradicional de servidores. Me di cuenta de que al definir mi arquitectura en un archivo YAML, puedo replicar un entorno completo de tres capas en cuestión de segundos. La lección más valiosa fue entender que en la nube "los servidores son desechables, pero los datos no"; al destruir intencionalmente el contenedor de mi base de datos y ver cómo el sistema se recuperaba intacto gracias a los volúmenes de almacenamiento. 

<a name="_page8_x82.00_y248.00"></a>BIBLIOGRAFÍA. 

- *Docker Compose*. (2026, enero 7). Docker Documentation; Docker Inc. [https://docs.docker.com/compose/ ](https://docs.docker.com/compose/)
- *NGINX reverse proxy*. (s/f). Nginx.com. Recuperado el 25 de febrero de 2026, de [https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy ](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy)
- (S/f). Reddit.com. Recuperado el 25 de febrero de 2026, de [https://www.reddit.com/r/docker/comments/1kgpa6j/help_changing_port_in_docker _compose/?tl=es-419 ](https://www.reddit.com/r/docker/comments/1kgpa6j/help_changing_port_in_docker_compose/?tl=es-419)
