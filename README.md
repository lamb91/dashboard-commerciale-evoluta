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

---

## 🗺️ Roadmap — Evoluzioni possibili

Sezione dedicata al dev di supporto: idee concrete, ordinate per impatto e complessità stimata.

---

### 🟢 Quick wins (bassa complessità, alto impatto)

#### 1. Persistenza dati con IndexedDB
Attualmente i file Excel vanno ricaricati ad ogni apertura del browser. Con IndexedDB (API nativa, no backend) si può salvare in locale l'ultimo dataset caricato e ripristinarlo automaticamente.

```js
// Sketch implementativo
const db = await openDB('ellysse-suite', 1, {
  upgrade(db) { db.createObjectStore('periods'); }
});
await db.put('periods', S.periods, 'last');
```

**Dove intervenire:** `Ellysse_Evolution.html` — funzioni `loadOneFile()` e `buildComparison()`.

---

#### 2. Export PDF del report
Aggiungere un pulsante "📄 Esporta PDF" che genera un report stampabile con: banner insight, KPI strip, tabella tracciamento e grafici.

**Libreria consigliata:** [`html2canvas`](https://html2canvas.hertzen.com/) + [`jsPDF`](https://github.com/parallax/jsPDF) (entrambe disponibili su cdnjs).

```js
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';

async function exportPDF() {
  const canvas = await html2canvas(document.getElementById('dashArea'));
  const pdf = new jsPDF('l', 'mm', 'a4');
  pdf.addImage(canvas.toDataURL(), 'PNG', 0, 0, 297, 210);
  pdf.save(`Ellysse_Report_${new Date().toISOString().slice(0,10)}.pdf`);
}
```

---

#### 3. Alert soglie probabilità
Notifica visiva (e opzionalmente via email/webhook) quando un deal scende sotto una soglia configurabile (es. prob < 30% su deal > €50k).

**Dove intervenire:** `Ellysse_Evolution.html` — dopo `matchDeals()`, aggiungere una funzione `checkAlerts()` che confronta le soglie configurabili con i dati tracciati.

---

#### 4. URL shareable con stato compresso
Codificare lo stato corrente (periodi caricati, filtri attivi) in un parametro URL base64, così un link condiviso ripristina la stessa vista.

```js
const state = btoa(JSON.stringify({ periods: S.periods.map(p => p.label), filters: {...} }));
history.replaceState(null, '', `?s=${state}`);
```

---

### 🟡 Medio termine (complessità media)

#### 5. Backend leggero con Supabase (dati condivisi tra utenti)
Sostituire il caricamento manuale Excel con una tabella Supabase sincronizzata. Gli agenti vedono i propri deal in real-time, il manager vede tutto.

**Stack:** Supabase (PostgreSQL + auth + realtime) — piano free sufficiente per i volumi Ellysse.

**Schema minimo:**
```sql
CREATE TABLE deals (
  id TEXT PRIMARY KEY,
  nome TEXT, company TEXT, agente TEXT,
  probabilita FLOAT, importo FLOAT, arr FLOAT,
  famiglia TEXT, periodo DATE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Dove intervenire:** sostituire `loadOneFile()` con una chiamata `supabase.from('deals').select()` con filtro per periodo.

---

#### 6. Connessione diretta CRM (HubSpot / Pipedrive / Salesforce)
Eliminare il passaggio Excel aggiungendo un'autenticazione OAuth e chiamate alle API del CRM. I dati si aggiornano in tempo reale senza caricare file.

**Librerie:** fetch nativo + OAuth2 PKCE flow (nessuna libreria necessaria).

**Dove intervenire:** creare un nuovo modulo `crm-connector.js` con classe astratta `CRMAdapter` e implementazioni specifiche per ciascun CRM. La Suite espone un pulsante "Connetti CRM" nel tab Evolution.

---

#### 7. Report AI automatico settimanale (via webhook)
Aggiungere un endpoint serverless (es. Cloudflare Workers o Vercel Edge Function) che:
1. Legge i dati dal CRM o da un bucket S3
2. Chiama Gemini con il prompt già usato in Evolution
3. Invia il report via email (Resend) o su Slack (Webhook)

**Cron:** ogni lunedì mattina alle 8:00.

---

#### 8. Autenticazione e profili agente
Aggiungere un login semplice (Supabase Auth o Google OAuth) per mostrare automaticamente all'agente solo i propri deal, nascondendo quelli degli altri.

**Logica:** dopo il login, filtrare `S.tracked` con `d.agent === currentUser.name` come default, con override per il manager.

---

### 🔴 Lungo termine (alta complessità, alto valore)

#### 9. Deal Scoring con ML
Addestrare un modello leggero (es. regressione logistica o XGBoost) sui dati storici per predire la probabilità di chiusura effettiva, indipendentemente dalla stima del commerciale.

**Stack:** Python + scikit-learn per il training offline → esportare il modello come ONNX → caricare in browser con `onnxruntime-web`.

**Output:** colonna aggiuntiva "Score AI" nella tabella tracciamento, con confronto vs probabilità manuale.

---

#### 10. Mobile app (PWA)
Trasformare la Suite in una Progressive Web App installabile su smartphone. Gli agenti possono consultare la pipeline in mobilità.

**Modifiche necessarie:**
- Aggiungere `manifest.json` e service worker
- Riscrivere i layout con `min-width` mobile-first
- Usare `IntersectionObserver` per lazy-load dei grafici

**manifest.json:**
```json
{
  "name": "Ellysse Suite",
  "short_name": "Ellysse",
  "start_url": "/Ellysse_Suite.html",
  "display": "standalone",
  "background_color": "#06090f",
  "theme_color": "#9b7ef8",
  "icons": [{ "src": "icon-192.png", "sizes": "192x192" }]
}
```

---

#### 11. Forecasting AI della pipeline
Vista predittiva che, sulla base dei trend storici (N mesi), proietta la pipeline futura con intervalli di confidenza. Utile per la pianificazione trimestrale.

**Approccio:** regressione lineare sui valori storici per periodo, implementabile in puro JS senza dipendenze esterne.

---

### 📌 Note architetturali per il dev

- Tutti i tool sono **single-file HTML** — CSS, JS e HTML in un unico file. Facile da distribuire, ma per evoluzioni significative conviene separare in moduli ES6 (`import/export`) e usare un bundler leggero (Vite).
- La comunicazione Suite ↔ iframe avviene tramite **accesso diretto al DOM** (`iframe.contentWindow`). Se si passa a HTTPS (GitHub Pages o altro), questo continua a funzionare purché tutti i file siano sullo stesso origin. Con domini diversi servirà `postMessage`.
- Il modello AI (`gemini-2.5-flash-lite`) è facilmente sostituibile: basta cambiare il parametro `model` nelle chiamate fetch a OpenRouter. Si può anche esporre un selector nel UI per scegliere il modello.
- Per aggiungere un **nuovo tool** alla Suite: creare il file HTML, aggiungere una entry in `TOOLS` in `Ellysse_Suite.html`, aggiungere il tab e il pane corrispondente, e implementare `injectKey()` per il nuovo tool se usa l'AI.

---

*Ellysse Suite — sviluppato per il team commerciale Ellysse*
