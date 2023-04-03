decoder_38

1.原理

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664179831563.jpg)

2.源码

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity decoder_3_8 is
--  Port ( );
port(
g1,g2a,g2b:in std_logic;
a,b,c: in std_logic;
y:out std_logic_vector(7 downto 0)
);
end decoder_3_8;

architecture Behavioral of decoder_3_8 is
signal indata: std_logic_vector(2 downto 0);
begin
indata<=c&b&a;
process(indata,g1,g2a,g2b)
begin
if (g1='1' and g2a='0' and g2b='0') then 
    case indata is
    when "000"=>y<="11111110";
    when "001"=>y<="11111101";
    when "010"=>y<="11111011";
    when "011"=>y<="11110111";
    when "100"=>y<="11101111";
    when "101"=>y<="11011111";
    when "110"=>y<="10111111";
    when "111"=>y<="01111111";
    when others=>y<="11111111";
    end case;
else 
y<="11111111";
end if;
end process;
end Behavioral;

```

3.testbench

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;



entity decoder_testbench is
--  Port ( );
end decoder_testbench;

architecture Behavioral of decoder_testbench is
component decoder_3_8
port(
g1,g2a,g2b:in std_logic;
a,b,c: in std_logic;
y:out std_logic_vector(7 downto 0)
);
end component;
signal g1,g2a,g2b: std_logic :='0';
signal indata : std_logic_vector(2 downto 0):="000";
signal y: std_logic_vector(7 downto 0):="11111111";
begin
u1:decoder_3_8 port map(
g1=>g1,
g2a=>g2a,
g2b=>g2b,
a=>indata(0),
b=>indata(1),
c=>indata(2),
y=>y
);
process
begin
wait for 60ns;
g1<='1';
wait;
end process;

process
begin
wait for 20ns;
indata<=indata+"001";--需要导入use IEEE.STD_LOGIC_UNSIGNED.ALL;
end process;

end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220925175106.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220925175216.png)
