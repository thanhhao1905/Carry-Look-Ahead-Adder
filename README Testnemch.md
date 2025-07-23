```verilog
`timescale 1ps/1ps
module tbCLA;
  parameter N=4;
  
  reg [N-1:0] a,b;
  reg cin;
  wire [N-1:0] s;
  wire cout;
  reg [N:0]exp_s;
  reg exp_c;
  integer j,k,l,err=0;
  
  CarryLookAheadAdder #(N) DUT(a,b,cin,s,cout);
  
  initial begin
    $monitor("time=%0t, a=%b, b=%b, cin=%b, s=%b, cout=%b | exp_s=%b, exp_c=%b",$time,a,b,cin,s,cout,exp_s,exp_c);
    
    for( j=0;j<2**N;j=j+1) begin
      for(k=0;k<2**N;k=k+1) begin
        for(l=0;l<2;l=l+1) begin
          a=j;
          b=k;
          cin=l;
          exp_s= a+b+cin;
          exp_c= exp_s[N];
          #5
          check(s,cout,exp_s[N-1:0],exp_c);
        end
      end
    end
    
    if (err==0) begin
      $display("------------");
      $display("Test Pass");
      $display("____________");
    end else begin
      $display("------------");
      $display("Test False with %d error",err);
      $display("____________");
    end
    
    $finish;
  end
    
    task check (input [N-1:0]s, input cout,input [N-1:0]exp_s,input exp_c);
      begin
        if(s!==exp_s || cout!== exp_c) begin
          $display ("[CHECK]: ERROR");
          err=err+1;
        end else begin
          $display ("[CHECK]: MATCHING");
        end
      end
    endtask
    
    initial begin
      $dumpfile("dump.vcd");
      $dumpvars;
    end
endmodule
