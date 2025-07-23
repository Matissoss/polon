<div align=center>
    <h1>polon</h1>
</div>

## about

`polon` is a WIP project of independent and modular toolchain.

This repo groups all info/repos that is/are released under `polon` project.

## projects

- x86-64 assembler [pasm](https://github.com/Matissoss/pasm)
- syntax notation [psyn](https://github.com/Matissoss/psyn)
- hex dump [sxd](https://github.com/Matissoss/sxd)

## version naming

`polon`'s codenames for major versions differ from version to version. They refer to great Polish kings, rulers, heroes, people, cities, etc.

Codenames are used internally and alongside official releases.

- `v1`: codename `mieszko` from Mieszko I: first ruler of Poland.
- `v2`: codename `sobieski` from Jan III Sobieski: king of Poland that participated in Battle Of Vienna (known in Poland as "odsiecz wiedeńska")

## roadmap v1 - mieszko

Goal: release self-hosted toolchain, for now with x86-64 arch support (and SPARC) and ELF/baremetal/pob target support.

- [ ] x86-64 assembler [pasm](https://github.com/Matissoss/pasm)
- [ ] SPARC assembler pasm-sparc
- [ ] SPARC and x86-64 static ELF linker + own baremetal object file format (`.pob`)
- [ ] Defining core architecture of `polon` (how to make modules work together, allow community to develop plug&play modules and remain SRP?)
- [ ] Create shared library for CLI's (to have unified info, debug, error, etc. formats)
- [ ] Define IR format and integrate it with assembler and linker; add first assembly instrincts
- [ ] Create shared library for IR (so we can parse it with minimal effort)
- [ ] Create optimizer for IR (even minimal will be fine; Jump Table is obviously necessary)
- [ ] Create `polon` CLI (for controlling behaviour of toolchain; like `Cargo` in Rust) alongside: 
- [ ] Create main language for self-hosting `polon` that uses `polon` as backend.
- [ ] Create stdlib for said language
- [ ] Port shared libraries for language
- [ ] Add more optimizations (to make `polon` atleast more performant than manual assembly) WITHOUT simd (that is for later).
- [ ] Rewrite one major module and test it against Rust's version. Optimize optimizer even more (with SIMD).
- [ ] Once the optimizer is well-tuned (matches Rust's performance in atleast 25% (75-125% of Rust's performance)), rewrite other major modules until we get atleast 90%+ parts backed by `polon`.
- [ ] Creating promo site for `polon` (with documentation viewer - md -> html) with (RSS included) blog.
- [ ] release v1

## roadmap v2 - sobieski

Main goal: support more targets and optimizations (very aggresive optimizations) (aka prepare for production-grade level).

TBD (because i shouldn't look this far into future)

## polon architecture

TODO

## credits

`polon` project is realized by matissoss
