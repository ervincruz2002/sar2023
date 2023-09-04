# Configuración de un Servidor DNS en Ubuntu

En esta guía, aprenderás cómo configurar un servidor DNS en un sistema Ubuntu utilizando BIND (Berkeley Internet Name Domain), uno de los servidores DNS más populares en sistemas Unix/Linux.

**Nota importante:** Asegúrate de tener acceso a un servidor Ubuntu con privilegios de superusuario (root) o un usuario con privilegios sudo. Antes de realizar cambios significativos, haz copias de seguridad de tu sistema.

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

2. Agrega una zona de búsqueda directa (forward zone) para tu dominio. Reemplaza `sar2023.com` con tu propio dominio y `ip_del_servidor` con la dirección IP de tu servidor DNS:

```plaintext
zone "sar2023.com" {
    type master;
    file "/etc/bind/zones/db.sar2023.com";
};
```

3. Crea el archivo de zona de búsqueda directa:

```bash
sudo nano /etc/bind/zones/db.sar2023.com
```

4. Configura la zona de búsqueda directa agregando registros DNS. Aquí tienes un ejemplo para un servidor web:

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

En las computadoras que deseen utilizar tu servidor DNS, configura la dirección IP de tu servidor DNS en el archivo `/etc/resolv.conf` o en la configuración de red de forma adecuada.

## Paso 7: Prueba la configuración

Para probar si tu servidor DNS está funcionando correctamente, puedes usar el comando `nslookup` o `dig` desde una computadora cliente:

```bash
nslookup sar2023.com
```

Deberías obtener respuestas correctas con las direcciones IP que configuraste en tu servidor DNS.

¡Eso es todo! Ahora tienes un servidor DNS funcionando en Ubuntu con el dominio `sar2023.com`. Asegúrate de mantener tu servidor y registros DNS actualizados según tus necesidades específicas.
```
