# ScPrime docker configuration

- [ScPrime docker configuration](#scprime-docker-configuration)
- [Documentacion Oficial](#documentacion-oficial)
- [Puertos del router](#puertos-del-router)
- [Montaje de carpetas](#montaje-de-carpetas)
  - [Synology](#synology)
  - [Otros](#otros)
    - [Docker data](#docker-data)
    - [Carpetas de almacenamiento](#carpetas-de-almacenamiento)
- [Montaje/Configuración](#montajeconfiguración)
  - [Arrancamos el docker](#arrancamos-el-docker)
  - [Revisar sincronización/estado](#revisar-sincronizaciónestado)
    - [Verificamos que los puertos estén abiertos](#verificamos-que-los-puertos-estén-abiertos)
    - [Verificación de la sincronización](#verificación-de-la-sincronización)
  - [Billetera](#billetera)
  - [Conexión al docker (opcional pero recomendado):](#conexión-al-docker-opcional-pero-recomendado)
  - [Añadir hosts](#añadir-hosts)
  - [Recompensas](#recompensas)
- [Gestión de la cartera](#gestión-de-la-cartera)
  - [Obtener Wallet publico (recibir SCP)](#obtener-wallet-publico-recibir-scp)
  - [Detalles del wallet](#detalles-del-wallet)
- [Anunciar el servidor](#anunciar-el-servidor)
- [Donate SCP](#donate-scp)


# Documentacion Oficial

https://docs.scpri.me/storageproviderindex/synology-docker-setup

# Puertos del router

Tenemos que abrir los puertos:

- 4282
- 4283
- 4285

# Montaje de carpetas

Creamos carpetas que contendrán hasta máximo `2TB`.

> 1 volumen = 1 carpeta de max 2TB


La nomenclatura que usaremos será scp_X_Y` siendo `X` el disco e `Y` el volumen.

Si solo tenemos un disco, pondremos 1 igualmente (para ampliación), lo mismo con el volumen.

## Synology

Creamos los volumenes `scp_X_Y` siendo `X` el disco e `Y` el volumen.

Cada unidad contendrá máximo 2TB

EJ:
- scp_1_1
- scp_1_2
- scp_2_1


## Otros

### Docker data

Creamos la carpeta `scp-data` donde queramos y modificamos el `volume` del `docker-compose.yml` correspondiente (`/volume2/docker/scp-data:/scp-data`)


### Carpetas de almacenamiento

Creamos las carpetas siguiendo la nomenclatura indicada dónde queramos:

- scp_1_1
- scp_1_2
- scp_2_1



# Montaje/Configuración

## Arrancamos el docker

1. Nos situamos en la carpeta del `docker-compose.yml` 
2. Creamos el docker ejecutando:

```bash
docker-compose up -d
```


## Revisar sincronización/estado

### Verificamos que los puertos estén abiertos

> Cambia la IP por la DNS que vayas a probar

https://www.ipfingerprints.com/portscan.php

- 4282
- 4283
- 4285

### Verificación de la sincronización

```bash
docker exec scprime spc
```

## Billetera

1. Creamos la semilla

```bash
spc wallet init
```

2. La añadimos en `SCPRIME_WALLET_PASSWORD` del `docker-compose.yml`
3. Recargamos el docker con `docker-compose up -d`


## Conexión al docker (opcional pero recomendado):

> Esto es opcional, simplemente cambia los comandos de `scp` por `sudo docker exec scp`

```bash
sudo docker exec -it scprime /bin/sh
```


## Añadir hosts

> Haz esto por cada carpeta que hayas creado, recueda, máximo `2000gb` (2TB) por **carpeta**

```bash
spc host folder add /scp_1_1/ 520gb
```

## Recompensas

> Recompensas recomendadas: https://docs.scpri.me/storageproviderindex/settings

```bash
spc host config maxduration 16w
spc host config collateral 5SCP
spc host config minstorageprice 5SCP
spc host config mindownloadbandwidthprice 1SCP
spc host config minuploadbandwidthprice 1SCP
spc host config collateralbudget 1KS
spc host config maxcollateral 50SCP
```

# Gestión de la cartera

> A partir de aquí entendemos que te has desconectado del docker (con `exit` o como sea)

## Obtener Wallet publico (recibir SCP)

> Cada vez que uses el comando, te dará un nuevo wallet público.

> Puedes tener tantos como quieras, ahí puedes recibir SCP

```bash
docker exec scprime spc wallet address
```

## Detalles del wallet

```bash
docker exec scprime spc wallet balance
```


# Anunciar el servidor

> Tenemos que tener SCP para realizar esta acción:

```bash
# sudo docker exec scprime spc host announce external_IPv4:4282 

sudo docker exec scprime spc host announce scp1.racs.es:4282

# Si tienes IP fija y no tienes Dynamic DNS
# docker exec scprime spc host announce
```


# Donate SCP

59fc0df34fd88c39b7fdca181196479860cb6891526e8c6d7df70db3c90d8cdf38d956e9317c