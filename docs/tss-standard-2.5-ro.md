# TitanScript STANDARD 2.5.0 — referința ediției distribuite

**Statut documentație:** referință tehnică pentru STANDARD 2.5.0 RC2.1, 21 octombrie 2026. **Numai documentația și exemplele** sunt publicate aici; distribuția separată a runtime-ului este încă în revizuire (HOLD). Acest document descrie **interpretorul pur-Python**, nu produsul comercial Shield, motorul C99 sau compilatorul nativ. Testele standalone Windows/macOS rămân neconfirmate.

## 1. Pornire

Din folderul extras, cu Python 3.10 sau mai nou:

```bash
python3 titanscript exemple/01_limbaj.tss
python3 titanscript -c 'print("Salut SoftCAD");'
python3 titanscript exemple/02_baza_tdb3.tss
bash ruleaza_teste.sh
```

Pe Windows sunt incluse `titanscript.bat` și `ruleaza_teste_windows.bat` (prezența verificată, execuția pe Windows **nevalidată**). Pentru REPL și mesajele bilingve, cu `runtime` pe PYTHONPATH: `python3 -m pytcad.tss.cli --lang ro`. Testele necesită `pytest`; runnerul nu are dependențe Python terțe.

## 2. Limbaj

- Instrucțiuni delimitate prin `;`, blocuri `{ ... }`, comentarii `//`, stringuri UTF-8 cu ghilimele duble.
- `let`, `const`, `function`, `return`, `if`/`else`, `for`/`of`, `while`, `break`, `continue`, `import`.
- Valori: număr, text, bool, `null`, array, obiect, funcție, buffer și mărime cu unități.
- Expresii: `+ - * / %`, comparații, `&&`, `||`, `!`, atribuiri și operatori compuși, expresii condiționale.
- Scope lexical, închideri, funcții anonime, generatori `yield` și fluxuri lazy.
- Importuri de utilizator: `import "lib/util.tss";` rezolvate relativ la fișierul curent, evaluate o dată per sesiune, cu raportarea ciclurilor. **Importul de `.tsx` nu este permis.**

### Exemplu

```tss
function aria(l, h) { return l * h; }
let parcele = [{"id": 1, "l": 40, "h": 38}, {"id": 2, "l": 22, "h": 40}];
for (let p of parcele) { print(p.id, aria(p.l, p.h)); }
print(dms(123, 45, 30));
```

## 3. Nucleul implicit (fără import)

`print`, `readline`, `bytes`, `len`, `is_numeric`, `ok`, `error`, `json_serialize`, `keys`, `values`, `has`, `str`, `num`, `range`, `abs`, `min`, `max`, `round`, `qty`, `unit`, `sin`, `cos`, `tan`, `dms`, `argv`.

- Unități: `mm`, `cm`, `m`, `km`, `mp`, `ar`, `ha`, `mc`, `gon`, `deg`, `rad`; operațiile verifică dimensiunile. Conversie explicită cu `.to("ha")`, extragere cu `.value()`, format sexagesimal cu `.dms()`.
- Colecții: metodele standard `push`, `pop`, `join`, `map`, `filter`, `find`, `reduce`, `sort`, `slice` etc., conform testelor pachetului.

## 4. Module importabile STANDARD

| Import | Exemple de funcții |
|---|---|
| `std.io` | `read_file`, `write_file` |
| `std.fs` | `file_exists`, `mkdir`, `read_file`, `write_file` |
| `std.time` | `time_micros`, `time_iso` |
| `std.json` | `json_parse`, `json_extract_string`, `json_extract_int`, `json_extract_bool` |
| `std.stream` | `stream`, `stream_range`, `stream_lines`, `stream_jsonl`, `stream_cgxml` |
| `std.db` | `db_open`, `db_create` peste biblioteca TDB3 inclusă |

`stream_cgxml` este un iterator pentru extragere de date; **nu** reprezintă un codec cadastral complet ori validare topologică.

### TDB3 prin `std.db`

```tss
import std.db;
let db = db_create("lucrari.tdb3");
db.write(function(tx) {
  tx.put("parcele", 47003, {"proprietar": "Popa", "suprafata": 1520.4});
}, {"author": "operator", "message": "prima incarcare"});
for (let p of db.query("parcele | sorteaza suprafata desc")) {
  print(p.proprietar, p.suprafata);
}
print(db.fingerprint());
db.close();
```

Într-o tranzacție reluată la conflict, nu produce efecte secundare externe. Fișierul `.tdb3` este creat în directorul de lucru dacă nu este dată o cale explicită.

## 5. Delimitarea față de ediția comercială

**Indisponibile:** `import engine.*`, `import titan`, Shield, toate fișierele `.tsx` (inclusiv text simplu cu extensia `.tsx`), `pack`, opțiunile Shield `--password` și semnare, job engine persistent, motor nativ C99, algoritmi CAD/topologie/teren, codecuri native TCAD/TCB/CGB/TXB, X.509.

Interfețele Python istorice pentru Job Engine răspund cu eroare explicită; ele **nu** lansează job-uri. Lipsa unui motor nu este ascunsă prin captură generală a excepțiilor.


## 6. Disponibilitate, licență și siguranță

- Acest pachet conține **doar documentație și două exemple sintetice**, nu interpretorul standalone, codecuri sau componente comerciale.
- TitanScript STANDARD 2.5.0 declară Apache-2.0 în sursa candidatului, iar TDB3 inclus are fișiere proprii LICENSE/NOTICE. Verificarea și aprobarea redistribuirii separate rămân deschise.
- Funcțiile `std.fs`, `std.io` și `std.db` pot citi sau modifica fișiere locale. Executați doar scripturi de încredere.
- FileCommander 4.4.3 include un editor și un runner TSS integrat: TITAN → TSS Editor (`Ctrl+E`), TITAN → TSS Run (`Ctrl+R`). Pornirea pe Windows a aplicației a fost confirmată de titular; nu echivalează cu certificarea tuturor funcțiilor TSS.
- Documentul nu este o specificație juridică, un audit de securitate sau promisiunea unui SDK autonom deja publicat.
