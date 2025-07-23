<div align=center>
    <h1>/documentation.md</h1>
</div>

## about

This file contains context for every document you can find in this repository.

## syntax format

To explain syntax of any tool we currently use [psyn v1](https://github.com/Matissoss/psyn) format.

It is a very minimal format made for this purpose. You can learn basics of `psyn v1` very quickly. Most commonly used so called "nodes" are:

- `<NODE>`: required node
- `[NODE]`: optional node
- `NODE`: static node
- `?NODE:TYPE`: node `NODE` of type `TYPE`
- `?TYPE`: unnamed typed node of type `TYPE`
- `<!>`: End Of Line (Nothing can be past that line)
- `[...]`: repeating

Examples:

- C function syntax: `<?RETURN:CType> <FN_NAME>([<?ARG0TYPE:CType> <ARG_NAME>, ] [...])`
- Rust function syntax (without generics): `[unsafe] [extern <?EXTERN:RustExtern>] [async] fn <FN_NAME>([<ARG_NAME>: <?RustType>,] [...]) [-> <?RETURN:RustType>]`

## conventions

Each documentation file should have following content at start (github-flavoured markdown):

```
<div align=center>
    <h1>/PATH/TO/FILE.md</h1>
</div>

## about

...
```
