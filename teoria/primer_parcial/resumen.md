<div align='center'>
<h1>Resumen Primer Parcial - Administración y Gestión de Redes</h1>
</div>

## Módulo 1: Introducción a la Gestión de Redes y SNMP

El **Protocolo Simple de Gestión de Red (SNMP)** se introdujo para satisfacer la creciente necesidad de un estándar para gestionar dispositivos de Protocolo de Internet (IP). SNMP proporciona a sus usuarios un conjunto "simple" de operaciones que permite gestionar estos dispositivos de forma remota.

SNMP es un conjunto simple de operaciones (y la información que recopilan) que otorga a los administradores la capacidad de cambiar el estado de algún dispositivo basado en SNMP. Puede utilizarse para gestionar muchos tipos de dispositivos.

Otro aspecto de la gestión de redes es la monitorización de red; es decir, monitorizar una red completa en lugar de routers, hosts y otros dispositivos individuales. El estándar de **Monitorización de Red Remota (RMON)** fue desarrollado para ayudarnos a entender cómo funciona la red en sí, así como la manera en que los dispositivos individuales afectan a la red en su conjunto.

La seguridad de SNMPv1 se basa en **comunidades (communities)**, que no son más que contraseñas: cadenas de texto plano que permiten que cualquier aplicación basada en SNMP que conozca dichas cadenas obtenga acceso a la información de gestión de un dispositivo. Hay tres comunidades: `readonly`, `readwrite` y `trap`.

En SNMPv1, SNMPv2 (esta versión se llama SNMPv2c) y SNMPv3, la principal contribución a la gestión de redes de esta última versión (SNMPv3) es la **seguridad**.

### 1.1. Arquitectura de SNMP: Gestores y Agentes

En el mundo de SNMP, existen dos tipos de entidades: **gestores (managers)** y **agentes (agents)**.
*   **Gestor (NMS):** Un gestor es un servidor que ejecuta algún software capaz de manejar las tareas de gestión de una red. A menudo se denomina a los gestores como **Estaciones de Gestión de Red (NMS)**. Una NMS es responsable de realizar *polling* (sondeos) y de recibir *traps* (trampas) de los agentes en la red.
*   **Agente:** Es un software que se ejecuta en los dispositivos de red que usted está gestionando.

#### Operaciones Básicas
*   **Poll (Sondeo):** Es el acto de consultar a un agente para obtener alguna pieza de información. Esta información puede utilizarse más tarde para determinar si ha ocurrido algún tipo de evento catastrófico. La NMS puede consultar el estado de cada interfaz y tomar las medidas adecuadas si alguna de ellas está caída.
*   **Trap (Trampa):** Forma que tiene el agente para decirle a la NMS que algo ha sucedido. Se envían de forma asíncrona. La NMS es, a su vez, responsable de realizar una acción basada en la información que recibe del agente (puede tomar alguna medida). Cuando el agente nota que algo malo ha sucedido, puede enviar una trampa a la NMS. Algunos dispositivos enviarán una trampa "all clear" cuando hay una transición de un estado malo a uno bueno.

---

### 1.2. MIB y SMI

*   **SMI (Estructura de la Información de Gestión):** Proporciona una forma de definir los objetos gestionados y su comportamiento. SMI proporciona la metodología para definir los objetos.
*   **MIB (Base de Información de Gestión):** Es la definición real de los objetos; la base de datos de objetos gestionados que el agente rastrea. Cualquier tipo de estado o información a la que la NMS pueda acceder se define en una MIB. Un agente posee una lista de los objetos a los que realiza un seguimiento. Esta lista define la información que la NMS puede utilizar para determinar el estado de salud general del dispositivo.

#### MIB-II y MIBs Propietarias
*   Todos los agentes implementan **MIB-II**. El objetivo principal de MIB-II es proporcionar información general de gestión de TCP/IP.
*   El fabricante define su propia MIB (**MIB propietaria**) que implementa objetos gestionados para el estado y la información de su nuevo router.

---

## Módulo 2: Concepto de Gestión de Red y FCAPS

¿Qué es la gestión de red? Es un concepto general que emplea el uso de diversas herramientas, técnicas y sistemas para ayudar a los seres humanos a gestionar diversos dispositivos, sistemas o redes.

El modelo de gestión de red se divide en cinco áreas, conocidas por sus siglas **FCAPS**:
*   **Fault (Fallas)**
*   **Configuration (Configuración)**
*   **Accounting (Contabilidad)**
*   **Performance (Rendimiento)**
*   **Security (Seguridad)**

### 2.1. Objetivos de las Áreas FCAPS

#### A) Gestión de Fallas (Fault)
El objetivo de la gestión de fallas es detectar, registrar y notificar problemas a los usuarios de sistemas o redes, ya que el tiempo de inactividad es inaceptable.
*   **Pasos para la resolución de fallos:**
    1.  Aislar el problema utilizando herramientas para determinar los síntomas.
    2.  Resolver el problema.
    3.  Registrar el proceso que se utilizó para detectar y resolver el problema.

#### B) Gestión de Configuración (Configuration)
El objetivo de la gestión de configuración es monitorizar la información de configuración de la red y el sistema, y rastrear y gestionar los efectos que el hardware y el software tienen sobre la red. Esta información se almacena en una base de datos. A medida que cambian los parámetros de configuración de los sistemas, esta base de datos se actualiza. Puede ayudar en la resolución de problemas.

#### C) Gestión de Contabilidad (Accounting)
El objetivo de la gestión de contabilidad es garantizar que los recursos informáticos y de red sean utilizados de manera justa por todos los que acceden a ellos. Los problemas de red pueden minimizarse, ya que los recursos se dividen en función de las capacidades.

#### D) Gestión de Rendimiento (Performance)
El objetivo de la gestión de rendimiento es medir e informar sobre diversos aspectos del rendimiento de la red. Se recopilan los datos de rendimiento. Se establecen los niveles de **línea base (baseline)** basándose en el análisis de los datos recopilados y se establecen **umbrales de rendimiento**. Cuando estos umbrales se superan, es indicativo de un problema.

#### E) Gestión de Seguridad (Security)
El objetivo de la gestión de seguridad es doble:
1.  Deseamos controlar el acceso a algún recurso.
2.  Ayudar a detectar y prevenir ataques que puedan comprometer las redes y los hosts.
*   El objetivo es garantizar que solo las personas autorizadas tengan acceso físico a los sistemas vulnerables.
*   La gestión de la seguridad de red se logra mediante el uso de diversas herramientas y sistemas como firewalls, antivirus y sistemas de gestión y cumplimiento de políticas.

---

### 2.2. Implementación de Negocio y Niveles de Actividad

La gestión de red implica resolver un problema empresarial a través de algún tipo de implementación. La idea básica es reducir costos y aumentar la efectividad. Antes de aplicar la gestión a un servicio o dispositivo, debe comprender los **niveles de actividad** y decidir cuál es el adecuado:

*   **Inactivo:** No se realiza ninguna monitorización y, si se recibiera una alarma, se ignoraría.
*   **Reactivo:** No se realiza monitorización; se reacciona a un problema solo si este ocurre.
*   **Interactivo:** Monitoriza los componentes, pero debe solucionar los problemas de forma interactiva para eliminar las alarmas por efectos secundarios e aislar la causa raíz.
*   **Proactivo:** Usted monitoriza los componentes y el sistema proporciona una alarma de causa raíz para el problema en cuestión, e inicia procesos de restauración automática para minimizar el tiempo de inactividad.

---

### 2.3. Herramientas Avanzadas en la Gestión de Fallas

*   **Análisis de Tendencias:** El objetivo del análisis de tendencias es identificar cuándo los sistemas, servicios o redes están empezando a alcanzar su capacidad máxima, con suficiente tiempo de antelación para hacer algo al respecto antes de que se convierta en un problema real para los usuarios finales.
*   **Correlación de Alarmas:** Trata de reducir muchas alertas y eventos a una única alerta o a unos pocos eventos que representen el problema real (*root-cause analysis*). Mantener esta "tormenta de alertas" y alarmas lejos del operador ayuda a la eficiencia general y mejora la capacidad de resolución de problemas del personal.
*   **Limpieza de Alarmas (Clearing Alarms):** También es importante como transición de estado. Ayuda a los operadores a saber que algo está activo y operativo.

---

### 2.4. Modelos de Referencia de Gestión

Los modelos de referencia de gestión sirven como marcos conceptuales para organizar las diferentes tareas y funciones que forman parte de la gestión de red. Un modelo de referencia puede usarse como guía y ayuda a proporcionar orientación de las siguientes maneras:
*   Facilita la comprobación de la integridad de un sistema de gestión.
*   Obliga a diferenciar las distintas tareas que deben abordarse.
*   Ayuda a categorizar y agrupar diferentes funciones, e identificar cuáles están relacionadas.
*   Ayuda a identificar escenarios y casos de uso (*cases of use*) que deben recopilarse, y a reconocer las interdependencias e interfaces entre las diferentes tareas.

