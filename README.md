# Instalação do OPENWRT

Baixe a imagem EXT4 de acordo com o modelo: https://firmware-selector.openwrt.org/

Inicie o OpenWRT pel Cartão SD.

Instalando no MicroSD:
Use o programa USBImager write *_squashfs-sysupgrade.img.gz to microSD
insert microSD an boot nanopi R3S

Coloque o firmware na pasta /tmp da memória interna do nanopi e execute:
```sh
dd if=/tmp/openwrt-24.10.1-rockchip-armv8-friendlyarm_nanopi-r3s-ext4-sysupgrade.img of=/dev/mmcblk0 bs=4M conv=fsync
````
```sh
sync
```

Após isso, desligue, remova o cartão sd e inicie o nanopi.

---

## Montar partição Overlay

Por padrão o OpenWRT irá utilizar apenas os 98 Mib de armazenamento e o restante nao será particionado.

Para fazer com o que o OpenWRT reconheça todos os 32 gb eMCC será necessário criar uma partição overlay:

Conecte o NanoPi a internet e faça:
```sh
opkg update
```
```sh
opkg install block-mount blkid fdisk
```
```sh
fdisk /dev/mmcblk0
```
Aqui você irá digitar 'p' para ver as partições.
Digite 'n' para criar uma partição nova, com o valor '3'.
O início do bloco deverá ser seguido da partição 2, por exemplo, se a partição 2 está terminando com 344063, a partição 3 deve iniciar com 344064.
O final pode dexar vazio que irá pegar todo espaço disponível.
```sh
mkfs.ext4 /dev/mmcblk0p3
```
Esse comando irá formatar a partição.

Agora pode seguir com os passos:
```sh
mkdir /mnt/newroot
```
```sh
mount /dev/mmcblk0p3 /mnt/newroot
```
---

## Comando para instalação

Execute o comando abaixo no terminal do seu roteador para baixar e instalar o script:

```sh
cd / && wget -O passwall-install.sh https://raw.githubusercontent.com/fleetvpngit/PASSWALL/refs/heads/main/passwall-install.sh && chmod +x passwall-install.sh && sh passwall-install.sh
