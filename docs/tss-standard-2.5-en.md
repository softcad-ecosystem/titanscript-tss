# TitanScript STANDARD 2.5.0 — distributed edition reference

**Documentation status:** technical reference for STANDARD 2.5.0 RC2.1, 21 October 2026. **Documentation and examples only** are provided here; standalone runtime redistribution is still under review (HOLD). The reference describes the **pure-Python interpreter**, not the commercial Shield/C99 engine or a native compiler. Standalone Windows/macOS validation remains pending.

## 1. Running

From the extracted directory, with Python 3.10 or newer:

```bash
python3 titanscript exemple/01_limbaj.tss
python3 titanscript -c 'print("Hello SoftCAD");'
python3 titanscript exemple/02_baza_tdb3.tss
bash ruleaza_teste.sh
```

On Windows, `titanscript.bat` and `ruleaza_teste_windows.bat` are included (file presence checked; **not executed on Windows**). The REPL and language selection are available using `python3 -m pytcad.tss.cli --lang en` with `runtime` added to PYTHONPATH. `pytest` is needed to run tests, not to run TSS.

## 2. Language

- Semicolon-separated statements, `{...}` blocks, `//` comments and double-quoted UTF-8 strings.
- `let`, `const`, `function`, `return`, `if`/`else`, `for`/`of`, `while`, `break`, `continue`, `import`.
- Number, string, bool, `null`, array, object, function, buffer and unit-bearing quantity values.
- Arithmetic, comparison, logical operators, compound assignment and conditional expressions.
- Lexical scope, closures, anonymous functions, `yield` generators and lazy streams.
- User file imports: `import "lib/util.tss";` resolved relative to the importing script, loaded once, with cycle detection. **`.tsx` file imports are forbidden.**

### Example

```tss
function area(w, h) { return w * h; }
let plots = [{"id": 1, "w": 40, "h": 38}, {"id": 2, "w": 22, "h": 40}];
for (let p of plots) { print(p.id, area(p.w, p.h)); }
print(dms(123, 45, 30));
```

## 3. Builtins (no import required)

`print`, `readline`, `bytes`, `len`, `is_numeric`, `ok`, `error`, `json_serialize`, `keys`, `values`, `has`, `str`, `num`, `range`, `abs`, `min`, `max`, `round`, `qty`, `unit`, `sin`, `cos`, `tan`, `dms`, `argv`.

- Units: `mm`, `cm`, `m`, `km`, `mp`, `ar`, `ha`, `mc`, `gon`, `deg`, `rad`. Dimensions are checked, `.to("ha")` converts, `.value()` strips to a numeric value, and `.dms()` formats angles.
- Collection methods include `push`, `pop`, `join`, `map`, `filter`, `find`, `reduce`, `sort` and `slice`, as covered by included tests.

## 4. STANDARD imports

| Import | Example functions |
|---|---|
| `std.io` | `read_file`, `write_file` |
| `std.fs` | `file_exists`, `mkdir`, `read_file`, `write_file` |
| `std.time` | `time_micros`, `time_iso` |
| `std.json` | `json_parse`, `json_extract_string`, `json_extract_int`, `json_extract_bool` |
| `std.stream` | `stream`, `stream_range`, `stream_lines`, `stream_jsonl`, `stream_cgxml` |
| `std.db` | `db_open`, `db_create` powered by the bundled TDB3 library |

`stream_cgxml` is a streaming reader only: it is **not** a full cadastral format codec or topology validator.

### TDB3 using `std.db`

```tss
import std.db;
let db = db_create("work.tdb3");
db.write(function(tx) {
  tx.put("plots", 47003, {"owner": "Popa", "area": 1520.4});
}, {"author": "operator", "message": "initial insert"});
for (let p of db.query("plots | sorteaza area desc")) {
  print(p.owner, p.area);
}
print(db.fingerprint());
db.close();
```

Avoid external side effects in a transaction callback, since it can be retried following a write conflict. If a path is relative, `.tdb3` is created relative to the working directory.

## 5. Commercial boundary

**Not included:** `import engine.*`, `import titan`, Shield, *any* `.tsx` file (even plaintext content with `.tsx` extension), `pack`, Shield `--password` or signing options, persistent Job Engine, native C99 engine, CAD/topology/terrain algorithms, TCAD/TCB/CGB/TXB codecs, and X.509.

Legacy Python Job Engine entry points raise explicit errors rather than executing jobs. Missing native engines are not silently hidden behind broad exception handlers.


## 6. Availability, licensing and safety

- This bundle contains **documentation and two synthetic examples only** — not the standalone interpreter, codecs or proprietary components.
- The STANDARD 2.5.0 source candidate declares Apache-2.0, while the bundled TDB3 component carries its own LICENSE/NOTICE. Separate redistribution approval remains under review.
- `std.fs`, `std.io` and `std.db` can read or modify local files. Run trusted scripts only.
- FileCommander 4.4.3 includes a TSS editor and runner under TITAN → TSS Editor (`Ctrl+E`) and TITAN → TSS Run (`Ctrl+R`). The owner confirmed Windows application startup; this is not a certification of all TSS runtime behavior.
- This document is neither a legal specification nor a security audit nor a claim that a standalone SDK has been publicly released.