#### Consideraciones sobre los Modelos de Referencia:
*   No es necesario que un sistema real siga literalmente un modelo de referencia.
*   Un modelo de referencia tiene que ser de aplicación general y no puede optimizarse para ningún caso específico.
*   Es ventajoso poder "rebanar" un espacio de problemas. Siempre se pueden combinar diferentes funciones más tarde; lo difícil es desglosarlas.

---

### 2.5. Detalles Funcionales de la Gestión de Fallas y Configuración

#### Gestión de Fallas
La gestión de fallas se encarga de monitorizar la red para asegurar que todo funcione sin problemas y de reaccionar cuando este no es el caso, garantizando que los usuarios no sufran interrupciones del servicio y que, cuando ocurran, se mantengan al mínimo. Incluye:
*   Monitorización de red (gestión y procesamiento de alarmas).
*   Diagnóstico de fallas, análisis de causa raíz y resolución de problemas.
*   Mantenimiento de registros históricos de alarmas.
*   Gestión de tickets.

##### Gestión de Alarmas
Las alarmas son mensajes no solicitados de la red que indican que ha ocurrido algún evento inesperado; resultan en el envío de un mensaje a un sistema de gestión, el cual permite que una aplicación o un operador decida qué hacer. La tarea más importante consiste en recopilar alarmas de la red y asegurarse de que no se pase por alto nada importante.
*   La recopilación de alarmas puede incluir mecanismos adicionales que verifican que no se perdieron alarmas y que pueden solicitar el reenvío de las mismas. Las alarmas pueden perderse de muchas maneras: transporte no confiable (ej. UDP en SNMP), red congestionada, o si la aplicación de gestión o la base de datos no funcionaba correctamente o se estaba reiniciando justo cuando llegó el mensaje.
*   Se debe mantener una lista de alarmas actuales, las cuales deben limpiarse cuando la condición subyacente desaparece. Deben poder ser buscadas, ordenadas y filtradas por criterios como severidad, tipo de alarma, equipo afectado o la hora.
*   **Mapas de Topología:** Método popular donde los iconos representan dispositivos y las líneas representan conexiones. Pueden animarse con colores según su estado:
    *   🔴 **Rojo:** Alarmas críticas.
    *   🟠 **Naranja:** Alarmas mayores.
    *   🟡 **Amarillo:** Alarmas menores.
    *   🟢 **Verde:** Funcionamiento normal.
    *   🔘 **Gris:** Conectividad de gestión perdida.
*   Los datos históricos de alarmas pueden ser explotados para ayudar con diagnósticos y correlaciones futuras, para establecer tendencias y ver cómo las tasas y los tipos de alarmas evolucionan con el tiempo.

##### Filtrado y Correlación para Evitar Sobrecarga de Eventos
Dos técnicas tratan con la sobrecarga potencial de información de eventos:
1.  **Filtrado:** Es eliminar la información de eventos que se considera poco importante o redundante, para permitir al receptor enfocarse en la información más relevante.
2.  **Correlación:** Es preprocesar y agregar datos de eventos y alarmas, y destilarlos en información más concisa y significativa.
    *   *Manejo de duplicados:* Si ocurren duplicados de la misma alarma, se debe permitir que pase al menos 1 minuto antes de notificar al destinatario sobre la misma alarma nuevamente. El mensaje de alarma se envía de nuevo anotado con un contador que indica cuántas instancias del mensaje han ocurrido de hecho.
    *   La correlación de alarmas se refiere a una función de filtrado inteligente y preprocesamiento. Los mensajes son interceptados, analizados y comparados para identificar qué alarmas están relacionadas. En lugar de reenviar e informar muchos mensajes o eventos individuales, solo unos pocos mensajes necesitan ser enviados para agregar y resumir la información. El contenido semántico de esos mensajes aumenta dramáticamente.

##### Diagnóstico, Causa Raíz e Historial de Tickets
Cuando ocurre una falla, la capacidad de diagnosticar el problema es clave para minimizar su impacto. El diagnóstico adecuado es la base para la acción de reparación. El proceso de análisis que conduce a un diagnóstico se denomina **análisis de causa raíz**.
*   El diagnóstico es apoyado por funciones de resolución de problemas (*troubleshooting*). Las pruebas pueden usarse no solo en la resolución de problemas, sino también proactivamente para reconocer cualquier deterioro en la calidad de servicio antes de que sea perceptible para un usuario. La mejor gestión de fallas es evitar las fallas por completo.
*   **Tickets de problemas (trouble tickets):** Son una forma en la que una organización proveedora de red puede mantener el rastro de la resolución de problemas de red. Se asignan a operadores, quienes son responsables de resolver el ticket. El sistema ayuda a comunicar un problema entre operadores adjuntando automáticamente todo el historial del problema y su resolución.

#### Gestión de Configuración
Para que la red haga lo que se supone que debe hacer, primero necesita ser configurada.
*   Incluye realizar operaciones para entregar y modificar los ajustes de configuración al equipo en la red (configuración inicial para poner en marcha y cambios continuos).
*   Incluye auditar la red para recuperar su configuración actual y asegurarse de que la información del sistema de gestión sobre la red esté actualizada.
*   La gestión de configuración a nivel de servicio se denomina **provisión de servicio** (*service provisioning*). Provisionar un servicio implica poner en marcha el servicio, modificar ciertos parámetros y darlo de baja.
*   **Sincronización de Vistas:** Mantiene una vista de gestión precisa y consistente frente a contradicciones de información entre el sistema de gestión y la red física:
    *   *Reconciliación:* La información es sincronizada desde la red hacia el sistema de gestión.
    *   *Reprovisionamiento:* La sincronización fluye desde el sistema de gestión hacia la red, resultando en cambios de configuración a los dispositivos para reflejar la información en el inventario de gestión.
    *   *Reporte de discrepancias:* Las discrepancias son simplemente detectadas e indicadas para el usuario. La aplicación de gestión no toma una decisión, es responsabilidad del usuario caso por caso.
*   **Respaldo y Restauración:** Es una función crítica aplicada a los datos de configuración de su red. Si alguna vez necesita restaurar una red, es fundamental tener tal capacidad en su lugar.

#### Gestión de Contabilidad (Accounting)
Permite a las organizaciones recaudar ingresos y obtener crédito por los servicios de comunicación que proporcionan, y mantener un registro de su uso.
*   **Tarifas Planas:** Son comunes porque los clientes prefieren precios simples y predecibles. Facilita la facturación para los proveedores de servicios, aunque no elimina por completo la necesidad de contabilizar el uso del servicio.
*   **Medición:** Para rastrear el consumo de los servicios de red, se deben configurar medidores que recolecten datos de uso basados en volumen, duración y/o la calidad. Los datos recolectados deben expresarse en unidades relevantes para el servicio particular (ej. megabytes de tráfico de datos y uso de servicios de mejor esfuerzo [*best-effort*]).
*   **Detección de Fraude:** Se ocupa de rastrear y prevenir el robo de servicios de comunicación, el cual causa fugas de ingresos. También impide la calidad del servicio que reciben los usuarios legítimos porque los recursos de comunicación de forma inesperada no están disponibles.

#### Gestión de Rendimiento (Performance)
El rendimiento de las redes se caracteriza por una multitud de características de desempeño medidas con métricas:
*   **Throughput (Rendimiento):** Medido por un número de unidades de comunicación realizadas por unidad de tiempo. En la capa de enlace, es el número de bytes que se transmiten por segundo.
*   **Utilización:** Estrechamente relacionada con el *throughput*. Mientras que el *throughput* es un número absoluto, la utilización es un número relativo que expresa el *throughput* como un porcentaje de la capacidad máxima teórica del sistema.
*   **Delay (Retardo):** Medido en una unidad de tiempo. En la capa de enlace, es el tiempo que tarda un octeto transmitido en alcanzar su destino al otro extremo de la línea.
*   **Calidad:** En la capa de red, es el número o porcentaje de paquetes descartados.
*   **Monitoreo del Rendimiento:** Permite anticipar problemas y ocuparse de ellos antes de que ocurran. Al observar los valores a lo largo del tiempo, usted podría ser capaz de determinar una tendencia.
*   **Métodos de Recolección de Datos:**
    *   El *polling* constante de datos de rendimiento puede poner rápidamente de rodillas a un sistema de gestión, la red y los dispositivos que están siendo sondeados.
    *   *Flujos de eventos:* Reportar los datos como flujos de eventos.
    *   *Recolección de datos en el dispositivo:* Una aplicación de gestión le indica al dispositivo en qué tipo de datos de rendimiento está interesada. El dispositivo toma una instantánea de estos datos en intervalos de tiempo predeterminados y los registra en un archivo. Cuando la carga de procesamiento general es baja, la aplicación de gestión recupera de los dispositivos los archivos que contienen los datos de rendimiento.

