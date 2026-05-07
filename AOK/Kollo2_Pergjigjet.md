# AOK - Pergjigjet Kollofjumit 2
Bazuar ne: Kap. 3 (Top Level View), Kap. 4 (Cache Memory)

---

## P1: Rretho modulet kryesore ne CPU

Modulet kryesore te CPU-se jane ALU (Arithmetic Logic Unit), Regjistrat, CU (Control Unit) dhe Intern BUS.

ALU kryen te gjitha operacionet aritmetike dhe logjike. CU (Control Unit) koordinon dhe kontrollon veprimet e te gjithe komponenteve te tjere. Regjistrat jane memoriet me te shpejta brenda CPU-se qe ruajne te dhena te perkohshme gjate ekzekutimit. Intern BUS lidh te gjithe keto module brenda CPU-se per transferim te te dhenave.

---

## P2: Cilat jane komponentet e BUS

BUS-i i sistemit perbehet nga tri komponentet kryesore: BUS i te dhenave (Data BUS), BUS i adresave (Address BUS) dhe BUS i kontrollit (Control BUS).

BUS i te dhenave transporton te dhenat ndermjet CPU-se, memories dhe pajisjeve I/O. BUS i adresave transmeton adresat e memories ose pajisjeve me te cilat komunikohet — eshte njekaheshem, vetem nga CPU. BUS i kontrollit transmeton sinjalet e kontrollit dhe koordinimit si lexim, shkrim dhe nderprerje — disa linja jane nga CPU drejt moduleve, disa te tjera ne kundershtim.

---

## P3: Ne regjistrin PC ruhet?

Ne regjistrin PC (Program Counter — Numeruesi i Programit) ruhet adresa e instruksionit te ardhshem qe do te ekzekutohet.

Pas cdo leximi te instruksionit nga memoria, PC perditësohet automatikisht per te treguar instruksionin pasardhes. Nese ekzekutohet nje instruksion jump ose branch, PC merr vleren e re te adreses se destinacionit. Keshtu CPU di gjithnje ku te vazhdoje ekzekutimin.

---

## P4: Cili regjister perdoret per ruajtje te shenimeve te pergjithshme

Regjistri qe perdoret per ruajtjen e shenimeve te pergjithshme eshte Akumulatori (Accumulator).

Akumulatori eshte regjistri kryesor i te dhenave brenda CPU-se. Ruan rezultatet e ndermjetme te operacioneve aritmetike dhe logjike te kryera nga ALU. Gjithashtu perdoret si operand hyrës dhe dalës ne shume instruksione. Ne arkitekturat e vjetra kishte vetem nje akumulator, ndersa processoret moderne kane shume regjistrat te pergjithshem (General Purpose Registers).

---

## P5: Cfare shkakton nje nderprerje ne nje sistem kompjuterik

Nje nderprerje (interrupt) mund te shkaktohet nga burime te ndryshme.

Nderprerjet e programit shkaktohen nga ekzekutimi i instruksioneve qe gjenerojne kushte te jashtezakonshme, si ndarja me zero, overflow aritmetik ose qasje ne adrese te pavlefshme. Nderprerjet e kllokut (timer) shkaktohen nga nje sinjal periodik i kllokut te sistemit per menaxhimin e multitasking. Nderprerjet e I/O shkaktohen nga pajisjet hyrje-dalje kur perfundojne nje operacion, si leximi i diskut ose marrja e nje pakete nga rrjeti. Nderprerjet e haredurit shkaktohen nga gabime fizike si nje gabim ne memorie.

---

## P6: Cili eshte qellimi i regjistrit MAR

Regjistri MAR (Memory Address Register — Regjistri i Adresave te Memories) ruan adresen e vendndodhjes ne memorie me te cilen behet qasja ne ate moment.

Kur CPU deshiron te lexoje ose shkruaje ne memorie, vendos adresen e deshiruar ne MAR. Kjo adrese dërgohet pastaj permes Address BUS tek memoria kryesore. MAR funksionon si ndermjetes ndermjet CPU-se dhe memories — CPU nuk komunikon direkt me memorien, por permes MAR. Mbahet i ndarë nga MDR (Memory Data Register) i cili ruan te dhenën aktuale qe po transferohet.

