# 📚 Media Library — Κεντρική Βιβλιοθήκη Πολυμέσων

Κεντρικό αποθετήριο πολυμέσων για το σύστημα εκπαιδευτικών εφαρμογών.  
Διαχειρίζεται URLs εικόνων, βίντεο, παρουσιάσεων και interactive books που χρησιμοποιούνται από όλες τις εφαρμογές.

---

## 🗂️ Περιεχόμενα repo

```
media-library/
├── media_library.json     ← Το αρχείο που διαβάζουν οι εφαρμογές (auto-generated)
├── media_library.xlsx     ← Αρχείο αναφοράς / backup
└── README.md
```

Το `media_library.json` **δεν επεξεργάζεσαι χειροκίνητα** — παράγεται αυτόματα από το Google Sheet μέσω Apps Script.

---

## ⚙️ Πώς λειτουργεί το σύστημα

```
Google Sheet (Media Library)
        ↓  [Apps Script — κουμπί Sync]
media_library.json στο GitHub
        ↓  [raw URL — public]
Lesson Orchestrator (claude.ai)
        ↓  [fetch κατά τη δημιουργία JSON]
JSON εφαρμογών με συμπληρωμένα media URLs
```

### Βήμα-βήμα ροή

1. Προσθέτεις/αλλάζεις εγγραφές στο **Google Sheet**
2. Πατάς **📚 Media Library → 🔄 Sync → GitHub** από το μενού του Sheet
3. Το Apps Script μετατρέπει το Sheet σε JSON και κάνει push στο GitHub
4. Το `media_library.json` ενημερώνεται αυτόματα στο repo
5. Ο **Lesson Orchestrator** διαβάζει το νέο JSON την επόμενη φορά που τρέχει

---

## 📋 Δομή του `media_library.json`

```json
{
  "meta": {
    "description": "Media Library — Κεντρική Βιβλιοθήκη Πολυμέσων",
    "version": "1.0",
    "last_updated": "2026-03-23",
    "total": 11
  },
  "media": [
    {
      "media_id": "IMG-002",
      "type": "image",
      "title": "Δελφοί — Χάρτης Κλασικής Ελλάδας",
      "url": "https://i.ibb.co/h1mwhLDV/Mapping-Classical-Greece-7.jpg",
      "source_platform": "imgbb",
      "app_context": ["Mind Palace", "Explorer History", "Timeline Map", "Interactive Book"],
      "unit_id": "",
      "tags": ["Δελφοί", "τόπος", "αρχαία Ελλάδα", "μαντείο"],
      "language": "el",
      "notes": "Από παρουσίαση Mapping Classical Greece",
      "date_added": "2026-03-19",
      "status": "active"
    }
  ]
}
```

### Πεδία εγγραφής

| Πεδίο | Τύπος | Περιγραφή |
|---|---|---|
| `media_id` | string | Μοναδικό ID π.χ. `IMG-001`, `VID-002`, `SLD-001` |
| `type` | string | `image`, `youtube`, `google_slides`, `interactive_book` |
| `title` | string | Περιγραφικός τίτλος |
| `url` | string | Πλήρες URL πολυμέσου |
| `source_platform` | string | `imgbb`, `YouTube`, `Google Drive`, `GitHub Pages` |
| `app_context` | array | Εφαρμογές που το χρησιμοποιούν |
| `unit_id` | string | Διδακτική ενότητα π.χ. `U01`, `U02` (προαιρετικό) |
| `tags` | array | Λέξεις-κλειδιά για αναζήτηση |
| `language` | string | `el`, `en`, `el/en` |
| `notes` | string | Σημειώσεις / πηγή |
| `date_added` | string | Ημερομηνία προσθήκης `YYYY-MM-DD` |
| `status` | string | `active`, `draft`, `archived`, `needs_review` |

### Σύμβαση ονομασίας media_id

| Prefix | Τύπος |
|---|---|
| `IMG-` | Εικόνα |
| `VID-` | Βίντεο YouTube |
| `SLD-` | Google Slides |
| `HTML-` | Interactive Book |

Αρίθμηση: `001`, `002`, `003`... με σειρά προσθήκης.

---

## 🔧 Εγκατάσταση Apps Script

### Προαπαιτούμενα

