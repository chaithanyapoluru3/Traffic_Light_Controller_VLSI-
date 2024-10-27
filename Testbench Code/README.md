`timescale 1ns / 1ps

module Traffic_Light_Controller_Cross_Shaped_tb;

  // Inputs
    reg clk;
    reg rst;

  // Outputs
    wire [2:0] light_R1; // Road 1 lights (North)
    wire [2:0] light_R2; // Road 2 lights (East)
    wire [2:0] light_R3; // Road 3 lights (South)
    wire [2:0] light_R4; // Road 4 lights (West)

  // Instantiate the Unit Under Test (UUT)
    Traffic_Light_Controller_Cross_Shaped uut (
        .clk(clk),
        .rst(rst),
        .light_R1(light_R1),
        .light_R2(light_R2),
        .light_R3(light_R3),
        .light_R4(light_R4)
    );

  // Clock generation
    initial begin
        clk = 0;
        forever #5 clk = ~clk; // 10ns clock period
    end

  // Test sequence
    initial begin
        // Apply reset
        rst = 1;
        #10; // Wait for a few clock cycles with reset active
        rst = 0;
        
  // Run simulation for a sufficient amount of time to observe all states
        #200; // Extend time if needed to observe more transitions

  // Stop simulation
        $stop;
    end

  // Display the output state at each clock cycle for debugging
    initial begin
        $monitor("Time: %0d | State: %0d | R1: %b | R2: %b | R3: %b | R4: %b", 
                 $time, uut.ps, light_R1, light_R2, light_R3, light_R4);
    end

endmodule

