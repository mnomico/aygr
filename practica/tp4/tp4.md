## [Volver atrás](/readme.md)

<div align="center">
<h1>TP 4. Simple Network Management Protocol (SNMP)</h1>
</div>

## Bibliografía

## Spanning Tree Protocol (STP)

Spanning Tree Protocol (STP) es un protocolo que permite la existencia de enlaces redundantes en una red de capa dos del modelo OSI, evitando a su vez la formación de bucles de conectividad que impactan seriamente en la performance de la red. El protocolo basa su funcionamiento en transformar lógicamente cualquier topología de red a un árbol de expansión mínima. Categorías ISO: FCAPS.

## Trabajo Práctico

1. Instalar el simulador de redes GNS3 siguiendo las instrucciones en el documento [GNS3 Installation](https://www.gns3.com/software/download).
2. Iniciar el simulador de red ejecutando, como usuario común, el comando **gns3**. Si por algún motivo este comando no funcionara, es posible utilizar una interfaz alternativa mediante el navegador web. Para utilizarla, ejecutar el comando **gns3server** y acceder a la dirección http://localhost:3080/static/web-ui/server/
3. Descargar e importar en GNS3 el laboratorio disponible en [STP_GNS.gns3project](https://drive.google.com/file/d/15YCIVXTE_rVr-d9zdDiWnKZk00vvAz1Z/view)
4. En el laboratorio de GNS3, iniciar una captura de tráfico en cualquiera de los enlaces entre switches.
5. Acceder a la terminal de configuración de cada switch y, utilizando el comando **show spanning-tree**, verificar que no se está ejecutando STP en cada uno de ellos.
6. En las PCs, vaciar las tablas ARP mediante el comando **clear arp**.
7. Realizar ping desde una PC a otra. ¿Cuántas tramas circulan por el enlace? ¿Qué sucede con el RTT en el ping?
8. Cerrar la captura (no hace falta guardarla) y detener y volver a ejecutar el laboratorio.
9. Iniciar una nueva captura de tráfico en cualquiera de los enlaces entre switches.
10. Acceder a la terminal de configuración de cada switch y habilitar STP ejecutando los siguientes comandos en la terminal de configuración:

        configure terminal
        no spanning-tree vlan 1
        spanning-tree vlan 1
        exit
        show spanning-tree brief

11. Aguardar unos instantes para que se determine el árbol STP y se establezca el estado de los puestos de cada switch.
12. Detener y guardar la captura como **captura-stp.pcap**.
13. Obtener el estado de los switches y puertos mediante los comandos:

        ```
        show spanning-tree brief
        show spanning-tree summary
        show spanning-tree
        show interfaces description
        ```

    Verificar las entradas de: MSTP ID, Root Cost, Root Port y Designated Root Bridge.
14. En base a la información obtenida y su conocimiento sobre el protocolo STP, determinar:
    1. ¿Cuál fue elegido como switch raíz de la topología? ¿Es el de Bridge-ID más bajo?
    2. ¿Qué interfaz fue seleccionada como puerto raíz en cada switch? ¿Es la de costo más bajo? ¿Es la de Port-ID más bajo? ¿Cuál fue elegida en el switch raíz?
    3. ¿Cuál es el switch y puerto designado de cada enlace? ¿Se corresponde con lo esperado según el algoritmo?
    4. ¿En qué estado quedó cada puerto? (disabled/blocking/listening/learning/forwarding)
    5. ¿Cuál es la topología del árbol generado mediante STP?
15. Seleccionar una de las tramas que portan STP en la captura y detallar en un cuadro los valores de los campos de dicha BPDU.