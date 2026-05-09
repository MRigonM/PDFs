# AOK — Guida Kryesore per Provim

Arkitektura dhe Organizimi i Kompjuterit — Kollofjumi 2. Kapitujt: 2 (Evolucioni), 3 (Top Level View), 4 (Cache), 5 (Memoria e Brendshme), 6 (Memoria e Jashtme).

---

## Struktura dhe Funksioni i Kompjuterit

Nje kompjuter ka kater funksione themelore. I pari eshte perpunimi i te dhenave (Data Processing), ku kompjuteri kryen llogaritje dhe manipulime mbi te dhena te ndryshme. I dyti eshte ruajtja e te dhenave (Data Storage), qofte afatshkurte ne memorien kryesore apo afatgjate ne disk. I treti eshte levizja e te dhenave (Data Movement), qe nenkupton transferimin e te dhenave ndermjet komponenteve te brendshme dhe pajisjeve periferike si tastiera, ekrani apo rrjeti. I katerti eshte kontrolli (Control), ku Control Unit menaxhon dhe koordinon ekzekutimin e tre funksioneve te tjera ne perputhje me instruksionet e programit.

Eshte e rendesishme te kuptosh dallimin mes struktures dhe funksionit. Funksioni i pergjigjet pyetjes "cfare ben kompjuteri" — pra operacionet qe kryen. Struktura i pergjigjet pyetjes "si eshte organizuar" — pra lidhjet fizike dhe logjike ndermjet komponenteve. Cdo komponent ka edhe strukturen e vet te brendshme, keshtu qe kemi nje hierarki strukturash te nderfutura.

---

## Komponentet e CPU-se dhe Regjistrat

CPU-ja perbehet nga kater module kryesore. ALU (Arithmetic Logic Unit) kryen te gjitha operacionet aritmetike dhe logjike. CU (Control Unit) koordinon dhe kontrollon veprimet e te gjithe komponenteve te tjere. Regjistrat jane memoriet me te shpejta qe gjenden brenda CPU-se dhe ruajne te dhena te perkohshme gjate ekzekutimit. Intern BUS lidh te gjithe keto module brenda CPU-se per transferim te te dhenave.

Regjistri PC (Program Counter) ruan adresen e instruksionit te ardhshem qe do te ekzekutohet. Pas cdo leximi te instruksionit, PC perdisohet automatikisht per te treguar instruksionin pasardhes, dhe nese ekzekutohet nje instruksion jump ose branch, PC merr vleren e re. Regjistri IR (Instruction Register) ruan instruksionin aktual qe po ekzekutohet. Regjistri MAR (Memory Address Register) ruan adresen e vendndodhjes ne memorie me te cilen behet qasja — kur CPU deshiron te lexoje ose shkruaje ne memorie, vendos adresen ne MAR dhe kjo dergohet permes Address BUS tek memoria. Regjistri MBR ose MDR (Memory Buffer/Data Register) ruan te dhenen aktuale qe po transferohet nga ose ne memorie. Akumulatori (Accumulator) eshte regjistri kryesor i te dhenave qe ruan rezultatet e ndermjetme te operacioneve aritmetike dhe logjike te kryera nga ALU.

---

## BUS-i i Sistemit

BUS-i i sistemit perbehet nga tri komponentet kryesore. Data BUS transporton te dhenat ndermjet CPU-se, memories dhe pajisjeve I/O dhe eshte dykaheshem. Address BUS transmeton adresat e memories ose pajisjeve me te cilat komunikohet — eshte njekaheshem, vetem nga CPU. Control BUS transmeton sinjalet e kontrollit dhe koordinimit si lexim, shkrim dhe nderprerje, dhe ka linja qe shkojne ne te dyja drejtimet.

Gjeresia e Data BUS (8, 16, 32 ose 64 bit) ndikon drejtperdrejt ne performanse, ndersa gjeresia e Address BUS percakton sa memorie mund te adresohet. Problemi i nje bus-i te vetem eshte se kur ka shume pajisje, krijohen vonesa dhe konflikte. Per kete arsye sistemet reale perdorin nje hierarki bus-esh — local bus, system bus dhe expansion bus.

