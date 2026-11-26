> [!NOTE]
> Consolidated into the unified Holo v11 monorepo at https://github.com/awesome-os/awesome-os (all existing v11 CAS release assets remain downloadable).

# sectium-es

> *"It's JavaScript, Jim â€” and this time we even left the 88 MB V8 monolith at home! Huge shout-out to Andrew Johnston (`billywhizz`, creator of `just-js` & `lo`) for proving JavaScript belongs at the systems layer."*

**`sectium-es`** (`awesome-os/sectium-es`) is a tiny, zero-V8, zero-Node C11 ECMAScript engine built for systems programming.

- **Single-binary footprint:** `< 200 KB` standalone Win64 PE (`sectium-es.exe`) and Linux ELF64 (`sectium-es`) binaries (`dup_v8=0 B`)
- **Sub-second native builds:** Compiles from C11 in `< 1 s` with `zig cc` â€” zero external dependencies, zero Node/npm toolchain, zero WSL required
- **Direct systems primitives:** Built-in zero-copy `Uint8Array` byte buffers, monotonic nanosecond clock, platform detection, and direct file I/O

## Quick Start

### Build with `zig cc`

**Windows (x86_64 PE):**

```powershell
zig cc -target x86_64-windows-gnu -std=c11 -O2 -s `
  src/b8g_es_vm.c src/b8g_es_main.c `
  -I src -o dist/sectium-es.exe
```

**Linux (x86_64 static musl ELF64):**

```powershell
zig cc -target x86_64-linux-musl -std=c11 -O2 -s `
  src/b8g_es_vm.c src/b8g_es_main.c `
  -I src -o dist/sectium-es
```

### Run

```powershell
# Check version and zero-V8 footprint
.\dist\sectium-es.exe --version

# Evaluate inline ECMAScript
.\dist\sectium-es.exe -e "function fib(n) { if (n <= 1) return n; return fib(n - 1) + fib(n - 2); } console.log(fib(20));"

# Execute a script file
.\dist\sectium-es.exe path/to/script.js

# Run the built-in benchmark (100,000-iteration loop + fib(20))
.\dist\sectium-es.exe --bench
```

## Supported ES-Core & Systems Builtins

### Language Core

| Category | Features |
| :--- | :--- |
| **Bindings & Types** | `let`, `const`, `var`, IEEE-754 `number`, `boolean`, `null`, `undefined`, strings (`"..."`, `'...'`, escapes, `+`, `.length`) |
| **Operators** | Arithmetic (`+`, `-`, `*`, `/`, `%`), assignments (`=`, `+=`, `-=`, `++`, `--`), comparisons (`==`, `===`, `!=`, `!==`, `<`, `<=`, `>`, `>=`), logical (`&&`, `\|\|`, `!`), bitwise (`&`, `\|`, `^`, `~`, `<<`, `>>`) |
| **Control Flow** | `if` / `else`, `while`, `for`, `break`, `continue`, `return` |
| **Functions** | `function name(a, b) { ... }` with lexical scoping and recursion |
| **Collections** | Arrays (`[...]`, `arr[i]`, `arr[i] = v`, `.length`, `.push(v)`) and Objects (`{ k: v }`, `obj.k`, `obj["k"]`, mutation) |

### Systems Builtins

| Builtin | Description |
| :--- | :--- |
| `console.log(...args)` | Space-separated stdout output followed by a newline |
| `new Uint8Array(n)` / `Uint8Array(n)` | Zero-copy byte buffer with `.length` and indexed byte read/write (`buf[i]`, `buf[i] = b`) |
| `sys.readFile(path)` | Read UTF-8 text file from disk |
| `sys.writeFile(path, text)` | Write UTF-8 text file to disk |
| `sys.clockNs()` | High-resolution monotonic clock in nanoseconds |
| `sys.platform()` | Host OS identifier (`"win32"` or `"linux"`) |

## Verification

```powershell
node --test test/b8g-es.test.mts
```

---

Part of the **Sectium** (`awesome-os/chromium.googlesource.com`) & **Awesome-OS** (`awesome-os/awesome-os`) sovereign systems stack â€” use `sectium-es` standalone for tiny systems scripts, or scale up to `Sectium` + `Awesome-OS` when you need the full browser/V8/OS stack.

## Quick Start (Windows / Linux / macOS)

```sh
# Clone & enter repository (all platforms)
git clone https://github.com/awesome-os/sectium-es.git && cd sectium-es
```

## Quick Install & Run (Windows / Linux / macOS)

```sh
# Install / hydrate from v11 CAS via holo-pm
holo-pm install repository://github.com/awesome-os/sectium-es@main

# Or project & run directly via holo-project (0-byte hardlink)
holo-project "project://cpp/sectium-es?from=repository://github.com/awesome-os/sectium-es@main"
```
