## [Volver atrás](/readme.md)

<div align="center">
<h1>TP 2. Herramientas de Gestión de Redes</h1>
</div>

**Objetivo**: Conocer el funcionamiento de un conjunto de herramientas que permitan gestionar, de forma básica, los recursos red. Conocer las características disponibles y el funcionamiento de uno de los protocolos más utilizados para la administración remota de infraestructura de red (routers, hosts y servidores). Categorías ISO: FCAPS.

## Bibliografía

- GORALSKI, W. 2017. Capítulo 30: “Secure Shell (Remote Access)” en The Illustrated Network: How TCP/IP Works in a Modern Network (2nd ed). Morgan Kaufmann.
- GEERLING, J. 2015. Capítulo 11: [“Server Security and Ansible”. Sección “A brief history of SSH and remote access” en Ansible for DevOps: Server and configuration management for humans. LeanPub.](https://www.jeffgeerling.com/blog/brief-history-ssh-and-remote-access)
- YLONEN, T.; LONVICK. C. 2006. [“RFC 4251: The Secure Shell (SSH) Protocol Architecture”. The Internet Society.](https://tools.ietf.org/html/rfc4253)


### Secure Shell (SSH)

El Secure Shell Protocol, más conocido como SSH, es un esquema de arquitectura cliente/servidor que crea un canal seguro de comunicación entre dos dispositivos sobre una red insegura. Se vale de técnicas de autenticación y cifrado mediante claves tanto simétricas como asimétricas. Esta clase cubrirá los usos más habituales del protocolo, mientras que los aspectos de criptografía se tratarán en clases posteriores de la asignatura.

El caso de uso más común es cuando un usuario se quiere conectar desde un host a la terminal de un servidor o router en un lugar distante. Para ello inicia una conexión con un cliente SSH que se comunica con el servidor que tiene un servicio de SSH activo. El servidor le solicitará la contraseña para el usuario que se desea comunicar.

Además de la funcionalidad de ejecución de comandos en forma remota (inicio de sesión y login remoto), el protocolo SSH sienta las bases para otros protocolos que operan sobre él, sin requerir servicios ni puertos adicionales en escucha en el sistema.

Sobre la infraestructura que proporciona el protocolo ssh se pueden utilizar diferentes utilidades:

- Acceso a shell remoto con el comando **ssh**.
- Copia de archivos a host remotos con el comando **scp**.
- Servicio ftp con el comando **sftp**.
- Servicio de file system remoto con **sshfs**.
- Creación de túneles para acceder a puertos en máquinas remotas, con el comando **ssh**.

## Trabajo Práctico

1. El equipo docente ha puesto a disposición un servidor remoto con un webserver y algunos otros servicios. ¿Qué herramienta puede utilizar para saber que procesos están en escucha en dicho equipo?

    ```
    ❯ nmap 178.104.92.103
    Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-19 13:25 -0300
    Nmap scan report for static.103.92.104.178.clients.your-server.de (178.104.92.103)
    Host is up (0.25s latency).
    Not shown: 997 closed tcp ports (conn-refused)
    PORT     STATE SERVICE
    22/tcp   open  ssh
    80/tcp   open  http
    1022/tcp open  exp2

    Nmap done: 1 IP address (1 host up) scanned in 10.33 seconds
    ```

    La herramienta nmap, que se utilizó en el trabajo práctico anterior, permite escanear los puertos de un host remoto para descrubir qué servicios/puertos están escuchando.

2. Inicie una sesión remota mediante SSH hacia el servidor remoto utilizando el nombre de usuario y contraseña provistos en clase.

    ```
    ❯ ssh mateon@178.104.92.103
    The authenticity of host '178.104.92.103 (178.104.92.103)' can't be established.
    ED25519 key fingerprint is: SHA256:jUnWIzSpvorWpdBsbIieKK7kQFClmf1NWb24pgKh1VU
    This key is not known by any other names.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '178.104.92.103' (ED25519) to the list of known hosts.
    mateon@178.104.92.103's password: 
    Linux aygr-ssh-server 6.12.74+deb13+1-arm64 #1 SMP Debian 6.12.74-2 (2026-03-08) aarch64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    mateon@aygr-ssh-server:~$ 
    ```

3. Utilizando **netstat**, obtenga un listado de los procesos que se encuentran en escucha en el host remoto. Guarde esta información en un archivo.

    ```
    mateon@aygr-ssh-server:~$ netstat -tnlp >> procesos_en_escucha.txt
    (No info could be read for "-p": geteuid()=1007 but you should be root.)
    mateon@aygr-ssh-server:~$ ls
    procesos_en_escucha.txt
    mateon@aygr-ssh-server:~$ cat procesos_en_escucha.txt 
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 0.0.0.0:1022            0.0.0.0:*               LISTEN      -                   
    tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN      -                   
    tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
    tcp6       0      0 :::1022                 :::*                    LISTEN      -                   
    tcp6       0      0 :::80                   :::*                    LISTEN      -                   
    tcp6       0      0 ::1:8000                :::*                    LISTEN      -                   
    tcp6       0      0 :::22                   :::*                    LISTEN      -      
    ```

    Los argumentos **-tnlp** de la herramienta **netstat** significan:
    - t: sólamente TCP.
    - n: mostrar IPs y puertos numéricos.
    - l: sólamente puertos escuchando.
    - p: mostrar procesos. Esta opción necesita privilegios de superusuario.

    ```
    ❯ sshfs mateon@178.104.92.103: carpeta_remota
    mateon@178.104.92.103's password: 

    ❯ ls carpeta_remota
     procesos_en_escucha.txt
    ```

    **sshfs** es una herramienta que monda el sistema de archivos del host remoto como si fuera un directorio local mediante ssh, permitiendo copiar el archivo sin usar scp o sftp.

4. La cuenta de usuario que está utilizando tiene privilegios limitados. ¿De qué manera(s) es posible “elevar privilegios” a fin de disponer de mayor control sobre el host? ¿Son éstas las únicas formas posibles?

    Con **sudo** se puede ejecutar un comando específico como root, utilizando la contraseña del usuario actual. Por ejemplo:
    ```
    sudo nano /etc/smokeping/config
    ```

    **su -** cambia al usuario root utilizando la contraseña de root.

    Otras manera de elevar privilegios serían mediante exploits sobre el kernel, que aprovechan fallas para obtener acceso root sin necesidad de autenticación.

5. Utilice alguno de los mecanismos tratados en el punto previo para elevar privilegios. Una vez obtenido control de la cuenta privilegiada, obtenga el registro (log en inglés) de autenticación. Dependiendo de la versión del sistema operativo Linux utilizado, puede que esté alojado en el archivo **/var/log/auth.log** (si existe), o bien puede obtenerse mediante el comando **journalctl -u ssh | tee /tmp/auth.log**

    ```
    mateon@aygr-ssh-server:~$ su -
    Password: 
    root@aygr-ssh-server:~# whoami
    root
    ```

    ```
    root@aygr-ssh-server:~# journalctl -u ssh | tee /tmp/auth.log
    root@aygr-ssh-server:~# ls /tmp/auth.log
    /tmp/auth.log
    ```

6. Utilizando los comandos **scp** o **sftp**, copie archivo **auth.log** de la máquina remota y guárdelo en su máquina local. Revise el archivo buscando los registros correspondientes a los inicios de sesión recientemente realizados. Preste atención a los registros que digan **Accepted**, **Failed** o **Invalid user**. Indique qué información se brinda en cada evento. Indique cuales son los nombres de usuario más “intentados”. Utilizando alguna herramienta en línea, determine a qué país y/o ISP corresponden las IP que aparecen.

    ```
    ❯ sftp mateon@178.104.92.103
    mateon@178.104.92.103's password: 
    Connected to 178.104.92.103.
    sftp> cd /tmp                         
    sftp> get auth.log
    Fetching /tmp/auth.log to auth.log
    auth.log                                                                    100% 1238KB 526.4KB/s   00:02    
    sftp> exit
    ```

    Para saber cuales fueron los intentos aceptados, fallidos, o de usuarios inválidos, utilizo el comando **grep** sobre el archivo auth.log, y **tail** para mostrar las últimas líneas ya que hay varias entradas:

    ```
    ❯ grep "Accepted" auth.log | tail -20
    Mar 18 23:23:15 aygr-ssh-server sshd-session[16261]: Accepted password for marioc from 190.7.225.2 port 46942 ssh2
    Mar 18 23:24:27 aygr-ssh-server sshd-session[16707]: Accepted password for marioc from 190.7.225.2 port 49824 ssh2
    Mar 18 23:26:20 aygr-ssh-server sshd-session[17466]: Accepted password for anapaular from 181.170.124.46 port 58954 ssh2
    Mar 18 23:26:37 aygr-ssh-server sshd-session[17485]: Accepted password for tobiasr from 181.170.102.186 port 39438 ssh2
    Mar 18 23:26:49 aygr-ssh-server sshd-session[17682]: Accepted password for maurom from 200.110.191.254 port 54640 ssh2
    Mar 18 23:27:14 aygr-ssh-server sshd-session[17794]: Accepted password for tobiasr from 181.170.102.186 port 56256 ssh2
    Mar 18 23:42:11 aygr-ssh-server sshd-session[23298]: Accepted password for maurom from 200.110.191.254 port 55128 ssh2
    Mar 18 23:42:24 aygr-ssh-server sshd-session[23388]: Accepted password for trinidadl from 190.191.85.169 port 63265 ssh2
    Mar 18 23:44:44 aygr-ssh-server sshd-session[24260]: Accepted password for trinidadl from 178.104.92.103 port 50562 ssh2
    Mar 18 23:45:00 aygr-ssh-server sshd-session[24352]: Accepted password for maurom from 200.110.191.254 port 55170 ssh2
    Mar 18 23:54:30 aygr-ssh-server sshd-session[28203]: Accepted password for maurom from 200.110.191.254 port 55199 ssh2
    Mar 18 23:55:14 aygr-ssh-server sshd-session[28549]: Accepted password for maurom from 200.110.191.254 port 55220 ssh2
    Mar 18 23:57:27 aygr-ssh-server sshd-session[29360]: Accepted password for maurom from 200.110.191.254 port 55241 ssh2
    Mar 19 08:52:50 aygr-ssh-server sshd-session[35734]: Accepted publickey for root from 186.157.63.185 port 4210 ssh2: RSA SHA256:ailZ+LI/kg9EsOtfXWzDbxPPmB2jIQ6+D8GX42Odze4
    Mar 19 10:33:13 aygr-ssh-server sshd-session[36206]: Accepted password for trinidadl from 190.191.85.169 port 49672 ssh2
    Mar 19 10:34:04 aygr-ssh-server sshd-session[36244]: Accepted password for trinidadl from 178.104.92.103 port 40332 ssh2
    Mar 19 10:35:01 aygr-ssh-server sshd-session[36261]: Accepted password for trinidadl from 190.191.85.169 port 49321 ssh2
    Mar 19 10:39:13 aygr-ssh-server sshd-session[36340]: Accepted password for trinidadl from 178.104.92.103 port 53986 ssh2
    Mar 19 10:54:46 aygr-ssh-server sshd-session[36376]: Accepted password for trinidadl from 190.191.85.169 port 57579 ssh2
    Mar 19 16:33:56 aygr-ssh-server sshd-session[37169]: Accepted password for mateon from 181.96.134.177 port 22364 ssh2
    ```

    ```
    ❯ grep "Failed" auth.log | tail -20
    Mar 19 16:59:40 aygr-ssh-server sshd-session[37344]: Failed password for root from 45.148.10.147 port 12078 ssh2
    Mar 19 16:59:44 aygr-ssh-server sshd-session[37344]: Failed password for root from 45.148.10.147 port 12078 ssh2
    Mar 19 16:59:47 aygr-ssh-server sshd-session[37344]: Failed password for root from 45.148.10.147 port 12078 ssh2
    Mar 19 17:01:32 aygr-ssh-server sshd-session[37351]: Failed password for root from 195.178.110.15 port 55076 ssh2
    Mar 19 17:01:34 aygr-ssh-server sshd-session[37351]: Failed password for root from 195.178.110.15 port 55076 ssh2
    Mar 19 17:01:37 aygr-ssh-server sshd-session[37351]: Failed password for root from 195.178.110.15 port 55076 ssh2
    Mar 19 17:01:39 aygr-ssh-server sshd-session[37351]: Failed password for root from 195.178.110.15 port 55076 ssh2
    Mar 19 17:01:42 aygr-ssh-server sshd-session[37351]: Failed password for root from 195.178.110.15 port 55076 ssh2
    Mar 19 17:03:30 aygr-ssh-server sshd-session[37362]: Failed password for root from 2.57.121.118 port 48166 ssh2
    Mar 19 17:03:32 aygr-ssh-server sshd-session[37362]: Failed password for root from 2.57.121.118 port 48166 ssh2
    Mar 19 17:03:35 aygr-ssh-server sshd-session[37362]: Failed password for root from 2.57.121.118 port 48166 ssh2
    Mar 19 17:03:37 aygr-ssh-server sshd-session[37362]: Failed password for root from 2.57.121.118 port 48166 ssh2
    Mar 19 17:03:42 aygr-ssh-server sshd-session[37362]: Failed password for root from 2.57.121.118 port 48166 ssh2
    Mar 19 17:05:29 aygr-ssh-server sshd-session[37370]: Failed password for root from 2.57.122.189 port 51018 ssh2
    Mar 19 17:05:32 aygr-ssh-server sshd-session[37370]: Failed password for root from 2.57.122.189 port 51018 ssh2
    Mar 19 17:05:34 aygr-ssh-server sshd-session[37370]: Failed password for root from 2.57.122.189 port 51018 ssh2
    Mar 19 17:05:36 aygr-ssh-server sshd-session[37370]: Failed password for root from 2.57.122.189 port 51018 ssh2
    Mar 19 17:05:38 aygr-ssh-server sshd-session[37370]: Failed password for root from 2.57.122.189 port 51018 ssh2
    Mar 19 17:05:52 aygr-ssh-server sshd-session[37378]: Failed password for invalid user admin from 132.145.140.70 port 27892 ssh2
    Mar 19 17:06:30 aygr-ssh-server sshd-session[37380]: Failed password for invalid user orangepi from 132.145.140.70 port 10286 ssh2
    ```

    ```
    ❯ grep "Invalid user" auth.log | tail -20
    Mar 19 15:57:39 aygr-ssh-server sshd-session[36996]: Invalid user init from 213.209.159.158 port 32354
    Mar 19 15:57:59 aygr-ssh-server sshd-session[36998]: Invalid user kenny from 213.209.159.158 port 8198
    Mar 19 15:58:14 aygr-ssh-server sshd-session[37001]: Invalid user qemu from 213.209.159.158 port 46086
    Mar 19 15:58:32 aygr-ssh-server sshd-session[37003]: Invalid user juan from 213.209.159.158 port 47028
    Mar 19 15:58:50 aygr-ssh-server sshd-session[37005]: Invalid user mm from 213.209.159.158 port 48318
    Mar 19 15:59:08 aygr-ssh-server sshd-session[37007]: Invalid user mary from 213.209.159.158 port 38352
    Mar 19 15:59:26 aygr-ssh-server sshd-session[37010]: Invalid user inami from 213.209.159.158 port 25920
    Mar 19 15:59:44 aygr-ssh-server sshd-session[37012]: Invalid user phpmyadmin from 213.209.159.158 port 57014
    Mar 19 16:00:01 aygr-ssh-server sshd-session[37014]: Invalid user ovh from 213.209.159.158 port 25346
    Mar 19 16:00:19 aygr-ssh-server sshd-session[37016]: Invalid user rabbitmq from 213.209.159.158 port 59348
    Mar 19 16:00:37 aygr-ssh-server sshd-session[37018]: Invalid user localtest from 213.209.159.158 port 11438
    Mar 19 16:00:57 aygr-ssh-server sshd-session[37020]: Invalid user gituser from 213.209.159.158 port 51066
    Mar 19 16:01:15 aygr-ssh-server sshd-session[37023]: Invalid user jerry from 213.209.159.158 port 62928
    Mar 19 16:01:33 aygr-ssh-server sshd-session[37025]: Invalid user kazuyuki from 213.209.159.158 port 3888
    Mar 19 16:01:48 aygr-ssh-server sshd-session[37028]: Invalid user pgsql from 213.209.159.158 port 23004
    Mar 19 16:02:39 aygr-ssh-server sshd-session[37036]: Invalid user post from 213.209.159.158 port 29514
    Mar 19 16:02:57 aygr-ssh-server sshd-session[37038]: Invalid user huwentao from 213.209.159.158 port 37870
    Mar 19 16:53:40 aygr-ssh-server sshd-session[37312]: Invalid user support from 91.202.233.33 port 48424
    Mar 19 17:05:51 aygr-ssh-server sshd-session[37378]: Invalid user admin from 132.145.140.70 port 27892
    Mar 19 17:06:28 aygr-ssh-server sshd-session[37380]: Invalid user orangepi from 132.145.140.70 port 10286
    ```

    ```
    ❯ grep "Invalid user" auth.log | awk '{print $8}' | sort | uniq -c | sort -rn | head -3
        83 admin
        63 ubuntu
        39 test
    ```

    Los nombres de usuarios más intentados son admin, ubuntu y test.

    ```
    ❯ grep "Failed\|Invalid" auth.log | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -c | sort -rn | head -3
    188 152.42.140.188
    187 59.24.226.8
    187 46.224.146.18
    ```

    Las IPs con mayor cantidad de intentos fallidos son 152.42.140.188, 59.24.226.8 y 46.224.146.18

    ```
    ❯ curl ipinfo.io/152.42.140.188
    curl ipinfo.io/59.24.226.8
    curl ipinfo.io/46.224.146.18
    {
    "ip": "152.42.140.188",
    "city": "Amsterdam",
    "region": "North Holland",
    "country": "NL",
    "loc": "52.3740,4.8897",
    "org": "AS14061 DigitalOcean, LLC",
    "postal": "1012",
    "timezone": "Europe/Amsterdam",
    "readme": "https://ipinfo.io/missingauth"
    }{
    "ip": "59.24.226.8",
    "city": "Yeongju",
    "region": "Gyeongsangbuk-do",
    "country": "KR",
    "loc": "36.8217,128.6308",
    "org": "AS4766 Korea Telecom",
    "postal": "36068",
    "timezone": "Asia/Seoul",
    "readme": "https://ipinfo.io/missingauth"
    }{
    "ip": "46.224.146.18",
    "hostname": "static.18.146.224.46.clients.your-server.de",
    "city": "Nürnberg",
    "region": "Bavaria",
    "country": "DE",
    "loc": "49.4542,11.0775",
    "org": "AS24940 Hetzner Online GmbH",
    "postal": "90402",
    "timezone": "Europe/Berlin",
    "readme": "https://ipinfo.io/missingauth"
    }
    ```

    Los paises de las tres IPs que más intentos fallidos tuvieron son Holanda, Corea del Sur, y Alemania.


7. En el host remoto se encuentra una aplicación web que solo es accesible por la interfaz loopback (**127.0.0.1**). Resulta necesario acceder a esta aplicación desde su propio equipo. ¿Qué estrategia puede utilizarse para poder acceder a dicho servicio?

    Una estrategia para poder acceder al servicio web es, una vez conectado mediante ssh al host remoto, utilizar la herramienta curl de la siguiente manera:

    ```
    root@aygr-ssh-server:~# curl http://localhost:8000
    <!DOCTYPE html><html><title>Webserver privado</title><body><h1>Felicitaciones\!</h1><p>Has encontrado el web server privado.</p></body></html>
    ```

    Una alternativa sería la siguiente, a partir del argumento -L de ssh podemos crear un tunnel que abre un puerto (en este caso 1111) en nuestro equipo y reenvía el tráfico a un socket del host remoto (127.0.0.1:8000):

    ```
    ❯ ssh -L 1111:localhost:8000 mateon@178.104.92.103
    mateon@178.104.92.103's password: 
    Linux aygr-ssh-server 6.12.74+deb13+1-arm64 #1 SMP Debian 6.12.74-2 (2026-03-08) aarch64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Thu Mar 19 16:33:57 2026 from 181.96.134.177
    ```

    Luego en mi equipo se puede obtener el documento mediante curl de la siguiente manera:

    ```
    ❯ curl http://localhost:1111 
    <!DOCTYPE html><html><title>Webserver privado</title><body><h1>Felicitaciones\!</h1><p>Has encontrado el web server privado.</p></body></html>
    ```

8. Analice las siguientes situaciones:

    1. Un servidor / servicio en remote.server.com está escuchando el puerto 11000. Desea usar su propia computadora personal local para conectarse. Sin embargo, el puerto 11000 está oculto detrás del firewall de remote.server.com.

        ```
        ssh -L 9000:localhost:11000 user@remote.server.com
        ```

        Accedemos localmente a la dirección http://localhost:9000 desde un navegador o utilizando curl.

    2. Un servidor / servicio en su computadora personal local está escuchando en el puerto 11000. Desea que remote.server.com se conecte a su servidor / servicio. Sin embargo, su computadora personal local no tiene una dirección de acceso público en Internet.

        ```
        ssh -R 11000:localhost:9000 user@remote.server.com 
        ```
        Accedemos desde el servidor remoto a la dirección http://localhost:9000 mediante un navegador o utilizando curl.

        Escriba los comandos necesarios para poder acceder a estos servicios.

        Referencias: https://builtin.com/software-engineering-perspectives/ssh-port-forwarding y https://ittavern.com/visual-guide-to-ssh-tunneling-and-port-forwarding/

9. ¿Qué riesgos presenta que el usuario **root** tenga acceso a un host mediante SSH? ¿Cómo puede habilitarse o deshabilitarse esta característica?

    Los riesgos que presenta que el usuario root tenga acceso a un host mediante ssh es que los bots que utilizan fuerza bruta (como los que aparecen en el auth.log) prueben iniciar sesión como root. Otro problema es que si varios usuarios usan root, no se puede distinguir con exactitud quién hizo qué bajo permisos de superusuario, esto se puede evitar si cada usuario usa su propio usuario y eleva sus privilegios con sudo, ya que queda registro de esto en los logs. El problema más grave es que un usuario root puede destruir el sistema completo.

    Para modificar esta característica, se debe utilizar la directiva PermitRootLogin junto con los argumentos no/prohibit-password/yes dentro del archivo /etc/ssh/sshd_config. prohibit-password deshabilita el acceso root sin contraseña, y requiere autenticación con clave pública. Luego de modificar el archivo, se debe recargar el servicio de ssh.
