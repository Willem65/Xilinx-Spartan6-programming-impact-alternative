## FPGA programmeren met xc3sprog (alternatief voor Xilinx iMPACT)

Voor het programmeren van de Xilinx Spartan FPGA kan het open‑source gereedschap **xc3sprog** gebruikt worden als alternatief voor de Xilinx iMPACT‑tool. xc3sprog werkt via JTAG en ondersteunt onder andere de Digilent JTAG‑HS2 kabel. Met dit programma kun je de FPGA direct laden (voor testen) of de externe SPI‑flash programmeren (voor permanente opslag van de bitstream).

Een eenvoudige manier om de FPGA direct te configureren is:

`xc3sprog -c jtaghs2 -v top.bit`

Hiermee wordt de bitfile `top.bit` via de JTAG‑HS2 kabel (`-c jtaghs2`) in het vluchtige configuratiegeheugen van de FPGA geladen. De optie `-v` zorgt voor uitgebreide (verbose) uitvoer, zodat je kunt zien wat er tijdens het programmeren gebeurt. Deze methode is geschikt voor snelle tests: de configuratie verdwijnt weer zodra de FPGA wordt gereset of uitgeschakeld.

Als het programmeren sneller mag verlopen, kun je de JTAG‑kloksnelheid verhogen:

`xc3sprog -c jtaghs2 -J 2000000 -v top.bit`

In dit commando stelt `-J 2000000` de JTAG‑snelheid in op 2 MHz. Dit verkort de programmeertijd, vooral bij grotere bitfiles. De rest van het commando werkt hetzelfde als in het vorige voorbeeld: de FPGA wordt direct geladen met `top.bit`, en `-v` geeft extra informatie tijdens het proces. Let erop dat niet elke kabel of setup hogere JTAG‑snelheden ondersteunt; als er problemen optreden, kan een lagere waarde (bijvoorbeeld 1000000) gebruikt worden.

Naast het direct laden van de FPGA kan xc3sprog ook gebruikt worden om de **SPI‑flash** achter de FPGA te programmeren. Dit is handig wanneer je wilt dat de FPGA na een power‑cycle automatisch met een bepaalde bitstream opstart. In dat geval wordt niet de FPGA zelf, maar de flashchip geprogrammeerd. Dat gebeurt met:

`xc3sprog -c jtaghs2 -p 0 -I topview1.bit`

Hier geeft `-p 0` aan dat het eerste device in de JTAG‑keten geselecteerd moet worden (meestal de SPI‑flash). De optie `-I` staat voor *indirect programming*: xc3sprog stuurt de bitfile `topview1.bit` naar de flash in plaats van naar de FPGA‑SRAM. Na succesvol programmeren zal de FPGA bij het inschakelen of resetten automatisch de inhoud van de flash laden en zich configureren met deze bitstream. Dit is de methode voor **permanente configuratie**, vergelijkbaar met het programmeren van een PROM of SPI‑flash via iMPACT.

Samengevat: gebruik `xc3sprog -c jtaghs2 -v top.bit` voor snel testen door de FPGA direct te laden, `xc3sprog -c jtaghs2 -J 2000000 -v top.bit` voor dezelfde methode maar met hogere JTAG‑snelheid, en `xc3sprog -c jtaghs2 -p 0 -I topview1.bit` om de SPI‑flash te programmeren zodat de FPGA na een reset automatisch met deze bitstream opstart.
