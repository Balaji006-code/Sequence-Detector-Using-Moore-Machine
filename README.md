# Design and Simulation of Sequence Detector Using Moore Machine in Verilog HDL

---

## **Aim**
To design, simulate, and verify a **Sequence Detector** using the **Moore Finite State Machine (FSM)** model in Verilog HDL.

---

## **Apparatus Required**
- System with **Vivado Design Suite**
---

## **Theory**

A **sequence detector** is a type of **finite state machine (FSM)** that detects the occurrence of a specific sequence of bits in a given input stream.

### **Moore Machine Concept**
In a **Moore Machine**, the **output depends only on the present state** and **not on the current input**.  
The output changes only when the state changes.

---

### **Example Sequence: 1011**
The sequence detector should produce an output `Z = 1` whenever the input sequence **“1011”** is detected in the serial input stream.

#### **State Description**
| State | Description | Output (Z) |
|:------:|:-------------|:-----------:|
| S0 | Initial State / No bits matched | 0 |
| S1 | ‘1’ detected | 0 |
| S2 | ‘10’ detected | 0 |
| S3 | ‘101’ detected | 0 |
| S4 | ‘1011’ detected | 1 |

#### **State Transition Diagram**
<img width="403" height="557" alt="image" src="https://github.com/user-attachments/assets/9b24604c-69d2-412f-a302-2c23aed79038" />

#### **State Transition Table**
| Current State | Input (X) | Next State | Output (Z) |
|:---------------|:----------|:------------|:------------|
| S0 | 0 | S0 | 0 |
| S0 | 1 | S1 | 0 |
| S1 | 0 | S2 | 0 |
| S1 | 1 | S1 | 0 |
| S2 | 0 | S0 | 0 |
| S2 | 1 | S3 | 0 |
| S3 | 0 | S2 | 0 |
| S3 | 1 | S4 | 1 |
| S4 | 0 | S2 | 0 |
| S4 | 1 | S1 | 0 |

---

## **Program**

```verilog
`timescale 1ns / 1ps
module moore_digital(clk,rst,in,out);
input clk,in,rst;
output reg out;
parameter s0=3'b000,
s1=3'b001,
s2=3'b010,
s3=3'b011,
s4=3'b100;
reg [2:0] current_state,next_state;
always @(posedge clk or posedge rst)
begin
if(rst)
current_state<=s0;
else
current_state<=next_state;
end
always @(*)begin
    case(current_state)
        s0:next_state<=in?s1:s0;
        s1:next_state<=in?s1:s2;
        s2:next_state<=in?s3:s0;
        s3:next_state<=in?s4:s2;
        s4:next_state<=in?s1:s0;
        default:next_state<=s0;
   endcase
end

always @(*)
begin
    case(current_state)
        s4:out<=1;
        default:out<=0;
    endcase
end
endmodule
```
### Testbench
```verilog
module moore_digital_tb;
reg clk,rst,in;
wire out;
moore_digital uut(clk,rst,in,out);
initial
begin
    clk=0;
    forever #5clk=~clk;
end
initial
begin
rst=1;
in=0;
#10 rst=0;
#10 in=1;
#10 in=0;
#10 in=1;
#10 in=1;

#10 rst=0;
#10 in=1;
#10 in=0;
#10 in=1;
#10 in=0;
#20 $finish;
end
endmodule
```
### Simulation Output


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a09dac3b-1f95-4f06-bd53-022eded25863" />

 

### Result

The Sequence Detector using Moore Machine was successfully designed and simulated in Vivado Design Suite using Verilog HDL.
The simulation confirmed that the output Z is asserted high only when the input sequence “1011” is detected.
