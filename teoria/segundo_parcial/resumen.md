<div align='center'>
<h1>Resumen Segundo Parcial - Administración y Gestión de Redes</h1>
</div>
---

## Módulo 1: VoIP (Voz sobre IP)

La **Voz sobre IP (VoIP)** es un conjunto de protocolos y tecnologías que permiten transmitir voz o video a través de redes basadas en el protocolo IP. En lugar de usar la red telefónica tradicional, las comunicaciones de voz se digitalizan, se dividen en paquetes de datos y se envían por la red.

Para que esto funcione correctamente, deben resolverse tres cuestiones principales:
1.  **Digitalización:** Cómo digitalizar la multimedia.
2.  **Paquetización:** Cómo transmitirla por la red.
3.  **Señalización:** Cómo establecer los canales de comunicación entre los dispositivos, considerando que pueden tener diferentes capacidades.

### 1.1. Ventajas de VoIP
VoIP ofrece numerosas ventajas frente a la telefonía tradicional. Dependiendo de la arquitectura utilizada:
*   Proporciona mayor versatilidad y un mejor control de las funcionalidades de comunicación.
*   Reduce los costos operativos y de instalación.
*   Simplifica el diseño de red y permite reutilizar la infraestructura existente, ya que todos los datos (voz, video y otros servicios) viajan por el mismo cable de red sin requerir un cableado telefónico adicional.
*   Es un sistema flexible que cuenta con hardware dedicado para terminales (teléfonos VoIP) y soluciones por software (*softphones*).
*   Facilita la administración y la contabilidad, y no requiere personal altamente especializado de telefonía tradicional.
*   Se adapta fácilmente a cambios o a la incorporación de nuevos teléfonos sin necesidad de recablear la red.

### 1.2. Componentes y Terminales
*   **PBX (Private Branch Exchange - Central Telefónica):** Es un dispositivo que proporciona enrutamiento de llamadas centralizado y aplicaciones de telefonía para un grupo de teléfonos empresariales. Pueden ser propietarias o basadas en software libre (con internos y funcionalidades definidas). Su configuración requiere conocimientos técnicos.
*   **Dialplan (Plan de Marcación):** Es el conjunto de reglas que rige el comportamiento de enrutamiento de llamadas de la PBX y es el centro de comando del sistema Asterisk. Cuando un usuario marca un número, el sistema busca en el dialplan la acción a realizar (llamar a un interno, redirigir la llamada, reproducir un audio o ejecutar un script).
*   **Terminales (Endpoints):**
    *   *Softphones (Terminales de Software):* Aplicaciones telefónicas que se ejecutan en computadoras, laptops o dispositivos móviles. Son económicos y fáciles de administrar, pero dependen de que el usuario tenga acceso a Internet y el dispositivo encendido.
    *   *Teléfonos IP Físicos (Hardphones):* Dispositivos de hardware compatibles con TCP/IP que se conectan directamente a la red de datos, típicamente a través de una conexión Ethernet RJ45. Internamente, un teléfono IP ejecuta una aplicación de software que implementa protocolos VoIP como SIP, SCCP o H.323.

---

### 1.3. Arquitectura de un Sistema VoIP

El proceso de comunicación en VoIP se divide en dos etapas principales:
1.  **Señalización:** Se encarga de establecer, mantener y finalizar la comunicación. Define cómo se transportará y codificará la voz, así como los parámetros de la sesión.
2.  **Transmisión de Datos:** Corresponde al envío de la información de voz en tiempo real.

La arquitectura se compone de tres capas principales:

```mermaid
graph TD
    subgraph Capa de Soporte
        DHCP[DHCP]
        TFTP[TFTP / Provisión]
        NTP[NTP / Sincronización]
        DNS[DNS / Resolución]
    end
    subgraph Capa de Señalización
        SIP[SIP / H.323 / SCCP] <--> SDP[SDP / Negociación]
    end
    subgraph Capa de Transmisión de Datos
        RTP[RTP / Audio y Video] <--> RTCP[RTCP / Control y Calidad]
    end
    Capa de Soporte --> Capa de Señalización
    Capa de Señalización --> Capa de Transmisión de Datos
```

#### A) Capa de Señalización (SIP, H.323, etc.)
Realiza las siguientes funciones críticas:
*   **Localización:** Los *endpoints* deben registrarse con el servidor de registros para que este sepa qué nodos requieren servicio. Este proceso se realiza mediante mensajes específicos (como el método `REGISTER` en SIP). Una vez registrado, la dirección IP y la dirección MAC del teléfono se vinculan a un número de teléfono o alias. El servidor de registro acepta el mensaje `REGISTER` y actualiza la ubicación del usuario.
*   **Establecimiento de llamadas (*call setup*):** En SIP, el método `INVITE` se utiliza para iniciar la llamada. Durante esta fase, se informa a los *endpoints* sobre el progreso de la llamada mediante mensajes de respuesta provisorios (Respuestas 1xx, como `100 Trying` o `180 Ringing`). Una vez que el destinatario contesta, se envía un mensaje de conexión exitosa (`200 OK` en SIP).
*   **Características de la sesión:** Se define cómo se transportará y codificará la voz. Los protocolos de señalización manejan la negociación de los parámetros de la conexión multimedia. El protocolo **SDP (Session Description Protocol)** se utiliza con SIP para describir el contenido multimedia (audio, video). El SDP negocia los códecs a utilizar, la velocidad de muestreo, el tamaño de *frame* y los números de puerto UDP que se usarán para la transmisión de medios (RTP).
*   **Terminación de llamada:** El protocolo de señalización se utiliza para finalizar (colgar) la comunicación de manera controlada (con el método `BYE` en SIP).

#### B) Capa de Transmisión de Datos (RTP y RTCP)
*   **Encapsulamiento de datos de audio (y video):** Los datos de voz se generan mediante un **códec** (codificador-decodificador) que convierte la señal analógica a digital (muestreo) y, a menudo, comprime los datos. La voz muestreada se coloca dentro de un paquete **RTP (Real-time Transport Protocol)**, que a su vez se encapsula en un encabezado **UDP (User Datagram Protocol)**. Se utiliza UDP porque es un protocolo ideal para datos en tiempo real, donde la retransmisión de paquetes perdidos o retrasados resultaría en una degradación insoportable del rendimiento o la usabilidad (latencia y jitter).
*   **Secuenciamiento, Marcas de tiempo, Identificación:** RTP incluye campos en su encabezado cruciales para reconstruir la conversación en el receptor:
    *   *Número de Secuencia:* Permite detectar paquetes perdidos y reordenarlos.
    *   *Marca de Tiempo (Timestamp):* Para manejar el jitter y asegurar la sincronización en la reproducción.
    *   *Tipo de Carga Útil (Payload Type):* Indica el códec utilizado.
    *   *Identificador de Fuente de Sincronización (SSRC):* Permite al receptor recolectar todos los paquetes del mismo flujo.

#### C) Capa de Soporte (DHCP, TFTP, NTP, DNS, etc.)
Gestionan servicios esenciales de red que, aunque no son específicos de VoIP, son vitales para su operación:
*   **Autoconfiguración:**
    *   *DHCP:* Además de asignar la dirección IP y la puerta de enlace, puede proporcionar la dirección del servidor TFTP y, a veces, la del servidor de llamadas (Call Server/Gatekeeper).
    *   *TFTP (Trivial File Transfer Protocol):* Se utiliza para la provisión de configuración, actualizaciones de firmware y distribución de la agenda telefónica global a los teléfonos IP.
*   **Calidad de Servicio (QoS):** La VoIP es sensible a la latencia, el jitter y la pérdida de paquetes. El protocolo **NTP (Network Time Protocol)** es necesario para sincronizar los relojes de la red, lo que es vital para la contabilidad y la gestión de la calidad.
*   **Autenticación, Autorización y Contabilidad (AAA):**
    *   *Autenticación y Autorización:* Las PBX deben realizar el control de admisión y la autenticación. La autenticación puede realizarse durante el registro utilizando credenciales. Para asegurar el tráfico de señalización, se puede usar **Secure SIP (SIPS)**, que utiliza TLS (Transport Layer Security) sobre el puerto 5061. Para proteger el flujo de voz (RTP), se utiliza **Secure RTP (SRTP)**, que proporciona cifrado.
    *   *Contabilidad:* Los protocolos de señalización registran eventos clave de la llamada en registros de detalle de llamadas (CDR - *Call Detail Records*), como el número marcado, la hora de marcado, la hora de conexión, la duración y la hora de desconexión. Estos registros son cruciales para el proceso de facturación.
