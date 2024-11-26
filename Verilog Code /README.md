module traffic_light_controller(
    input wire clk,            // Clock input
    input wire reset,          // Reset input
    output reg [1:0] vert_light,     // Vertical light (00: Red, 01: Green, 10: Yellow)
    output reg [1:0] horiz_light,    // Horizontal light (00: Red, 01: Green, 10: Yellow)
    output reg north_turn_east,      // North allows East turn
    output reg south_turn_west,      // South allows West turn
    output reg west_turn_north,      // West allows North turn
    output reg east_turn_south       // East allows South turn
);

// State encoding
    parameter VERT_GREEN_HORIZ_RED        = 3'b000;
    parameter VERT_YELLOW_HORIZ_RED       = 3'b001;
    parameter HORIZ_GREEN_VERT_RED        = 3'b010;
    parameter HORIZ_YELLOW_VERT_RED       = 3'b011;
reg [2:0] current_state, next_state;

// State transition and output logic
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= VERT_GREEN_HORIZ_RED;
        end else begin
            current_state <= next_state;
        end
    end

// Next state logic
    always @(*) begin
        case (current_state)
            VERT_GREEN_HORIZ_RED: begin
                next_state = VERT_YELLOW_HORIZ_RED;
            end
            VERT_YELLOW_HORIZ_RED: begin
                next_state = HORIZ_GREEN_VERT_RED;
            end
            HORIZ_GREEN_VERT_RED: begin
                next_state = HORIZ_YELLOW_VERT_RED;
            end
            HORIZ_YELLOW_VERT_RED: begin
                next_state = VERT_GREEN_HORIZ_RED;
            end
            default: begin
                next_state = VERT_GREEN_HORIZ_RED;
            end
        endcase
    end

// Output logic
    always @(*) begin
        // Default settings
        vert_light = 2'b00; // Red
        horiz_light = 2'b00; // Red
        north_turn_east = 0;
        south_turn_west = 0;
        west_turn_north = 0;
        east_turn_south = 0;

case (current_state)
            VERT_GREEN_HORIZ_RED: begin
                vert_light = 2'b01; // Green for vertical
                horiz_light = 2'b00; // Red for horizontal
                north_turn_east = 1;  // North turns East
                south_turn_west = 1;  // South turns West
            end
            VERT_YELLOW_HORIZ_RED: begin
                vert_light = 2'b10; // Yellow for vertical
                horiz_light = 2'b00; // Red for horizontal
            end
            HORIZ_GREEN_VERT_RED: begin
                vert_light = 2'b00; // Red for vertical
                horiz_light = 2'b01; // Green for horizontal
                west_turn_north = 1;  // West turns North
                east_turn_south = 1;  // East turns South
            end
            HORIZ_YELLOW_VERT_RED: begin
                vert_light = 2'b00; // Red for vertical
                horiz_light = 2'b10; // Yellow for horizontal
            end
        endcase
    end
endmodule
