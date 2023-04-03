```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;

entity （测试平台名）is
end ;

architecture Behavioral of （测试平台名）is
signal a:std_logic;--激励变量声明，无需指定端口类型
signal b:std_logic;
...
signal q:std_logic;
constant clk_period:time:=20ns;
component （待测试文件实体名）--声明待测试元件
	port(a: in std_logic;
		  b: in std_logic;
		  q: out std_logic
		 );
end component;

begin
i1:（待测试文件名）	--连接测试文件
	port map(
			a=>a,
			b=>b,
			q=>q
	        );
	        
clk< =not clk after clk_period/2; --产生时钟信号

pr1:process
begin
（产生激励）
end process;
pr2:process
begin
（产生激励）
end process;
end Behavioral;


```