*   **Resolución de Nombres:** **DNS** se utiliza para la resolución de nombres. En VoIP, la resolución traduce identificadores de recursos, como SIP URIs, en identificadores de capa de red (direcciones IP y números de puerto) para que los *endpoints* puedan comunicarse.

---

### 1.4. Protocolos de Señalización

Existen diferentes protocolos que se pueden usar para la señalización, definiendo cómo funcionará la comunicación y los actores involucrados:
*   **SIP (Session Initiation Protocol - IETF)**
*   **H.323 (ITU-T)**
*   **SCCP o Skinny (Cisco)**
*   **IAX (Inter-Asterisk eXchange - Digium)** *(mencionado como AIX en el original)*

#### El Protocolo SIP (RFC 3261)
Es un protocolo de la capa de aplicación similar a HTTP, basado en texto plano, que opera bajo un esquema de petición/respuesta utilizando por defecto el puerto TCP/UDP 5060.
*   Un mensaje SIP es un requerimiento de un cliente a un servidor o una respuesta de un servidor a un cliente.
*   Involucra interacciones del tipo **User Agent Client (UAC)** $\leftrightarrow$ **User Agent Server (UAS)**.
*   Fue diseñado con la premisa de la simplicidad para iniciar, modificar y finalizar sesiones multimedia.

##### Componentes de la Arquitectura SIP:
*   **User Agent (UA):** Terminal o teléfono que inicia o responde a las transacciones SIP. Es *stateful* (mantiene el estado de la sesión).
*   **Registrar Server (Servidor de Registro):** Servidor de registro de cuentas. Cuando se enciende un teléfono físico o se inicia un *softphone*, este envía un mensaje SIP para decir "¡acá estoy!". El servidor de registro vincula el número del usuario con su dirección IP actual. El servidor mantiene consultas periódicas para saber si siguen conectados. Puede requerir autenticación.
*   **Proxy Server:** Servidor que actúa como intermediario entre cliente y servidor. Se usa para tareas de enrutamiento y para aplicar políticas como la autenticación.
*   **Redirect Server (Servidor de Redireccionamiento):** Servidor que redirige mensajes a un conjunto alternativo de URIs.

##### Direccionamiento en Nomenclatura URI (RFC 3261)
Puede ser un dominio mnemónico o una dirección IP. El puerto puede omitirse si es el puerto bien conocido (5060).
*   `sip:user@domain:port` (ej. `sip:ale@sip.unlu.edu.ar:5060`)
*   `sip:user@host:port` (ej. `sip:mauro@10.100.20.2:5060`)
*   `sip:phone_number@domain` (ej. `sip:1590@servmarce.unlu.edu.ar`)

##### Peticiones SIP (Métodos)
Formato: `Method <SP> Request-URI <SP> SIP-Version <CRLF>`
*   `REGISTER`: Registra la información de contacto y la ubicación del *endpoint* en el servidor de registro.
*   `INVITE`: Inicia el establecimiento de una sesión, invita a un usuario a una llamada.
*   `ACK`: Confirma que el cliente ha recibido una respuesta final a un `INVITE`.
*   `CANCEL`: Cancela una llamada que está pendiente pero que aún no se ha conectado.
*   `BYE`: Termina una conexión o sesión activa. Puede ser enviado por cualquiera de las partes.
*   `OPTIONS`: Consulta a los servidores por sus capacidades, devolviendo los métodos que soporta.

##### Respuestas SIP
Formato: `SIP-Version SP Status-Code SP Reason-Phrase CRLF`
*   `1xx (Provisional):` Indica que la petición fue recibida y se sigue procesando (ej. `100 Trying`, `180 Ringing`).
*   `2xx (Success):` La acción fue recibida, entendida y aceptada exitosamente (ej. `200 OK`).
*   `3xx (Redirection):` Se requiere una acción adicional para completar la petición.
*   `4xx (Client Error):` La petición tiene sintaxis incorrecta o no puede ser cumplida en este servidor (ej. `404 Not Found`).
*   `5xx (Server Error):` El servidor falló al cumplir una petición aparentemente válida.
*   `6xx (Global Failure):` La petición no puede ser cumplida en ningún servidor.

##### Estructura de Mensajes SIP
*   Línea inicial (*start-line*)
*   Uno o más campos de encabezados (*header fields*)
*   Una línea en blanco que indica la finalización de los encabezados
*   Cuerpo del mensaje (*message-body*), opcional (típicamente contiene el SDP)

##### Encabezados Mínimos Requeridos en Requests
*   **To:** Especifica el destinatario de la solicitud en formato URI. (ej. `To: Carol <sip:carol@chicago.com>`).
*   **From:** Indica la identidad del iniciador de la solicitud en formato URI, incluyendo una etiqueta (*tag*) que ayuda a especificar el diálogo. (ej. `From: "Bob" <sips:bob@biloxi.com>;tag=a48s`).
*   **CSeq (Command Sequence):** Valor numérico que ayuda a identificar y ordenar transacciones dentro de un diálogo. Contiene el número de secuencia y el método. (ej. `CSeq: 4711 INVITE`).
*   **Call-ID:** Valor único que agrupa todos los mensajes (solicitudes y respuestas) que pertenecen a un mismo diálogo o sesión. (ej. `Call-ID: f81d4fae-7dec-11d0-a765-00a0c91e6bf6@foo.bar.com`).
*   **Max-Forwards:** Limita el número de saltos (*hops*) que un mensaje puede recorrer. El valor recomendado es 70. (ej. `Max-Forwards: 70`).
*   **Via:** Indica a los nodos involucrados dónde enviar la respuesta SIP. Debe comenzar con `SIP/2.0` y debe incluir el parámetro `branch` que empieza con `z9hG4bk`. (ej. `Via: SIP/2.0/UDP pc33.atlanta.com;branch=z9hG4bK776asdhds`).
*   **Contact:** Debe estar presente en solicitudes `INVITE` y `REGISTER`. Contiene una única URI que representa la ubicación actual del *endpoint*. (ej. `Contact: <sip:bob@192.0.2.4>`).

> [!NOTE]
> En los mensajes `REGISTER`, los encabezados `To` y `From` son iguales, ya que el usuario se está registrando a sí mismo.

---

### 1.5. Ciclos de Vida e Intercambio de Mensajes

#### Proceso de Inicio (Startup) de un Teléfono VoIP
Cuando un teléfono IP se conecta a la red por primera vez, realiza el siguiente intercambio de mensajes:
1.  **DHCP:** El teléfono se enciende y contacta a un servidor DHCP para obtener una dirección IP, una máscara de subred, la puerta de enlace predeterminada y las direcciones del servidor TFTP y del servidor de llamadas (Call Server/Gatekeeper).
2.  **TFTP:** El teléfono contacta al servidor TFTP para actualizar su firmware y descargar un archivo de configuración que contiene parámetros operativos (dirección del Call Server, idioma, disposición de botones, etc.).
3.  **Señalización (Registro):** El teléfono envía un mensaje SIP `REGISTER` al servidor de llamadas (Registrar Server) para informar de su ubicación (vínculo IP y URI) y dar de alta su servicio.

#### Flujos de Datos en una Llamada Completa
Existen dos flujos independientes en una llamada VoIP:
1.  **Flujo de Señalización (SIP):** Es el tráfico de control, corto e intermitente, típicamente transportado sobre UDP o TCP en el puerto 5060 entre el teléfono y el servidor de llamadas (Proxy o Registrar).
2.  **Flujo de Transmisión de Medios (RTP/RTCP):** Contiene los datos de voz o video. Es consistente y prolongado, y se transporta sobre UDP. Una vez establecida la llamada, los paquetes RTP suelen fluir directamente entre los dos teléfonos (ruta de llamada independiente), salvo excepciones donde la PBX deba permanecer en medio del tráfico (por ejemplo, para NAT o grabación).

