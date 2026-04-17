# InkTime - Open Source Smartwatch

InkTime este un smartwatch open source și accesibil, bazat pe un display e-paper și microcontroller-ul nRF52840.

---

## Diagramă bloc

```
┌─────────────┐     ┌─────────────────────────────────────────────────┐
│  USB-C      │─5V─▶│                                                 │
└─────────────┘     │                                                 │
                    │  BQ25180YBGR (LiPo Charger)                    │
┌─────────────┐     │         │ VBAT                                  │
│ AKYGA       │────▶│         ▼                                       │
│ LP502030    │     │  RT6160AWSC (DC/DC) ──▶ 3.3V                   │
│ 3.7V 250mAh │     └─────────────────────────────────────────────────┘
└─────────────┘                    │ 3V3
                                   ▼
                    ┌──────────────────────────────┐
                    │         nRF52840              │
                    │      MCU Principal            │
                    │      Bluetooth 5.0            │
                    └──────────────────────────────┘
                         │        │        │        │
                       SPI       I2C     GPIO      SWD
                         │        │        │        │
                    ┌────┘   ┌───┘    ┌───┘    ┌───┘
                    ▼        ▼        ▼        ▼
              e-Paper    ICM42421  FIT0774   Debug
              1.54"      IMU      Vibrații   SWD
              200x200               +
                                  3x Butoane
```

---

## BOM (Bill of Materials)

## BOM (Bill of Materials)

