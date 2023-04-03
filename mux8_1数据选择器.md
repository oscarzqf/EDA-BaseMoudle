mux8_1数据选择器

1.源码

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity mux8_1 is
port(
data_in: in std_logic_vector(7 downto 0);
sel1,sel2,sel3: in std_logic;
d: out std_logic
);
end mux8_1;
--when else 语句描述
architecture mux8_1_arc1 of mux8_1 is
signal sel: std_logic_vector(2 downto 0);
begin
sel<=sel3&sel2&sel1;
d<= data_in(0) when sel="000" else
    data_in(1) when sel="001" else
    data_in(2) when sel="010" else
    data_in(3) when sel="011" else
    data_in(4) when sel="100" else
    data_in(5) when sel="101" else
    data_in(6) when sel="110" else
    data_in(7) when sel="111" else
    'X';
end mux8_1_arc1;
--with...select描述
architecture mux8_1_arc2 of mux8_1 is  
signal sel: std_logic_vector(2 downto 0);
begin
sel<=sel3&sel2&sel1;
with sel select
d<=data_in(0) when "000",
  data_in(1) when "001",
  data_in(2) when "010",
  data_in(3) when "011",
  data_in(4) when "100",
   data_in(5) when "101",
  data_in(6) when "110",
  data_in(7) when "111",
  'X' when others;
end mux8_1_arc2;      
--配置结构体
configuration mux8_1_con of mux8_1 is
for mux8_1_arc1 --结构体选择
end for;
end mux8_1_con;
```

2.testbench

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
entity mux8_testbench is
end mux8_testbench;
architecture Behavioral of mux8_testbench is
component mux8_1
port(
data_in: in std_logic_vector(7 downto 0);
sel1,sel2,sel3: in std_logic;
d: out std_logic
);
end component;
signal data_in: std_logic_vector(7 downto 0):="10101010";
signal sel: std_logic_vector(2 downto 0):="000";
signal d: std_logic:='0';
begin
u0: mux8_1 port map(
data_in=>data_in,
sel1=>sel(0),
sel2=>sel(1),
sel3=>sel(2),
d=>d
);
--端口选择
process
begin
wait for 20ns;
sel<=sel+1;
end process;
--数据变化
process
begin
wait for 160ns;
data_in<=to_stdlogicvector(to_bitvector(data_in) rol 1);--循环左移
end process;
end Behavioral;
```

3.rtl（两种语句结果一致）

<img src="https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220927194658.png" style="zoom: 25%;" />

4.仿真结果（两种语句结果一致）

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220927194608.png)

