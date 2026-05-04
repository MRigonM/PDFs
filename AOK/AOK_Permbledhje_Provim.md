# AOK - Permbledhje per Provim
> Bazuar ne: Kap. 4 (Cache), Kap. 5 (Memoria e Brendshme), Kap. 6 (Memoria e Jashtme)

---

## PYETJET E PROVIMIT (Kollofjumi i 2-te - Ramadan Dervishi)

### P1: Cilat jane tre funksionet e mapingeve?

**Tre llojet (funksionet) e mapimit:**

| Lloji | Si funksionon | Per / Kunder |
|-------|--------------|--------------|
| **Mapimi Direkt** | Cdo bllok i memories kryesore mapohet ne vetem NJE linje te cache-it (fiks) | + I thjeshte, i lire — Fenomeni *thrashing* |
| **Mapimi Asociativ** | Blloku mund te futet ne CILENDO linje te cache-it | + Fleksibel — Kerkim i shtrenjte (krahason te gjitha tags njekohesisht) |
| **Mapimi Set Asociativ** | Cache ndahet ne bashkesi; blloku mapohet ne cilendo linje brenda bashkesise se dhene | + Kompromis ideal mes dy metodave te mesiperme |

---

### P8: Cili funksion mapimi eshte me i mire per kerkim dhe cili per shfrytezim?

- **Per kerkim (search):** Mapimi **Asociativ** — krahason te gjitha etiketat njekohesisht, me shpejt per te gjetur te dhenat
- **Per shfrytezim (use/performance):** Mapimi **Set Asociativ** — balancon shpejtesine dhe koston, me i pershtatshmi per perdorim praktik
- Mapimi Direkt eshte me i thjesht por me i dobet per kerkim shkak fenomenit *thrashing*

---

### P2: Dallimi mes RAM dhe ROM

| Karakteristika | RAM | ROM |
|----------------|-----|-----|
| Qasja | Lexim dhe Shkrim | Vetem Lexim (Read Only) |
| Volatile? | Po — humbet te dhenat pa rryme | Jo — **Nonvolatile**, ruan te dhenat |
| Perdorim | Memoria kryesore e perkohshme | BIOS, firmware, tabela funksionesh |
| Fshijshme? | Po | Sipas tipit (ROM-nuk, EPROM-UV, EEPROM-elektrikisht) |
| Shpejtesia | E shpejte | Me e ngadalshme per shkrim |

---

### P3: Se paku 4 lloje te memories se brendshme

1. **RAM Dinamik (DRAM)** — Bitat ruhen si ngarkesa kondenzatoresh; kerkon freskim; perdoret per memorien kryesore
2. **RAM Statik (SRAM)** — Perdor flip-flop; nuk kerkon freskim; me i shpejte; perdoret per cache
3. **ROM** (Read Only Memory) — E shkruar gjate prodhimit; permanente; nonvolatile
4. **PROM** (Programmable ROM) — Programohet nje here nga perdoruesi me pajisje speciale
5. **EPROM** — Fshihet me rreze UV (ultraviolet) ne nivel chip; riprogramohet
6. **EEPROM** — Fshihet elektrikisht byte per byte; shkrimi shume me i ngadalshme se leximi
7. **Flash Memory** — Fshihet elektrikisht ne nivel blloku; kompromis mes EPROM dhe EEPROM

> Tabela e tipit (per reference):
> - RAM → Read-write, volatile
> - ROM/PROM → Read-only, nonvolatile
> - EPROM/EEPROM/Flash → Read-mostly, nonvolatile

---

### P4: Dallimi mes RAM Statik dhe RAM Dinamik