| Referință   | Componentă      | Model                  | Cantitate | Link achiziție                                                                  | Datasheet                                                                                                      |
| ----------- | --------------- | ---------------------- | --------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| U1          | MCU             | nRF52840               | 1         | [JLC](https://jlcpcb.com/partdetail/NordicSemiconductor-nRF52840_QIAA/C190794) | [Datasheet](https://4donline.ihs.com/images/VipMasterIC/IC/NRSA/NRSA-S-A0021008244/NRSA-S-A0021008244-1.pdf)  |
| IC1         | LiPo Charger    | BQ25180YBGR            | 1         | [JLC](https://jlcpcb.com/partdetail/TexasInstruments-BQ25180YBGR/C2681658)     | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf)                                                     |
| IC2         | Haptic Driver   | DRV2605YZFR            | 1         | [JLC](https://jlcpcb.com/partdetail/TexasInstruments-DRV2605YZFR/C2683503)     | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf)                                                     |
| IC3         | IMU             | BMA421                 | 1         | [JLC](https://jlcpcb.com/partdetail/Bosch-BMA421/C558030)                      | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma421-ds000.pdf)    |
| IC9         | DC/DC Converter | RT6160AWSC             | 1         | [JLC](https://jlcpcb.com/partdetail/Richtek-RT6160AWSC/C2842937)               | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf)                                |
| U3          | Fuel Gauge      | MAX17048G+T10          | 1         | [JLC](https://jlcpcb.com/partdetail/MaximIntegrated-MAX17048GT10/C521500)      | [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf)         |
| D3          | ESD Protection  | USBLC6-2SC6Y           | 1         | [JLC](https://jlcpcb.com/partdetail/STMicroelectronics-USBLC62SC6Y/C2687116)   | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf)                                             |
| J4          | USB-C Connector | KH-TYPE-C-16P          | 1         | [JLC](https://jlcpcb.com/partdetail/KH-TYPE-C-16P/C2765186)                    | [Datasheet](https://datasheet.lcsc.com/lcsc/2108132230_Korean-Hroparts-Elec-TYPE-C-31-M-12_C165948.pdf)        |
| J1          | E-Paper Connector | 503480-2400E         | 1         | [JLC](https://jlcpcb.com/partdetail/Molex-5034802400/C2838772)                 | [Datasheet](https://www.molex.com/en-us/products/part-detail/503480-2400)                                      |
| ANT1        | Antenă BLE      | 2450AT18B100E          | 1         | [JLC](https://jlcpcb.com/partdetail/Johanson-2450AT18B100E/C89854)             | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E/2450AT18B100E.pdf)                     |
| X1          | Crystal 32MHz   | SMD 2016               | 1         | [JLC](https://jlcpcb.com/partdetail/YXC-X322532MSB4SI/C9002)                   | [Datasheet](https://datasheet.lcsc.com/lcsc/1811151223_YXC-X322532MSB4SI_C9002.pdf)                            |
| X2          | Crystal 32.768kHz | SMD 2016             | 1         | [JLC](https://jlcpcb.com/partdetail/Epson-FC135R32768CA2/C32346)               | [Datasheet](https://datasheet.lcsc.com/lcsc/1811021129_Epson-FC-135R_C32346.pdf)                               |
| SW1-3       | Buton tactil    | EVP-AKE31A             | 3         | [JLC](https://jlcpcb.com/partdetail/Panasonic-EVPAKE31A/C255509)               | [Datasheet](https://industrial.panasonic.com/cdbs/www-data/pdf/ATV0000/ATV0000COL8.pdf)                        |
| J2          | SWD Connector   | TC2030-IDC             | 1         | [JLC](https://jlcpcb.com/partdetail/TagConnect-TC2030IDC/C5396715)             | [Datasheet](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-IDC_1.pdf)                   |
| Q2          | MOSFET P-ch     | DMG2305UX-7            | 1         | [JLC](https://jlcpcb.com/partdetail/Diodes-DMG2305UX7/C378960)                 | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf)                                            |
| Q3          | MOSFET N-ch     | SI1308EDL-T1-GE3       | 1         | [JLC](https://jlcpcb.com/partdetail/Vishay-SI1308EDLT1GE3/C409594)             | [Datasheet](https://www.vishay.com/docs/63383/si1308edl.pdf)                                                   |
| D2, D4, D5  | Diodă Schottky  | MBR0530                | 3         | [JLC](https://jlcpcb.com/partdetail/Micro-Commercial-MBR0530/C14679)           | [Datasheet](https://www.mccsemi.com/pdf/Products/MBR0530(SOD-123).pdf)                                         |
| L1          | Inductanță 0.47µH | FTC252012SR47MBCA    | 1         | [JLC](https://jlcpcb.com/partdetail/Taiyo-Yuden-FTC252012SR47MBCA/C408369)     | [Datasheet](https://datasheet.lcsc.com/lcsc/2202251530_TAIYO-YUDEN-FTC252012SR47MBCA_C408369.pdf)              |
| L2          | Inductanță 15nH | RF matching 0201       | 1         | [JLC](https://jlcpcb.com/partdetail/Sunlord-SDCL0201C15NJTDF/C389547)          | [Datasheet](https://datasheet.lcsc.com/lcsc/2108131930_Sunlord-SDCL0201C15NJTDF_C389547.pdf)                   |
| L3          | Inductanță 3.9nH | RF matching 0201      | 1         | [JLC](https://jlcpcb.com/partdetail/Sunlord-SDCL0201C3N9STDF/C389544)          | [Datasheet](https://datasheet.lcsc.com/lcsc/2108131930_Sunlord-SDCL0201C3N9STDF_C389544.pdf)                   |
| L5          | Inductanță 68µH | EPD driver SMD         | 1         | [JLC](https://jlcpcb.com/partdetail/Sumida-CDMC4D28NP680MC/C408336)            | [Datasheet](https://datasheet.lcsc.com/lcsc/2202251530_Sumida-CDMCDRC4D28NP_C408336.pdf)                       |
| L7          | Inductanță 10µH | DC/DC SMD              | 1         | [JLC](https://jlcpcb.com/partdetail/Sunlord-MWSA0402S100MT/C408352)            | [Datasheet](https://datasheet.lcsc.com/lcsc/2108131930_Sunlord-MWSA0402S100MT_C408352.pdf)                     |
| C*          | Condensatoare   | 100nF decuplare 0201   | multiple  | [JLC](https://jlcpcb.com/partdetail/Samsung-CL03B104KO3NNNC/C15195)            | -                                                                                                               |
| C*          | Condensatoare   | >100nF bulk 0402       | multiple  | [JLC](https://jlcpcb.com/partdetail/Samsung-CL05A475KP5NRNC/C25771)            | -                                                                                                               |
| R*          | Rezistențe      | Diverse valori 0201    | multiple  | [JLC](https://jlcpcb.com/partdetail/UNI-ROYAL-0201WAF1001T5E/C17513)           | -                                                                                                               |

---

## Descriere funcționalitate hardware

### MCU — nRF52840

Microcontroller-ul principal al proiectului. Are integrat Bluetooth 5.0 și suportă comunicație SPI, I2C și GPIO. Rulează la 3.3V și consumă ~1.5mA în mod activ.

### Display e-Paper 1.54" (Waveshare V2)

Display electrofotoreic bistabil cu rezoluție 200×200 pixeli. Conectat prin interfață SPI 4 fire. Consumul în deep sleep este sub 2μA — ideal pentru un smartwatch. Imaginea rămâne afișată fără consum de energie.

### Baterie AKYGA LP502030

Baterie Li-Po de 250mAh, 3.7V nominal. Se conectează direct la test pad-urile de pe PCB (fără conector JST). Asigură autonomie estimată de aproximativ 7 zile în condiții normale de utilizare.

### LiPo Charger — BQ25180YBGR

Circuit integrat de încărcare a bateriei, conectat prin I2C la MCU. Suportă încărcare CC/CV, protecție la supraîncărcare (4.3V) și supradescărcare (2.5V). Tensiunea de intrare vine de la USB-C (VBUS).

### DC/DC — RT6160AWSC

Convertor DC/DC step-down care generează tensiunea de 3.3V necesară MCU-ului și celorlalte componente. Eficiență ridicată, minimizând consumul din baterie.

### IMU — ICM42421

Senzor inertial (accelerometru + giroscop) conectat prin I2C. Folosit pentru detectarea mișcărilor, orientării și numărarea pașilor. Întreruperile sunt conectate la pinii IMU_INT1 și IMU_INT2.

### Motor vibrații — FIT0774

Motor DC de vibrații (10×2.7mm), controlat prin GPIO via un tranzistor. Folosit pentru notificări tactile.

### Butoane

3 butoane fizice conectate direct la GPIO-urile MCU-ului, pentru navigarea în interfață.

---

## Pini nRF52840 utilizați

| Componentă         | Semnal    | Pin nRF52840 | Interfață | Motiv               |
| ------------------ | --------- | ------------ | --------- | ------------------- |
| e-Paper            | EPD_CS    | P0.04        | SPI       | Chip Select display |
| e-Paper            | EPD_BUSY  | P0.13        | GPIO      | Status display      |
| e-Paper            | EPD_RST   | P0.14        | GPIO      | Reset display       |
| e-Paper            | MOSI      | P0.30        | SPI       | Date SPI            |
| e-Paper            | SCK       | P0.29        | SPI       | Clock SPI           |
| I2C (IMU, Charger) | SDA       | P0.06        | I2C       | Date I2C            |
| I2C (IMU, Charger) | SCL       | P0.07        | I2C       | Clock I2C           |
| IMU                | IMU_INT1  | P0.08        | GPIO      | Întrerupere IMU     |
| IMU                | IMU_INT2  | P0.09        | GPIO      | Întrerupere IMU     |
| LiPo Charger       | PMIC_INT  | P0.11        | GPIO      | Întrerupere PMIC    |
| Motor vibrații     | HAPTIC_EN | P0.12        | GPIO      | Enable motor        |
| SWD                | SWDIO     | P1.02        | SWD       | Debug data          |
| SWD                | SWDCLK    | P1.00        | SWD       | Debug clock         |

---

## Calcul consum de energie

| Componentă      | Consum activ      | Consum sleep |
| --------------- | ----------------- | ------------ |
| nRF52840        | ~1.5mA            | ~2μA         |
| e-Paper display | ~8mAs/refresh     | ~2μA         |
| ICM42421 IMU    | ~0.5mA            | ~6μA         |
| BQ25180 Charger | ~50mA (încărcare) | ~5μA         |
| RT6160 DC/DC    | eficiență ~90%    | -            |

**Estimare autonomie:**

- Baterie: 250mAh
- Consum mediu estimat (refresh display 1x/min, BT off): ~0.5mA
- Autonomie estimată: **~500 ore (~20 zile)**

---

## Decizii de design

- Bateria se conectează direct la test pad-uri pe PCB (nu prin conector JST) pentru economisirea spațiului în carcasă.
- PCB-ul are grosimea de 1mm (nu standard 1.6mm) pentru a încăpea în carcasă.
- Antena nRF52840 este plasată spre exteriorul PCB-ului, cu decupaj sub antenă conform cerințelor.
- Componentele sunt plasate exclusiv pe layer-ul TOP.
- Condensatoarele de decuplare 100nF sunt plasate cât mai aproape de pinii de alimentare.
- Via stitching aplicat între planurile de masă TOP și BOTTOM, în special lângă circuitul radio.

---

## Licență

Apache 2.0