#### Gestión de Seguridad
*   **Seguridad de la gestión:** Asegura que la gestión misma sea segura. Se ocupa de asegurar que las operaciones de gestión mismas sean seguras y que el acceso a la gestión esté restringido a usuarios autorizados. El acceso a las aplicaciones de gestión mismas necesita ser asegurado.
*   **Gestión de la seguridad:** Gestiona la seguridad de la red. Las amenazas de seguridad no apuntan tanto a la red, sino a los dispositivos conectados a ella. Involucra funciones como:
    *   *Detección de intrusos:* Monitorear el tráfico en la red para detectar patrones sospechosos que podrían indicar un ataque en curso.
    *   *Aplicación de políticas:* Limitar o permitir solo aumentar gradualmente la cantidad de tráfico que se dirige hacia un destino o que se origina desde una fuente.
    *   *Lista negra (blacklist):* Capacidad de poner en lista negra puertos y direcciones de red en los que se observan patrones de tráfico sospechosos y a través de los cuales los infractores pueden entrar en la red.

*Nota:* Muchos casos de funcionalidad no pueden ser categorizados fácilmente porque pueden ser usados para diferentes propósitos que caen bajo diferentes categorías de funciones. Un buen diseño de gestión de red puede ayudar a una organización a alcanzar sus objetivos de disponibilidad, rendimiento y seguridad.

---

## Módulo 3: Metodologías de Diseño de Redes

Un buen diseño de red debe reconocer que los requisitos de un cliente encarnan muchos objetivos técnicos y de negocio, incluyendo requisitos de disponibilidad, escalabilidad y seguridad. Se deben realizar elecciones de diseño de red difíciles y compensaciones al diseñar la red lógica antes de seleccionar cualquier dispositivo físico o medio.

### 3.1. Diseño Ascendente (Bottom-Up) vs. Descendente (Top-Down)

*   **Diseño Ascendente (Bottom-Up):** Cuando un cliente espera una respuesta rápida a una solicitud de diseño de red, se puede utilizar una metodología ascendente siempre y cuando las aplicaciones y objetivos del cliente sean bien conocidos. Sin embargo, aparecen problemas inesperados de escalabilidad y rendimiento a medida que aumenta el número de usuarios.
*   **Diseño Descendente (Top-Down):** Permite evitar problemas de escalabilidad y rendimiento realizando un análisis de requisitos antes. Es una metodología para diseñar redes que comienza en las capas superiores del modelo de referencia OSI antes de pasar a las capas inferiores. Centra su atención en las aplicaciones, las sesiones y el transporte de datos antes de la selección de los routers, switches y medios que operan en las capas inferiores. Es importante obtener primero una visión general de los requisitos del cliente. Reconoce que el modelo lógico y el diseño físico pueden cambiar a medida que se recopila más información.

### 3.2. Características del Análisis de Sistemas Estructurado:
*   El sistema se diseña en una secuencia descendente (*top-down*).
*   Se pueden utilizar varias técnicas y modelos para caracterizar el sistema, determinar los requisitos de los usuarios y proponer una estructura para el sistema futuro.
*   Se pone el foco en el flujo de datos, los tipos de datos y los procesos que acceden o cambian los datos.
*   Se pone el foco en comprender la ubicación y las necesidades de los usuarios que acceden o cambian los datos y procesos.
*   Se desarrolla un **modelo lógico** antes que el **modelo físico**.
*   Las especificaciones se derivan de los requisitos recopilados al principio.

La mayoría de los sistemas siguen un conjunto cíclico de fases, donde el sistema se planifica, crea, prueba y optimiza. La retroalimentación de los usuarios hace que el sistema sea rediseñado o modificado, probado y optimizado nuevamente a medida que surgen nuevos requisitos.

El diseño de red se divide en cuatro fases principales que se llevan a cabo de manera cíclica:
1.  Analizar requerimientos.
2.  Desarrollar el diseño lógico.
3.  Desarrollar el diseño físico.
4.  Probar, optimizar y documentar el diseño.

---

### 3.3. Análisis de Requerimientos de Negocio y Técnicos

*   Comprender la estructura corporativa puede ayudarlo a ubicar las principales comunidades de usuarios y caracterizar el flujo de tráfico.
*   Pida a su cliente que declare un **objetivo general** del proyecto de diseño de red y que lo ayude a comprender los **criterios de éxito** del cliente. Además, debe determinar las **consecuencias del fracaso**.
*   **Gobernanza y Cumplimiento (Compliance):** Afectan el diseño de redes. La gobernanza se refiere a un enfoque en decisiones, políticas y procesos consistentes y cohesivos que protegen a una organización de la mala gestión y de las actividades ilegales de los usuarios de los servicios de TI. El cumplimiento (*compliance*) se refiere a la adhesión a las regulaciones que protegen contra el fraude y la divulgación inadvertida de datos privados de los clientes.
*   **Disponibilidad y Resiliencia:** Muchos gerentes empresariales informan que la red debe estar disponible el 99.99 por ciento del tiempo. Podría ser un objetivo razonable para las empresas que experimentarían una pérdida severa de ingresos o credibilidad si la red estuviera caída. Las redes deben ser **resilientes**. Además de la seguridad, otro objetivo es la necesidad de **continuidad del negocio** durante y después de un desastre.
*   **Objetivos de Negocio Típicos:**
    *   Aumentar los ingresos y las ganancias.
    *   Reducir costos.
    *   Ofrecer nuevos servicios al cliente.
    *   Ofrecer un mejor soporte al cliente.
    *   Evitar la interrupción del negocio.

#### Alcance y Restricciones
*   Uno de los primeros pasos al iniciar un proyecto de diseño de red es determinar su alcance. Al analizar el alcance, puede referirse a las siete capas del modelo de referencia OSI para especificar los tipos de funcionalidad que el nuevo diseño de red debe abordar.
    *   **Segmento:** Una sola red limitada por un switch o router y basada en un protocolo particular de Capa 1 y Capa 2.
*   La identificación de las aplicaciones de su cliente debe incluir tanto las aplicaciones actuales como las nuevas.
*   **Restricciones de Negocio:** Es importante analizar cualquier restricción que afectará su diseño de red. Debe discutir con su cliente cualquier normativa (*policy*) sobre protocolos, estándares y proveedores. Su diseño de red debe ajustarse al **presupuesto** del cliente. El presupuesto debe incluir asignaciones para compras de equipos, licencias de software, acuerdos de mantenimiento y soporte, pruebas, capacitación y personal. Los presupuestos reducidos o los recursos limitados a menudo obligan a los diseñadores de redes a seleccionar la solución más asequible en lugar de la mejor solución.

---

## Módulo 4: Conceptos de Conmutación de Capa 2: STP, RSTP y VLANs

### 4.1. STP (Spanning Tree Protocol) y RSTP (Rapid STP)

El Spanning Tree Protocol (STP) es un protocolo y algoritmo para "podar" dinámicamente una topología arbitraria de switches de Capa 2 conectados en un árbol de expansión. El *spanning tree* tiene un *root bridge* y un conjunto de puertos en otros *bridges* que reenvían tráfico hacia el *root bridge*.
*   Los *bridges* envían **BPDUs (Bridge Protocol Data Units)** entre sí para construir y mantener el *spanning tree*. Las BPDU identifican al *root bridge* y ayudan a los otros *bridges* a calcular su camino de menor costo hacia el *root*.
*   Los *bridges* envían BPDU de notificación de cambio de topología (TCN) cuando los puertos del *bridge* cambian de estado. Los *bridges* envían BPDU de configuración cada 2 segundos para mantener el *spanning tree*.
*   El camino de menor costo es usualmente el camino de mayor ancho de banda, aunque el costo es configurable.

#### RSTP (Rapid STP)
El objetivo de la **Reconfiguración Rápida de Spanning Tree** era estandarizar un modo mejorado de operación de los switches que redujera el tiempo que STP tarda en converger a un árbol de expansión de menor costo y en restaurar el servicio después de fallas en los enlaces. Con el estándar original, las redes tardaban casi un minuto en converger. RSTP (802.1w) puede lograr la convergencia o reconvergencia en unos pocos cientos de milisegundos.

#### Estados y Roles de Puerto tras la Convergencia
En STP, el puente con el ID de puente más bajo se elige como el *root bridge*. El *bridge ID* tiene dos partes: un campo de prioridad y la dirección MAC del switch. Si todas las prioridades se dejan en su valor por defecto, el switch o bridge con la dirección MAC más baja se convierte en el root. Cada puente tiene un costo de ruta al root asociado. Para el *root bridge* esto es cero. Para todos los demás puentes, es la suma de los costos de ruta de los puertos en la ruta de menor costo hacia el *root bridge*.
*   **Estados de puerto en RSTP:** Los puertos de los puentes pueden estar en uno de tres estados: **Discarding, Learning y Forwarding**.
*   **Roles de puerto:**
    *   **Root:** Asignado al único puerto en un puente no raíz que proporciona la ruta de menor costo hacia el *root bridge*. Si un puente tiene dos o más puertos con el mismo costo, el puerto con el ID de puerto más bajo es seleccionado como el *root port*. Un *root port* es un puerto de reenvío.
    *   **Designated:** Asignado al único puerto conectado a una LAN que proporciona la ruta de menor costo desde esa LAN hacia el *root bridge*. Todos los puertos en el *root bridge* son puertos designados. Si hay dos o más puentes con el mismo costo, el puente con el ID de puente más bajo es el puente designado. Si el puente designado tiene dos o más puertos conectados a la LAN, el puerto del puente con el ID de puerto más bajo es el designado.
    *   **Disabled:** Asignado a un puerto que no está operativo o está excluido de la topología activa por la administración de la red. Un puerto deshabilitado es un puerto de descarte.
