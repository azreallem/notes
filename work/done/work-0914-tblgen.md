``./build/lib/Target/AMDGPU/AMDGPUGenAsmMatcher.inc``

> ``VariantID = 1`` 表示 64 bits encoding;
> ``Mnemonic`` 表示助记符；
> ``Mnemonic.size()`` 表示助记符的长度;
>> 比如 ``v_or_b32_e64`` 的 ``Mnemonic.size() = 12`` , ``Mnemonic[2] = 'o'``

```cpp
static void applyMnemonicAliases(StringRef &Mnemonic, uint64_t Features, \ 
                                 unsigned VariantID) {
switch (VariantID) {
    case 0:
    ......
    case 1:
        switch (Mnemonic.size()) {
            ......
            case 13:
                ......
                Mnemonic = "v_mov_b32";      // "v_mov_b32_e64"
                ......
            break;
            ......
        }
    break;
    case 1:
    ......
}

}
```
