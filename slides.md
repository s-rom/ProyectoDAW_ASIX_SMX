---
marp: true
theme: default
class: invert
---

# Despliegue de una aplicación Web
Miguel Molina
Sergi Romero

---


# Paso 1
## Preparar las máquinas virtuales

---

Necesitaremos:

* **Virtual Box**
    * **Modo de red:** Adaptador puente
* **Sistema Operativo:** Ubuntu Server/Desktop

---

Instalamos `net-tools` y `openssh-server`

```shell
sudo apt update
sudo apt upgrade
sudo apt install net-tools
sudo apt install openssh-server
```

Mediante el comando `ifconfig` podemos obtener la IP de la máquina


```shell
sergi@server~$ ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.18.127.188  netmask 255.255.0.0  broadcast 172.18.255.255
        inet6 fe80::a00:27ff:fee7:4d23  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:e7:4d:23  txqueuelen 1000  (Ethernet)
...
```

---

Podemos conectarnos a la máquina desde *nuestra máquina Windows* con el comando `ssh`. Para ello, abrimos un cmd e introducimos el siguiente comando: 

```cmd
ssh <usuario>@<ip>
```

Donde:
* `<usuario>` es el nombre de usuario que habéis creado en la instalación de Ubuntu
* `<ip>` es la dirección IP de vuestra máquina virtual

Ejemplo: 
```shell
ssh sergi@172.18.177.188
```


--- 

# ¿Cómo sé si he completado el paso 1?

* Vuestra máquina virtual está conectada a SJO-Alumnes
* Tiene asignada una IP dentro del rango `172.18.0.0/16`
* Podeis conectaros mediante `ssh` a la máquina desde un cmd


---
# Paso 2
## Instalar el código de la aplicación web

--- 


**Instalación del código**

* Nos aseguramos de estar en nuestro directorio principal
* Descargamos el repositorio del proyecto de Github

```shell
sergi@server~$ cd
sergi@server~$ git clone <url_del_proyecto>
```

Una vez ejecutados ambos comandos, se habrá descargado una carpeta que contiene todo el código necesario para ejecutar la aplicación web.

**IMPORTANTE** Cambiad el directorio de trabajo (con `cd`) a la carpeta que se acaba de descargar

---

# Creación de un entorno virtual Python

Un entorno virtual permite gestionar y aislar las dependencias de un proyecto. 
 
1.- Creamos un entorno virtual y lo "activamos"
2.- Cuando se instale cualquier dependencia (con `pip install`) se instalará únicamente en este entorno virtual


---
# Creación del entorno virtual

* Nos aseguramos que el gestor de paquetes de python esté instalado
```shell
sergi@server~/Proyecto_DAW$ sudo apt install python3-pip
```

* Instalamos el gestor de entornos virtuales

```shell
sergi@server~/Proyecto_DAW$ sudo pip install  python3.10-venv
```


---

* Creamos el entorno virtual llamado 'proyecto_web'

```shell
sergi@server~/Proyecto_DAW$ python3 -m venv proyecto_web
```



* Activamos el entorno virtual

```shell
sergi@server~/Proyecto_DAW$ source proyecto_web/bin/activate

(proyecto_web)sergi@server~/Proyecto_DAW$
```


Fijaos que se habrá creado un directorio llamado igual que el entorno virtual y el prompt ha cambiado para indicar que estamos usando un entorno virtual.


---

# Instalación de dependencias

El proyecto debe contener un fichero llamado `requeriments.txt`. Al ejecutar el siguiente comando se instalarán todas las dependencias en nuestro entorno virtual:

```shell
(proyecto_web)sergi@server~/Proyecto_DAW$ pip install --require-virtualenv -r requeriments.txt
```

---


Ya podemos hacer la primera prueba poniendo en marcha la página web de manera provisional.


```shell
(proyecto_web)sergi@server~/Proyecto_DAW$ python3 -m flask --app app run --host=0.0.0.0
```

Para acceder a la página web, introducimos en nuestro navegador 

```
http://<ip_de_la_maquina>:5000
```