*   *Manejo de cambios topológicos:* En la especificación 802.1D original, un puente que detectaba un cambio de topología generaba una notificación de cambio de topología hacia la raíz. La raíz entonces inundaba el cambio hasta que expiraban los temporizadores *Maximum Age* y *Forward Delay*.

---

### 4.2. Redes LAN Virtuales (VLANs)

Una buena solución de interconexión de redes debe combinar los beneficios tanto de los routers como de los switches. Los principales beneficios de incorporar switches en los diseños de redes incluyen: mayor ancho de banda, mejor rendimiento, menores costos y configuración simplificada.

Los switches de VLAN imponen una jerarquía lógica en redes planas, pero requieren routers para manejar el tráfico entre dominios. Resuelven varios problemas importantes:
*   **Escalabilidad:** Las VLANs resuelven los problemas de escalabilidad al dividir el dominio de difusión físico en varios dominios de difusión más pequeños llamados redes LAN virtuales. El uso de VLANs reduce la cantidad de routers requeridos, ya que las VLANs crean dominios de difusión utilizando switches de Capa 2 en lugar de routers.
*   **Capacidad de formar grupos de trabajo virtuales:** Las VLANs están diseñadas para resolver cómo manejar los movimientos y cambios en un entorno empresarial sin tener que reasignar constantemente direcciones o reconfigurar las estaciones de trabajo y puertos de los usuarios. Para crear redes LAN virtuales, se asocia un ID de VLAN con cada interfaz en el switch, de modo que cuando una estación de trabajo de usuario se conecta a esa interfaz, el ID de VLAN se asocia automáticamente con el usuario, controlado centralmente por el administrador de la red.
*   **Administración simplificada:** Si un usuario se mueve dentro de una VLAN, la reconfiguración de los routers es innecesaria. Se puede reducir o eliminar otro trabajo administrativo (aunque agregan una capa adicional de complejidad administrativa).
*   **Seguridad de LAN:** Los datos sensibles pueden verse comprometidos por una red de área local; colocar solo a aquellos usuarios que pueden tener acceso a esos datos en una VLAN mejora la seguridad. Se pueden usar para controlar dominios de difusión, establecer firewalls, restringir el acceso e informar al administrador de red sobre una intrusión.

#### Canales de Comunicación y Etiquetado
Los switches proporcionan conectividad y comunicación para los usuarios individuales de la VLAN, mientras que los routers proporcionan comunicación *inter-VLAN*.
*   **Etiquetado Explícito:** Cuando un switch o puente VLAN recibe datos de una estación de trabajo, etiqueta estos datos con un identificador de VLAN que indica la VLAN de la cual se originaron estos datos.
*   **Etiquetado Implícito:** La trama no se etiqueta, sino que la VLAN de origen se determina en función de otra información, como el puerto en el que llegaron estos datos.

Las VLAN se definen a nivel de puerto del switch; la ubicación física es transparente. La mayoría de los sistemas finales no conocen los mecanismos de etiquetado; las VLANs son totalmente transparentes para el usuario final. El etiquetado no se ve fuera de los puertos troncales de VLAN, que conectan las VLANs entre switches. Los switches utilizarán el etiquetado en puertos troncales dedicados y eliminarán las etiquetas antes de reenviar la trama a un segmento de usuario. El etiquetado aumenta el tamaño de la trama y, por lo tanto, puede crear una trama ilegal (es decir, que exceda la MTU).

El etiquetado puede basarse en el puerto del que proviene, la dirección MAC de origen, la dirección de red de origen o algún otro campo o combinación de campos. Las VLANs se clasifican según el método utilizado. El switch tendría que mantener una base de datos actualizada que contenga un mapeo entre las VLANs y cualquier campo que se utilice para el etiquetado, denominada **base de datos de filtrado**.

Una vez que el switch determina hacia dónde deben ir estos datos, necesita determinar si el identificador de VLAN debe agregarse y enviarse:
*   Si van a un dispositivo que reconoce VLANs (*VLAN-aware*), el identificador de VLAN se agrega a estos datos.
*   Si van a un dispositivo que no tiene conocimiento de las VLANs (*VLAN-unaware*), el puente envía estos datos sin el identificador de VLAN.

---

### 4.3. Tipos de VLAN y Conexiones de Enlace

*   **VLAN de Capa 1: Membresía por puerto:** La membresía en una VLAN puede definirse basándose en los puertos que pertenecen a la VLAN. Una desventaja de este método es que la movilidad del usuario puede verse comprometida si el número de puertos de switch sobrantes configurados con el ID de VLAN apropiado es pequeño.
*   **VLAN de Capa 2: Membresía por dirección MAC:** La membresía en una VLAN se asocia con la dirección MAC de la estación de trabajo. El switch rastrea las direcciones MAC que pertenecen a cada VLAN. Cuando una estación de trabajo se mueve, no se necesita reconfiguración para permitir que la estación de trabajo permanezca en la misma VLAN. El principal problema con este método es que la membresía de la VLAN debe asignarse inicialmente.
*   **VLAN de Capa 2: Membresía por tipo de protocolo:** También puede basarse en el campo de tipo de protocolo que se encuentra en el encabezado de la Capa 2.
*   **VLAN de Capa 3: Membresía por dirección de subred:** La dirección de subred IP de la red se puede utilizar para clasificar la membresía de la VLAN. Generalmente toma más tiempo reenviar paquetes utilizando información de Capa 3 que utilizando direcciones MAC.
*   **VLANs de capas superiores:** Es posible definir la membresía de la VLAN basándose en aplicaciones o servicios.

Los dispositivos en una VLAN pueden conectarse de tres maneras, basándose en si los dispositivos conectados son conscientes de las VLAN (*VLAN-aware*) o no (*VLAN-unaware*):
*   **Trunk link (Enlace troncal):** Todos los dispositivos conectados a un enlace troncal, incluyendo las estaciones de trabajo, deben ser conscientes de las VLAN. Estas tramas especiales se llaman tramas etiquetadas (*tagged frames*).
*   **Access link (Enlace de acceso):** Conecta un dispositivo no consciente de las VLAN al puerto de un puente consciente de las VLAN. Todas las tramas en los enlaces de acceso deben estar etiquetadas implícitamente (sin etiqueta).
*   **Hybrid link (Enlace híbrido):** Es un enlace donde se conectan tanto dispositivos conscientes como no conscientes de las VLAN. Puede tener tanto tramas etiquetadas como no etiquetadas, pero todas las tramas para una VLAN específica deben ser o bien etiquetadas o no etiquetadas.

Dado que los IDs de VLAN son transparentes para los usuarios, esto requiere que los IDs sean insertados y eliminados por el equipo de VLAN.

---

### 4.4. Estructura de la Base de Datos de Filtrado y Estándar IEEE 802.1Q

El **IEEE 802.1Q** es un protocolo de etiquetado de VLAN que requiere que se inserte una etiqueta entre la dirección MAC de origen y el campo EtherType o longitud. Un puente, al recibir datos, determina a qué VLAN pertenecen los datos; en el etiquetado explícito, se agrega un encabezado de etiqueta (*tag header*) a los datos.

La base de datos de filtrado consta de los siguientes tipos de entradas:
*   **Entradas estáticas:** La información estática es agregada, modificada y eliminada únicamente por la administración.
    *   *Entradas de filtrado estático:* Especifican para cada puerto si las tramas que se enviarán a una dirección MAC específica y en una VLAN específica deben reenviarse o descartarse, o si deben seguir la entrada dinámica.
    *   *Entradas de registro estático:* Especifican si las tramas que se enviarán a una VLAN específica deben estar etiquetadas o no etiquetadas y qué puertos están registrados para esa VLAN.
*   **Entradas dinámicas:** Son aprendidas por el puente y no pueden ser creadas ni actualizadas por la administración. El proceso de aprendizaje observa el puerto desde el cual se recibe una trama con una dirección de origen y un ID de VLAN (VID) determinados y actualiza la base de datos de filtrado. Las entradas se eliminan mediante el proceso de envejecimiento (*aging*). Las entradas permiten la reconfiguración automática de la base de datos de filtrado si la topología de la red cambia. Hay tres tipos de entrada dinámica:
    1.  Entradas de filtrado dinámico.
    2.  Entradas de registro de grupo.
    3.  Entradas de registro dinámico.

#### Encabezado de Etiqueta 802.1Q
Las operaciones de VLAN requieren que las tramas lleven alguna forma de identificación para indicar la VLAN a la que pertenecen. Para lograr esto, se agrega un campo llamado encabezado de etiqueta (*tag header*) a las tramas LAN estándar entre la Capa 2 y la Capa 3. Las tramas etiquetadas se envían a través de enlaces híbridos y troncales; es responsabilidad de los dispositivos conscientes de las VLAN etiquetar y desetiquetar las tramas.

