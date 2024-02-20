# Máquina: Blue

**Tryhackme: Blue**

Lo primero que haremos, será lanzar un NMAP para ver qué puertos tiene abiertos la máquina:

![img00]()

Una vez hayamos visualizado los puertos, lanzaremos de nuevo NMAP pero cargando su módulo de detección de vulnerabilidades (vuln) para intentar encontrar alguna vulnerabilidad en la máquina objetivo.

![img01]()

Como se observa en la imagen anterior, la máquina tiene una vulnerabilidad en el protocolo SMB. El CVE de dicha vulnerabilidad es el CVE-2017-0143.

![img02]()

A continuación, iniciamos Metasploit y vamos a intentar ganar acceso a la máquina a través de la vulnerabilidad que hemos encontrado.

La búsqueda que realizaremos en Metasploit, será la siguiente:

![img03]()

Y las opciones que configuraremos, serán la siguientes:

![img04]()

- RHOSTS: Aquí pondremos la dirección IP de la máquina objetivo.
- PAYLOAD: Aquí pondremos el siguiente payload: windows/x64/shell/reverse_tcp

Una vez configurado el exploit, lo lanzaremos con "run".

![img05]()

Como observamos en la imagen anterior, hemos obtenido shell. Ahora haremos un "whoami" para ver qué tipo de usuario somos en el sistema.

![img06]()

Somos "nt authority\system", la mayor autoridad del sistema.

Para comenzar con la post explotación, vamos a upgradear la shell que hemos obtenido a una "Meterpreter". Para ello, vamos a utilizar un módulo de Metasploit llamado "post/multi/manage/shell_to_meterpreter".

![img07]()

Modificamos las opciones pertinentes y lo lanzamos. Una vez lanzado, se nos abrirá una segunda sesión con una "Meterpreter" creada.

![img08]()

Ahora vamos a utilizar la herramienta "hashdump" para intentar sacar todos los hashes de las contraseñas de los distintos usuarios de la máquina.

![img09]()

Como observamos en la imagen anterior, tenemos los hashes de los 3 usuarios que hay en el sistema, pero nosotros nos vamos a centrar en el hash de la contraseña del usuario Jon, que es el que nos interesa. Antes de romperlo, vamos a tratar de identificar a qué tipo de hash nos estamos enfrentando realizando una búsqueda en internet.

![img10]()

Ahora que sabemos que el hash es de tipo NTLM, vamos a utilizar una herramienta que nos ayude a romper dicho hash, por ejemplo, John The Ripper.

![img11]()

Como se observa en la imagen anterior, hemos conseguido romper el hash con éxito.

Por último, vamos a buscar las 3 flags que están preparadas en la máquina para ser encontradas:

# Flag 1

La primera flag, nos la encontraremos en *:\ como "*****.txt"

![img12]()

# Flag 2

La segunda flag nos la encontraremos en "*:\\*******\\********\\******\\" como "*****.txt".

# Flag 3

La tercera flag, nos la encontraremos en *:\*****\***\*********\ como "*****.txt"

![img14]()
