compare4

1.四位比较器

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664181108318.jpg)

2.源码

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity compare4 is
--  Port ( );
port(
a,b:in std_logic;
GT,EQ,LT:out std_logic
);
end compare4;

architecture compare4_arc1 of compare4 is
begin
process(a,b)
begin
if a>b then
GT<='1';
EQ<='0';
LT<='0';
elsif a=b then
GT<='0';
EQ<='1';
LT<='0';
elsif a<b then
GT<='0';
EQ<='0';
LT<='1';
end if;
end process;
end compare4_arc1;

```

3.testbench

```vhdl



library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity compare4_testbench is
--  Port ( );
end compare4_testbench;

architecture Behavioral of compare4_testbench is
component compare4
port(
a,b:in std_logic;
GT,EQ,LT:out std_logic
);
end component;
signal a,b: std_logic:='0';
signal output: std_logic_vector(2 downto 0):="010";
begin
u0: compare4 port map(
a=>a,
b=>b,
GT=>output(2),
EQ=>output(1),
LT=>output(0)
);
process
begin
wait for 20ns;
a<='1';
b<='0';
wait for 20ns;
a<='1';
b<='1';
wait for 20ns;
a<='0';
b<='1';
end process;
end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926163115.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926163029.png)