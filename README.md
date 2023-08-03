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

<details>
<summary>iverilog simulation commands for asynchronous reset:</summary>

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

</details>
<details>
<summary>iverilog simulation commands for asynchronous set:</summary>

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
</details>  
<details>
<summary>iverilog simulation commands for synchronous reset:</summary>    


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
<details>
<summary>Synthesis of asynchronous reset</summary>  

Using the following commands:
    
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog dff_asyncres.v
    synth -top dff_asyncres
    dfflibmap ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
    
Hierarchy:
Inverter added for active high reset of the DFF
    ![l4 dff show async res](https://github.com/walaa-amer/VSD-HDP/assets/85279771/a6f234aa-b1ad-411a-89bc-fd4f6878daa6)

</details>    
    
<details>
<summary>Synthesis of asynchronous set</summary>  

Using the following commands:
    
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog dff_async_set.v
    synth -top dff_async_set
    dfflibmap ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
    
Hierarchy:
Inverter added for active high reset of the DFF
    ![l4 dff show async set](https://github.com/walaa-amer/VSD-HDP/assets/85279771/bc096373-9e72-49db-9525-576ee65d3c3b)


</details>    
    
<details>
<summary>Synthesis of synchronous set</summary>  

Using the following commands:
    
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog dff_sync_set.v
    synth -top dff_sync_set
    dfflibmap ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
    
Hierarchy:
The AND gate with an inverter before the input replaces a 2-1 mux controller by sync reset
   ![l4 dff show sync res](https://github.com/walaa-amer/VSD-HDP/assets/85279771/31548646-d811-404e-bd45-a7b4acd73c06)


</details>  
    
<details>
<summary>Optimization of multiplication</summary> 

Multiplication by 2 is basically the input appended by '0' at the end.

Using the following commands:
    
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog mult_2.v
    synth -top mul2
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
Running abc is not necessary since there are no cells in the design.
    
    
Hierarchy:
![l4 mul2 synth](https://github.com/walaa-amer/VSD-HDP/assets/85279771/d9119ac1-fc18-4fcb-96e9-d033de8acd73)

    
Thus, a special case where this optimization i useful is when multiplying by 8:
    y = a x 9 = a x (8 + 1) = a x 8 + a x 1
Which means that is a appended by 3 '0's is added with the 3-bit input itself, so the last 3 bits of y are connected to the input and the last 3 bits of y are also connected to a.
    
    The synthesis hierarchy results and the verilog netlist code show that:
![l4 mul8 synth](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e1328216-38f0-4aea-8043-c04ee5152545)
![l4 mul8 netlist verilog result](https://github.com/walaa-amer/VSD-HDP/assets/85279771/654cec55-fffc-42b8-b175-0d9753fb08ab)

    
</details>  
</details>

## Day 3
<details>
<summary>Combinational and sequential optimizations</summary> 
Combinational: Simplify the logical expressions to use less gates
Sequential: Sequential constants where signals hold a constant for any input change can help optimize a circuit.
Advanced optimization methods: State optimization, Cloning, Retiming
</details>

<details>
<summary>Combinational Logic Optimizations</summary>
The opt_check.v code is a logic expression that represents a 2-1 mux. 
    
![d3 l6 opt_check code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/c8405f70-9433-472c-b8b0-90931994e145)

This expression can be reduced to an AND gate since a.b + a'.0 = a.b. To achieve this optimization, we run the synthesis using the following commands:
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog opt_check.v
    synth -top opt_check
    opt_clean -purge //to remoce unused components of the design
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
Hierarchy:
We can see that it reduced the design to an AND gate.
    
![d3 l6 opt_check show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/1e5854fb-a4e2-43e2-8331-62c01491404b)
    
The opt_check2.v code is also a logic expression that represents a 2-1 mux. 
    
![d3 l6 opt_check2 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/4b2a81d2-7053-403f-a0c4-35a0ec17ae8a)
    
This expression can be reduced to an OR gate since a.1 + a'.b = a + a'.b = a + b. To achieve this optimization, we run the synthesis using the following commands:
    ```
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog opt_check2.v
    synth -top opt_check2
    opt_clean -purge //to remoce unused components of the design
    abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    ```
Hierarchy:
We can see that it reduced the design to an OR gate.
    
![d3 l6 opt_check2 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/4c3d50eb-d74b-4fd9-9ee5-3ecb7fbc9eab)

</details>

<details>
<summary>Sequential Logic Optimizations</summary>
<details>
<summary>dff_const1</summary> 
In dff_const1.v, if RST goes from high to low, Q will ot take the value of D immediately, but will wait for the next clock rising edge -> the circuit cannot be reduced to an inverter of RST.
    
![d3 l6 dff_const1 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/cf883c44-558d-4c95-afda-9ab864b671d3)


We can see that in the following gtkwave snapshot:

![d3 l6 dff_const1 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/de3e6b01-5dd9-486c-8d92-4a2b774b7791)

Synthesis:
    
 ```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Hierarchy:
![d3 l6 dff_const1 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/63494649-c767-4bd7-a287-c4f4cb888380)

</details>
<details>
<summary>dff_const2</summary> 
    
However in dff_const2.v, Q will be high in any case, so it can be optimized. 
    
![d3 l6 dff_const2 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/18913cf3-38f7-44a3-bd0e-4794d352bb73)
    
We can see that in the following gtkwave snapshot:

![d3 l6 dff_const2 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e7cfa58a-72a3-4c58-bbd9-db93be710872)

We need 0 FLOPs/cells.
Synthesis:
    
 ```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Hierarchy:
![d3 l6 dff_const2 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/0f86fa3a-6a38-413a-9f71-fb7a908aaa70)
</details>
<details>
<summary>dff_const3</summary> 
In dff_const3.v, we have 2 consecutive FFs where when the first one changes the output value, the second one picks up the old value as its input at the rising clock edge (becuase of delay), and will pick up the new value one clock cycle after.

![d3 l6 dff_const3 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/46f524bc-f334-40ae-8ff8-08fd46d7af3d)


And we can see that in the simulation result:
![d3 l6 dff_const3 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/6fe89243-d8aa-477b-8696-1cd4455219a2)

Synthesis:
    
 ```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Hierarchy:
![d3 l6 dff_const3 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/d0753939-aebc-431b-b335-77f9e40fd6eb)
</details>
<details>
<summary>dff_const4</summary> 
In dff_const4.v, Q will be high whatever the input was, so is Q1.
    
![d3 l6 dff_const4 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/0b9a6da2-8d27-4460-9c07-829b22ded9db)


And we can see that in the simulation result:
![d3 l6 dff_const4 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/23d0b7c9-b59b-4443-82d2-d6eb085357ba)


Synthesis:
    
 ```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Hierarchy:
![d3 l6 dff_const4 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/81a44e30-6ef6-4e3e-b39e-9de1da12fc2f)
    
</details>
<details>
<summary>dff_const5</summary>     
In dff_const5.v, we have 2 consecutive FFs where when the first one changes the output value, the second one picks up the old value as its input at the rising clock edge (becuase of delay), and will pick up the new value one clock cycle after.
    
![d3 l6 dff_const5 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/09903b06-85c3-4c3b-afd5-cc8cc409ae6d)


And we can see that in the simulation result:
![d3 l6 dff_const5 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/ad0b8f50-6859-4f8d-94ca-fed21c49b867)


Synthesis:
    
 ```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
    
Hierarchy:
![d3 l6 dff_const5 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/63658903-bcf1-40b6-814f-74e9d38f995a)
    
</details> 
</details>


<details>
<summary>Sequential Unused Output Optimization</summary> 
<details>
<summary>counter_opt</summary> 
In the counter verilog code, the first 2 bits of the counter output are not used, as shown in the code below:
    
![d3 l6 counter_opt code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/6678a25a-e921-4260-8fae-57a61be2ee7e)
    
Synthesis:
    
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
      
Hierarchy:
    
![d3 l6 counter_opt show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/3612caee-fcf6-4790-93df-558dc161265b)

    
We see that the output of the synthesis uses only 1 DFF as an optimization since the first 2 bits of the counter are not being used.
</details>
    
    
<details>
<summary>counter_opt2</summary> 
We modify the counter verilog code to use all of the bits of count, as shown in the code below:
    
![d3 l6 counter_opt2 code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/8dc5c3e7-10cc-4a99-b18e-abc381e0cb2a)
    
Synthesis:
    
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
      
Hierarchy:
    
![d3 l6 counter_opt2 show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/02c62eed-cabc-40d5-9a3c-36f2aab3ee6e)

    
We see that the output of the synthesis uses 3 DFFs since we need all of the output bits .
    
</details>
</details>

## Day 4

<details>
<summary>GLS</summary> 
Running the netlix as the DUT with the same test bench as the one for RTL design using iverilog   . This helps us ensure that the timing of the design is met (alogside testing of the functionality) if the gate-level verilog model is timing-aware.
</details>

<details>
<summary>Synthesis Simulation Mismatch</summary> 
<details>
<summary>Missing Sensitivity List</summary> 
The simulator will look for activity (change in inputs) to evaluate the output. More specifically, it will look at a change in the sensitivity list. If inputs are not included in the sensitivity list, the simulator will not evaluate the output for changes in these variables. This will cause a mismatch between the design simulation in RTL and in the netlist. 
In verilog the syntax always(*) is used to indicate evaluation at any change.
    
Testing on a ternary mux:
![d4 l1 ternary_mux code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/cd58dba3-d8b9-46b1-9d51-dac3c24309f6)
    
Simulating this design would result in the follwing wave showing that y=i0 when sel=0 and y=i1 when sel=1:
![d4 l1 ternary_mux gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/0eaac8e7-fc53-4d8c-96bb-3e2a12a766a6)
  
Hierarchy:
![d4 l1 ternary_mux show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/d39b4355-6bdc-4516-ad42-bbf40ef4ce16)

After the synthesis, we write the output to a netlist verilog file and simulate that file using iverilog with the following command:
    
```
iverilog ../my_lib/verilog_models/primitives.v ../my_lib/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```
    
Visualizing the results using gtkwave:
![d4 l1 ternary_mux_net gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/bc9ce6ab-ac09-4035-9b3b-18d34d411e91)

Now looking at bad_mux.v, we se that sensitivity list only includes the select line, which means the simulator will only record changes in the output when sel is high and disregard any change in the inputs.
![d4 l1 bad_mux code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/96cf450b-6554-4222-88b0-ddc3a1ef2a87)


Visualizing on gtkwave shows that the design acts like flop more than a mux:

![d4 l1 bad_mux gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/3e5e35f5-d34d-4bf4-8cff-8afd9b6ce449)

However after synthesis and GSL simulation on the netlist, we notice that the behavior of the design is different, as the inputs are reflected on the output, anbd not just the select line, meaning that there is a mismatch is the simulations before and after synthesis because of missing sensitivity list:
    
![d4 l1 bad_mux_net gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/94a20691-4781-45c1-ac30-9ae58ea2be56)


</details>
    
<details>
<summary>Blocking Statements</summary> 
Inside the always block, some statements are blocking (execute sequentially in order of writing) and some are non-blocking (execute oin parallel). Using blocking statements instead of non-blocking would sometimes cause issues in the design. Thus we must use non-blocking statements when dealing with sequential circuits.
Example:
For a 2 consecutive DFFs design:
    
```
q0=d;
q=q0;
```
    
will lead to a 1 DFF design cause by the time q is evaluated, q0 already has the value of d.
VS
    
```
q0<=d;
q<=q0;
```
    
where the order of evaluation does not matter and the sequence in the circuit is conserved.
    
Testing this on blocking_caveat.v:

    
In this code, we are using x in the first statement, and then updating it in the second statement. So we're using the past value of x as if there is a flop saving the value of x for the next clock cycle. 
This behavior shows in the wave visualized by gtkwave:
    
![d4 l1 blocking_caveat gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/8f92e8a5-fbcc-4f24-ae41-39ad42cdecff)
    
After synthesis, we can see that there is no flop in the hierarchy:
![d4 l1 blocking_caveat show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/eee2220f-6fb2-42f2-82f4-2c3dc50e6c9b)

Visualizing the wave after synthesis shows a mismatch in the results with the RTL design:
![d4 l1 blocking_caveat2 gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e990763e-0fa0-439f-9762-4bdab5233a41)
  
In this wave, it is as if the statements were switched, and no flop is needed.


</details>
</details>

## Day 5

<details>
<summary>If case construct</summary> 
<details>
<summary>Incomplete if</summary>  
In an if-else statement, priority is given to if conditions in the order they were written in. We should be careful not to keep the if-statement incomplete (without else statement) since the tool will then add an inferred latch to conserve the value of he output in case all the if consitions were not met and there is no else. Sometimes in combinational circuits, inferred latches are okay since latching the output is needed part of the design.
Example counter:
    ```
    always @ (posedge clk, posedge reset)
    begin
        if (reset)
            count <= 3'b000;
        else if (en)
            count <= count+1;
     end
     ```
In this example, there is an incomplete if. However, when reset =0 and en=0, count should keep itsold value, which means a latch should be used to do that. Inferred latches provide this design, which in this case are convenient. 
In the incomp_if.v example shows a mux design where only 1 case for the select shows:
                
![d5 l1 incompif code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/c778d9e5-6acd-40ca-95d2-2998eed9bc22)

This incomplete if will infer a latch since the output will hold its value fori0=0, which is what we see in the simulation:
                
![d5 l1 incompif gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/f6f8f10a-e37c-4630-9012-7f7529357bf9)

The synthesis shows a latch instead of a mux:
![d5 l1 incompif show](https://github.com/walaa-amer/VSD-HDP/assets/85279771/a30f0752-4445-4811-b5a5-dbd3c2603df7)
</details>

<details>
<summary>Case caveat</summary> 
Case statements can also be incomplete and create inferred latches. The solution to this is to code cae with default case. This default will act as an else and will avoid inferred latches.
                
Another problem for cases is partial assignment, is when you do not assign a value for some of the outputs, leading to create an inferred latch for those outputs.
                
Another problem is overlapping cases. When 2 cases match an input it might cause unpredictable output.
                
</details>
</details>
                
<details>
<summary>Looping constructs</summary>

<details>
<summary>For loop</summary>
The for construct is used inside of the 'always' block. It is used for evaluating expressions multiple times. For example, if we need to write a 32:1 mux, we need to iterate over all 32 conditions to pass the correct output.

Example:
```
integer i;
always@(*)
begin
    for(i=0; i<32; i=i+1)
        begin
        if (i==sel)
            y=inp[i];
    end
end
```
If we want to write the same code for 256:1 mux, we simply change the consition in the for loop only. This code writing style provides a concise and simple code that is easy to scale.

Another example: 1:8 demux
```
integer i;
always@(*)
begin
    op_bus[7:0] = 8'b0;
    for (i=0; i<8; i=i+1)
    begin
    if (i==sel)
        op_bus[i] = input;
    end
end
```
When the for loop runs, it evaluates the condition 8 times. At one point, the condition will match for 1 value and the output of that bit will be changed.

Applying this concept to the mux_generate.v:

![d5 l1 forloop mux code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/22607621-d358-4789-8d0b-a9c32aac6ba8)


As shown in the code above, there are 4 inputs being lumped into 1 bus 'i_int'. The for loop is used inside the always block to evaluate the 'sel' value 4 times and pass the input to the output wherever the sel condition is satisfied.


The simulatiom results show that the output follows the input depending on the select line value as it is supposed to do.
![d5 l1 forloop mux gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/5a86646f-8245-44bd-8d50-8451ec79f52e)


The simulation of the synthesized design also show the same results:
//fix error in netlist

Another example is the demux where we compare the code written without a for loop and with a for loop:

![d5 l1 forloop demuxcase gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/87d57bad-ec8d-408c-8dd4-4e2c80332f7d)
    
![d5 l1 forloop demuxgen code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/c49cc642-d95e-41df-b9ed-2f770c76b0da)

    
First the output is initialized to 0. Using a case statement, we check the select line for all the values possible and pass the input to the output in only 1 case. If we want to scale to a 1:256, we're going to need 256 lines of code for the case statement, that we can replace with a few lines using the for loop. 


The results of the 2 simulated designs deliver the same results of a demux shown below:

![d5 l1 forloop demuxcase gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/be082913-e8d8-431e-b9b3-69f29ca20faf)

![d5 l1 forloop demuxgen gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/9cad4f30-8098-4e22-a213-db8c6286d919)

The simulation of the synthesized designs also show the same results:
//fix error in netlist
</details>

<details>
<summary>For generate</summary>
The generate for loop is used outside of the 'always' block and cannot be used isnide of 'always'. It is used for instantiating hardware multiple times/ replication of hardware.
Example: instantiate an AND gate 500 times.
    
Instead of writing:
    
```
and u_and1(.a(); .b(); .y());
and u_and2(.a(); .b(); .y());
    ...
and u_and500(.a(); .b(); .y());
```
    
We use for generate:
    
```
genvar i;
generate
    for (i=0; i<8; i=i+1)
       begin
            and u_and(.a(in1[i]); .b(in2[i]); .y(y[i]));
       end
endgenerate
```
The AND gate is replicated 8 times, and each output of each gate is 1 bit of the final output.

Replication of harwdare is clearly needed in design like the ripple-carry adders where we need the same piece of hardware several times to create that ripple effect.

 Applying this to rca.v:
 ![d5 l1 forgen rca code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/afda53b2-fe9b-4209-80a9-675702c34d96)

 
We can see that since the inputs are 8-bit long, the output will be 9 bits. Firet we instantiate the full adder 8 times and then the connections are made for the ripple effect.

The variable used for the for generate is 'genvar' and not an integer. The simulation shows the expected result of the RCA:

![d5 l1 rca gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/de91e092-dcb9-4de4-a4d6-850a1bf54d12)

The simulation of the synthesized designs also show the same results:
//fix error in netlist


</details>
</details>
</details>

## Day 6

<details>
<summary>Simulation/synthesis of my design</summary>

The design that I chose is a johnson counter, which is a type of ring counters. Please find the code in the johnson_counter.v file.
Simulating this design results in the following:
![d6 johnson gtkwave](https://github.com/walaa-amer/VSD-HDP/assets/85279771/66e56fd2-664f-4a8d-8508-4396604e8815)

Synthesizing using the following code:

```
read_liberty -lib 
read_verilog johnson_counter.v
synth -top johnson_counter
abc -liberty 
show
```

</details>

<details>
<summary>Issue faced</summary>

Following is my Johnson counter verilog code:
```
module johnson_counter(clk, ori, count);
	input clk;
	input ori;
	output[2:0] count;
	reg[2:0] temp;
	always @(posedge clk or posedge ori)
	begin
		if(ori == 1)
		begin 
			temp <= 3'b100;
		end
		else
			temp <= {temp[0], temp[2:1]};
	end
	assign count = temp;

endmodule
```

And the following is the testbench:

```
`timescale 1ns/1ps
module tb_johnson_counter;
	reg clk;
	reg ori;
	wire [2:0] count;
	johnson_counter uut (.clk(clk), .ori(ori), .count(count));
	initial
	begin
		clk = 0;
		ori = 1;
		#1000 $finish;
	end
	initial
	begin
		$dumpfile("tb_johnson_counter.vcd");
		$dumpvars(0, tb_johnson_counter);
		#20
		ori = 0;
		#200;
		ori = 1;
		#20
		ori = 0;
		#1000 $finish;
	end
	
	always #10 clk = ~clk;
endmodule
```
Pre-synthesis results:

![Screenshot from 2023-07-01 17-11-24](https://github.com/walaa-amer/VSD-HDP/assets/85279771/f6ad0f39-5d7d-4ed6-b90e-1926fe31996f)


To synthesize in yosys:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog johnson_counter.v
synth -top johnson_counter
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog johnson_counter_net.v
show
```

The design hierarchy is as follows:

![Screenshot from 2023-07-01 17-13-21](https://github.com/walaa-amer/VSD-HDP/assets/85279771/1346ef82-5b4b-4e0e-9bc7-582698bc1a25)



The following is the netlist verilog code:

```
/* Generated by Yosys 0.28+12 (git sha1 4251d37f4, clang 10.0.0-4ubuntu1 -fPIC -Os) */

(* top =  1  *)
(* src = "johnson_counter.v:1.1-17.10" *)
module johnson_counter(clk, ori, count);
  (* src = "johnson_counter.v:6.2-14.5" *)
  wire [2:0] _00_;
  (* src = "johnson_counter.v:6.2-14.5" *)
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  (* src = "johnson_counter.v:3.8-3.11" *)
  wire _05_;
  (* src = "johnson_counter.v:5.11-5.15" *)
  wire _06_;
  wire _07_;
  wire _08_;
  wire _09_;
  (* src = "johnson_counter.v:2.8-2.11" *)
  input clk;
  wire clk;
  (* src = "johnson_counter.v:4.14-4.19" *)
  output [2:0] count;
  wire [2:0] count;
  (* src = "johnson_counter.v:3.8-3.11" *)
  input ori;
  wire ori;
  (* src = "johnson_counter.v:5.11-5.15" *)
  wire [2:0] temp;
  sky130_fd_sc_hd__clkinv_1 _10_ (
    .A(_06_),
    .Y(_01_)
  );
  sky130_fd_sc_hd__clkinv_1 _11_ (
    .A(_05_),
    .Y(_02_)
  );
  sky130_fd_sc_hd__clkinv_1 _12_ (
    .A(_05_),
    .Y(_03_)
  );
  sky130_fd_sc_hd__clkinv_1 _13_ (
    .A(_05_),
    .Y(_04_)
  );
  (* src = "johnson_counter.v:6.2-14.5" *)
  sky130_fd_sc_hd__dfrtp_1 _14_ (
    .CLK(clk),
    .D(temp[1]),
    .Q(temp[0]),
    .RESET_B(_07_)
  );
  (* src = "johnson_counter.v:6.2-14.5" *)
  sky130_fd_sc_hd__dfrtp_1 _15_ (
    .CLK(clk),
    .D(temp[2]),
    .Q(temp[1]),
    .RESET_B(_08_)
  );
  (* src = "johnson_counter.v:6.2-14.5" *)
  sky130_fd_sc_hd__dfstp_2 _16_ (
    .CLK(clk),
    .D(_00_[2]),
    .Q(temp[2]),
    .SET_B(_09_)
  );
  assign _00_[1:0] = temp[2:1];
  assign count = temp;
  assign _06_ = temp[0];
  assign _00_[2] = _01_;
  assign _05_ = ori;
  assign _07_ = _02_;
  assign _08_ = _03_;
  assign _09_ = _04_;
endmodule
```

The post-synthesis simulation:

![Screenshot from 2023-07-01 17-18-35](https://github.com/walaa-amer/VSD-HDP/assets/85279771/7757927d-cc33-4949-9657-9dfaabc415ed)


</details>

<details>
<summary>Solution</summary>

The problem was fixed by running the GLS post-synthesis simulation using the primitives.v and sky130_fd_sc_hd.v files added to this repo instead of the ones provided in the workshop repo using the following command:

```
iverilog -DFUNCTIONAL -DUNIT_DELAY=#0 new_my_lib/primitives.v ../new_my_lib/sky130_fd_sc_hd.v johnson_counter_net.v tb_johnson_counter.v
```

The results thus match the pre-synthesis simulation ones:

![d6 solution to issue](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e717e843-203d-40e8-b2ca-e344ffc81d9a)



</details>


## Day 7

### Static Timing Analysis

<details>
<summary>STA Basics</summary>

Min and Max delay constraints are a glimpse of STA that we have seen before.
    
Tclk > TCQ_A + TCOMBI + TSETUP_B: This equation lets us find the maximum delay the combinational circuit can handle.
    
THOLD_B < TCQ_A + TCOMBI: This equation lets us find the minimum delay needed to pass the correct data.

Setup time is always before the sampling point and holding is right after the point.
               
Min and max delay are correlated (2 sides of the same coin) since we need a max delay because the clock was pushed. We wouldn't need a min delay if the clock was not pushed.

The water bucket analogy shows us that there is more delay to fill the bucket when there is less inflow, and less delay when there is more inflow. The inflow of weater translates to inflow of current. Fast current sourcing (or fast rise) means we're going to have less delay.
The analogy also shows us that if we have the same inflow but 1 bucket larger than the other. In this case, delay is a function of bucket size to be filled, which translates to load capacitance.
               
![d7 water bucket analogy](https://github.com/walaa-amer/VSD-HDP/assets/85279771/9613b86b-5b5e-453f-add2-b77a281ffe91)
               
As a result, delay of a cell will be a result of both the input transition (inflow) and the output load.
               
               
Example:
![d7 example of delay](https://github.com/walaa-amer/VSD-HDP/assets/85279771/0fb9ecca-0496-4cd7-89f5-e7b45c5bc0b1)
            

The circuit above shows an OR gate followed by a XOR gate and preceeded by a buffer. The OR gate is connected to the XOR using a very lengthy net/wire, which will increase the load capacitance of this gate, and thus increase its delay. If the buffer rise time is also lengthy, it will also increase the delay of the OR gate.
The output load can also increase due to a high fan-out of the gate.
               
For a combinational cell, delay information from each input to every output that it can control are present in the timing arcs.
For a sequential circuit, the timing arcs will present the clk to A delay for DFF, the clk to Q delay and D o Q delay for D-latch, and the setup and hold times, as shown in the figure below.
               
![d7 timing arcs seq](https://github.com/walaa-amer/VSD-HDP/assets/85279771/eeddacf6-2304-4b5e-83a7-5782acda2a10)

</details>
    
<details>
<summary>Design Compiler Constraints</summary>

    
In the example below, we can see that there are 2 timing paths
Path 1: TCK >= TCQ + TCOMBI + TSU => TCK >= 0.5+1.2+0.5 => TCK >= 2.2ns
Path 2: TCK >= TCQ + TCOMBI + TSU => TCK >= 0.5+0.7+0.5 => TCK >= 1.7ns
    
=> Path 1 is the critical path since it is liomiting the clock frequency.
fclk = 1000/2.2 = 454.5MHz.
    
The delay can be improved by reducing the delayof the critical path. For example, if we decrease the AND gate delay to 0.5 from 0.7, TCOMBI = 1ns => TCK > = 2ns => fclk <= 500MHz.

![d7 timing paths example](https://github.com/walaa-amer/VSD-HDP/assets/85279771/d731dc78-c65a-410f-bca5-c9daebdc27df)

To pass the timing constraints to the tool, we need to define constraints.

Timing paths:
- Start point: can be the input ports and clock pins of registers
- End points: Output ports and D pin of DFF / DLAT
Timing paths start at one of the start ponts and one of the end points:
    - CLK to D : Reg 2 Reg timing path
    - CLK to output : IO timing path
    - Input to D : IO timing path
    - Input to output : IO paths that should not be present


    
<details>
<summary>Reg 2 Reg Constraints</summary>
    
The used clock will define how much delay is allowed in a circuit => Reg 2 Reg paths are constrained by the clock.

In the example below, we want the circuit to run at 500MHz => TCLK = 2ns => TCOMBI <= 1ns. The clock period will limit the delays in all Reg 2 Reg paths. Passing the needed freqeuncy to the tool will let it know which component to coose from the library in order to meet the wanted frequency requirement.

![d7 timing constraints example](https://github.com/walaa-amer/VSD-HDP/assets/85279771/b814d725-030c-49f9-827d-1b795bef8912)
</details>                                                               
    
<details>
<summary>IO Constraints</summary>                
The input and the output are connected to external logic to the boundary. This means that there are more synchronous (running at the same clock) Reg 2 Reg paths that need to be contrained.
                                                                                         
![d7 external paths](https://github.com/walaa-amer/VSD-HDP/assets/85279771/f277629e-654e-4361-b4fe-d89149bbfd1c)
                                                                                         
Assuming that fclk = 500MHz and for all flops: TCLK = 2ns, TCQ = 0.5ns, TSU = 0.5ns
Looking at the input, REG_1 will take 0.5ns out of the 2ns for setup. This will leave the input logic and the outside world with 1.5ns. Assuming that we're giving 50% of this time to the outside logic. This means that the the input logic is left with 0.75ns. This is the input external delay. The tool will then squeeze the logic on the input to meet these requirements.
Same for the output external delay.
    
We can get these valuesby standard interface specifications orr by IO Budgeing based on interactions between different parts of the design.

However, the signals are not ideal, so due to non-zero rise time that has not been accounted for, the input delay will increase.    
Another detail that should also be taken into consideration is the output load since it can also increase the ourput delay.
    
A rule of thumb usually used is the 70-30 rule, which means we give 70% of the clock period to the external delay and 30% to the internal delay.

    
</details>
</details>
    
<details>
<summary>Timing .libs</summary>
The library takes into consideration that the delay is a function of the output capacitance, which is equal to Cload = Cpin output + Cnet + sum(Cinputs) => it sets a maximum capacitance limit. If the capacitance value is large and we want to limit the input fanout capacitance, we can buffer the output, so the gate only sees the cap of the buffers now and those are driving the output o the next gate.
With respect to clock, the data is non-unate since the value might or drop
![d7 mac output cap](https://github.com/walaa-amer/VSD-HDP/assets/85279771/f2fa0d24-73df-4543-a61f-e26cbfabb1cf)
    
Delay Model Lookup Table:
The delay is a function of the input transition and the load output capacitance. The llokup table gives a delay value for each input trans and output load capacitance. For every gate in the design, the tool will find the input transition time and the output capacitance, and will map these values to a range in the lookup table, and will use theses 4 values from the table to do an interpolation and find the corresponding delay value.
![d7 delay model lookup table](https://github.com/walaa-amer/VSD-HDP/assets/85279771/05b1090e-116c-4d30-87be-d97e8a4fb6af)
Information about the gates are also presented in the library file. It gives the area, leakage power and power alongside a description of each of the block's pins (e.g. if it needs a clock or not).
    
Unateness:
This chacteristic is positive when the input rises and the output either does not change or also rises. This is seen in the AND and OR gates. In the case of NOT, NAND, and NOR when the input rises, the output falls or does not change, which is negative unateness.
In the case of XOR, both positive and negative unateness are observed. Thus, it is non-unate since input rising can cause the output to rise or fall. This unateness information is present in the output pin documentation under "timing_sense" in the lib file and used to propagate the input transition.
    
More details about about the timing type (falling or rising edge) of flip flops is given in the file. For a latches, sampling will happen before the edges. If the latch is negative-edge, the sampling will happen at the rising edge and the setup will happen before it, and vice versa.
</details>
<details>
<summary>DC Shell Commands</summary>   
We can use the DC shell to find cells in the library using the following commands:
    
```
dc_shell
echo $target_library
get_lib_cells */* -filter "is_sequential == true" #looking for all the sequential library cells
```
    
To list the loaded libraries:
```
list_lib
```
    
To find all the and gates in the library file:
```
get_lib cells */*and*
```
The output is a collection. To display the values of this collection seperately, we use the following command:
```
set my_lib_cell_name [get_object_name $my_lib_cell]; echo $my_lib_cell;
```
    
To find the pins of a gate:
```
get_lib_pins <library_path>/<gate_name>/*
```
To get the output pin:
```
foreach_in_collection my_pins [get_lib_pins  <library_path>/<gate_name>/*] {
set my_pin_name [get_object_name $my_pins];
set pin_dir [get_lib_attribute <lib_cell or lib_pin> direction; #finds the      direction of the pins
echo $my_pin_name $pin_dir;
}
```
The output of this command is all the input pins with 1 next each one and all the output pins with 2 next to each one.
    
To find the function of a gate:
```
get_lib_attribute <library_path>/<gate_name>/<output_pin> function
```
    
To run several commands, we can add them in a .tcl file. In my_script.tcl:
```
set my_list [list <library_path>/<gate_name1> \
<library_path>/<gate_name2> \
<library_path>/<gate_name3> \
<library_path>/<gate_name4> \
<library_path>/<gate_name5>]
    
#for each cell in the list, find the output pin name and its functionality
foreach my_cell $my_list{
    foreach_in_collection my_lib_pin [get_lib_pins $(my_cell)/*]{
        set my_lib_pin_name [get_object_name $my_lib_pin];
        set a [get_lib_attribute $my_lib_pin_name direction;
        if {$a > 1}{
            set fn [get_lib_attribute $my_lib_pin_name function];
            echo $my_lib_opin_name $a $fb;
        }
    }
}
```
Then saving and sourcing the file back in the DC shell:
```
source my_script.tcl
```
We get an output similar to the one below, for each cell displaying the functionality:
![d7 output of dc shell source  tcl](https://github.com/walaa-amer/VSD-HDP/assets/85279771/afefdbf9-fb54-4087-b153-29773a88ce5a)

To find the area of a cell:
```
get_lib_attribute <library_path>/<gate_name> area
```  

To find the pin capacitance of a cell:
```
get_lib_attribute <library_path>/<gate_name>/<pin> capacitance
```    

To check if a pin is a clock:
```
get_lib_attribute <library_path>/<gate_name>/<pin> clock
```

Getting all inputs, outputs, clocks, and registers (run by a specific clock):
```
all_inputs
all_outputs
all_clocks
all_registers
all_regsiters -clock <clock_name>
```

To get the fanout from a port and print its endpoints:
```
all_fanout -from <port_name>
all_fanout -flat -endpoints_only -from <port_name>
```

To get the fanin to a port and print its start points:
```
all_fanin -to <port_name>
all_fanin -flat -startpoints_only -to <port_name>
```
    
</details>
    
## Day 8
    
<details>
<summary>Uncertainty in clock tree modelling</summary>
Dataflow: RTL->Synthesis->DFT->floor plan->Clock tree synthesis->place and route->final physical design database
Logic is optimized in the synthesis step, where the clock is considered ideal. After CTS, flops might see the clock arriving at different times due to buffer delays, which creates clock skew.
-> Tclk  Tskew > TCQ_A + TCOMBI + TSETUP_B
Another problem is jitter which is caused by the stochastic variation of the clock generation which causes the signal to arrive in a time window instead of an exact instance, and thus can arrive in a different instance in this window every cycle.
    
So we need to model the clock for the following:
- period
- source latency: time taken by the source to generate the clock
- clock network latency: time taken by the clock distribution network
- clock skew:CTS will reduce this skew but it will not eliminate it
- jitter :(period or duty cycle)

Clock uncertainty is the jitter and the skew combined.
Post CTS, the clock network is real so th2 modelled clock skew and network latency constraints should be eliminated and the tool should be able to compute these values. These constraints are useful to emulate the clock post CTS before going through the process, thus are not useful after that. Jitter constraints should be kept after the CTS.
    
Constraints:
- clocks: R2R paths: period, latency, uncertainty
- IO: IO2REG and REG2IO paths: input delay, input transition, output delay, output load
</details>
    
<details>
<summary>DC Shell Commands</summary>
DC takes constraints inthe form of SDC (Synopsis Design Constraints).
    
Getting ports:
```
get_ports clk;
get_ports *clk* #returns a collection of ports having 'clk' in their names
get_ports * #returns all ports
get_ports * -filter "direction==in/out"
```
    
Getting clocks:
```
get_clocks *
get_clocks *clk*
get_clocks * -filter "period > 10"
get_attribute [get_clocks my_clk] period
get_attribute [get_clocks my_clk] is_generated
report_clocks my_clk
```
    
Listing cells in a combo_logic hierarchical design where we instantiate a physical cell U1:
```
get_cells * -hier #lists all cells (physical and hierarchical)
get_attribute [get_cells u_combo_logic] is_hierarchical #returns true
get_attribute [get_cells u_combo_logic/U1] is_hierarchical #returns false
```
    
To create a clock:
```
create_clock -name <clk_name> -per <period> [clock definition point] (optional)-wave{first rising and falling edges};
set_clock_latency <delay> <clk_name>; #models network clock delay
set_clock_uncertainty <skew+jitter> <clk_name>; #should be changed to only jitter post CTS
```
Clocks must be created on the clock generators (PLL, Oscillators) or primary IO pins (for external clocks). Clocks should not be created on hierarchical pims which are not clock generators.
    
To constraint the inputs:
```
set_input_delay -max <max delay> -clock [get_clocks <ref clk name>] [get_ports <name>*];
set_input_delay -min <min delay> -clock [get_clocks <ref clk name>] [get_ports <name>*];
set_input_transition -max <max delay> [get_ports <name>*];
set_input_transition -min <min delay> [get_ports <name>*];
```
    
To constraint the outputs:
```
set_output_delay -max <max delay> -clock [get_clocks <ref clk name>] [get_ports <name>*];
set_output_delay -min <min delay> -clock [get_clocks <ref clk name>] [get_ports <name>*];
set_output_load -max <max load> [get_ports <name>*];
set_output_load -min <min load> [get_ports <name>*];
```    
    
</details>

<details>
<summary>Project design using design shells</summary>

### Lab 8:
```
csh
dc_shell
read_verilog lab8_circuit.v
link
compile_ultra
foreach_in_collection my_ports [get_ports *]{
    set my_port_name [get_object_name $my_port];
    echo $my_port_name;
}
get_attribute [get_ports <name>] direction
foreach_in_collection my_ports [get_ports *]{
    set my_port_name [get_object_name $my_port];
    set my_port_dir [get_attribute $my_ports_name direction];
    echo $my_port_name $my_port_dir;
} #print direction of all ports
get_cells *
get_attribute [get_cells <cell name>] is hierarchical
get_cells * -hier -filter "is_hierarchical == true" #to get hierarchical cells
get_attribute [get_cells <cell name> ref_name #to get the reference name of a cell
get_nets * #to get the nets in a design
foreach_in_collection my_pin [all_connected n5]{
    set pin_name [get_object_name $my_pin];
    set dir [get_attribute $pin_name direction];
    echo $pin_name $dir;
} #to get the direction for each net and check their drivers
write -f ddc -out lab8_circuit.v
```
Note:
In digital design, nets only have 1 driver, which means a net can only be connected to 1 source unless each source is connected to a switch that allows connecting the net to only 1 source at a time.

### Lab 9:

```
get_pins *
foreach_in_collection my_pin [get_pins *]{
    set pin_name [get_object_name $my_pin];
    echo $pin_name;
} #to print the names of all pins in the design
foreach_in_collection my_pin [get_pins *]{
    set pin_name [get_object_name $my_pin];
    set pin_dir [get_attribute [get_pins $pin_name] direction];
    if ([regex $pin_dir in]){
        set pin_clk [get_attribute [get_pins $pin_name] clock -quiet];
        if ($pin_clk){
            echo $pin_name;
        }
    }
} #to check if the input pins are clocks
get_attribute [get_pins <pin_name>] clocks; #to find which clocks are driving this pin
```

### Lab 10:

```
current_design #to find the name of the top module we're currently working on
create_clock -name MYCLK -per -10 [get_ports clk];
get_attribute [get_clocks MYCLK] period
get_attribute [get_clocks MYCLK] is_generate_clock #to check if the clock is a master clock
get_attribute [get_ports out_clk] clocks;
foreach_in_collection my_pin [get_pins *]{
    set pin_name [get_object_name $my_pin];
    set pin_dir [get_attribute [get_pins $pin_name] direction];
    if ([regex $pin_dir in]){
        set pin_clk [get_attribute [get_pins $pin_name] clock -quiet];
        if ($pin_clk){
            set clk [get_attribute [get_pins $my_pin_name] clocks];
            set clk_name [get_object_name $clk];
            echo $pin_name $clk_name;
        }
    }
} #to check if the input pins are clocks and whcih clocks are reaching them
report_clock * #return a detailed description of the clocks created in the design
remove_clock <clock name> # to remove a created clock
```
### Lab 11:

Note:
TCQ + TCOMBI + TSU <= Tclk - Tuncertainty
TCQ + TCOMBI <= Tclk - TSU - Tuncertainty
Arrival time <= Required time
```
set_clock_latency -source 1 [get_clocks MYCLK]; #set delay of the source
set_clock_latency 1 [get_clocks MYCLK]; #set delay on the network
set_clock_uncertainty 0.5 [get_clocks MYCLK]; #set setup time
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]; #set delay time
report_timing * #shows constraints for all paths
```

When the uncertainty constraint is added, the value is subtracted by the tool to find the max value to violate the constraint, and is added when finding the min value.
![d8 uncertainty tool](https://github.com/walaa-amer/VSD-HDP/assets/85279771/b07e7a18-1e9b-47ef-b47e-25b8e8c7cd1d)

### Lab 12

```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports <input port>] #to model the max delay on the input
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports <input port>] #to model the max delay on the input

set_input_transition -max 0.3 [get_ports <input port>]
set_input_transition -min 0.1 [get_ports <input port>]

report_timing -from <input port> -trans -net -cap #to show the timing report for the max path
report_timing -from <input port> -trans -net -cap -nosplit -delay_type_min #to show the timing report for the min path

set_output_delay -max 5 -clock [get_clocks MYCLK] [get_ports <output port>] #to model the max delay on the input
set_output_delay -min 1 -clock [get_clocks MYCLK] [get_ports <output port>] #to model the max delay on the input

set_load -max 0.4 [get_ports <output port>]
set_load -min 0.1 [get_ports <output port>]

report_timing -from <output port> -trans -net -cap #to show the timing report for the max path
report_timing -from <output port> -trans -net -cap -nosplit -delay_type_min #to show the timing report for the min path

```
</details>

<details>
<summary>Generated clocks</summary>
The output clock connected to the input clock is logically the same but it not physically because it is delayed.
	
```
create_generated_clock -name MY_GEN_CLK -master [get_clocks MY_CLK] -source [get_ports CLK] -div 1 [get_ports OUT_CLK] #generate an output clock from the input clock (its frequency can be divided wrt to the input)
get_attribute [get_clock MY_GEN_CLK] is_generated #return true
set_output_delay -max 5 [get_ports <output port> -clock [get_clocks MY_GEN_CLK]
set_output_delay -min 1 [get_ports <output port> -clock [get_clocks MY_GEN_CLK]
report_timing -to <output port>
```

In case the design has 2 input clocks that drive 2 seperate parts of the design, we need to constraint the design in a different way. 
</details>

<details>
<summary>Delay</summary>

Negative input delay for max is relaxing the constraint since we are gaining more available time since the data is stable before the rise of the clock edge.
Negative input delay for min should be avoided to avoid hold failure, since positive delay helps ensure the hold time, so we should delay the input.
So:
+ -ve delay for max is relaxing the path
+ +ve delay for max is tightening the path
+ -ve delay for min is tightening the path
+ +ve delay for min is relaxing the path
 

Same applies for the output delay.

### Ways to constrain additional combinational path
#### Set max latency to the path
To constrain this additional combinational path in the circuit shown below, we use:

```
set_max_latency 1 -from [get_ports IN_C] -to [get_ports OUT_Z]
set_max_latency 1 -from [get_ports IN_D] -to [get_ports OUT_Z]
```

![d8 combo path constr](https://github.com/walaa-amer/VSD-HDP/assets/85279771/e80c5fc4-2154-4d1a-b5a4-94f3284357f9)

#### Virtual clock

Looking at the case where a flip flop passes its output to IN_C and another flip flop takes as input OUT_Z, and these 2 flip flops (that are not part of the implemented module) follow a second clock CLK2. If we do not take into consideration the constraints on the combinational path, the system will suffer. The solution is to create a virtual clock using:
```
create_clock -name MY_VCLK -per <period_value> #without specifying a clock definition point and no latency
```
And then, we add constraints wrt to this virtual clock:
```
set_output_delay -max 2.5 -clock MY_VCLK [get_ports OUT_Z]
set_input_delay -max 1.5 -clock MY_VCLK [get_ports IN_C]
set_input_delay -max 1.5 -clock MY_VCLK [get_ports IN_D]
```
This is equaivalent to setting the max latency to 1ns.

### IO Constraints revisited

#### Effect of external input and output logic
If the input is connected to external logic where the flip flop is negative edge, the flag -clock_fall should be added to the set input_delay line to specify that the data needs x ns to arrive after the falling edge of the clock and to be captured in the next rising edge of the clock. 
If one input has to be constrained twice on the delay, the flag -add should be added.

```
set_input_delay -max 2 -clock CLK [get_ports IN_A]
set_input_delay -max 3 -clock CLK __-clock_fall -add__ [get_ports IN_A]
```
Same applies to the output path.

#### Driving cells

When working with module impementations, the input load migh affect the input transition (larger load would lead to longer transition time). Thus, we model the transition using a driving cell instead of a constant, ehich makers it more flexible and adaptive to load variation, so it is more accurate and recommended for module level implementations. We replace the set_input_transition commands, which is more convenient for top module level IOs, with set_driving_cell command as follows:
```
set_driving_cell -lib_cell <lib_cell_name> <ports>
set_driving_cell -lib_cell sky130_fd_sc_hd__buf_1 [all_inputs] #example
```
</details>

<details>
<summary>Additional combinational path case</summary>

### Constraining using set_max_delay

```
csh
dc_shell
read_verilog lab14_circuit.v
link
compile_ultra
report_timing -to OUT_Z #shows that the new added path is unconstrained
report_timing -from IN_C #shows that the new added path is unconstrained
set_max_delay 0.1 -from [all_inputs] -to [get_ports OUT_Z]
report_timing -to OUT_Z
```

If the constraint is violated after this command, we can recompile for the tool to optimize the design to our contraints (choose different cells) and then rerun the timing report to check if the constraint is met.

### Constraining using virtual clock

A virtual clock is a clock used in the system but not in the modulke implemented, so it is created in our implementation without a definition point (no source).

```
create_clock -name MYVCLK -per 10
set_input_delay -max 5 [get_ports IN_C] -clock [get_clocks MYVCLK]
set_input_delay -max 5 [get_ports IN_D] -clock [get_clocks MYVCLK]
set_output_delay -max 4.9 [get_ports IN_D] -clock [get_clocks MYVCLK]
report_timing -to OUT_Z
compile_ultra (if violated previously tm optimize for constraint added)
report_timing -to OUT_Z
```

</details>

## Day 9

<details>
<summary>Constraints</summary>
    
Adding constraints to my Johnson counter design.
I defined the constraints in a johnson_counter_const.sdc file:

```
create_clock -name MYCLK -per 10 [get_ports clk]
set_clock_latency -source 1 [get_clocks MYCLK]
set_clock_latency 1 [get_clocks MYCLK]
set_clock_uncertainty 0.5 [get_clocks MYCLK]
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]
set_output_delay -max 5 -clock [get_clocks MYCLK] [get_ports count]
set_output_delay -min 1 -clock [get_clocks MYCLK] [get_ports count]
set_load -max 0.4 [get_ports count]
set_load -min 0.1 [get_ports count]
```

And I created the johnson_counter_script.tcl file to apply and evaluate the constraints:

```
read_liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog johnson_counter_net.v
link_design johnson_counter
read_sdc johnson_counter_const.sdc
report_checks -path_delay min_max -fields {nets cap skew input_pins} -digits {4} > walaa_johnson.log
```

</details>


<details>
<summary>Timing report</summary>

Running on OpenSTA:

![d9 opensta](https://github.com/walaa-amer/VSD-HDP/assets/85279771/79edeeff-4fd8-44e1-91a6-3251abab814a)


Results in walaa_johnson.log:

Max delay path:

![d9 timing report max](https://github.com/walaa-amer/VSD-HDP/assets/85279771/a328b10f-8970-47db-98f5-6a656c5469d2)

Min delay path:

![d9 timing report min](https://github.com/walaa-amer/VSD-HDP/assets/85279771/1c91da3e-ae44-4a45-b99c-f06878ce208e)


</details>


## Day 10

<details>
<summary>Intro to circuit design</summary>

A delay table will help us find the delay for each gate based on the intersection between the input skew and the output load of that specific gate.
Spice will provide these tables based on detail characterization of the CMOS gates.

</details>


<details>
<summary>NMOS structure</summary>

An NMOS is a 4-terminal device consisting of a p-substrate (body B) and 2 n diffusion regions (source S and drain D), the gate oxide area, and the polysilicon or metal gate (G) and 2 isolation regios to separate transistors from each other.
A PMOS is an inverted version of the NMOS structure.

- Threshold voltage:
  Considering Vg = Vs = Vd = Vb = 0. The substrate-source and the substrate-drain form a p-n junction that are off since no voltage => high resistance. If a small positive voltage is applied to the gate, a positive charge is passed to the gate which will try to repel all positive charges in the p-substrate, leaving negatively charged holes forming a depletion region. Increasing the voltage at the gate, the depletion region gets larger, creating an n-type surface in the p-substrate when the voltage hits a threshold voltage (Vt). The positive charge at the gate will now also attract the negative charge from the n diffusion regions passing from the source through the n-type channel to the drain. Vt is the Vgs voltage value at whoch a strong inversion happens.

  The threshold voltage depends on the voltage Vsb. As Vsb is increased, the depletion region right below the source increased in area. Some charges from the channel also start to be pulled towards the source, slowing down the process of surface inversion to n-type. At Vsb = 0, the threshold voltage is Vt0 and Vgs = Vt0. At a positive Vsb, Vgs = Vt0 + V1. This is the body effect.
  The threshold equation is:
  Vt = Vt0 + sigma.(sqrt(abs(-2phi(f) + Vsb)) - sqrt(abs(-2phi(f)))) / sigma is the body coefficient and phi(f) is the Fermi Potential

  This equation will characterize every transistor model.

For Vgs > Vt, a charge equivalent to the voltage value Vgs - Vt is induced in the channel. 
Applying a voltage Vd at the drain will lead to a potential difference between the drain and the source. At a point x in the channel, Vgate-to-channel(x) = Vgs- V(x).
The charge at point x is then Q(x) = -Cox (Vgs - V(x) - Vt) with Cox = eox / tox. 

From the device's point of view, there are 2 currents: drift current iduced by the difference in potential and the diffusion current induced by the difference in carrier concentration. The drift current is equal to the velocity of the charge carriers X available charge / channel width.

Model for ID for the spice simulation:

ID = -Vn(x) . Q(x) . W
From deriving this equation:
ID = un.Cox.(W/L).[(Vgs-Vt).Vds - Vds^2/2]
ID = kn.[(Vgs-Vt).Vds - Vds^2/2] with kn = un.Cox.(W/L)

If Vds<=(Vgs - Vt), Vds <<< so we can neglect the squared value in the equation and the MOSFET would be operating in the linear/resistive region:
ID = kn.(Vgs-Vt).Vds

If Vds>(Vgs-Vt), the channel starts thinning from the drain side of the MOSFET at pinch-off and disappearing as we increase Vds even more.
Pinch-offf condition: Vgs-Vds <= Vt
At saturation: ID = (kn/2) . (Vgs-Vt)^2 = (kn'/2) . (W/L) . (Vgs-Vt)^2
Taking into consideration channel length modulation: ID = (kn'/2) . (W/L) . (Vgs-Vt)^2.[1+lambda.Vds]

</details>

<details>
<summary>Intro to SPICE: Spice Setup</summary>

We have models for the threshold voltage and the drain current for the linear region and the saturation region.

In these equation, we have constants specific for each technology node that we provide through the model file.

The spice software will take the model parameters and the netlist to generate the (ID-Vds) characteristic.


<details>
<summary>Technology file</summary>

We need the models for the MOSFETs. The model files will provide the constants needed to compute the threshold voltage and the drain current.

The model file would look as follows:

```
.MODEL nmos(model name) NMOS(foundation name) (TOX = .. VTH0 = .. U0 = .. GAMMA1 = ..)
.MODEL pmos(model name) PMOS(foundation name) (TOX = .. VTH0 = .. U0 = .. GAMMA1 = ..)
.endl
```

The model name is the name used in the netlist. This file is then packaged into a .mod file.

</details>
<details>
<summary>SPICE Netlist</summary>
Given the following circuit, we would like to build the netlist:
	
![d10-circuit](https://github.com/walaa-amer/VSD-HDP/assets/85279771/38c52231-b041-4318-9c78-5a698671664c)


Wires are collapsed to nodes joining several components. Nodes are given names to represent them. 

A MOSFET is described using the following line:

```
MOSFET_NAME DRAIN_NODE_NAME GATE_NODE_NAME SOURCE_NODE_NAME BODY_NODE_NAME  MOSFET_NAME_IN_TECH_FILE W=width L=length
```

A resistor is described as follows:

```
RESISTOR_NAME INPUT_NODE OUTPUT_NODE value
```

A voltage source is described as follows:

```
SOURCE_NAME_STARTING_WITH_V POS_NODE NEG_NODE value
```

To include the model file:

```
.LIB "Models.mod" CMOS_MODELS
```

Commands added to run spice smulations, such as a sweep over a voltage source using:

```
.dc VOLTAGE_SOURCE_NAME START_VAL END_VAL INCREMENT_VAL (needs continuation)
```


</details>

<details>
<summary>First SPICE simulation</summary>

to run ngpice, we run the following command:

```
ngspice <name (day1_nfet_idvds_L2_W5.spice)>
```

In the ngspice command prompt, we add "plot". For any point, we can click on the plot and the coordinates of it will show on the terminal.

Following are the result of the simulation that I ran:

![d10 firstspicesim](https://github.com/walaa-amer/VSD-HDP/assets/85279771/01a47866-1728-46c6-83e9-ca83677cdfb4)

</details>
</details>

## Day 11

<details>
<summary>Short and Long channel devices</summary>

In the image attached in the last simulation, we can see that there are 2 seperate regions where in one of them the MOSFET woiuld be working linearly and in the other one the drain current would saturate and would not change signifcantly with the change in Vds. We also notice that the drain current increase quadratically with the gate voltage.

If we reduce the length and the width to result in a short-length device but in a way to conserve the ratio W/L, the drain current's relationship with the gate voltage becomes linear. This is due to the effect of velocity saturation in short-length channels. After a threshold electric field (Ec), the velocity of electrons will saturate at Vsat, stopping the ID to reach the usual saturation region. In that case, a new mode of operation exists: Velocity saturation. To take that int consideration, we will need to remodel the current drain to become:

ID = kn. [(Vgs-vt).Vmin - (Vmin^2)/2].[1+lambda.Vds] where Vmin = min(Vgs-Vt, Vds, Vdsat)

</details>

<details>
<summary>Short channel simulations</summary>

To simulate on ngspice, we sweep both vds and vgs:

```
ngspice day2_nfet_idvds_L015_W019.spice
```

and then we enter the command "plot -vdd#branch" to plot ID vs Vds. The following results show a linear relationship between ID and Vgs:
![d11 short channel characteristic](https://github.com/walaa-amer/VSD-HDP/assets/85279771/06f16cdd-bf98-429c-9fc5-a4df0b68cdad)


A step further is to sweep only Vgs while keeping Vds constant using the following command

```
ngspice day2_nfet_idvgs_L015_W019.spice
```

Plotting shows a clear linear relationship between ID and Vgs:
![d11 id vs vgs](https://github.com/walaa-amer/VSD-HDP/assets/85279771/deb58660-dcfb-4156-94df-a6187ecf6741)

To find the threshold voltage from this plot, we draw the tangent to the linear part of the curve and find the intersection of it wrt to he X-axis, as shown below:

![d11 find vt](https://github.com/walaa-amer/VSD-HDP/assets/85279771/624c13ac-3bd1-4ac2-a610-b5140cb55b13)


</details>

<details>
<summary>CMOS Voltage-Transfer Characteristics (VTC)</summary>

In an CMOS inverter, Vin is connected to the gates of both the NMOS and the PMOS. 
If Vin is high, Vgs(n) > Vt(n) which means the NMOS is ON and is represented by a resistor, and Vgs(p) > Vt(p), so the transistor PMOS is OFF. 

If Vin is low, Vgs(p) < Vt(p) so the PMOS is ON and is represented by a resistor, and Vgs(n) < Vt(n) so NMOS is OFF.

This is described in the following diagram along with the naming scheme for the differet components and nodes in the circuit:

![d11 cmos inverter 1](https://github.com/walaa-amer/VSD-HDP/assets/85279771/398b5d4f-6377-483c-8c19-e74b6fc2d866)

We have:
VgsN = Vin
VdsN = Vout
VgsP = Vin - Vdd
VdsP = Vout - Vdd

IsdP = -IdsN
![d11 inverter characteristic 1](https://github.com/walaa-amer/VSD-HDP/assets/85279771/56ff34dc-0c7f-491a-bf16-3be3dd1ee3d9)

To obtain the  inverter's characteristic, we get the I_V curve for the NMOS and the PMOS, then we modify the PMOS curve to represent it in terms of IdsN and VdsN (Vout), which means first invert it wrt to the x-axis and then shift it to the right by Vdd, as this is the graphical representation of applying the equations above:

![d11 inverter characteristic 2](https://github.com/walaa-amer/VSD-HDP/assets/85279771/c5434579-af2d-4789-a536-26d69a8b34a3)

Merging the 2 curves helps us find the voltage transfer characteristic which tells us the value of Vout for each point of Vin:

![d11 inverter characteristic 3](https://github.com/walaa-amer/VSD-HDP/assets/85279771/8dfb10b8-cba4-47d0-b8d4-c2172597a0d0)


It is worth noting that the linear region in the Vin-Vout curve is a high gain range, so a small change in the input leads to a high change in the output.


</details>

## Day 12 & 13

<details>
<summary>CMOS VTC SPICE Simulation</summary>

We first need to identify the connectivity of the components in the circuit and the component values (W, L, CL, Vin, Vout...).
We then have to identify the nodes in the circuit and give them names.

![d11 circuit of inverter](https://github.com/walaa-amer/VSD-HDP/assets/85279771/39a14a39-c2ee-4414-93d0-97df90f6efe8)


For the case of this circuit, the netlist would be:

```
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u
cload out 0 10f
Vdd vdd 0 2.5
Vin in 0 2.5

.op
.dc Vin 0 2.5 0.05

.LIB "tsmc_025um_model.mod CMOS_MODELS
.end
```
In this case we have Wn = Wp and Ln = Lp. The simulation will result in this VTC waveform:

![d12 vtc of inveter](https://github.com/walaa-amer/VSD-HDP/assets/85279771/13088fc4-43d6-4151-94b8-66fd483efdcf)

Increasing Wp = 2.5Wn and simulating would result in the waveform on the right:



The 2 waveforms' shapes are almost the same, which shows that the CMOS design is very robust.

The robustness of the design is measured by the switching threshold where Vin = Vout. In the first waveform, VM ~ 1V, and in the second waveform, VM ~ 1.2V. In this point of the waveform, both transistors are saturated and Vgs = Vds and the 2 currents running in both transistors are equal in value but in different directions.

To find the best value of Vm, we need to solve the following equation:
IdsP = -IdsN
IdsP + IdsN = 0

We know that:
ID = un . Cox . (W/L). [(Vgs-vt).Vdsat - (Vdsat^2)/2]

So:
IdsN = kn. [(Vm-vt).Vdsatn - (Vdsatn^2)/2]
IdsN = kp. [(Vm-Vdd-vt).Vdsatp - (Vdsatp^2)/2]

Making the equation:
kn. [(Vm-vt).Vdsatn - (Vdsatn^2)/2] + kp. [(Vm-Vdd-vt).Vdsatp - (Vdsatp^2)/2] = 0

Solving it for Vm gives:
Vm = R.Vdd / (1+R) with R = [(Wp/Lp).kp'.Vdsatp]/[(Wn/Ln).kn'.Vdsatn]

Going the other way around, we can find Wp and Wn from a specific value of Vm:
IdsP = -IdsN
kn. [(Vm-vt).Vdsatn - (Vdsatn^2)/2] = kp. [(-Vm+Vdd+vt).Vdsatp + (Vdsatp^2)/2]

kn. Vdsatn. [(Vm-vt) - (Vdsatn)/2] = kp. Vdsatp. [(-Vm+Vdd+vt) + (Vdsatp)/2]

[kn. Vdsatn]/[kp. Vdsatp] = [(-Vm+Vdd+vt) + (Vdsatp)/2] / [(Vm-vt) - (Vdsatn)/2]

[(Wn/Ln) .kn'. Vdsatn]/[(Wp/Lp) .kp'. Vdsatp] = [(-Vm+Vdd+vt) + (Vdsatp)/2] / [(Vm-vt) - (Vdsatn)/2]

(Wn/Ln)/(Wp/Lp) = [kp'.Vdsatp.[(-Vm+Vdd+vt) + (Vdsatp)/2]] / [kn'.Vdsatn.[(Vm-vt) - (Vdsatn)/2]]

Wee can now plug in the value of Vm and find the ratio.

As Wp increases, Vm is pulled more towards Vdd and the curve is shifted further to the right, rise delay decreases and fall delay increases. 
At a ratio around 2, the fall time is equal to the rise time, which is a desired behavior, especially in the clock buffer cell design.

</details>

<details>
<summary>VTC and Transiant analysis simulations</summary>

Simulating the case where Wp = 2.33Wn.
Running on ngspice:
```
ngspice day3_inv_vtc_Wp084_Wn036.spice
```

Then use the command
```
plot out vs in
```

The result is this curve thatlets us find the switching threshold voltage (VM) which is equivalent in this case to around 0.8:
![d12 outvsin plot](https://github.com/walaa-amer/VSD-HDP/assets/85279771/7ef140cb-2cc4-4842-980d-02a756d88a43)

Then running the transient analysis:

```
ngspice day3_inv_tran_Wp084_Wn036.spice
```
This spice netlist looks as follows:

![d12 tran analysis code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/fe2322c2-59ae-452d-9b7e-95be537f8f3f)

The pulse wave syntax is as follows:
```
Vx <input_node> <output_node> <start_value> pulse <start_val_pulse> <end_val_pulse> <start_time> <rise_time> <fall_time> <period>
```

Running and plotting using
```
plot out time in
```
results in:

![d12 tran analysis result](https://github.com/walaa-amer/VSD-HDP/assets/85279771/915d3c08-e8cc-48f6-98d8-25c699d37372)

From this plot, we can find the rise and fall delay by clicking on the graph to find the coordinates of the points needed and subtracting these values.

</details>

## Day 14

<details>
<summary>Noise Margins</summary>

In an inverter, the output will take some time to decrease from high to low as the input voltage increases. However, there is a range of low input values of Vin where Vout stays high (NML) and there is a range of high values of Vin where Vout is low (NMH).

NML = VIL - VOL   
NMH = VOH - VIH   
where VIL = first occurance of voltage at tangent slope = -1
and VIH = second occurance of voltage at tangent slope = -1

In these ranges of voltages, noise will not affect the output.

For Wn = Wp: NML = NMH -> large noise margins from both sides   
As Wp increases, the PMOS is larger and able to hold the logic 1 in the capacitor for longer, so NML increases and NMH decreases, as the PMOS becomes stronger than the NMOS in the circuit.

</details>

<details>
<summary>Noise Margins Simulation</summary>

To run the simulation on ngspice:

```
ngspice day4_inv_noisemargin_wp1_wn036.spice
```

then run:

```
plot out vs in
```
![d13 noise margin plot](https://github.com/walaa-amer/VSD-HDP/assets/85279771/edf95fb0-928f-4742-bdbf-75bd82f4045f)


From the plot, we can find VIL and VIH, and then calculating NML and NMH.   
In this case:  
VIL = 0.754098   
VOH = 1.73636   
VIH = 0.987705   
VOL = 0.0863636   
->   
NML = VIL - VOL ~ 0.67   
NMH = VOH - VIH ~ 0.75


</details>

## Day 15

<details>
<summary>Power Supply Scaling</summary>

The CMOS inveter's robustness should also be tested for sypply voltage scaling, which means a change in Vdd should nott affect the functionality of the inverter.

The following code is  used to run a dc sweep over the input for 5 different values of Vdd:
![d15 loop spice code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/21af237c-cc2d-4388-8d33-a765ca5a1ad7)

The ".control" allows us to script in the usual coding syntax to do some complex operations.

Comparing running at high vs low supply voltage:    
Gain = dVout/dVin: larger for low voltage (advantage of low)
energy = 0.5CV^2: lower for low voltage (advantage of low)
rise and fall time: larger for low voltage (disadvantage of low)

</details>

<details>
<summary>Design Variations</summary>

Variations can come from different sources like:    
- etchine: the process of chemically defining the shapes of different metal components. This process defines the overlap between metal layers. A distortion in the etched layers can lead to variations in a chip, mainly on the W and L characteristics of the MOSFET, and thus affect ID.
- oxidation: inthis process, the oxidation process could lead to different variations across transistors, which will affect Cox, and thus affect ID.

</details>

<details>
<summary>Design Variation Simulations</summary>

The following code allows us to modify the width of the PMOS and NMOS 5 times (going from strong PMOS and weak NMOS to weak PMOS and strong NMOS) and apply a dc sweep over the input every time:

![d15 variations code](https://github.com/walaa-amer/VSD-HDP/assets/85279771/2519fcb1-eb98-40dc-ab48-da41a2a97726)

These variations would affect the switching threshold and the noise margins, but they keep them in a reasonable range, so they do not highly affect the robustness of the CMOS design.

To run a strong PMOS and weak ?NMOS case on ngspice:

```
ngspice day5_inv_devicevariation_wp7_wn042.spice
```

Then run:

```
plot out vs in
```

The result plot is:
![d15 variations simulation](https://github.com/walaa-amer/VSD-HDP/assets/85279771/02fcdae9-cdae-4c9b-8c10-747a8d68457b)


where we can see that the switching threshold is around ~ 0.995V which is around 90mV higher than Vdd/2. The low noise margins are also much higher than the high noise margins. In this case we are looking at a stromg PMOS and weak NMOS.


</details>

## Day 16

<details>
<summary>Post-Synthesis STA on my design</summary>

I ran post-syntesis STA on my Johnson counter design on different ss (slow), ff (fast), tt (typical). The .lib files were cloned from the following github repo: https://github.com/Geetima2021/vsdpcvrd.    
The constraints added are the following:

```
create_clock -name MYCLK -per 10 [get_ports clk]
set_clock_latency -source 1 [get_clocks MYCLK]
set_clock_latency 1 [get_clocks MYCLK]
set_clock_uncertainty 0.5 [get_clocks MYCLK]
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]
set_output_delay -max 5 -clock [get_clocks MYCLK] [get_ports count]
set_output_delay -min 1 -clock [get_clocks MYCLK] [get_ports count]
set_load -max 0.4 [get_ports count]
set_load -min 0.1 [get_ports count]
```

And I ran the johnson_counter_script.tcl file to apply and evaluate the constraints for every .lib file and saved the results in a corresponsing log file:

```
read_liberty vsdpcvrd/resources/timing_libs/sky130_fd_sc_hd__X.lib
read_verilog johnson_counter_net.v
link_design johnson_counter
read_sdc johnson_counter_const.sdc
report_checks -path_delay min_max -fields {nets cap skew input_pins} -digits {4} > walaa_johnson_X.log
report_tns >> walaa_johnson_X.log
```


</details>


<details>
<summary>Resultsn</summary>
	
I collected the WNS (worst negative slack), the WHS (worst hold slack), and the TNS (total negative slack) and put them in the table below:

![d16 pvt tble](https://github.com/walaa-amer/VSD-HDP/assets/85279771/890ce083-9484-4c76-acb2-c57fb72466a9)


They are also represented in the folowing plot:

![d16 plot](https://github.com/walaa-amer/VSD-HDP/assets/85279771/846afbf0-91d3-4005-8452-23ca0533b10e)






</details>

## Day 17

<details>
<summary>How to talk to computers</summary>

A chip has different components:     
1- Core: where the digital logic is placed. In the core, there are foundery IPs and Macros.    
2- PADS: the communication line between the inside and the outside of the chip.
3- Die: area of the chip on the silicon wafer.   

<details>
<summary>Intro to RISC-V</summary>

Communication with a compouter is established through their instruction set architecture (ISA). A program that needs to be run on your hardware is compiled to its assembly format that then is converted to the binary format executed in a particular layout. Another communication line between the laytout and the ISA is the RTL implementation. So the RTL implements the architecture which includes the ISA and the layout is generated through a RTL2GDS flow. So for the RISC-V, the flow becomes: RISC-V architecture -> RTL implementation -> layout

</details>

<details>
<summary>From Software Application to Hardware</summary>

The communication btewen software apps and hardware happen on the system software level where applications are compiled to executable assembly files and those are then assembled to amchine code that runs on he hardware

</details>
</details>


<details>
<summary>SoC design and OpenLANE</summary>

<details>
<summary>Simplified RTL2GDSII flow</summary>
A PDK (Process Design Kit) is a collection of files used to model a fabrication process for the EDA tools used to design a IC. It has information such as The process design rules (DRC, LVS, PEX), device models, digital standard cell libraries, I/O liraries...

For an ASIC design, the methodology followed is a RTL to GDSII flow as follows:      

(RTL)Synthesis -> floor and power planning -> placement (+PDK) -> Clock Tree Synthesis -> Routing -> Sign Off (GDSII)

1- Synthesis: Converts RTL to a circuit out of the components from the standard cell library to generate the gate-level netlist (GLS) which is equivalent to the RTL.

2- Floor and Power Planning: This step changes depending on whether we are designing a macro (component) in the chip or the whole chip. In chip floor planning,the chip is partitioned between different system building blocks and place the I/O pads. In macro floor planning, the macro floor dimensions pin locations and the rows definition are defined.   
For power p-lanning, the power network is constructed. Chips are usually powered through external sources that connect to the chip through metal straps that use upper thicker metal layer characterized by lower resistance values.

3- Placement:For macros, we place the gate-level entlist cells on the virtual rows. It is obvious that connected components shoud be placed closely to each other to reduce the interconnect delay and to enable successful routing afterwards. Pacement is typically done in 2 steps: global that tries to find optyimal positions for all cells = and these positions might not be legal so cells can overlap or go off rows, and tale placement wehere the global placement is altered to make them all legal.

4- Clock Tree Synthesis: we create a clock distribution network to deliver the clock to all sequential elements with minimum skew.

5- Routing: with components placed, we need now to establish connections between them. Given this placement and a number of metal leyrs, it is required to find a convenient pattern of horizontal and vertical metal lines to implement the nets that connect the cells together. The metal layer characteristics (thickness, pitch, the difference [], and the minumum width) are defined by the PDK. It also defines the vias used to connect different metal layers together. The skypwater PDK defines 6 routing layers. The lowest layer is the local interconnect layer made of titanium nitrite. The following 5 layers are all aluminum layers. For large routing grids, router usually use a divide and conquer method where first the global routing uses coarse-grained grids to generate the routing guides and then detailed routing uses fine-grained grids and the routing guides to implement the actual wiring.

6- Signoff: We can now generate the final layout which is subjectto physical verifications such as design rules checking (DRC) and the layout 
vs schematic (LVS) to make sure that the generated layout matches the gate-level netlist we started with. Finally, we need to run timing verification which is done though static timing analysis to check that timing constraints are met and the circuit can run at the designated frequency.
</details>

<details>
<summary>OpenLANE Open Source tools</summary>

Going through this process is harder using open source tools as you need tools qualification, calibration, and missing tools. For eFabless, the reference open source implementation flow which is called OpenLANE. The goal is to produce a clean GDSII with no human intervention (no human-in-the-loop) which means its has no LVS violations, no DRC violations, and hopefully no timing violations. This flow is tuned for the SkyWater 130nm Open PDK. It can be used to harden macros and chips. It has 2 modes of operation: autonomous (with 1 push button get the GDSII) or interactive (run commands one by one and check the results of each step). It has design space exploration feature which can be used to find the best set of flow configurations. It comes with a large number of design examples on the public github repo.

The OpenLANE ASIC flow starts with RTL Synthesis using yosys and abc (offers strategies that optimize for area and others for timing) followed by static timing analysis using OpenSTA. The synthesis exploration allows us to generate report to check how the design delay and area are affected by the synthesis strategies and pick the best one. OpenLANE also has design exploration abilities that sweeps the design configurations and generate reports showing design metrics to find the best one. This design exploration can also be used for regression testing.    
After synthesis comes the design for test (DFT) step used for scan insertion, automoatic test pattern generation, test patterns compaction, and fault coverage and simulation. This step is optional and will add logic that can be used for testing later. It will also add digital control which will give access to the internal sketching.    
Nest, there is the physical implementation implemented using the OpenROAD. This tool implements floor and power planning , end decoupling capacitors and Tap cells insertion, global and detailed placement, post-placement optimization, clock tree synthesis, and global and detailed routing.   
Next, logic equivalence checking should be done using yosys since in the previous steps we applied some optimiations which change the gate-level netlist and thus we need to compare that the 2 versions are functionally equivalent.   
The next step is fake antenna diodes insertion which is required to address the antenna rules violations (when a metal wire segment is fabricated and is long enough to act as an antenna, it can collect charges and damage transistors connected to it during fabrication).IF the router fails to eliminate these violations, there are 2 solutions: bridging which attaches a higher layer intermediary or adding antenna diode cells to leak away the charges and are provided by the standard cell library. In OpenLANE, fake antenna diode cells tjhat amtch the footprints of real ones are added to the SCL and used after cell input after placement. An Antenna Checker is then run and if it reports a violation on the cell input pin, the fake diode is replaced by a real one.    
Finally, the sign-off includes STA, design rule checking and layout vs schematic. Timing reports are generated to check for any timing violations.
</details>

<details>
<summary>OpenLANE and tool installation</summary>

To install the needed tools for OpenLANE, I used the following script:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
```

After the reboot, I revalidated the correctness of my installation using:

```
docker run hello-world
```

And checked the dependencies using:

```
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h
```

And finally I installed theOpenLANE tools and the PDKs:

```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

The following screenshot shows the end of the installation process:

![d17 openlane installation](https://github.com/walaa-amer/VSD-HDP/assets/85279771/82c1abe9-3a18-4425-8333-f15aef5cd0e0)


</details>

<details>
<summary>picorv32a using OpenLANE</summary>

I invoked the OpenLANE tool in the OpenLANE directoy using:

```
make mount
```

Then I ran the flow eith the -interactive flag (to diable automatic mode) using:

```
./flow.tcl -interactive
```

In the new prompt I ran:

```
package require openlane 0.9
```

All of the designs are stored in the /designs directory. If we want to add our own design add a folder for it there. In that folder we add the src files in a "\src" file and the "config.tcl file". This file will overwrite the OpenLANE default configurations. Another "sky130A_sky130_fd_sc_hd__config.tcl" file would overwrite the changes in "config.tcl" (it is the highest level config file but not essential for every design).

We can find a list of the configuration switches (variables) for each stage of the flow in the README file in the configuration folder.

Before running the synthesis and beginning to run the flow, we need to setup the design using:

```
prep -design picorv32a
```

This step will first merge the 2 LEF files (the cell specific one and the technology/layers one). It will create a new runs folder in our design folder and in that run it will create another folder specific to this run with all of the needed files for each step of the OpenLANE tools and a config.tcl file that will have the final configuration of the design and a cmds file that hass the commands ran. The changes in the flow will be reflected in the config.tcl file immediately after running them.

For more info abut the OpenLANE tools: Youtube: fossi dial up

The results of the synthesis can be found in the /log/synthesis/1-synthesis.log file or in the reports/synthesis/1-synthesis.AREA_0.stat.rpt file.   
To find the flop ratio, we divide the number of dff cells by the total number of cells as seen in the screenshot below:

![d17 openlane synth nbr cells](https://github.com/walaa-amer/VSD-HDP/assets/85279771/51c8e0cc-2da9-4dd5-84e7-fcf869a0d0d9)

In this case the ratio is: 1596/10104*100 ~ 15.8%

</details>
</details>

## Day 18

<details>
<summary>Floor plan</summary>

<details>
<summary>Floor planning considerations</summary>

### Defining the width and height of core and die

To find W and H, we need to find the dimensions of the standard cells and flipflops used in the netlist. Let's assume the following netlist:

![d18 circuit example](https://github.com/walaa-amer/VSD-HDP/assets/85279771/901f9781-d066-402a-b596-e1089803aa26)

Placed next to each other, the different components of the netlist have a total width of 2 units and height of 2 units so a total area of 4 units.

On a wafer, several dies exist. Each die has a core where the logic of the netlist is implemented.    
Utilization factor of the core = (area occupied by netlist)/(total area of the core)
Apect ratio of the core = height/width (characterizes the shape of the core)

### Defining locations of preplaced cells

A part of the netlist might be reused several times. The best way t implement thoseis by black boxing them and using them as block modules. These modules have fixed locations in the chip decided before placement thus called preplaced cells. These locations will not be modified by the tool which will only automate placement and routing for other components on the chip.

### Surround pre-placed cells with decoupling capacitors

One issue that might be faced is the voltage drop from the voltage source to a gate in the circuit that is switching from 0 to 1. This voltage drop will be compensated by a decoupling capacior, which is a huge capacitor that decouples parts of the citcuit from the source since it becomes the vdd source for the circuit and providesthe needed current for switchng as shown below:

![d18 decoupling cap](https://github.com/walaa-amer/VSD-HDP/assets/85279771/08a265be-c746-4cab-8f54-6d519df9b145)


### Power planning

Assuming several gates in the circuit are switching from low to high, the decoupling capacitor needs to provide all of them with the needed current which could lead to a voltage droop. The same case applies for the ground where several gates are discharging to the same line causing a ground bounce shown in the figure below:

![d18 ground bounce](https://github.com/walaa-amer/VSD-HDP/assets/85279771/1cdaa34e-5453-4a6e-969e-e205edc01464)


To solve this problem, a power plan is used to distribute several Vdd and Vss pins across the core as shown below:

![d18 power planning](https://github.com/walaa-amer/VSD-HDP/assets/85279771/df57013d-50e7-46ea-9cdb-267724e141af)


### Pin placement

Pin placement is highly depenedent on the logic design inside the chip. Cells are put in proximity of their input pins. Clock pins are larger as they drive many cells.

### Logical cell placemnt blockage

The area between the core and the die are blocked for the tool to avoid placement and routing in those areas.
</details>

<details>
<summary>Floor planning using OpenLANE</summary>

The parameters set for the floopplan stage are found in the floorplan.tcl file in the configuration folder. This is the lowest priority config file. These parameters are overwritten in the config.tcl file in the design folder that we are working in.

To run the floor plan:

```
run_floorplan
```

To check the results, we open the ".def" file in the "/results/floorplan" folder. This file will give us the area of the die by specifying the coordinates of the lower left point and the upper right point of the die using the units specified right above the coordinate statement. This file will also show where each component is placed on the die.

To visualize the placement, we use magic:

```
magic -T <Tech_file_path> lef read <merged.lef_file_path def read <design_def_file_path> &
```

In the picorv32a case, this command in the floorplan result folder would be:

```
magic -T /<open_pdks_path>/open_pdks/sky130/magic/sky130.tech lef read ../../merged.nom.lef def read picorv32a.def &
```

I downloaded the pdk files from the folowing open_pdks github repo: https://github.com/RTimothyEdwards/open_pdks/tree/master/sky130.

The floorplan is show below:
![d18 floorplan magic](https://github.com/walaa-amer/VSD-HDP/assets/85279771/748a92d1-446d-4410-bee3-f0686c611ba2)



</details>
</details>

<details>
<summary>Placement</summary>

We now have the floorplan  with the preplacements and have the physical view of the circuit from the synthesis. We need now to place these components on the floorplan. The components would be placed as close as possible to other components they communicate with or to the die pins they connect to. The placement is not allowed in the preplaced areas occupied by preplaced blocks and decouplig capacitors.

The next step would be to optimize the placement based on estimation of wire lengths and capacitances. Intermediate components are added in a long path to conserve the signal and not lose it to large capacitance. These components are called repeaters or buffers. The results would look as follows:

![d18 optimzed placement](https://github.com/walaa-amer/VSD-HDP/assets/85279771/11bbc9d3-616f-407f-97fc-ebf46f3891df)


<details>
<summary>Placement using OpenLANE</summary>

There are 2 levels of placement: global and detailed. Detailed placement legalizes the global placement.

To run global placement:

```
run_placement
```

The visualization of the placement is also done through magic in the placement result folder:

```
magic -T /<open_pdks_path>/open_pdks/sky130/magic/sky130.tech lef read ../../merged.nom.lef def read picorv32.def &
```

Showing the following result:
![d18 magic placement](https://github.com/walaa-amer/VSD-HDP/assets/85279771/da6ad11c-0aff-4920-bd9c-2bebadc0b734)

Zooming in shows how the components are placed between the rows:
![d18 placement magic zoom](https://github.com/walaa-amer/VSD-HDP/assets/85279771/422388f5-228f-4fb1-a5d3-16a759a8e265)



</details>
</details>