En las tramas Ethernet, el encabezado de etiqueta comprende un **Identificador de Protocolo de Etiqueta (TPID)** de dos bytes y un campo de **Información de Control de Etiqueta (TCI)** de dos bytes. El TPID se transpone sobre el campo EtherType estándar con un valor especial de `0x8100`, y el campo EtherType se desplaza para que siga al campo TCI.

**Campos del TCI (Tag Control Information):**
*   **Prioridad de usuario (3 bits):** Cero es la prioridad más baja y siete es la prioridad más alta.
*   **CFI (Canonical Format Indicator - 1 bit):** Se utiliza para indicar que todas las direcciones MAC presentes en el campo de datos están en formato canónico (utilizado para decodificar tramas encapsuladas).
*   **VID (VLAN Identifier - 12 bits):** Utilizado para identificar de forma única la VLAN a la que pertenece la trama. Puede haber un máximo de ($2^{12} - 1$) VLANs, con la excepción de los siguientes valores reservados:
    *   `0x000`: El VID NULL (permite que la prioridad se codifique en LANs sin prioridad).
    *   `0x001`: El VID por defecto.
    *   `0xFFF`: Reservado para uso de implementación.

---

### 4.5. Consideraciones de Dominio de Difusión y Conectividad
Si las VLAN lógicas están distribuidas a través de una LAN física, entonces el tráfico *intra-VLAN* necesita ser dispersado a todas las conexiones físicas. Esto puede llevar a sobrecargas de tráfico graves y a la consecuente degradación del rendimiento.
*   Una VLAN define dominios de difusión (*broadcast*) en la Capa 2. Un router define automáticamente dominios de difusión en cada interfaz. Cada puente virtual configurado en el switch LAN establece un dominio de difusión distinto, o VLAN. Las tramas de una VLAN no pueden pasar directamente a otra VLAN en el switch LAN; deben utilizarse dispositivos de interconexión de Capa 3, como routers, para conectar las VLAN. Todos los dispositivos que pueden comunicarse directamente sin un router comparten el mismo dominio de difusión.
*   Una característica clave de las VLAN es la capacidad de colocar los puertos de los switches en dominios de difusión virtuales. La otra característica clave es la capacidad de etiquetar tramas Ethernet con un identificador de VLAN para que los dispositivos puedan distinguir fácilmente los límites de los dominios de difusión. Los dispositivos de interconexión pueden leer las etiquetas y establecer límites de VLAN basándose en la información de la etiqueta.
*   Las etiquetas VLAN añaden 4 bytes de información entre los campos de Dirección de Origen (*Source Address*) y Tipo/Longitud (*Type/Length*) de las tramas Ethernet. El tamaño máximo de la trama Ethernet modificada aumenta de 1518 a 1522 bytes, por lo que la secuencia de verificación de trama (FCS) debe recalcularse cuando se añade la etiqueta VLAN. Los identificadores de VLAN pueden oscilar entre 0 y 4095.

#### Razones para Construir VLANs:
*   **Seguridad:** La información sensible, o el tráfico departamental, puede aislarse con redes LAN virtuales.
*   **Reducción de broadcast:** Las VLAN pueden aislar las difusiones de los protocolos para que lleguen solo a los sistemas que necesitan escucharlas.
*   **Reducción de retardo:** Las VLAN pueden utilizarse para establecer límites lógicos que no necesiten emplear un router para llevar el tráfico de un segmento LAN a otro.

---

## Módulo 5: Calidad de Servicio (QoS) en IP

Un servicio es la descripción del tratamiento general que recibe el tráfico de un cliente a través de un dominio específico. El objetivo del servicio es maximizar la satisfacción del usuario final, lo que requiere que sus aplicaciones funcionen de manera eficaz.

Definimos la "calidad" de un servicio IP en función de los requerimientos subyacentes de una aplicación, los cuales se expresan mediante las métricas de SLA (Acuerdo de Nivel de Servicio):
*   **Retardo, jitter (variación de retardo), pérdida de paquetes, rendimiento (throughput), disponibilidad del servicio y preservación de la secuencia por flujo.**

El problema de garantizar que una red cumpla con estos requerimientos es un problema de **gestión de la congestión**. Al diseñar la QoS de un servicio de red, existe implícitamente otra restricción importante: minimizar el costo. Si ocurre congestión, parte del tráfico deberá retrasarse hasta que haya capacidad o deberá ser descartado.
*   La QoS es un problema de optimización cuyo fin es maximizar la satisfacción del usuario final minimizando el costo. Minimizar el costo requiere no sobredimensionar la red para entregar esa calidad, lo que puede obligar a diferenciar los niveles de servicio ofrecidos a distintas aplicaciones.
*   **Clases de servicio:** Nos referimos a la clasificación de un flujo de tráfico agregado en una serie de clases donde se aplicarán diferentes acciones a cada “clase de servicio”.
*   **Tipo de servicio:** Nos referimos al uso del campo Tipo de Servicio (*Type of Service*) del paquete IPv4.
*   **Mejor esfuerzo (Best-effort):** Describe un servicio de red que intenta entregar el tráfico a su destino, pero que no proporciona ninguna garantía de entrega y, por lo tanto, no tiene compromisos de retardo, jitter, pérdida y rendimiento (*throughput*). El mejor esfuerzo implica la ausencia de compromisos de SLA; por lo tanto, un servicio que proporcione cualquier compromiso de SLA no puede definirse como mejor esfuerzo, por muy bajos que sean esos compromisos.

Las razones principales para usar QoS en IP se derivan del hecho de que IP es la tecnología de capa de red de extremo a extremo utilizada por la gran mayoría de las aplicaciones actuales. Para proporcionar un servicio de bajo retardo, bajo jitter y baja pérdida, la red debe estar diseñada para eliminar todos los puntos de congestión en la ruta de extremo a extremo para ese servicio; para asegurar diferentes SLAs para diferentes clases de tráfico, debemos aplicar diferenciación en todos los puntos de congestión.
*   Es posible que se utilicen diferentes tecnologías de capa 2 subyacentes para los distintos tramos de una ruta de capa 3. Dado que IP es la capa común más baja de extremo a extremo, tiene sentido fundamental utilizar técnicas de QoS en IP siempre que sea posible, y mapearlas a las capacidades de QoS en las tecnologías de capas inferiores cuando sea necesario.
*   La QoS implica el uso de una gama de funciones y características dentro del contexto de una arquitectura superior con el fin de asegurar que un servicio de red entregue las características de SLA que las aplicaciones destinadas a ese servicio necesitan para funcionar eficazmente.

---

### 5.1. Plano de Datos vs. Plano de Control

Los mecanismos utilizados para la ingeniería de la QoS en una red pueden dividirse en mecanismos del plano de datos y del plano de control, aplicados en dispositivos de red como routers:

*   **Mecanismos de QoS del plano de datos:** Se aplican en los nodos de la red y pueden impactar en el reenvío de los paquetes. Pueden categorizarse en términos de las características de comportamiento que imparten a los flujos de tráfico:
    *   *Clasificación:* Es el proceso de categorizar un flujo de tráfico agregado en un número de clases de modo que cualquiera de las siguientes acciones pueda aplicarse a cada "clase de servicio" individual.
    *   *Marcado:* Es el proceso de establecer el valor de los campos asignados para la clasificación de QoS en los encabezados de los paquetes IP o MPLS para que el tráfico pueda ser identificado fácilmente.
    *   *Aplicación de tasa máxima:* *Policing* (Vigilancia) y *Shaping* (Modelado) pueden utilizarse para imponer una tasa máxima a una clase de tráfico.
    *   *Priorización:* Se utilizan técnicas como la planificación por prioridad para priorizar algunos tipos de tráfico sobre otros, gestionando así el retardo y el jitter.
    *   *Garantía de tasa mínima:* Se pueden utilizar técnicas de planificación como *Weighted Fair Queuing* (WFQ) y *Deficit Round Robin* (DRR) para proporcionar a las diferentes clases de tráfico diferentes garantías de ancho de banda mínimo.
*   **Mecanismos de QoS del plano de control:** Suelen ocuparse del control de admisión y la reserva de recursos, y pueden utilizarse para configurar las funciones de QoS del plano de datos. Las funciones del plano de control se implementan como procesos de software, junto con otras funciones como los protocolos de enrutamiento.
    *   Solo hay un protocolo ampliamente utilizado para la señalización de QoS en el plano de control: **RSVP**.
    *   RSVP se utiliza en la arquitectura **Intserv** para realizar la reserva de recursos y el control de admisión por flujo.
    *   RSVP se utiliza en **MPLS** para el control de admisión y para establecer túneles.

Estas funciones y mecanismos de QoS se usan conjuntamente dentro del marco de una arquitectura de QoS superior, donde los mecanismos se combinan para lograr un efecto de extremo a extremo.

---

### 5.2. Mecanismos del Plano de Datos en Detalle

