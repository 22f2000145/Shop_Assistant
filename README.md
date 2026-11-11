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

## Other features

- **Setup tab:** set the shop name and UPI ID, and add items one after another.
- **Spoken replies:** turn on "Speak replies aloud" in Settings and the app reads each reply ("Total 347 rupaye"). Uses the browser's text-to-speech. The mic ignores input while the app is speaking.
- **WhatsApp:** send a finished bill on WhatsApp, and send an udhaar reminder to a customer once their number is saved in Khata. These open WhatsApp with the message filled in; you press send.
- **Bill history:** the Report tab lists the last 20 bills. Tap one to see its items, copy it or send it again.
- **Stock update:** once a day, on first open, the app shows a stock update: items that are out, items that will finish within 7 days at the speed they are selling (based on the last 14 days), items below their warning level, and items with no sale in 14 days. Say `stock update` to get it any time.
- **Sales analysis:** the Report tab has Today, Last 7 days and Last 30 days. It shows sales, bills, average bill, estimated profit, busiest hour, best weekday, top selling items, sales by weekday, sales day by day and how customers paid. Say `sabse zyada kya bika` for a spoken summary.
- **Empty shop:** Settings can erase everything and start with no items or customers, keeping the shop name.

## Works without Claude

`index.html` is a normal web page. It needs no server, no account and no paid service.

- **Voice:** uses the browser's built-in speech recognition (Chrome or Edge, on Android or desktop). It needs internet and microphone permission. In other browsers the mic is hidden and you type.
- **Data:** stored in an on-device database (IndexedDB, database name `voiceshop`). Nothing is sent to a server. Use Settings > Backup to copy your data as text, and Restore to load it on another device.
- **Free hosting:** in this repo go to Settings > Pages, choose "Deploy from a branch", branch `main`, folder `/ (root)`. The app is then live at `https://22f2000145.github.io/Shop_Assistant/`. GitHub Pages on a free account needs the repository to be public.

## Database

The database lives on the phone or computer where the app is opened.

| Store | Key | Holds |
|---|---|---|
| `meta` | `shop` | Shop name, UPI ID, voice language, next bill number, the open bill and held bills |
| `items` | `id` | Name, other names, unit, selling price, purchase price, stock, low-stock level, peti size |
| `customers` | `id` | Name, balance, ledger entries (udhaar and jama) |
| `bills` | `id`, index on `t` | Bill number, time, lines, discount, total, payment mode, customer |
| `expenses` | `id`, index on `t` | Time, amount, note |

**Retention: 6 months (183 days).** Each time the app opens, bills, expenses and khata ledger entries older than that are deleted. Items, customers and their balances are never deleted by retention.

The app keeps the most recent 1,000 bills in memory for reports; older ones within the 6 months stay in the database.

## Limits of this prototype

- The app opens with an **example shop** (sample items, prices and customers). Replace them in Stock and Khata, or erase everything in Settings.
- Data lives on one device. There is no sync between phones; move data with Backup and Restore. Clearing the browser's site data, or uninstalling the browser, deletes the database.
- No login, staff roles, GST invoices, printer support or supplier ledger yet. See the spec for the planned phases.
- The command parser is rule-based. Unusual phrasing may not be understood.
- Browser speech recognition accuracy in a noisy shop has not been tested.

## Next steps

See section 16 (MVP scope) and section 21 (roadmap) of the spec.