---

## P7: Cilat nga opsionet e meposhtme jane funksionet e mappimit

Tri funksionet e sakta te mappimit te cache-it jane Mapimi Direkt (Direct Mapping), Mapimi Asociativ (Associative Mapping) dhe Mapimi Set Asociativ (Set-Associative Mapping).

Mapimi direkt — cdo bllok i memories kryesore ka vetem nje linje te caktuar cache ku mund te vendoset. Mapimi asociativ — cdo bllok mund te vendoset ne cilendo linje te cache-it. Mapimi set asociativ — cache ndahet ne bashkesi dhe blloku mund te vendoset ne cilendo linje brenda bashkesise se percaktuar. Opsionet si random dhe sequential nuk jane funksione te mappimit te cache-it.

---

## P8: Shkruani funksionet e nje kompjuteri

Nje kompjuter ka kater funksione themelore.

Funksioni i pare eshte perpunimi i te dhenave (Data Processing). Kompjuteri mund te kryeje llogaritje dhe manipulime te ndryshme mbi te dhena, pavaresisht formes — numerike, tekst, grafike apo audio.

Funksioni i dyte eshte ruajtja e te dhenave (Data Storage). Kompjuteri ruan te dhena ne menyre afatshkurte (memoria kryesore) dhe afatgjate (disku), si per perdorim te menjehershem ashtu edhe per te ardhmen.

Funksioni i trete eshte levizja e te dhenave (Data Movement). Kompjuteri transferon te dhena ndermjet komponenteve te tij te brendshme dhe ndermjet pajisjet periferike si tastiera, ekrani, rrjeti dhe disku.

Funksioni i katert eshte kontrolli (Control). Control Unit menaxhon dhe koordinon ekzekutimin e tre funksioneve te mesiperme, duke siguruar rendin e duhur te operacioneve ne perputhje me instruksionet e programit.

---

## P9: Shkruani nje format te adreses se cdo funksioni te mappimit ne cache

Secili funksion mapimi perdor nje format te ndryshme te adreses se memories.

Mapimi Direkt ndane adresen ne tre fusha: Tag, Line dhe Word. Fusha Line identifikon se ne cilin linje te cache-it mapohet blloku. Fusha Tag krahasohet me etiketën e ruajtur ne ate linje per te verifikuar nese kemi hit. Fusha Word tregon pozicionin e bajtit brenda bllokut.

Mapimi Asociativ e ndane adresen ne vetem dy fusha: Tag dhe Word. Nuk ka fushe Line sepse blloku mund te jete ne cilendo linje — kerkohen te gjitha etiketat paralelisht. Fusha Tag eshte me e gjere se ne mapimin direkt.

Mapimi Set Asociativ e ndane adresen ne tri fusha: Tag, Set dhe Word. Fusha Set identifikon se ne cilin bashkesi mapohet blloku. Brenda bashkesise, fusha Tag krahasohet me etiketat e linjave te asaj bashkesie. Fusha Word tregon pozicionin brenda bllokut.

---

## P10: Cka paraqet struktura e cka funksioni

Struktura dhe funksioni jane dy koncepte themelore ne studimin e nje kompjuteri.

Funksioni paraqet se cfare ben kompjuteri — operacionet dhe sjelljet qe kompjuteri kryen. Pra, funksioni i pergjigjet pyetjes "cfare?". Kater funksionet kryesore jane perpunimi, ruajtja, levizja dhe kontrolli i te dhenave. Funksioni eshte i pavarur nga menyra se si jane lidhur komponentet fizikisht.

Struktura paraqet se si jane te organizuara dhe te nderliderura komponentet e kompjuterit — lidhjet fizike dhe logjike ndermjet tyre. Pra, struktura i pergjigjet pyetjes "si?". Struktura e nje kompjuteri pershkruan CPU-ne, memorien, BUS-in dhe pajisjet I/O dhe menyren se si lidhen me njeri-tjetrin. Cdo komponent ka edhe strukturen e vet te brendshme, keshtu qe kemi nje hierarki strukturash te nderfutura.
