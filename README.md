# drs_lv
Codes from Laboratory Exercises 


## LV1
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
## LV2
#### Vodič za Importat vlastitu jezgru:
<ol>
  <li><b>Kreirat</b> peripheral</li>
  <li><b>Napisat driver</b> - uredit vhdl datoteke </li>
  <li>Rescan User repository</li>
  <li><b>Importat</b> peripheral</li>
  <li>Dodat instancu komponente - (pod LV1 piše kako)</li>
</ol>

#### Pojašnjenje VHDL koda u 8_1:
<code>
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
</code>
