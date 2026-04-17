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

| Componentă      | Model                      | Cantitate | Link achiziție                                                                 | Datasheet                                                                              |
| --------------- | -------------------------- | --------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| MCU             | nRF52840                   | 1         | [JLC](https://jlcpcb.com/partdetail/NordicSemiconductor-nRF52840_QIAA/C190794) | [Datasheet](https://4donline.ihs.com/images/VipMasterIC/IC/NRSA/NRSA-S-A0021008244/NRSA-S-A0021008244-1.pdf?hkey=61A2E4C270F0397D049F8F05BD4F1054)                |
| Display         | Waveshare 1.54" e-Paper V2 | 1         | [Waveshare](https://www.waveshare.com/1.54inch-e-paper-module.htm)             | [Datasheet](https://www.waveshare.com/w/upload/e/e5/1.54inch_e-paper_V2_Datasheet.pdf) |
| Baterie         | AKYGA LP502030             | 1         | [TME](https://www.tme.eu/ro/details/akу0106/acumulatori-li-pol/akyga/)         | [Datasheet](https://www.tme.eu/Document/b9e12bf26ad0ba929a22ab5d58f022cd/AKY0106.pdf)  |
| LiPo Charger    | BQ25180YBGR                | 1         | [JLC](https://jlcpcb.com/partdetail/TexasInstruments-BQ25180YBGR/C2681658)     | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf)                             |
| Motor vibrații  | FIT0774                    | 1         | [TME](https://www.tme.eu/ro/details/df-fit0774/motoare-dc/dfrobot/fit0774/)    | [Datasheet](https://www.mouser.com/pdfDocs/ProductOverview_DFRobot-FIT0774.pdf)                                 |
| USB-C connector | USB4135-GF-A               | 1         | [JLC](https://ar.mouser.com/datasheet/2/837/usb4135-2930399.pdf)     |     - [Datasheet]  (https://ar.mouser.com/datasheet/2/837/usb4135-2930399.pdf) |                                                                                      |

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
