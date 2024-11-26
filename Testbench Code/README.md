`timescale 1ns / 1ps

module tb_traffic_light_controller;
    // Parameters
    reg clk;                   // Clock signal
    reg reset;                 // Reset signal
    wire [1:0] vert_light;     // Vertical light output
    wire [1:0] horiz_light;    // Horizontal light output
    wire north_turn_east;      // North allows East turn
    wire south_turn_west;      // South allows West turn
    wire west_turn_north;      // West allows North turn
    wire east_turn_south;      // East allows South turn

  // Instantiate the traffic light controller
    traffic_light_controller uut (
        .clk(clk),
        .reset(reset),
        .vert_light(vert_light),
        .horiz_light(horiz_light),
        .north_turn_east(north_turn_east),
        .south_turn_west(south_turn_west),
        .west_turn_north(west_turn_north),
        .east_turn_south(east_turn_south)
    );

  //Generate clock signal
    initial begin
        clk = 0; // Initialize clock to 0
        forever #5 clk = ~clk; // Toggle clock every 5 ns
    end

  // Test sequence
    initial begin
        // Initialize reset
        reset = 1; // Assert reset
        #10; // Wait for 10 ns
        reset = 0; // Deassert reset

  // Wait for a few clock cycles to observe behavior
        #50; // Observe outputs for 50 ns (5 clock cycles)

  // Additional test cycles
        #50; // Wait for another 50 ns
        #50; // Wait for another 50 ns
    end
endmodule
