# Copper Balancing for Altium Designer

A DelphiScript tool for **Altium Designer** that automatically generates copper balancing / copper thieving patterns on selected PCB copper layers.

The script was developed and tested with **Altium Designer 26.9.x** and is intended to simplify the process of adding evenly distributed copper patterns without manually using Via Stitching or converting vias into free pads.

<img width="1471" height="1140" alt="image" src="https://github.com/user-attachments/assets/3ae0ee2e-db95-4edd-ad57-713ad5c164b1" />


## Features

- Generate copper balancing on multiple copper layers at once
- Select layers using checkboxes directly in the GUI
- Three pattern types:
  - Squares
  - Diamonds
  - Circles
- Pattern preview directly in the GUI
- Adjustable:
  - Element size
  - Horizontal spacing
  - Vertical spacing
  - Clearance from existing copper
  - Clearance from the PCB edge
- Automatic collision checking with existing PCB objects
- Automatic board-outline checking
- Supports staggered patterns for diamonds and circles
- Generated elements can remain independent for manual editing or deletion
- Optional grouping of generated elements
- Fast global removal of all generated Copper Balancing elements
- Progress bar during generation and removal
- Generated Free Pads are marked internally so they can be safely identified and removed later
- Compatible with Altium's DRC when `PCB.Rules.DeadCopperNoNet` is configured appropriately

## Pattern Types

### Squares

Aligned rows and columns:

```text
[ ]   [ ]   [ ]
[ ]   [ ]   [ ]
[ ]   [ ]   [ ]
```

### Diamonds

Rectangular pads rotated by 45 degrees, with every second row shifted by half of the horizontal spacing:

```text
<>    <>    <>
   <>    <>    <>
<>    <>    <>
```

### Circles

Circular pads with every second row shifted by half of the horizontal spacing:

```text
(O)   (O)   (O)
   (O)   (O)   (O)
(O)   (O)   (O)
```

## Generated Objects

By default, balancing elements are created as **Free SMD Pads**.

Generated pads are:

- No Net
- individually editable
- individually deletable
- solder-mask tented
- without paste-mask openings
- internally marked with `CB_BALANCE`

This allows the script to safely remove only objects that it previously generated.

## Global Removal

Use the **Remove Copper Balancing** button in the GUI to remove all generated balancing elements from all layers.

The removal process uses bulk object collection before deletion, making it significantly faster than deleting elements one at a time.

## Internal Layers

The script supports positive internal signal layers (`MidLayer`).

Traditional Altium **Internal Plane** layers are intentionally excluded because they use negative artwork. Adding objects to a negative plane would create copper voids instead of adding balancing copper.

## Installation

1. Open Altium Designer.
2. Open a script, CopperBalancing.PrjScr.
3. Open the target `.PcbDoc`.
4. Go to File -> Run Script...
5. Run the `Start` procedure.
6. Select the required layers and pattern parameters.
7. Click **Generate**.


## Important

Always verify the generated PCB before manufacturing:

- Run Design Rule Check
- Inspect Top and Bottom solder-mask layers
- Inspect all modified internal copper layers
- Review Gerber or ODB++ output
- Verify board cutouts and keepout areas

**It is recommended to test the script on a copy of the PCB before using it on a production design.**

## Compatibility

Tested with:

**Altium Designer 26.9.x**

Other Altium Designer versions may require small changes because the DelphiScript PCB API can differ between releases.

## Disclaimer

This script is provided as a PCB design automation aid. Always perform a complete design review and manufacturing-output verification before releasing a board for production.
