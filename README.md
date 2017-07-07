Entorno auto instalable basado en docker
================

**En linux:**

Se asume que ya tenemos git ;-)


**Agregamos los vhost correspondientes:**  

<code>
sudo echo "127.0.0.1 login.stp.gov.py login spr.stp.gov.py spr" >> /etc/hosts
</code>


**Nos desplazamos dentro del repositorio que acabamos de clonar y ejecutamos:**

Asumiendo que el usuario desde el cual se esta trabajando es user:

<pre>
cd /home/user/stack-docker
sh build_dev.sh
</pre>

Con esto el script comenzará a instalar las dependencias necesarias y nos pedirá reiniciar el equipo, volvemos a correr el script.

La instalación completa lleva aproximadamente 30minutos y se descargan aproximadamente 1GB de datos.

Una vez completada la instalación, **iniciamos el entorno**.

**Ciclo de Desarrollo**:
========================

1. **Iniciamos el entorno**:
<pre>
cd /home/user/stack-docker
sh run.sh
</pre>

2. **Para acceder al sistema utilizar las siguientes credenciales**:

**Dirección**: http://spr.stp.gov.py  
**Usuario**: pruebadgtic@stp.gov.py  
**Contraseña**: stp123  

3. **Modificamos el codigo**:

* Los repositorios son clonados automaticamente en el directorio "repos".

4. **Recompilamos el codigo**:

Nos desplazamos dentro del repositorio correspondiente:

* /home/user/stack/repos/spr (Sistema de Planificacion por Resultados)
* /home/user/stack/repos/tcp (Tablero de Control Presidencial)


y ejecutamos:  

<code>mvn clean install </code>

a fines de recompilar el codigo


**Para ver la lista de contenedores ejecutar:**

<code>docker ps</code>

**Para ver los logs del tomcat ejecutar:**

<code>docker logs -f stack_backend_1 </code>

**Para iniciar sesion ssh dentro de un contenedor correr:**

<code>docker exec -it stack_XXXX_1 /bin/bash</code>  
o  
<code>docker exec -it stack_XXXX_1 /bin/sh</code>  


Correspondiendo XXXX, al nombre del equipo correspondiente:

* db
* cache
* backend
* front

Según descripción en stp-dev.yml.



Problemas Conocidos
=======


En caso del que docker levante sore IPV6, los vhost dirigidos a 127.0.0.1 no serán resueltos de manera apropiada.

**Existen dos workarounds posibles**:

* **Apuntar sobre ipv6**:

<code>
sudo echo "::1 login.stp.gov.py login spr.stp.gov.py spr" >> /etc/hosts
sudo echo ":: login.stp.gov.py login spr.stp.gov.py spr" >> /etc/hosts
</code>


* **Deshabilitar ipv6**:

<code>
sudo nano /etc/default/grub
</code>

Reemplazar la linea:

<code>
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
</code>

con:

<code>
GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1 quiet splash"
</code>

Actualizamos grub:

<code>
sudo update-grub
</code>

Y finalmente reiniciamos, deberia estar resuelto el issue.
