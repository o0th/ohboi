### Oh boi

This is owerwhelming. So first thing first we need an assembler/linker for
Gameboy/Gameboy Color, from now on GB, GBC. We have an open source one RGBDS

```
https://rgbds.gbdev.io/
https://github.com/gbdev/rgbds
```

Once we have rgbds this is how we use them:

```
source code -> rgbasm -> object file -> rgblink -> raw rom -> rgbfix -> fix rom
```

A fix rom is basically a ROM with some metadata: Nintendo's logo, rom size and
two checksum.

Let's start with an example

```asm
INCLUDE "hardware.inc"

SECTION "Header", ROM0[$100]

    jp EntryPoint

    ds $150 - @, 0 ; Make room for the header

EntryPoint:
    ; Shut down audio circuitry
    ld a, 0
    ld [rNR52], a
```

### Numbers

Number without prefix are in base 10. To write in binary the prefix is `%`.
To write in hexadecimal the prefix is`$`. Is it possible to write big number
with underscore `123_456` `%1101_1010` `$DE_04_EF`.

### General purpose registers

The CPU has 7 general purpose registers, from now on GPR, of 8-bit. `a`, `b`,
`c`, `d`, `e`, `h` and `l`. `a` is the acculmulator. `bc`, `de`, `hl` can be
paired to be treated as 16-bit registers.

### Instructions

This is an instruction

```asm
; this is a comment
ld a, 0
```

`ld` stand for `load` and it will copy the right hand side (RHS) to the left
hand side (LHS), so in this case it will copy 0 to the registry `a`.

### Directives

This is a directive

```asm
INCLUDE "hardware.inc"
```

`INCLUDE` directive is the same as copy/paste the file `hardware.inc` in the
current file.

### Sections

Sections are contiguous range of memory that we do not know where they end up
in memory unles we specify it.

```asm
SECTION "Header", ROM0[$100]
```

In this particular case we are defining the section `header` which has memory
type `ROM0` and it will be placed at memory address `$100` which is reserved
for the ROM's headers.

### Reserving space

The symbol `@` refer to the current memory address. `ds` is used for statically
allocate memory. The first argument is how many bytes to allocate and the
second with what. In this case Take address `$150` and subtract `@`.

```asm
ds $150 - @, 0
```

### Memory

| start | end   | name | Description             |
|-------|-------|------|-------------------------|
| $0000 | $7fff | ROM  | Game ROM                |
| $8000 | $9fff | VRAM | Video RAM               |
| $a000 | $bfff | SRAM | Save RAM                |
| $c000 | $dfff | WRAM | Work RAM                |
| $fe00 | $fe9f | OAM  | Object Attribute Memory |
| $ff00 | $ff7f | HRAM | High RAM                |
| $ffff | $ffff | IE   | A lone I/O byte         |

### Labels

It is possible to assign addresses to labels like

```asm
EntryPoint:
    ld a, 0
```

`EntryPoint` point to the first byte of the next instruction `ld a, 0`

### [Header](https://gbdev.io/gb-asm-tutorial/part1/header.html)

$0104 to $014F (inclusive) and it contains metadata about the rom. On real
hardware is not important what this header contain. On emulator it does.

```asm
SECTION "Header", ROM0[$100] ; we start at $0100
    jp EntryPoint            ; the jp is 3 bytes
    ds $150 - @, 0           ; @ is the current address
                             ; So this will fill from $103 to $150 with 0
```


So if you forget to make space for the header the emulator/hardware will not
load the ROM.

`ds` stand for define space.

### Inifnite loop

In the end we put an inifite loop to make the logo stay on the screen

```asm
Done:
    jp Done
```