| Karakteristika | SRAM (Statik) | DRAM (Dinamik) |
|----------------|--------------|----------------|
| Si ruan bitat | Flip-flop (gjendja e nderprerjesve) | Ngarkesa kondenzatoreshe |
| Freskim | **NUK nevojitet** | Nevojitet freskim periodik |
| Konstruksion | Me kompleks (6 tranzistora/cel) | Me i thjeshte (1 tranzistor + kondenzator) |
| Madhesia fizike | Me e madhe per bit | Me e vogel per bit |
| Cmimi | Me i shtrenjte | Me i lire |
| Shpejtesia | **Me i shpejte** | Me i ngadalshme |
| Perdorim | **Cache** | **Memoria kryesore** |
| Natyra | Digjitale (logjike) | Analoge (nivel ngarkese) |

---

### P6: Pse RAM Statik eshte me i shpejt se RAM Dinamik?

**Arsyeja kryesore:** SRAM perdor **qarqe logjike flip-flop** (si ne mikroprocesor) qe mbajne gjendjen ne menyre stabile pa asnje vonesë shtese.

DRAM perdor **kondenzatore** qe:
1. Zbrazin gradualisht → kerkon **freskim** (refresh)
2. Gjate fresikimit chip-i eshte i paaftesuar → **vonesë shtese**
3. Leximi eshte destruktiv → ngarkesa duhet **restauruar pas cdo leximi**

SRAM: Te dhenat jane gjithnje ne gjendje stabile → qasja eshte e menjehershme.

---

### P5: Zajedhe numrin binar 10011111 me Kodin e Hammingut

**Hapi 1: Identifiko bitat e te dhenave dhe bitat e paritetit**

Me 8 bite te dhena, duhen **4 bite pariteti** (p1, p2, p4, p8) → gjithsej 12 bite.

Pozicioni:  `1  2  3  4  5  6  7  8  9 10 11 12`
Tipi:       `p1 p2 d1 p4 d2 d3 d4 p8 d5 d6 d7 d8`

**Vendos te dhenat** (10011111 → d1=1, d2=0, d3=0, d4=1, d5=1, d6=1, d7=1, d8=1):

```
Poz: 1  2  3  4  5  6  7  8  9 10 11 12
     p1 p2  1 p4  0  0  1 p8  1  1  1  1
```

**Hapi 2: Llogarit bitat e paritetit (paritet cift)**

- **p1** (poz 1,3,5,7,9,11): bitat = 1,0,1,1,1 → shuma=4 (cift) → **p1 = 0**
- **p2** (poz 2,3,6,7,10,11): bitat = 1,0,1,1,1 → shuma=4 (cift) → **p2 = 0**
- **p4** (poz 4,5,6,7,12): bitat = 0,0,1,1 → shuma=2 (cift) → **p4 = 0**
- **p8** (poz 8,9,10,11,12): bitat = 1,1,1,1 → shuma=4 (cift) → **p8 = 0**

**Rezultati final:**
```
Poz: 1  2  3  4  5  6  7  8  9 10 11 12
     0  0  1  0  0  0  1  0  1  1  1  1
```
**Kodi Hamming: 001000101111**

---

### P7: Dallimi mes PROM dhe EEPROM

| Karakteristika | PROM | EEPROM |
|----------------|------|--------|
| Emri i plote | Programmable ROM | Electrically Erasable PROM |
| Programohet | Nje here (WORM) | Shume here |
| Fshihet | Nuk mund te fshihet | Elektrikisht, byte per byte |
| Shpejtesia | — | Shkrimi shume me i ngadalshme se leximi |
| Pajisje speciale | Po (per programim fillestar) | Jo (fshihet/shkruhet normalisht) |
| Perdorim | Firmware e qendrueshme | Konfigurimin e ruajtur, BIOS i azhurueshem |
| Kategoria | Read-only memory | Read-mostly memory |

---

## HIERARKIA E MEMORIES (e rendesishme!)