Kur ka shume masters si CPU dhe DMA controller, vetem njeri mund te perdore bus-in ne nje moment. Kjo zgjidhet me arbitrim, qe mund te jete i centralizuar (nje arbiter i vetem jep leje) ose i shperndare (modulet negociojne mes vete). BUS-i mund te jete i dedikuar (linja te ndara per te dhena dhe adresa) ose i multipleksuar (ndajne te njejtat linja, me pak tela por kontroll me kompleks).

---

## Cikli i Instruksionit dhe Nderprerjet

Cdo instruksion kalon neper dy faza kryesore. Faza e pare eshte Fetch, ku vlera e PC vendoset ne MAR, lexohet memoria, instruksioni kalon ne MBR pastaj ne IR, dhe PC inkrementohet. Faza e dyte eshte Execute, ku dekodohet instruksioni nga IR dhe kryhet operacioni perkates — qofte transferim te dhenash, operacion aritmetik, I/O ose kontroll (jump).

Nderprerjet (Interrupts) lejojne module te jashtme te nderhyjne ne ekzekutimin normal. Nderprerjet e programit shkaktohen nga kushte te jashtezakonshme si ndarja me zero, overflow ose qasje ne adrese te pavlefshme. Nderprerjet e timer-it (klokut) shkaktohen nga sinjale periodike per menaxhimin e multitasking. Nderprerjet e I/O shkaktohen nga pajisjet kur perfundojne nje operacion si leximi i diskut. Nderprerjet e harduerit shkaktohen nga gabime fizike si gabimi ne memorie.

Kur ndodh nje nderprerje, CPU ruan kontekstin aktual (PC dhe regjistrat), ngarkon adresen e interrupt handler-it ne PC, ekzekuton handler-in, pastaj rikthjen kontekstin dhe vazhdon programin normal. Nese ka nderprerje te shumefishta, ato mund te trajtohen sekuencialisht (duke disable nderprerjet e tjera gjate trajtimit) ose me prioritet ku nje nderprerje me e rendesishme mund te nderprese edhe handler-in e nje nderprerje tjeter.

---

## Evolucioni i Kompjuterit

Kompjuteri i pare elektronik ishte ENIAC (1946), i ndertuar ne Universitetin e Pensilvanise. Perdorte 18,000 tuba vakum, peshonte 30 ton dhe mund te kryente 5,000 mbledhje ne sekonde. Programohej manualisht permes switch-eve.

Hapi vendimtar erdhi me konceptin e von Neumann-it dhe makinen IAS (1952), ku per here te pare programi dhe te dhenat ruhen ne te njejten memorie kryesore. Kjo arkitekture eshte baza e te gjitha kompjutereve moderne.

IBM 701 (1953) ishte kompjuteri i pare i IBM me program te ruajtur. IBM 360 (1964) prezantoi familjen e pare te kompjutereve te pajtueshëm me instruction set te njejte. DEC PDP-8 (1964) ishte minikompjuteri i pare dhe prezantoi bus structure (Omnibus). Intel 4004 (1971) ishte mikroprocesori i pare — gjithe CPU ne nje chip te vetem, 4-bit.

Evolucioni i x86 nisi me 8080, pastaj 8086 (16-bit), 80386 (32-bit, multitasking), 80486 (pipeline + FPU), Pentium (superscalar), Pentium Pro (out-of-order execution), e deri te Core 2 Quad me 4 berthamb dhe 820 milion tranzistora.

Gjeneratat e harduerit nisen me tuba vakumi (1946–1957), pastaj tranzistora (1958–1964), integrimi ne shkalle te vogel SSI/MSI, pastaj LSI (deri 100K pajisje per chip), VLSI (deri 100M) dhe ULSI (100M+) nga 1991 e tutje. Tranzistoret zevendesuan tubat e vakumit sepse ishin me te vogla, me te lira dhe prodhonin me pak nxehtesi.

