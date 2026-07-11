# Integrated circuit BOM — HR-2510 family

Reference designators below follow the **Realistic HTX-100** service manual electrical parts list (OCR), which maps cleanly onto the Uniden **HR-2510 / Lincoln / HR-2600 / 2830** family for everything **except display glass and meter implementation**.

Where HR-2510 SM text differs, notes call it out.

## CPU / control board (primary reverse-engineering target)

| Ref (HTX) | Part | Function | Datasheet / notes |
|-----------|------|----------|-------------------|
| IC402 | **UC1208** (HTX) / **UC1201** (HR-2510 SM) | **4-bit single-chip MCU** (Uniden mask ROM). Drives PLL serial data, LCD serial data, key matrix, PTT/TX/mute, beep, VCXO selects, etc. | **No public datasheet** — custom Uniden. Pin functions in SM “semiconductor lead identification” / pin connection pages. DIP vs **QFP** across board revisions. |
| IC401 | **LH5008TP** (HTX) / **IR2429** (HR-2510 LCD path) | Display driver | **LH5008TP**: HTX LCD driver (unique glass). **IR2429**: Sharp **LED/LCD level meter / LCD driver** family used on Uniden LCD path (RF Parts lists IR2429 for HR-2510). Full IR2429 PDF hard to source; related Sharp IR243x level-meter PDF kept as family reference. |
| IC403 | **HD14093BP** | Quad 2-input NAND Schmitt (CMOS) — encoder / pulse shaping for channel knob | ≡ **CD4093B** → `hr2510/ics/CD4093B.pdf` |
| IC404 | **M51953BL** | Power-on **reset** / voltage detector with delay | Mitsubishi/Renesas reset IC. Public PDF scarce; pin view in SM (Vcc, NC, GND, C_delay, OUTPUT). |
| IC405 | **L78M05CV** | **+5 V** regulator for logic | Use any 78M05 family data: `hr2510/ics/LM7805.pdf` (TI LM340/78xx family) |

### MCU interface summary (from HTX-100 circuit description + pin table OCR)

On power-up, reset (IC404) releases the MCU; it then clocks **PLL data** and **LCD display data** to the respective ICs.

Notable UC120x lines (names from HTX SM pin table; verify on your board):

| Group | Examples | Role for a new CPU |
|-------|----------|--------------------|
| PLL | `DATA`, `CLK`, `STB1` / `STB2`, `ENABLE`, `LOCK` | Serial programming of **PLL0305A** (+ related enables) |
| Display | `LCD BUSY`, `LCD STR`, `LCD SI`, `LCD LS`, `LCD C/D`, `LCD Reset` | Serial interface to **display driver** (IR2429 or LH5008TP) |
| TX control | `PTT IN`, `TX OUT`, `MUTE` | TX path and AF mute (CW monitor exception) |
| Front panel | Key matrix rows/cols, `F-K1`/`F-K2` encoder inputs | Channel / mode UI |
| Analog / VCXO | VCXO select outputs, SSB I/O | Carrier / sideband selection |
| Audio | `BEEP` ~2 kHz 50 ms | Sidetone into AF amp |
| Power | Multiple `5V` / `GND` | Logic supply |
| Backup | Super-cap / `BATTERY` terminal | Memory retention (~months on stock design) |

**ROM dumps** (your private material) belong against the specific MCU marking and package.

## PLL / synthesizer board

| Ref | Part | Function | Datasheet |
|-----|------|----------|-----------|
| IC305 | **PLL0305A** | Serial-input PLL frequency synthesizer (NPC / Uniden ecosystem) | `hr2510/ics/PLL0305A.pdf`, `PLL0305A_jmargolin.pdf` |
| IC302 | **SM5152A1** | Prescaler / related synthesizer LSI (NPC SIP-style) | **No good public PDF found** — pin view in SM |
| IC303 | **M54460L** | 1/10, 1/100 high-speed divider (Mitsubishi) | Pin view in SM; PDF scarce |
| IC304, IC306 | **SO42P** (S042P) | Siemens balanced mixer ≤ ~200 MHz | `SO42P.pdf`, `SO42.pdf`, `TBB042.pdf` (TBB042 related) |
| IC307 | **NJM4558S** | Dual op-amp (loop filter / analog) | `NJM4558.pdf` |
| IC301 | **L7808CV** | **+8 V** regulator | 78xx family (`LM7805.pdf` as family reference; use 7808 curves) |

## Main RF / AF board