- Google Sheet με tab ονόματι **📚 Media Library**
- GitHub Personal Access Token (classic) με scope **`repo`**

### Βήμα 1 — Άνοιξε τον Apps Script editor

Στο Google Sheet:  
**Extensions → Apps Script**

Ανοίγει νέα καρτέλα με τον editor.

### Βήμα 2 — Επικόλλησε τον κώδικα

1. Διέγραψε ό,τι υπάρχει (`function myFunction() {}`)
2. Επικόλλησε ολόκληρο τον κώδικα του `MediaLibrary_AppScript.js`
3. **Ctrl+S** για αποθήκευση

### Βήμα 3 — Βάλε το GitHub Token

Βρες τη γραμμή:
```javascript
GITHUB_TOKEN: '',  // ← εδώ
```
Βάλε το token σου:
```javascript
GITHUB_TOKEN: 'ghp_xxxxxxxxxxxxxxxxxxxx',
```

> ⚠️ **Ποτέ μην κάνεις commit τον κώδικα με το token στο GitHub.** Το script μένει μόνο στο Google Sheet.

### Βήμα 4 — Πρώτη εκτέλεση (permissions)

Στον editor:
1. Από το dropdown επίλεξε `onOpen`
2. Πάτα **▶ Run**
3. Θα εμφανιστεί παράθυρο — επίλεξε **Review permissions → Allow**

Αυτό γίνεται **μόνο μία φορά**.

### Βήμα 5 — Επιστροφή στο Sheet

1. Κλείσε τον editor
2. **F5** ή ανανέωσε τη σελίδα
3. Θα εμφανιστεί νέο μενού **📚 Media Library** στη γραμμή μενού

---

## 🚀 Καθημερινή χρήση

### Προσθήκη νέου media

1. Άνοιξε το Google Sheet
2. Πρόσθεσε νέα γραμμή με τα πεδία
3. **📚 Media Library → 🔄 Sync → GitHub**
4. Μήνυμα επιτυχίας → το JSON είναι ενημερωμένο

### Preview πριν το sync

Αν θέλεις να δεις τι θα ανεβάσεις **χωρίς** να κάνεις push:  
**📚 Media Library → 👁️ Preview JSON (Logs)**  
Μετά: **Extensions → Apps Script → Logs** για να δεις το πλήρες JSON.

### Αν κάτι πάει στραβά

Άνοιξε τον Apps Script editor → **View → Logs** — εκεί φαίνεται ακριβώς τι έγινε και ποιο ήταν το σφάλμα.

---

## 🔗 Raw URL για τον Orchestrator

```
https://raw.githubusercontent.com/dporpatonelis-crypto/media-library/refs/heads/main/media_library.json
```

Αυτό το URL χρησιμοποιεί ο **Lesson Orchestrator** για να διαβάζει το library κατά τη δημιουργία JSON.  
Δεν χρειάζεται authentication — είναι public.

---

## 🔄 Σχέση με τον Lesson Orchestrator

Ο Orchestrator (HTML artifact στο claude.ai) φέρνει αυτόματα το library όταν τρέχει και αντιστοιχίζει media με βάση:

- **Θέμα μαθήματος** — αν λέει "Δελφοί", βλέπει `IMG-002`
- **app_context** — βάζει μόνο media που αντιστοιχούν στην εφαρμογή-στόχο
- **type** — για Timeline βάζει `image` στο `img`, για Interactive Books βάζει `youtube` στο `videoId`, κ.λπ.

### Αντιστοίχιση type → πεδίο JSON εφαρμογής

| Media type | Εφαρμογή | Πεδίο JSON |
|---|---|---|
| `image` | Timeline Explorer | `img` |
| `image` | History Explorer 3D | (assets) |
| `youtube` | Interactive Books | `videoId` |
| `google_slides` | Timeline Explorer | `slides` |
| `google_slides` | Mind Palace | `book_reference` |
| `interactive_book` | Mind Palace | layers reference |

---

## 📅 Ιστορικό εκδόσεων

| Ημερομηνία | Έκδοση | Αλλαγές |
|---|---|---|
| 2026-03-23 | 1.0 | Αρχική έκδοση — 11 εγγραφές |

---

*Μέρος του συστήματος εκπαιδευτικών εφαρμογών — Δ. Πορπατονέλης*