#### A) Clasificación de Tráfico
Es el proceso de identificar flujos de paquetes y agrupar flujos de tráfico individuales en flujos agregados, de modo que se puedan aplicar acciones a dichos flujos.
*   **Flujos y Fragmentación:** Cuando un paquete se fragmenta, la información del puerto TCP/UDP se incluye solo en el primer fragmento; por lo tanto, puede que no sea posible identificar unívocamente los fragmentos posteriores como pertenecientes al mismo flujo. IPv6 introdujo un campo de **Etiqueta de Flujo (Flow Label)** para superar este problema. La fragmentación, el cifrado o el tunelamiento pueden dificultar la clasificación de algunos flujos, ya que algunos de estos campos podrían no estar disponibles.
*   **Criterios de clasificación:**
    *   *Flujo de datos:* Es una agregación de flujos basada en algún criterio de clasificación común.
    *   *Clases de tráfico:* Es una agregación de flujos o flujos de datos individuales con el propósito de aplicar una acción común.
    *   *Clasificación implícita:* Clasificación que no requiere conocimiento del encabezado o contenido del paquete (ej. puerto físico).
    *   *Clasificación compleja:* Permite una mayor granularidad que la clasificación implícita. Consiste en identificar el tráfico basándose en los valores de campos específicos o combinaciones de campos en el encabezado del paquete IP. También puede clasificar el tráfico utilizando criterios de Capa 2, como las direcciones MAC de origen o destino.
        *   **Inspección profunda de paquetes (DPI - Deep Packet Inspection):** Capacidad de analizar más allá del encabezado del paquete, examinando los datos subyacentes (*payload*).
        *   **Inspección de estado (SI - Stateful Inspection):** Capacidad de clasificar flujos manteniendo un registro (estado) de la información contenida en paquetes sucesivos, en lugar de analizar cada paquete de forma individual.
        *(Nota: Cuando se combinan DPI y SI, la unión resulta útil para clasificar aplicaciones que no pueden identificarse por otros medios, como algunas aplicaciones peer-to-peer [P2P]).*
    *   *Clasificación simple:* Se basa en campos de los encabezados de los paquetes que han sido diseñados explícitamente para la clasificación de QoS (como el octeto ToS). No requiere comprender otros campos del encabezado IP o de los datos, y no necesita tener visibilidad de los flujos.

#### B) Campos de Marcado de Paquetes
Es el proceso de establecer el valor de los campos asignados para la clasificación de QoS en los encabezados de los paquetes IP para que el tráfico pueda identificarse fácilmente. El tráfico generalmente se marca en el sistema final de origen o lo más cerca posible:
*   **Marcado en el origen:** Si el sistema final se considera confiable, entonces se puede confiar en este marcado en el resto de la red, requiriendo solo una clasificación simple para identificar el flujo.
*   **Marcado de ingreso:** Si el sistema final no es capaz de marcar los paquetes o no se puede confiar en que lo haga correctamente, un dispositivo confiable cercano a la fuente (en el ingreso a la red) puede utilizar una clasificación implícita o compleja para identificar el flujo y marcar el tráfico.

##### Campos Utilizados
*   **Tipo de Servicio (ToS) de IPv4:** La especificación original de IPv4 definió un campo de 8 bits para la clasificación de QoS llamado octeto de *Type of Service*. Este campo ha sido sustituido por el campo de Servicios Diferenciados.
*   **Clase de Tráfico de IPv6:** Campo de 8 bits para el marcado de QoS. Este campo ha sido sustituido por el campo de Servicios Diferenciados.
*   **Flow Label (Etiqueta de Flujo) de IPv6:** Campo de etiqueta de flujo que ayuda a clasificar un flujo de forma inequívoca cuando falta información.
*   **Servicios Diferenciados (DS):** De los 8 bits del campo DS, 6 se definieron como el **Punto de Código de Servicios Diferenciados (DSCP)** y los 2 bits de menor orden para la **Notificación de Congestión Explícita (ECN)**.

*¿Por qué diferenciamos entre clasificación compleja y simple?* Porque la técnica de clasificación que elija utilizar y cómo se apliquen pueden tener un impacto en la complejidad y escalabilidad del diseño de QoS. La arquitectura de Servicios Integrados utiliza un protocolo de señalización para establecer clasificadores por flujo, y la arquitectura de Servicios Diferenciados utiliza una clasificación simple, basada en la coincidencia de agregados de tráfico identificados por el marcado del campo DSCP; donde se requiere una clasificación compleja, esta se limita a los bordes de ingreso de la red.

#### C) Policing (Vigilancia) y Shaping (Modelado) de Tráfico

##### Policing (Vigilancia)
Mecanismo que asegura que un flujo no exceda una tasa máxima definida. Un *policer* se visualiza normalmente como un mecanismo de **cubeta de fichas (token bucket)**.
*   **Cubeta de fichas simple de una sola tasa:** Tiene una profundidad de cubeta máxima definida conocida como la ráfaga *B* (*burst*), y una tasa definida *R* a la que la cubeta se llena con fichas de tamaño de un byte. Las fichas se añaden a la tasa *R* ya sea cada vez que el *policer* procesa un paquete o a intervalos regulares, hasta un máximo de fichas *B*.
    Cuando llega un paquete de tamaño *b*:
    *   Si hay al menos tantas fichas de bytes en la cubeta como bytes en el paquete (fichas $\ge b$), el paquete ha **"cumplido"** (*conformed*), y se decrementa de la cubeta un número de fichas igual a *b*.
    *   Si hay menos fichas en la cubeta que bytes en el paquete, el paquete ha **"excedido"** (*exceeded*).
    Las acciones más simples son transmitir el paquete si cumple y descartar el paquete si excede, imponiendo una tasa máxima de *R* y una ráfaga de *B* sobre el flujo.
*   **Marcador de tres colores de tasa única (SRTCM):** Se utiliza tanto para vigilar el tráfico como para marcarlo. Utiliza dos cubetas de fichas, **C** y **E** (*committed* y *excess*, o *conform* y *exceed*) con tamaños de ráfaga máximos **CBS** y **EBS**. Ambas cubetas se llenan a la misma tasa **CIR** (*Committed Information Rate*).
    Cuando llega un paquete de tamaño *b*:
    *   Si hay al menos tantas fichas en la cubeta *C* como bytes en el paquete, se decrementa la cubeta *C* por una cantidad de fichas igual a *b*. Se dice que el paquete es **verde**.
    *   Si no hay tantas fichas en *C*, se compara contra la cubeta *E*. Si hay al menos tantas fichas en *E* como bytes en el paquete, se decrementa la cubeta *E* por una cantidad igual a *b*. Se dice que el paquete es **amarillo**.
    *   Si no hay fichas suficientes en *C* ni en *E*, el paquete es **rojo** y ninguna cubeta es decrementada.
    *   *Acciones:* Los paquetes verdes y amarillos se transmiten (y se pueden marcar); los rojos se pueden transmitir, marcar y transmitir, o descartar.
*   **Marcador de tres colores de doble tasa (TRTCM):** Utiliza dos cubetas, **C** y **P** (*committed* y *peak*), con tamaños de ráfaga **CBS** y **PBS**, llenadas a tasas diferentes: *C* se llena a la tasa **CIR**, y *P* se llena a la tasa **PIR** (*Peak Information Rate*), donde `PIR > CIR` y `PBS > CBS`.
    Cuando llega un paquete de tamaño *b*:
    *   Se compara *b* con las fichas en la cubeta *P*. Si hay menos fichas en *P* que bytes en el paquete, ninguna cubeta se decrementa y el paquete es **rojo**.
    *   Si hay suficientes fichas en *P*, se compara con la cubeta *C*. Si hay menos fichas en *C* que bytes en el paquete, solo se decrementa la cubeta *P* en una cantidad igual a *b*, y el paquete es **amarillo**.
    *   Si hay suficientes fichas en *C* y en *P*, ambas cubetas se decrementan por una cantidad igual a *b*, y el paquete es **verde**.
    *   El TRTCM impone una tasa máxima de CIR y ráfaga de CBS; el exceso es marcado como fuera de contrato hasta una tasa máxima de PIR y ráfaga de PBS.
*   **Modo Color-blind vs Color-aware (Conocimiento de Color):**
    *   El *policer* tiene en cuenta cualquier marcado preexistente al determinar la acción adecuada.
    *   En modo **color-aware**, el SRTCM asegura que los paquetes premarcados como amarillos o rojos no se contabilicen contra la cubeta *C*, y que los paquetes premarcados como rojos no se contabilicen contra la cubeta *E*.
    *   Los *policers* con conocimiento de color se utilizan en los límites de confianza donde se espera que un nodo descendente (Nodo A) haya aplicado un *policer* antes de enviar el tráfico a un nodo ascendente (Nodo B), el cual aplica un *policer* sensible al color para verificar el acondicionamiento.

La **medición de tráfico** es el proceso de medir la tasa y las características de ráfaga de un flujo de tráfico con fines de contabilidad o medición.

