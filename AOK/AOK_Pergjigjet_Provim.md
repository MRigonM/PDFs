# AOK - Pergjigjet e Provimit
Kollofjumi i 2-te — Ramadan Dervishi
Kapitujt: 4 (Cache), 5 (Memoria e Brendshme), 6 (Memoria e Jashtme)

---

## P1: Cilat jane tre funksionet e mapingeve?

Tri llojet e mapimit te cache-it jane mapimi direkt, mapimi asociativ dhe mapimi set asociativ.

Mapimi direkt eshte metoda me e thjeshte. Cdo bllok i memories kryesore mapohet ne vetem nje linje te caktuar te cache-it, sipas formules i = j mod m. Avantazhi eshte se eshte i thjeshte dhe i lire ne implementim, por disavantazhi kryesor eshte fenomeni i quajtur thrashing — kur dy blloqe qe perdoren shpesh mapojne ne te njejten linje dhe zevendesojne njeri-tjetrin vazhdimisht.

Mapimi asociativ i plote lejon cdo bllok te futet ne cilendo linje te cache-it. Cache-i krahason te gjitha etiketat paralelisht dhe njekohesisht per te gjetur bllokun e kerkuar. Eshte me fleksibel dhe eliminon thrashing, por kosto e harduerit eshte e larte sepse kerkon krahasim te plote te te gjitha linjave.

Mapimi set asociativ eshte kompromisi mes dy metodave te mesiperme. Cache-i ndahet ne bashkesi dhe cdo bashkesi permban k linja. Blloku mapohet ne nje bashkesi te caktuar, por brenda asaj bashkesie mund te futet ne cilendo linje. Kjo redukton thrashing dhe eshte me i lire se asociativi i plote, prandaj perdoret ne te gjithe processoret moderne.

---

## P2: Dallimi mes RAM dhe ROM

RAM (Random Access Memory) dhe ROM (Read Only Memory) jane dy llojet kryesore te memories gjysempercuese te brendshme dhe ndryshojne ne disa aspekte themelore.

RAM lejon lexim dhe shkrim te te dhenave dhe eshte volatile, domethene kur hiqet rrymja te dhenat humbasin. Perdoret si memoria kryesore e perkohshme e kompjuterit, ku ruhen programet dhe te dhenat qe jane ne perdorim aktiv.

ROM, nga ana tjeter, lejon vetem lexim dhe eshte nonvolatile, domethene te dhenat ruhen edhe pas hiqjes se rrymjes. Perdoret per ruajtjen e firmware-it dhe BIOS-it. Ekzistojne variante si EPROM qe fshihet me rreze UV dhe EEPROM qe fshihet elektrikisht, por te gjitha jane me te ngadalshme per shkrim se RAM-i dhe kategorizohen si read-only ose read-mostly.

---

## P3: Se paku 4 lloje te memories se brendshme

Ekzistojne shume lloje te memories gjysempercuese te brendshme.

RAM Dinamik (DRAM) ruan bitat si ngarkesa kondenzatoresh dhe kerkon freskim periodik sepse kondenzatori zbrazet me kalimin e kohes. Perdoret si memoria kryesore e kompjuterit sepse eshte e lire dhe ka densitet te larte.

RAM Statik (SRAM) ruan bitat me qarqe flip-flop dhe nuk kerkon freskim. Eshte me i shpejte se DRAM por me i shtrenjte dhe me densitet me te ulet. Perdoret per cache.

ROM eshte i shkruar gjate prodhimit ne fabrike dhe nuk mund te ndryshohet. Eshte permanent dhe nonvolatile, perdoret per firmware baze.

EPROM fshihet duke e ekspozuar ndaj rrezeve ultraviolet ne nivel chip-i dhe pastaj riprogramohet me pajisje speciale. Eshte nonvolatile.

EEPROM fshihet elektrikisht byte per byte pa e hequr chip-in nga qark. Shkrimi eshte shume me i ngadalshme se leximi. Eshte nonvolatile dhe perdoret per BIOS dhe konfigurime te ruajtura.

Flash Memory fshihet elektrikisht ne nivel blloku, jo byte per byte si EEPROM. Eshte kompromis mes EPROM dhe EEPROM dhe perdoret ne USB, SSD dhe kamera.

PROM programohet nje here nga perdoruesi me pajisje speciale dhe nuk mund te fshihet me pas (WORM — Write Once Read Many).

---

## P4: Dallimi mes RAM Statik dhe RAM Dinamik

SRAM dhe DRAM ndryshojne ne menyre themelore ne teknologjine e ruajtjes se biteve, dhe kjo ndikon ne shpejtesi, cmim dhe perdorim.

SRAM perdor qarqe flip-flop per te ruajtur cdo bit. Flip-flopi mban gjendjen ne menyre stabile pa asnje operacion shtese, prandaj SRAM nuk kerkon freskim periodik. Cdo cel kerkon rreth 6 tranzistora, gje qe e ben me te madh fizikisht, me te shtrenjte dhe me densitet te ulet, por edhe me te shpejte. Per kete arsye perdoret per cache (L1, L2, L3).

