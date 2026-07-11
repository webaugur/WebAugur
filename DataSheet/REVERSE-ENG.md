# Reverse-engineering plan — new CPU + new display (HR-2510 family)

## Goals

1. **Drop-in CPU** that plugs in place of the original Uniden 4-bit MCU (UC1201 / UC1208 class), or a **replacement CPU board** that presents the same edge interface to the radio.
2. **Intercept display-driver signals** to drive a modern LCD (or OLED) instead of aging zebra-striped glass / IR2429 path.
3. Optionally unify the **S-meter bar** path (analog voltage already present).

## Architecture (stock)

```
  Front panel keys / encoder
           │
           ▼
  ┌────────────────────┐     serial      ┌──────────────┐
  │  UC120x MCU        │───────────────►│ PLL0305A     │──► VCO / LO chain
  │  (mask ROM)        │     DATA/CLK/  │ + SM5152A1   │
  │                    │     STB/ENA    │ + M54460L    │
  │                    │                │ + SO42P ×2   │
  │                    │     serial      └──────────────┘
  │                    │───────────────►┌──────────────┐
  │                    │  LCD BUSY/STR/ │ Display      │──► LCD glass
  │                    │  SI/LS/C/D/RST │ driver       │    (Uniden)
  └────────┬───────────┘                │ IR2429 etc.  │
           │ TX / MUTE / beep           └──────────────┘
           │
           ▼
     RF/AF boards (shared across family)

  Analog S/RF/SWR voltage ──► Uniden: into LCD driver (on-glass bar)
                              HTX-100: into LB1423N → LED bar
```

HTX-100 is **electrically the same radio** for PLL/RF/CPU duties; its **display glass and bargraph implementation differ**, but the **meter voltage and connection** are the interoperability point for bargraph recreation.

## Phase A — Capture the original CPU interface

1. Identify package on your unit: **DIP vs QFP**, marking **UC1201 / UC1208 / other**.
2. From SM pin table + continuity:
   - Map every pin to net name (PLL, LCD, matrix, PTT, supplies, crystals).
3. Scope (logic analyzer preferred):
   - **PLL bus**: DATA, CLOCK, strobe/enable, lock detect polarity and bit order.
   - **LCD bus**: STR / SI / LS / C/D / BUSY / RESET timing relative to keypress and frequency change.
   - **Reset** release vs 5 V rail (M51953BL delay).
4. Dump ROM if possible (your secret material) for behavioral reference; a green-field MCU may reimplement control without the dump once protocols are known.

## Phase B — Drop-in MCU board

Suggested modern approach (WebAugur / ESP32-class or small ARM/RISC-V):

| Stock net | Replacement strategy |
|-----------|----------------------|
| +5 V / GND | Level-shift if using 3.3 V MCU |
| PLL serial | Bit-bang or SPI-like timing matching PLL0305A datasheet + captures |
| LCD serial | Either drive original IR2429 **or** break out to new panel MCU |
| Key matrix | Scan same row/col timing or debounce in software |
| PTT / TX / MUTE | Open-drain or push-pull matching original levels |
| BEEP | PWM into AF path |
| Crystal / clock | Match original clock if any external; else self-clocked MCU |

Mechanical: adapter PCB matching DIP footprint or QFP land pattern; keep crystal and reset compatible so the rest of the radio stays stock.

## Phase C — Display intercept

Two independent signals:

### 1. Digital segment / character path

- Sniff **MCU → IR2429 (or LH5008 on HTX)** serial lines.
- Decode frame format (bit order, latch, which segments = frequency digits / mode icons).
- New display firmware either:
  - **A.** Emulates IR2429 inputs and drives modern glass, or
  - **B.** Ignores IR2429 and has the new CPU speak natively to SPI/I²C LCD.

For a **whole new CPU**, option **B** is simpler long-term; option **A** keeps the original MCU longer.

### 2. Analog bargraph path

- Uniden LCD: meter voltage into driver → **internal LCD bar**.
- HTX: same voltage idea → **LED bar** via LB1423N.
- New display: ADC the meter voltage (same node) and render a bar in software. Calibrate against SM alignment steps (“adjust VR so ‘9’ just lights”).

## Phase D — Full CPU board redesign (optional)

Replace the entire CPU sub-board (MCU + reset + LCD driver + regulator + matrix diodes) with:

- MCU + 5 V LDO
- Optional FPGA/CPLD only if you need cycle-accurate bus playback
- New LCD FPC connector
- Pass-through connectors matching chassis wiring (use HR-2600 / HTX wiring diagrams in `family/schematics/`)

Keep PLL and RF boards stock until control is solid.

## Test order (safe)

1. RX only, dummy load on antenna for any TX tests later  
2. 5 V rail and reset  
3. Key matrix → frequency display on new LCD  
4. PLL lock (TP voltages per SM)  
5. RX audio  
6. TX only after power/ALC/SWR paths verified  

## Documents to keep open while probing

- `hr2510/uniden_hr2510_service_manual_archive.pdf` — voltages, alignment, lead ID  
- `family/schematics/hr2600_cpu_sch.jpg` — CPU sheet clarity  
- `family/schematics/htx100_cpu_sch.png` — pin names often cleaner in OCR SM  
- `hr2510/ics/PLL0305A.pdf` — synthesizer programming  
- `BOM-ICs.md` — IC map  

## Placeholder for your material

| Item | Path suggestion |
|------|-----------------|
| ROM dumps | `DataSheet/private/rom/` |
| Logic captures (Saleae, etc.) | `DataSheet/private/captures/` |
| Board photos / pin maps | `DataSheet/private/photos/` |
| New CPU schematic | `DataSheet/private/cpu-board/` |
