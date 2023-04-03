half_adder

1.原理

最简单的加法器叫做半加器，把2个1位二进制操作数x和y相加，产生一个2位和，和的范围为0-2，和的低位命名为HS(半加和)，较高位命名为CO(半加进位或进位输出)。

HS=X xor Y=(X and Y) or (not x and y)

CO=X and Y

2.源码

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity half_adder is
--  Port ( );
port(
x,y:in std_logic;
hs,co:out std_logic
);
end half_adder;

architecture Behavioral of half_adder is
begin
hs<=x xor y;
co<=x and y;
end Behavioral;

```

3.testbench

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity halfadder_testbench is
--  Port ( );
end halfadder_testbench;

architecture Behavioral of halfadder_testbench is
component half_adder
port(
x,y:in std_logic;
hs,co:out std_logic
);
end component;
signal x,y: std_logic:='0';
signal hs,co: std_logic:='0';
begin
u0: half_adder port map(
x=>x,
y=>y,
co=>co,
hs=>hs
);
process
begin
wait for 20ns;--循环出现各种情况
x<='0';
y<='1';
wait for 20ns;
x<='1';
y<='0';
wait for 20ns;
x<='1';
y<='1';
end process;
end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926211547.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926210745.png)