| Ref | Part | Function | Datasheet |
|-----|------|----------|-----------|
| IC001 | **M5223L** | Dual op-amp / AF pre (Mitsubishi SIP) | Pin view in SM |
| IC002 | **AN612** | Dual NPN transistor array / RF utility (Panasonic) | Pin view in SM |
| IC003 / IC307 | **NJM4558S** | Dual op-amp | `NJM4558.pdf` |
| IC005 | **TDA1905** | Audio power amplifier | Need ST TDA1905 PDF (download blocked on some mirrors); SM pinout present |
| IC006 | **TA7320P** | Toshiba **balanced modulator / demodulator** | `TA7320P.pdf`, `TA7320P_rigpix.pdf` |

Optional / related from SM semiconductor pages:

| Part | Notes |
|------|--------|
| **NJM2902N** | Quad op-amp — appears on HR-2510 SM lead-ID pages | `NJM2902.pdf` |
| **NJM3402AS / NJM3404AS** | Op-amp family (listed in some OCR passes) | Related NJM series |

## Meter / indicator path (display divergence)

| Platform | IC / hardware | Electrical idea |
|----------|---------------|-----------------|
| **Uniden LCD** | **IR2429** (+ LCD glass) | Analog **voltmeter-level** S/RF/SWR voltage feeds the **LCD driver**, which draws an **on-glass bar graph** |
| **HTX-100** | **LB1423N** + discrete LEDs (D601…) | Same **analog voltage / connection intent**, implemented as **LED bar graph** |

For a new display:

- Recreate or sample the **meter analog voltage** (one path).
- Recreate or sniff the **digital LCD serial bus** for frequency / mode / channel segments (second path).
- HTX LED driver data is **not** a drop-in for Uniden glass, but the **source voltage** for the bar is the interoperability point.

## Power / miscellaneous

| Part | Role |
|------|------|
| L78M05CV / L7808CV | Logic 5 V, analog 8 V |
| M51953BL | MCU reset |
| Super capacitor on CPU board | RAM/backup hold time |

## Discrete semiconductors (not full “IC BOM”, but PA-relevant)

From HR-2510 semiconductor lead identification (OCR):

- **MRF-477** (PA-class RF transistor on some builds; related radios sometimes use MRF497)
- 2SC2166, 2SC2086, 2SC2712, 2SC2812, 2SC2814, 2SC3242A, 2SK192A, 2SK302, 2SA733, 2SC945, etc.
- Diodes: 1N4148, 1N4003, 1N60, MV-1YH-S, etc.

Full passives BOM is multi-board (Main PB-111AB, PLL PB-121BA, LCD PB-112AA, NB, FM, SWR, switch, mic, vol/sq, RIT, channel SW, mode SW). Prefer the scanned **parts price list** pages in the service manuals rather than retyping thousands of R/C/L lines here.

## Datasheet inventory (`hr2510/ics/`)

| File | Covers |
|------|--------|
| `PLL0305A.pdf` / `PLL0305A_jmargolin.pdf` | PLL0305A |
| `TA7320P.pdf` / `TA7320P_rigpix.pdf` | Balanced mod/demod |
| `SO42P.pdf` / `SO42.pdf` / `TBB042.pdf` | Balanced mixer |
| `CD4093B.pdf` | HD14093BP equivalent |
| `NJM4558.pdf` / `NJM2902.pdf` | Dual / quad op-amps |
| `LM7805.pdf` | 78xx family regulator reference |
| `IR243x_level_meter.pdf` | Sharp level-meter driver **family** (proxy while IR2429 full sheet missing) |
| `semiconductor_data.pdf` | Misc radio semiconductor catalog snippets |

### Still missing or custom (document from SM / your dumps)

| Part | Status |
|------|--------|
| **UC1201 / UC1208** | Custom Uniden 4-bit MCU — no public datasheet |
| **SM5152A1** | NPC-family LSI — SM pinout only so far |
| **M54460L** | Divider — SM + catalog refs |
| **M51953BL** | Reset IC — SM pinout |
| **M5223L** | Op-amp SIP — SM pinout |
| **AN612** | Array — SM pinout |
| **IR2429** | Uniden LCD driver — commercial NOS listings; full PDF not free-mirrored cleanly |
| **LH5008TP** | HTX LCD driver — SM pinout |
| **LB1423N** | HTX LED bar driver — SM pinout |
| **TDA1905** | AF PA — get ST PDF when mirror allows |

## Cross-model CPU note

Community practice includes **CPU board swaps** between HR-2510 and HR-2600 (frequency range / features follow the MCU firmware). That supports treating the **socket + bus** as stable and the **mask ROM** as the variant — ideal for a modern CPU plug-in once pin timing is captured.