##### Shaping (Modelado)
Mecanismo utilizado para asegurar que un flujo de tráfico no exceda una tasa máxima definida, suavizando el perfil del tráfico.
*   **Diferencia clave con Policing:** Mientras el *policing* corta los picos del tráfico de ráfaga (descartando o remarcando), el *shaping* suaviza el perfil retrasando los picos.
*   **Modelador de Cubeta de Fichas:** Cuando llega un paquete de tamaño *b*, si hay suficientes fichas en la cubeta, el paquete se transmite sin demora. Si hay menos fichas que bytes en el paquete, el paquete se encola hasta que haya suficientes fichas; cuando esto ocurre, el paquete se envía y la cubeta se decrementa en una cantidad de fichas igual a *b*.
*   **Cubeta con fugas (Leaky Bucket):** Los paquetes que llegan se colocan en una cubeta que tiene un agujero en el fondo. La profundidad de la cubeta, *B*, determina el número máximo de paquetes que pueden encolarse; si un paquete llega cuando la cubeta está llena, se descarta. Los paquetes se transmiten a una tasa constante *R*, suavizando así las ráfagas.
*   **Uso del Modelador:** Actúa como un cuello de botella artificial para imponer una tasa máxima (ej. para ofrecer un servicio de tasa reducida). Puede aplicarse a una clase individual de tráfico (encolando el tráfico retrasado en la cola para esa clase) o a un flujo de tráfico agregado que comprenda varias clases, en cuyo caso debe combinarse con un planificador.

#### D) Planificación (Scheduling) y Encolado
Un planificador de paquetes IP actúa sobre los paquetes encolados para determinar su tiempo o su orden de salida. Las colas y los planificadores se utilizan en conjunto para controlar los retardos de encolado y dar garantías de ancho de banda a los flujos.

##### Planificación por Prioridad
Puede ser con apropiación (*preemptive*) o sin apropiación (*non-preemptive*). La apropiación puede aplicarse a nivel de paquete o a nivel de cuanto (*quantum*):
*   **Con apropiación a nivel de paquete:** Un paquete de menor prioridad que se esté transmitiendo actualmente sería interrumpido (apropiado) antes de terminar por un paquete prioritario. El paquete interrumpido enviado parcialmente debe reenviarse en su totalidad, lo que causa ineficiencias de ancho de banda.
*   **Con apropiación a nivel de cuanto:** Se permite terminar el paquete que se está transmitiendo actualmente; esto evita ineficiencias pero puede dar como resultado retardo para el paquete de prioridad.
*   **Inanición (Starvation):** Dado que la cola de prioridad se atiende con prioridad sobre otras colas, si la cola de prioridad está constantemente activa, las otras colas pueden sufrir inanición. Para evitar esto, es común aplicar *policing* al tráfico que ingresa a la cola de prioridad, imponiendo una tasa máxima. Si esta tasa es menor que la del enlace, siempre habrá ancho de banda disponible para las otras colas. Al controlar la carga ofrecida a la cola de prioridad, se pueden acotar las características de retardo, jitter y pérdida de la cola.

##### Planificadores Ponderados (Work-conserving)
Si la cola de prioridad está inactiva, hay una serie de otras colas que se atienden generalmente de manera ponderada en orden FIFO. Las ponderaciones determinan la proporción del ancho de banda del enlace para cada cola.
*   **Weighted Round Robin (WRR):** El ejemplo más simple de planificación ponderada. En una ronda, el planificador visita cada cola y atiende una cantidad de tráfico determinada por sus ponderaciones. Si alguna cola está inactiva, el planificador pasa a la siguiente; por lo tanto, el ancho de banda no utilizado se redistribuye entre las colas activas en proporción a sus ponderaciones relativas. Los planificadores con esta característica se denominan **planificadores conservadores de trabajo (work-conserving)**.
*   **Weighted Fair Queuing (WFQ):** Realiza el seguimiento de una variable llamada **número de ronda (round number)**, que representa el número de rondas completas de servicio byte por byte que el planificador ha completado.
    *   Cuando un paquete llega a una cola previamente inactiva, su número de secuencia se calcula tomando el tiempo de ronda actual y sumando el tamaño del paquete multiplicado por el peso de la cola.
    *   Cuando llega a una cola activa, su número de secuencia se calcula sumando el tamaño del paquete multiplicado por el peso de la cola al número de secuencia más alto de los paquetes que ya están en la cola.
    *   El planificador atiende primero el paquete con el número de secuencia más bajo y actualiza la ronda para que sea igual al número de secuencia de ese paquete.
*   **Deficit Round Robin (DRR):** Modifica a WRR para que sea equitativo sin conocer los tamaños promedio de los paquetes. Esto se logra manteniendo un **contador de déficit** para cada cola. Un planificador DRR visita cada cola en una ronda y tiene como objetivo atender el valor de un peso o **cuanto (quantum)** de cada cola. El cuanto se define en bytes.
    *   Si el primer paquete es mayor que el cuanto disponible, no se atenderán paquetes de esa cola en esa ronda.
    *   Si hay más paquetes en la cola de los que pueden ser acomodados por el cuanto, cualquier cuanto no utilizado se arrastrará a la siguiente ronda (acumulándose en el contador de déficit); de lo contrario, el contador de déficit se reinicia.
    *   Las colas que no obtuvieron su parte equitativa en una ronda reciben una compensación en la siguiente ronda. El DRR demuestra equidad independientemente de los tamaños de los paquetes.

##### Comparativa de Planificadores:
*   **Equidad (Fairness):** Medida de qué tan cerca logra el planificador la asignación de ancho de banda pretendida. DRR y WFQ son algoritmos preferidos por su equidad.
*   **Límites de retardo en el peor de los casos (Worst-case delay bounds):** El tráfico con bajos requerimientos de retardo se atiende con una cola de prioridad estricta en lugar de una cola ponderada. Los límites de retardo en el peor de los casos dependen del algoritmo utilizado y del número de colas.
*   **Simplicidad:** Un algoritmo simple ofrece beneficios; cuantos menos ciclos y estados se necesiten para implementarlo, menor será la potencia de procesamiento y memoria requeridas, facilitando el escalado y abaratando costos. **DRR se prefiere donde se requiere un alto grado de escalabilidad**.

#### E) Evitación de Congestión (Mecanismos de Descarte)
Si la tasa de llegada de tráfico excede continuamente el ancho de banda del enlace, la cola excederá la memoria de búfer (que es finita) y los paquetes se descartarán. Las razones principales para gestionar la profundidad de una cola son acotar el retardo experimentado por los paquetes o intentar optimizar el rendimiento (*throughput*).
*   **Descarte de cola (Tail drop):** Se utiliza para establecer un límite estricto en el número de paquetes que se pueden mantener en una cola. Antes de encolar un paquete, se comprueba la profundidad de la cola; si excede el límite máximo, el paquete es descartado.
    *(Nota: Con algunas aplicaciones, si un paquete se retrasa demasiado, no servirá de nada y es mejor descartarlo en lugar de consumir ancho de banda de la red y ser descartado en el destino. Esto explica por qué es mejor limitar el retardo descartando paquetes que intentar enviarlos todos).*
*   **Descarte de cabeza (Head drop):** Es una alternativa al descarte de cola. Los paquetes se descartan desde la cabeza de la cola cuando la profundidad excede el límite máximo. Esto mejora el rendimiento de TCP al permitir que la señal de indicación de congestión llegue al emisor más rápido que esperando a que se transmita la cola completa.
*   **Descarte de cola ponderado (Weighted tail drop):** Admite más de un límite dentro de una misma cola. Si hay congestión (la tasa de llegada del tráfico $R_a$ excede la tasa de servicio $R_s$, y la profundidad comienza a aumentar), entonces un subconjunto del tráfico se descartará preferencialmente.
    *   La tasa de llegada es $R_a = R_{a1} + R_{a2}$, donde $R_{a1}$ es el tráfico a descartar preferencialmente y $R_{a2}$ es el resto.
    *   El descarte preferencial se logra aplicando un límite de cola inferior $qlimit_1$ al subconjunto de tráfico a descartar primero, y un límite superior $qlimit_2$ para el resto.
    *   Si la tasa de llegada para el resto del tráfico $R_{a2}$ es menor que la tasa de servicio de la cola $R_s$, y la ráfaga de este tráfico es menor que la diferencia entre los dos límites ($qlimit_2 - qlimit_1$), entonces en caso de congestión de la cola, solo el tráfico del subconjunto a descartar preferencialmente ($R_{a1}$) será eliminado.
