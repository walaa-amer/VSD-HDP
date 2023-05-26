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
 
 
We can see that since the inputs are 8-bit long, the output will be 9 bits. Firet we instantiate the full adder 8 times and then the connections are made for the ripple effect.

The variable used for the for generate is 'genvar' and not an integer. The simulation shows the expected result of the RCA:


The simulation of the synthesized designs also show the same results:
//fix error in netlist


</details>
</details>