#### SDP (Session Description Protocol) y Negociación de Medios
El SDP viaja encapsulado dentro del cuerpo de los mensajes SIP. Su objetivo es transmitir información de los flujos multimedia para permitir a los destinatarios participar en la sesión.
*   **Uso en la llamada:** El SDP se envía por primera vez dentro del mensaje `INVITE` (anunciando las capacidades del emisor). El receptor responde confirmando la negociación dentro de la carga útil del mensaje `200 OK`.
*   **Contenido de una descripción de sesión SDP:**
    *   *Descripción de Sesión:* Nombre de la sesión y propósito.
    *   *Descripción Temporal:* Tiempos de inicio y detención en los que la sesión está activa.
    *   *Descripción de Medios:* Tipo de contenido (video, audio), protocolo de transporte (ej. RTP/AVP) y formato del contenido (códecs).
    *   *Direcciones y puertos:* IP de destino y puertos UDP para el flujo RTP.
*   **Campos comunes en capturas SDP:**
    *   `v=` Versión del protocolo.
    *   `o=` Identificador y origen de la sesión (nombra de manera única la solicitud).
    *   `s=` Nombre de la sesión.
    *   `c=` Información de la conexión (dirección IP para el canal de medios).
    *   `t=` Tiempo activo de la sesión de medios.
    *   `m=` Medios y códecs disponibles (tipo de medio, puerto, protocolo de transporte y lista de códecs).
    *   `a=` Atributos (mapea números con códecs, tasa de muestreo, perfiles, etc.).

> [!TIP]
> **QoS por Inspección Profunda (DPI):** La inspección profunda de la carga útil SDP en el firewall o router de borde permite conocer dinámicamente qué puertos UDP se utilizarán para la transmisión de voz (RTP). El router puede usar esta información para priorizar dichos puertos dinámicamente y aplicar políticas de QoS.

##### Flujo de Establecimiento de Llamada SIP (Tríada UAC - UAS - UAC)
1.  `INVITE` (UAC $\rightarrow$ UAS): El UAC envía la invitación inicial al Proxy/UAS.
2.  `100 Trying` (UAS $\rightarrow$ UAC): El servidor detiene el temporizador de retransmisión indicando que está procesando la solicitud.
3.  `180 Ringing` (UAS $\rightarrow$ UAC): Indica que el teléfono de destino está timbrando.
4.  `200 OK` (UAS $\rightarrow$ UAC): El usuario llamado descuelga y acepta la llamada.
5.  `ACK` (UAC $\rightarrow$ UAS): El emisor confirma la recepción del `200 OK`.
6.  *Inicio del Flujo RTP de medios bidireccional.*

---

### 1.6. Digitalización de la Voz

#### ¿Qué es un CODEC?
Abreviatura de codificador-decodificador (*coder-decoder*). Es un algoritmo cuyo propósito es convertir la señal de voz analógica en una serie de muestras digitales en la fuente, y luego volver a convertirla a analógica en el receptor.

#### Proceso Completo desde el Micrófono hasta el Paquete RTP
1.  **Captura Analógica:** El micrófono captura las vibraciones físicas del sonido en el aire y las convierte en una onda analógica con infinitos valores posibles de tiempo y amplitud (valores continuos). Esta señal tiene tres características: frecuencia (Hz), amplitud y fase.
2.  **Conversión de Analógico a Digital (Codec):** Para ser transmitida en redes de paquetes IP, la señal analógica se procesa mediante **PCM (Modulación por Codificación de Pulsos)** a través de:
    *   *Muestreo:* Evaluación periódica de la amplitud de la señal analógica en momentos específicos.
    *   *Cuantificación:* Asignación del valor de cada muestra al valor binario más cercano dentro de una escala limitada y graduada.
3.  **Paquetización y Transporte (RTP):** El flujo digital codificado se divide en partes iguales (framing), se encapsula en paquetes RTP, y estos a su vez en cabeceras UDP/IP para ser transmitidos.

#### Parámetros de Calidad y Digitalización
*   **Resolución:** Cantidad de bits utilizados por muestra para representar la amplitud. Una cuantificación estándar usa 8 bits por muestra, proporcionando 256 valores posibles de resolución.
*   **Frecuencia de Muestreo:** Cantidad de mediciones tomadas por segundo (medida en Hz).
    *   *Teorema de Nyquist:* Para reproducir con precisión una señal, la tasa de muestreo debe ser de al menos dos veces el ancho de banda original de la señal.
    *   Dado que el canal de voz telefónica tradicional asignado es de hasta 4000 Hz, la frecuencia de muestreo estándar es de **8000 Hz** ($2 \times 4000\text{ Hz}$). Si la frecuencia es inferior, se pierden detalles críticos de la señal original.

#### El Dilema de la Paquetización (*Framing*)
El tamaño de los paquetes requiere equilibrar el retraso (*delay*) con la sobrecarga (*overhead*) y la resiliencia a errores:
*   **Pocos paquetes grandes (Muchos bits por paquete):** Reducen la sobrecarga de la red (menos cabeceras IP/UDP/RTP por segundo), pero aumentan el retraso (*lag*) porque el paquete debe esperar a estar completamente lleno antes de transmitirse. Si se pierde un paquete, la pérdida de información es muy grande (vacíos de audio prolongados).
*   **Muchos paquetes pequeños (Pocos bits por paquete):** Mejoran el *delay* y la pérdida de paquetes individuales apenas se nota, pero aumentan drásticamente la sobrecarga de red debido al envío continuo de cabeceras.

##### Cálculo de Ancho de Banda para G.711
El códec G.711 utiliza 8000 Hz de muestreo y 8 bits de resolución, con un envío estándar de 50 paquetes por segundo (packet rate = 50 Hz, o tramas de 20 ms de audio).
*   *Payload de voz:* $8000\text{ muestras/s} \times 8\text{ bits/muestra} \times 0.02\text{ s} = 1280\text{ bits} = 160\text{ bytes}$.
*   *Cabeceras del Paquete:*
    *   Ethernet: 22 bytes (incluyendo preámbulo y espacio entre tramas).
    *   IP: 20 bytes.
    *   UDP + RTP: 20 bytes ($8 + 12$ bytes).
    *   Sobrecarga total por paquete: 62 bytes.
*   *Ancho de banda total:*
    $$\text{Ancho de banda} = (160\text{ bytes payload} + 62\text{ bytes cabeceras}) \times 8\text{ bits/byte} \times 50\text{ paq/s} = 88.800\text{ bps (88.8 kbps)}$$
    *(Nota: Aunque se necesitan más de 10,000 llamadas para saturar un enlace de gran capacidad, la sobrecarga acumulada es crítica al aplicar políticas de QoS).*

##### Catálogo de Códecs Comunes
*   **G.711 (PCM):** 64 kbps (payload), alta calidad, sin compresión.
*   **G.722 (SB-ADPCM):** Códec de banda ancha (alta fidelidad).
*   **G.723.1:** Diseñado para videoconferencia sobre líneas lentas. Posee dos tasas:
    *   5.3 kbps usando ACELP (*Algebraic Code Excited Linear Prediction*).
    *   6.3 kbps usando ML-MLQ (*Multipulse Maximum Likelihood Quantization*).
*   **G.726 (ADPCM):** Compresión adaptativa diferencial.
*   **G.729 (CS-ACELP):** Muy utilizado por su alta compresión (8 kbps) manteniendo buena calidad.
*   **iLBC (Internet Low Bitrate Codec):** Tolerante a la pérdida de paquetes en conexiones de internet públicas.
*   **Opus (RFC 6716):** Códec moderno y adaptable que combina tecnologías de Skype (SILK) y CELT, permitiendo cambios dinámicos de ancho de banda.
*   **Transcodificación:** Ocurre cuando dos endpoints no soportan el mismo códec. El servidor de llamadas debe intervenir y decodificar el flujo en un códec para volver a codificarlo en el otro, lo cual consume recursos de procesamiento críticos.

---

### 1.7. Protocolos RTP y RTCP

