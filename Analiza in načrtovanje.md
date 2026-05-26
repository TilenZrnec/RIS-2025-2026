# Analiza in načrtovanje: Program lojalnosti Maestro

## 1. Podatkovni model

![Podatkovni model](slike/podatkovni_model_sc.png)

## 2. Funkcionalna dekompozicija

![Funkcionalna dekompozicija](https://vip.lavbic.net/plantuml/png/RLL1Rjim4Bpd5Jn6GUD6bD8ajoGe2XH5sW21UorM5cFJaac1ebb0V_0ZyXVdNzqbAPbgUpFisTdPcLtwlhTWxE-gFpkxOCz6hmtiMAktq2hTMycUGFJMQFprWkKBjkmCk7OBQYlOyT22wgq39XNRTzf0fUJxMoCtpC3ni5VQOTN5BwpBsJEowKuqp8YrH4POIQJ0GkTff7M2m06D-9Uk5LP12WT67rGZsFcNowiyI_2S_HH6liecuCIbWhxLg8oGF4KMxBEkHIkjS6n0bTX4h8aPxBuF-3B5bwZy_Kt6aHlqIgv4QX1LN6VZe7pcKEs7zlvqHh_ADZWIa3Zaajmp0PiOI8A2Wq5GaILL75F26N4snx34Gkindr9CWQBH0GlEpxaMVHkORb9KoPP6XEQw5zXwWuGrs4Ox3xxDVEfymPv4Buu7LAChz8vwdp-2NMregfNePHoVHa9njQwayxGZNMpAVRAFnl-ceDbtivvomCyKcq_awmk9oAycRiuUdDKxi788AIlfz1R7EuKnzU5XupH98GV6U3iZQcRryO3zICNEzvWMX7j_uxIQq1i81ksmBf3unFDYabMm7rK8An2pmU75w4fCd0MAYPoILTutESnAoEsxB2dWoPO6IAT7tKesUwyX3_aKYtWiSjo7ORgK8BBmkOXr6uPGMx-Hq5ZfKjiA7yy8770gYYSgZDINaHaoBs7Qki8VuLIhHEyJLMnOOmXEgXWuhTR_gMwoKIndtm01Uatr2l15gccsy3PD_NYYDUmIRKr40Vk8j-f985Bu9hTPKezzjC5xmqU08fwYoE-2iUM2FTGxgSRtfsz6XuFIaYuukVOUWMBNq_j7tygMz8ygOj_IUvnMHUWfN3mkeBR1bkanAXT_lownSjfIc6zfGdATaXGpadzSEfLndes3SlOTwa6R3mmKu5gIIdhKjCHjhwTTBsUf12NQnPndr5TPKi9d9UYYR99rIQhoj8eUkNQxhvGuwQ0p6TyJVsp3p0YEPtjsBrID_tT_0000)

## 3. Diagram prehajanja stanj med statusi

![Diagram prehajanja stanj](https://vip.lavbic.net/plantuml/png/ZP7FQeD04CRlFiNS2WNR4EWbM45JAIr1GTCUOZniT4d5P5TcDT24FaBVgWzML-EVJGtqDhlxpVpc-rPaASi9aZs8WXGKJXg0JU9iYxnaoPplE1h6Ylnyqu9cfWcBTFjHF2Fvb3gE2SLQ0qy4is1NJQZVlZGjl8r0rqtm1EC7bi8CmdOCvCXZAWZSLrLwED8YhUcbY3JjfkhI8LXQxgip0ozmlNVtHTvfpOBjl5C5QMl92P1kiEOmafKvc8_mC9c0y-OhidwTHcPqgWjou3kshv1RWYItjHovgcoFSTSidDx1dWeQ4pYId2DD8gNJHiw8A8RRJF_Rs_iDHHfJ15vjoIY8vlvFagk5jHowQRQMNffgBCC-d5vppUqbp_OzrmeKP6TNKhl3jL-fXoggaIy-nmLh1cE1AUs3jDN8DkU7w1S0)

> **Opomba:** Ko stranki dodeljujemo točke zvestobe, ji najprej spremenimo status, v kolikor izpolnjuje pogoje in šele potem dodelimo ustrezno število točk.

## 4. Odločilna tabel za točkovanje

| Znesek nakupov v mesecu | Osnovni | Bronasti | Srebrni | Zlati |
| ----------------------- | ------- | -------- | ------- | ----- |
| **do 200 EUR**          |       5 |        0 |     7.5 |    10 |
| **200 EUR – 1000 EUR**  |      10 |        5 |      15 |    20 |
| **nad 1000 EUR**        |      20 |       10 |      30 |    40 |

## 5. Zaslonske maske

[Zaslonske maske narejene v orodju Figma](https://www.figma.com/make/qoBau0vFBUvsg8QTCeWloy/Loyalty-Program-Web-App-UI?t=EnjvQbXlX6Pe8hvX-0&preview-route=%2Fdashboard
)

## 6. API načrt

Vsi API-ji v tem poglavju opisujejo komunikacijo med našim sistemom in **zunanjimi sistemi**. Gre za integracijske točke, ki jih naš backend kliče kot odjemalec oziroma ki jih zunanji sistemi kličejo na našem backendu.

### 6.1 Integracija s Poslovnim IS Maestro

Poslovni IS Maestro je obstoječi sistem, ki hrani podatke o opravljenih nakupih strank v fizičnih in spletnih prodajalnah. Naš sistem ga kliče enkrat mesečno v sklopu batch procesa (F-09, F-10).

**`GET /external/poslovni-is/api/v1/nakupi`** – Pridobitev mesečnih nakupnih podatkov za vse stranke programa lojalnosti za pretekli mesec.
* **Response:**
```json
[
  {
    "ID_stranke_IS": "MAE-00012345",
    "e_naslov": "janez.novak@maestro.si",
    "mesec": 12,
    "leto": 2024,
    "skupni_znesek_eur": 540.50
  },
  {
    "ID_stranke_IS": "MAE-00067890",
    "e_naslov": "ana.kovac@example.com",
    "mesec": 12,
    "leto": 2024,
    "skupni_znesek_eur": 1250.00
  }
]
```

### 6.2 Integracija z E-poštnim sistemom (SMTP / Mail API)

E-poštni sistem se uporablja na dveh mestih: ob registraciji stranke za verifikacijo e-naslova (F-02) ter ob ponastavitvi gesla (F-07). Naš backend komunicira s poštnim strežnikom prek standardnega SMTP-protokola ali REST API-ja ponudnika transakcijske pošte (npr. SendGrid, Mailgun).

**`POST /external/mail/api/v1/send`** – Pošiljanje verifikacijskega e-sporočila ob registraciji (F-02).
* **Payload:**
```json
{
  "from": "noreply@maestro.si",
  "to": "janez.novak@maestro.si",
  "subject": "Potrdite vaš e-naslov – Maestro program lojalnosti",
  "template_id": "verifikacija_emaila",
  "template_data": {
    "ime": "Janez",
    "verifikacijska_povezava": "https://lojalnost.maestro.si/verify?token=abc123xyz"
  }
}
```

* **Response:**
    
```json
{
  "status": "queued",
  "message_id": "msg-789456"
}
```

**`POST /external/mail/api/v1/send`** – Pošiljanje e-sporočila za ponastavitev gesla (F-07).
* **Payload:**
```json
{
  "from": "noreply@maestro.si",
  "to": "janez.novak@maestro.si",
  "subject": "Ponastavitev gesla – Maestro program lojalnosti",
  "template_id": "ponastavitev_gesla",
  "template_data": {
    "ime": "Janez",
    "povezava_za_ponastavitev": "https://lojalnost.maestro.si/reset-password?token=def456uvw"
  }
}
```
* **Response:**
```json
{
  "status": "queued",
  "message_id": "msg-789457"
}
```
* **Opomba:** Veljavnost žetonov za verifikacijo in ponastavitev gesla je časovno omejena (npr. 24 ur). Žetoni so shranjeni v interni bazi do uveljavitve ali preteka.

### 6.3 Integracija s Poštno / Logistično storitvijo

Po uspešni registraciji in verifikaciji stranke naš sistem posreduje zahtevo za tisk in dostavo kartice lojalnosti zunanji poštni oziroma logistični storitvi (F-05).

**`POST /external/posta/api/v1/narocila`** – Oddaja naročila za tisk in dostavo kartice lojalnosti (F-05).
* **Payload:**
```json
{
  "tip_posiljke": "kartica_lojalnosti",
  "prejemnik": {
    "ime": "Janez",
    "priimek": "Novak",
    "ulica": "Slovenska cesta",
    "hisna_stevilka": "1a",
    "postna_stevilka": "1000",
    "kraj": "Ljubljana",
    "drzava": "SI"
  },
  "podatki_kartice": {
    "ID_stranke": 12345,
    "nivo_lojalnosti": "osnovni"
  },
  "referenca": "KARTICA-MAE-12345"
}
```
   * **Response:**
```json
{
  "status": "sprejeto",
  "stevilka_sledenja": "SI123456789SI",
  "ocenjeni_datum_dostave": "2024-12-20"
}
```
* **Opomba:** Naročilo se odda takoj po uspešni verifikaciji e-naslova. Številka sledenja se shrani v interni bazi za morebitne kasnejše reklamacije.

### 6.4 Integracija s Sistemsko uro (Batch sprožilnik)

Mesečni batch proces (F-09) sproži sistemska ura. Gre za interno opravilo (cron job), ki se izvede enkrat mesečno po koncu obračunskega obdobja, brez zunanjega klica.

**Cron izraz:** (vsak 1. v mesecu ob 02:00)

**`POST /internal/batch/izracun-tock`** – Zažene sekvenco:
    1. Branje nakupnih podatkov iz Poslovnega IS (F-10)
    2. Posodobitev statusov strank (F-12, F-13)
    3. Dodelitev točk zvestobe po točkovniku (F-11)
* **Response:**
```json
{
  "status": "zakljuceno",
  "obdelanih_strank": 523147,
  "napak": 0,
  "cas_izvajanja_s": 184
}
```
* **Opomba:** Endpoint je dostopen izključno iz internega omrežja in ni izpostavljen javno. V primeru napake se sproži alarm na e-poštni naslov administratorjev.

## 7. Razredni diagram

### F-06 Prijava v portal

**Razredni diagram:**

![RD](https://vip.lavbic.net/plantuml/png/XPDFQzmm4CNl_XGYlJYa6xRGKbZIXPJ-ImljWY67NXPP6c_6bZL6ajNMfT-zOpcxiUb2Bx9v6h_H-sdi6xITTB8LoaZlRqDmMFO1xXJP0WSGwymlIpwG3KNuPK2KP7IM9L7i7DOooY8uQSNDJGkrnUrMF5DGLUgr-9q9iH8TcHhUjnBDHd0bsiEJ_16yeRcyukLAT0uREU2tqia1L7wv4P5GJriKdCPxItufJrzybPriAFSTELBuPBqiXTwtuBXO3-GixGnvHIUxFyTqjp603UohEEKNrwAns73CXbauCbFKSgrVLjLZr5R-B094RhmZt_IUuaQW3UADU3KF1v6xxsXSsKL13dCwpyLXfF99-eVPyKiY0z8cXzw7A1r-_X6-Go2Requtq43gTZdZUINYq94FCc81DUN3M7yEoY1BC33WMmU39W-7vPd-OJCrb4nZH1GWJ3IggjIbVgHnzsxlP-TA6hQmd0A7B3KB3VPMCe2u41EJ_SCCIf8vMkZzVaHPJJS5v_O3gUPqYCKrf0dC_qUyXllKGI7doMUR36Jn4roykE06s_GyFpyR7c7lGFSryDjf1hBprIU_zVKrlnwnZoOunAEBWDM4hC_-0W00)

**Diagram Zaporedja:**

![Diagram Zaporedja](https://vip.lavbic.net/plantuml/png/pLJ1Qjj04BthAmRd8aC2fHGA9ZOujBHDg6DmdK8fX9KqjbTQxwYxAuNg7_8H_P3sbFnNPrUsB9lqteiHEs_clPbv7bj8NGeZjc3nHuKwnSzehLZZLCcrXXIrGnFP3cNGbgJj7dM5YXwcnj03E5DX58dKaO7134iZSr6h73KiIp8YqR8OWB2mZ1AgT2RCJnZC0qWid17wkxwyt6AkvMGISFC5dAMuMgLlevIqCwHWCAqT2vA5I3dlncobuge-iMWHNVBq1iwILzcRrIho2Owv0aaXF3eyeUlGpBHQ9LcIHT5u3BTMKwqbE2fWF8wY9LnCl9eHK5OQ1EiBGIEWRP5ypx8M3XaKCMwFvd0X6dcR6D5W6Wz3Z8FBVUpv3mArKOXBubcztmgfpY5yVq2hQDYdhpLQ1BpGWYCrI0rhZm7U4ASuayaZ-l1ofydkg2T-AfuQQpHoJlwN9vPYZ2rjgFPHsCtmqDBq9eeuZPFRchAfCQjovCzEINM0BwAsny1K6-qRpB5mi9j-lrQh8xGPzwXx1OAbCknLt-M7Q6LTBOoyVd3Vq96EFxsx3wifS1XfOhYHwuqRmPsNKVBkf1MyrkklNw210xXVicvAiaxKSe4vnOqlUrph-RT_u_yAs8tnrRHWL-GyG9LMhRhz_2jRugQldaTvv0wix9O7C1DPGqRJN95DwTGcEv3dqJiWb8gUVpqzKhyuMyzxTwhO35hmuyx0YzSZcF6W5JirZA7R-ectMtFfbw4zX_tBxF-Zso0MYaTNAJKyiL7OgTV4kQF3NofKO0FaV2PyQj1uls6H48O3bLzrtp6DI51X8wyCDXbPNBVj3O-i4AGLb17li1G5dhZYXQCnypYA-TOQIgM_0tqNXgZmUlXu57o5yBpLovDAfEgsY7nKkzwwVbwyGpQOYiqLA9yUcHDsSkM9p_u2)

### F-18 Pregled kataloga nagrad

**Razredni diagram:**

![RD](https://vip.lavbic.net/plantuml/png/XPFRYjim48Rl_HGYlKaFkwHGA9Wbi10AIzeGIAzwOyRME3kMB1bfrj2KFaBVgWzMMNASEANqne8vwVz-ZRqYqu63LbGQl7zaTbI-C3vLrXpeX6AL-Nd99slumDyrYj9gcMKnABR0eEtnm0wCU7XeiJ6qNejN5TPrzL7yo8Iu4nvF1jeT71t8N7mcpqFuBdpUYiwHWXEDSvOc5c6gUfsBtfACTJQCKB2n7e-weHwN9EEbAjIIcPME8_SEJiOU64o3otaUIT8EjjQHht18azoTBxuQzlE6SkjQqePK05N13MTrWYrNyaDT9zY9IjgI7XLJRt2SFOWOuQE87XrDGDRpacsbS6_POMcIadSYncv8IsirWZdVr3deTOFc6-mZ-T_MV87k0fFKG810myJ82QAkIrdAySMUDeMvCYJ6JF2qpqKOJUOO8xOZFmi4Cw7eYq4pF5ywFGE_-Cl4gmuPCctHeswWDy43AcnOaGqmNiniCFa8ools2IUX_XV7NpaIOzJEbGGZGqZDOgFmWMTgJ9gEo5-7bnaa3OaWP_OhBDulGkawdhVRauiJF3Fs85JAPBSLK_Vg_lxDOdgnG8yE1QIj4gEV_-uEvzcT_BoI0rFO-B1Nlt-fx0y0)

**Diagram Zaporedja:**

![Diagram Zaporedja](https://vip.lavbic.net/plantuml/png/pLJ1Qjj04BtlLmp9nO4qb58eXDYGKkZ1D0IKN8eXZAonijPwHzrTALJ_q8_ekPzGo24q_zNPbQtAgQ7N-YHhtxnvy-QjVALjZGutuEbBGtP9dyXxBEdIwDB2acmWf_o0SkHhjdw7Ua-67owTumS8hY6bS1dOmT6rqsjipPeiD3KxpAskZW0ztAgS6rkW-mf7SmnekCo2Gxkm5nSRMbizcoNSF8FZ9QrMotTgoJOuDWD-xZRQ25yigVgpFsLpECvV5szNAoNATP0kLrWwB32YqoiA0_eAZbUhlCYB0PoqL0V1sG1MqobyDDXQ3O4hc4wLkeLNiwZv77INaoDms1Lag8AjQB19HlH7jnX8m4f9GIoRIrdjTErwnpwZk5TXDz55BuTEHcLpAPkFomhEzMKbnvRwKMboeYmBrkdoFgZh7WAj9y0UjnL3IdUGOqTGSu6XubPyA23Mxfh4YKeADxoi8kqW8rCYVIV21moDlmi2KWMDCCemrz2XzCQAjrrPWVpsPg2MdFx0dagprYtxC5b842tUjOaKxqyNDZxGu8Vex2NkR6-TZEMfivBHli_2Ti3oNneN8sezbfZSp8RTrSbtlLtPPElIIOyHqEqEy_n8KEqjTc45tkds55uAJnfmNuDGSXs0IqD5DDznRkFxOJ7HehyoLRF7ZOpumia3sWvH9il6isSfoCbIkTyfojcRSxZICRmr6tv-Z7hr_PRYlmpwNWY4MCm6eoMH_rxpTKHp5j2k4zFepZpzl75aco1Mjxj5Ds2rEkpCcLoob64X4CeMeTgWQSwUVemlr84zSTwIxCZmtd7Ayl3Pb8-Hp6InwDY9dbgwV__rZL_kyZS0)

### F-20 Pregled statusov strank

**Razredni diagram:**

![RD](https://vip.lavbic.net/plantuml/png/XLF1Yjim4BtxAuRinTlDBYaK2cO9B8I5i8KkpMqlOMoDqubjAKYAeIxzc7vC_wjZAqxi9ErUB7aqy_JcpPChME_GOWLPYSuzUMyvtNjQPaQlk26OHMwSycoqN_aV9K5gIXL5NH1M7ltUFQcAzSfRrF3uMD5MytmES9DHdcUVuMS4S0yvbvxiGjAhhoe1sT1vV64kuW4_aNiiwqrxS2-VEzXPBh3XvouflasWDgp2YV5MyT8LgBSqFWartwZPbm9BiJtiE84lxFoLt5UBZJP2uXoBczeq7EV6LkXxUdt2hx4wBxRYKdCXsXGYeZLbfyzDEbEVyZp84xfYbt6eeLWDbPg26Weq1JVIvJOqq3FVMQpBBUatNBOquvsHwkBGvHGkI8kAD2GjSonGs9BsxCa7jdcUgbGTXhbSh5sNHWcmzbsCAnhEgIKYeM8KexL6Lvfw5EnQMrlQS8kPyPPEDWYwRCY_YcfcMNLJEDGU6RzjZ38rQrm7MTxbtXmA1korJBlkww_GCqFFXf0QshKYHza9hYHlZd-XYRrqHms-ME5bfSagDgVf_dkmgJ4beMwxlNXiauyJGPztwvdN4jW76w2ugdiBqLd6iNZsyF1XSaN4f7k3bYr2Wpjs1BLiTC7l8FK6IbeBA9DoZH0iI0SHD7-7u9-eJmuO_oCZzslK-EStoXhz1G00)

**Diagram Zaporedja:**

![Diagram Zaporedja](https://vip.lavbic.net/plantuml/png/vLJ1Yjim4BtxAuRTInPIwA5GM3OBAQMmfEqkpIq45jaQfufjZIifMkf_w2_rrFvNHrBiEDbiwRqv97OqJ_FcpRonbTAjCA81Yzzsg0lyXDQA3SvhjT5Ge7PeCZj46RGjQTj7lSEQdkR6q1CuiyCi4uKZ0nTpsIYjh3E2Noz0M9XdEUsq5EOxN2oTS3iROvOSejVwvgR1hLQtjp5sEL_2PO5bMRpD2jBEKDs35gPHjLPLW5Jfw01Rm6N-MhufounfAyUPLXkIv9KMZpR2HS35GtG3RiNPoZANkHnmaLPrDleGdCrcS6yrUQt0KGMpMPRDuTLjv7yDgilH81ZHIQonWbI00Xll3aefBno6w2mBa07P6bM9JdrGDOi_kKg7V3hlptx_H2ZZAKmatzhSood4X_Sq1SrOtnTyTSJ9QxG8b4lAjmXA0xM5sd9x--lsA0ZFBJaZrGcr-D-ThOJU8bCRJXUXObkeL2MUePMklXeLliYocgY5Je2tekBVLbdmyAnh6V27Kt2YLM2nOmsn4ml3dJ5gYyrVqWNea7ArZE-hf6SaxOr6o8bg53eUIZmDZr_KElZr0v_dvhwduOVz9n4DF3Ve7dX0wQu9MlNd1m4Ea1C-gedTuMRpKpM4ZQWTi9hubJmtRBcuXjuW84aARXVhKU5QU_PtfwE1krtDqf2F5w0EGy-p74WVnYHm-CZ-7OUFao_EJ-oNVJHEUfnwONhtR98Nrdh2PKxxSllUxPlvI7rR7oNnn8L67EqNO5FHfiPKhw5SatKAHAtOWvuwrSiGAnwveHDV7NgHec1Iacc4ItqQZpNJQetmzHRrIjo5s4cwK4rXZ1dBqy_1iQJiQ3oTiJxuI2MCFWsyM2nDeNpkSaMfLsDqM1uyW75X1H_p8hg6pufha-Wgyi5Sl8dub0RV1sMInASxwzb-sA-5qJeuIlq_x8PIr1RFR8ONRCEZqh7MDKyd_Ojd7c8yIvD4E-iMoZtoIDSqNMlwohrKd0jbclFG8iF-F1sBFm00)