DRAM perdor nje kondenzator dhe nje tranzistor per cdo bit. Kondenzatori zbrazet gradualisht, keshtu qe duhet freskim periodik — gjate te cilit chip-i eshte i paaftesuar per qasje. Gjithashtu leximi eshte destruktiv, pra ngarkesa humbet gjate leximit dhe duhet restauruar. Keto vonesa e bejne DRAM me te ngadalshme. Por sepse kerkon me pak tranzistora, eshte me e vogel, me e lire dhe me densitet te larte. Perdoret si memoria kryesore e kompjuterit.

---

## P5: Zajedhe numrin binar 10011111 me Kodin e Hammingut

Kodi i Hammingut perdoret per detektimin dhe korigjimin e gabimeve ne memorie. Per te koduar numrin binar 10011111 (8 bite te dhena) duhen 4 bite pariteti (p1, p2, p4, p8), pra gjithsej 12 pozicione.

Pozicionet 1, 2, 4 dhe 8 rezervohen per bitat e paritetit. Bitat e te dhenave (d1 deri d8) vendosen ne pozicionet 3, 5, 6, 7, 9, 10, 11, 12. Duke marre numrin 10011111, kemi d1=1, d2=0, d3=0, d4=1, d5=1, d6=1, d7=1, d8=1. Pas vendosjes tabela duket keshtu:

```
Pozicioni:  1   2   3   4   5   6   7   8   9  10  11  12
            p1  p2   1  p4   0   0   1  p8   1   1   1   1
```

Llogaritja e biteve te paritetit behet me paritet cift (shuma e biteve te mbuluar duhet te jete cift):

p1 mbulon pozicionet 1,3,5,7,9,11 — bitat e te dhenave: 1,0,1,1,1 — shuma=4 (cift) — p1=0.
p2 mbulon pozicionet 2,3,6,7,10,11 — bitat e te dhenave: 1,0,1,1,1 — shuma=4 (cift) — p2=0.
p4 mbulon pozicionet 4,5,6,7,12 — bitat e te dhenave: 0,0,1,1 — shuma=2 (cift) — p4=0.
p8 mbulon pozicionet 8,9,10,11,12 — bitat e te dhenave: 1,1,1,1 — shuma=4 (cift) — p8=0.

Rezultati final eshte:

```
Pozicioni:  1   2   3   4   5   6   7   8   9  10  11  12
            0   0   1   0   0   0   1   0   1   1   1   1
```

Kodi Hamming: 001000101111

---

## P6: Pse RAM Statik eshte me i shpejte se RAM Dinamik?

SRAM eshte me i shpejte se DRAM per shkak te diferencave themelore ne teknologjine e ruajtjes se biteve.

SRAM perdor flip-flop, i cili eshte nje qarc logjik qe ruan gjendjen ne menyre stabile. Kjo gjendje nuk ndryshon pa nje sinjal te jashtme, prandaj qasja ne te dhena eshte e menjehershme dhe nuk kerkon asnje operacion shtese.

DRAM perdor kondenzatore, te cilet zbrazin gradualisht me kalimin e kohes. Kjo krijon tre vonesa shtesuese: se pari, chip-i duhet te beje freskim periodik — gjate te cilit nuk mund te aksesohet; se dyti, leximi eshte destruktiv, pra ngarkesa humbet dhe duhet restauruar pas cdo leximi; dhe se treti, gjendja e kondenzatorit eshte analoge (nivel ngarkese), jo digjitale, gje qe kerkon me shume kohe per interpretim. Te gjitha keto vonesa e bejne DRAM ndjeshme me te ngadalshme se SRAM.

---

## P7: Dallimi mes PROM dhe EEPROM

PROM (Programmable ROM) dhe EEPROM (Electrically Erasable Programmable ROM) jane dy lloje te memories nonvolatile read-mostly, por ndryshojne ne fleksibilitetin e programimit.

PROM programohet nje here nga perdoruesi me nje pajisje speciale dhe nuk mund te fshihet apo riprogramohet me pas. Eshte i pershtatshem per firmware te qendrueshem qe nuk ndryshon kurre.

EEPROM mund te fshihet elektrikisht byte per byte, pa e hequr chip-in nga qark, dhe riprogramohet shume here. Kjo e ben te pershtatshem per BIOS te azhurueshem dhe ruajtjen e konfigurimeve. Disavantazhi eshte se shkrimi eshte shume me i ngadalshme se leximi. Te dyja jane nonvolatile — ruajne te dhenat pa rryme.

---

## P8: Cili funksion mapimi eshte me i mire per kerkim dhe cili per shfrytezim?

Per kerkim, mapimi asociativ i plote eshte me i miri. Ai krahason te gjitha etiketat e linjave te cache-it njekohesisht dhe paralelisht, keshtu qe koha e gjetjes se nje blloku eshte konstante dhe minimale pavarur nga numri i linjave. Disavantazhi eshte se kerkon shume hardware dhe eshte i shtrenjte.

Per shfrytezim praktik, mapimi set asociativ eshte me i pershtatshmi. Ai kombinon shpejtesine e asociativit me koston e ulet te mapimit direkt. Duke vendosur bllokun ne nje bashkesi te caktuar por me liri zgjedhjeje brenda bashkesise, redukton thrashing pa kerkuar krahasim te plote te te gjitha linjave. Per kete arsye perdoret ne te gjithe processoret moderne.

Mapimi direkt eshte me i thjeshte por me i dobet, sepse shkakton thrashing kur dy blloqe te perdorshme shpesh ndajne te njejten linje cache.
