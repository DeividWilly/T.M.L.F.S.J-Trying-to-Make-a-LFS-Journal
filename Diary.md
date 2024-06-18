# T.M.L.F.S.J-Trying-to-Make-a-LFS-Journal
Diary of my journey trying to create an [LFS](https://linuxfromscratch.org/lfs/view/stable/index.html) from the book and my minimal knowledge :)

# Tabela de referências

| Link | Descrição |
| ---- | --------- |
| [Linux From Scratch](https://linuxfromscratch.org) | Livro/Guia para construr Linux do zero |
| [Debian](https://www.debian.org/index.pt.html) | Sistema Linux que será usado em ambiente virtual para construção do nosso sistema |


## Dia 1

Já estou há alguns dias lendo em inglês devagar, para compreender melhor as instruções e explicações.
O [capítulo 1](https://linuxfromscratch.org/lfs/view/stable/chapter01/how.html) não é de muita importância, apenas as sujestões dede distribuições Linux que podemos usar para construir o LFS. Apartir do capítulo 2 somos orientados sobre o esquema de partições e tipos de "file systems" podemo utilizar. Escolhi a versão mais moderna da família ``ext``: [``ext4``](https://wikipedia.org/wiki/Ext4). 
Usando Debian em uma máquina virtual com disco de 50GBs.
```shell
makelfs@debian:~$ sudo fdisk -l

Disk /dev/vda: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 99ADD031-FE7F-4E1B-9D5B-BCE537802A41

Device        Start       End  Sectors  Size Type
/dev/vda1      2048    976895   974848  476M EFI System
/dev/vda2    976896  40038399 39061504 18.6G Linux filesystem
/dev/vda3  40038400  55662591 15624192  7.5G Linux swap
/dev/vda4  55662592 104855551 49192960 23.5G Linux filesystem
```
A partição dedicado ao LFS é ``/dev/vda4`` enquanto a partição de boot será a mesma usada no sistema principal (no caso da máquina virtual).
Configurei 4 núcleos lógicos e 12GBs de RAM dedicadas para a máquina virtual, mais 8GBs de swap. Desconfio que não será o suficiente, talvez seja necessário aumentar a quantidade núcleos lógicos se for possível no virt-manager.

Agora somos orientados a baixar uma grande [lista de tarballs e patches](https://gist.githubusercontent.com/SirMegaMU/317f4f438e811945a6a5423ecf01d616/raw/1726c6cadcdab448f5531851a7acdf65aa05ad29/wget-list-sysv) necessários para o funcionamento básico de um sistema Linux:
```shell
acl-2.3.2.tar.xz
attr-2.5.2.tar.gz
autoconf-2.72.tar.xz
automake-1.16.5.tar.xz
bash-5.2.21.tar.gz
bash-5.2.21-upstream_fixes-1.patch
bc-6.7.5.tar.xz
binutils-2.42.tar.xz
bison-3.8.2.tar.xz
bzip2-1.0.8-install_docs-1.patch
bzip2-1.0.8.tar.gz
check-0.15.2.tar.gz
check.sh
coreutils-9.4-i18n-1.patch
coreutils-9.4.tar.xz
dbus-1.14.10.tar.xz
dejagnu-1.6.3.tar.gz
diffutils-3.10.tar.xz
e2fsprogs-1.47.0.tar.gz
elfutils-0.190.tar.bz2
expat-2.6.0.tar.xz
expect5.45.4.tar.gz
extra.sh
file-5.45.tar.gz
findutils-4.9.0.tar.xz
flex-2.6.4.tar.gz
flit_core-3.9.0.tar.gz
gawk-5.3.0.tar.xz
gcc-13.2.0.tar.xz
gdbm-1.23.tar.gz
gettext-0.22.4.tar.xz
glibc-2.39-fhs-1.patch
glibc-2.39.tar.xz
gmp-6.3.0.tar.xz
gperf-3.1.tar.gz
grep-3.11.tar.xz
groff-1.23.0.tar.gz
grub-2.12.tar.xz
gzip-1.13.tar.xz
iana-etc-20240125.tar.gz
inetutils-2.5.tar.xz
intltool-0.51.0.tar.gz
iproute2-6.7.0.tar.xz
Jinja2-3.1.3.tar.gz
kbd-2.6.4-backspace-1.patch
kbd-2.6.4.tar.xz
kmod-31.tar.xz
less-643.tar.gz
lfs-bootscripts-20230728.tar.xz
libcap-2.69.tar.xz
libffi-3.4.4.tar.gz
libpipeline-1.5.7.tar.gz
libtool-2.4.7.tar.xz
libxcrypt-4.4.36.tar.xz
linux-6.7.4.tar.xz
m4-1.4.19.tar.xz
make-4.4.1.tar.gz
man-db-2.12.0.tar.xz
man-pages-6.06.tar.xz
MarkupSafe-2.1.5.tar.gz
md5sums
meson-1.3.2.tar.gz
mpc-1.3.1.tar.gz
mpfr-4.2.1.tar.xz
ncurses-6.4-20230520.tar.xz
ninja-1.11.1.tar.gz
openssl-3.2.1.tar.gz
patch-2.7.6.tar.xz
perl-5.38.2.tar.xz
pkgconf-2.1.1.tar.xz
procps-ng-4.0.4.tar.xz
psmisc-23.6.tar.xz
python-3.12.2-docs-html.tar.bz2
Python-3.12.2.tar.xz
readline-8.2.tar.gz
readline-8.2-upstream_fixes-3.patch
sed-4.9.tar.xz
setuptools-69.1.0.tar.gz
shadow-4.14.5.tar.xz
sysklogd-1.5.1.tar.gz
systemd-255.tar.gz
systemd-255-upstream_fixes-1.patch
systemd-man-pages-255.tar.xz
sysvinit-3.08-consolidated-1.patch
sysvinit-3.08.tar.xz
tar-1.35.tar.xz
tcl8.6.13-html.tar.gz
tcl8.6.13-src.tar.gz
texinfo-7.1.tar.xz
tzdata2024a.tar.gz
udev-lfs-20230818.tar.xz
util-linux-2.39.3.tar.xz
vim-9.1.0041.tar.gz
wheel-0.42.0.tar.gz
XML-Parser-2.47.tar.gz
xz-5.4.6.tar.xz
zlib-1.3.1.tar.gz
zstd-1.5.5.tar.gz
```

No capítulo 4 criaremos este script:
```shell
mkdir -pv $LFS/{etc,var} $LFS/usr/{bin,lib,sbin} # Cria diretórios separados

for i in bin lib sbin; do
	ln -sv usr/$i $LFS/$i # Supunho pelo conhecimento básico que este loop faz um link em cada diretório.
done

case $(uname -m) in
	x86_64) mkdir -pv $pv $LFS/lib64 ;; # Verifica a arquitetura e cria o diretório correto
esac
```
Na página do capítulo 4.2, há uma observação que diz não deve ser utilizado o diretório ``usr/lib64`` por motivos não explicados, mas caso exista pode ocorrer alguma quebra no sistema.
[Leia aqui.](https://linuxfromscratch.org/lfs/view/stable/chapter04/creatingminlayout.html)

## Dia 2

Criamos um usuário dedicado para a construção do sistema e fazemos algumas configurações para o user:

```shell
.bashrc

set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/usr/bin
if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
PATH=$LFS/tools/bin:$PATH
CONFIG_SITE=$LFS/usr/share/config.site
MAKEFLAGS=-j$(nproc)
export LFS LC_ALL LFS_TGT PATH CONFIG_SITE MAKEFLAGS
```

```shell
.bash_profile

exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
```
