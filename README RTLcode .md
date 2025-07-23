```verilog
module CarryLookAheadAdder #(parameter N=4) (input wire [N-1:0] a,b,
                                             input wire cin,
                                             output wire [N-1:0] s,
                                             output wire cout);
  wire [N-1:0] P;
  wire [N-1:0] G;
  wire [N:0] C;
  
  assign C[0] = cin;
  assign G = a & b;
  assign P = a ^ b;
  
  
  genvar i;
  generate
    for (i=0;i<N;i=i+1) begin
      assign C[i+1] = G[i]|(P[i] & C[i]);
    end
  endgenerate
  
  assign s = P ^ C[N-1:0];
  
  assign cout = C[N];
  
endmodule
