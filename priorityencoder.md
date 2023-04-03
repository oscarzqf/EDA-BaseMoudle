priorityencoder

1.原理

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664179964141.jpg)

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664179935603.jpg)

2.源码

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity priorityencoder is
--  Port ( );
port(
input:in std_logic_vector(7 downto 0);
y:out std_logic_vector(2 downto 0)
);
end priorityencoder;

architecture Behavioral of priorityencoder is

begin
process(input)
begin
if input(0)='0' then --if语句实现优先级，input(0)优先级最高
y<="111";
elsif input(1)='0' then
y<="110";
elsif input(2)='0' then
y<="101";
elsif input(3)='0' then
y<="100";
elsif input(4)='0' then
y<="011";
elsif input(5)='0' then
y<="010";
elsif input(6)='0' then
y<="001";
else 
y<="000";
end if;
end process;
end Behavioral;

```

3.testbench

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;



entity encoderTestbench is
--  Port ( );
end encoderTestbench;

architecture Behavioral of encoderTestbench is
component priorityencoder
port(
input:in std_logic_vector(7 downto 0);
y:out std_logic_vector(2 downto 0)
);
end component;
signal input: std_logic_vector(7 downto 0):="11111110";
signal y: std_logic_vector(2 downto 0):="111";
begin
u0: priorityencoder port map(
input=>input,
y=>y
);
process
begin
wait for 20ns;--每20ns加1，溢出后循环
input<= to_stdlogicvector(to_bitvector(input) rol 1);--循环左移运算
end process;

end Behavioral;

```

4.rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220925203032.png)

5.仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220925202954.png)