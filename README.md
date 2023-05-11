# VSD-HDP
## Day 0
### Tools installed:
<details>
<summary>Yosys</summary>

Installed it following these steps:

```
git clone https://github.com/YosysHQ/yosys.git
cd yosys-master 
sudo apt install make (If make is not installed please install it) 
sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
make 
sudo make install
```
![yosys snap](https://github.com/walaa-amer/VSD-HDP/assets/85279771/c194bd80-f969-4c46-a8a5-32bd614fe93b)

</details>

<details>
<summary>OpenSTA</summary>

First installed the packages needed using:

```
sudo apt-get install cmake clang gcctcl swig bison flex
```
Then installed and built OpenSTA using:
```
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make
```
![opensta snap](https://github.com/walaa-amer/VSD-HDP/assets/85279771/5aaab38c-074b-4553-aa74-190414f0098b)


</details>

<details>
<summary>ngspice</summary>

Downloaded the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory and unpacked it using:

```
tar -zxvf ngspice-37.tar.gz
cd ngspice-37
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install

```
![ngspice snap](https://github.com/walaa-amer/VSD-HDP/assets/85279771/62b233aa-e5ad-43d9-a471-2b1d59bd1378)

</details>

<details>
<summary>iverilog</summary>

Installed it using:

```
sudo apt-get install iverilog

```

</details>

<details>
<summary>gtkwave</summary>

Installed it using:

```
sudo apt-get install gtkwave

```

</details>

<details>
<summary>magic</summary>

Installed it following these steps:

```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev


```

</details>

## Day 1
<details>
<summary>Simulation using iverilog and visualization using gtkwave:</summary>
Command:
    
```
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
    
![lab2 iverilog](https://github.com/walaa-amer/VSD-HDP/assets/85279771/49f31aad-81f8-46f6-88c8-d954f65f992e)

Results:

![lab2 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/07dad12f-d621-45c0-970a-01613311322f)
</details>
 <details>
<summary>Synthesis using yosys:</summary>

Reading input Verilog and lib files on the yosys prompt:
    
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

Results:
     
![lab2 yosys 2](https://github.com/walaa-amer/VSD-HDP/assets/85279771/6baa3704-14f0-4f93-8a5a-49f9fb7bf44e)
</details>
<details>
<summary>Generating the netlist on the yosys prompt:</summary>
    
```
write_verilog -noattr good_mux_netlist.v
!vim good_mux_netlist.v
```

Results:
    
![lab2 yosys 3](https://github.com/walaa-amer/VSD-HDP/assets/85279771/5c31de61-2caa-477d-9b16-a3760bbfde73)

</details>
