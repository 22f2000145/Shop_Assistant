# VoiceShop

Voice-first billing, stock and khata for small shops. The shopkeeper speaks (or types) a command in Hinglish, Hindi or English and the app makes the bill, updates inventory and keeps the udhaar ledger.

This repository holds a working **prototype** in a single HTML file, plus the full product spec.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app: UI, command parser, storage. No build step. |
| `docs/VoiceShop_Product_Spec.md` | Product spec: features, voice grammar, architecture, data model, roadmap. |

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Commands it understands

| Say or type | What happens |
|---|---|
| `do kilo atta, ek surf, teen maggi` | Adds items to the bill |
| `100 rupaye ka tel` | Amount-based sale, quantity is calculated |
| `maggi do kar do` / `surf hatao` | Change quantity / remove |
| `20 rupaye kam karo` / `10 percent discount` | Discount |
| `bill complete karo` | Shows total and asks how the customer pays |
| `cash, 500 diye` | Completes the bill and tells the change |
| `UPI` | Shows a UPI QR (set your UPI ID in Settings) |
| `Sharma ji ke khaate mein` | Puts the bill on udhaar, after confirmation |
| `Verma ji ne 200 diye` | Records a payment received |
| `50 maggi aayi` / `3 anda toot gaye` | Stock in / damage |
| `doodh 68 ka kar do` | Price change, after confirmation |
| `atta kitna bacha hai` / `kya mangwana hai` | Stock check / reorder list |
| `aaj kitna bika` | Today's sales, payment split, estimated profit |
| `undo` | Reverses the last change |

It understands Indian quantities and units: pav, adha, dedh, dhai, sawa, saadhe, paune, darjan, peti, bora. Hindi (Devanagari) input is transliterated before parsing.

## Limits of this prototype

- **Voice and account storage only work inside Claude.** The microphone and the private per-account save use the Claude artifact runtime. Opened from this repo, the mic button is hidden (type instead) and data is saved in the browser's `localStorage` on that device only.
- The app opens with an **example shop** (sample items, prices and customers). Replace them in Stock and Khata, or erase everything in Settings.
- No login, staff roles, GST invoices, printer support, supplier ledger or multi-device sync yet. See the spec for the planned phases.
- The command parser is rule-based. Unusual phrasing may not be understood.

## Next steps

See section 16 (MVP scope) and section 21 (roadmap) of the spec.
