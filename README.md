# MIPS 32-Bit Single Cycle Processor

## Abstract
Designing a 32-bit MIPS single cycle processor to implement R-type and I-type formats. Each component of the processor was created using Quartus II VHDL software. The processor consisted of a PC, instruction memory, multiplexers, registers, ALUs, data memory, a sign extender, a shift left 2, and control units for##  the ALU and the processor. Each component was emulated and tested for functionality. Once all components were confirmed to function properly, the processor was assembled using a Quartus II block schematic that was made up of block symbols of each component of the full data path. The assembled processor was compiled successfully and verified to work properly. 

## Introduction
Using Altera Quartus to design a 32-bit MIPS single cycle processor. The processor is built to implement R-type and I-type formats for MIPS commands. The full data path consists of a PC, instruction memory, registers, ALUs, multiplexers, data memory, a sign extender, a shift left 2, and controls for the ALU and processor. Each component was designed using Quartus II VHDL and emulated with timing simulations to test for functionality. 

## Methods
The VHDL for each component was written to emulate the component’s functionality to assemble the circuit for the processor. The components were tested using timing simulations to verify the functionality of each component. The assembled processor was built using an Altera Quartus block diagram. The schematic was compiled and tested.

## Results
![image](https://user-images.githubusercontent.com/124304251/216444135-276f1d7f-df2a-4c7e-a913-d890d1f4ea7f.png)

Figure 1. Full Data Path of MIPS Processor.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;
use IEEE.STD_LOGIC_UNSIGNED.all;

entity MUX is
  port(A, B : IN STD_LOGIC_VECTOR (31 downto 0);
       S : IN STD_LOGIC;
       R : OUT STD_LOGIC_VECTOR (31 downto 0));
end MUX;

architecture behavior of MUX IS
begin
  process(A, B, S)
  begin
    if S = '0' then
    R <= A;
    else
    R <= B;
    end if;
  end process;
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/216445059-9ca09cf2-c749-4482-9ddb-c549651daa46.png)
![image](https://user-images.githubusercontent.com/124304251/216445087-62a5b462-6855-4340-9f07-325e3aaa1db1.png)

Figure 2. VHDL and Timing Simulation of 2-1 Multiplexers.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity mux5 IS
  port(A, B : IN STD_LOGIC_VECTOR(4 downto 0);
       S : IN STD_LOGIC;
       R : OUT STD_LOGIC_VECTOR(4 downto 0));
end mux5;

architecture behavior of mux5 IS
begin
  with s select
  R <= A when '0',
       B when '1';
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/216446205-12f0dcf9-524c-487b-8ee1-efd9b7d10d71.png)
![image](https://user-images.githubusercontent.com/124304251/216446234-2289b050-2a9f-4d9b-b1f4-5086e4dabdba.png)

Figure 3. VHDL and Timing Simulation of a 5 Multiplexer

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity SHIFT_LEFT2 IS
  port(A : IN STD_LOGIC_VECTOR (31 downto 0);
       B : OUT STD_LOGIC_VECTOR (31 downto 0));
end SHIFT_LEFT2;

architecture behavior of SHIFT_LEFT2 IS
begin
  B <= A(29 downto 0) & "00";
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/216447063-ad952245-38e1-4473-862b-0f0e012197f2.png)

Figure 4. VHDL and Timing Simulation of Shift Left 2.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity Sign_extend IS
  port(A : IN STD_LOGIC_VECTOR (15 downto 0);
       R : OUT STD_LOGIC_VECTOR (31 downto 0));
end Sign_extend;

architecture behavior of Sign_extend IS
begin
  R <= (X"0000" & A) WHEN A(15) = '0' ELSE
       (X"ffff" & A);
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/216447934-d510fc28-68bc-490f-9140-400583d846c7.png)
![image](https://user-images.githubusercontent.com/124304251/216447976-14c8453e-de25-4763-83be-20aa4e43c25a.png)

Figure 5. VHDL and Timing Simulation for Sign Extender. 