Ligji i Moore-it thote qe numri i tranzistoreve ne nje chip dyfishohet çdo 18 muaj, ndersa kostoja mbetet pothuajse konstante. Densiteti me i larte do te thote rruge me te shkurtra elektrike, performanse me e larte dhe madhesi me e vogel.

Ligji i Amdahl-it kufizon perfitimin nga processoret paralel. Formula eshte Speedup = 1 / [(1-f) + f/N], ku f eshte fraksioni i kodit qe mund te paralelizohet dhe N eshte numri i procesoreve. Nese pak kod eshte i paralelizueshem, shtimi i procesoreve ndihmon shume pak. Kur N shkon ne infinit, speedup kufizohet nga 1/(1-f).

Problemi kryesor i performanses eshte se shpejtesia e CPU-se rritet shume me shpejt se ajo e memories. Zgjidhjet perfshijne cache on-chip, DRAM me te gjere qe merr me shume bite njekohesisht, dhe bus me te shpejte.

---

## Hierarkia e Memories

Memoria e kompjuterit eshte e organizuar si nje hierarki. Ne maje jane regjistrat e CPU-se — me te shpejtet dhe me te shtrenjtit. Pastaj vjen L1 Cache (SRAM, on-chip), L2 Cache, L3 Cache, memoria kryesore (DRAM), cache e diskut, disku (HDD/SSD), memoriet optike (CD/DVD), dhe ne fund shiritat magnetik qe jane me te ngadalshmit dhe me te liret.

Duke zbritur ne hierarki bie cmimi per bit, rritet kapaciteti, rritet koha e qasjes, dhe bie frekuenca e qasjes nga procesori. Kjo eshte arsyeja pse ekziston hierarkia — nese do te kishte nje memorie qe ishte njekohesisht e shpejte, e madhe dhe e lire, nuk do te kishim nevoje per hierarki.

Ka kater metoda qasjes ne memorie. Qasja sekuenciale (Sequential) perdoret ne shiritat magnetik — per te arritur te dhenat duhet kaluar neper te gjitha te dhenat para tyre. Qasja direkte (Direct) perdoret ne disqe HDD — koka leviz ne nje zone te pergjithshme pastaj kerkon sektorin e duhur. Qasja e rastesishme (Random) perdoret ne RAM — cdo lokacion eshte i arritshëm drejtperdrejt ne kohe konstante. Qasja asociative (Associative) perdoret ne cache — te dhenat gjenden permes krahasimit te etiketave, jo permes adreses.

---

## Memoria Cache

Cache eshte nje memorie e vogel dhe e shpejte qe vendoset ndermjet CPU-se dhe memories kryesore. Ajo ruan te dhenat qe jane perdorur se fundmi ose qe perdoren shpesh. Kur CPU kerkon te dhena, se pari kontrollon cache-in. Nese te dhenat gjenden aty, quhet "hit" dhe qasja eshte e shpejte. Nese nuk gjenden, quhet "miss" — blloku sillet nga memoria kryesore, vendoset ne cache, dhe pastaj dergohet ne CPU.

Dizajni i cache-it perfshin gjashte vendime kryesore: madhesia e cache-it (me shume eshte me mire deri ne nje pike), funksioni i mapimit, algoritmi i zevendesimit, politika e shkruarjes, madhesia e bllokut (optimale 8-32 bajta), dhe numri i cache-ve (L1/L2/L3, te ndara ose te bashkuara).

### Funksionet e Mapimit

Mapimi direkt (Direct Mapping) eshte metoda me e thjeshte. Cdo bllok i memories kryesore mapohet ne vetem nje linje te caktuar te cache-it, sipas formules i = j mod m. Adresa ndahet ne tri fusha: Tag, Line dhe Word. Fusha Line identifikon ne cilin linje te cache-it mapohet blloku, fusha Tag krahasohet per te verifikuar nese kemi hit, dhe fusha Word tregon pozicionin brenda bllokut. Avantazhi eshte se eshte i thjeshte dhe i lire, por disavantazhi kryesor eshte fenomeni i thrashing — kur dy blloqe qe perdoren shpesh mapojne ne te njejten linje dhe zevendesojne njeri-tjetrin vazhdimisht.

