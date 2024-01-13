//32-bit ALU (Arithmetic Logic Unit) implemented in behavioral Verilog:

module ALU(
    input [31:0] operandA,
    input [31:0] operandB,
    input [2:0]  opcode,
    output reg [31:0] result,
    output reg zeroFlag,
    output reg overflowFlag,
    output reg carryFlag
);

always @(operandA, operandB, opcode)
begin
    case (opcode)
        3'b000: result = operandA + operandB; // Addition
        3'b001: result = operandA - operandB; // Subtraction
        3'b010: result = operandA & operandB; // Bitwise AND
        3'b011: result = operandA | operandB; // Bitwise OR
        3'b100: result = operandA ^ operandB; // Bitwise XOR
        3'b101: result = operandA << 1; // Left shift
        3'b110: result = operandA >> 1; // Right shift (logical)
        3'b111: result = (operandA[31] == 1) ? {32{1'b1}} : {32{1'b0}}; // Sign extension
        default: result = 32'b0; // Default to 0 for undefined opcode
    endcase

    // Set flags based on the result
    zeroFlag = (result == 32'b0);
    overflowFlag = ((operandA[31] == operandB[31]) && (result[31] != operandA[31]));
    carryFlag = (result < operandA);
end

endmodule
