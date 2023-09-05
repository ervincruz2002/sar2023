# Configuración de un Servidor DNS en Ubuntu Server

En esta guía, aprenderás cómo configurar un servidor DNS en un sistema Ubuntu utilizando BIND (Berkeley Internet Name Domain), uno de los servidores DNS más populares en sistemas Unix/Linux.

## Link al video del Laboratorio de DNS en Ubuntu Server: 

[Video Explicacion Laboratorio DNS](https://youtu.be/mjiwamWDz38)

## Paso 1: Actualizar el sistema

Antes de comenzar, actualiza tu sistema Ubuntu:

```bash
sudo apt update
sudo apt upgrade
```

## Paso 2: Instalar BIND9

Instala el paquete BIND9, que se utilizará para configurar el servidor DNS:

```bash
sudo apt install bind9
```

## Paso 3: Configurar el servidor DNS

1. Abre el archivo de configuración principal de BIND:

```bash
sudo nano /etc/bind/named.conf.local
```

2. Agrega una zona de búsqueda directa (forward zone) para tu dominio. Reemplaza `ip_del_servidor` con la dirección IP de tu servidor DNS:

```plaintext
zone "sar2023.com" {
    type master;
    file "/etc/bind/zones/db.sar2023.com";
};
```
2. Crea el directorio `zones` en `/etc/bind/`:
```bash
sudo mkdir /etc/bind/zones/
```
4. Crea el archivo de zona de búsqueda directa:

```bash
sudo nano /etc/bind/zones/db.sar2023.com
```

4. Configura la zona de búsqueda directa agregando registros DNS. Reemplaza `ip_del_servidor` con la dirección IP de tu servidor DNS: Aquí un ejemplo para un servidor web:

```plaintext
$TTL    86400
@       IN      SOA     ns1.sar2023.com. admin.sar2023.com. (
                        2023090401     ; Serial
                        86400           ; Refresh
                        7200            ; Retry
                        604800          ; Expire
                        86400 )         ; Minimum TTL
;
@       IN      NS      ns1.sar2023.com.
@       IN      A       ip_del_servidor
www     IN      A       ip_del_servidor
```

5. Guarda y cierra el archivo.

## Paso 4: Configurar reenviadores (forwarders)

Si deseas que tu servidor DNS reenvíe consultas DNS no resueltas a otros servidores DNS, configura reenviadores:

```bash
sudo nano /etc/bind/named.conf.options
```

Agrega las siguientes líneas, reemplazando `8.8.8.8` y `8.8.4.4` con las direcciones IP de los servidores DNS que deseas utilizar como reenviadores:

```plaintext
forwarders {
    8.8.8.8;
    8.8.4.4;
};
```

## Paso 5: Reiniciar el servidor DNS

Reinicia el servicio BIND9 para aplicar los cambios:

```bash
sudo systemctl restart bind9
```

## Paso 6: Configurar la resolución DNS en el cliente

En las computadoras que deseen utilizar tu servidor DNS, configura la dirección IP de tu servidor DNS en el archivo `/etc/resolv.conf` o en la configuración de red de forma adecuada. En el archivo `resolv.conf` colocar lo siguiente:
```bash
nameserver ip_del_servidor
```

## Paso 7: Prueba la configuración

Para probar si tu servidor DNS está funcionando correctamente, puedes usar el comando `nslookup` o `dig` desde una computadora cliente:

```bash
nslookup google.com
```

Deberías obtener respuestas correctas con las direcciones IP que configuraste en tu servidor DNS.



# Configuración de un Servidor DNS en Windows Server (Demostracion)

En esta guía, aprenderás cómo configurar un servidor DNS en un sistema Windows Server y verificar su funcionamiento mediante Ubuntu Server.

## Link al video del Laboratorio de DNS en Windows Server:

[Video Demostracion DNS](https://youtu.be/Cb2KwLBDjPE)

## Paso 1: Abrimos Windows Server

Esta es la interfaz principal de Windows Server:

![Interfaz principal de Windows Server](https://github.com/ervincruz2002/sar2023/blob/main/demo0.jpeg)

## Paso 2: Asignamos el rol de servidor DNS a nuestro Windows Server:

1. Nos vamos al segundo apartado `Agregar roles y características` y damos click en siguiente hasta que nos salga este apartado:

![Ventana de configuracion de roles y caracteristicas](https://github.com/ervincruz2002/sar2023/blob/main/demo1.jpeg)

2. Ahi le damos a la opcion de servidor DNS y hacemos click en agregar, le damos siguiente y por ultimo instalar. Y comenzara a instalar el DNS:
   
![Ventana de configuracion de roles y caracteristicas](https://github.com/ervincruz2002/sar2023/blob/main/demo2.jpeg)

## Paso 3: Configuracion del servidor DNS:

1.  Nos vamos al apartado `Herramientas>DNS`:

![Seleccion del apartado Herramientas>DNS en la ventana principal de Windows Server](https://github.com/ervincruz2002/sar2023/blob/main/demo3.jpeg)

2. Nos mostrará el apartado siguiente:

![Ventana Administrador DNS](https://github.com/ervincruz2002/sar2023/blob/main/demo4.jpeg)

3. En el apartado `Zonas de búsqueda directa`, damos click derecho y damos click izquierdo en `Zona nueva`:

![Seleccion de Zonas de busqueda directa>Zona Nueva](https://github.com/ervincruz2002/sar2023/blob/main/demo5.jpeg)

4. Damos click en siguiente hasta que nos salga nombre de zona, ahi le asignamos uno:

![Asignacion del nombre de la nueva zona](https://github.com/ervincruz2002/sar2023/blob/main/demo6.jpeg)

5. Damos click en siguiente, siguiente y finalizar.

6. Luego realizamos lo mismo con zonas de búsqueda invertida, damos click derecho y `Zona nueva` y debería de salir el siguiente apartado:

![Ventana de asistente para nueva zona](https://github.com/ervincruz2002/sar2023/blob/main/demo7.jpeg)

7. Colocamos la direccion ip de nuestro server.
   
8. Damos click en siguiente, siguiente y finalizar.

9. Nos vamos a la zona anteriormente creada:

![Ventana Adninistrador DNS](https://github.com/ervincruz2002/sar2023/blob/main/demo8.jpeg)

10. Damos click derecho y luego click izquierdo en `Host nuevo A o AAAA`:

![Ventana Adninistrador DNS](https://github.com/ervincruz2002/sar2023/blob/main/demo9.jpeg)

11. Colocamos la dirección ip de nuestro server y damos click en `Agregar host`:
    
![Ventana Host Nuevo](https://github.com/ervincruz2002/sar2023/blob/main/demo10.jpeg)

Es opcional agregar un host www, si se quiere agregar uno solo, en nombre de host se coloca el www y abajo la ip del server.

## Paso 4: Configuracion del cliente:

1. En un Ubuntu Server nos vamos a editar el archivo `/etc/resolv.conf` y en la configuracion `nameserver` colocamos la ip de nuestro servidor DNS:

```bash
nameserver 192.168.126.134
options edns0 trust-ad
search localdomain
```

3. Cerramos y guardamos el archivo.

## Paso 5: Pruebas de funcionamiento:
Podemos comprobar el server con el comando `nslookup nombre_del_server` o `nslookup ip_del_server`. Y tendriamos un ouput similar a este:

```bash
silva2405@sar2023:~$ nslookup sar2023.com.co
Server:         192.168.126.134
Address:        192.168.126.134#53

Name:   sar2023.com.co
Address: 192.168.126.134

silva2405@sar2023:~$ nslookup 192.168.126.134
134.126.168.192.in-addr.arpa    name = www.sar2023.com.co.
134.126.168.192.in-addr.arpa    name = sar2023.com.co.

silva2405@sar2023:~$
```



   

   