Mapimi asociativ i plote (Associative Mapping) lejon cdo bllok te futet ne cilendo linje te cache-it. Adresa ndahet vetem ne dy fusha: Tag dhe Word — nuk ka fushe Line sepse blloku nuk eshte i kufizuar ne nje linje. Cache-i krahason te gjitha etiketat paralelisht dhe njekohesisht per te gjetur bllokun. Eshte me fleksibel dhe eliminon thrashing, por kostoja e harduerit eshte e larte sepse kerkon krahasim te plote te te gjitha linjave.

Mapimi set asociativ (Set Associative Mapping) eshte kompromisi ideal. Cache-i ndahet ne bashkesi dhe cdo bashkesi permban k linja. Blloku mapohet ne nje bashkesi te caktuar, por brenda asaj bashkesie mund te futet ne cilendo linje. Adresa ndahet ne Tag, Set dhe Word. Kjo redukton thrashing pa kerkuar krahasim te plote te te gjitha linjave, prandaj perdoret ne te gjithe processoret moderne.

Per kerkim, mapimi asociativ eshte me i miri sepse krahason te gjitha etiketat njekohesisht dhe gjetja eshte e menjehershme. Per shfrytezim praktik, mapimi set asociativ eshte me i pershtatshmi sepse balancon shpejtesine me koston.

### Algoritmet e Zevendesimit

Kur cache-i eshte plot dhe duhet vendosur nje bllok i ri, duhet zgjedhur cilin bllok te hiqim. LRU (Least Recently Used) zevendeson bllokun qe nuk eshte perdorur per kohen me te gjate — eshte algoritmi me i perdoruri. FIFO (First In First Out) zevendeson bllokun me te hershme qe ka hyre ne cache. LFU (Least Frequently Used) zevendeson bllokun qe ka pasur me pak qasje. Random thjesht zgjedh rastesisht.

### Politikat e Shkruarjes

Write Through shkruan njekohesisht ne cache dhe ne memorien kryesore. Kjo siguron konsistence — memoria kryesore eshte gjithmone e perditesuar — por gjeneron shume trafik dhe ngadalesohet shkruarje.

Write Back shkruan vetem ne cache. Memoria kryesore perdisohet vetem kur blloku zevendesohet dhe largohet nga cache-i. Kjo eshte me e shpejte, por mund te krijoje inkonsistence — komponentet e tjere si DMA mund te shohin te dhena te vjetruara ne memorien kryesore.

### Nivelet e Cache-it

Processoret moderne perdorin disa nivele. L1 eshte cache-i on-chip, me i shpejti dhe me i vogli, shpesh i ndare ne cache instruksionesh dhe cache te dhenash per te shmangur konfliktet. L2 eshte me i madh por pak me i ngadalshem. L3 eshte edhe me i madh. Si shembull, Pentium 4 kishte L1 prej 8KB, L2 prej 256KB, dhe L3 on-chip.

---

## Memoria e Brendshme

### RAM vs ROM

RAM (Random Access Memory) lejon lexim dhe shkrim te te dhenave dhe eshte volatile — kur hiqet rrymja te dhenat humbasin. Perdoret si memoria kryesore e perkohshme ku ruhen programet dhe te dhenat ne perdorim aktiv.

ROM (Read Only Memory) lejon vetem lexim dhe eshte nonvolatile — te dhenat ruhen edhe pa rryme. Perdoret per firmware dhe BIOS. Ekzistojne variante te ndryshme, por te gjitha jane me te ngadalshme per shkrim se RAM-i.

### Tipet e memories gjysempercuese

