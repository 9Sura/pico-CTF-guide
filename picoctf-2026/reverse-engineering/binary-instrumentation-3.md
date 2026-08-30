# BINARY INSTRUMENTATION 3 PROBLEM GUIDE:

*(Reverse Engineering — Medium, 300pt)*

## HINTS:
Hint 1: Frida is a great starting point.

## TOOLS:
Frida (Python bindings) ; IDA ; base64

## WALKTHROUGH:
1. `bin-ins.exe` is a custom packer: it unpacks a PE into memory, wipes the header, then calls the entry. Hook the header-erase function (`sub_1400015A0`) with Frida and dump the PE **before** it's wiped:
```js
Interceptor.attach(baseAddr.add(0x15A0), {
  onEnter(args){
    const pe = args[0];
    if (pe.readU16() === 0x5A4D) {                       // 'MZ'
      const e_lfanew = pe.add(0x3C).readU32();
      const size = pe.add(e_lfanew).add(0x50).readU32(); // SizeOfImage
      send({type:'dump', name:'payload_dumped.exe'}, pe.readByteArray(size));
    }
  }
});
```
    - Python side writes the received bytes to `payload_dumped.exe`.

2. Open the dump in IDA. Searching strings finds `C:\random\output_flag.txt`; create `C:\random` and run the exe. Also, an `I didn't work` path contains a base64 blob:
    - `cGljb0NURns0MTFfNHIzXzRwMTVfbjA3aDFuOV8zbDUzXzc5MjcyZjVifQo=`

3. `$ echo '<blob>' | base64 -d`:
    - Answer: `picoCTF{411_4r3_4p15_n07h1n9_3l53_79272f5b}`

## NOTES:
- Frida (dynamic instrumentation) lets you snapshot unpacked code from memory before anti-analysis wipes it.
- Dumping at the header-erase call is the key timing — the PE is fully unpacked but still intact.
