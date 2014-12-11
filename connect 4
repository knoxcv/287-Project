//Help on the VGA code from Wilson Guo and Bailey 

module connect_4(CLK_50, VGA_R, VGA_B, VGA_G, VGA_HS, VGA_VS, VGA_SYNC_N, VGA_BLANK_N, VGA_CLK,sw17,sw16,sw15,sw14,sw13,sw12,sw11,sw1,rst,drop1,drop2,empty, player_space);

   input CLK_50, rst;

   output reg [7:0] VGA_R, VGA_B, VGA_G;//RGB
   output reg	 VGA_HS, VGA_VS;
	output VGA_BLANK_N,VGA_SYNC_N, VGA_CLK;
   reg [31:0] 	 xCoin = 32'd0;
   reg [31:0] 	 yCoin = 32'd1066;
   reg [31:0] 	 frameCounter = 32'd0;
	
   input sw17,sw16,sw15,sw14,sw13,sw12,sw11,sw1,drop1,drop2;

	reg [3:0] S;
	reg [3:0] NS;
	

	reg [2:0] count_C0;
	reg [2:0] count_C1;
	reg [2:0] count_C2;
	reg [2:0] count_C3;
	reg [2:0] count_C4;
	reg [2:0] count_C5;
	reg [2:0] count_C6;
	reg [5:0] space;
	
	reg p1_winner;
	reg p2_winner;
	reg full;
	
	reg [4:0] i;
	reg [4:0] j;
	reg [4:0] m;
	reg [4:0] n;
	
	output reg [41:0] empty;
	output reg [41:0] player_space;
	
	wire CLK_108;

   
   reg 		 yCLK = 1'b0;
   reg 		 frameClk;
   
   
   parameter xWidth = 32'd1688;
   parameter yHeight = 32'd1066;
   parameter frameClkTime = 32'd1799408; //clock cycle for one frame

  
	parameter start = 4'd0;
	parameter p1_wait = 4'd1;
	parameter p1 = 4'd2;
	parameter p1_check = 4'd3;
	parameter p1_win = 4'd4;
	parameter p2_wait = 4'd5;
	parameter p2 = 4'd6;
	parameter p2_check = 4'd7;
	parameter p2_win = 4'd8;
	parameter game_over = 4'd9;
	
   
   assign VGA_BLANK_N = VGA_HS & VGA_VS;
   assign VGA_SYNC_N = 1'b0;
   assign VGA_CLK = CLK_108;
	
   clock108 CLK__108(rst, CLK_50, CLK_108, locked);//108Mhz clock
	
   //frame clock
   always@(posedge CLK_108)
     if (frameCounter < frameClkTime)
       begin
	  frameCounter <= frameCounter + 1;
	  frameClk <= 0;
       end
     else
       begin
	  frameCounter <= 0;
	  frameClk <= 1;
       end
		       
   
   //X counter
   always@(posedge CLK_108)
     if (xCoin <= xWidth)
       begin
	  xCoin <= xCoin + 1;
	  yCLK <= 0;
       end
     else
       begin
	  xCoin <= 0;
	  yCLK <= 1;
       end
   
   //Y counter
   always@(posedge yCLK)
     if (yCoin > 0)
       yCoin <= yCoin - 1;
     else
       yCoin <= 1066;

   //X sync
   always@(*)
     if (xCoin <= 1280)
       VGA_HS = 1;
     else if (xCoin > 1280 && xCoin <= 1440)
       VGA_HS = 0;
     else
       VGA_HS = 0;

   //Y Sync
   always@(*)
     if (yCoin > 42)
       VGA_VS = 1;
     else if (yCoin > 38 && yCoin <= 42)
       VGA_VS = 0;
     else
       VGA_VS = 1;
   
	// makes the lines for the board and gives each player a designated color 
	
	always@(*) 
	begin
	
	//vertical lines for the board
	if ( (yCoin< 1024 & yCoin>294) &((xCoin > 10 & xCoin < 40)| (xCoin > 120 & xCoin < 150) | (xCoin>230 & xCoin< 260 ) | (xCoin>340 & xCoin< 370) | (xCoin>450 & xCoin<480)
			| (xCoin>560 & xCoin< 590) | (xCoin>670 & xCoin<700) | (xCoin>780 & xCoin<810)))
	begin
		VGA_R = 8'd255;
		VGA_B = 8'd0;
		VGA_G = 8'd255;	
	 end
	 // horizontal lines for the board
	else if ((xCoin > 10 & xCoin < 810)& ((yCoin<1024 & yCoin > 954) | (yCoin< 874 & yCoin> 844) | (yCoin<764 & yCoin> 734)
			|(yCoin <654 & yCoin>624) | (yCoin<544 & yCoin> 514) | (yCoin<434 & yCoin >404) | (yCoin< 324 & yCoin> 294)))
	begin 
		VGA_R = 8'd255;
		VGA_B = 8'd0;
		VGA_G = 8'd255;	
	 end
	 
	 // player 1 red squares
	 else if ((((yCoin< 954 & yCoin>874) &(xCoin > 40 & xCoin < 120) & (empty[35] == 1 & player_space[35] == 0))  | ((yCoin< 954 & yCoin>874) &(xCoin > 150 & xCoin < 230) & (empty[36] == 1 & player_space[36] == 0)) | ((yCoin< 954 & yCoin>874) &(xCoin > 260 & xCoin < 340) & (empty[37] == 1 & player_space[37] == 0)) | ((yCoin< 954 & yCoin>874) &(xCoin > 370 & xCoin < 450) & (empty[38] == 1 & player_space[38] == 0)) | ((yCoin< 954 & yCoin>874) &(xCoin > 480 & xCoin < 560) & (empty[39] == 1 & player_space[39] == 0)) | ((yCoin< 954 & yCoin>874) & (xCoin > 590 & xCoin < 670) & (empty[40] == 1 & player_space[40] == 0)) | ((yCoin< 954 & yCoin>874) &(xCoin > 700 & xCoin < 780) & (empty[41] == 1 & player_space[41] == 0))) //bottom row
				| (((yCoin< 844 & yCoin>764) &(xCoin > 40 & xCoin < 120) & (empty[28] == 1 & player_space[28] == 0)) | ((yCoin< 844 & yCoin>764) &(xCoin > 150 & xCoin < 230) & (empty[29] == 1 & player_space[29] == 0)) | ((yCoin< 844 & yCoin>764) &(xCoin > 260 & xCoin < 340) & (empty[30] == 1 & player_space[30] == 0)) | ((yCoin< 844 & yCoin>764) &(xCoin > 370 & xCoin < 450) & (empty[31] == 1 & player_space[31] == 0)) | ((yCoin< 844 & yCoin>764) &(xCoin > 480 & xCoin < 560) & (empty[32] == 1 & player_space[32] == 0)) | ((yCoin< 844 & yCoin>764) & (xCoin > 590 & xCoin < 670) & (empty[33] == 1 & player_space[33] == 0)) | ((yCoin< 844 & yCoin>764) &(xCoin > 700 & xCoin < 780) & (empty[34] == 1 & player_space[34] == 0))) // 2nd row
				| (((yCoin< 734 & yCoin>654) &(xCoin > 40 & xCoin < 120) & (empty[21] == 1 & player_space[21] == 0)) | ((yCoin< 734 & yCoin>654) &(xCoin > 150 & xCoin < 230) & (empty[22] == 1 & player_space[22] == 0)) | ((yCoin< 734 & yCoin>654) &(xCoin > 260 & xCoin < 340) & (empty[23] == 1 & player_space[23] == 0)) | ((yCoin< 734 & yCoin>654) &(xCoin > 370 & xCoin < 450) & (empty[24] == 1 & player_space[24] == 0)) | ((yCoin< 734 & yCoin>654) &(xCoin > 480 & xCoin < 560) & (empty[25] == 1 & player_space[25] == 0)) | ((yCoin< 734 & yCoin>654) & (xCoin > 590 & xCoin < 670) & (empty[26] == 1 & player_space[26] == 0)) | ((yCoin< 734 & yCoin>654) &(xCoin > 700 & xCoin < 780) & (empty[27] == 1 & player_space[27] == 0))) // 3rd row 
				| (((yCoin< 624 & yCoin>544) &(xCoin > 40 & xCoin < 120) & (empty[14] == 1 & player_space[14] == 0)) | ((yCoin< 624 & yCoin>544) &(xCoin > 150 & xCoin < 230) & (empty[15] == 1 & player_space[15] == 0)) | ((yCoin< 624 & yCoin>544) &(xCoin > 260 & xCoin < 340) & (empty[16] == 1 & player_space[16] == 0)) | ((yCoin< 624 & yCoin>544) &(xCoin > 370 & xCoin < 450) & (empty[17] == 1 & player_space[17] == 0)) | ((yCoin< 624 & yCoin>544) &(xCoin > 480 & xCoin < 560) & (empty[18] == 1 & player_space[18] == 0)) | ((yCoin< 624 & yCoin>544) & (xCoin > 590 & xCoin < 670) & (empty[19] == 1 & player_space[19] == 0)) | ((yCoin< 624 & yCoin>544) &(xCoin > 700 & xCoin < 780) & (empty[20] == 1 & player_space[20] == 0))) // 4th row
				| (((yCoin< 514 & yCoin>434) &(xCoin > 40 & xCoin < 120) & (empty[7] == 1 & player_space[7] == 0)) | ((yCoin< 514 & yCoin>434) &(xCoin > 150 & xCoin < 230) & (empty[8] == 1 & player_space[8] == 0)) | ((yCoin< 514 & yCoin>434) &(xCoin > 260 & xCoin < 340) & (empty[9] == 1 & player_space[9] == 0)) | ((yCoin< 514 & yCoin>434) &(xCoin > 370 & xCoin < 450) & (empty[10] == 1 & player_space[10] == 0)) | ((yCoin< 514 & yCoin>434) &(xCoin > 480 & xCoin < 560) & (empty[11] == 1 & player_space[11] == 0)) | ((yCoin< 514 & yCoin>434) & (xCoin > 590 & xCoin < 670) & (empty[12] == 1 & player_space[12] == 0)) | ((yCoin< 514 & yCoin>434) &(xCoin > 700 & xCoin < 780) & (empty[13] == 1 & player_space[13] == 0))) // 5th row 
				| (((yCoin< 404 & yCoin>324) &(xCoin > 40 & xCoin < 120) & (empty[0] == 1 & player_space[0] == 0)) | ((yCoin< 404 & yCoin>324) &(xCoin > 150 & xCoin < 230) & (empty[1] == 1 & player_space[1] == 0)) | ((yCoin< 404 & yCoin>324) &(xCoin > 260 & xCoin < 340) & (empty[2] == 1 & player_space[2] == 0)) | ((yCoin< 404 & yCoin>324) &(xCoin > 370 & xCoin < 450) & (empty[3] == 1 & player_space[3] == 0)) | ((yCoin< 404 & yCoin>324) &(xCoin > 480 & xCoin < 560) & (empty[4] == 1 & player_space[4] == 0)) | ((yCoin< 404 & yCoin>324) & (xCoin > 590 & xCoin < 670) & (empty[5] == 1 & player_space[5] == 0)) | ((yCoin< 404 & yCoin>324) &(xCoin > 700 & xCoin < 780) & (empty[6] == 1 & player_space[6] == 0))))// last row
	 begin
		VGA_R = 8'd255;
		VGA_B = 8'd0;
		VGA_G = 8'd0;
	end
	
	// player 2 blue squares
	 else if ((((yCoin< 954 & yCoin>874) &(xCoin > 40 & xCoin < 120) & (empty[35] == 1 & player_space[35] == 1))  | ((yCoin< 954 & yCoin>874) &(xCoin > 150 & xCoin < 230) & (empty[36] == 1 & player_space[36]  == 1)) | ((yCoin< 954 & yCoin>874) &(xCoin > 260 & xCoin < 340) & (empty[37] == 1 & player_space[37]  == 1)) | ((yCoin< 954 & yCoin>874) &(xCoin > 370 & xCoin < 450) & (empty[38] == 1 & player_space[38]  == 1)) | ((yCoin< 954 & yCoin>874) &(xCoin > 480 & xCoin < 560) & (empty[39] == 1 & player_space[39]  == 1)) | ((yCoin< 954 & yCoin>874) & (xCoin > 590 & xCoin < 670) & (empty[40] == 1 & player_space[40]  == 1)) | ((yCoin< 954 & yCoin>874) &(xCoin > 700 & xCoin < 780) & (empty[41] == 1 & player_space[41]  == 1))) //bottom row
				| (((yCoin< 844 & yCoin>764) &(xCoin > 40 & xCoin < 120) & (empty[28] == 1 & player_space[28] == 1)) | ((yCoin< 844 & yCoin>764) &(xCoin > 150 & xCoin < 230) & (empty[29] == 1 & player_space[29]  == 1)) | ((yCoin< 844 & yCoin>764) &(xCoin > 260 & xCoin < 340) & (empty[30] == 1 & player_space[30]  == 1)) | ((yCoin< 844 & yCoin>764) &(xCoin > 370 & xCoin < 450) & (empty[31] == 1 & player_space[31]  == 1)) | ((yCoin< 844 & yCoin>764) &(xCoin > 480 & xCoin < 560) & (empty[32] == 1 & player_space[32]  == 1)) | ((yCoin< 844 & yCoin>764) & (xCoin > 590 & xCoin < 670) & (empty[33] == 1 & player_space[33]  == 1)) | ((yCoin< 844 & yCoin>764) &(xCoin > 700 & xCoin < 780) & (empty[34] == 1 & player_space[34]  == 1))) // 2nd row
				| (((yCoin< 734 & yCoin>654) &(xCoin > 40 & xCoin < 120) & (empty[21] == 1 & player_space[21] == 1)) | ((yCoin< 734 & yCoin>654) &(xCoin > 150 & xCoin < 230) & (empty[22] == 1 & player_space[22]  == 1)) | ((yCoin< 734 & yCoin>654) &(xCoin > 260 & xCoin < 340) & (empty[23] == 1 & player_space[23]  == 1)) | ((yCoin< 734 & yCoin>654) &(xCoin > 370 & xCoin < 450) & (empty[24] == 1 & player_space[24]  == 1)) | ((yCoin< 734 & yCoin>654) &(xCoin > 480 & xCoin < 560) & (empty[25] == 1 & player_space[25]  == 1)) | ((yCoin< 734 & yCoin>654) & (xCoin > 590 & xCoin < 670) & (empty[26] == 1 & player_space[26]  == 1)) | ((yCoin< 734 & yCoin>654) &(xCoin > 700 & xCoin < 780) & (empty[27] == 1 & player_space[27]  == 1))) // 3rd row 
				| (((yCoin< 624 & yCoin>544) &(xCoin > 40 & xCoin < 120) & (empty[14] == 1 & player_space[14] == 1)) | ((yCoin< 624 & yCoin>544) &(xCoin > 150 & xCoin < 230) & (empty[15] == 1 & player_space[15]  == 1)) | ((yCoin< 624 & yCoin>544) &(xCoin > 260 & xCoin < 340) & (empty[16] == 1 & player_space[16]  == 1)) | ((yCoin< 624 & yCoin>544) &(xCoin > 370 & xCoin < 450) & (empty[17] == 1 & player_space[17]  == 1)) | ((yCoin< 624 & yCoin>544) &(xCoin > 480 & xCoin < 560) & (empty[18] == 1 & player_space[18]  == 1)) | ((yCoin< 624 & yCoin>544) & (xCoin > 590 & xCoin < 670) & (empty[19] == 1 & player_space[19]  == 1)) | ((yCoin< 624 & yCoin>544) &(xCoin > 700 & xCoin < 780) & (empty[20] == 1 & player_space[20]  == 1))) // 4th row
				| (((yCoin< 514 & yCoin>434) &(xCoin > 40 & xCoin < 120) & (empty[7] == 1 & player_space[7] == 1)) | ((yCoin< 514 & yCoin>434) &(xCoin > 150 & xCoin < 230) & (empty[8] == 1 & player_space[8]  == 1)) | ((yCoin< 514 & yCoin>434) &(xCoin > 260 & xCoin < 340) & (empty[9] == 1 & player_space[9]  == 1)) | ((yCoin< 514 & yCoin>434) &(xCoin > 370 & xCoin < 450) & (empty[10] == 1 & player_space[10]  == 1)) | ((yCoin< 514 & yCoin>434) &(xCoin > 480 & xCoin < 560) & (empty[11] == 1 & player_space[11]  == 1)) | ((yCoin< 514 & yCoin>434) & (xCoin > 590 & xCoin < 670) & (empty[12] == 1 & player_space[12]  == 1)) | ((yCoin< 514 & yCoin>434) &(xCoin > 700 & xCoin < 780) & (empty[13] == 1 & player_space[13]  == 1))) // 5th row 
				| (((yCoin< 404 & yCoin>324) &(xCoin > 40 & xCoin < 120) & (empty[0] == 1 & player_space[0] == 1)) | ((yCoin< 404 & yCoin>324) &(xCoin > 150 & xCoin < 230) & (empty[1] == 1 & player_space[1]  == 1)) | ((yCoin< 404 & yCoin>324) &(xCoin > 260 & xCoin < 340) & (empty[2] == 1 & player_space[2]  == 1)) | ((yCoin< 404 & yCoin>324) &(xCoin > 370 & xCoin < 450) & (empty[3] == 1 & player_space[3]  == 1)) | ((yCoin< 404 & yCoin>324) &(xCoin > 480 & xCoin < 560) & (empty[4] == 1 & player_space[4]  == 1)) | ((yCoin< 404 & yCoin>324) & (xCoin > 590 & xCoin < 670) & (empty[5] == 1 & player_space[5]  == 1)) | ((yCoin< 404 & yCoin>324) &(xCoin > 700 & xCoin < 780) & (empty[6] == 1 & player_space[6]  == 1))))// last row
	 begin
		VGA_R = 8'd0;
		VGA_B = 8'd255;
		VGA_G = 8'd0;
	end
	
	else
	begin
		VGA_R = 8'd255;
     VGA_B = 8'd255;
     VGA_G = 8'd255;
	end
	end