RAM Dinamik (DRAM) ruan bitat si ngarkesa kondenzatoresh. Meqe kondenzatori zbrazet me kalimin e kohes, kerkon freskim periodik. Konstruksioni eshte i thjeshte — vetem nje tranzistor dhe nje kondenzator per bit — gje qe e ben te lire dhe me densitet te larte. Per kete arsye perdoret si memoria kryesore.

RAM Statik (SRAM) ruan bitat me qarqe flip-flop, te cilat mbajne gjendjen ne menyre stabile pa asnje operacion shtese. Nuk kerkon freskim. Cdo cel kerkon rreth 6 tranzistora, gje qe e ben me te madh fizikisht dhe me te shtrenjte, por edhe shume me te shpejte. Per kete arsye perdoret per cache.

SRAM eshte me i shpejte se DRAM per tri arsye. Se pari, DRAM kerkon freskim periodik gjate te cilit chip-i eshte i paaftesuar per qasje. Se dyti, leximi ne DRAM eshte destruktiv — ngarkesa humbet gjate leximit dhe duhet restauruar. Se treti, gjendja e kondenzatorit eshte analoge dhe kerkon me shume kohe per interpretim. SRAM, nga ana tjeter, perdor flip-flop qe eshte nje qarc logjik digjital — gjendja eshte gjithmone stabile dhe qasja e menjehershme.

ROM eshte i shkruar gjate prodhimit ne fabrike dhe nuk mund te ndryshohet kurre. PROM (Programmable ROM) programohet nje here nga perdoruesi me nje pajisje speciale — eshte WORM (Write Once Read Many) dhe nuk mund te fshihet me pas. EPROM fshihet duke e ekspozuar ndaj rrezeve ultraviolet ne nivel chip-i dhe pastaj riprogramohet. EEPROM fshihet elektrikisht byte per byte pa e hequr chip-in nga qark, dhe riprogramohet shume here — shkrimi eshte shume me i ngadalshem se leximi. Flash Memory fshihet elektrikisht ne nivel blloku, jo byte per byte si EEPROM — eshte kompromis mes EPROM dhe EEPROM dhe perdoret ne USB, SSD dhe kamera.

Dallimi kryesor mes PROM dhe EEPROM eshte se PROM programohet vetem nje here dhe nuk fshihet, ndersa EEPROM mund te fshihet elektrikisht dhe riprogramohet shume here. PROM perdoret per firmware qe nuk ndryshon kurre, ndersa EEPROM perdoret per BIOS te azhurueshem dhe konfigurime te ruajtura.

### Organizimi i avansuar i DRAM-it

SDRAM (Synchronous DRAM) eshte sinkronizuar me kllokun e jashtme te sistemit, keshtu CPU di sakteisht kur do te jene gati te dhenat dhe mund te beje pune tjeter nderkohe. Mbeshtet burst mode per transferim te blloqeve.

DDR-SDRAM transferon te dhena dy here per cikël kloku — ne tehun rrites dhe ne tehun renes — duke dyfishuar throughput-in efektiv.

Cache DRAM integron nje SRAM te vogel prej 16KB brenda chip-it DRAM. Kjo eshte efektive si per qasje te rastesishme ashtu edhe per transferim serik te blloqeve.

RAMBUS DRAM perdor nje bus me shpejtesi te larte prej 1.6 Gbps me 28 linja dhe protokoll asinkron per bllokun. Perdorej ne processoret Pentium dhe Itanium.

### Korigjimi i gabimeve me Kodin e Hammingut

Ka dy lloje gabimesh ne memorie. Gabimet e harduerit (hard errors) jane defekte permanente fizike. Gabimet e softuerit (soft errors) jane te rastit, nuk shkaktojne demtim permanent, dhe ndodhin zakonisht nga nderhyrje elektromagnetike.

