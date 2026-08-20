# 4_1_Multiplexer
# EXP NO: 1.A SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER
# AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

# APPARATUS REQUIRED
Vivado 2023.1

# Procedure
1.Open Vivado 2023.1.
2.Create a New RTL Project and give a name (e.g., Mux4_to_1).
3.Add/create your Verilog files and testbench.
4.Select an FPGA part (e.g., xc7a35ticsg324-1L).
5.Run Synthesis to check for errors.
6.Run Simulation → Run Behavioral Simulation.
7.Observe the waveforms of inputs and outputs.
8.Adjust simulation time if needed (e.g., 1000ns).
9.Save the project and take screenshots of results.
10.Close simulation.

# Logic Diagram

<img width="614" height="424" alt="Screenshot 2026-02-11 195225" src="https://github.com/user-attachments/assets/03cabe3f-914c-4163-bea7-2ba5257ed5a7" />


# Truthtable 

<img width="496" height="376" alt="Screenshot 2026-02-11 194234" src="https://github.com/user-attachments/assets/3530b20d-cc75-480c-bc14-d10b21a36376" />


# Verilog Code
4:1 MUX Gate-Level Implementation
```
// Gate Level Modelling - Skeleton
module mux4_gate (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    wire nS0, nS1;
    wire w0, w1, w2, w3;
    not g1 (nS0, S0);
    not g2 (nS1, S1);
    and g3 (w0, I0, nS1, nS0);
    and g4 (w1, I1, nS1, S0);
    and g5 (w2, I2, S1, nS0);
    and g6 (w3, I3, S1, S0);
    or  g7 (Y, w0, w1, w2, w3);
endmodule
```
4:1 MUX Gate-Level Implementation- Testbench
```
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_gate;
    reg I0, I1, I2, I3;
    reg S0, S1;
    wire Y;

    mux4_gate uut (
        .I0(I0), .I1(I1), .I2(I2), .I3(I3),
        .S0(S0), .S1(S1),
        .Y(Y)
    );
    initial begin
        I0 = 1; I1 = 0; I2 = 1; I3 = 0;        
        {S1, S0} = 2'b00; #10;
        {S1, S0} = 2'b01; #10;
        {S1, S0} = 2'b10; #10;
        {S1, S0} = 2'b11; #10;
        $finish;
    end
endmodule
```
# Simulated Output Gate Level Modelling
<img width="622" height="397" alt="image" src="https://github.com/user-attachments/assets/953e1cb0-e96a-4c5f-9ed1-bfcd5f70d573" />

4:1 MUX Data flow Modelling
```
// Dataflow Modelling - Skeleton
module mux4_dataflow (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    assign Y = (~S1 & ~S0 & I0) | 
               (~S1 &  S0 & I1) | 
               ( S1 & ~S0 & I2) | 
               ( S1 &  S0 & I3);
endmodule
```
4:1 MUX Data flow Modelling- Testbench
```
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_dataflow;
    reg I0, I1, I2, I3;
    reg S0, S1;
    wire Y;

    mux4_dataflow uut (
        .I0(I0), .I1(I1), .I2(I2), .I3(I3),
        .S0(S0), .S1(S1),
        .Y(Y)
    );

    initial begin
        I0 = 0; I1 = 1; I2 = 0; I3 = 1;
        
        {S1, S0} = 2'b00; #10;
        {S1, S0} = 2'b01; #10;
        {S1, S0} = 2'b10; #10;
        {S1, S0} = 2'b11; #10;
        
        $finish;
    end
endmodule
```
# Simulated Output Dataflow Modelling
<img width="1285" height="426" alt="image" src="https://github.com/user-attachments/assets/836c6cc6-c165-4ffb-bf06-d651f4e75501" />

4:1 MUX Behavioral Implementation
```
module mux4_to_1_behavioral (
    input wire A,
    input wire B,
    input wire C,
    input wire D,
    input wire S0,
    input wire S1,
    output reg Y
);
    always @(*) begin
        case ({S1, S0})
            2'b00: Y = A;
            2'b01: Y = B;
            2'b10: Y = C;
            2'b11: Y = D;
            default: Y = 1'bx;
        endcase
    end
endmodule
```
#4:1 MUX Behavioral Modelling- Testbench
```
// Testbench Skeleton
`timescale 1ns/1ps
mux4_to_1_behavioral uut (
        .A(I0), .B(I1), .C(I2), .D(I3),
        .S0(S0), .S1(S1),
        .Y(Y)
    );

    initial begin
       I0 = 1; I1 = 0; I2 = 0; I3 = 1;
        {S1, S0} = 2'b00; #10;
        {S1, S0} = 2'b01; #10;
        {S1, S0} = 2'b10; #10;
        {S1, S0} = 2'b11; #10;
        
        $finish;
        #10 $stop;
    end

endmodule
```
# Simulated Output Behavioral Modelling
_______ Here Paste the Simulated output ___________

#4:1 MUX Structural Implementation
```
module mux2_to_1 (
    input wire A,
    input wire B,
    input wire S,
    output wire Y
);
    assign Y = S ? B : A;
endmodule

module mux4_to_1_structural (
    input wire A,
    input wire B,
    input wire C,
    input wire D,
    input wire S0,
    input wire S1,
    output wire Y
);
wire m1_out, m2_out;

    mux2_to_1 mux_low  (.A(A), .B(B), .S(S0), .Y(m1_out));
    mux2_to_1 mux_high (.A(C), .B(D), .S(S0), .Y(m2_out));
    mux2_to_1 mux_out  (.A(m1_out), .B(m2_out), .S(S1), .Y(Y));
endmodule
```
# Testbench Implementation
```
`timescale 1ns / 1ps
module mux4_to_1_tb;
    reg A, B, C, D;
    reg S0, S1;
    wire Y_structural;
    mux4_to_1_structural uut (.A(A),.B(B),.C(C),.D(D),.S0(S0),.S1(S1),.Y(Y_structural));
    initial begin
        $monitor(
            "Time = %0t | A=%b B=%b C=%b D=%b | S1S0=%b%b | Y=%b",
            $time, A, B, C, D, S1, S0, Y_structural
        );
        A = 1'b0;
        B = 1'b1;
        C = 1'b0;
        D = 1'b1;
        S1 = 1'b0;
        S0 = 1'b0;
        #10;
        S1 = 1'b0;
        S0 = 1'b1;
        #10;
        S1 = 1'b1;
        S0 = 1'b0;
        #10;
        S1 = 1'b1;
        S0 = 1'b1;
        #10;

        $finish;
    end

endmodule
```
# Simulated Output Structural Modelling
<img width="784" height="311" alt="image" src="https://github.com/user-attachments/assets/e5855efd-72cb-45d6-83b2-3a307710622e" />

# CONCLUSION
In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.



endmodule
