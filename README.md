# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench

## Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench. To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software. Verilog HDL compiler.

## Verilog code for 4-bit Ripple carry adder
```
module ripple_adders (
    input [3:0] A, 
    input [3:0] B, 
    input Cin, 
    output [3:0] Sum, 
    output Cout
);

    reg [3:0] sum_temp;
    reg cout_temp;
    reg cout_final;

    task full_adder;
        input a, b, cin;
        output sum, cout;
    begin
        sum = a ^ b ^ cin;
        cout = (a & b) | (b & cin) | (cin & a);
    end
    endtask

    always @(*) begin
        full_adder(A[0], B[0], Cin, sum_temp[0], cout_temp);
        full_adder(A[1], B[1], cout_temp, sum_temp[1], cout_temp);
        full_adder(A[2], B[2], cout_temp, sum_temp[2], cout_temp);
        full_adder(A[3], B[3], cout_temp, sum_temp[3], cout_final);
    end

    assign Sum = sum_temp;
    assign Cout = cout_final;

endmodule
```
## Testbench code for 4-bit Ripple carry adder
```
module ripple_adder_tb;

    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the ripple carry adder
    ripple_adders uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Test cases
        A = 4'b0001; B = 4'b0010; Cin = 0; #10;
        A = 4'b0110; B = 4'b0101; Cin = 0; #10;
        A = 4'b1111; B = 4'b0001; Cin = 0; #10;
        A = 4'b1010; B = 4'b1101; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;
        $stop;
    end

    initial begin
        $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b",
                 $time, A, B, Cin, Sum, Cout);
    end
```
# OUTPUT
![Screenshot (38)](https://github.com/user-attachments/assets/2f922b86-56a7-4581-a399-0502974fd477)


# Verilog code for 4-bit Ripple counter
```
module ripple_counter_4bit (
input clk,           // Clock signal
input reset,         // Reset signal
output reg [3:0] Q   // 4-bit output for the counter value
);

// Function to calculate next state
function [3:0] next_state;
    input [3:0] curr_state;
    begin
        next_state = curr_state + 1;
    end
endfunction

// Sequential logic for counter
always @(posedge clk or posedge reset) begin
    if (reset)
        Q <= 4'b0000;       // Reset the counter to 0
    else
        Q <= next_state(Q); // Increment the counter
end

endmodule
```
# Testbench code for 4-bit Ripple counter
```
module ripple_counter_4bit_tb;

reg clk;
reg reset;
wire [3:0] Q;

// Instantiate the ripple counter
ripple_counter_4bit uut (
    .clk(clk),
    .reset(reset),
    .Q(Q)
);

// Clock generation (10ns period)
always #5 clk = ~clk;

initial begin
    // Initialize inputs
    clk = 0;
    reset = 1;

    // Hold reset for 20ns
    #20 reset = 0;

    // Run simulation for 200ns
    #200 $stop;
end

initial begin
    $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
end

endmodule
```
# OUTPUT
![Screenshot (39)](https://github.com/user-attachments/assets/6efadb71-92bc-44d7-b99a-1e61ed494c36)


# CONCLUSION
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs. The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.
