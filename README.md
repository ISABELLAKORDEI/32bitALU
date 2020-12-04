# 32bitALU
This how to implement A 32 bit ALU using Verilog in Quartus 

we are developing an ALU that takes two 32-bit inputs A and B,and executes the following seven instructions: add, sub, and, or, xor, nor, slt (set less than i.e. A-B)

************************************************************************************************************************************************************************
*************************************************Verilog code for the 32 bit alu with the specific intrusctions*********************************************************

module trial(                            
           input [31:0] A,B,  // ALU 32-bit Inputs                 
           input [2:0] ALU_Sel,// ALU Selection
           output [31:0] ALU_Out, // ALU 32-bit Output
           output CarryOut // Carry Out Flag
    );
    reg [31:0] ALU_Result; //to store the output 
    wire [32:0] tmp;
    assign ALU_Out = ALU_Result; // ALU out
    assign tmp = {1'b0,A} + {1'b0,B}; //
    assign CarryOut = tmp[32]; // Carryout flag
    always @(*)
    begin
        case(ALU_Sel)
        3'b000: // Addition
           ALU_Result = A + B ; 
        3'b001: // Subtraction
           ALU_Result = A - B ;
          3'b010: //  Logical and 
           ALU_Result = A & B;
          3'b100: //  Logical or
           ALU_Result = A | B;
          3'b101: //  Logical xor 
           ALU_Result = A ^ B;
          3'b110: //  Logical nor
           ALU_Result = ~(A | B);
          3'b111: // Slt  
             if (A < B)
			         ALU_Result = 1;
          default: ALU_Result = A + B ; 
        endcase
    end

endmodule

***************************************************************************************************************************************************************************
*************************************************************The Test bench code ******************************************************************************************

module alu;
//Inputs
 reg[31:0] A,B;
 reg[2:0] ALU_Sel;

//Outputs
 wire[31:0] ALU_Out;
 wire CarryOut;
 // Verilog code for ALU
 integer i;
 trial test_unit(
            A,B,  // ALU 32-bit Inputs                 
            ALU_Sel,// ALU Selection
            ALU_Out, // ALU 32-bit Output
            CarryOut // Carry Out Flag
     );
    initial begin
    // hold reset state for 100 ns.
      A = 32'h05;
      B = 32'h02;
      ALU_Sel = 3'h0;
      
      for (i=0;i<=6;i=i+1)
      begin
       ALU_Sel = ALU_Sel + 32'h01;
       #10; // wait for 10 seconds
      end
      
      
    end
endmodule 
