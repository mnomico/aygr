## [Volver atrás](/readme.md)

<div align="center">
<h1>TP 1. Herramientas de Diagnóstico de Redes</h1>
</div>

**Objetivo**: Conocer el funcionamiento y familiarizarse con las herramientas que permiten explorar, diagnosticar problemas y medir diferentes aspectos de una red. Categorías ISO: FCAPS.

Tanto en este práctico como en los sucesivos, es sumamente conveniente que realicen una transcripción de lo escrito y obtenido por pantalla. Para ello, antes de iniciar la práctica, ejecute el comando **script guardado_de_salida_fecha.txt**. Al finalizar los ejercicios, escriba el comando **exit** para cerrar y guardar el archivo. Toda la documentación de los comandos que se utilizarán se puede obtener con **man comando**.

## Bibliografía

- OPPENHEIMER, P., 2011, Top-Down Network Design (3d ed). CISCO Press.
    - Capítulo 2. Sección “Network Performance” (pp. 32-44)
    - Capítulo 3. Sección “Checking the Health of the Existing Internetwork” (pp. 71-81)

## Trabajo Práctico

### ping (RTT)

1. Instale y configure la herramienta [SmokePing](http://smokeping.org/). Mida durante al menos dos horas contra 3 hosts localizados en distintos continentes (puede emplear aquellos de la consigna previa). Adjunte los gráficos correspondientes a las mediciones realizadas y comente los comportamientos que puede observar a partir de ellos. Encontrará una breve [guía de configuración de la herramienta](http://www.labredes.unlu.edu.ar/sites/www.labredes.unlu.edu.ar/files/site/data/aygr/smokeping.pdf) adjunta a esta práctica.
2. ¿Qué comportamiento se observa? ¿Qué implicaría un incremento/disminución de la latencia a partir de un patrón establecido? ¿Qué otras utilidades ofrece esta herramienta?
3. ¿De qué manera afecta la latencia a las aplicaciones? Describa y brinde ejemplos.
4. Configure Smokeping para que además de medir latencia contra los destinos consignados, mida el uptime de tres servicios web: un portal de noticias, una app web y una API REST pública. Adjunte la configuración utilizada.
5. Conceptualmente, ¿qué diferencias principales hay entre medir latencia contra un destino mediante ICMP y medir latencia mediante protocolo HTTP/S?

### traceroute

1. Verifique la ruta que podría llegar a seguir un paquete IP hacia el host **8.8.8.8** (servidor DNS público de Google). Ejecute la misma consulta varias veces y en momentos distintos ¿Qué conclusión puede obtener?
2. En qué situaciones puede llegar a ser útil esta herramienta. Ejemplifique.
3. En una red externa a la Universidad, realice **traceroute** al sitio web **www.unlu.edu.ar** y otro a **www.ut.ee**. Indique el ISP que provee el servicio de conectividad a Internet en ese momento. Adjunte la salida del traceroute. En clase, compare su salida contra el de sus compañeros. ¿Hay dispositivos (hosts o direcciones IP) en común entre las salidas de sus pares? ¿Cuáles son?

### nmap (exploración de la red)

1. Lea el manual de la herramienta y ejecute el ejercicio 1 de la experiencia de laboratorio contra **localhost**. Comente que información adicional visualiza respecto al ejercicio anterior. Compárelo con la ejecución del ejemplo 1 al dominio de la UNLu, y comente brevemente por qué una mala configuración puede representar un riesgo de seguridad.
2. Una de las ventajas de nmap es que permite, mediante comodines o con formato CIDR, hacer un escaneo completo de un segmento de red para descubrir dispositivos presentes en la misma. Busque en el manual la sección “TARGET SPECIFICATION” (o bien en español: ESPECIFICACIÓN DE OBJETIVOS) y deduzca como puede encontrar todos los dispositivos conectados a su red. Puede ver su dirección IP actual mediante el comando **ip addr show**.
3. Investigue que opción permite hacer escaneo de puertos UDP y luego utilícela contra un host particular. ¿Por qué podría resultar útil realizar un análisis de ésta característica, si la mayoría de los servicios de red utilizan TCP?
4. Instale en un equipo de su hogar la aplicación **nmap** y realice un escaneo a toda la red de su hogar. ¿Que ha logrado descubrir? ¿Existen otros host aparte de su equipo? ¿Qué puertos poseen en escucha? ¿se corresponden con los servicios que usted esperaba?

### iperf (throughput)

1. En el caso de TCP: Realice mediciones empleando diversos tamaños de ventana. Considerando valores: 1kb, 2kb, 16kb, 128kb, 320kb, 10mb Confeccione una gráfica que represente el throughput respecto del tamaño de ventana efectivamente asignado por el programa.
2. ¿Qué permite establecer la opción **-M**? ¿Cómo afecta esto al throughput? Investigue la técnica “Path MTU discovery”.
3. ¿Qué efecto presenta la opción **-N**? ¿Qué tipo de aplicaciones pueden requerir tal utilidad?

### iptraf (estadísticas de uso de la red)

[iptraf](http://iptraf.seul.org/1.4/manual.html#intro) es una herramienta para monitorizar redes IP. Intercepta los paquetes que cursan la red y presenta varias estadísticas acerca del tráfico actual en ella.

1. Inicie la utilidad mediante el comando **iptraf-ng** (como usuario **root**).
2. Consulte las opciones “IP Traffic Monitor”, “Detailed Interface Statistics” y visite el sitio web de la UNLu y otros sitios. ¿Qué información proporciona cada opción?

### ntop (estadísticas de uso de la red)

Herramienta para el monitoreo y análisis de tráfico en la red. Provee una interfaz web para los reportes muy completa e intuitiva.

1. Instale **ntop** en su distribución o mediante docker siguiendo las [instrucciones de instalación](https://www.ntop.org/support/documentation/software-installation/). El servicio levanta automaticamente, si no lo hace Iniciar ntop en su forma básica: como root o con sudo: **ntop -i <interfaz de red>**
2. Luego ingrese vía web browser a http://localhost:3000. Debería estar visualizando la interfaz de ntop.
3. Revise las pestañas "Flows", "Hosts". Comente muy brevemente las opciones que le resulten mas útiles o interesantes. Si visualiza poca información, navegue por un par de sitios externos y vuelva a recargar la pagina de Ntop (F5).
4. ¿Por qué cree que Ntop debe ejecutarse con permisos de root?
5. Tras algunos minutos de captura, obtenga los siguientes informes. Indique cómo los obtuvo y qué información relevante puede usted derivar de ellos:
    
    1. ¿Qué hosts en su red son los que más intercambian datos?
    2. ¿Qué protocolos de aplicación son los que más tasa de transferencia entrante o saliente cursan?
    3. ¿Qué destinos son los más contactados?
    4. ¿Qué puertos origen y destino son los más utilizados?
    5. Si el administrador necesita revisar la actividad de la red por periodo de tiempo, ¿cuál listado de ntop ofrece una mejor visualización al respecto?
    6. ¿Cómo se pueden establecer alertas mediante Ntop?
    7. Si necesita ver con ntop un resumen del trafico de los protocolos de la Capa de Aplicación del stack TCP/IP, ¿a que opción debería dirigirse?
    8. Si el administrador necesita revisar la actividad de la red por periodo de tiempo, cual listado de ntop ofrece una mejor visualización al respecto.

### Herramientas gráficas

1. Investigue qué herramientas gráficas existen para monitorear redes y centros de datos. Seleccione una y comente sus funcionalidades.

## Guía de Lectura

De la siguiente lista de herramientas, lea la sinopsis, la descripción y los ejemplos de las páginas de manual correspondiente a cada una de ellas (man herramienta en la terminal o bien [en línea](https://manpages.ubuntu.com/)):

    ps, htop, netstat, ip link, ip addr, ip neigh, ss, mii-tool, ping, arping, traceroute, netperf, iperf, iptraf (paquete iptraf-ng), tcpdump, tshark, rfkill, inxi, wireshark, speedtest (speedtest-cli), vnc (paquete tigervnc-viewer), ssh, dig, nslookup, hping (paquete hping3), iotop, nmcli, curl, wget, smokeping, ab/apachebench (paquete apache2-utils), nmap, nc (paquete netcat), telnet, iw, resolvectl, dsniff, ethtool, nethogs, wrk, sockperf, jmeter, h2load (paquete nghttp2-client)

Categorice las mismas según su objetivo principal:

- Configuración y/o determinación de estado
- Prueba de conectividad
- Determinación y prueba de caminos
- Pruebas de throughput
- Captura de tráfico/paquetes
- Descubrimiento de dispositivos y servicios
- Determinación de la performance
- Pruebas de protocolos de capa de enlace y/o red
- Pruebas de protocolos de capa de aplicación
- Pruebas de seguridad
- Acceso remoto
