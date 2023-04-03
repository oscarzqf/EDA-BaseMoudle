mux4

1.多路选择器，四选一选择器

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664179509541.jpg)

2.源码

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity mux4 is
--  Port ( );
port(
a,b:in std_logic;
input:in std_logic_vector(3 downto 0);
y:out std_logic
);
end mux4;

architecture mux4_arc1 of mux4 is
signal sel: std_logic_vector(1 downto 0);
begin
sel<=a&b;
process(sel,input)
begin
if sel="00" then
y<=input(0);
elsif sel="01" then
y<=input(1);
elsif sel="10" then
y<=input(2);
elsif sel="11" then
y<=input(3);
else
y<='Z';
end if;
end process;
end mux4_arc1;

```



3.testbench

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity mux4_testbench is
--  Port ( );
end mux4_testbench;

architecture Behavioral of mux4_testbench is
component mux4
port(
a,b:in std_logic;
input:in std_logic_vector(3 downto 0);
y:out std_logic
);
end component;
signal sel: std_logic_vector(1 downto 0):="00";
signal input: std_logic_vector(3 downto 0):="0001";
signal y: std_logic :='0';
begin
u0: mux4 port map(
a=>sel(1),
b=>sel(0),
input=>input,
y=>y
);
process
begin
wait for 20ns;
sel<=sel+1;
end process;

process
begin
wait for 80ns;
input<=input+1;
end process;
end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926160307.png)

5.仿真截图

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926160225.png)