Kodi i Hammingut detekton dhe korrigjon gabime single-bit duke shtuar bite pariteti ne pozicione specifike. Per 8 bite te dhena, duhen 4 bite pariteti (p1, p2, p4, p8), gjithsej 12 pozicione. Pozicionet 1, 2, 4 dhe 8 rezervohen per bitat e paritetit, dhe te dhenat vendosen ne pozicionet e tjera. Cdo bit pariteti mbulon nje grup pozicionesh specifike dhe llogaritet me paritet cift — shuma e biteve te mbuluar duhet te jete cift.

Si shembull, per numrin 10011111, vendosim te dhenat (d1=1, d2=0, d3=0, d4=1, d5=1, d6=1, d7=1, d8=1) ne pozicionet 3, 5, 6, 7, 9, 10, 11, 12. Pastaj llogaritim cdo bit pariteti: p1 mbulon pozicionet 1,3,5,7,9,11 ku bitat jane 1,0,1,1,1 me shume 4 (cift), pra p1=0. p2 mbulon 2,3,6,7,10,11 me shume 4, pra p2=0. p4 mbulon 4,5,6,7,12 me shume 2, pra p4=0. p8 mbulon 8,9,10,11,12 me shume 4, pra p8=0. Rezultati perfundimtar eshte 001000101111.

---

## Memoria e Jashtme

### Disku Magnetik

Disku magnetik perbehet nga nje substrat (fillimisht alumin, sot qelq) i mbuluar me material magnetik si oksid hekuri. Nje koke leximi/shkrimi leviz mbi pllakat qe rrotullohen. Gjate shkrimit, rrymja elektrike permes bobines se kokes krijon nje fushe magnetike qe regjistrohet ne siperfaqe. Gjate leximit, nje koke e vecante magnetorezistive detekton ndryshimet e fushes magnetike.

Te dhenat organizohen ne trasa koncentrike, secila e ndare ne sektore. Madhesia minimale e bllokut eshte nje sektor. Disqet me shume pllaka formojne cilindra — trasat ne te njejten pozite radiale ne te gjitha pllakat. Leximi nga i njejti cilinder shmang levizjen e kokes dhe permireson shpejtesine.

Performansa e diskut matet me kohen e qasjes, qe eshte shuma e kohes se kerkimit (koha per te levizur koken te traseja e duhur) dhe voneses se rrotullimit (pritja qe sektori i duhur te rrotullohet nen koke). Transfer rate eshte shpejtesia e leximit pasi koka eshte ne pozicion.

### RAID

Qasja ne disk eshte shume me e ngadalshme se CPU dhe RAM. RAID (Redundant Array of Independent Disks) perdor disa disqe ne paralel per te permiresuar shpejtesine dhe/ose besueshmërine. Ka 7 nivele (0 deri 6) qe nuk jane hierarkike.

RAID 0 perdor vetem striping pa asnje redundanse — ofron shpejtesi maksimale por asnje toleranse ndaj gabimeve. RAID 1 perdor pasqyrim (mirroring) duke mbajtur dy kopje te te gjitha te dhenave — rikuperimi eshte i lehte por eshte i shtrenjte. RAID 2 perdor Hamming code neper disqe me shume redundanse, por nuk perdoret ne praktike. RAID 3 perdor nje disk te dedikuar per paritet me bit-interleaving dhe ofron transfer rate te larta. RAID 4 perdor block-level parity ne nje disk te vetem, i mire per I/O te medha. RAID 5 eshte si RAID 4 por pariteti shperndahet ne te gjitha disqet — perdoret gjersisht ne servere rrjeti. RAID 6 perdor dy llogaritje pariteti ne disqe te ndare — tre disqe duhet te deshtojne para se te humbasin te dhenat, duke ofruar disponueshmeri te larte.

### Memoriet Optike

CD-ROM eshte nje disk polikarbonat i veshur me alumin. Te dhenat ruhen si pits (gropa) dhe lands (siperfaqe te sheshta) qe lexohen me lazer. Tranzicionet pit/land perfaqesojne bitat. Kapaciteti eshte rreth 650MB ose 70 minuta audio. Eshte vetem per lexim (read-only), me shpejtesi konstante lineare.