*   **RTP (Real-time Transport Protocol):** Proporciona servicios de entrega de extremo a extremo para datos con características de tiempo real (audio y video) sobre UDP.
    *   *Campos del Encabezado RTP:*
        *   `V (Version - 2 bits):` Versión del protocolo (actualmente 2).
        *   `P (Padding - 1 bit):` Si hay bytes de relleno al final del paquete.
        *   `X (Extension - 1 bit):` Si hay encabezados extendidos.
        *   `CC (CSRC Count - 4 bits):` Conteo de fuentes contribuyentes (en conferencias).
        *   `M (Marker - 1 bit):` Indica eventos importantes (ej. inicio de habla tras un silencio).
        *   `PT (Payload Type - 7 bits):` Identifica el códec del contenido.
        *   `Sequence Number (16 bits):` Incrementa en uno por paquete enviado, útil para ordenar el flujo.
        *   `Timestamp (32 bits):` Instante en que se tomó la primera muestra del paquete. Mide el jitter.
        *   `SSRC (32 bits):` Identificador único de la fuente de sincronización para agrupar paquetes del mismo origen.
        *   `CSRC (32 bits c/u):` Identificadores de fuentes contribuyentes en caso de mezcla.
*   **RTCP (Real-time Transport Control Protocol):** Protocolo de control complementario a RTP para monitorear la calidad del servicio de forma periódica.
    *   *Funciones:* Proporciona *feedback* sobre pérdidas, jitter y retardos; asocia nombres canónicos (CNAME) para identificar permanentemente a los participantes. La tasa de envío de paquetes RTCP se ajusta según el número total de participantes.
    *   *Tipos de paquetes RTCP:*
        *   `SR (Sender Report):` Estadísticas de transmisión de emisores activos (SSRC, marcas de tiempo NTP/RTP, paquetes y bytes enviados).
        *   `RR (Receiver Report):` Estadísticas de recepción de participantes que no transmiten de forma activa (fracción de paquetes perdidos, acumulado de pérdidas, máximo número de secuencia, jitter, retardo desde el último SR recibido).
        *   `SDES (Source Description):` Contiene el nombre canónico (CNAME) y datos como el número telefónico o dirección de correo del usuario.
        *   `BYE:` Indica que una fuente abandona la sesión de medios.
        *   `APP:` Para funciones experimentales de aplicaciones específicas.

*Nota:* La calidad percibida se mide mediante el **MOS (Mean Opinion Score)**, un promedio de calificaciones subjetivas hechas por personas en una escala del 1 (pésima) al 5 (excelente).

---

### 1.8. Servidores, Clientes e H.323

*   **Software Servidor (SoftPBX):** Asterisk, Elastix, Issabel.
*   **Software Cliente (Softphones):** Ekiga, Linphone, Zoiper.
*   **Hardware:** Teléfonos IP físicos, placas PCI de interfaz para telefonía tradicional (FXS/FXO, T1/E1), equipos de videoconferencia dedicados.
*   **PoE (Power over Ethernet):** Permite alimentar eléctricamente teléfonos IP y Access Points a través del propio cable de datos UTP.

#### Protocolo H.323 (ITU-T)
Propuesto originalmente para videoconferencias en redes LAN, evolucionó para ser una solución completa de reemplazo de PBX. Es un conjunto de protocolos complejo basado en la arquitectura de la telefonía tradicional.
*   **Subprotocolos:**
    *   `H.225`: Maneja el registro, la admisión, el control y la señalización de llamadas.
    *   `H.245`: Protocolo de control para la negociación de capacidades multimedia.
*   **Componentes:** Terminales (endpoints), gateways y **gatekeepers** (proporcionan control centralizado, traducción de direcciones y control de admisión).

#### Factores de Red que Afectan a la VoIP
*   **Latencia (Retardo):** Tiempo que tarda en viajar el sonido del emisor al receptor. Un retardo superior a **150 ms** de extremo a extremo dificulta la conversación, provocando interrupciones mutuas.
*   **Jitter:** Variación en los tiempos de llegada de los paquetes. Caused by congestion or processing overload. Produce audio entrecortado.
*   **Pérdida de Paquetes:** Debe mantenerse por debajo del **1%**. No se permite la retransmisión de paquetes perdidos porque añadiría más retardo destructivo al audio.
*   **NAT (Network Address Translation):** Los firewalls interfieren con los protocolos de señalización (SIP/H.323) debido a que los puertos de transmisión de medios (RTP) se abren dinámicamente, requiriendo soluciones de paso específicas (como STUN o proxies de medios).

---

## Módulo 2: Seguridad en Redes de Datos

La seguridad busca garantizar que la información de una organización mantenga su integridad, disponibilidad y confidencialidad. Dado que la información es uno de los activos más importantes, su protección es fundamental para la continuidad operativa.

### 2.1. Conceptos Fundamentales
*   **Activos:** Elementos a proteger (información, software, hardware y la red misma).
*   **Amenazas:** Cualquier factor (deliberado o accidental, como fallas técnicas o desastres naturales) que pueda comprometer la seguridad.
*   **Vulnerabilidades:** Debilidades en los activos (como falta de actualizaciones o malas configuraciones). Las vulnerabilidades se registran globalmente con identificadores **CVE (Common Vulnerabilities and Exposures)**.
*   **Riesgo:** Probabilidad de que una amenaza explote una vulnerabilidad sobre un activo.
*   **Daño:** El resultado directo de la acción de una amenaza.
*   **Impacto:** Gravedad de las consecuencias del daño para la organización.
*   **Controles y Salvaguardas:** Mecanismos que disminuyen el riesgo previniendo amenazas, detectando ataques o recuperando información.
*   **CSIRT / CERT:** Equipos especializados de respuesta a incidentes de seguridad que analizan incidentes y coordinan defensas las 24 horas.

---

### 2.2. Pilares de la Seguridad de la Información (Tríada CIA)

*   **Integridad:** Garantizar que la información no sea alterada por entidades no autorizadas y que permanezca exacta y completa. Se verifica mediante mecanismos de detección de alteraciones (ej. *checksums*, firmas digitales).
*   **Confidencialidad:** Garantizar que solo las personas autorizadas tengan acceso a la información. Requiere cifrado y controles de acceso estrictos.
*   **Disponibilidad:** Garantizar el acceso a los recursos y servicios cuando el personal autorizado lo requiera, mitigando fallas de hardware y ataques como **Denegación de Servicio (DoS)**.

---

### 2.3. Arquitectura de Seguridad OSI: Recomendación ITU-T X.800

Divide la seguridad en tres áreas conceptuales:

#### A) Ataques a la Seguridad
Cualquier acción que comprometa la protección de la información:
*   **Ataques Pasivos:** Acceden a la información sin modificarla (ej. escucha no autorizada o análisis de tráfico - *sniffing*). Son difíciles de detectar, por lo que el enfoque es la prevención mediante cifrado.
*   **Ataques Activos:** Modifican o generan información haciéndose pasar por usuarios legítimos, alterando mensajes o interrumpiendo servicios (ej. ataques de denegación de servicio - DoS).

#### B) Mecanismos de Seguridad
Procesos o dispositivos diseñados para detectar, prevenir o recuperar información ante ataques (ej. cifrado, firmas digitales, control de acceso).

#### C) Servicios de Seguridad
Procesos de comunicación que mejoran la seguridad del procesamiento de datos en la organización (Integridad, Confidencialidad, Disponibilidad).
*   *Otros servicios importantes:*
    *   **Autenticación:** Garantiza la identidad del emisor o usuario (verificar que es quien dice ser).
    *   **No repudio:** Evita que el emisor o receptor nieguen haber enviado o recibido un mensaje (ej. mediante firma digital).
    *   **Control de acceso:** Restringe las acciones que puede tomar un usuario sobre un recurso (ej. lectura, escritura, ejecución).

---

### 2.4. Identificación, Autenticación y Autorización (Proceso triple)
1.  **Identificación:** El usuario indica su cuenta o identidad al sistema.
2.  **Autenticación:** El sistema verifica la identidad mediante tres posibles factores:
    *   *Algo que se sabe* (contraseñas, PIN).
    *   *Algo que se tiene* (tokens, llaves físicas, certificados).
    *   *Algo que se es* (huella dactilar, biometría).
3.  **Autorización:** El sistema comprueba los permisos asignados a esa identidad para permitir o denegar la acción sobre el recurso.

---

