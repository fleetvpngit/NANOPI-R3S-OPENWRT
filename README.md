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
Este script instala o **PASSWALL** e o **XRAY-CORE** no OpenWrt.

Devido ao armazenamento insuficiente, a instalação do Xray-Core é feita na memória temporária do roteador, ou seja, ao reiniciar é necessário inserir novamente o arquivo xray na pasta /tmp.

---

## Detalhes do ambiente compatível

- **Versão do OpenWrt:** 22.03.05  
- **Modelo do roteador:** Xiaomi MI 4 A Gigabit  
- **Espaço de armazenamento necessário:** 8 MB+  
- **Memória RAM necessária:** 128 MB+

---

## Comando para instalação

Execute o comando abaixo no terminal do seu roteador para baixar e instalar o script:

```sh
cd / && wget -O passwall-install.sh https://raw.githubusercontent.com/fleetvpngit/PASSWALL/refs/heads/main/passwall-install.sh && chmod +x passwall-install.sh && sh passwall-install.sh