```
Shpejt/Shtrenjte
      ↑
  Regjistrat (ne CPU)
      ↓
  L1 Cache (SRAM, on-chip)
      ↓
  L2 Cache (SRAM)
      ↓
  L3 Cache
      ↓
  Memoria Kryesore (DRAM)
      ↓
  Cache Disku
      ↓
  Disku (HDD/SSD)
      ↓
  Optike (CD/DVD)
      ↓
  Shirita Magnetik
      ↓
I ngadalshme/Lire/Kapacitet i madh
```

**Duke zbritur ne hierarki:**
- Bie cmimi per bit
- Rritet kapaciteti
- Rritet koha e qasjes
- Bie frekuenca e qasjes nga procesori

---

## MEMORIA CACHE - Detaje

### Operimi i Cache-it
1. CPU kerkon te dhena
2. Kontrolloje cache-in (hit ose miss?)
3. **Hit** → Merr te dhenat nga cache (shpejt)
4. **Miss** → Sill bllokun nga memoria kryesore → vendos ne cache → dergoje ne CPU

### Dizajni i Cache-it (6 elemente)
1. **Madhesia** — me shume cache = me shpejt (deri ne nje mase)
2. **Funksioni i mapimit** — Direkt / Asociativ / Set Asociativ
3. **Algoritmi i zevendesimit** — LRU / FIFO / LFU / Random
4. **Politika e shkruarjes** — Write Through / Write Back
5. **Madhesia e bllokut** — optimum 8-32 bajta
6. **Numri i cache-ve** — L1/L2/L3, unik apo i ndare

### Algoritmet e Zevendesimit
| Algoritmi | Si funksionon |
|-----------|--------------|
| **LRU** (Least Recently Used) | Zevendeso bllokun me te "vjeter" (pa reference me gjate) |
| **FIFO** (First In First Out) | Zevendeso bllokun me te hershme ne cache |
| **LFU** (Least Frequently Used) | Zevendeso bllokun me me pak qellime |
| **Random** | Zevendesim i rastit |

### Politika e Shkruarjes
- **Write Through** — Shkruaj njekohesisht ne cache DHE ne memorien kryesore
  - Pro: Konsistenca
  - Kunder: Shume trafik, shkruarje e ngadalesuar
- **Write Back** — Shkruaj vetem ne cache; shkruaj ne memorie kryesore vetem kur blloku zevendësohet
  - Pro: Me i shpejte
  - Kunder: Inkonsistencë e mundshme

---

## MEMORIA E BRENDSHME - Detaje

### Tipet e Memories Gjysempercuese
| Tipi | Kategoria | Fshihet si | Volatile? |
|------|-----------|-----------|-----------|
| RAM | Read-write | Elektrikisht (byte) | Po |
| ROM | Read-only | Nuk fshihet | Jo |
| PROM | Read-only | Nuk fshihet | Jo |
| EPROM | Read-mostly | UV light (chip-level) | Jo |
| EEPROM | Read-mostly | Elektrikisht (byte) | Jo |
| Flash | Read-mostly | Elektrikisht (block) | Jo |

### SRAM vs DRAM — Krahasim i plote
```
SRAM: Flip-flop → Stabil → Pa freskim → Cache
DRAM: Kondenzator → Zbrazet → Freskim periodik → Memoria kryesore
```

### Organizimi i avansuar i DRAM
- **SDRAM** (Synchronous DRAM) — sinkronizuar me kllokun e sistemit; CPU nuk pret; burst mode
- **DDR-SDRAM** — dërgon te dhena 2 here per cikël kloku (tehu rritës DHE rënës)
- **Cache DRAM** — DRAM me SRAM te integruar (16KB cache brenda çipit DRAM)
- **RAMBUS** — 28 linja, 1.6Gbps, perdoret per Pentium/Itanium

### Korigjimi i Gabimeve (Hamming Code)
- **Gabim hardueri** — Defekt permanent
- **Gabim softueri** — I rastit, jo destruktiv
- Detektimi & korigjimi behet me **Kodin e Hammingut**

---