### 2.5. Principios de Diseño de Seguridad
*   **Fallar en forma segura (Fail-safe defaults):** Si ocurre un error o caída de un servicio, los accesos deben denegarse por defecto.
*   **Diversidad de defensa:** Implementar múltiples tipos de mecanismos de seguridad de diferentes proveedores para evitar que una sola vulnerabilidad comprometa todo.
*   **Simplicidad:** Un diseño más simple facilita la auditoría, la detección de fallas y la aplicación de controles eficaces.
*   **Transparencia (Principio de Kerckhoffs):** La seguridad debe basarse en la solidez del diseño y las claves, no en ocultar el funcionamiento del algoritmo (*no seguridad por oscuridad*).

---

## Módulo 3: Firewalls, IDS/IPS y Sistemas de Gestión de Seguridad

### 3.1. Cortafuegos (Firewalls)

Es un dispositivo de hardware o software ubicado en el límite entre dos redes que decide, basándose en reglas predefinidas, qué paquetes pueden transitar y cuáles no. 

#### Objetivos de Diseño de un Firewall:
1.  Todo el tráfico entrante y saliente debe pasar obligatoriamente a través del firewall.
2.  Solo se permite el paso del tráfico autorizado por la política local.
3.  El dispositivo de firewall en sí debe ser altamente seguro e inmune a intrusiones.

#### Servicios Comunes:
*   Control de acceso (filtrado de paquetes).
*   Registro de actividades (*logging* para auditoría forense).
*   Terminación de NAT (Traducción de Direcciones de Red) y túneles VPN.

#### Políticas de Seguridad del Firewall ("Las 4 P")
*   **Paranoica:** Todo está prohibido por defecto, incluso servicios estándar de red.
*   **Prudente:** Todo está prohibido, salvo lo explícitamente permitido.
*   **Permisiva:** Todo está permitido, salvo lo explícitamente prohibido.
*   **Promiscua:** Todo está permitido por defecto, incluso tráfico de riesgo.

#### Tipos de Firewall (Generaciones)
*   **Stateless Packet Filtering (Filtro sin estado - 1ra Generación):** Analiza paquetes de forma aislada basándose únicamente en campos del encabezado (IP origen/destino, puertos, protocolo). Requiere reglas explícitas duplicadas para ida y vuelta.
*   **Stateful Inspection (Filtro con estado - 2da Generación):** Mantiene una tabla de conexiones activas. Permite las respuestas automáticas de conexiones salientes legítimas sin necesidad de reglas redundantes de entrada.
*   **Application-Level Gateway o Proxy (3ra Generación):** Actúa como intermediario de una aplicación específica. Entiende la semántica del protocolo de aplicación y puede filtrar comandos específicos (ej. proxy HTTP).
*   **Circuit-Level Gateway:** Establece conexiones TCP o UDP intermedias actuando como puente de transporte para múltiples aplicaciones (ej. proxy SOCKS).

#### Emplazamiento de Firewalls
*   *Borde de la red:* Firewall perimetral en la entrada de la organización.
*   *Host-Based Firewall:* Software instalado en un servidor específico para proteger sus propios servicios.
*   *Personal Firewall:* Firewall integrado en el sistema operativo de estaciones de trabajo de usuarios.
*   *WAF (Web Application Firewall):* Filtra tráfico HTTP/HTTPS buscando vulnerabilidades web específicas (ej. inyecciones SQL).
*   *Process Firewall:* Monitorea llamadas a sistema hechas por procesos de software locales.

---

### 3.2. Topologías de Red Clásicas con DMZ (Zona Desmilitarizada)

```
[Internet - WAN] <───► [Firewall Externo] <───► [DMZ (Servidores Públicos: Web, Mail)]
                              │
                              ▼
                      [Firewall Interno]
                              │
                              ▼
                        [Red Local - LAN]
```

*   **LAN:** Red interna y privada de la organización (computadoras de usuarios).
*   **DMZ:** Zona aislada donde se colocan servidores públicos (Web, Mail, DNS).
*   **WAN:** Internet pública.
*   *Esquema de doble firewall:* El firewall externo filtra el tráfico de Internet hacia la DMZ. Si un servidor en la DMZ se ve comprometido, el firewall interno impide el acceso del atacante hacia la LAN interna.
*   *Esquema de firewall único:* Un único dispositivo con múltiples interfaces gestiona las tres zonas. Es económico pero presenta un único punto de falla.

---

### 3.3. IDS, IPS, SIEM, UTM y MSSP

*   **IDS (Intrusion Detection System):** Monitorea el tráfico de red de forma pasiva buscando actividades sospechosas. Genera alertas y registros, pero no detiene el tráfico directamente. Puede actuar en conjunto con el firewall para aplicar reglas de bloqueo dinámicas.
    *   *Modo Patrones / Firmas:* Compara el tráfico con una base de datos de ataques conocidos (inútil ante ataques de día cero).
    *   *Modo Comportamiento / Anomalías:* Compara el tráfico con una línea base de comportamiento normal aprendida previamente.
*   **IPS (Intrusion Prevention System):** Funciona de forma activa "en el medio" del tráfico (*inline*). Si detecta un ataque (por firmas o estadísticas), puede descartar los paquetes maliciosos, restablecer la conexión TCP o bloquear la IP origen.
    *   *Tipos:* Basados en host (HIPS, ej. Fail2ban) o red (NIPS, ej. Suricata).
*   **SIEM (Security Information and Event Management):** Centraliza y correlaciona los registros (*logs*) de múltiples equipos de la red para detectar incidentes complejos. Combina:
    *   `SEM (Security Event Management)`: Análisis en tiempo real de eventos de seguridad.
    *   `SIM (Security Information Management)`: Almacenamiento y reporte a largo plazo para auditorías y cumplimiento normativo.
*   **UTM (Unified Threat Management):** Dispositivo físico o software de seguridad integral "todo en uno" que combina firewall, IDS/IPS, antivirus, VPN, antispam y filtrado de contenido en una única consola.
*   **MSSP (Managed Security Service Provider):** Empresa externa proveedora de servicios gestionados de seguridad que monitorea la red de un cliente remotamente mediante VPNs (con enlaces secundarios de respaldo celular o POTS).

---

## Módulo 4: Netfilter y Configuración de Firewalls en Linux

**Netfilter** es un *framework* extensible dentro del kernel de Linux que permite interceptar y manipular paquetes de red. Su función principal es dar soporte para el filtrado, NAT y marcas de QoS.

### 4.1. Conceptos Fundamentales
*   **Tablas (Tables):** Contenedores de reglas organizados por funcionalidad:
    *   `filter`: Tabla por defecto para el filtrado de paquetes.
    *   `nat`: Para traducción de direcciones y puertos.
    *   `mangle`: Para modificar campos del encabezado (bits DSCP/TOS) y marcar paquetes.
*   **Cadenas (Chains):** Listas de reglas asociadas a puntos específicos del procesamiento de red (*hooks*). Tienen políticas por defecto (`accept` o `drop`).
*   **Reglas (Rules):** Condiciones (análogas a ACLs) que definen criterios de coincidencia de paquetes y acciones.
*   **Veredictos (Verdicts - Acciones):**
    *   `ACCEPT`: Permite transitar el paquete.
    *   `DROP`: Descarta silenciosamente el paquete.
    *   `REJECT`: Descarta el paquete enviando un mensaje ICMP de error de vuelta.
    *   `LOG`: Registra el paquete en los *logs* del sistema y continúa evaluando la cadena.
    *   `RETURN`: Detiene la evaluación de la cadena actual y regresa a la cadena llamadora.

---

### 4.2. Ciclo de Vida de un Paquete en el Kernel (Hooks de Netfilter)

```
[Entrada de Red] ──► [INGRESS] ──► [PREROUTING] ──► [Decisión de Enrutamiento]
                                         │                    │
                                         ▼ (Local)            ▼ (Hacia otro host)
                                      [INPUT]             [FORWARD]
                                         │                    │
                                         ▼                    ▼
                                    [Aplicación]              │
                                         │                    │
                                         ▼ (Origen Local)     │
                                      [OUTPUT] ───────────────┼──► [POSTROUTING] ──► [Salida de Red]
```

