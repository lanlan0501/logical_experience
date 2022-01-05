# logical_experience
// quartus

*4_bits_counter

  module lab_2 (
	  output reg [3:0] A,
	  output           C,
	  input [3:0]      D,
  	input            count,
		  				  load,
		  				  clear,
	  					  clk
  	);
  
	 assign C=count&(~load)&(A==4'b1111);
	
    always@(posedge clk,negedge clear)
	
      if(~clear)	   A <= 4'b0000;
	
      else if(load)  A <= D;
	
      else if(count) A <= A+1'b1;
	
      else           A <= A;
endmodule

*binary_counter

module binary_cou(
	output y,o0,o1,o2,o3,
	input  x,clk,re
	);
	reg[3:0]	st;
  
	  parameter s0=4'b0000,s1=4'b0001,s2=4'b0010,s3=4'b0011,s4=4'b0100,s5=4'b0101,s6=4'b0110,s7=4'b0111,s8=4'b1000,s9=4'b1001;
	  always@(posedge clk,negedge re)
		 if(re==0)st<=s0;
		else case(st)
			s0:	if(x)st<=s1;else st<=s0;
			s1:	if(x)st<=s2;else st<=s1;
			s2:	if(x)st<=s3;else st<=s2;
			s3:	if(x)st<=s4;else st<=s3;
			s4:	if(x)st<=s5;else st<=s4;
			s5:	if(x)st<=s6;else st<=s5;
			s6:	if(x)st<=s7;else st<=s6;
			s7:	if(x)st<=s8;else st<=s7;
			s8:	if(x)st<=s9;else st<=s8;
			s9:	if(x)st<=s0;else st<=s9;
		endcase
		assign y=(st==s7);
		assign o0=st[0];
		assign o1=st[1];
		assign o2=st[2];
		assign o3=st[3];
endmodule

*JK_flip_flop

module jk_ff(input j,k,cl,output reg q,output q_b);

    assign q_b=~q;
  	always@(posedge cl)
	  	case({j,k})
		  	2'b00:q<=q;
		  	2'b01:q<=1'b0;
			  2'b10:q<=1'b1;
			  2'b11:q<=~q;
		endcase
endmodule

*fpga_8x8

module lab_2(output [7:0] dr,dg,db,output reg [3:0]comm,input clk);
	divfreq F0(clk,clk_div);
	assign dr=8'b01010101;
	assign dg=8'b00110011;
	assign db=8'b00001111;
	always @(posedge clk_div)
	 if(comm==4'b0000)
		comm=4'b1000;
	 else
		comm=comm+1'b1;
endmodule

module divfreq(input clk,output reg clk_div);
	reg[24:0] count;
  
	always @(posedge clk)
		begin
			if(count>50000)
				begin
					count<=25'b0;
					clk_div<=~clk_div;
				end
			else
				count<=count+1'b1;
		end
endmodule

*fpga_2x7_segment

module lab_1 (output reg [6:0] s, output reg com1,com2,input clk);
	divfreq F0(clk,clk_div);
	
	initial
		begin
			s=7'b1000000;
			com1=1'b0;
			com2=1'b1;
		end
		
	always @(posedge clk_div)
		begin
			com1= ~ com1;
			com2= ~ com2;
			if(com1)
				s=7'b1000000;
			else
				s=7'b0000000;
		end
		
endmodule

module divfreq(input clk,output reg clk_div);
	reg [24:0] count;
  
	always @(posedge clk)
		begin
			if (count>250000)
			 begin 
				count<=25'b0;
				clk_div<=~clk_div;
			 end
			else
				count<=count+1'b1;
		end
endmodule

