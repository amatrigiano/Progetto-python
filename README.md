# Smart Campus Space Finder

**Challenge 1 – Smart Campus | Python Innovation Hackathon**

Smart Campus Space Finder è un prototipo per aiutare gli studenti universitari a individuare rapidamente aule e spazi studio disponibili nel campus. Per ogni spazio mostra edificio, stato, posti disponibili, presenza di prese elettriche e orario fino al quale risulta libero.

Il progetto utilizza una CLI Python per gestire i dati e una dashboard statica HTML/CSS/JavaScript per consultarli. Non richiede server, framework backend o database.

## Funzionalità

- Gestione CLI di aule: inserimento, aggiornamento ed eliminazione.
- Filtri CLI per aule libere, edificio e prese elettriche.
- KPI aggregate: aule libere, posti disponibili ed edificio con più aule libere.
- Esportazione automatica e manuale in JSON e CSV.
- Dashboard web responsive con KPI, ricerca e filtri istantanei.
- Caricamento del JSON tramite `input type="file"` e `FileReader`, compatibile con apertura locale `file://`.

## Requisiti

- Python 3.8 o superiore (solo libreria standard).
- Browser moderno aggiornato (Chrome, Firefox, Edge o Safari).
- Nessuna dipendenza Python esterna e nessuna installazione npm.

## Avvio rapido

### 1. Clona la repository

```bash
git clone https://github.com/<username>/smart-campus-space-finder.git
cd smart-campus-space-finder
```

Sostituisci `<username>` con il nome utente GitHub del proprietario della repository.

### 2. Avvia la CLI

```bash
python main.py
```

Su alcuni sistemi il comando può essere `python3 main.py`.

Il menu consente di:
1. visualizzare tutte le aule;
2. inserire una nuova aula o aggiornare un'aula esistente;
3. eliminare un'aula;
4. filtrare le aule libere;
5. visualizzare i KPI;
6. esportare manualmente JSON e CSV;
7. salvare ed uscire.

I dati iniziali di esempio sono già inclusi in `data.json` e `data.csv`. Ogni inserimento, aggiornamento o eliminazione effettiva aggiorna automaticamente entrambi i file.

### 3. Apri la dashboard web

1. Apri `index.html` con un doppio clic oppure trascinalo in una finestra del browser.
2. Premi **Scegli JSON**.
3. Seleziona il file `data.json` presente nella cartella del progetto.
4. Esplora KPI, ricerca per nome/ID, filtro edificio e filtro per prese elettriche.

> **Nota sul caricamento locale:** il browser non legge automaticamente i file locali per ragioni di sicurezza. La dashboard non usa `fetch()` e non necessita di un server: l'utente seleziona esplicitamente `data.json` e il contenuto viene letto tramite FileReader API.

## Architettura

```text
          Gestione e aggiornamento
       +-----------------------------+
       |       Python CLI            |
       |          main.py            |
       +--------------+--------------+
                      |
             serializzazione
                      |
          +-----------+-----------+
          |                       |
     +----v-----+            +----v-----+
     | data.json|            | data.csv |
     +----+-----+            +----------+
          |
   selezione esplicita
   tramite input file
          |
     +----v-------------------------+
     | Dashboard statica            |
     | index.html + style.css       |
     | app.js (FileReader API)      |
     +------------------------------+
```

**Flusso obbligatorio:** `main.py` genera/aggiorna `data.json` e `data.csv`; il frontend legge `data.json` solo dopo la selezione manuale dell'utente. Non sono presenti backend, API HTTP, database o framework server.

### Modello dati

Ogni aula è rappresentata da un oggetto JSON con questi campi:

| Campo | Tipo | Descrizione |
|---|---|---|
| `id` | string | Identificativo univoco |
| `edificio` | string | Edificio o area del campus |
| `aula` | string | Nome o codice dello spazio |
| `stato` | string | `Libera`, `In Lezione`, `Occupata Studio` |
| `posti_disponibili` | integer > 0 | Numero di posti indicati |
| `prese_elettriche` | boolean | Presenza di prese elettriche |
| `libera_fino_a` | string | Orario `HH:MM` oppure `N/D` |
| `data_aggiornamento` | string | Timestamp dell'ultimo aggiornamento |

`posti_disponibili` deve essere un intero maggiore di zero; il KPI dei posti studio somma questo campo solo per le aule con stato `Libera`. Il dataset incluso è dimostrativo e non rappresenta occupazione in tempo reale.

## Struttura repository

```text
smart-campus-space-finder/
├── .gitignore
├── README.md
├── main.py
├── data.json
├── data.csv
├── index.html
├── style.css
└── app.js
```

## Team e ruoli – Hackathon

Adattare nomi e assegnazioni ai componenti effettivi del team.

| Ruolo | Responsabilità |
|---|---|
| Python / CLI Developer | Menu, validazione input, gestione record, export JSON/CSV |
| Frontend Developer | Dashboard, responsive UI, rendering e filtri client-side |
| Data & Product Analyst | Definizione campi, dataset mock, KPI e validazione del caso d'uso |
| UX/UI & Pitch | Esperienza studente, test usabilità, slide e presentazione |

In un team più piccolo, i ruoli possono essere accorpati.

## Limiti e possibili evoluzioni

- I dati sono aggiornati manualmente: non sono collegati a sensori, calendari o sistemi universitari.
- La dashboard mostra lo snapshot contenuto nel file selezionato; dopo un nuovo export CLI occorre ricaricare il JSON.
- In una versione successiva si potrebbero aggiungere importazione CSV, ordinamento, mappa del campus e sincronizzazione con fonti istituzionali, mantenendo chiaro il perimetro del prototipo offline.

## Licenza

Progetto realizzato a fini dimostrativi per il Python Innovation Hackathon. Aggiungere una licenza (ad esempio MIT) prima della pubblicazione se si desidera consentire formalmente il riutilizzo del codice.
