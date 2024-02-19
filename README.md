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

La opción que configuraremos, será la siguiente:

![img04]()

RHOSTS: Aquí pondremos la dirección IP de la máquina objetivo.

Nota: La IP cambió porque se me cerró la máquina de THM.

Una vez configurado el exploit, lo lanzaremos con "run".