## MEMORIA E JASHTME - Detaje

### Disku Magnetik
- Substrat i mbuluar me oksid hekuri (sot: qelq)
- Organizimi: **Trasa → Sektore → Blloqe**
- **Shpejtesia/Vonesa e diskut:**
  - Koha e qasjes = **Koha e kerkimit** + **Vonesa e rrotullimit**
  - Koha e kerkimit = koha per zhvendosjen e kokes te traseja
  - Vonesa e rrotullimit = pritja qe sektori te rrotullohet nen koke

### RAID (Redundant Array of Independent Disks)
| Niveli | Karakteristika | Perdorim |
|--------|---------------|----------|
| **RAID 0** | Pa redundanse; striping; shpejtesi max | Performanse e larte |
| **RAID 1** | Pasqyrim (mirroring); 2 kopje | Siguri e larte, i shtrenjte |
| **RAID 2** | Hamming code; shume redundanse | Nuk perdoret ne industri |
| **RAID 3** | 1 disk pariteti; bit-interleaved | Transfer rates te larta |
| **RAID 4** | Block-level parity; disqe te pavarur | I/O te mëdha |
| **RAID 5** | Pariteti distribuohet ne te gjitha disqet | Serverë rrjeti |
| **RAID 6** | Dy paritime; N+2 disqe | Disponueshmëri e larte |

### Memoriet Optike
| Tipi | Qasja | Shkrim | Çmimi |
|------|-------|--------|-------|
| **CD-ROM** | Laser (lexim) | Jo (read-only) | I lire |
| **CD-R** | Laser | Nje here (WORM) | I lire |
| **CD-RW** | Laser | Shume here | Mesatar |
| **DVD** | Laser | Varesisht tipit | — |

- CD-ROM kapaciteti: **650MB** / ~70 min audio
- DVD kapaciteti: **4.7GB per shtrese** (shumështresore)
- Dendesia e paketimit konstante; shpejtësia konstante lineare

### Shiritat Magnetik
- Qasje sekuenciale (serike) — jo random access
- Te ngadalshëm, shume te lire
- Per backup dhe ruajtje afatgjate

---

## METODAT E QASJES NE MEMORIE

| Metoda | Kohë qasjes varet nga lokacioni? | Shembull |
|--------|----------------------------------|---------|
| **Sekuenciale** | Po | Shiritat magnetik |
| **Direkte** | Po (pjeserisht) | Disku HDD |
| **E rastesishme (Random)** | **Jo** | RAM |
| **Asociative** | **Jo** | Cache |

---

## KARAKTERISTIKAT FIZIKE TE MEMORIES

- **Dekompozimi (Decay)** — a prishet info pa energji
- **Paqendrueshmeria (Volatility)** — a humbet info kur hiqet rrymja
- **Fshijshmeria (Erasability)** — a mund te ndryshoje info
- **Shpenzimi i energjise**

---

## PIKAT KYCE PER PROVIM

> **Mapimi Direkt** — 1 bllok → 1 linje (fiks). Struktura: [Tag | Line | Word]
>
> **Mapimi Asociativ** — 1 bllok → cilado linje. Struktura: [Tag | Word]
>
> **Mapimi Set Asociativ** — 1 bllok → cilado linje brenda bashkesise. Struktura: [Tag | Set | Word]

> **RAM** = volatile, lexim+shkrim | **ROM** = nonvolatile, vetem lexim

> **SRAM** = flip-flop, pa freskim, i shpejte → **Cache**
> **DRAM** = kondenzator, freskim, i ngadalshme → **Memorie Kryesore**

> **PROM** = programohet 1 here | **EEPROM** = fshihet elektrikisht, riprogramohet shume here

> **Hamming Code** = detekton dhe korrigjon gabime single-bit ne memorie

> **Hierarkia**: Regjistrat → L1 Cache → L2 Cache → RAM → Disk → Optike → Shirit
