D_T_RS_jk触发器

1.一般D触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
--一般D触发器
entity d_flip_flop is
port(
d: in std_logic;
clk: in std_logic;
q: out std_logic
);
end d_flip_flop;

architecture Behavioral of d_flip_flop is
begin
process(clk)
begin
if(clk'event and clk='1') then
q<=d;
end if;
end process;
end Behavioral;

```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220928193152.png)

2.异步复位的D触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity d_flip_flop_syn1 is
--  Port ( );
port(
d,clr,clk: in std_logic;
q,qn: out std_logic
);
end d_flip_flop_syn1;

architecture Behavioral of d_flip_flop_syn1 is
begin
process(clk,clr)
begin
if clr='0' then
    q<='0';
    qn<='1';
    elsif (clk'event and clk='1') then
    q<=d;
    qn<=not d;
end if;
end process;

end Behavioral;

```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220928194045.png)

3.异步复位/置位的D触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity d_flip_flop_syc2 is
--  Port ( );
port(
d,clr,clk,set: in std_logic;
q: out std_logic
);
end d_flip_flop_syc2;

architecture Behavioral of d_flip_flop_syc2 is
begin
process(clk,set,clr)
begin
if set='0' then
    q<='1';
    elsif clr='0' then
    q<='0';
    elsif (clk'event and clk='1') then
    q<=d; 
end if;
end process;

end Behavioral;

```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220928194833.png)

4.同步复位D触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity d_flip_flop_synch is
--  Port ( );
port(
d,clr,clk: in std_logic;
q: out std_logic
);
end d_flip_flop_synch;

architecture Behavioral of d_flip_flop_synch is
begin
process(clk)
begin
if(clk'event and clk='1') then
    if clr='0' then
        q<='0';
    else 
        q<=d;
    end if;
end if;
end process;

end Behavioral;

```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220928195542.png)

5.带使能端T触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity t_flip_flop is
--  Port ( );
port(
clk,t: in std_logic;
q: out std_logic
);
end t_flip_flop;

architecture Behavioral of t_flip_flop is
signal q_tmp: std_logic :='0';
begin
process(clk)
begin
if (clk'event and clk='1') then
    if t='1' then 
        q_tmp<=not q_tmp;
     else
        q_tmp<=q_tmp;
     end if;
end if;
end process;
q<=q_tmp;
end Behavioral;

```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220928202338.png)

6.rs触发器

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity rs_flip_flop is
--  Port ( );
port(
r,s: in std_logic;
q,qn:out std_logic 
);
end rs_flip_flop;
architecture Behavioral of rs_flip_flop is
signal q_tmp,qn_tmp: std_logic :='0';
begin
    q_tmp<=qn_tmp nor r;
    qn_tmp<=q_tmp nor s;
    q<=q_tmp;
    qn<=qn_tmp;
end Behavioral;
```

![](https://cdn.jsdelivr.net/gh/oscarzqf/typoraPictures/20220929091431.png)

|  S   |  R   |  q   |
| :--: | :--: | :--: |
|  0   |  0   | 保持 |
|  0   |  1   |  0   |
|  1   |  0   |  1   |
|  1   |  1   | 未定 |

*sr="11"是不允许的，sr再同时为0时会使电路进入亚稳态，对噪声敏感，输出不确定。

7.边沿触发jk触发器

|  J   |  K   |  CLK   |   Q    |   QN   |
| :--: | :--: | :----: | :----: | :----: |
|  x   |  x   |   0    | 上个Q  | 上个QN |
|  x   |  x   |   1    | 上个Q  | 上个QN |
|  0   |  0   | 上升沿 | 上个Q  | 上个QN |
|  0   |  1   | 上升沿 |   0    |   1    |
|  1   |  0   | 上升沿 |   1    |   0    |
|  1   |  1   | 上升沿 | 上个QN | 上个Q  |

特征方程：Q*=J.Q'+K'Q

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity jk_flip_flop is
--  Port ( );
port(
j,k: in std_logic;
clk: in std_logic;
q,qn: out std_logic
);
end jk_flip_flop;

architecture Behavioral of jk_flip_flop is
signal q_tmp: std_logic:='0';
signal qn_tmp: std_logic:='1';
begin
process(clk)
begin
if (clk'event and clk='1') then
    q_tmp<=(j and qn_tmp) or (not k and q_tmp);
    qn_tmp<=not((j and qn_tmp) or (not k and q_tmp));
end if;
end process;
q<=q_tmp;
qn<=qn_tmp;
end Behavioral;
```

