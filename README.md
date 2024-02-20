# Máquina: Blue

**Tryhackme: Blue**

Lo primero que haremos, será lanzar un NMAP para ver qué puertos tiene abiertos la máquina:

![BL1](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL1.png)
![BL2](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL2.png)
![BL3](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL3.png)

Una vez hayamos visualizado los puertos, lanzaremos de nuevo NMAP pero cargando su módulo de detección de vulnerabilidades (vuln) para intentar encontrar alguna vulnerabilidad en la máquina objetivo.

![BL4](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL4.png)

Como se observa en la imagen anterior, la máquina tiene una vulnerabilidad en el protocolo SMB. El CVE de dicha vulnerabilidad es el CVE-2017-0143.

A continuación, iniciamos Metasploit y vamos a intentar ganar acceso a la máquina a través de la vulnerabilidad que hemos encontrado.

La búsqueda que realizaremos en Metasploit, será la siguiente:

![BL5](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL5.png)

Y las opciones que configuraremos, serán la siguientes:

![BL6](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL6.png)

- RHOSTS: Aquí pondremos la dirección IP de la máquina objetivo.
- PAYLOAD: Aquí pondremos el siguiente payload: windows/x64/shell/reverse_tcp

Una vez configurado el exploit, lo lanzaremos con "run".

![BL7](https://github.com/AntonioPC94/Blue/blob/6337c6923b2e1a8ab5f05a4207e8d93b7edb39ff/img/BL7.png)

Como observamos en la imagen anterior, hemos obtenido shell. Ahora haremos un "whoami" para ver qué tipo de usuario somos en el sistema.

![BL17](https://github.com/AntonioPC94/Blue/blob/552fff35b8e43f9e11cdf47b5df2d7225fd7c1ca/img/BL17.png)

Somos "nt authority\system", la mayor autoridad del sistema.

Para comenzar con la post explotación, vamos a upgradear la shell que hemos obtenido a una "Meterpreter". Para ello, vamos a utilizar un módulo de Metasploit llamado "post/multi/manage/shell_to_meterpreter".

![BL8](https://github.com/AntonioPC94/Blue/blob/552fff35b8e43f9e11cdf47b5df2d7225fd7c1ca/img/BL8.png)

Modificamos las opciones pertinentes y lo lanzamos. Una vez lanzado, se nos abrirá una segunda sesión con una "Meterpreter" creada.

![BL9](https://github.com/AntonioPC94/Blue/blob/552fff35b8e43f9e11cdf47b5df2d7225fd7c1ca/img/BL9.png)

Ahora vamos a utilizar la herramienta "hashdump" para intentar sacar todos los hashes de las contraseñas de los distintos usuarios de la máquina.

![BL10](https://github.com/AntonioPC94/Blue/blob/552fff35b8e43f9e11cdf47b5df2d7225fd7c1ca/img/BL10.png)

Como observamos en la imagen anterior, tenemos los hashes de los 3 usuarios que hay en el sistema, pero nosotros nos vamos a centrar en el hash de la contraseña del usuario Jon, que es el que nos interesa. Antes de romperlo, vamos a tratar de identificar a qué tipo de hash nos estamos enfrentando realizando una búsqueda en internet.

![BL11](https://github.com/AntonioPC94/Blue/blob/552fff35b8e43f9e11cdf47b5df2d7225fd7c1ca/img/BL11.png)

Ahora que sabemos que el hash es de tipo NTLM, vamos a utilizar una herramienta que nos ayude a romper dicho hash, por ejemplo, John The Ripper.

![BL12](https://github.com/AntonioPC94/Blue/blob/4284fdae53980157487c066f2856c531e4bdcba2/img/BL12.png)

Como se observa en la imagen anterior, hemos conseguido romper el hash con éxito.

Por último, vamos a buscar las 3 flags que están preparadas en la máquina para ser encontradas:

# Flag 1

La primera flag, nos la encontraremos en *:\ como "*****.txt"

![BL13](https://github.com/AntonioPC94/Blue/blob/4284fdae53980157487c066f2856c531e4bdcba2/img/BL13.png)

# Flag 2

![BL15](https://github.com/AntonioPC94/Blue/blob/4284fdae53980157487c066f2856c531e4bdcba2/img/BL15.png)
![BL16](https://github.com/AntonioPC94/Blue/blob/4284fdae53980157487c066f2856c531e4bdcba2/img/BL16.png)

La segunda flag nos la encontraremos en "*:\\*******\\********\\******\\" como "*****.txt".

# Flag 3

La tercera flag, nos la encontraremos en *:\*****\***\*********\ como "*****.txt"

![BL14](https://github.com/AntonioPC94/Blue/blob/4284fdae53980157487c066f2856c531e4bdcba2/img/BL14.png)
