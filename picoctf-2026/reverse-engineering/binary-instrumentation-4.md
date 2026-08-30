# BINARY INSTRUMENTATION 4 PROBLEM GUIDE:

*(Reverse Engineering — Hard, 400pt)*

## HINTS:
Hint 1: Frida is a great starting point.
Hint 2: Compare is an API too.

## TOOLS:
Frida ; ncat ; base64

## WALKTHROUGH:
1. `bin-ins.exe` (another packer) prints "Connection failed." Hook `ws2_32.dll!connect` with Frida to read the target — `192.168.29.25:9867`.

2. Rewrite the sockaddr in the `connect` hook to redirect the IP to `127.0.0.1`, and listen locally: `$ ncat -lvp 9867`. The exe prompts `Enter the key:`.

3. Hook the key comparison. `lstrcmpA` compares your input to the real key `key9640ed84`. Sending it over ncat fails (trailing newline), so force the compare to succeed instead:
```js
Interceptor.attach(Module.findExportByName('kernel32.dll','lstrcmpA'), {
  onEnter(a){ this.s1=Memory.readAnsiString(a[0]); this.s2=Memory.readAnsiString(a[1]); },
  onLeave(ret){ if (this.s1==='key9640ed84' || this.s2==='key9640ed84') ret.replace(0); }
});
```

4. The server then sends the base64 flag:
    - `cGljb0NURntuM3R3MHJrXzFzXzRQMXNfNFNfVzMxMV85NjQwZWQ4NH0K`
    - `$ echo '<blob>' | base64 -d`:
        - Answer: `picoCTF{n3tw0rk_1s_4P1s_4S_W311_9640ed84}`

## NOTES:
- Three Frida moves: inspect `connect` (find the C2), rewrite it (redirect to localhost), and patch `lstrcmpA`'s return (bypass the key check).
- Patching a comparison's return value (`ret.replace(0)`) sidesteps needing the exact input formatting.