1.  **Recepción e INGRESS:** Se valida el *checksum* del paquete y la MAC destino. Entra en el hook `INGRESS` (filtrado temprano antes del reensamblado; no apto para ver puertos TCP/UDP de paquetes fragmentados).
2.  **Hook PREROUTING:** Se ejecuta en todos los paquetes recibidos antes de tomar la decisión de enrutamiento. Es el punto ideal para realizar **DNAT (Destination NAT)** o redireccionamiento a puertos locales.
3.  **Decisión de Enrutamiento:**
    *   *Destino Local:* Si el paquete va dirigido al propio host, pasa al hook **INPUT**. Si se acepta, se coloca en el búfer de la aplicación receptora.
    *   *Destino Remoto (Reenvío):* Si el host actúa como router y el destino es otro equipo, pasa al hook **FORWARD**. Es el núcleo de los firewalls de red.
4.  **Hook OUTPUT:** Se ejecuta en paquetes generados localmente por aplicaciones del sistema antes de salir.
5.  **Hook POSTROUTING:** Se ejecuta en todos los paquetes salientes después de la decisión de enrutamiento, justo antes de abandonar el equipo. Es el punto ideal para **SNAT (Source NAT / Masquerading)**.

---

### 4.3. Sintaxis y Configuración Práctica (nftables)

`nftables` es el reemplazo moderno de `iptables`.

#### Creación de Tabla y Cadena en nft
```bash
nft add table inet FW
nft add chain inet FW INPUT { type filter hook input priority 0; policy drop; }
```

#### Reglas de Coincidencia Comunes
*   `ip protocol tcp|udp|icmp`
*   `ip saddr` (IP origen), `ip daddr` (IP destino)
*   `iif` (interfaz entrada), `oif` (interfaz salida)
*   `tcp dport` (puerto destino TCP), `tcp sport` (puerto origen TCP)
*   `ct state new|established|related` (módulo de seguimiento de conexiones para firewalls *stateful*).
*   Se puede negar una condición anteponiendo el signo `!`.

#### Ejemplo de Configuración Completa (`/etc/nftables.conf`)
```
table inet firewall {
    chain input {
        type filter hook input priority filter; policy drop;
        ip protocol icmp ip saddr 192.0.2.0/24 accept
        iif "eth0" ip protocol tcp ip saddr 192.0.2.200 tcp dport 22 accept
    }
    chain output {
        type filter hook output priority filter; policy drop;
        ip protocol icmp ip daddr 192.0.2.0/24 accept
        oif "eth0" ip protocol tcp ip daddr 192.0.2.200 tcp sport 22 accept
    }
    chain forward {
        type filter hook forward priority filter; policy drop;
        ip protocol icmp ip saddr 192.0.2.0/24 accept
        ip protocol icmp ip daddr 192.0.2.0/24 accept
        ip saddr 192.0.2.0/24 tcp dport 110 log
        ip saddr 192.0.2.0/24 tcp dport 110 drop
        iif "eth0" tcp dport 80 ct state established,new accept
        oif "eth0" tcp sport 80 ct state established accept
    }
}
```

> [!WARNING]
> **Seguridad en Servidores Comprometidos:** Si un atacante compromete un servidor web mediante una vulnerabilidad de aplicación y obtiene una consola, el firewall perimetral ya no detendrá el tráfico malicioso saliente (se iniciará desde el puerto local y parecerá tráfico legítimo). Para mitigar este riesgo de movimiento lateral interno, se debe segmentar la red utilizando DMZs, restringir los privilegios del servidor, realizar endurecimiento del sistema (*hardening*) e implementar firewalls de host basados en los hooks **INPUT** y **OUTPUT** locales.

---

## Módulo 5: Criptografía y Seguridad de la Información

La criptografía es la ciencia que estudia las técnicas para proteger la información mediante su transformación, asegurando confidencialidad, integridad y autenticidad.

### 5.1. Criptosistemas y Clasificaciones
Un criptosistema se define como el conjunto $\{m, c, K, E, D\}$:
*   $m$ (*plaintext*): Mensaje original en claro.
*   $c$ (*ciphertext*): Mensaje cifrado.
*   $K$ (*keys*): Claves del sistema.
*   $E$ (*encryption*): Algoritmo de cifrado (genera una transformación $E_k(m)$).
*   $D$ (*decryption*): Algoritmo de descifrado.
*   *Relación fundamental:* $D_k(E_k(m)) = m$.

#### Clasificación por Operación:
*   **Sustitución:** Se reemplaza un elemento por otro (ej. Cifrado César monoalfabético, Cifrado Vigenère polialfabético).
    *   *Fórmula César:* $C = (m + 3) \bmod L$.
    *   *Fórmula Vigenère:* $E_k(m_i) = m_i + k_{(i \bmod d)} \bmod n$.
*   **Transposición:** Se reordenan los elementos del mensaje (ej. *Rail Fence*, Máquinas de rotores como Enigma).

#### Clasificación por Claves:
*   **Sistemas Simétricos (Clave Secreta):** Misma clave para cifrar y descifrar. Son rápidos y eficientes para grandes datos, pero difíciles de escalar en su distribución.
*   **Sistemas Asimétricos (Clave Pública):** Clave pública (compartida) para cifrar y clave privada (secreta) para descifrar. Basados en problemas matemáticos complejos (factorización de primos). Son lentos pero fáciles de distribuir y proveen autenticidad y no repudio.

#### Clasificación por Procesamiento:
*   **Cifrado por Bloques:** Procesa datos en bloques de tamaño fijo (ej. AES).
*   **Cifrado Continuo (Flujo):** Procesa datos de forma secuencial byte a byte.

#### Enfoque Híbrido Práctico
En la práctica se combinan ambos sistemas:
1.  El emisor genera una clave simétrica aleatoria para cifrar el mensaje.
2.  Cifra la clave simétrica con la clave pública del receptor (criptografía asimétrica).
3.  Envía el mensaje cifrado y la clave simétrica cifrada.
4.  El receptor descifra la clave simétrica con su clave privada y luego descifra el mensaje.

---

### 5.2. Funciones Hash y Autenticación

Una función hash $H(m)$ genera un resumen de tamaño fijo a partir de un mensaje de cualquier tamaño.
*   **Propiedades:** Determinismo, unidireccionalidad (irreversible), sensibilidad (efecto avalancha) y resistencia a colisiones ($m \neq m' \Rightarrow H(m) \neq H(m')$).
*   **Algoritmos:** MD5 (inseguro), SHA-1 (obsoleto), SHA-256 (seguro actualmente).

#### MAC (Message Authentication Code) y HMAC
*   **MAC:** Provee integridad y autenticidad del origen utilizando una función y una clave secreta compartida ($k$):
    $$MAC = C_k(m)$$
*   **HMAC (Hash-based MAC):** Implementación de MAC que utiliza funciones hash criptográficas combinando la clave y el mensaje mediante operaciones lógicas:
    $$HMAC(k, m) = H\big( (k \oplus opad) \mathbin{\Vert} H( (k \oplus ipad) \mathbin{\Vert} m ) \big)$$

#### Firma Digital
Mecanismo de autenticación basado en criptografía asimétrica que garantiza el origen, la integridad y el no repudio:
1.  **Firmado:** El emisor calcula el hash del mensaje $H(m)$ y lo cifra con su **clave privada** ($k_{PrivA}$). El resultado es la firma digital adjunta al mensaje.
2.  **Verificación:** El receptor descifra la firma con la **clave pública** del emisor ($k_{PubA}$), obteniendo la huella hash original. Calcula el hash del mensaje recibido y compara ambos valores. Si coinciden, el mensaje no fue alterado y proviene del emisor real.

> [!NOTE]
> **Firma Digital vs. Firma Electrónica:** La firma electrónica es cualquier manifestación de voluntad realizada por medios electrónicos (no necesariamente criptográfica). La firma digital es un tipo específico de firma electrónica basada en criptografía asimétrica y certificados digitales que garantiza jurídicamente el no repudio.

---

### 5.3. Infraestructura de Clave Pública (PKI) y Certificados X.509

*   **RA (Registration Authority - Autoridad de Registro):** Verifica la identidad del solicitante de un certificado.
*   **CA (Certification Authority - Autoridad de Certificación):** Emite y firma digitalmente un certificado para vincular una identidad con su clave pública, previniendo suplantaciones.
*   **Certificado X.509:** Estándar de formato para certificados digitales que incluye el nombre del titular, su clave pública, la CA emisora, las fechas de validez, el número de serie y la firma de la CA.
    *   *Autofirmados:* Proveen confidencialidad e integridad, pero no autenticidad de origen para terceros.
    *   *Emitidos por CA:* Proveen confidencialidad, integridad y autenticidad verificable.
