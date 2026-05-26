# Specifikacija zahtev: Program lojalnosti Maestro

## Kratek opis sistema

Program lojalnosti Maestro je informacijska rešitev, ki omogoča trgovski verigi Maestro nagrajevanje zvestih strank z zbiranjem točk lojalnosti glede na višino mesečnih nakupov. Sistem strankam ponuja štiri nivoje zvestobe (osnovni, bronasti, srebrni, zlati), ki se samodejno posodabljajo glede na nakupno aktivnost. Stranke se registrirajo prek spleta, dostopajo do svojih točk in ugodnosti prek spletnega portala, fizično kartico lojalnosti pa prejmejo po pošti. Administratorji imajo dostop do naprednih poizvedb, statistik in upravljanja pravil programa.

Sistem se integrira z obstoječim poslovnim informacijskim sistemom trgovske verige (vir podatkov o nakupih) in mora biti sposoben podpirati vsaj 500.000 aktivnih članov ter biti primeren za internacionalizacijo.

## 1. Diagram primerov uporabe

![Kontekstni diagram](https://vip.lavbic.net/plantuml/png/ZLR1Rjj64BtpAmQvn3eeCYdBiehOW5qdWD784QAeXnP5CHBNqehKBh6xP14M_OFw8_8lpdzrTYbnILg9wnD7EszsvhsP7QVimO2HeFXwOo3cYgqCboBG2HSPAjm1henKS_CbPO1_CIrth76OuIh1DQkEBHR5EcL1y1fcYkzG2GwO6AQs74vILiXUuCB1gtkcs9fhexX20Nac5Iu5Jru1QhZ_jF1kcU9RBD8jU-Orgxrct_FnBjVnZWjx7vBfpgvlfq5Gfso3oBKjAPLRJ8NKXiCK6I7aA_Wx201Um-Nb9JpzcwBGff0m02cM4bNCnTeUaKkksGer1NlVFmrXdgNC28HzfRYYBC5-Y8Iv51O___d-DEpW8lYLAVx0aygVzGNgL1Re4L47COG59OHgYsBBQWQ-VKtOcYx7fyTSDD333leCRcNCKcvO0IKvKyeMfmsQN3TmPntS26RosrUULhScg0nViGPx7ZdoxDtIYSo8kap91vuUeUqSAZKH2g1A3AO-q7d7Tqmf43i62vVzckcqaV2uutq12yMgRALcnD26BeZH9BBme0kVwmsQrMOWIum9SSA5OMkd8HjWOUGE3KdKErB11APCiwT7wlsb3G6yh4IWENjwJFnTapOmF8KRvT8JcEHPCpyAGT-s3PmsT_CwI7ZQ2H8sv7Jt4R3w831FSe_iD5uO4PjQndBfi7lzIIWM7_0skPqFqEd3S7WeN5hK1YkkTuIlWlcM28UUvfaJ-J0Np_aCRKSm6j4OoYNv42rj7iDEMuHZ3n1CE_e4b6ighU1Ypsv3yx3JAE55ZK-GKfF4k43IFlQGYmva0hz8HID7usDhxYGuyTMwT-X8aMUmo9HzmnndzfEcmylcsphip64uEeQi-SVZ_8_QGQBJkkJALHPxGMf8r6cqA6n1w87ZoNDkeqwJHL4hs-_n6tLQAnfIcwJRV4doPPAN1OkNzgiU43ThfIym6WR_s2tq6przWfQFflpOZfQGPaakukPYwa4VRj_Dxp_MswGQD4vZvZS0nRsvcnzStlKpUlkCE0I6a_dqysmmHPxsdE_q-kw3ZsTTSK25MfPeFkphIkr2OrPjgO_ppzS7xxaBg1E4NErL_tGTs5hw_JxfoZLFEPIkBhQlmV8ECTCqU4urE9dHaZABwGT8yOlC3Mpe-UhP247rQ43_zmsFOoR-w5-vmgh5qlhPrL0zuAtVgtG3ry_7_-pwbmKhiQWpBE2aNisyzqCmhN_AzEgm7GDQzDu-RvlZ86YQ5qsBteMcDMvPRKyQKFftmDjdRNFKCcaIMcRODgEsEVpVJ9LxLVckxjSVGW-IjQGQUxlMpcjs47ylUa2jvScRl93edLYbUKohKCO5rzL_IBINIjnG8mzanjssa2kciVTouY6yURE_E8AVuErRNeM_kdfsFdpX_Eo5yr7dl7AOr0x4o940ZVDZ2TAFzVdm1NpOFlyF)

### Podrobnejši opis PU

### F-06 Prijava v portal

| Polje | Opis |
|---|---|
| **ID** | F-06 |
| **Naziv** | Prijava v portal |
| **Akterji** | Neprijavljen uporabnik |
| **Predpogoj** | Uporabnik ima ustvarjen in verificiran uporabniški račun. |
| **Posledica** | Uporabnik je prijavljen v portal in ima dostop do svojih podatkov. |

**Glavni tok:**

1. Uporabnik odpre prijavno stran portala.
2. Sistem prikaže obrazec za prijavo (polje za e-naslov in geslo).
3. Uporabnik vnese svoj e-naslov in geslo ter potrdi vnos.
4. Sistem preveri, ali e-naslov obstaja v bazi in ali se geslo ujema s shranjenim geslom.
5. Sistem ustvari JWT-žeton in vzpostavi sejo.
6. Sistem preusmeri uporabnika na njegovo nadzorno ploščo.

**Stranski tok – Napačen e-naslov ali geslo:**

1. Koraki 1–3 so enaki kot v glavnem toku.
2. Sistem ugotovi, da e-naslov ne obstaja ali da se geslo ne ujema s shranjenim.
3. Sistem prikaže sporočilo o napaki: *»Napačen e-naslov ali geslo. Preverite vnos in poskusite znova.«*
4. Sistem ne razkrije, ali je napaka pri e-naslovu ali geslu (varnostna zahteva).
5. Uporabnik ima možnost ponovnega vnosa prijavnih podatkov ali prehoda na primer uporabe F-07 (Ponastavitev gesla).

### F-18 Pregled kataloga nagrad

| Polje | Opis |
|---|---|
| **ID** | F-18 |
| **Naziv** | Pregled kataloga nagrad |
| **Akterji** | Prijavljen uporabnik |
| **Predpogoj** | Uporabnik je uspešno prijavljen v portal (F-06). |
| **Posledica** | Uporabnik si je ogledal razpoložljive nagrade in pozna pogoje za njihovo unovčenje. |

**Glavni tok:**

1. Prijavljen uporabnik v portalu izbere razdelek *Katalog nagrad*.
2. Sistem iz baze pridobi seznam vseh aktivnih nagrad.
3. Sistem prikaže katalog nagrad z nazivom vsake nagrade in ceno v točkah.
4. Sistem ob vsaki nagradi prikaže, ali ima uporabnik dovolj točk za unovčenje (vizualno označeno).
5. Uporabnik si lahko ogleda posamezno nagrado in njene podrobnosti.

**Stranski tok – Ni razpoložljivih nagrad:**

1. Koraka 1–2 sta enaka kot v glavnem toku.
2. Sistem ugotovi, da v katalogu ni nobene aktivne nagrade.
3. Sistem prikaže obvestilo: *»Trenutno v katalogu ni razpoložljivih nagrad. Preverite znova kmalu.«*
4. Uporabnik se vrne na nadzorno ploščo.

### F-20 Pregled statusov strank

| Polje | Opis |
|---|---|
| **ID** | F-20 |
| **Naziv** | Pregled statusov strank |
| **Akterji** | Administrator |
| **Predpogoj** | Administrator je prijavljen v administrativni del portala. |
| **Posledica** | Administrator je pridobil pregled nad statusi strank za izbrano obdobje. |

**Glavni tok:**

1. Administrator v administrativnem vmesniku izbere razdelek *Statusi strank*.
2. Sistem prikaže filter za izbiro obdobja (od–do) in opcijski filter za posamezen status (osnovni, bronasti, srebrni, zlati).
3. Administrator vnese željeno obdobje in po potrebi omeji prikaz na določen status ter potrdi iskanje.
4. Sistem prikaže seznam strank z naslednjimi podatki: ime in priimek, trenutni status, datum pridobitve statusa in datum morebitne spremembe.
5. Administrator si po potrebi ogleda podrobnosti posamezne stranke ali izvozi podatke.

**Stranski tok – Ni zadetkov za izbrano obdobje:**

1. Koraki 1–3 so enaki kot v glavnem toku.
2. Sistem ugotovi, da za izbrano kombinacijo obdobja in filtra ni zapisov.
3. Sistem prikaže obvestilo: *»Za izbrano obdobje ni zadetkov. Poskusite z drugačnimi parametri iskanja.«*
4. Sistem ohrani vnesene filtre, da jih administrator lahko prilagodi brez ponovnega vnosa.

## 2. Tehnične zahteve

#### Zmogljivost in razširljivost

- Sistem mora podpirati vsaj **500.000 aktivnih članov** (≥ 70 % strank Maestro).
- Arhitektura mora omogočati **horizontalno skaliranje** za podporo bistveno večjemu številu uporabnikov (internacionalizacija).
- Mesečni batch izračun točk mora biti zmožen obdelati vse aktivne člane v razumnem času (SLA definira naročnik).

#### Podatkovna baza

- Podatkovna baza: **Oracle** (naročnik ima obstoječe licence).
- Shranjevanje zgodovine statusov in točk za vsaj 5 let (skladnost s poslovnimi zahtevami).

#### Varnost

- Registracija z **verifikacijo e-naslova** (double opt-in – preprečevanje zlorabe tujega e-naslova).
- Gesla hranjena s kriptografskim zgoščevanjem (npr. bcrypt, Argon2).
- Komunikacija izključno prek **HTTPS** (TLS 1.2+).
- Avtentikacija z **JWT** ali sejnimi piškotki s kratko veljavnostjo.
- Ločen dostop za stranke in administratorje (RBAC).
- Zaščita pred pogostimi napadi (OWASP Top 10): SQL injection, XSS, CSRF.
- Skladnost z **GDPR** (osebni podatki, pravica do pozabe).

#### Razpoložljivost in zanesljivost

- Ciljna razpoložljivost portala: **99,5 %** (SLA).
- Batch procesi se izvajajo izven prometnih konic (npr. ponoči).
- Beleženje revizijske sledi (audit log) za spremembe točk, statusov in pravil.

#### Jezikovna podpora

- Portal in vsa komunikacija (e-pošta, obvestila) morata biti na voljo v **slovenščini in angleščini**.
- Arhitektura mora podpirati dodajanje novih jezikov brez posegov v izvorno kodo (i18n).

#### Tehnologije uporabniškega vmesnika

- Spletna aplikacija z **modernimi frontend tehnologijami** (npr. React, Angular ali Vue).
- Vmesnik mora biti **intuitiven**, responziven (mobilne naprave) in dostopen (WCAG 2.1 AA).

#### Integracije

- Enosmerna integracija s **poslovnim IS** za branje mesečnih nakupnih podatkov (API ali batch datotečna izmenjava – format in protokol se dogovorita z naročnikom).
- Integracija s **poštno storitvijo** za pošiljanje fizičnih kartic lojalnosti.
- Integracija z **e-poštnim strežnikom** za verifikacijske e-maile in obvestila.

## 3. Slovar izrazov

| Izraz | Definicija |
|---|---|
| **Program lojalnosti** | Sistem nagrajevanja strank na podlagi vrednosti njihovih nakupov v Maestro trgovinah. |
| **Kartica lojalnosti** | Fizična kartica, ki jo stranka prejme po pošti po uspešni registraciji in jo uporablja za identifikacijo ob nakupu. |
| **Točke lojalnosti** | Virtualne enote, ki jih stranka zbira z nakupi in jih lahko zamenja za nagrade iz kataloga. |
| **Status stranke** | Nivo zvestobe stranke v programu. Možne vrednosti: **osnovni**, **bronasti**, **srebrni**, **zlati**. |
| **Točkovnik** | Tabela pravil, ki določa število točk glede na status stranke in višino mesečnih nakupov. |
| **Batch proces** | Avtomatizirani mesečni postopek, ki iz poslovnega IS pridobi nakupne podatke ter za vsako stranko posodobi status in dodeli točke. |
| **Prag nakupa** | Mejna vrednost mesečnega nakupa (v EUR), ki sproži spremembo statusa ali vpliva na število dodeljenih točk. |
| **Napredovanje statusa** | Prehod stranke na višji nivo (npr. iz osnovni v srebrni) ob izpolnitvi nakupnih pogojev. |
| **Znižanje statusa** | Prehod stranke na nižji nivo ob neizpolnjevanju pogojev za ohranitev trenutnega statusa. |
| **Koriščenje točk** | Postopek, s katerim stranka točke zamenja za nagrado iz kataloga programa. |
| **Portal** | Spletna aplikacija, prek katere stranka pregleduje točke, nakupe in katalog nagrad ter koristi točke. |
| **Administracijski portal** | Del portala, namenjen pooblaščenim administratorjem za upravljanje programa, pravil in nagrad. |
| **Poslovni IS** | Obstoječ informacijski sistem trgovske verige Maestro, iz katerega sistem pridobiva podatke o nakupih. |
| **Verifikacija e-naslova** | Postopek, ki potrdi, da stranka ob registraciji navede sebi lasten e-naslov (double opt-in z verifikacijsko povezavo). |
| **Double opt-in** | Varnostni mehanizem, pri katerem stranka svojo registracijo potrdi s klikom na verifikacijsko povezavo, poslano na navedeni e-naslov. |
| **RBAC** | Role-Based Access Control – nadzor dostopa na podlagi vlog (stranka / administrator). |
| **i18n** | Internacionalizacija – arhitekturna podpora za večjezičnost brez posegov v izvorno kodo. |
| **GDPR** | Splošna uredba o varstvu podatkov (EU 2016/679) – pravni okvir za obdelavo osebnih podatkov. |
| **SLA** | Service Level Agreement – dogovor o ravni storitve (razpoložljivost, odzivni čas itd.). |
| **Audit log** | Revizijska sled – zapis vseh pomembnih sprememb v sistemu (spremembe točk, statusov, pravil) za namene sledljivosti. |
