+--------------------------+
 riscv32-toolchain v-1.0.0
+--------------------------+

���ʹ��
--------
ִ�� extract-gcc.sh ��ѹ�� riscv32 gcc ������
ִ�� source envsetup.sh ���û�������


��α���� riscv32-unknown-elf-gcc
----------------------------------
git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
./configure --prefix=$PWD/install --with-arch=rv32imac --with-abi=ilp32 --disable-gdb
make


��α���� riscv32-picolibc
---------------------------
��װ pip3 �� meson
git clone https://github.com/picolibc/picolibc

vim do-riscv32-configure
#!/bin/sh
ARCH=riscv32-unknown-elf
DIR=`dirname $0`
meson "$DIR" \
    -Dincludedir=picolibc/$ARCH/include \
    -Dlibdir=picolibc/$ARCH/lib \
    --cross-file "$DIR"/cross-$ARCH.txt \
    "$@"

vim cross-riscv32-unknown-elf.txt
[binaries]
c = 'riscv32-unknown-elf-gcc'
ar = 'riscv32-unknown-elf-ar'
as = 'riscv32-unknown-elf-as'
strip = 'riscv32-unknown-elf-strip'
exe_wrapper = ['sh', '-c', 'test -z "$MESON_SOURCE_ROOT" || "$MESON_SOURCE_ROOT"/run-rv32imac "$@"', 'run-rv32imac']

[host_machine]
system = 'unknown'
cpu_family = 'riscv'
cpu = 'riscv32'
endian = 'little'

[properties]
c_args = [ '-nostdlib', '-msave-restore', '-march=rv32imac', '-mabi=ilp32']
needs_exe_wrapper = true
skip_sanity_check = true

mkdir build-riscv32-unknown-elf
cd build-riscv32-unknown-elf
../do-riscv32-configure --prefix=$PWD/install
ninja
ninja install

�޸� picolibc.specs �ļ����滻����·��Ϊ %R �� sysroot


rockcarry
2020-10-27

