subtracter

1.一位全减器

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664196239502.jpg)

2.源码

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity subtracter is
--  Port ( );
port(
x,y,cin:in std_logic;--被减数x,减数y，借位输入cin
sub,cout:out std_logic
);
end subtracter;

architecture Behavioral of subtracter is
begin
sub<=x xor y xor cin;--差
cout<=(not x and y) or (not x and cin) or (y and cin);--借位输出
end Behavioral;
```

3.testbench

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity subtracter_testbench is
--  Port ( );
end subtracter_testbench;

architecture Behavioral of subtracter_testbench is
component subtracter
port(
x,y,cin:in std_logic;--被减数x,减数y，借位输入cin
sub,cout:out std_logic
);
end component;
signal x,y,cin:std_logic:='0';
signal sub,cout: std_logic:='0';
begin
u0: subtracter port map(
x=>x,
y=>y,
cin=>cin,
sub=>sub,
cout=>cout
);
process
begin
wait for 20ns;--对真值表8中情况循环出现
x<='0';
y<='0';
cin<='0';
wait for 20ns;
x<='0';
y<='0';
cin<='1';
wait for 20ns;
x<='0';
y<='1';
cin<='0';
wait for 20ns;
x<='0';
y<='1';
cin<='1';
wait for 20ns;
x<='1';
y<='0';
cin<='0';
wait for 20ns;
x<='1';
y<='0';
cin<='1';
wait for 20ns;
x<='1';
y<='1';
cin<='0';
wait for 20ns;
x<='1';
y<='1';
cin<='1';
end process;
end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926204300.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926204219.png)