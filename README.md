# RING-COUNTER

Design code:

module ring_counter (
    input clk,   
    input reset,  
    output reg [3:0] q  
);
initial begin
    q = 4'b0001; 
end
always @(posedge clk or posedge reset) begin
    if (reset) begin
        q <= 4'b0001;  
    end else begin
        q <= {q[2:0], q[3]};  
    end
end
endmodule

Test bench:

module ring_counter_tb;
reg clk;
reg reset;
wire [3:0] q;
ring_counter uut (
    .clk(clk),
    .reset(reset),
    .q(q)
);
initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end
initial begin
    reset = 1;   
    #10;         
    reset = 0;    
    #100;
    reset = 1;
    #10;
    reset = 0;
    #100;
    $stop;
end
initial begin
    $monitor("At time %0t, q = %b, reset = %b", $time, q, reset);
end
endmodule

