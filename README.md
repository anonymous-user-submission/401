# Prototype of Hedwig software and hardware

## Dependencies

We use DPDK v21.11 in Hedwig software, and use Vivado v2020.2 to synthesize the Hedwig hardware.

## Software

Check the following commands to build the Hedwig software.
```bash
# install DPDK v21.11 library
cd software
mkdir build
cd build
cmake ..
make -j
```

## Hardware

There are three parts to build Hedwig hardware: (1) patch hardware code based on Corundum; (2) patch configs for the AU250 board; (3) get a CAM implementation, where we use a [open-source implementation](https://github.com/alexforencich/verilog-cam) and fix a subtle bug there.

```bash
# patch hardware code
git clone https://github.com/corundum/corundum.git hw-prototype
cd hw-prototype
git checkout 56fe10f27d9b42f1ff9abe4d735b113008e4be9d
cd fpga/common/rtl
patch -i ~/401/hardware/src-code.patch -p2
# copy the CAM implementation
cd -
cp -r ~/401/hardware/verilog-cam fpga/lib
# patch AU250 configs
cd fpga/mqnic/AU250/fpga_100g
patch -i ~/401/hardware/config.patch -p2
# build
make all
```

