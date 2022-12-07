# Multiplexado-de-Displays-en-Verilog

FUNCIONAMIENTO:

Se utilizaron 2 multiplexores 2x4 y un decodificador (que hace de controlador CD4511) de BCD a 7segmentos,
en el primer multiplexor habilitamos los displays de 7 segmentos (anodo comun) enviandole una señal de bajo nivel (GND - 0 logico) a la salida "en";
El segundo mux  seleccionara el display (d0,d1) que enviara la señal a un registro "mux_sal", el cual a su vez ingresa en el decodificador y selecciona la salida.


Los cambios en la seleccion de entrada en los dispositivos viene dado por un contador que cada 5nS, activa el registro "clk_200"
el cual modifica el valor del registro de 2bits que actua como entrada de seleccion de ambos multiplexores.
