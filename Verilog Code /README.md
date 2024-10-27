`timescale 1ns / 1ps

module Traffic_Light_Controller_Cross_Shaped (
    input clk,
    input rst,
    output reg [2:0] light_R1, // Road 1 lights (North)
    output reg [2:0] light_R2, // Road 2 lights (East)
    output reg [2:0] light_R3, // Road 3 lights (South)
    output reg [2:0] light_R4  // Road 4 lights (West)
);

  // State encoding
    parameter S1 = 0, S2 = 1, S3 = 2, S4 = 3, S5 = 4, S6 = 5, S7 = 6, S8 = 7;
    reg [3:0] ps; // Present state
    reg [3:0] count;
    parameter sec7 = 7, sec5 = 5, sec2 = 2, sec4 = 4;

  // State transition and output logic
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            ps <= S1;
            count <= 0;
        end else begin
            case (ps)
                S1: if (count < sec7) begin
                        ps <= S1;
                        count <= count + 1;
                    end else begin
                        ps <= S2;
                        count <= 0;
                    end
                S2: if (count < sec2) begin
                        ps <= S2;
                        count <= count + 1;
                    end else begin
                        ps <= S3;
                        count <= 0;
                    end
                S3: if (count < sec7) begin
                        ps <= S3;
                        count <= count + 1;
                    end else begin
                        ps <= S4;
                        count <= 0;
                    end
                S4: if (count < sec2) begin
                        ps <= S4;
                        count <= count + 1;
                    end else begin
                        ps <= S5;
                        count <= 0;
                    end
                S5: if (count < sec5) begin
                        ps <= S5;
                        count <= count + 1;
                    end else begin
                        ps <= S6;
                        count <= 0;
                    end
                S6: if (count < sec2) begin
                        ps <= S6;
                        count <= count + 1;
                    end else begin
                        ps <= S7; // Turning state for R1 and R3
                        count <= 0;
                    end
                S7: if (count < sec4) begin
                        ps <= S7;
                        count <= count + 1;
                    end else begin
                        ps <= S8; // Turning state for R2 and R4
                        count <= 0;
                    end
                S8: if (count < sec4) begin
                        ps <= S8;
                        count <= count + 1;
                    end else begin
                        ps <= S1;
                        count <= 0;
                    end
                default: ps <= S1;
            endcase
        end
    end

  // Output logic based on current state (Mealy outputs)
    always @* begin
        case (ps)
            S1: begin
                light_R1 = 3'b001; // Green for R1 (North-South)
                light_R2 = 3'b100; // Red for R2
                light_R3 = 3'b001; // Green for R3
                light_R4 = 3'b100; // Red for R4
            end
            S2: begin
                light_R1 = 3'b010; // Yellow for R1
                light_R2 = 3'b100; // Red for R2
                light_R3 = 3'b010; // Yellow for R3
                light_R4 = 3'b100; // Red for R4
            end
            S3: begin
                light_R1 = 3'b100; // Red for R1
                light_R2 = 3'b001; // Green for R2 (East-West)
                light_R3 = 3'b100; // Red for R3
                light_R4 = 3'b001; // Green for R4
            end
            S4: begin
                light_R1 = 3'b100; // Red for R1
                light_R2 = 3'b010; // Yellow for R2
                light_R3 = 3'b100; // Red for R3
                light_R4 = 3'b010; // Yellow for R4
            end
            S5: begin
                light_R1 = 3'b001; // Green for R1
                light_R2 = 3'b100; // Red for R2
                light_R3 = 3'b001; // Green for R3
                light_R4 = 3'b100; // Red for R4
            end
            S6: begin
                light_R1 = 3'b010; // Yellow for R1
                light_R2 = 3'b100; // Red for R2
                light_R3 = 3'b010; // Yellow for R3
                light_R4 = 3'b100; // Red for R4
            end
            S7: begin
                light_R1 = 3'b011; // Turn left/right for R1
                light_R2 = 3'b100; // Red for R2
                light_R3 = 3'b011; // Turn left/right for R3
                light_R4 = 3'b100; // Red for R4
            end
            S8: begin
                light_R1 = 3'b100; // Red for R1
                light_R2 = 3'b011; // Turn left/right for R2
                light_R3 = 3'b100; // Red for R3
                light_R4 = 3'b011; // Turn left/right for R4
            end
            default: begin
                light_R1 = 3'b000; // Off
                light_R2 = 3'b000; // Off
                light_R3 = 3'b000; // Off
                light_R4 = 3'b000; // Off
            end
        endcase
    end
endmodule

