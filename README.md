# Multiplexado-de-Displays-en-Verilog

FUNCIONAMIENTO:

Se utilizaron 2 multiplexores 2x4 y un decodificador (que hace de controlador CD4511) de BCD a 7segmentos,
en el primer multiplexor habilitamos los displays de 7 segmentos (anodo comun) enviandole una señal de bajo nivel (GND - 0 logico) a la salida "en";
El segundo mux  seleccionara el display (d0,d1) que enviara la señal a un registro "mux_sal", el cual a su vez ingresa en el decodificador y selecciona la salida.


Los cambios en la seleccion de entrada en los dispositivos viene dado por un contador que cada 5nS, activa el registro "clk_200"
el cual modifica el valor del registro de 2bits que actua como entrada de seleccion de ambos multiplexores.

CODIGO:

```
		
		module MUX
    input [3:0]d0,
	input [3:0]d1,
  //input [3:0]d2, 
  //input [3:0]d3,
    input clk,
    input rst,
    output reg [7:0]seg7,
  	output reg [3:0]en    );	
	 
reg [3:0] mux_sal;
reg [1:0] sel;
reg [13:0] clk_delay;
reg clk_200;


always @(*)
	 case (sel)
			0 : mux_sal <= d0;
			1 : mux_sal <= d1;
			2 : mux_sal <= 10; //d2
      default : mux_sal <= 10; //d3
	 endcase
		
always @(*)
	 case (sel)
			0 : en <= 4'b1011;
			1 : en <= 4'b1101;
			2 : en <= 4'b1111; //Inhabilitado
      default : en <= 4'b1111; //Inhabilitado
	 endcase
	 
always @(posedge clk)
	 if (rst) begin
			clk_delay <= 0;
			clk_200 <= 0;
	 end
	 else if (clk_delay < 5000) begin
			clk_delay <= clk_delay + 1;
			clk_200 <= 0;
	 end	
	 else begin
			clk_delay <= 0;
			clk_200 <= 1;
	 end


always @(posedge clk_200)
	if (rst) sel <= 0;
	else     sel <= sel+1;


always @(*)
      case (mux_sal)
			4'b0000 : seg7 <= 7'b0000001;   // 0
			4'b0001 : seg7 <= 7'b1001111;   // 1
            4'b0010 : seg7 <= 7'b0010010;   // 2
            4'b0011 : seg7 <= 7'b0000110;   // 3
            4'b0100 : seg7 <= 7'b1001100;   // 4
            4'b0101 : seg7 <= 7'b0100100;   // 5
            4'b0110 : seg7 <= 7'b0100000;   // 6
            4'b0111 : seg7 <= 7'b0001111;   // 7
            default : seg7 <= 7'b1111111;   // APAGADO

	  endcase
endmodule
		
```
  
