# BGKeeB encoder troubleshooting (both halves)

This config expects a **separate rotary encoder on each half**.

## 1) Firmware-side wiring expectations

Current shield configuration uses:

- Encoder A: `pro_micro` pin **8**
- Encoder B: `pro_micro` pin **9**
- Flags: `GPIO_ACTIVE_LOW | GPIO_PULL_UP`

That means the encoder must be wired as pull-up-to-ground style:

- Encoder common pin (**C/COM**) -> **GND**
- Encoder A -> pin **8**
- Encoder B -> pin **9**

For split behavior, each half firmware now enables only its local encoder:

- left firmware: only `left_encoder`
- right firmware: only `right_encoder`

## 2) Soldering/wiring checklist

For **each half**:

1. Solder encoder **COM** to keyboard GND.
2. Solder encoder **A** and **B** to pins 8 and 9.
3. Keep wires short and avoid routing next to noisy power lines.
4. Reflow any dull/cracked joint (cold solder joint).

If direction is reversed, swap A/B wires.

## 3) Multimeter checks (power off)

Use continuity mode:

1. Pin 8 pad <-> encoder A leg: should beep.
2. Pin 9 pad <-> encoder B leg: should beep.
3. Encoder COM <-> board GND: should beep.
4. No short between pin 8 and GND at rest.
5. No short between pin 9 and GND at rest.

## 4) Multimeter checks (power on)

Use DC voltage mode, black probe on GND:

1. At rest, pin 8 should sit near logic-high (due to pull-up).
2. At rest, pin 9 should sit near logic-high.
3. While rotating slowly, each pin should periodically drop low.

If pins stay low all the time, there is likely a short to GND or wrong encoder pinout.

## 5) Flashing sanity checklist

1. Flash **left** UF2 to left half.
2. Flash **right** UF2 to right half.
3. Re-pair split after flashing.
4. Test each encoder on its own half.

## 6) Important note about encoder push button

The Arduino test includes a button on `pin_Btn 7`, but this ZMK config currently does not map encoder push-button behavior.
Also, pin 7 is used by matrix row scan in this shield, so wiring encoder button there will conflict.