*   **Random Early Detection (RED):** Mecanismo de gestión de colas activa (AQM - *Active Queue Management*). Los mecanismos AQM detectan la congestión antes de que las colas se desborden y proporcionan una retroalimentación de esta congestión a los sistemas finales con el objetivo de evitar la pérdida excesiva de paquetes y mantener un alto rendimiento de red minimizando al mismo tiempo los retrasos de encolado. Se conocen también como técnicas de "evitación de congestión".
    *   *Sincronización global:* Es un comportamiento que ocurre cuando múltiples sesiones TCP se agregan en una sola cola y, al ocurrir congestión, se excede el límite de la cola, provocando descartes de paquetes en múltiples sesiones TCP simultáneamente. Las sesiones impactadas reaccionan ralentizando su tasa de envío de forma síncrona; la congestión disminuye y el rendimiento agregado efectivo cae por debajo de la tasa de línea. Luego, como no hay congestión, las sesiones aumentan su tasa de envío de forma simultánea hasta que ocurre la congestión nuevamente, repitiendo el ciclo.
    *   *Funcionamiento:* RED intenta romper la sincronización global de TCP realizando un seguimiento de la profundidad promedio de la cola ($q_{avg}$) y utilizándola como indicación de cuándo se aproxima la congestión. Esta indicación se devuelve a los sistemas finales descartando aleatoriamente paquetes a medida que aumenta la profundidad promedio.
    RED toma la decisión de descarte antes de encolar un paquete basándose en $q_{avg}$ y parámetros configurados:
    1.  Si la profundidad promedio de la cola ($q_{avg}$) está por debajo de un umbral mínimo configurable (**$q_{minth}$**), el paquete se encola.
    2.  Si la profundidad promedio de la cola ($q_{avg}$) está por encima de un umbral máximo definido (**$q_{maxth}$**), el paquete siempre se descarta.
    3.  Si la profundidad promedio ($q_{avg}$) está entre $q_{minth}$ y $q_{maxth}$, el paquete puede ser descartado con una probabilidad creciente y aleatoria.
*   **Weighted RED (WRED):** Extiende el concepto básico de RED al permitir que se utilicen varios perfiles de RED diferentes para la misma cola. Si hay congestión en la cola, un subconjunto del tráfico en la cola se descartará preferencialmente teniendo un perfil de WRED más agresivo (ajustes de $q_{minth}$ y $q_{maxth}$ más bajos).

---

### 5.3. Arquitecturas de QoS: IntServ vs DiffServ

Las arquitecturas de QoS definen las estructuras dentro de las cuales desplegamos los mecanismos de QoS para ofrecer garantías de QoS de extremo a extremo o SLAs. Deben proporcionar el contexto en el que mecanismos tales como clasificación, marcado, vigilancia (*policing*), encolado, planificación (*scheduling*), descarte y modelado (*shaping*) se utilizan conjuntamente para asegurar un SLA especificado para un servicio.

*   **IP Precedence y Type of Service (ToS):**
    *   IP Precedence define la precedencia como “una medida independiente de la importancia de este datagrama”. Proporcionó la capacidad para marcar paquetes para determinar mediante qué cola en el planificador debería ser atendido el paquete. Las limitaciones de IP Precedence y ToS llevaron al desarrollo de las arquitecturas de Servicios Integrados y Servicios Diferenciados, resultando en la obsolescencia del campo ToS por el campo de Servicios Diferenciados (DS).
*   **Servicios Integrados (Intserv):**
    *   Diseñada para proporcionar soporte para aplicaciones con requisitos de SLA acotados (como VoIP y video).
    *   Intserv aborda el problema gestionando los recursos de ancho de banda y los planificadores **flujo por flujo**; los recursos se reservan y el control de admisión se realiza para cada flujo individual.
    *   El **Protocolo de Reservación de Recursos (RSVP - ReSerVation Protocol)** es el protocolo de señalización de extremo a extremo utilizado para configurar las reservaciones de Intserv.
*   **Servicios Diferenciados (Diffserv):**
    *   Las preocupaciones sobre la escalabilidad de Intserv llevaron a la definición de Servicios Diferenciados (DS) o *Diffserv*.
    *   Diffserv comprende componentes clave utilizados conjuntamente para admitir compromisos de retardo, fluctuación (jitter) y pérdida diferenciados de extremo a extremo en la misma red IP para diferentes tipos o clases de servicio.
    *   **Clasificación y acondicionamiento de tráfico:** El borde del dominio Diffserv es el límite proveedor/cliente. Al ingresar al dominio, el tráfico del cliente se clasifica (clasificación implícita, simple o compleja) en un número limitado de clases llamadas **agregados de comportamiento**. Estos agregados se comprueban para ver si cumplen con los perfiles acordados, denominados **Acuerdos de Acondicionamiento de Tráfico (TCAs)**. Los mecanismos de QoS se utilizan para "acondicionar" el tráfico y asegurar que cumpla con el TCA (el cual se deriva del SLA entre proveedor y cliente y define las características del tráfico).
    *   **Marcado DSCP:** Los paquetes ya vienen premarcados utilizando el Punto de Código de Servicios Diferenciados (DSCP) dentro del campo DS, o se marcan al ingresar al dominio Diffserv para identificar a qué clase pertenecen. Los nodos Diffserv posteriores solo necesitan realizar una clasificación simple utilizando el DSCP para determinar la clase de un paquete.
    *   **Comportamientos por salto (PHB - Per-hop behaviors):** El acondicionamiento en los bordes asegura que todo el tráfico que ingresa esté dentro de los TCAs comprometidos y marcado adecuadamente. En el núcleo, se aplican mecanismos de planificación y encolado basados en el marcado DSCP. Diffserv utiliza un nivel de abstracción al definir los comportamientos de reenvío de "caja negra" observables externamente, denominados **Comportamientos por Salto (PHBs)**, que se aplican al tráfico en cada salto.
    *   **Escalabilidad:** Diffserv logra la escalabilidad realizando funciones de QoS complejas por cliente y manteniendo el estado por cliente **solo en los bordes de la red**. La distribución de estas funciones en el borde facilita el escalado, ya que el borde se expande naturalmente a medida que la red crece. La escalabilidad se logra utilizando solo un número limitado de clases, que se clasifican simplemente mediante el marcado DSCP dentro de cada paquete.
    *   **Campo de Servicios Diferenciados (DS):** Redefinió el uso del campo ToS. Los primeros seis bits del campo se designan como el DSCP. Una combinación particular de bits DSCP se denomina "punto de código" (*codepoint*), el cual selecciona el PHB que el paquete experimentará en cada nodo (hasta 64 puntos de código distintos). El uso de los puntos de código CS (Class Selector) proporciona compatibilidad hacia atrás con *IP precedence*.
    *   **Per-Domain Behaviors (PDBs):** Están destinados a definir comportamientos específicos de extremo a extremo entregados por un dominio Diffserv en su conjunto. Un PDB puede considerarse como la definición de los comportamientos de reenvío de "caja negra" experimentados por una clase de paquetes a través del dominio Diffserv completo.

---

### 5.4. ECN (Explicit Congestion Notification)

La **Notificación Explícita de Congestión (ECN)** tiene como objetivo mejorar aún más el rendimiento de TCP y reducir los retrasos de encolado añadiendo la capacidad de que la red indique explícitamente a los sistemas finales cuándo ha ocurrido congestión. En lugar de utilizar mecanismos AQM como RED para descartar paquetes cuando se experimenta congestión, estos se utilizan para marcar los paquetes de forma explícita; las pilas TCP de los sistemas finales utilizarían entonces la marca del paquete para determinar cuándo ha ocurrido congestión y, por lo tanto, ralentizar su tasa de envío. ECN reduce la pérdida de paquetes y mejora el rendimiento global.

Esta indicación explícita se proporciona mediante el marcado del campo ECN (bits 6 y 7 del campo DS). El campo ECN es establecido tanto por los sistemas finales (para indicar que están utilizando un protocolo de capa de transporte con capacidad ECN) como por los routers (para indicar explícitamente cuándo se experimenta congestión).

ECN requiere los siguientes comportamientos:
1.  **Negociación:** Esto se hace durante el establecimiento de la sesión utilizando dos nuevas banderas en el encabezado TCP: **ECN-Echo (ECE)** y **Congestion Window Reduced (CWR)**. Con la negociación ECN completada, ambos extremos (A y B) pueden originar paquetes en esta sesión TCP con **ECT (ECN-Capable Transport)** establecido, indicando que ambos soportan ECN.
2.  **Marcado en el Router:** Un router que recibe un paquete ECT utiliza un mecanismo como RED para determinar si establece o no **CE (Congestion Experienced)**. El comportamiento de RED modificado para soportar ECN es el siguiente:
    *   *Sin congestión:* No se realiza ninguna marca.
    *   *Congestión moderada:* Se marca el paquete como CE en lugar de descartarlo.
    *   *Congestión extrema:* Se descarta el paquete normalmente.
3.  **Eco en el Receptor:** Al recibir un paquete con CE establecido, el receptor establece la bandera **ECN-Echo (ECE)** en su próximo ACK de TCP enviado al emisor.
4.  **Reacción en el Emisor:** Al recibir un ACK de TCP con la bandera ECE establecida, el emisor aplica los mismos algoritmos de control de congestión que aplicaría un sistema final no ECT ante la presencia de un único paquete perdido (reduciendo su ventana de congestión). El emisor también establece la bandera **CWR** en el encabezado TCP del siguiente paquete enviado al receptor para confirmar la recepción de la señal.

Los beneficios de ECN solo se obtienen realmente cuando todos los sistemas finales cooperan en la adhesión a los comportamientos de ECN; si esto no se puede asegurar, los beneficios de ECN tampoco pueden asegurarse.