*   **Esteganografía:** Disciplina dedicada a ocultar la existencia misma del mensaje dentro de otro archivo portador (ej. dentro de pixeles de una imagen), a diferencia del cifrado que solo oculta el contenido.

---

### 5.4. Protección en Capas del Modelo OSI

*   **Capa Enlace (L2):** Cifrado en redes Wi-Fi (WPA2/WPA3).
*   **Capa Red (L3):** **IPSec**, cifra paquetes IP completos a nivel de red corporativa.
*   **Capa Transporte (L4):** **TLS (Transport Layer Security)**, cifra conexiones de flujo extremo a extremo (ej. HTTPS).
*   **Capa Aplicación (L7):** Herramientas de cifrado locales como **OpenPGP** (S/MIME, GnuPG) para archivos y correos electrónicos (soporta firmas, cifrado simétrico/asimétrico, compresión y codificación Radix-64 / ASCII Armor).

---

### 5.5. Protocolo TLS (Transport Layer Security)

Se implementa sobre TCP para añadir seguridad a nivel de transporte para HTTP (HTTPS), correo (SMTPS, IMAPS) y VoIP (SIPS - puerto 5061).

#### Arquitectura de TLS:

```
+-------------------------------------------------------------------------+
|  Handshake Protocol  |  Alert Protocol  |  Change Cipher Spec |  App (HTTP) |
+-------------------------------------------------------------------------+
|                           TLS Record Protocol                           |
+-------------------------------------------------------------------------+
|                                  TCP                                    |
+-------------------------------------------------------------------------+
|                                  IP                                     |
+-------------------------------------------------------------------------+
```

*   **TLS Record Protocol:** Fragmenta los datos de la aplicación (máx. 16.384 bytes), calcula el MAC (HMAC) para la integridad y cifra el fragmento utilizando un algoritmo simétrico. Añade un encabezado con: *Content-Type*, *Version* y *Length*.
*   **Alert Protocol:** Transmite alertas de 2 bytes: criticidad (1=warning, 2=fatal) y código de error (ej. certificado corrupto). Si la alerta es fatal, la conexión se cierra inmediatamente.
*   **Change Cipher Spec Protocol (v1.2 y anteriores):** Mensaje de 1 byte con valor 1 que avisa que los siguientes registros se enviarán cifrados con las nuevas claves negociadas.
*   **Handshake Protocol (Negociación TLS 1.2):**
    *   *Fase 1:* `ClientHello` $\rightarrow$ `ServerHello`. Se negocian la versión de TLS, cipher suites, métodos de compresión e identificadores de sesión.
    *   *Fase 2:* Servidor envía `Certificate`, `ServerKeyExchange` (intercambio de claves), `CertificateRequest` (opcional al cliente) y `ServerHelloDone`.
    *   *Fase 3:* Cliente envía `Certificate` (si se pidió), `ClientKeyExchange` y `CertificateVerify`.
    *   *Fase 4:* `ChangeCipherSpec` y `Finished` en ambos extremos para activar el canal cifrado.
*   **Mejoras en TLS 1.3:** Reduce el número de mensajes del saludo. Utiliza **claves precompartidas (PSK)** basadas en sesiones previas y permite el envío de **datos tempranos (0-RTT / Early Data)** antes de completar el saludo.

---

## Módulo 6: VPNs (Redes Privadas Virtuales)

Una VPN permite interconectar redes o usuarios remotos a través de una red pública (Internet) de forma segura, creando un "túnel" cifrado que garantiza confidencialidad, autenticidad e integridad.

### 6.1. Casos de Uso y Servicios
*   **Acceso Remoto (Road Warrior):** Usuarios externos se conectan a la LAN corporativa (ej. teletrabajadores).
*   **Intranet (Net-to-Net):** Conexión segura entre sucursales de la misma organización.
*   **Extranet:** Conexión segura para permitir el acceso controlado a socios o clientes externos.
*   **Servicios Necesarios:** Cifrado de datos, enrutamiento/encapsulamiento (cabeceras adicionales para tunelizar), soporte multiprotocolo, autenticación de usuarios y paquetes, administración de claves y asignación de IPs privadas.

---

### 6.2. Comparativa de Implementaciones VPN

| Protocolo | Capa OSI / Ubicación | Modos de Red / Características | Ventajas y Desventajas |
| :--- | :--- | :--- | :--- |
| **IPSec** | Capa 3 (Kernel) | *Modo Transporte:* Cifra solo payload.<br>*Modo Túnel:* Cifra paquete IP completo. | Alta seguridad nativa, pero configuración compleja. |
| **OpenVPN** | Capa 4/7 (Espacio de Usuario) | *Routing Mode (L3 - tun):* Rutas IP.<br>*Bridging Mode (L2 - tap):* Tránsito Ethernet completo. | Muy flexible y multiplataforma. Mayor *overhead* por cambio de contexto kernel/usuario. Se aconseja usar UDP. |
| **WireGuard** | Capa 3 (Kernel) | Crea interfaz `wgX` sobre UDP. Intercambio de claves públicas fuera de banda. | Diseño moderno, código simplificado, alto rendimiento, reconexión rápida (*roaming*). |

#### A) Detalles de IPSec
Utiliza tres subprotocolos principales:
*   **AH (Authentication Header):** Autenticación e integridad sin cifrado (sin confidencialidad).
*   **ESP (Encapsulating Security Payload):** Autenticación, integridad y confidencialidad mediante cifrado.
*   **IKE (Internet Key Exchange):** Negocia automáticamente las claves y las **SAs (Security Associations)**.
    *   *Security Association (SA):* Definición unidireccional de parámetros criptográficos para el tráfico. Se identifica por el **SPI (Security Parameter Index)**, IP destino y protocolo (AH o ESP).
    *   *SADB (Security Association Database):* Almacena las SAs activas del sistema.
    *   *SPD (Security Policy Database):* Base de datos de políticas que definen si un paquete debe descartarse (`DISCARD`), transmitirse sin proteger (`BYPASS`) o protegerse con IPSec (`PROTECT`).

#### B) Detalles de OpenVPN
Funciona en espacio de usuario a través del driver virtual TUN/TAP:
*   *Routing Mode (L3 - tun0):* Encapsula paquetes IP. No pasa tráfico L2 (ej. broadcasts o ARP). Muy escalable.
*   *Bridging Mode (L2 - tap0):* Encapsula tramas Ethernet completas, permitiendo DHCP y ARP transparentes a través del túnel. Menos escalable por la sobrecarga del tráfico de difusión.
*   *Problema de TCP sobre TCP:* Si se encapsula tráfico TCP dentro de un túnel OpenVPN configurado sobre TCP, se duplican los mecanismos de control de congestión, búferes y retransmisiones (provocando el efecto *TCP meltdown*). Se recomienda usar UDP como transporte.

#### C) Detalles de WireGuard
Implementación moderna y eficiente en kernel.
*   Mensajes: `Iniciación` (inicio de handshake), `Respuesta` (cierre de handshake y claves temporales), `Transporte de datos` (tráfico cifrado) y `Cookie reply` (defensa contra ataques DoS de handshakes).

---

## Módulo 7: Normas de Data Centers y Virtualización

### 7.1. Infraestructura de Data Centers

Un Data Center es un espacio físico diseñado para alojar equipos críticos. Se requiere operación continua **7x24** (24 horas, 7 días de la semana).

*   **Arquitectura Física:** Ubicación, seguridad física, salas de racks, refrigeración, UPS y generadores.
*   **Arquitectura Lógica:** Conectividad (routers/switches), firewalls, almacenamiento (storage), servidores y respaldos.
*   **Norma TIA-942:** Evalúa espacios, cableado estructurado, niveles de confiabilidad y control ambiental.
*   **Clasificación Tier del Uptime Institute:** Certifica tres etapas: Diseño (TCDD), Construcción (TCCF) y Sustentabilidad Operacional (TCOS).

