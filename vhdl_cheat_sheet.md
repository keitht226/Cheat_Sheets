# VHDL Cheat Sheet

## Contents
**[Concurrent Statements](#concurrent-statements)**  
**[Sequential Statements](#sequential-statements)**  
**[Reserved Word](#reserved-words)**  
**[Naming Rules](#naming-rules)**  
**[Objects](#objects)**  
**[Data Types](#data-types)**  
**[Array Aggregate](#array-aggregate)**  
**[Operators](#operators)**  
**[Type Conversion](#type-conversion)**  
**[Synthesis Guidelines](#synthesis-guidelines)**  
**[Common Logic Units](#common-logic-units)**

## Concurrent Statements
### Signal Assignment
```vhdl
signal_name <= value_expression
```
* closed feedback loops are possible (`q <= ((not q) and (not en)) or (d and en)`) but should be avoided completely. Signal assignment statements that contani a closed eedback loop become sensitive to internal propagation delay and may exhibit race or oscillation.  
### Conditional Signal Assignment
* Results in multiple cascading multiplexers
* Better when certain inputs conditions are given preferenctial treatment (such as a priority encoder)
```vhdl
signal_name <= value_expr_1 when boolean_expr_1 else
               value_expr_2 when boolean_expr_2 else
               value_expr_n;
```
### Selected Signal Assignment Statement
* Results in one large multiplexor
* Good for things such as truth tables, decoders, and multiplexors.
```vhdl
with select_expression select
     signal_name <= value_expr1 when choice_1,
                    value_expr2 when choice_2,
                    value_expr_n when choice_n;
```

## Sequential Statements
### Process With a sensitivity list
* Sensitivity list is a list of signals to which the process responds.
* A VHDL process is activated when a signal in the sensitivity list changes in value
* Incomplete sensitivity lists create memory. All input signals are needed to be complete.
```vhdl
process(sensitivity_list)
     declarations;
begin
     sequential statement;
     sequential statement;
end process;
```
### Wait Statement
* Multiple wait statements are allowed, but in synthesis only a few well-defined forms of wait statements can be used, and normally only one wait statement is allowed per process.
* A sensitivity list isn't needed with wait statements, but is clearer and the preferred method
```vhdl
process
begin
     y <= a and b and c;
     wait on a, b, c;
end process;
-- wait on signals;
-- wait until boolean_expression;
-- wait for time_expression;
```

### Sequential Signal Assignment
```vhdl
a,b,c,d,y : std_logic;
-- ...
process(a,b,c,d)
begin
     y <= a or c:
     y <= a and b: -- because this is sequential
     y <= c and d; -- only this line matters
end process;
```

### Variable assignment
* see object section for more details
* best to use signals when possible. Only resort to variables for characterics that cannot be described by signals. 
```vhdl
signal a,b,y: std_logic;

process(a,b)
     variable tmp: std_logic;
begin
     tmp := '0';
     tmp := tmp or a;
     tmp := tmp or b;
     y <= tmp;
end process;
```

### if statement
* Leaving out conditions results in memory.
* Every output must be assigned per condition or else memory is created. 
     * assigning defualt values before the first condition prevents this and is neater.
* Similar to conditional signal assignment
```vhdl
if boolean_expr1 then
     sequential statements;
elsif boolean_expr2 then
     sequential statements;
else
     sequential statemetns;
end if;
```
     
### case statement
* Similar to select signal assignment
* Incomplete case statements are not allowed and will produce an error. 
* Default output values need to be provided or else memory is created. 
```vhdl
case case_expression is
    when choice_1 =>
          sequential statements;
    when choice_2 =>
          sequential statements;
    when others =>
          sequential statements;
end case;
```

### for loop
```vhdl
for index in loop-range loop
     sequential statements;
end loop;
```
#### bitwise xor example
```vhdl
library ieee;
use ieee.std_logic_1164.all;
entity bit_xor is
     port(
          a, b: in std_logic_vector(3 downto 0);
          y: out std_logic_vector93 downto 0)
     );
end bit_xor;

architecture demo_arch of bit_xor is
     constant WIDTH: integer := 4;
begin
     process(a,b)
     begin
          for i in (WIDTH-1) downto 0 loop
               y(i) <= a(i) xor b(i);
          end loop;
     end process;
end demo_arch;
```

## Reserved Words
|   | | | | |
| --- | --- | --- | --- | --- |
| abs | access | after | alias | all |
| and | architecture | array | assert | attribute |
| begin | block | body | buffer | bus |
| case | component | configuration | constant | disconnect |
| downto | else | elsif | end | entity |
| exit | file | for | function | generate |
| generic | guarded | if | impure | in |
| intertial | inout | is | label | library |
| linkage | literal | loop | map | mod |
| nand | new | next | nor | not |
| null | of | on | open | or |
| others | out | package | port | postponed |
| procedure | process | pure | range | record |
| register | reject | rem | report | return |
| rol | ror | select | severity | shared |
| signal | sla | sll | sra | srl |
| subtype | then | to | transport | type |
| unaffected | units | until | use | variable |
| wait | when | while | with | xnor/xor |

## Naming rules
* Base is determined with #. `2#101101# or `16#2D#`
* Underscores have no effect on numbers: `1000_1010 is the same as 10001010`
* Characters have '', strings have ""
     * **strings cannot use underscores like numbers** `"1000_1010 is different with "10001010"`
* Identifier can only contain alphabetic letters, decimal digits, and underscores
* The first character must be a letter
* The last character cannot be an underscore
* Two successive underscores are not allow
* VHDL is not case-sensitive

## Objects
### Signals
* Represents a wire, or a wire with memory (i.e. a register or latch)
```vhdl
signal signal_name, signal_name, ... : data_type;
signal a, b, c : std_logic := '0';
```
### Constant
```vhdl
constant CONSTANT_NAME : data_type := value_expression;
```
### Variables
```vhdl
variable variable_name, variable_name, ... : data_type
--optional initial value can be given
variable_name := value_expression; -- := means immediate assignment
```
* Can only be declared and used in a process and is local to that process.
* Has no direct mapping with hardware
* Has no associated timing information.  
### Alias
* Alias is not supported by all synthesis tools
```vhdl
signal word: std_logic_vector(15 downto 0);
alias op: std_logic_vector(6 downto 0) is word(15 downto 9);
alias reg1: std_logic_vector(2 downto 0) is word(8 downto 6);
```

## Data Types
* integer: range not defined, but minimal range is -(2<sup>31</sup>-1) to 2<sup>31</sup>-1 which is 32 bits. 
    * natural and positive subtypes. Natural starts at 0, positive at 1.
* boolean: false, true
* bit: '0' or '1'
* bit_vector: one-dimensional array with elements of the bit data type
* std_logic: **only '0' '1' and 'Z' are used in synthesis**
    * 'U': uninitialized
    * 'X' 'W': unkown and weak unkown 
    * '0' '1': forcing logic 0, forcing logic 1 (driven by a circuit with a regular driving current)
    * 'Z': high impedance (usually encountered in a tri-state buffer)
    * 'L' 'H': weak logic 0, weak logic 1 (signal is obtained from wired-logic types of circuits in which the driving current is weak
    * '-': stands for don't care
* std_logic_vector: array of elements with the std_logic data type (acts as a group of signals or a bus)
    * `signal a: std_logic_vector(7 downto 0); --7 is the most significant bit`
### signed and unsigned
* requires `use ieee.numeric_std.all;`
* array of elements with std_logic data type
* support arithmetic operations, unlike std_logic_vector
| Overloaded operator | Data type of operand a | Data type of operand b | Data Type of result |
| --- | --- | --- | --- | 
| abs a | signed | N/A | signed |
| - a | | | |
| a \* b | | | |
| a / b | unsigned | unsigned, nautral | unsigned |
| a mod b | unsigned, natural | unsigned | unsigned |
| a rem b | signed | signed, integer | signed |
| a + b | signed, integer | signed | signed |
| a -b | | | | | 
| a = b | | | | |
| a /= b | unsigned | unsigned, natural | boolean |
| a < b | unsigned, natural | unsigned | boolean |
| a <= b | signed | signed, integer | boolean |
| a > b | signed, integer | signed | boolean |
| a >= b | | | | |

| Function | Description | Data type of operand a | Data type of operand b | Data type of result |
| --- | --- | --- | --- | --- |
| shift_left(a,b) | shift left | unsigned, signed | natural | same as a |
| shift_right(a,b) | shift right | same | same | same |
| rotate_left(a,b) | rotate left | same | same | same |
| rotate_right(a,b) | rotate right | same | same | same |
| resize(a,b) | resize array | same | same | same |
| std_match(a,b) | compare '-' | unsigned, signed, std_logic_vector, std_logic | same as a | boolean |
| to_integer(a) | data type covnersion | unsigned, signed | N/A | integer |
| to_unsigned(a,b) | same | natural | natural | unsigned |
| to signed(a,b) | same | integer | natural | signed |
    
## Array Aggregate
```vhdl
a <= "10100000";
a <= ('1','0','1','0','0','0','0','0');
a <= (7=>'1',6=>'0',0=>'0',1=>'0',5=>'1',
      4=>'0',3=>'0',2=>'0');
a <= (7|5 => '1', 6|4|3|2|1|0=>'0');
a <= (7|5=>'1', others=>'0');
```

## Operators
### Operators
| Operator | Description | Data Type of Operand a | Data Type of Operand B | Data Type Result |
| --- | --- | --- | --- | --- |
| a \*\* b | exponentiation | integer | integer | integer |
| abs a | absolute value | integer | N/A | integer |
| not a | negation | boolean, bit, bit_vector, std_logic_vector, std_logic | N/A | same as a |
| a \* b | multiplication | integer | integer | integer |
| a / b | division | integer | integer | integer |
| a mod b | modulo | integer | integer | integer |
| a rem b | remainder | integer | integer | integer |
| + a | identity | integer | N/A | integer |
| - a | negation | integer | N/A | integer |
| a + b | addition | integer | integer | integer |
| a - b | subtraction | integer | integer | integer |
| a & b | concatentation | 1-d array, element | 1-D array, element | 1-D array |
| a sll b | shift-left logical | bit_vector | integer | bit_vector |
| a srl b | shift-right logical | bit_vector | integer | bit_vector |
| a sla b | shift-left arithmetic | bit_vector | integer | bit_vector |
| a sra b | shift-right arithemetic | bit_vector | integer | bit_vector |
| a rol b | rotate left | bit_vector | integer | bit_vector |
| a ror b | rotateright | bit_vector | integer | bit_vector |
| a = b | equal to | any | any | boolean | **Note:** arrays of different lengths always evaluate to false
| a /= b | not equal to | any | any | boolean |
| a < b | less than | scalar or 1-D array | scalar or 1-D array | boolean |
| a <= b | less than or equal | scalar or 1-D array | scalar or 1-D array | boolean |
| a > b | greater than | scalar or 1-D array | scalar or 1-D array | boolean |
| a >= b | greater than or equal | scalar or 1-D array | scalar or 1-D array | boolean |
| a and b | and | boolean, bit, bit_vector, std_logic_vector, std_logic | same as a | same as a |
| a or b | or | boolean, bit, bit_vector, std_logic_vector, std_logic | same as a | same as a |
| a xor b | xor | boolean, bit, bit_vector, std_logic_vector, std_logic | same as a | same as a |
| a nand b | nand | boolean, bit, bit_vector, std_logic_vector, std_logic | same as ar | same as a |
| a nor b | nor | boolean, bit, bit_vector, std_logic_vector, std_logic | same as a | same as a |
| a xnor b | xnor | boolean, bit, bit_vector, std_logic_vector, std_logic | same as a | same as a |

### Precedence
| Precedence | Operators |
| --- | --- |
| Highest | \*\* abs not<br>\* / mod rem<br>+ - (identity and negation)<br>& + - (arithmetic)<br>sll srl sla sra rol ror<br>= /= < <= > >=|
| Lowest | and or nand nor xor xnor |

## Type Conversion
| Function | Data type of operand a | Data type of result |
| --- | --- | --- |
| to_bit(a) | std_logic | bit |
| to_stdulogic(a) | bit | std_logic |
| to_bitvector(a) | std_logic_vector | bit_vector |
| to_stdlogicvector(a) | bit_vector | std_logic_vector |
| std_logic_vector(a) | unsigned, signed | std_logic_vector |
| unsigned(a) | signed, std_logic_vector | unsigned |
| signed(a) | unsigned, std_logic_vector | signed | 
| to_integer(a) | unsigned, signed | integer |
| to_unsigned(a, size) | natural | unsigned |
| to_signed(a, size) | integer | signed |

## Synthesis Guidelines
* Use the std_logic_vector and std_logic data types instead of the bit_vector or bit data types
* Use the numeric_std package and the unsigned and signed data types for synthesizing arithmetic operations
* Use only the descending range (i.e. downto) in the array specification
* Use parentheses to clarify the inteded order of evaluation
* Variables should be used with care. A Signal is generally preferred. 
* Except for the default value, avoid overriding as ignal multiple times ina  process.
* Think of the if and case statements as routing structures rather than as sequential control constructs. ('if' leads to cascading priority chains, 'case' leads to one large mux)

## Common Logic Units
### Multiplexer
### Decoder
### Simple ALU
### D FF with enable
```vhdl
library ieee;
use ieee.std_logic_1164.all;
entity dff_en is
     port(
          clk: in std_logic;
          reset: in std_logic;
          en: in std_logic;
          d: in std_logic;
          q: out std_logic
     );
end dff_en;

architecture two_seg_arch of dff_en is
     signal q_reg: std_logic;
     signal q_next: std_logic;
begin
     -- D FF
     process(clk, reset)
     begin
          if (reset='1') then
               q_reg <= '0';
          elsif (clk'event and clk='1') then
               q_reg <= q_next;
          end if;
     end process;
     -- next-state logic
     q_next <= d when en='1' else
               q_reg;
     -- output logic
     q <= q_reg;
end two_seg_arch;
```
```
