full_adder

1.原理

​	全加器除了数位输入X和Y，还有进位输入CIN(来自低位的进位)，三个输入和的范围为0-3,仍然可以用两个输出为表示：S（全加和），COUT(送给高位的进位)

S=X  xor Y xor CIN;

COUT=(X and Y)  or (X and CIN) or (Y and CIN);（最高位进位作为结果的最高位）

2.源码

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity full_adder is
--  Port ( );
port(
x,y,cin: in std_logic;
s,cout: out std_logic
);
end full_adder;

architecture Behavioral of full_adder is
begin
s<=x xor y xor cin;
cout<=(x and y) or (x and cin) or (y and cin);
end Behavioral;

```

3.testbench

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;


entity fulladder_testbench is
--  Port ( );
end fulladder_testbench;

architecture Behavioral of fulladder_testbench is
component full_adder
port(
x,y,cin: in std_logic;
s,cout: out std_logic
);
end component;
signal input : std_logic_vector(2 downto 0):="000";
signal s,cout: std_logic:='0';
begin
u0: full_adder port map(
x=>input(0),
y=>input(1),
cin=>input(2),
s=>s,
cout=>cout
);
process
begin
wait for 20ns;--循环产生8种情况
input<=input+1;
end process;

end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926213220.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220926213052.png)

