<div align=center>
    <h1>/standards/assembly.md</h1>
</div>

## about

This file defines standard base syntax for assemblers under POLON project. This format only focuses on few things and does not try to be a universal format for assembly.

This document uses [psyn v1](https://github.com/Matissoss/psyn) format for documentation.

## file layout

```
*ROOT*
section <NAME> [*SECTION_ATTRIBUTES*]
    [*SECTION_ATTRIBUTES*]
    [...]
    [*LABEL_ATTRIBUTES*]
    [*VISIBILITY*] [*LABEL_TYPE*] label: [*INSTRUCTION*]
        [*INSTRUCTIONS*]
    [...]
[...]
```

## Comments

```
; This is a comment
// This is also a comment
# THIS IS NOT A COMMENT
/* AND THIS IS ALSO NOT A COMMENT */
```

## Shared ROOT Directives

### isa

`isa <ARCH_EXTENSIONS/OPTIONS>`

`isa` directive defines extension of base architecture that is used.

### define

`define <NAME> <IMMEDIATE>`

Defines define symbol `NAME` of value `VALUE`. Referencing defines should be prefixed with `$DEFINE_NAME`.

Example (for x86-64):

```
polon_extension enable

define 1_plus_2 3
...

__polon_load rax, $1_plus_2
```

For `polon_extension` and `__polon_load` look into `## Instructions`

### extern

`extern <NAME>`

Adds undefined symbol `NAME` that is resolved later on linking phase.

### format

`format <FORMAT>`

Specifies final output format. This should have allowed variant of: `format "polon"` (POLON output format will be defined in `standards/pout.md`)

### output

`output <PATH>`

Specifies output file path.

## Labels

`[VISIBILITY] [TYPE] <LABEL_NAME>: [INSTRUCTION]`

Labels are storage for instructions. They have: a name (required), type, align and visibility and architecture-specific info.

> [!NOTE]
> To specify `align` for labels you have to use external attributes; look for info in `### #() Closure`

## Sections

`section <NAME> [SECTION_ATTRIBUTES]`

Sections are storage for sections. They have: a name (required), align, permissions (`alloc`, `executable` and `writeable` permissions) and architecture-specific info.

> [!NOTE]
> `align` directive used on `section` CANNOT be on the same line as the section declaration.
> To use `align` directive do:
> ```
> <SECTION_DECLARATION>
>   align <ALIGN>
> ```
> This rule also applies to other architecture specific attributes that require values.

## Closures

Closures are shared across multiple archtectures. Here are ones that are shared.

### () Closure

This closure is used IF architecture has support for memory addressing.

Example:

- for x86: `(<?BASE:Register> [+ <?INDEX:Register> ] [* <?SCALE:1/2/4/8>] [+/- <?DISPLACEMENT:>])`
- for SPARC (this can be invalid, i have little knowledge about SPARC): `(<?RS:Register> [+/- <?DISP:Signed13BitImm>]/[+ <?RS2:Register>])`

### $() Closure

`$()` closure is used for assembly time mathematical evaluations. 

In "normal" (most barebones) implementation, they allow for the following:

- Tagging: `[u8/u16/u32/u64/i8/i16/i32/i64:]$()`
- Arithmetical Operations: `+` (add), `-` (sub), `*` (mul), `/` (div), `~` (neg), `%` (mod)
- Bit Operations: `<<` (lsh), `>>` (rsh), `&` (and), `|` (or), `!` (not), `^` (xor)

Depending on implementation there also should be following "extensions"/features:

- `evalf-base`: Basic Float Evaluations; `f32` (IEE single precision) and `f64` (IEE double precision)
- `evalf-complex`: Complex Float Evaluations (DEPENDENT on `evalf-base`)
- `evalf-f16`: (EXPERIMENTAL) 16-bit Float Evaluations (IEE half precision `f32` and `bf16` (Brain Float))

These features are discussed in header `## Extensions`

### #() Closure

`#()` closure contains external attributes for `Labels`.

Base features of `#()` is currently limited to (shared across architecture):

- `align=<?u16>`: align address of label to value of `u16`.

Attributes are split using `,`.

These attributes must be BEFORE label declaration like:

```
[EXTERNAL ATTRIBUTES]
<LABEL DEFINITION>
```

## Symbols

Symbols are key part in every POLON assembler implementation. Symbols are: every label, every define and every section user defines.

These can later be referenced using: `@<SYMBOL_NAME>[:<?RELOCATION_TYPE>][:<OFFSET>]`.

## Registers

Registers should NEVER be prefixed. Naming of these registers is dependent on architecture.

## Immediates

Immediates should be able to be written in: `f32/64`, `u8/16/32/64`, `i8/16/32/64`.

There also should be instructions like: `movconst` for setting consts if you cannot set full 16/32/64-bit values that consist of native multiple instructions.

## Instructions

`*INSTRUCTION* = [ADDITIONAL_MNEMONIC] <MNEMONIC> [DST [, <SRC> [, <SSRC> [, <TSRC> [, <FSRC>]]]]]`

If architecture allows for additional mnemonics (like in x86-64: `LOCK`/`REP` prefixes), then these are neccesary. Otherwise you can skip it.

For unification THERE ARE MANDATORY "instructions" that should be implemented. They are hidden behind `polon_extensions enable` directive and their names are prefixed with `__polon_`.

Here is full list of them:

- `__polon_load <REGISTER>, <IMMEDIATE>/<DEFINE_REF>`: loads value of `DEFINE_REF`/`IMMEDIATE` in `REGISTER`.
- `__polon_load_address <REGISTER>, <SYMBOL>`: stores address of `SYMBOL` in `REGISTER`
- `__polon_store_8 <ADDRESS>, <REGISTER>/<IMMEDIATE>`: stores 8-bit value in `ADDRESS`
- `__polon_store_16 <ADDRESS>, <REGISTER>/<IMMEDIATE>`: stores 16-bit value in `ADDRESS`
- `__polon_store_32 <ADDRESS>, <REGISTER>/<IMMEDIATE>`: stores 32-bit value in `ADDRESS`
- `__polon_store_64 <ADDRESS>, <REGISTER>/<IMMEDIATE>`: stores 64-bit value in `ADDRESS`

- `__polon_string <STRING>`: stores string
- `__polon_b64_le <64-bit IMMEDIATE>`: stores 64-bit little-endian value 
- `__polon_b64_be <64-bit IMMEDIATE>`: stores 64-bit big-endian value 
- `__polon_b32_le <32-bit IMMEDIATE>`: stores 32-bit little-endian value 
- `__polon_b32_be <32-bit IMMEDIATE>`: stores 32-bit big-endian value 
- `__polon_b16_le <16-bit IMMEDIATE>`: stores 16-bit little-endian value 
- `__polon_b16_be <16-bit IMMEDIATE>`: stores 16-bit big-endian value 
- `__polon_b8 <8-bit IMMEDIATE>`: stores 8-bit value 

> [!NOTE]
> This list may be extended.

## Naming Conventions

- `DST.` - destination; first operand (index 0)
- `SRC.` - source; second operand (index 1)
- `SSRC.` - second source; third operand (index 2)
- `TSRC.` - third source; fourth operand (index 3)
- `FSRC.` - fourth source; fifth operand (index 4)

If possible operand number is greater than 5, then `HLP<N>` naming is required. `HLP` = Helper Operand; `N = Operand Index - 4`
