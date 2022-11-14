
# LV1
#### Pri dodavanju instance komponente treba:
<ol>
  <li>Spojit na sabirnicu</li>
  <li>Proglasit portove eksternima</li>
  <li>Izgenerirat komponentu</li>
</ol>
Eksterni portovi su oni koji komuniciraju s Nexys pločom.

#### Pri pisanju koda potrebno:
<ol>
  <li>Napravit <b>XGpio</b> varijablu </li>
  <li>Inicijalizirat</li>
  <li>Postavit smjer</li>
</ol>

Svi parametri nalaze se u <b>xparameters.h</b> headeru.

#### Funkcije koje treba zapamtit:
<ul>
  <li> <b>XGpio_Initialize()</b>  </li>
  <li> <b>XGpio_SetDataDirection()</b>  </li>
  <li> <b>XGpio_DiscreteRead()</b>  </li>
  <li> <b>XGpio_DiscreteWrite()</b>  </li>
</ul>


---
# LV2
#### Vodič za Importat vlastitu jezgru:
<ol>
  <li><b>Kreirat</b> peripheral</li>
  <li><b>Napisat driver</b> - uredit vhdl datoteke </li>
  <li>Rescan User repository</li>
  <li><b>Importat</b> peripheral</li>
  <li>Dodat instancu komponente - (pod LV1 piše kako)</li>
</ol>

## Pojašnjenje VHDL koda u 8_1:
````
signal mask : std_logic_vector(0 to 1);    
signal masked_data : std_logic_vector(0 to 7); 
mask <= slv_reg0(30 to 31);

with mask select
	masked_data <=  "0000" & "0000" when "00",
			"0000" & data_in(4 to 7) when "01",
			      data_in(0 to 3) & "0000" when "10",
			      data_in when others;
							
data_out_1 <= masked_data(4 to 7);
data_out_2 <= masked_data(0 to 3);
````

<b>8bitni</b> input dijelimo na 2 <b>4bitna</b> outputa - tj. stanje sa 8 sklopki cemo pisat na 2 seta po 4 ledice

U ovisnosti o <b>zadnja 2 bita slave registra0 </b> radit ce ili jedan ili drugi set ledica - ako ce u registru biti 11 - radit ce oba kanala.

<b>Masku</b> cemo citati sa buttona, <b>Masked data</b> biti ce vrijednost sa switcheva, ali ona ce se slat u ovisnosti upaljenom ili ugasenom kanalu.


## Pojašnjenje VHDL koda u 8_3:
````
  signal mask : std_logic;
  signal output : std_logic_vector(0 to 4);

begin
  mask <= slv_reg0(31);                          
  with mask select output <=  ('0' & data_in_1) + ('0' & data_in_2) when '0'
                              ('0' & data_in_2) - ('0' & data_in_1) when others;
  
  data_out <= output;ž
````
Inputi su <b>4bitni</b> pa ih treba castat u <b>5bitne</b> - to se napravi tako da se <b>konkatenira 0 na pocetak</b>

npr 15 = 1111 - konkaterniramo 0 ispred: 0'&'1111 = 01111 - sad je 5bita a i dalje je to 15

Kada je u registru 0, zbrajaj - kada je 1, oduzimaj
## Pojašnjenje C koda u 8_3:
Dostupno u LV2\SDK\lv2_64\src\8_3.c

