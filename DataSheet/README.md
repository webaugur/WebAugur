# DataSheet — Uniden HR-2510 family reverse-engineering pack

Material for building a **drop-in CPU** (and optionally a full CPU board) plus **display intercept / new LCD** for the Uniden **HR-2510** and electrically related radios.

## Family (same core radio)

These are **mostly the same circuit** with packaging and display differences:

| Branding | Model notes |
|----------|-------------|
| Uniden / President | **HR-2510**, **HR-2600**, **Lincoln**, **2830** |
| Radio Shack / Realistic | **HTX-100** |

### What is shared

- Main RF / IF / AF chain
- PLL / synthesizer (PLL0305A + dividers / mixers)
- Control philosophy: **4-bit Uniden MCU** (mask ROM) driving PLL serial data + display serial data + key matrix + PTT/mute/TX lines
- Reset / 5 V rail / glue logic around the CPU board

### What differs (important for reverse engineering)

| Area | Uniden HR-2510 / Lincoln / 2600 / 2830 | HTX-100 |
|------|----------------------------------------|---------|
| MCU marking | **UC1201** (and variants; DIP or QFP depending on board rev) | **UC1208** |
| Package | DIP on some, **QFP** on others | See service manual |
| Frequency LCD path | **IR2429** (and related LCD glass) | **LH5008TP** LCD driver + HTX-unique glass |
| S / RF / SWR bar | **Analog voltmeter voltage** into the **LCD driver**, which paints an **internal bar graph** on the LCD | Same **analog voltage / connection idea**, but **LED bar graph** driven by **LB1423N** + discrete LEDs |

So for WebAugur-style work:

1. **CPU replacement** — treat the electrical host interface as common; mask ROM / pinout may need per-board verification (DIP vs QFP).
2. **Display replacement** — intercept the **MCU → display-driver serial bus** (and separately the **analog meter voltage** if you want a modern bar/meter). Do **not** assume HTX LED wiring equals Uniden LCD glass.

## Directory layout

```
DataSheet/
  README.md                 ← this file
  BOM-ICs.md                ← integrated-circuit bill of materials + roles
  REVERSE-ENG.md            ← CPU socket, display intercept notes
  hr2510/
    uniden_hr2510_service_manual_archive.pdf   # full SM (Archive.org scan)
    sm_archive.txt                             # OCR text extract
    hr2510_sch_sheet1.gif                      # main schematic sheet 1
    hr2510_sch_sheet2.gif                      # main schematic sheet 2
    ics/                                       # public IC datasheets (where available)
  family/
    htx-100-service-manual.pdf                 # Realistic HTX-100 SM
    htx100_ocr.txt                             # text extract (best IC lists / pin tables)
    president_lincoln_cookbook.pdf             # family mods (M0SKO)
    schematics/
      hr2600_cpu_sch.jpg
      hr2600_main_sch.jpg
      htx100_cpu_sch.png
      htx100_main_sch.png
```

## Sources

- Uniden HR-2510 service manual: [Internet Archive](https://archive.org/details/servicemanualuni00unse)
- Schematics / CBTricks project pages: [HR-2510](https://cbtricks.org/radios/uniden/hr_2510/), [HR-2600](https://cbtricks.org/radios/uniden/hr_2600/), [HTX-100](https://cbtricks.org/radios/realistic/htx_100/)
- HTX-100 SM: [repeater-builder](https://www.repeater-builder.com/radio-shack/htx-100/htx-100-service-manual.pdf)
- Individual IC datasheets: manufacturer / public archives (see `BOM-ICs.md`)

## Your private material

Drop ROM dumps, scope captures, custom pin maps, and board photos under e.g.:

```
DataSheet/private/   # gitignored if needed
```

Do not commit secrets you want kept local without reviewing `.gitignore`.

## Legal / safety

Service manuals and datasheets are for **repair, interoperability, and educational reverse engineering** of equipment you own. Transmit only where licensed and legal. RF and high-current PA stages can destroy gear and cause interference — isolate the CPU/display work from TX until the RF chain is understood.
