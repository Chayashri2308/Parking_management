# Parking_management
# Parking Management System using Verilog, to identify the occupied and vacant space in a parking lot.

`timescale 1ns / 1ps
// Parking Management System Verilog Code
module ParkingManagementSystem #(parameter NUM_SPACES = 8) (
    input [NUM_SPACES-1:0] sensor_input,  // Sensor signals for each space (1 = occupied, 0 = vacant)
    output reg [NUM_SPACES-1:0] status,   // Status of each space (1 = occupied, 0 = vacant)
    output reg [3:0] occupied_count,      // Count of occupied spaces
    output reg [3:0] vacant_count         // Count of vacant spaces
);

    // Update status and counts based on sensor input
    always @ (sensor_input) begin
        integer i;
        occupied_count = 0;
        vacant_count = 0;
        
        for (i = 0; i < NUM_SPACES; i = i + 1) begin
            status[i] = sensor_input[i];  // Update status based on sensor input
            
            if (sensor_input[i] == 1) begin
                occupied_count = occupied_count + 1;  // Increment occupied count
            end else begin
                vacant_count = vacant_count + 1;      // Increment vacant count
            end
        end
    end

    // Display module to print occupancy status
    always @ (sensor_input) begin
        $display("Total Spaces: %d | Occupied: %d | Vacant: %d", NUM_SPACES, occupied_count, vacant_count);
    end
endmodule


// Testbench to simulate Parking Management System
module Testbench;
    reg [7:0] sensor_input;                 // Simulated sensor inputs
    wire [7:0] status;                      // Status output
    wire [3:0] occupied_count, vacant_count; // Count outputs

    // Instantiate the ParkingManagementSystem module
    ParkingManagementSystem #(.NUM_SPACES(8)) parking_system (
        .sensor_input(sensor_input),
        .status(status),
        .occupied_count(occupied_count),
        .vacant_count(vacant_count)
    );

    // Test cases
    initial begin
        // Initial state: all spaces vacant
        sensor_input = 8'b00000000;
        #10;  // Wait for 10 time units
        // Case 1: First four spaces occupied
        sensor_input = 8'b00001111;
        #10;
        // Case 2: Last four spaces occupied
        sensor_input = 8'b11110000;
        #10;
        // Case 3: All spaces occupied
        sensor_input = 8'b11111111;
        #10;
        // Case 4: All spaces vacant
        sensor_input = 8'b00000000;
        #10;
        
        $finish;  // End the simulation
    end
endmodule

