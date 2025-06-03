# Instalação do OpenWRT no NanoPi R3S

---

## 📦 Instalação do OpenWRT no NanoPi R3S

### 🔻 1. Baixe a imagem EXT4

Acesse o [OpenWRT Firmware Selector](https://firmware-selector.openwrt.org/) e baixe a imagem `.ext4` compatível com o seu dispositivo.

---

### 💾 2. Gravar imagem no microSD

Use o programa [USBImager](https://gitlab.com/bztsrc/usbimager) (ou similar) para gravar a imagem `.img` no cartão microSD.

- Insira o cartão no dispositivo.
- Inicie o NanoPi R3S com o microSD conectado.

---

### 📥 3. Instalar na memória interna (eMMC)

Após inicializar com o cartão, copie o firmware para a pasta `/tmp` da memória interna via SCP ou outro método. Em seguida, execute os comandos:

```sh
dd if=/tmp/openwrt-24.10.1-rockchip-armv8-friendlyarm_nanopi-r3s-ext4-sysupgrade.img of=/dev/mmcblk0 bs=4M conv=fsync
sync
```
⚠️ Atenção: certifique-se de que /dev/mmcblk0 aponta para a eMMC interna. Use lsblk ou fdisk -l para confirmar.

Depois disso, desligue o dispositivo, remova o cartão SD e ligue novamente. O sistema será iniciado pela eMMC.

### Expandir armazenamento com partição overlay
Por padrão, o OpenWRT usa apenas ~98 MiB do eMMC, o restante permanece não particionado. Para utilizar todo o espaço (ex: 32 GB), siga os passos abaixo para criar e configurar uma partição overlay.

1. Instale os utilitários necessários:
Conecte o NanoPi à internet e execute:
```sh
opkg update
opkg install block-mount blkid fdisk
```

2. Crie uma nova partição:
```sh
fdisk /dev/mmcblk0
```
Digite p para listar as partições.
Digite n para nova partição. Escolha o número 3.
Início: use o próximo bloco disponível (ex: se a partição 2 termina em 344063, inicie com 344064).
Fim: pressione Enter para usar o espaço restante.
Salve com w.

3. Formate a partição:
```sh
mkfs.ext4 /dev/mmcblk0p3
```

4. Copie os dados da overlay atual:
```sh
mkdir /mnt/newroot
mount /dev/mmcblk0p3 /mnt/newroot
mount | grep overlay
tar -C /overlay -cvf - . | tar -C /mnt/newroot -xf -
```

5. Configure o fstab com a UUID:
```sh
block info /dev/mmcblk0p3 | grep UUID
```
Copie a UUID obtida e insira nos comandos abaixo:

```sh
uci -q delete fstab.extroot
uci set fstab.extroot="mount"
uci set fstab.extroot.uuid="COLOQUE-A-UUID-AQUI"
uci set fstab.extroot.target="/overlay"
uci set fstab.extroot.enabled="1"
uci commit fstab
```

6. Finalize e reinicie:
```sh
umount /mnt/newroot
reboot
```