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
<summary>Simulation using iverilog and visualization using gtkwave</summary>
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
<summary>Synthesis using yosys</summary>

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

Generating the netlist on the yosys prompt:
    
```
write_verilog -noattr good_mux_netlist.v
!vim good_mux_netlist.v
```

Results:
    
![lab2 yosys 3](https://github.com/walaa-amer/VSD-HDP/assets/85279771/5c31de61-2caa-477d-9b16-a3760bbfde73)

</details>

## Day 2

 <details>
<summary>Synthesis of multiple modules</summary>
    
Commands:
    
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr multiple_modules_netlist.v
!vim multiple_modules_netlist.v
    
 ```
    ![lab 5  synth commands](https://github.com/walaa-amer/VSD-HDP/assets/85279771/f9595922-a9f1-412d-abc8-699641b1417c)

 Results:
![lab 5 submodule show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/a9f02479-9eba-4b2b-b93b-772a960b05d3)

Hierarchy:
![lab 5 module hier](https://github.com/walaa-amer/VSD-HDP/assets/85279771/2884cd34-ef5b-4036-92e1-4234089838e8)
    
</details>
 <details>
<summary>Synthesis of flattened modules</summary>
    
Flattening the modules will replace the submodules in the netlist by the actual gates being used directly into the netlist.
    
Commands:
```
write_verilog -noattr multiple_modules_flat.v
!vim multiple_modules_flat.v
```

Results:
    
![lab 5 flattened submodules](https://github.com/walaa-amer/VSD-HDP/assets/85279771/8df3b0f0-30f4-4ecf-85a4-1202eb74edd6)
    
Hierarchy:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
```
![lab 5 flattened submodules show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/df11cbd7-d484-4499-8732-eed3c184b7b3)

</details>
    
<details>
<summary>Submodule-level synthesis</summary>
This type of synthesis is useful when you have multiple instances of the same submodule, so it would be useful to su=ynthesize it once and replicate its netlist as many times as needed. It is also useful when using the divide-and-conquer approach for massive designs.
    
Commands:
    
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Results:
![lab 5 submodule synth results](https://github.com/walaa-amer/VSD-HDP/assets/85279771/185a2e9f-b459-427e-834d-f9f04b97fd6f)
Hierarchy:
![lab 5 submodule show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/8e3696d5-5a18-4cb3-9174-2b41c1c303f3)

</details>

<details>
<summary>FLOPS</summary>
FLOPS are memory components used to eliminate glitching effect between combinational circuits.


iverilog simulation commands for asynchronous reset:

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

Results:
    ![lab 5 l3 flop iverilog simulation gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e7a487cf-97cd-46d5-a95b-cd1f2b0066ea)
A few remarquable points seen in this simulation:
    - In this instance in time, Q goes high when the clock goes high:
    ![lab 5 l3 flop iverilog simulation asyncres](https://github.com/walaa-amer/VSD-HDP/assets/85279771/08e855d1-0608-47ba-b07e-e9cdfb029146)

    - In this instance in time, Q goes low once the asynchronous reset goes to high:
    ![lab 5 l3 flop iverilog simulation asyncres 2](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e8a8db56-30e2-4de6-b1ef-fb89b1d5deeb)

iverilog simulation commands for asynchronous set:

```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

Results:
    
![lab 5 l3 flop iverilog simulation gtkwave async set](https://github.com/walaa-amer/VSD-HDP/assets/85279771/43b70c92-c491-4c8a-acae-afe0400b1c1c)

A few remarquable points seen in this simulation:
    - In this instance in time, Q goes high when the clock goes high once async set is set to low:
    ![lab 5 l3 flop iverilog simulation asyncset](https://github.com/walaa-amer/VSD-HDP/assets/85279771/33f4a417-f33b-4167-9fa9-62557402b130)

    - In this instance in time, Q goes high once the asynchronous set goes to high:
    ![lab 5 l3 flop iverilog simulation asyncset 2](https://github.com/walaa-amer/VSD-HDP/assets/85279771/5f061a26-6a43-4eba-86be-503c41fc608e)
  
    
iverilog simulation commands for synchronous reset:

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

Results:
    

![lab 5 l3 flop iverilog simulation gtkwave sync reset](https://github.com/walaa-amer/VSD-HDP/assets/85279771/10a4516c-ac6d-412a-b646-28744ccdacab)


A few remarquable points seen in this simulation:
    - In this instance in time, Q goes low only when the clock goes high when sync reset is set to high:
    ![lab 5 l3 flop iverilog simulation asyncset](https://github.com/walaa-amer/VSD-HDP/assets/85279771/33f4a417-f33b-4167-9fa9-62557402b130)

    
    
</details>