CD-R (Recordable) mund te shkruhet nje here — eshte WORM (Write Once Read Many) dhe eshte kompatibel me CD-ROM drives. CD-RW (Rewritable) mund te fshihet dhe rishkruhet shume here duke perdorur material phase-change me dy nivele reflektiviteti.

DVD perdor te njejten teknologji lazeri por me densitet me te larte. Ofron 4.7GB per shtrese dhe mund te arrije deri ne 17GB per disqe me dy ane dhe dy shtresa. Mbeshtet kompresim MPEG per filma te plote.

### Shiritat Magnetik

Shiritat magnetik ofrojne vetem qasje sekuenciale — per te arritur te dhenat duhet kaluar neper te gjitha te dhenat para tyre. Jane shume te ngadalshem por edhe shume te lire. Perdoren kryesisht per backup dhe ruajtje afatgjate te te dhenave.

---

## Metrikat e Performanses

Clock speed (frekuenca e klokut ne Hz) nuk tregon gjithe historine sepse instruksione te ndryshme kerkojne numra te ndryshem ciklesh. MIPS (miliona instruksione per sekonde) dhe MFLOPS (miliona operacione floating-point per sekonde) jane metrika te zakonshme por varen shume nga instruction set, kompajleri dhe hierarkia e memories.

SPEC benchmarks jane programe te standardizuara te krijuara nga System Performance Evaluation Corporation per krahasimin e sistemeve. CPU2006 perdor 17 programe FP dhe 12 integer. Rezultatet llogariten me mesatare gjeometrike te raporteve (koha e sistemit ndaj kohes se makines referuese).

---

## Pika Kyce per Provim

Tri funksionet e mapimit jane direkt, asociativ dhe set asociativ. Direkt mapon 1 bllok ne 1 linje fikse me adrese [Tag, Line, Word] dhe shkakton thrashing. Asociativ lejon bllokun te shkoje ne cilendo linje me adrese [Tag, Word] por eshte i shtrenjte. Set Asociativ ndane cache-in ne bashkesi me adrese [Tag, Set, Word] dhe eshte kompromisi ideal qe perdoret ne processoret moderne.

RAM eshte volatile dhe lejon lexim e shkrim, ndersa ROM eshte nonvolatile dhe lejon vetem lexim. SRAM perdor flip-flop, nuk kerkon freskim, eshte e shpejte dhe perdoret per cache. DRAM perdor kondenzator, kerkon freskim, eshte me e ngadalshme dhe perdoret per memorien kryesore. PROM programohet 1 here, EEPROM fshihet elektrikisht dhe riprogramohet shume here, Flash fshihet ne nivel blloku.

PC ruan adresen e instruksionit te ardhshem. MAR ruan adresen e memories ku behet qasja. Akumulatori ruan rezultatet e ndermjetme te ALU. Kater funksionet e kompjuterit jane perpunimi, ruajtja, levizja dhe kontrolli i te dhenave.

Write Through shkruan ne cache dhe memorie njekohesisht. Write Back shkruan vetem ne cache, ne memorie vetem kur zevendesohet blloku. LRU eshte algoritmi me i perdorur per zevendesim. Per kerkim, asociativi eshte me i miri. Per shfrytezim, set asociativi eshte me i pershtatshmi.

Hierarkia e memories shkon nga regjistrat tek L1, L2, L3 cache, pastaj RAM, disk, optike dhe shirita. Kodi i Hammingut per 8 bite te dhena kerkon 4 bite pariteti ne pozicionet 1, 2, 4, 8 — gjithsej 12 pozicione. RAID 0 ofron shpejtesi pa redundanse, RAID 1 pasqyrim, RAID 5 paritet te shperndare per servere, RAID 6 dy paritime per disponueshmeri maksimale. Ligji i Amdahl-it: Speedup = 1/[(1-f) + f/N] — kufizon perfitimin nga procesoret paralel.