always @(posedge CLK_50 or posedge rst) 
begin
	if (rst == 1'b1)
	begin
		S <= start;
	end
	else
	begin
		S <= NS;
	end
end
	
	//state machine
always @(posedge CLK_50)
begin
/*
	case(S)
		start: 
		begin
			empty = 42'b0;
			player_space = 42'b0;
			count_C0 = 0;
			count_C1 = 0;
			count_C2 = 0;
			count_C3 = 0;
			count_C4 = 0;
			count_C5 = 0;
			count_C6 = 0;
			empty[41] = 1;
			empty[34] = 1;
			empty[27] = 1;
			empty[20] = 1;
			player_space[41] = 0;
			player_space[34] = 0;
			player_space[27] = 0;
			player_space[20] = 0;
			
			NS = p1_check;
		end
		




		p1_check: //check winner player 1 
		begin
			for(j = 0; j < 6; j = j + 1) // player 1 check row
			begin
				for(i = 0; i < 4; i = i + 1)
				begin
					if((player_space[j*7+i] == 0) & (player_space[j*7+i+1] == 0) & (player_space[j*7+i+2] == 0) & (player_space[j*7+i+3] == 0) & (empty[j*7+i] == 1) & (empty[j*7+1+i] == 1) & (empty[j*7+i+2] == 1) & (empty[j*7+i+3] == 1) )
					begin
						NS = p1_win;	
					end
				end
			end
			
			for(m = 0; m < 7; m = m + 1)	//player 1 check col
			begin
				for(n = 0; n < 3; n = n + 1)
				begin
					if((player_space[m+n*7] == 0) & (player_space[m+n*7+7] == 0) & (player_space[m+n*7+14] == 0) & (player_space[m+n*7+21] == 0) & (empty[m+n*7] == 1) & (empty[m+n*7+7] == 1) & (empty[m+n*7+14] == 1) & (empty[m+n*7+21] == 1))
					begin
						NS = p1_win;
					end
				end
			end
			
			NS = p2_wait;
			
		end
	endcase
end


*/








	case(S)
		start: 
		begin
			empty = 42'b0;
			player_space = 42'b0;
			count_C0 = 0;
			count_C1 = 0;
			count_C2 = 0;
			count_C3 = 0;
			count_C4 = 0;
			count_C5 = 0;
			count_C6 = 0;
			NS = p1_wait;
			p1_winner = 0;
			p2_winner = 0;
			full = 1;
		end
		
		p1_wait:
		begin
			if ((sw17 == 0 & drop1 == 0) | (sw16 == 0 & drop1 == 0) | (sw15 == 0 & drop1 == 0) | (sw14 == 0 & drop1 == 0) | (sw13 == 0 & drop1 == 0) | (sw12 == 0 & drop1 == 0) | (sw11 == 0 & drop1 == 0))
			begin
				NS = p1;
			end
			else NS = p1_wait;
		end
				
		p1:
		begin
			if(count_C0 < 6 & sw17 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C0*7] == 0)
					begin
						empty[count_C0*7] = 1;
						player_space[count_C0*7] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C0 = count_C0 + 1;
			end
		
			
			else if(count_C1 < 6 & sw16 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C1*7 + 1] == 0)
					begin
						empty[count_C1*7 + 1] = 1;
						player_space[count_C1*7 + 1] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C1 = count_C1 + 1;
			end
			
			
			else if(count_C2 < 6 & sw15 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C2*7 + 2] == 0)
					begin
						empty[count_C2*7 + 2] = 1;
						player_space[count_C2*7 + 2] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C2 = count_C2 + 1;
			end
			
		
			else if(count_C3 < 6 & sw14 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C3*7 + 3] == 0)
					begin
						empty[count_C3*7 + 3] = 1;
						player_space[count_C3*7 + 3] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C3 = count_C3 + 1;
			end
		
		
			else if(count_C4 < 6 & sw13 == 1 & drop1 == 0  & !(drop2 == 0))
			begin
				if (empty[count_C4*7 + 4] == 0)
					begin
						empty[count_C4*7 + 4] = 1;
						player_space[count_C4*7 + 4] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C4 = count_C4 + 1;
			end
			
			
			else if(count_C5 < 6 & sw12 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C5*7 + 5] == 0)
					begin
						empty[count_C5*7 + 5] = 1;
						player_space[count_C5*7 + 5] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C5 = count_C5 + 1;
			end
			
			
			else if(count_C6 < 6 & sw11 == 1 & drop1 == 0 & !(drop2 == 0))
			begin
				if (empty[count_C6*7 + 6] == 0)
					begin
						empty[count_C6*7 + 6] = 1;
						player_space[count_C6*7 + 6] = 0;	//player 1 in the block
						NS = p1_check;
					end
				else 
					count_C6 = count_C6 + 1;
			end
			else NS = p1;	
			
		end
		
		
		p1_check: //check winner player 1 
		begin
			for(j = 0; j < 6; j = j + 1) // player 1 check row
			begin
				for(i = 0; i < 4; i = i + 1)
				begin
					if((player_space[j*7+i] == 0) & (player_space[j*7+i+1] == 0) & (player_space[j*7+i+2] == 0) & (player_space[j*7+i+3] == 0) & (empty[j*7+i] == 1) & (empty[j*7+1+i] == 1) & (empty[j*7+i+2] == 1) & (empty[j*7+i+3] == 1) )
					begin
						p1_winner = 1;
					end
				end
			end
			
			for(m = 0; m < 7; m = m + 1)	//player 1 check col
			begin
				for(n = 0; n < 3; n = n + 1)
				begin
					if((player_space[m+n*7] == 0) & (player_space[m+n*7+7] == 0) & (player_space[m+n*7+14] == 0) & (player_space[m+n*7+21] == 0) & (empty[m+n*7] == 1) & (empty[m+n*7+7] == 1) & (empty[m+n*7+14] == 1) & (empty[m+n*7+21] == 1))
					begin
						p1_winner = 1;
					end
				end
			end
			
			//check diagonal /
			if((player_space[14] == 0)&(player_space[22] == 0)&(player_space[30] == 0)&(player_space[38] == 0) & (empty[14] == 1)&(empty[22] == 1)&(empty[30] == 1)&(empty[38] == 1) |
			(player_space[7] == 0)&(player_space[15] == 0)&(player_space[23] == 0)&(player_space[31] == 0) & (empty[7] == 1)&(empty[15] == 1)&(empty[23] == 1)&(empty[31] == 1) |
			(player_space[15] == 0)&(player_space[23] == 0)&(player_space[31] == 0)&(player_space[39] == 0) & (empty[15] == 1)&(empty[23] == 1)&(empty[31] == 1)&(empty[39] == 1) |
			(player_space[0] == 0)&(player_space[8] == 0)&(player_space[16] == 0)&(player_space[24] == 0) & (empty[0] == 1)&(empty[8] == 1)&(empty[16] == 1)&(empty[24] == 1) |
			(player_space[8] == 0)&(player_space[14] == 0)&(player_space[24] == 0)&(player_space[32] == 0) & (empty[8] == 1)&(empty[14] == 1)&(empty[24] == 1)&(empty[32] == 1) |
			(player_space[14] == 0)&(player_space[24] == 0)&(player_space[32] == 0)&(player_space[40] == 0) & (empty[14] == 1)&(empty[24] == 1)&(empty[32] == 1)&(empty[40] == 1) |
			(player_space[1] == 0)&(player_space[9] == 0)&(player_space[17] == 0)&(player_space[25] == 0) & (empty[1] == 1)&(empty[9] == 1)&(empty[17] == 1)&(empty[25] == 1) |
			(player_space[9] == 0)&(player_space[17] == 0)&(player_space[25] == 0)&(player_space[33] == 0) & (empty[9] == 1)&(empty[17] == 1)&(empty[25] == 1)&(empty[33] == 1) |
			(player_space[17] == 0)&(player_space[25] == 0)&(player_space[33] == 0)&(player_space[41] == 0) & (empty[17] == 1)&(empty[25] == 1)&(empty[33] == 1)&(empty[41] == 1) |
			(player_space[2] == 0)&(player_space[10] == 0)&(player_space[18] == 0)&(player_space[26] == 0) & (empty[2] == 1)&(empty[10] == 1)&(empty[18] == 1)&(empty[26] == 1) |
			(player_space[10] == 0)&(player_space[18] == 0)&(player_space[26] == 0)&(player_space[34] == 0) & (empty[10] == 1)&(empty[18] == 1)&(empty[26] == 1)&(empty[34] == 1) |
			(player_space[3] == 0)&(player_space[11] == 0)&(player_space[19] == 0)&(player_space[27] == 0) & (empty[3] == 1)&(empty[11] == 1)&(empty[19] == 1)&(empty[27] == 1))
			begin
				p1_winner = 1;
			end
			
			if((player_space[3] == 0)&(player_space[9] == 0)&(player_space[15] == 0)&(player_space[21] == 0) & (empty[3] == 1)&(empty[9] == 1)&(empty[15] == 1)&(empty[21] == 1) |
			(player_space[4] == 0)&(player_space[10] == 0)&(player_space[16] == 0)&(player_space[22] == 0) & (empty[4] == 1)&(empty[10] == 1)&(empty[16] == 1)&(empty[22] == 1) |
			(player_space[10] == 0)&(player_space[16] == 0)&(player_space[22] == 0)&(player_space[28] == 0) & (empty[10] == 1)&(empty[16] == 1)&(empty[22] == 1)&(empty[28] == 1) |
			(player_space[5] == 0)&(player_space[11] == 0)&(player_space[17] == 0)&(player_space[23] == 0) & (empty[5] == 1)&(empty[11] == 1)&(empty[17] == 1)&(empty[23] == 1) |
			(player_space[11] == 0)&(player_space[17] == 0)&(player_space[23] == 0)&(player_space[29] == 0) & (empty[11] == 1)&(empty[17] == 1)&(empty[23] == 1)&(empty[29] == 1) |
			(player_space[17] == 0)&(player_space[23] == 0)&(player_space[29] == 0)&(player_space[35] == 0) & (empty[17] == 1)&(empty[23] == 1)&(empty[29] == 1)&(empty[35] == 1) |
			(player_space[6] == 0)&(player_space[12] == 0)&(player_space[18] == 0)&(player_space[24] == 0) & (empty[6] == 1)&(empty[12] == 1)&(empty[18] == 1)&(empty[24] == 1) |
			(player_space[12] == 0)&(player_space[18] == 0)&(player_space[24] == 0)&(player_space[30] == 0) & (empty[12] == 1)&(empty[18] == 1)&(empty[24] == 1)&(empty[30] == 1) |
			(player_space[18] == 0)&(player_space[24] == 0)&(player_space[30] == 0)&(player_space[36] == 0) & (empty[18] == 1)&(empty[24] == 1)&(empty[30] == 1)&(empty[36] == 1) |
			(player_space[13] == 0)&(player_space[19] == 0)&(player_space[25] == 0)&(player_space[31] == 0) & (empty[13] == 1)&(empty[19] == 1)&(empty[25] == 1)&(empty[31] == 1) |
			(player_space[19] == 0)&(player_space[25] == 0)&(player_space[31] == 0)&(player_space[37] == 0) & (empty[19] == 1)&(empty[25] == 1)&(empty[31] == 1)&(empty[37] == 1) |
			(player_space[20] == 0)&(player_space[26] == 0)&(player_space[32] == 0)&(player_space[38] == 0) & (empty[20] == 1)&(empty[26] == 1)&(empty[32] == 1)&(empty[38] == 1))
			begin
				p1_winner = 1;
			end
			
			if(p1_winner == 1)
			begin
				NS = p1_win;
			end
			else
			begin
				NS = p2_wait;
			end
			
		end
		
		
		p2_wait:
		begin
			if ((sw17 == 0 & drop2 == 0) | (sw16 == 0 & drop2 == 0) | (sw15 == 0 & drop2 == 0) | (sw14 == 0 & drop2 == 0) | (sw13 == 0 & drop2 == 0) | (sw12 == 0 & drop2 == 0) | (sw11 == 0 & drop2 == 0))
			begin
				NS = p2;
			end
			else NS = p2_wait;
		end
		
		p2: 
		begin
			if(count_C0 < 6 & sw17 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C0*7] == 0)
					begin
						empty[count_C0*7] = 1;
						player_space[count_C0*7] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C0 = count_C0 + 1;
			end
		
			
			else if(count_C1 < 6 & sw16 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C1*7 + 1] == 0)
					begin
						empty[count_C1*7 + 1] = 1;
						player_space[count_C1*7 + 1] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C1 = count_C1 + 1;
			end
			
			
			else if(count_C2 < 6 & sw15 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C2*7 + 2] == 0)
					begin
						empty[count_C2*7 + 2] = 1;
						player_space[count_C2*7 + 2] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C2 = count_C2 + 1;
			end
			
		
			else if(count_C3 < 6 & sw14 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C3*7 + 3] == 0)
					begin
						empty[count_C3*7 + 3] = 1;
						player_space[count_C3*7 + 3] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C3 = count_C3 + 1;
			end
		
		
			else if(count_C4 < 6 & sw13 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C4*7 + 4] == 0)
					begin
						empty[count_C4*7 + 4] = 1;
						player_space[count_C4*7 + 4] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C4 = count_C4 + 1;
			end
			
			
			else if(count_C5 < 6 & sw12 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C5*7 + 5] == 0)
					begin
						empty[count_C5*7 + 5] = 1;
						player_space[count_C5*7 + 5] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C5 = count_C5 + 1;
			end
			
			
			else if(count_C6 < 6 & sw11 == 1 & drop2 == 0 & !(drop1 == 0))
			begin
				if (empty[count_C6*7 + 6] == 0)
					begin
						empty[count_C6*7 + 6] = 1;
						player_space[count_C6*7 + 6] = 1;	//player 1 in the block
						NS = p2_check;
					end
				else 
					count_C6 = count_C6 + 1;
			end
			else NS = p2;	
		end
		
		p2_check: //check winner player 1 
		begin
			for(j = 0; j < 6; j = j + 1) // player 1 check row
			begin
				for(i = 0; i < 4; i = i + 1)
				begin
					if((player_space[j*7+i] == 1) & (player_space[j*7+i+1] == 1) & (player_space[j*7+i+2] == 1) & (player_space[j*7+i+3] == 1) & (empty[j*7+i] == 1) & (empty[j*7+1+i] == 1) & (empty[j*7+i+2] == 1) & (empty[j*7+i+3] == 1) )
					begin
						p2_winner = 1;
					end
				end
			end
			
			for(m = 0; m < 7; m = m + 1)	//player 1 check col
			begin
				for(n = 0; n < 3; n = n + 1)
				begin
					if((player_space[m+n*7] == 1) & (player_space[m+n*7+7] == 1) & (player_space[m+n*7+14] == 1) & (player_space[m+n*7+21] == 1) & (empty[m+n*7] == 1) & (empty[m+n*7+7] == 1) & (empty[m+n*7+14] == 1) & (empty[m+n*7+21] == 1))
					begin
						p2_winner = 1;
					end
				end
			end
			
			if((player_space[14] == 1)&(player_space[22] == 1)&(player_space[30] == 1)&(player_space[38] == 1) & (empty[14] == 1)&(empty[22] == 1)&(empty[30] == 1)&(empty[38] == 1) |
			(player_space[7] == 1)&(player_space[15] == 1)&(player_space[23] == 1)&(player_space[31] == 1) & (empty[7] == 1)&(empty[15] == 1)&(empty[23] == 1)&(empty[31] == 1) |
			(player_space[15] == 1)&(player_space[23] == 1)&(player_space[31] == 1)&(player_space[39] == 1) & (empty[15] == 1)&(empty[23] == 1)&(empty[31] == 1)&(empty[39] == 1) |
			(player_space[0] == 1)&(player_space[8] == 1)&(player_space[16] == 1)&(player_space[24] == 1) & (empty[0] == 1)&(empty[8] == 1)&(empty[16] == 1)&(empty[24] == 1) |
			(player_space[8] == 1)&(player_space[14] == 1)&(player_space[24] == 1)&(player_space[32] == 1) & (empty[8] == 1)&(empty[14] == 1)&(empty[24] == 1)&(empty[32] == 1) |
			(player_space[14] == 1)&(player_space[24] == 1)&(player_space[32] == 1)&(player_space[40] == 1) & (empty[14] == 1)&(empty[24] == 1)&(empty[32] == 1)&(empty[40] == 1) |
			(player_space[1] == 1)&(player_space[9] == 1)&(player_space[17] == 1)&(player_space[25] == 1) & (empty[1] == 1)&(empty[9] == 1)&(empty[17] == 1)&(empty[25] == 1) |
			(player_space[9] == 1)&(player_space[17] == 1)&(player_space[25] == 1)&(player_space[33] == 1) & (empty[9] == 1)&(empty[17] == 1)&(empty[25] == 1)&(empty[33] == 1) |
			(player_space[17] == 1)&(player_space[25] == 1)&(player_space[33] == 1)&(player_space[41] == 1) & (empty[17] == 1)&(empty[25] == 1)&(empty[33] == 1)&(empty[41] == 1) |
			(player_space[2] == 1)&(player_space[10] == 1)&(player_space[18] == 1)&(player_space[26] == 1) & (empty[2] == 1)&(empty[10] == 1)&(empty[18] == 1)&(empty[26] == 1) |
			(player_space[10] == 1)&(player_space[18] == 1)&(player_space[26] == 1)&(player_space[34] == 1) & (empty[10] == 1)&(empty[18] == 1)&(empty[26] == 1)&(empty[34] == 1) |
			(player_space[3] == 1)&(player_space[11] == 1)&(player_space[19] == 1)&(player_space[27] == 1) & (empty[3] == 1)&(empty[11] == 1)&(empty[19] == 1)&(empty[27] == 1))
			begin
				p2_winner = 1;
			end
			
			if((player_space[3] == 1)&(player_space[9] == 1)&(player_space[15] == 1)&(player_space[21] == 1) & (empty[3] == 1)&(empty[9] == 1)&(empty[15] == 1)&(empty[21] == 1) |
			(player_space[4] == 1)&(player_space[10] == 1)&(player_space[16] == 1)&(player_space[22] == 1) & (empty[4] == 1)&(empty[10] == 1)&(empty[16] == 1)&(empty[22] == 1) |
			(player_space[10] == 1)&(player_space[16] == 1)&(player_space[22] == 1)&(player_space[28] == 1) & (empty[10] == 1)&(empty[16] == 1)&(empty[22] == 1)&(empty[28] == 1) |
			(player_space[5] == 1)&(player_space[11] == 1)&(player_space[17] == 1)&(player_space[23] == 1) & (empty[5] == 1)&(empty[11] == 1)&(empty[17] == 1)&(empty[23] == 1) |
			(player_space[11] == 1)&(player_space[17] == 1)&(player_space[23] == 1)&(player_space[29] == 1) & (empty[11] == 1)&(empty[17] == 1)&(empty[23] == 1)&(empty[29] == 1) |
			(player_space[17] == 1)&(player_space[23] == 1)&(player_space[29] == 1)&(player_space[35] == 1) & (empty[17] == 1)&(empty[23] == 1)&(empty[29] == 1)&(empty[35] == 1) |
			(player_space[6] == 1)&(player_space[12] == 1)&(player_space[18] == 1)&(player_space[24] == 1) & (empty[6] == 1)&(empty[12] == 1)&(empty[18] == 1)&(empty[24] == 1) |
			(player_space[12] == 1)&(player_space[18] == 1)&(player_space[24] == 1)&(player_space[30] == 1) & (empty[12] == 1)&(empty[18] == 1)&(empty[24] == 1)&(empty[30] == 1) |
			(player_space[18] == 1)&(player_space[24] == 1)&(player_space[30] == 1)&(player_space[36] == 1) & (empty[18] == 1)&(empty[24] == 1)&(empty[30] == 1)&(empty[36] == 1) |
			(player_space[13] == 1)&(player_space[19] == 1)&(player_space[25] == 1)&(player_space[31] == 1) & (empty[13] == 1)&(empty[19] == 1)&(empty[25] == 1)&(empty[31] == 1) |
			(player_space[19] == 1)&(player_space[25] == 1)&(player_space[31] == 1)&(player_space[37] == 1) & (empty[19] == 1)&(empty[25] == 1)&(empty[31] == 1)&(empty[37] == 1) |
			(player_space[20] == 1)&(player_space[26] == 1)&(player_space[32] == 1)&(player_space[38] == 1) & (empty[20] == 1)&(empty[26] == 1)&(empty[32] == 1)&(empty[38] == 1))
			begin
				p2_winner = 1;
			end
			
			if(p2_winner == 1)
			begin
				NS = p2_win;
			end	
			/*else if((empty[0] == 1)&(empty[1] == 1)&(empty[2] == 1)&(empty[3] == 1)&(empty[4] == 1)&(empty[5] == 1)&(empty[6] == 1)&(empty[7] == 1)&(empty[8] == 1)&(empty[9] == 1)&
			(empty[10] == 1)&(empty[11] == 1)&(empty[12] == 1)&(empty[13] == 1)&(empty[14] == 1)&(empty[15] == 1)&(empty[16] == 1)&(empty[17] == 1)&(empty[18] == 1)&(empty[19] == 1)&
			(empty[20] == 1)&(empty[21] == 1)&(empty[22] == 1)&(empty[23] == 1)&(empty[24] == 1)&(empty[25] == 1)&(empty[26] == 1)&(empty[27] == 1)&(empty[28] == 1)&(empty[29] == 1)&
			(empty[30] == 1)&(empty[31] == 1)&(empty[32] == 1)&(empty[33] == 1)&(empty[34] == 1)&(empty[35] == 1)&(empty[36] == 1)&(empty[37] == 1)&(empty[38] == 1)&(empty[39] == 1)&
			(empty[40] == 1)&(empty[41] == 1))
			begin
				empty[0] = 0;
			end
			*/
			else
			begin
				NS = p1_wait;
			end
			
		end
		
		p1_win:
		begin
			if(sw1 == 1)
			begin
				empty[0] = 0;empty[1] = 0;empty[2] = 1;empty[3] = 1;empty[4] = 1;empty[5] = 0;empty[6] = 0;empty[7] = 0;empty[8] = 0;empty[9] = 0;
				empty[10] = 1;empty[11] = 0;empty[12] = 0;empty[13] = 0;empty[14] = 0;empty[15] = 0;empty[16] = 0;empty[17] = 1;empty[18] = 0;empty[19] = 0;
				empty[20] = 0;empty[21] = 0;empty[22] = 0;empty[23] = 0;empty[24] = 1;empty[25] = 0;empty[26] = 0;empty[27] = 0;empty[28] = 0;empty[29] = 0;
				empty[30] = 1;empty[31] = 1;empty[32] = 0;empty[33] = 0;empty[34] = 0;empty[35] = 0;empty[36] = 0;empty[37] = 0;empty[38] = 1;empty[39] = 0;
				empty[40] = 0;empty[41] = 0;
				
				
				player_space[2] = 0;player_space[3] = 0;player_space[4] = 0;player_space[10] = 0;player_space[17] = 0;player_space[24] = 0;player_space[30] = 0;
				player_space[31] = 0;player_space[38] = 0;
			end
				NS = p1_win;
		end
		
		p2_win:
		begin
			if(sw1 == 1)
			begin
				empty[0] = 0;empty[1] = 0;empty[2] = 1;empty[3] = 1;empty[4] = 1;empty[5] = 0;empty[6] = 0;empty[7] = 0;empty[8] = 0;empty[9] = 1;
				empty[10] = 0;empty[11] = 0;empty[12] = 0;empty[13] = 0;empty[14] = 0;empty[15] = 0;empty[16] = 1;empty[17] = 0;empty[18] = 0;empty[19] = 0;
				empty[20] = 0;empty[21] = 0;empty[22] = 0;empty[23] = 1;empty[24] = 1;empty[25] = 1;empty[26] = 0;empty[27] = 0;empty[28] = 0;empty[29] = 0;
				empty[30] = 0;empty[31] = 0;empty[32] = 1;empty[33] = 0;empty[34] = 0;empty[35] = 0;empty[36] = 0;empty[37] = 1;empty[38] = 1;empty[39] = 1;
				empty[40] = 0;empty[41] = 0;
			
				player_space[2] = 1;player_space[3] = 1;player_space[4] = 1;player_space[9] = 1;player_space[16] = 1;player_space[23] = 1;player_space[24] = 1;
				player_space[25] = 1;player_space[32] = 1;player_space[37] = 1;
				player_space[38] = 1;player_space[39] = 1;
			end
			NS = p2_win;
		end
		
	endcase

