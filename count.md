count

##### 同步计数器，触发器在同一时钟clk下同时变化

1.异步复位、同步使能的四进制加‘1’同步计数器

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/qq_pic_merged_1664716701820.jpg)

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity counter10en is
--  Port ( );
port(
clr,clk,en : in std_logic;
qa,qb,qc,qd: out std_logic
);
end counter10en;

architecture Behavioral of counter10en is
signal out_tmp : std_logic_vector(3 downto 0);
begin
qa<=out_tmp(0);
qb<=out_tmp(1);
qc<=out_tmp(2);
qd<=out_tmp(3);
process(clr,clk)
begin
if clr='1' then
    out_tmp<="0000";
elsif clk'event and clk='1' then
    if en='1' then
        if out_tmp="1001" then
            out_tmp<="0000";
        else 
            out_tmp<=out_tmp+1;
        end if;
    end if;
end if;
end process;
end Behavioral;
```

testbench

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity counter10en_testbench is
--  Port ( );
end counter10en_testbench;

architecture Behavioral of counter10en_testbench is
component counter10en
port(
clr,clk,en : in std_logic;
qa,qb,qc,qd: out std_logic
);
end component;
signal clr,clk: std_logic:='0';
signal en: std_logic:='1';
signal q: std_logic_vector(3 downto 0):="0000";
begin
u0: counter10en port map(
clr=>clr,
clk=>clk,
en=>en,
qa=>q(0),
qb=>q(1),
qc=>q(2),
qd=>q(3)
);
process
begin
wait for 10ns;
clk<=not clk;
end process;

process
begin
clr<='1';
wait for 20ns;
clr<='0';
wait for 220ns;
en<='0';
wait for 100ns;
clr<='1';
wait;
end process;
end Behavioral;
```

rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20221002211553.png)

仿真结果

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20221002211204.png)



2.同步可逆计数器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
entity counterup_down is
--  Port ( );
port(
clr,clk,updown: in std_logic;
qa,qb,qc,qd: out std_logic
);
end counterup_down;

architecture Behavioral of counterup_down is
signal count4 : std_logic_vector(3 downto 0);
begin
qa<=count4(0);
qb<=count4(1);
qc<=count4(2);
qd<=count4(3);
process(clr ,clk)
begin
 if clr='1' then
    count4<="0000";
 elsif clk'event and clk='1' then
    if updown='1' then
    count4<=count4+1;--递增计数 ，计数到15回到0
    else 
    count4<=count4-1;--递减计数 ，计数到0回到15
    end if;
 end if;
end process;

end Behavioral;
```

testbench

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity counterup_down_testbench is
--  Port ( );
end counterup_down_testbench;

architecture Behavioral of counterup_down_testbench is
component counterup_down
port(
clr,clk,updown: in std_logic;
qa,qb,qc,qd: out std_logic
);
end component;
signal clr: std_logic :='0';
signal clk: std_logic:='0';
signal updown: std_logic :='1';
signal q: std_logic_vector(3 downto 0):= "0000";
begin
u0: counterup_down port map(
clr=>clr,
clk=>clk,
updown=>updown,
qa=>q(0),
qb=>q(1),
qc=>q(2),
qd=>q(3)
);
process
begin
wait for 10ns;
clk<=not clk;
end process;

process
begin
clr<='1';
wait for 10ns;
clr<='0';
wait for 350ns;
updown<='0';
wait;
wait;
end process;
end Behavioral;
```

rtl

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20221002215026.png)

仿真截图

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20221002214947.png)



##### 异步计数器是指构成计数器的低位计数触发器输出作为相邻高位触发器的时钟输入





分频器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

--占空比为50%的奇数分频器，以下为11分频
entity div7 is
--  Port ( );
port(
clk:in std_logic;
clk7: out std_logic
);
end div7;

architecture Behavioral of div7 is
signal cnta,cntb: std_logic_vector( 3 downto 0):="0000";
signal clka,clkb: std_logic:='0';
begin
clk7<=clka or clkb;
process(clk)
begin
if (clk'event and clk='1') then
    if(cnta="1010") then
    cnta<="0000";
    else
    cnta<=cnta+1;
    end if;
    if(cnta<"0101") then
    clka<='1';
    else
    clka<='0';
    end if;
    elsif (clk'event and clk='0')then
        if(cntb="1010") then
    cntb<="0000";
    else
    cntb<=cnta+1;
    end if;
    if(cntb<"0101") then
    clkb<='1';
    else
    clkb<='0';
    end if;
end if;    
end process;

end Behavioral;
```



