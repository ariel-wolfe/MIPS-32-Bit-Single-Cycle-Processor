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

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.NUMERIC_STD.ALL;

entity REGFILE IS
  port(
       clock : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
       read_reg1 : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
       read_reg2 : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
       write_reg : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
       write_data : IN STD_LOGIC_VECTOR (31 DOWNTO 0);
       reg_write : IN STD_LOGIC;
       read_data1 : OUT STD_LOGIC_VECTOR (31 DOWNTO 0);
       read_data2 : OUT STD_LOGIC_VECTOR (31 DOWNTO 0));
end REGFILE;

architecture behavior of REGFILE IS
TYPE reg_file_type IS array(0 to 3) OF STD_LOGIC_VECTOR (31 DOWNTO 0);
signal array_reg : reg_file_type := (X"00000000",
                                     X"11111111",
                                     X"22222222",
                                     X"33333333");
begin
process(clock, reg_write) THEN 
begin
  if RISING_EDGE(clock) THEN
    if (reg_write = '1') THEN
      array_reg (TO_INTEGER(UNSIGNED(write_reg))) <= write_data;
    end if;
  end if;
end process;

read_data1 <= array_reg (TO_INTEGER(UNSIGNED(read_reg1)));
read_data2 <= array_reg (TO_INTEGER(UNSIGNED(read_reg2)));

end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/217319140-12a5187c-da3a-421a-bfaf-45cca7b157f5.png)

Figure 6. VHDL and Timing Simulation of Register File.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity ALU_CONTROL IS
port(INPUT : IN STD_LOGIC_VECTOR(5 DOWNTO 0);
     OPCODE : IN STD_LOGIC_VECTOR(1 DOWNTO 0);
     OUTPUT : OUT STD_LOGIC_VECTOR(3 DOWNTO 0));
end ALU_CONTROL;

architecture behavior of ALU_CONTROL IS
begin
process(OPCODE, INPUT)
begin
case(OPCODE) IS
  WHEN "00" => OUTPUT <= "1000"; --ADD COMMAND
  WHEN "01" => OUTPUT <= "0110"; --SUB COMMAND
  WHEN "10" => CASE (INPUT) IS --R TYPE COMMANDS
               WHEN "100000" => OUTPUT <= "1000";
               WHEN "100010" => OUTPUT <= "0110";
               WHEN "100100" => OUTPUT <= "0010";
               WHEN "100101" => OUTPUT <= "0100";
               WHEN "101010" => OUTPUT <= "1001";
               WHEN OTHERS => OUTPUT <= "0000";
               END CASE;
  WHEN OTHERS => OUTPUT <= "1000";
  END CASE;
  END PROCESS;