end

endmodule



 // synopsys translate_off
`timescale 1 ps / 1 ps
// synopsys translate_on
module clock108 (areset, inclk0, c0, locked);

    input     areset;
    input     inclk0;
    output    c0;
    output    locked;

`ifndef ALTERA_RESERVED_QIS
// synopsys translate_off
`endif

tri0      areset;

`ifndef ALTERA_RESERVED_QIS
// synopsys translate_on
`endif

    wire [0:0] sub_wire2 = 1'h0;
    wire [4:0] sub_wire3;
    wire  sub_wire5;
    wire  sub_wire0 = inclk0;
    wire [1:0] sub_wire1 = {sub_wire2, sub_wire0};
    wire [0:0] sub_wire4 = sub_wire3[0:0];
    wire  c0 = sub_wire4;
    wire  locked = sub_wire5;

altpll  altpll_component (
            .areset (areset),
            .inclk (sub_wire1),
            .clk (sub_wire3),
            .locked (sub_wire5),
            .activeclock (),
            .clkbad (),
            .clkena ({6{1'b1}}),
            .clkloss (),
            .clkswitch (1'b0),
            .configupdate (1'b0),
            .enable0 (),
            .enable1 (),
            .extclk (),
            .extclkena ({4{1'b1}}),
            .fbin (1'b1),
            .fbmimicbidir (),
            .fbout (),
            .fref (),
            .icdrclk (),
            .pfdena (1'b1),
            .phasecounterselect ({4{1'b1}}),
            .phasedone (),
            .phasestep (1'b1),
            .phaseupdown (1'b1),
            .pllena (1'b1),
            .scanaclr (1'b0),
            .scanclk (1'b0),
            .scanclkena (1'b1),
            .scandata (1'b0),
            .scandataout (),
            .scandone (),
            .scanread (1'b0),
            .scanwrite (1'b0),
            .sclkout0 (),
            .sclkout1 (),
            .vcooverrange (),
            .vcounderrange ());
defparam
    altpll_component.bandwidth_type = "AUTO",
    altpll_component.clk0_divide_by = 25,
    altpll_component.clk0_duty_cycle = 50,
    altpll_component.clk0_multiply_by = 54,
    altpll_component.clk0_phase_shift = "0",
    altpll_component.compensate_clock = "CLK0",
    altpll_component.inclk0_input_frequency = 20000,
    altpll_component.intended_device_family = "Cyclone IV E",
    altpll_component.lpm_hint = "CBX_MODULE_PREFIX=clock108",
    altpll_component.lpm_type = "altpll",
    altpll_component.operation_mode = "NORMAL",
    altpll_component.pll_type = "AUTO",
    altpll_component.port_activeclock = "PORT_UNUSED",
    altpll_component.port_areset = "PORT_USED",
    altpll_component.port_clkbad0 = "PORT_UNUSED",
    altpll_component.port_clkbad1 = "PORT_UNUSED",
    altpll_component.port_clkloss = "PORT_UNUSED",
    altpll_component.port_clkswitch = "PORT_UNUSED",
    altpll_component.port_configupdate = "PORT_UNUSED",
    altpll_component.port_fbin = "PORT_UNUSED",
    altpll_component.port_inclk0 = "PORT_USED",
    altpll_component.port_inclk1 = "PORT_UNUSED",
    altpll_component.port_locked = "PORT_USED",
    altpll_component.port_pfdena = "PORT_UNUSED",
    altpll_component.port_phasecounterselect = "PORT_UNUSED",
    altpll_component.port_phasedone = "PORT_UNUSED",
    altpll_component.port_phasestep = "PORT_UNUSED",
    altpll_component.port_phaseupdown = "PORT_UNUSED",
    altpll_component.port_pllena = "PORT_UNUSED",
    altpll_component.port_scanaclr = "PORT_UNUSED",
    altpll_component.port_scanclk = "PORT_UNUSED",
    altpll_component.port_scanclkena = "PORT_UNUSED",
    altpll_component.port_scandata = "PORT_UNUSED",
    altpll_component.port_scandataout = "PORT_UNUSED",
    altpll_component.port_scandone = "PORT_UNUSED",
    altpll_component.port_scanread = "PORT_UNUSED",
    altpll_component.port_scanwrite = "PORT_UNUSED",
    altpll_component.port_clk0 = "PORT_USED",
    altpll_component.port_clk1 = "PORT_UNUSED",
    altpll_component.port_clk2 = "PORT_UNUSED",
    altpll_component.port_clk3 = "PORT_UNUSED",
    altpll_component.port_clk4 = "PORT_UNUSED",
    altpll_component.port_clk5 = "PORT_UNUSED",
    altpll_component.port_clkena0 = "PORT_UNUSED",
    altpll_component.port_clkena1 = "PORT_UNUSED",
    altpll_component.port_clkena2 = "PORT_UNUSED",
    altpll_component.port_clkena3 = "PORT_UNUSED",
    altpll_component.port_clkena4 = "PORT_UNUSED",
    altpll_component.port_clkena5 = "PORT_UNUSED",
    altpll_component.port_extclk0 = "PORT_UNUSED",
    altpll_component.port_extclk1 = "PORT_UNUSED",
    altpll_component.port_extclk2 = "PORT_UNUSED",
    altpll_component.port_extclk3 = "PORT_UNUSED",
    altpll_component.self_reset_on_loss_lock = "OFF",
    altpll_component.width_clock = 5;
endmodule 
