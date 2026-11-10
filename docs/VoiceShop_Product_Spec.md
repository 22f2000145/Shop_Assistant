# VoiceShop — Voice-First Inventory & Billing for Small Shops

> **One-line pitch:** A shopkeeper talks to their phone like they'd talk to a helper — "Do kilo cheeni, ek Parle-G, total bata do" — and the app makes the bill, updates stock, records payment, and keeps the books.

*Working name: "VoiceShop" (placeholder — rename freely).*

---

## Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [Vision & Goals](#2-vision--goals)
3. [Target Users & Personas](#3-target-users--personas)
4. [Core Principles](#4-core-principles)
5. [Feature List (Complete)](#5-feature-list-complete)
6. [Voice Command System](#6-voice-command-system)
7. [User Flows](#7-user-flows)
8. [Screens & UI](#8-screens--ui)
9. [System Architecture](#9-system-architecture)
10. [Data Model](#10-data-model)
11. [Voice/AI Pipeline in Detail](#11-voiceai-pipeline-in-detail)
12. [Offline-First Strategy](#12-offline-first-strategy)
13. [Security, Privacy & Permissions](#13-security-privacy--permissions)
14. [Edge Cases & Error Handling](#14-edge-cases--error-handling)
15. [Tech Stack Recommendation](#15-tech-stack-recommendation)
16. [MVP Scope vs Later Phases](#16-mvp-scope-vs-later-phases)
17. [Monetization](#17-monetization)
18. [Competitive Landscape](#18-competitive-landscape)
19. [Success Metrics](#19-success-metrics)
20. [Risks & Mitigations](#20-risks--mitigations)
21. [Development Roadmap](#21-development-roadmap)
22. [Open Questions](#22-open-questions)
23. [Glossary](#23-glossary)

---

## 1. Problem Statement

Small shopkeepers (kirana stores, medical shops, hardware stores, stationery, small garment shops, etc.) run their business mostly from memory, paper notebooks (*bahi-khata*), and a calculator.

**Pain points:**

| Pain | What happens today |
|---|---|
| Billing is slow | Items added one by one on calculator; mistakes during rush hours |
| Stock is unknown | Owner realises an item is out only when a customer asks for it |
| Udhaar (credit) tracking | Written in a notebook, often lost, disputes with customers |
| Typing is hard | Many owners are not comfortable typing, especially in English; hands are busy weighing/packing |
| Existing software is complex | Menus, forms, SKUs, barcodes — built for accountants, not for shopkeepers |
| No insight | Owner doesn't know daily profit, best sellers, or dead stock |
| GST compliance | Confusing; needs a CA for even basic reports |

**Core insight:** The shopkeeper already *speaks* every transaction out loud ("2 kilo atta, 1 Surf, 500 ka tel"). If software can listen and understand that, the friction disappears.

---

## 2. Vision & Goals

**Vision:** Make running a small shop as easy as talking.

**Goals:**

1. **Bill in under 10 seconds** for a typical 3–5 item purchase, by voice alone.
2. **Zero manual typing** required for daily operations (typing remains available as backup).
3. **Always-accurate stock** — every sale, purchase, and return updates inventory automatically.
4. **Works offline** — shop internet is unreliable.
5. **Speaks the shopkeeper's language** — Hindi, English, Hinglish, and regional languages, with local units (kilo, pav, dozen, packet, peti).
6. **Answers questions** — "Aaj kitna bika?", "Kaun sa maal khatam hone wala hai?"

**Non-goals (for now):**

- Full-fledged accounting software (Tally replacement)
- E-commerce storefront (can come later)
- Large retail chains / multi-warehouse enterprises

---

## 3. Target Users & Personas

### Persona 1 — Ramesh, Kirana Store Owner (primary)
- Age 45, Class 10 educated, uses WhatsApp & UPI comfortably
- Speaks Hindi/Hinglish; reads basic English
- 300–800 SKUs, 100–250 customers per day
- Keeps an udhaar notebook with ~60 regular customers
- **Needs:** fast billing, udhaar tracking, know what to reorder

### Persona 2 — Priya, Medical Store Owner
- Age 32, B.Pharm, comfortable with smartphones
- Needs batch numbers and **expiry tracking**
- **Needs:** expiry alerts, GST bills, supplier tracking

### Persona 3 — Helper / Staff (secondary user)
- Shop assistant who does billing when owner is away
- **Needs:** can make bills but should not see profit or edit prices

### Persona 4 — Hardware / Stationery Owner
- Many small items with variants (sizes, colours)
- **Needs:** variant handling ("half inch screw, 100 piece")

---

## 4. Core Principles

1. **Voice first, screen second.** Every action is possible by voice; the screen *confirms* what was understood.
2. **Confirm, don't assume.** Before anything is saved (bill, stock change), show/speak a short confirmation. Money and stock changes are never silent.
3. **Forgiving.** Understand mispronunciations, local names, brand nicknames ("Surf" = Surf Excel, "Parle" = Parle-G).
4. **Undo everything.** Any action can be reversed by saying "Galat hai, hatao" / "Undo".
5. **Big, simple UI.** Large buttons, large text, minimal menus, icons over words.
6. **Offline always.** Core features never need internet.
7. **Learns the shop.** Over time, the app learns this shop's item names, prices, and regular customers.

---

## 5. Feature List (Complete)

### 5.1 Billing (Point of Sale)
- Create bill by voice: "Ek kilo chawal, do Maggi, ek Colgate"
- Add/remove/change quantity by voice: "Maggi teen kar do", "Colgate hatao"
- Auto price lookup from item master
- Custom price on the fly: "Chawal 60 rupaye kilo lagao"
- Discount: "50 rupaye kam karo" / "10 percent discount"
- Loose-quantity & amount-based sale: "100 rupaye ka tel" → app calculates quantity
- Payment modes: Cash, UPI, Card, Udhaar (credit), Split payment
- Show UPI QR on screen with exact amount
- Calculate change: "500 diye" → "₹127 wapas dena hai"
- Print bill (Bluetooth thermal printer), share on WhatsApp, or send SMS
- GST and non-GST bills
- Hold/park a bill and resume ("Ye bill rok do", "Sharma ji wala bill kholo")
- Returns & exchange: "Ek Surf wapas aaya"

### 5.2 Inventory Management
- Add new items by voice: "Naya item — Tata Salt, ek kilo, 28 rupaye, 50 piece"
- Add items by barcode scan or photo of product (optional)
- Stock in (purchase): "Aaj 20 peti Parle-G aayi, 300 rupaye peti"
- Auto stock reduction on every sale
- Units & conversions: kg/gram, litre/ml, piece, dozen, packet, box, peti/carton (e.g., 1 peti = 24 packets)
- Item variants (size, flavour, colour)
- Low stock alerts (per-item threshold, auto-suggested from sales speed)
- Expiry tracking with batch numbers (important for medical/FMCG)
- Stock adjustment: damage, theft, expired, self-use ("2 anda toot gaye")
- Stock count / audit mode: speak counts while walking the shelves
- Price update: "Amul doodh ab 68 ka hai"
- Item aliases: multiple names for one item (Hindi name, brand nickname)
- Categories (Grocery, Personal care, Dairy, etc.)

### 5.3 Customers & Udhaar (Credit Ledger / Khata)
- Customer profiles (name, phone, nickname — "Sharma ji", "Gupta aunty")
- Add to udhaar: "Ye bill Verma ji ke khaate mein daalo"
- Record payment: "Verma ji ne 500 jama kiye"
- Check balance: "Verma ji ka kitna baaki hai?"
- Automatic WhatsApp/SMS payment reminders (with owner's approval)
- Credit limit per customer with warning
- Full ledger history per customer
- Shareable statement (PDF/image) for disputes

### 5.4 Suppliers & Purchases
- Supplier list (distributor/wholesaler)
- Record purchase bills by voice or by photo of supplier bill (OCR)
- Track amount owed to suppliers
- Auto-generated reorder list: "Kya mangwana hai?" → list grouped by supplier
- Send order to supplier on WhatsApp in one tap

### 5.5 Expenses
- "Aaj 200 rupaye bijli ka bill diya", "Chai 40 rupaye"
- Categories: rent, electricity, salary, transport, misc
- Included in profit calculation

### 5.6 Reports & Insights (ask by voice)
- "Aaj kitna bika?" → Today's sales, cash vs UPI vs udhaar
- "Is mahine ka profit kitna hua?"
- Best-selling items, slow/dead stock
- Total udhaar outstanding and oldest dues
- Stock value (at cost and at selling price)
- Daily closing summary (cash in drawer reconciliation)
- Weekly summary pushed on WhatsApp/notification
- GST reports (GSTR-1 summary, HSN-wise) — export for CA

### 5.7 Staff & Roles
- Owner, Manager, Staff roles
- Staff can bill but cannot see profit, edit prices, or delete bills (configurable)
- Activity log: who did what, when
- Voice PIN / phone PIN to switch user

### 5.8 Settings & Setup
- Shop profile (name, address, GSTIN, logo for bill)
- Language selection (UI and voice separately)
- Wake word on/off, push-to-talk mode
- Printer setup
- Backup & restore (Google Drive / cloud)
- Multi-device sync (phone + tablet at counter)

### 5.9 Onboarding Helpers
- Pre-loaded catalogue of common products (top ~5,000 FMCG items with images, MRP, HSN)
- Import from Excel/CSV
- "Photo of shelf" quick setup (later phase)
- Guided voice tutorial on first launch

---

## 6. Voice Command System

### 6.1 How the user talks to the app

| Mode | How it works | When to use |
|---|---|---|
| **Push-to-talk** (default) | Hold big mic button, speak, release | Noisy shops; most reliable |
| **Tap-to-talk** | Tap once, app listens until silence | Hands partly free |
| **Wake word** (optional) | "Hey Dukaan" / custom word | Hands completely busy; quieter shops |

### 6.2 Command Categories (Intents)

| Intent | Example (Hinglish) | Example (English) |
|---|---|---|
| `ADD_TO_BILL` | "Do kilo atta, ek Surf" | "Add 2 kg flour and one Surf" |
| `UPDATE_BILL_ITEM` | "Atta teen kilo kar do" | "Make flour 3 kg" |
| `REMOVE_FROM_BILL` | "Surf hatao" | "Remove Surf" |
| `APPLY_DISCOUNT` | "20 rupaye kam karo" | "Give 20 rupees off" |
| `SHOW_TOTAL` | "Total kitna hua?" | "What's the total?" |
| `COMPLETE_BILL` | "Cash mein, 500 diye" | "Paid cash, 500 given" |
| `BILL_TO_CREDIT` | "Sharma ji ke khaate mein" | "Put on Sharma's credit" |
| `HOLD_BILL` / `RESUME_BILL` | "Bill rok do" / "Pichhla bill kholo" | "Hold bill" / "Open last bill" |
| `RETURN_ITEM` | "Ek Maggi wapas" | "Return one Maggi" |
| `STOCK_IN` | "50 Maggi aayi, 12 rupaye wali" | "Received 50 Maggi at 12 each" |
| `ADD_ITEM` | "Naya item Fortune tel, ek litre, 160" | "New item Fortune oil 1L, 160" |
| `UPDATE_PRICE` | "Doodh 68 ka kar do" | "Set milk price to 68" |
| `CHECK_STOCK` | "Atta kitna bacha hai?" | "How much flour is left?" |
| `ADJUST_STOCK` | "3 anda toot gaye" | "3 eggs broken" |
| `RECORD_PAYMENT` | "Verma ji ne 1000 diye" | "Verma paid 1000" |
| `CHECK_BALANCE` | "Gupta ji ka kitna baaki?" | "How much does Gupta owe?" |
| `ADD_EXPENSE` | "Bijli ka bill 1200" | "Electricity bill 1200" |
| `QUERY_REPORT` | "Aaj kitna bika?" | "Today's sales?" |
| `REORDER_LIST` | "Kya mangwana hai?" | "What should I order?" |
| `UNDO` | "Galat hai, wapas karo" | "Undo" |
| `CANCEL` | "Chhodo" / "Cancel" | "Cancel" |
| `HELP` | "Kya bol sakta hoon?" | "What can I say?" |

### 6.3 Entities the system must extract

- **Item** — matched against the shop's catalogue + aliases (fuzzy match)
- **Quantity** — numbers in words/digits, any language ("dedh" = 1.5, "dhai" = 2.5, "sawa" = 1.25, "paune do" = 1.75)
- **Unit** — kilo, gram, pav (250 g), adha kilo, litre, packet, dozen, peti, darjan
- **Price / Amount** — "100 ka", "50 rupaye wala"
- **Customer** — name/nickname match
- **Payment mode** — cash, UPI, PhonePe/GPay/Paytm (→ UPI), udhaar/khaata
- **Date/time** — "kal", "pichhle hafte", "is mahine"

### 6.4 Indian quantity & unit dictionary (must support)

| Spoken | Meaning |
|---|---|
| pav / paav | 250 g |
| adha / aadha | 0.5 |
| dedh | 1.5 |
| dhai / dhaai | 2.5 |
| sawa ___ | ___ + 0.25 |
| saadhe ___ | ___ + 0.5 |
| paune ___ | ___ − 0.25 |
| darjan | 12 |
| peti / carton | configured per item (e.g., 24/48) |
| "100 ka" / "100 rupaye ka" | amount-based quantity |

### 6.5 Confirmation behaviour

- **Low risk** (adding item to a bill): show on screen instantly + short beep; no spoken confirmation needed.
- **Medium risk** (completing a bill, stock in): show summary; auto-confirm after 2 seconds unless user says "ruko"/"cancel".
- **High risk** (deleting items, price change, udhaar entry, payment received): require explicit "haan"/"yes" or tap.
- **Low confidence** (ASR or matching uncertain): ask a clarifying question — "Tata Salt ya Tata Salt Lite?" with on-screen tappable options.

### 6.6 Voice feedback (text-to-speech)

- Optional spoken replies in the shopkeeper's language ("Total 347 rupaye")
- Can be turned off (some shopkeepers prefer silent screens)
- Short, natural phrases; never long sentences

---

## 7. User Flows

### 7.1 Quick Sale (most common flow)

```
Shopkeeper (holds mic): "Do kilo atta, ek Surf Excel, teen Maggi"
App: shows 3 items in bill, running total ₹312
Shopkeeper: "Maggi do kar do"
App: updates Maggi qty → total ₹298
Shopkeeper: "UPI se"
App: shows UPI QR for ₹298
(Payment confirmed via UPI notification / sound box / manual tap)
App: "Bill ho gaya" → prints/WhatsApps bill, stock reduced
```

### 7.2 Udhaar Sale

```
Shopkeeper: "Ek kilo cheeni, aadha kilo daal, Sharma ji ke khaate mein"
App: "Sharma ji (Gandhi Nagar) — ₹145 khaate mein. Pehle ka baaki ₹1,200. Kul ₹1,345. Theek hai?"
Shopkeeper: "Haan"
App: saves bill, updates ledger, optionally sends WhatsApp to Sharma ji
```

### 7.3 Receiving Stock

```
Shopkeeper: "Stock aaya — 10 peti Parle-G, 5 bori chawal 25 kilo wali, supplier Agarwal Traders, 8,400 ka bill, 5,000 diye"
App: shows parsed list → asks for any missing purchase prices
Shopkeeper: confirms
App: adds stock, records supplier payable ₹3,400
```

### 7.4 End of Day

```
Shopkeeper: "Aaj ka hisaab"
App: "Aaj 186 bill, ₹24,560 bikri. Cash ₹14,200, UPI ₹8,110, udhaar ₹2,250. Anumanit munafa ₹3,900. 6 cheezein khatam hone wali hain."
```

### 7.5 Ambiguity Handling

```
Shopkeeper: "Ek Colgate"
App (multiple matches): shows cards — Colgate 100g ₹55 | Colgate 200g ₹105 | Colgate MaxFresh ₹95
App (voice): "Kaun sa Colgate?"
Shopkeeper: "Sau gram wala" / taps option
App: learns preference — next time "Colgate" defaults to the most frequently sold variant
```

### 7.6 Unknown Item

```
Shopkeeper: "Ek Kurkure Masala Munch"
App: "Ye item list mein nahi hai. Daam bataiye?"
Shopkeeper: "20 rupaye"
App: adds to bill AND creates new item (flagged "needs stock details" for later)
```

---

## 8. Screens & UI

### 8.1 Main screens

1. **Home / Billing screen** (default) — current bill, giant mic button, total, payment buttons
2. **Inventory** — item list with stock levels, search, low-stock filter
3. **Khata (Customers)** — customers with balances, sorted by amount due
4. **Reports** — today / week / month cards
5. **More** — suppliers, expenses, staff, settings

### 8.2 Billing screen layout (phone)

```
┌─────────────────────────────┐
│ Shop name        🟢 Online   │
├─────────────────────────────┤
│ 🎤 "do kilo atta, ek surf"   │  ← live transcript
├─────────────────────────────┤
│ Atta (Aashirvaad)  2kg  ₹110 │
│ Surf Excel 1kg      1   ₹140 │
│ Maggi              3    ₹42  │
│                              │
├─────────────────────────────┤
│ TOTAL                  ₹292  │
├─────────────────────────────┤
│ [Cash] [UPI] [Udhaar] [More] │
│                              │
│        ( 🎤 HOLD TO TALK )   │  ← biggest element
└─────────────────────────────┘
```

### 8.3 UI rules
- Minimum touch target 56 dp; mic button ~120 dp
- High contrast; readable in sunlight
- Product images where available (helps low-literacy users)
- Numbers always in large font
- Colour cues: green = done, orange = needs confirmation, red = error/low stock
- Every voice action also has a tap equivalent

---

## 9. System Architecture

### 9.1 High-level diagram

```
┌──────────────────────── Mobile / Tablet App ────────────────────────┐
│                                                                     │
│  Mic → [On-device VAD] → [Speech-to-Text] → [Intent & Entity Parser] │
│                              │                     │                │
│                     (on-device or cloud)    (rules + LLM fallback)  │
│                                                    ↓                │
│                                         [Action Engine / Validator] │
│                                                    ↓                │
│                       [Confirmation UI + Text-to-Speech feedback]   │
│                                                    ↓                │
│                         [Local DB (SQLite)] ←→ [Sync Engine]        │
│                                                    │                │
└────────────────────────────────────────────────────┼────────────────┘
                                                     │ (when online)
┌─────────────────────────── Cloud Backend ─────────┼────────────────┐
│  API Gateway → Auth → Sync Service → PostgreSQL   ↓                │
│  ├── Voice/NLU Service (heavier models, LLM)                       │
│  ├── Reports & Analytics                                            │
│  ├── Notifications (WhatsApp Business API, SMS, Push)               │
│  ├── Product Catalogue Service (master FMCG list)                   │
│  ├── OCR Service (supplier bills)                                   │
│  └── Backup storage                                                 │
└─────────────────────────────────────────────────────────────────────┘
```

### 9.2 Key components

| Component | Responsibility |
|---|---|
| **Voice Capture** | Mic handling, noise suppression, voice activity detection (VAD) |
| **ASR (Speech-to-Text)** | Converts audio to text; supports Hindi/English/code-mixed speech |
| **NLU (Understanding)** | Detects intent + extracts entities (item, qty, unit, price, customer) |
| **Catalogue Matcher** | Fuzzy-matches spoken item names to shop's items using aliases, phonetics, and sales history |
| **Action Engine** | Converts parsed command into a structured action; validates (stock exists, price present, credit limit) |
| **Confirmation Manager** | Decides if confirmation is needed based on risk + confidence |
| **Local DB** | Source of truth on device; all operations work offline |
| **Sync Engine** | Two-way sync with cloud; conflict resolution |
| **Notification Service** | WhatsApp bills, udhaar reminders, low-stock alerts |
| **Reports Engine** | Aggregations; can run locally for basic reports |

---

## 10. Data Model

### 10.1 Entities (simplified)

```
Shop
  id, name, address, phone, gstin, logo_url, language, created_at

User
  id, shop_id, name, phone, role (owner|manager|staff), pin_hash

Item
  id, shop_id, name, category_id, brand, barcode, hsn_code,
  base_unit (kg|g|l|ml|pcs), selling_price, purchase_price, mrp,
  gst_rate, current_stock, low_stock_threshold, track_expiry (bool),
  image_url, is_active, created_at, updated_at

ItemAlias
  id, item_id, alias_text, language, source (user|system|learned)

ItemUnitConversion
  id, item_id, unit_name (peti|dozen|bori), factor_to_base_unit

ItemVariant (optional)
  id, item_id, variant_name, price, stock

Batch (for expiry tracking)
  id, item_id, batch_no, expiry_date, quantity, purchase_price

Customer
  id, shop_id, name, nickname, phone, address, credit_limit,
  current_balance, created_at

Supplier
  id, shop_id, name, phone, gstin, current_payable

Bill (Sale)
  id, shop_id, bill_number, customer_id (nullable), user_id,
  subtotal, discount, tax_total, grand_total, status (draft|held|completed|cancelled),
  is_gst_bill, created_at, voice_session_id

BillItem
  id, bill_id, item_id, batch_id, quantity, unit, unit_price,
  discount, tax_amount, line_total

Payment
  id, shop_id, bill_id (nullable), customer_id (nullable),
  mode (cash|upi|card|credit), amount, reference, created_at

LedgerEntry (Khata)
  id, customer_id, type (debit|credit), amount, bill_id, payment_id,
  note, balance_after, created_at

Purchase
  id, shop_id, supplier_id, invoice_no, total, paid, created_at

PurchaseItem
  id, purchase_id, item_id, quantity, unit, cost_price, batch_no, expiry_date

StockMovement   ← audit trail for every stock change
  id, item_id, batch_id, change_qty, reason (sale|purchase|return|damage|adjustment|count),
  reference_id, user_id, created_at

Expense
  id, shop_id, category, amount, note, created_at

VoiceLog        ← for debugging & improving recognition (with consent)
  id, shop_id, user_id, transcript, parsed_intent_json, confidence,
  action_taken, was_corrected (bool), created_at

ActivityLog
  id, shop_id, user_id, action, entity, entity_id, before_json, after_json, created_at
```

### 10.2 Important rules
- Stock is **never** edited directly; it changes only through `StockMovement` records (gives full audit + undo).
- Udhaar balance is derived from `LedgerEntry`; `current_balance` is a cached value.
- All IDs are UUIDs (needed for offline creation + sync).
- Soft deletes only (`is_active` / `status=cancelled`) — nothing financial is hard-deleted.

---

## 11. Voice/AI Pipeline in Detail

### 11.1 Step-by-step

1. **Capture** — record audio while mic held; apply noise suppression (shops are noisy: traffic, customers, TV).
2. **ASR** — convert to text.
   - Options: on-device model (fast, offline, lower accuracy) vs cloud model (higher accuracy for Hindi/Hinglish).
   - Strategy: on-device first; if confidence low and online, re-run in cloud.
   - Bias the ASR with the shop's item names and customer names (custom vocabulary / contextual biasing).
3. **Normalise** — convert number words to digits ("dedh" → 1.5), standardise units, transliterate Devanagari ↔ Latin for matching.
4. **Intent + Entity Extraction**
   - **Layer 1: Rule/grammar parser** — handles 80% of common patterns quickly and offline.
   - **Layer 2: Small on-device ML model** — for less structured sentences.
   - **Layer 3: LLM (cloud)** — for complex/unusual commands; returns strict JSON schema.
5. **Catalogue matching** — fuzzy + phonetic match (e.g., Double Metaphone adapted for Indian names, edit distance on transliterated text), boosted by:
   - item popularity in this shop
   - recent items
   - learned aliases from corrections
6. **Validation** — does item exist? enough stock? price known? customer exists? within credit limit?
7. **Confirmation decision** — based on action risk + confidence score.
8. **Execute** — write to local DB inside a transaction; create undo record.
9. **Feedback** — update screen instantly; optional TTS.
10. **Learn** — if the user corrected something, store alias/preference.

### 11.2 Example: parser output (JSON)

Input: *"do kilo atta, teen Maggi aur sau rupaye ka tel, Sharma ji ke khaate mein"*

```json
{
  "intent": "CREATE_BILL",
  "items": [
    { "spoken": "atta",  "item_id": "itm_102", "qty": 2,   "unit": "kg",  "confidence": 0.94 },
    { "spoken": "maggi", "item_id": "itm_045", "qty": 3,   "unit": "pcs", "confidence": 0.98 },
    { "spoken": "tel",   "item_id": null,      "amount": 100, "confidence": 0.61,
      "candidates": ["itm_210 Fortune Soyabean", "itm_211 Mustard Oil Loose"] }
  ],
  "payment": { "mode": "credit", "customer_id": "cus_007", "customer_spoken": "sharma ji" },
  "needs_clarification": ["items[2]"]
}
```

### 11.3 Accuracy improvement loop
- Log (with consent) transcripts + corrections
- Shop-specific alias learning runs on-device
- Anonymised aggregate data improves global models (opt-in only)

### 11.4 Latency targets
| Step | Target |
|---|---|
| Speech end → transcript | < 700 ms (on-device) |
| Transcript → items on screen | < 300 ms |
| Total voice-to-screen | **< 1.2 s** |

---

## 12. Offline-First Strategy

- All core features (billing, stock, khata, basic reports) run fully offline.
- Local database (SQLite) is the source of truth on the device.
- Sync queue: every change is logged as an operation and pushed when online.
- **Conflict resolution:**
  - Stock: merge using movements (additive), not overwrite.
  - Prices/item details: last-write-wins with timestamp; owner's changes win over staff.
  - Bills: never conflict (unique UUIDs, append-only).
- Cloud-only features degrade gracefully: WhatsApp messages queue until online; cloud ASR falls back to on-device.
- Periodic automatic backup when on Wi-Fi.

---

## 13. Security, Privacy & Permissions

- **Login:** phone number + OTP; device-level PIN/biometric for app open.
- **Roles:** owner / manager / staff with configurable permissions.
- **Data encryption:** at rest (encrypted SQLite) and in transit (TLS).
- **Voice privacy:**
  - Audio is processed and discarded by default; not stored.
  - Mic is active only when user presses the button (or wake word enabled explicitly).
  - Clear indicator when listening.
- **Customer data:** phone numbers used only for bills/reminders the owner sends; never sold.
- **Compliance:** follow India's Digital Personal Data Protection Act (DPDP) requirements — consent, purpose limitation, data deletion on request. *(Verify current rules with a legal advisor before launch.)*
- **Audit log** for all sensitive actions (price changes, bill deletion, stock adjustment).

---

## 14. Edge Cases & Error Handling

| Situation | Behaviour |
|---|---|
| Noisy environment, nothing understood | "Phir se boliye" + show tap-based fallback |
| Two people speaking | Use push-to-talk; ignore audio outside press window |
| Item not in catalogue | Ask price → add to bill + create draft item |
| Multiple matches | Show options as large cards; learn choice |
| Stock goes negative | Allow sale (real stock may exist) but flag item for stock check |
| Customer name ambiguous ("Sharma ji" ×3) | Show options with area/phone |
| Udhaar over credit limit | Warn owner; require explicit "haan" |
| Wrong item added | "Galat hai" removes last action; full undo stack per bill |
| Price changed mid-day | Old bills keep old price (price stored on BillItem) |
| App crash mid-bill | Draft bill auto-saved locally, restored on reopen |
| Phone battery low | Warning; keep drafts safe |
| Payment received by UPI but not tapped | Integrate UPI payment notification (later) or remind to confirm |
| Staff tries restricted action by voice | "Ye karne ki permission nahi hai" |
| Mixed language sentence | Supported by design (code-mixed ASR + NLU) |
| Decimal confusion ("do sau pachaas" vs "2.50") | Context rules: price vs quantity; confirm unusual values |
| Very large number spoken (likely error) | Sanity check vs typical qty/price; ask to confirm |

---

## 15. Tech Stack Recommendation

> These are suggestions; choose based on your team's skills. Verify current versions, pricing and language support before committing.

| Layer | Option A (recommended start) | Option B |
|---|---|---|
| Mobile app | **Flutter** (Android first — most shopkeepers use Android) | React Native / Kotlin native |
| Local DB | SQLite (via Drift/sqflite) | Realm / ObjectBox |
| Backend | Node.js (NestJS) or Python (FastAPI) | Go |
| Cloud DB | PostgreSQL | — |
| Sync | Custom op-log sync, or PowerSync / ElectricSQL-style tool | Firebase Firestore (simpler, less control) |
| On-device ASR | Whisper-family small models / Vosk / Android on-device speech | — |
| Cloud ASR (Indian languages) | Indian-language-focused speech APIs (e.g., AI4Bharat IndicConformer open models, Sarvam AI, Bhashini) | Google Cloud Speech, Azure Speech |
| NLU | Rule parser + small classifier; LLM (e.g., Claude API) for complex/fallback with JSON output | — |
| TTS | Android TTS (offline) + Indian-language cloud TTS | — |
| Messaging | WhatsApp Business API (via a BSP), SMS gateway | — |
| Payments | UPI intent / dynamic QR via a payment aggregator | — |
| Printing | ESC/POS Bluetooth thermal printer (58mm/80mm) | — |
| OCR (supplier bills) | Cloud OCR / vision LLM | — |
| Hosting | AWS / GCP (Mumbai region for latency & data locality) | — |
| Analytics & crash | Firebase Crashlytics, PostHog/Mixpanel | — |

---

## 16. MVP Scope vs Later Phases

### MVP (Phase 1) — "Bill and stock by voice"
- Android app, Hindi + English + Hinglish
- Push-to-talk voice billing (add/remove/change qty, total, complete)
- Item master with aliases, units, prices; pre-loaded common catalogue
- Auto stock deduction; stock-in by voice
- Cash / UPI QR / Udhaar payment
- Basic khata (customer balance, record payment)
- WhatsApp bill sharing (share intent — no API needed initially)
- Today's sales & low-stock list (voice query)
- Offline mode + cloud backup
- Single user (owner)

### Phase 2 — "Run the whole shop"
- Suppliers & purchases, payables
- Expenses & profit report
- Staff roles & permissions
- Bluetooth printer
- Udhaar reminders via WhatsApp API
- Expiry & batch tracking
- GST bills and basic GST reports
- More languages (Bengali, Marathi, Tamil, Telugu, Gujarati, Bhojpuri/Maithili-accented Hindi, etc.)

### Phase 3 — "Smart shop"
- Wake word mode
- Supplier bill OCR (photo → stock in)
- Smart reorder suggestions (sales velocity, seasonality, festivals)
- Multi-device sync (counter tablet + owner phone)
- Customer loyalty / offers
- Online ordering link for customers (WhatsApp catalogue)
- Integration with distributors for direct ordering
- Lending/working-capital partnerships based on sales data (with consent)

---

## 17. Monetization

| Model | Details |
|---|---|
| **Freemium** | Free: billing + stock up to N bills/month. Paid: unlimited, reports, staff, GST, reminders |
| **Subscription tiers** | e.g., Basic / Pro / Business — monthly or yearly (yearly discount) |
| **Hardware bundles** | Printer + app subscription package |
| **Transaction services** | Commission on payments, SMS/WhatsApp packs |
| **B2B data insights** | Aggregated, anonymised demand data for FMCG brands (strictly opt-in, privacy-safe) |
| **Distributor partnerships** | Referral fees for reorders placed via app |
| **Credit/lending partners** | Referral fee (regulated — needs legal review) |

*Price point must be low — compare with what shopkeepers already pay for existing billing/khata apps.*

---

## 18. Competitive Landscape

Existing apps in this space (verify current features before final positioning):

- **Khata/ledger apps** — digital udhaar books (e.g., Khatabook, OkCredit)
- **Billing/inventory apps** — GST billing & stock (e.g., Vyapar, myBillBook, Marg ERP for pharmacies)
- **Payment-led apps** — payment companies offering merchant tools

**Our differentiator:**
1. **Voice-first** for *everything*, not just search
2. **Understands how shopkeepers actually speak** (local units, Hinglish, nicknames, amount-based sales)
3. **Zero-setup** — learns item names and prices as you use it
4. **One app** for billing + stock + khata + reports

---

## 19. Success Metrics

| Metric | Target (initial) |
|---|---|
| Voice command success rate (no correction needed) | > 90% |
| Avg. time to complete a 4-item bill | < 15 seconds |
| % of bills created by voice | > 60% |
| Day-7 retention | > 40% |
| Day-30 retention | > 25% |
| Daily active shops / monthly active shops | > 50% |
| Free → paid conversion | 5–10% |
| Crash-free sessions | > 99.5% |

---

## 20. Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Voice accuracy poor in noisy shops | Push-to-talk, noise suppression, custom vocabulary, always-visible confirmation, easy correction |
| Dialect/accent variety | Start with one region, collect (consented) data, fine-tune models |
| Cloud ASR/LLM costs too high | Rule parser + on-device first; cloud only as fallback; cache results |
| Shopkeepers don't trust software with money | Clear confirmations, undo, audit log, WhatsApp copies to customers |
| Onboarding effort (entering 500 items) | Pre-loaded catalogue, learn-as-you-sell, Excel import, (later) shelf photo |
| Strong existing competitors | Differentiate on voice UX; target segment that finds current apps too hard |
| Data loss | Auto backup, local + cloud copies |
| Regulatory (GST, data privacy, payments, lending) | Legal/CA review before related features launch |
| Low willingness to pay | Generous free tier, value-proof reports ("you saved ₹X in lost udhaar") |

---

## 21. Development Roadmap

| Week | Milestone |
|---|---|
| 1–2 | User research: visit 15–20 shops, record (with permission) how they speak during billing; finalise MVP scope |
| 3–4 | Design: wireframes, voice grammar, data model; clickable prototype tested with 5 shopkeepers |
| 5–8 | Build core: item master, billing, stock, khata, local DB, offline mode |
| 7–10 | Voice pipeline: ASR integration, rule parser, catalogue matcher, confirmation UI |
| 11–12 | Reports, WhatsApp share, UPI QR, backup/sync |
| 13–14 | Internal testing + noisy-environment testing |
| 15–18 | **Pilot** with 20–50 shops; daily feedback; fix accuracy issues |
| 19–20 | Public beta on Play Store |
| 21+ | Phase 2 features based on pilot learnings |

**Suggested initial team:** 1 product/founder, 2 mobile devs, 1 backend dev, 1 ML/voice engineer, 1 designer (part-time), 1 field person for shop onboarding.

---

## 22. Open Questions

1. Which region/language should the pilot start with?
2. Which shop type first — kirana only, or also medical/hardware?
3. Phone only, or also counter tablet?
4. Build own ASR fine-tuning, or rely on third-party APIs initially?
5. How will UPI payment be confirmed automatically (sound-box integration, PG webhook, SMS read)?
6. Pricing — what will a small shopkeeper realistically pay per month?
7. Is GST billing needed in MVP, or can it wait (many small shops are below the GST threshold)?
8. Will staff use their own phones or the shop's device?
9. Brand name and voice persona (male/female voice, formal/friendly tone)?

---

## 23. Glossary

| Term | Meaning |
|---|---|
| Kirana | Neighbourhood grocery store |
| Udhaar | Goods given on credit |
| Khata / Bahi-khata | Ledger / account book |
| Peti | Carton/case of packaged goods |
| Bori | Sack (rice, flour, sugar) |
| Pav | Quarter kilo (250 g) |
| ASR | Automatic Speech Recognition (speech-to-text) |
| NLU | Natural Language Understanding (meaning extraction) |
| TTS | Text-to-Speech |
| VAD | Voice Activity Detection |
| SKU | Stock Keeping Unit (one distinct item) |
| HSN | Harmonized System of Nomenclature — product codes used in GST |
| MRP | Maximum Retail Price |
| GSTIN | GST Identification Number |

---

*Document version 0.1 — first draft. Next step: validate with real shopkeepers before building.*