END behavior;
```
![image](https://user-images.githubusercontent.com/124304251/217355229-1da8449e-21f2-4157-aa4c-42357362d062.png)
![image](https://user-images.githubusercontent.com/124304251/217355253-eeb560f3-8fa1-463e-8b23-c36da5fe1306.png)
![image](https://user-images.githubusercontent.com/124304251/217355281-120ef7d9-ddec-4907-9fa9-a1dc1f611982.png)
![image](https://user-images.githubusercontent.com/124304251/217355299-53b04d7c-0cc1-4739-a54f-8f7a6db3109e.png)

Figure 7. VHDL and Timing Simulation of ALU Control Unit.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.NUMERIC_STD.ALL;

entity IM IS
port(CLOCK : IN STD_LOGIC;
     READADD : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     INSTOUT : OUT STD_LOGIC_VECTOR(31 DOWNTO 0));
end IM;

architecture behavioral of IM IS
type RAM_IM IS ARRAY(0 TO 3) OF STD_LOGIC_VECTOR(31 DOWNTO 0);
SIGNAL InstMem : RAM_IM := (X"01285024", --AND t2, t1, t0
                            X"018B6825", --OR t%, t4, t3
                            X"01285020", --AND t2, t1, t0
                            X"2108000A"); --addi t0, t0, l0
begin
process(CLOCK)
begin
if RISING_EDGE(CLOCK) THEN
  INSTOUT <= INSTMEM((TO_INTEGER(UNSIGNED(READADD))));
end if;
end process;
end behavioral;
```
![image](https://user-images.githubusercontent.com/124304251/217357003-ab89fa1f-b182-4c2b-96eb-8cc520a3e97b.png)

Figure 8. VHDL and Timing Simulation for Instruction Memory.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.NUMERIC_STD.ALL;

entity PC IS
port(CLOCK : IN STD_LOGIC;
     PCADD : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     IMADD : OUT STD_LOGIC_VECTOR(31 DOWNTO 0));
end PC;

architecture behavioral of PC IS
signal X : STD_LOGIC := '0';
begin
process(CLOCK, PCADD, X)
begin
if RISING_EDGE(CLOCK) THEN
  if X = '0' THEN
    IMADD <= X"00000000";
    X <= '1';
    ELSEIF (X = '1') THEN
      IMADD <= PCADD;
    END IF;
end if;
end process;
end behavioral;
```
![image](https://user-images.githubusercontent.com/124304251/217358058-dd3d975d-3fea-423f-890d-b4bad3b31a53.png)

Figure 9. VHDL and Timing Simulation for PC.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity DM IS
port(clock : IN STD_LOGIC;
     address : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     write_data : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     mem_read : IN STD_LOGIC;
     mem_write : IN STD_LOGIC;
     read_data : OUT STD_LOGIC_VECTOR(31 DOWNTO 0));
end DM;

architecture behavioral of DM IS
type RAM_DM IS array(0 to 4 -1) of STD_LOGIC_VECTOR(31 DOWNTO 0);
signal DM : RAM_DM := (X"00000000",
                       X"00000001",
                       X"00000005"
                       X"00000000");
begin
process(mem_write, mem_read, clock)
begin
if RISING_EDGE(clock) THEN
  if(mem_write = '1') THEN
    DM((TO_INTEGER(UNSIGNED(address)))) <= write_data;
  end if;
end if;
end process;
end behavioral;
```
![image](https://user-images.githubusercontent.com/124304251/220734997-eb30df00-2cb5-4f87-82bf-66d797a4b5ef.png)

Figure 10. VHDL and Timing Simulation for Data Memory.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity ALU IS
Port(A, B : in STD_LOGIC_VECTOR (31 downto 0);
	OPCODE : in STD_LOGIC_VECTOR (3 downto 0);
	R : out STD_LOGIC_VECTOR (31 downto 0));
Zero : out stdlogic
	end ALU;
	
architecture behavior of ALU IS

begin
	
process (A, B, OPCODE) IS
	begin
	case OPCODE IS
	-- Logical Operations
	when "0000" =>
	R <= NOT(A);
	
	when "0001" =>
	R <= NOT(B);
	
	when "0010" =>
	R <= A AND B;
	
	when "0011" =>
	R <= A NAND B;
	
	when "0100" =>
	R <= A OR B;
	
	when "0101" =>
	R <= A NOR B;
	
	when "0110" =>
	R <= A XOR B;
	
	when "0111" =>
	R <= A XNOR B;
	
	-- Arithmetic Operations
	when "1000" =>
	R <= A + B;
	
	when "1001" => 
	if A < B then
	R <= "00000000000000000000000000000001";
	else
	R <= "00000000000000000000000000000000";
	end if;
	
	when "1010" =>
	R <= A + "00000000000000000000000000000001";
	
	when "1011" =>
	R <= A - "00000000000000000000000000000001";
	
	when "1100" =>
	R <= B + "00000000000000000000000000000001";
	
	when "1101" =>
	R <= B - "00000000000000000000000000000001";
	
	when "1110" =>
	R <= NOT(A) + "00000000000000000000000000000001";
	
	when "1111" =>
	R <= NOT(B) + "00000000000000000000000000000001";


	
	end case;
end process;
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/220741536-2c342602-2b50-45a1-a8ef-1d77105fad64.png)
![image](https://user-images.githubusercontent.com/124304251/220741565-62887510-1027-4838-b071-018815e069ef.png)
![image](https://user-images.githubusercontent.com/124304251/220741585-10ccf464-11d8-44b7-a8fd-9e2541244356.png)

Figure 11. VHDL and Timing Simulation of ALU.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;
use IEEE.NUMERIC_STD.all;

entity Main_Control IS
port( OPCODE : IN STD_LOGIC_VECTOR(5 DOWNTO 0);
	  REGDEST : OUT STD_LOGIC;
	  MEMREAD : OUT STD_LOGIC;
	  MEMWRITE : OUT STD_LOGIC;
	  MEMTOREG : OUT STD_LOGIC;
	  ALUOP : OUT STD_LOGIC_VECTOR(1 DOWNTO 0);
	  ALUSRC : OUT STD_LOGIC;
	  BRANCH : OUT STD_LOGIC;
	  REGWRITE : OUT STD_LOGIC);
end Main_Control;

architecture behavioral of Main_Control IS
begin
process(OPCODE)
begin
REGWRITE <= '0';
case OPCODE IS
WHEN "000000" =>		-- ALL R-TYPE
	REGDEST <= '0';
	MEMREAD <= '0';
	MEMWRITE <= '0';
	MEMTOREG <= '0';
	ALUOP <= "10";
	ALUSRC <= '0';
	REGWRITE <= '1' AFTER 10ns;
	BRANCH <= '0';
	
WHEN "100011" =>		-- FOR LW INTRUCTIONS
	REGDEST <= '0';
	MEMREAD <= '1';
	MEMWRITE <= '0';
	MEMTOREG <= '1';
	ALUOP <= "00";
	ALUSRC <= '1';
	REGWRITE <= '1' AFTER 10ns;
	BRANCH <= '0';
	
WHEN "101011" =>		--FOR SW INSTRUCTIONS
	REGDEST <= '1';
	MEMREAD <= '0';
	MEMWRITE <= '1';
	MEMTOREG <= '0';
	ALUOP <= "00";
	ALUSRC <= '1';
	REGWRITE <= '0' AFTER 10ns;
	BRANCH <= '0';
	
WHEN "000100" =>		--FOR BEQ INSTRUCTIONS
	REGDEST <= '0';
	MEMREAD <= '0';
	MEMWRITE <= '0';
	MEMTOREG <= '0';
	ALUOP <= "10";
	ALUSRC <= '0';
	REGWRITE <= '0' AFTER 10ns;
	BRANCH <= '1';
	
WHEN OTHERS => NULL;
	REGDEST <= '0';
	MEMREAD <= '0';
	MEMWRITE <= '0';
	MEMTOREG <= '0';
	ALUOP <= "00";
	ALUSRC <= '0';
	REGWRITE <= '0';
	BRANCH <= '0';
	
END CASE;
END PROCESS;
END BEHAVIORAL;
```
![image](https://user-images.githubusercontent.com/124304251/220741783-7d27e3b1-5e67-46d3-8c80-733cab35251f.png)
![image](https://user-images.githubusercontent.com/124304251/220741804-2674fb0f-9bb9-4671-9935-aa816b8b466b.png)
![image](https://user-images.githubusercontent.com/124304251/220741837-abbdc91d-48f4-4344-ad9e-ee46dfcca546.png)
![image](https://user-images.githubusercontent.com/124304251/220741876-ed07a7d4-5377-4952-9fe0-e084ecb62195.png)
![image](https://user-images.githubusercontent.com/124304251/220741961-c1c0be2d-aa65-4d77-b65a-c40b56bdeecf.png)

Figure 12. VHDL and Timing Simulation for Main Control Unit.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity adder IS
port(A, B : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     R : OUT STD_LOGIC_VECTOR(31 DOWNTO 0));
end adder;

architecture behavior of adder IS
begin
  R <= A + B;
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/220742670-2ffa1e97-6b29-4864-b0a6-1592d27adab1.png)
![image](https://user-images.githubusercontent.com/124304251/220742696-772e2c88-f487-4c94-8709-857eb8bb38a1.png)

Figure 13. VHDL and Timing Simulation for Adder.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity add4 IS
port(A : IN STD_LOGIC_VECTOR(31 DOWNTO 0);
     R : OUT STD_LOGIC_VECTOR(31 DOWNTO 0));
end add4;

architecture add4 IS
begin
	R <= A + "X0100";
end behavior;
```
![image](https://user-images.githubusercontent.com/124304251/220743413-ae98ea46-113e-44d9-82c1-8bf2472d7113.png)
![image](https://user-images.githubusercontent.com/124304251/220743477-124917c1-c517-4eba-814a-1be1c7eb25be.png)

Figure 14. VHDL and Timing Simulation for Add4.