#### Clasificación Tier (Uptime Institute):
1.  **Tier I (Básico):** Infraestructura básica sin componentes redundantes (DE: 99.671%).
2.  **Tier II (Componentes Redundantes):** Añade redundancia simple en energía y refrigeración (DE: 99.741%).
3.  **Tier III (Mantenimiento Concurrente):** Permite retirar cualquier componente para mantenimiento sin interrumpir los servicios (DE: 99.982%).
4.  **Tier IV (Tolerante a Fallas):** Soporta cualquier fallo imprevisto de un sistema o ruta de distribución sin afectar la operación (DE: 99.995%).

#### Certificación ICREA-131-2019
Clasifica las salas de cómputo en 6 niveles de disponibilidad:
*   **Nivel I:** Sala de cómputo certificada (95%).
*   **Nivel II:** Sala de cómputo clase mundial (99%).
*   **Nivel III:** Sala confiable con certificación (99.9%).
*   **Nivel IV:** Sala de alta seguridad certificada (99.99%).
*   **Nivel V:** Sala de alta seguridad y alta disponibilidad (99.999%).
*   **Nivel VI:** Red redundante de salas de cómputo (99.9999%).

---

### 7.2. Virtualización de Sistemas

La virtualización consiste en la abstracción de recursos físicos (CPU, memoria, almacenamiento, red) para presentar múltiples entornos lógicos independientes (*guests*) corriendo sobre el hardware físico (*host*) a través de una **capa de virtualización (hipervisor)**.

*   **Diferencia clave con Contenedores:**
    *   *Máquinas Virtuales:* Replicación de hardware completo. Cada VM incluye su propio sistema operativo huésped (*guest OS*), drivers y kernel. Ofrece un aislamiento profundo pero consume muchos recursos.
    *   *Contenedores:* Comparten el mismo kernel del sistema operativo anfitrión. Aíslan aplicaciones mediante espacios de nombres (*namespaces*) y cgroups. Son ligeros y rápidos, pero con aislamiento superficial a nivel de sistema operativo.

#### Tipos de Hipervisor:
*   **Type I (Bare-metal / Nativo):** Se ejecuta directamente sobre el hardware físico (ej. VMware ESXi). Debe gestionar los drivers del hardware. Alto rendimiento.
*   **Type II (Hosted / Alojado):** Se ejecuta como una aplicación sobre un sistema operativo convencional anfitrión (ej. VirtualBox). Utiliza los drivers del sistema operativo base.

#### Teorema de Popek-Goldberg
Define tres propiedades que debe cumplir un Monitor de Máquina Virtual (VMM):
1.  **Equivalencia:** El comportamiento de un programa ejecutado en la VM debe ser idéntico al de un sistema físico real.
2.  **Control de Recursos (Seguridad):** El VMM debe mantener el control completo de los recursos asignados; ningún guest puede acceder a recursos no autorizados.
3.  **Eficiencia:** Las instrucciones no privilegiadas deben ejecutarse directamente sobre el hardware real sin intervención del VMM.

#### Instrucciones Privilegiadas y Sensibles
*   **Instrucciones Privilegiadas:** Aquellas que solo se ejecutan en modo supervisor (Ring 0). Si se ejecutan en modo usuario, producen una trampa (*trap*) en el procesador.
*   **Instrucciones Sensibles:** Afectan al estado global del sistema. Se dividen en *control sensitive* (modifican asignación de recursos) y *behavior sensitive* (su comportamiento depende del estado del hardware).
*   *Condición de virtualización:* Para que una arquitectura sea virtualizable directamente, **el conjunto de instrucciones sensibles debe ser un subconjunto de las instrucciones privilegiadas**.

##### Anillos de Privilegio (Rings) e Hypervisors
La arquitectura x86 organiza la ejecución en niveles de privilegio (Ring 0 a Ring 3).
*   El hipervisor corre en Ring 0. El SO invitado no puede correr en Ring 0 porque interferiría con el hipervisor. Para solucionar esto:
    *   *Full Virtualization:* El hipervisor traduce las instrucciones privilegiadas del guest al vuelo (*binary translation*).
    *   *Paravirtualización:* Se modifica el kernel del SO invitado para que reemplace las instrucciones privilegiadas por llamadas directas al hipervisor (*hypercalls*), corriendo el guest en Ring 1.
    *   *HVM (Hardware-assisted Virtualization):* Intel VT-x y AMD-V introducen un nuevo modo de ejecución fuera de los anillos tradicionales para el hipervisor, permitiendo al guest correr en Ring 0 físico de forma aislada.

---

## Módulo 8: Resiliencia, Redundancia y Balanceo de Carga

La alta disponibilidad maximiza el tiempo de acceso de un servicio a usuarios autorizados.

### 8.1. Redundancia en Múltiples Capas
Consiste en duplicar componentes críticos para evitar puntos únicos de falla:
*   *Capa de Usuario:* Múltiples administradores y documentación clara para evitar la dependencia humana.
*   *Capa de Aplicación:* Servicios replicados (DNS, CDN, DHCP) y balanceadores como HAProxy o Nginx.
*   *Capa de Transporte:* Balanceadores TCP a ese nivel.
*   *Capa de Red:* Routers redundantes y protocolos de puerta de enlace por software (BGP, VRRP, CARP, HSRP).
*   *Capa de Enlace:* Bucles de fibra redundantes y protocolos como MSTP, Bonding LACP o MPLS.
*   *Capa Física:* Enlaces físicos duplicados y switches en *stacking*.
*   *Infraestructura:* Alimentación de energía redundante (UPS dobles, generadores a combustible) y climatización redundante de refrigeración.

---

### 8.2. Patrones de Diseño de Redundancia Maestro/Esclavo
*   **Maestro + Cold Spare (Respaldo en Frío):** El nodo de respaldo permanece apagado o sin configurar. Requiere intervención manual para activarlo ante fallas. Presenta recursos ociosos.
*   **Maestro + Hot Spare (Respaldo en Caliente):** El nodo de respaldo está encendido y sincronizando datos en tiempo real. Disminuye el tiempo de conmutación.
*   **Maestro/Esclavos con Heartbeat:** Monitoreo automatizado continuo mediante mensajes de latido (*heartbeat*). Si el esclavo detecta que el maestro no responde, toma el rol de forma automática.
*   **Maestro/Slave Read-Only:** El maestro atiende las solicitudes de escritura y replica las actualizaciones a los esclavos; la lectura se distribuye entre todos los nodos (mejora el rendimiento de bases de datos).
*   **Maestro/Maestro (Activo/Activo):** Ambos nodos operan simultáneamente atendiendo solicitudes de lectura y escritura. Requiere sincronización bidireccional compleja y puede sufrir de **split-brain** (donde tras un corte de comunicación, ambos nodos creen ser el único activo y escriben datos contradictorios).

---

### 8.3. Algoritmos y Estrategias de Balanceo de Carga
El balanceador distribuye las solicitudes de los usuarios entre múltiples réplicas de servidores:
*   **Round Robin (RR):** Distribución secuencial equitativa uno a uno sin considerar la carga actual de los servidores.
*   **Weighted Round Robin (WRR):** RR asignando pesos proporcionales a las capacidades de hardware de cada servidor.
*   **Hashing:** Distribución fija basada en una función hash de un parámetro del cliente (ej. IP origen o ID de sesión).
*   **Least Loaded (LL):** Envía la solicitud al servidor que tenga menor carga de procesamiento o conexiones activas en ese instante.
*   **Least Loaded con Slow Start:** Evita saturar servidores recién encendidos enviándoles carga de forma gradual.
*   **Utilization Limit:** Evita enviar tráfico a un servidor si este ha superado un umbral límite configurado de recursos (ej. 80% de CPU).
*   **Least Time / Least Latency:** Dirige la petición al servidor que responde con menor tiempo de latencia.
*   **Power of Two Random Choices:** El balanceador elige dos servidores al azar y envía la solicitud al menos cargado de los dos. Es altamente eficiente y consume muy pocos recursos en el propio balanceador.
*   **Cascade:** Envía solicitudes probando servidores en un orden estricto; solo pasa al siguiente si el anterior está saturado o fuera de servicio.

*Manejo del Estado:* En servicios *stateful*, el balanceador requiere de persistencia de sesión (ej. mediante *cookies* o almacenamiento de estado compartido en un servidor de caché como Redis) para evitar que el usuario pierda su sesión al ser redirigido a una réplica diferente.
