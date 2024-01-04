# personal.yli147.test_optee

Compile QEMU
```
git clone https://github.com/yli147/qemu.git -b dev-standalonemm-rpmi
export WORKDIR=`pwd`
cd qemu
./configure --target-list=riscv64-softmmu
make -j $(nproc)
```

Compile OpenSBI

```
cd $WORKDIR
git clone https://github.com/Penglai-Enclave/opensbi.git -b dev-standalonemm-rpmi
cd opensbi
CROSS_COMPILE=riscv64-linux-gnu- make FW_PIC=n PLATFORM=generic
cp build/platform/generic/firmware/fw_dynamic.elf $WORKDIR
```

Compile OPTEE
```
cd $WORKDIR
git clone https://github.com/intel-sandbox/personal.yli147.optee_os.git -b nuclei/3.18_dev
cd optee_os
make CROSS_COMPILE64=riscv64-linux-gnu- ARCH=riscv CFG_RV64_core=y CFG_TZDRAM_START=0x80C00000 CFG_TZDRAM_SIZE=0x800000 CFG_SHMEM_START=0xFEE00000 CFG_SHMEM_SIZE=0x200000 PLATFORM=nuclei ta-targets=ta_rv64 MARCH=rv64imafdc MABI=lp64d
cp out/riscv-plat-nuclei/core/tee-pager_v2.bin $WORKDIR
```

Compile U-Boot
```
cd $WORKDIR
git clone https://github.com/Nuclei-Software/u-boot.git
cd u-boot
git checkout b9162c6c8f30098b09bdf79aa2b40204deed7bfd
cp ../uboot_rv64imafdc_sd_config .config
make CROSS_COMPILE=riscv64-linux-gnu- olddefconfig
make CROSS_COMPILE=riscv64-linux-gnu- all
cp u-boot.bin $WORKDIR
```