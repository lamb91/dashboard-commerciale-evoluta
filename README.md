# Ellysse Suite — Dashboard Commerciale Evoluta

Una suite di 4 tool intelligenti per l'analisi della pipeline commerciale Ellysse. Funziona interamente nel browser — nessun server, nessun backend, i dati non escono mai dal tuo dispositivo.

---

## Come iniziare

1. Scarica o clona la repo
2. Apri **`Ellysse_Suite.html`** nel browser (doppio click)
3. Inserisci la **chiave API OpenRouter** una sola volta in alto a destra → viene distribuita automaticamente a tutti i tool
4. Carica i tuoi file Excel e inizia ad analizzare

> La chiave OpenRouter è gratuita su [openrouter.ai/keys](https://openrouter.ai/keys). Il modello usato è `google/gemini-2.5-flash-lite` — costo stimato < $0.01/query.

---

## I 4 Tool

### 📈 Pipeline Evolution — Analisi multi-periodo
**File:** `Ellysse_Evolution.html`

Il tool principale. Carica gli Excel delle mensilità (uno per mese) e confronta automaticamente l'evoluzione di ogni trattativa.

**Flow:**
1. Trascina 2+ file Excel nella sidebar (un file = un periodo/mese)
2. Il sistema rileva automaticamente le colonne Ellysse (`Trattativa ID`, `Probabilità previsione`, `Importo`, `Company Name`, `Proprietario`, ecc.)
3. I periodi vengono ordinati **cronologicamente** in automatico
4. La vista principale **Tracciamento deal periodo per periodo** mostra ogni deal con:
   - Probabilità per ciascun mese (con badge colorati %)
   - Variazione `Δpp` (es. `+15pp`, `-8pp`)
   - Stato: 🟢 Salita / 🔴 Discesa / 🔵 Nuovo / ⬜ Stabile / ✗ Uscito
   - Importo e ARR
5. I grafici secondari mostrano: pipeline totale, fasce probabilità per periodo, distribuzione famiglie offerta, top deal, stato deal, ARR per agente
6. La **chat AI** in basso risponde a domande sui dati (es. *"Chi sta performando meglio?"*, *"Quali deal ad alto valore sono a rischio?"*)

**Filtri disponibili:** per stato, per agente, ricerca testo libero

---

### 📊 Smart Dashboard — Explorer generico Excel
**File:** `Ellysse_SmartDash.html`

Dashboard universale: carica qualsiasi file Excel e ottieni KPI, grafici e analisi AI automatiche — senza alcuna configurazione.

**Flow:**
1. Carica un file Excel (qualsiasi formato)
2. La dashboard analizza le colonne e genera automaticamente: KPI numerici, grafici a barre, a torta, trend temporali
3. Seleziona il foglio da analizzare dal menu a tendina
4. Usa la **chat AI** per fare domande in linguaggio naturale sui tuoi dati

---

### 🤖 AI Analyst — Interrogazione dati in linguaggio naturale
**File:** `Ellysse_AIAnalyst.html`

Tool focalizzato sull'interazione conversazionale con i dati Excel. Progettato per chi preferisce fare domande piuttosto che guardare grafici.

**Flow:**
1. Alla prima apertura: inserisci la chiave OpenRouter (già pre-compilata se usata dalla Suite)
2. Carica uno o più file Excel dalla sidebar
3. Scrivi domande in italiano es: *"Quali sono le 5 aziende con più fatturato?"*, *"Mostrami i trend mensili"*, *"C'è correlazione tra probabilità e importo?"*
4. L'AI risponde con tabelle, analisi e insight strutturati

---

### ⚔️ War Room 2026 — Dashboard strategica
**File:** `Ellysse_WarRoom_2026.html`

Vista d'insieme delle performance commerciali 2026. Dashboard con KPI ad anello, metriche chiave e analisi visuale delle performance di team e singoli agenti. Pagina scrollabile, non richiede caricamento file.

---

## Struttura della repo

```
dashboard-commerciale-evoluta/
│
├── Ellysse_Suite.html          ← PUNTO DI ACCESSO UNICO (apri questo)
│
├── Ellysse_Evolution.html      ← Tool 1: analisi multi-periodo pipeline
├── Ellysse_SmartDash.html      ← Tool 2: explorer generico Excel + AI
├── Ellysse_AIAnalyst.html      ← Tool 3: chat AI su dati Excel
├── Ellysse_WarRoom_2026.html   ← Tool 4: dashboard strategica 2026
│
└── README.md                   ← questo file
```

---

## API Key — inserimento unico dalla Suite

La Suite gestisce la chiave OpenRouter centralmente:

```
[Campo API Key in alto a destra nella Suite]
         ↓ propagazione automatica
┌────────────────┬────────────────┬───────────────┐
│ Evolution      │ Smart Dashboard│ AI Analyst    │
│ (apiInp)       │ (S.apiKey)     │ (STATE.apiKey)│
└────────────────┴────────────────┴───────────────┘
```

- La chiave viene iniettata nel DOM di ogni iframe al momento del caricamento
- Persiste per tutta la sessione del browser (`sessionStorage`)
- War Room non usa AI, quindi non necessita di chiave

---

## Stack tecnico

| Componente | Tecnologia |
|---|---|
| UI | HTML5 + CSS custom (dark theme) |
| Parsing Excel | [SheetJS / xlsx 0.18.5](https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/) |
| Grafici | [Chart.js 4.4.0](https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/) |
| AI | Google Gemini 2.5 Flash Lite via [OpenRouter](https://openrouter.ai) |
| Font | Inter + JetBrains Mono (Google Fonts) |
| Hosting | File statici — funziona in locale o su GitHub Pages |

---

## Hosting su GitHub Pages

Per rendere la Suite accessibile online:

1. Vai su **Settings** della repo → **Pages**
2. Source: `Deploy from a branch` → branch `main` → cartella `/` (root)
3. Dopo qualche minuto sarà disponibile su:
   `https://lamb91.github.io/dashboard-commerciale-evoluta/Ellysse_Suite.html`

---

## Note di sicurezza

- I file Excel vengono letti **solo in memoria nel browser** — non vengono mai caricati su server
- La chiave OpenRouter viene inviata **esclusivamente ad OpenRouter** per le chiamate AI
- Nessun cookie di tracciamento, nessun analytics

---

*Ellysse Suite — sviluppato per il team commerciale Ellysse*
