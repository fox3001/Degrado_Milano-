# Vizi Nascosti — analisi completa e lista delle cose da sistemare

Documento di passaggio: contiene tutto quello che serve a un'altra persona o a un'altra AI per capire l'app e correggerla senza ripartire da zero.

- **App online:** https://degrado-milano.web.app (Firebase Hosting, progetto `degrado-milano`)
- **Versione precedente:** repo GitHub `fox3001/Vizi-Milano-`. Il file `vizi_v57_firebase_ready.html` è stato cancellato dal repo, ma resta recuperabile dalla cronologia (commit `7676fd1`). Usava un altro progetto Firebase (`vizi-milano`).
- **Analisi fatta il:** 18 ottobre 2026, leggendo il sorgente pubblicato sul sito. Ho provato il login con un codice esistente e guardato le quattro schede. Non ho generato codici né scritto sul database.

## ✅ Stato delle correzioni (aggiornato al 18/10/2026)

Le sezioni 2–5 descrivono la versione **originale**, che resta salvata in `archivio/index-originale.html`. Nella versione attuale (`public/index.html`):

| Bug | Stato | Come è stato risolto |
|---|---|---|
| 1. Database leggibile | ✅ nel codice · ⚠️ va pubblicato | `database.rules.json`: l'elenco si legge solo dopo il login della guida, il singolo codice è leggibile da tutti. Va inserita l'email della guida e poi lanciato `firebase deploy` (vedi README) |
| 2. Zoom rotto | ✅ | Lo zoom trasforma direttamente l'immagine e resta fermo sotto le dita. Doppio tocco per tornare normale. La scheda è a larghezza piena e si scorre in verticale |
| 3. Immagini pesanti | ✅ | Schede ritagliate dello spazio vuoto e salvate in JPG da 1080 px (210–260 KB invece di 5–6 MB). Spillo e damascato estratti dall'HTML in `public/img/`. `index.html` passa da 2,2 MB a circa 40 KB |
| 4. Codici eterni | ✅ | Ogni generazione crea una partita (`stato/partitaAttiva`, `partite/{id}`). Un codice vale solo se appartiene alla partita attiva. Ci sono i pulsanti "Chiudi la partita" ed "Elimina i codici delle partite chiuse" |
| 5. Area guida aperta | ✅ | Tolto il link dalla home. L'area si apre su `/#guida` e richiede il login Firebase (email e password). Eliminata la parola d'ordine nel codice |
| 6. CSS vagante | ✅ | Rimosso |
| 7. 30° giocatore | ✅ | Aggiunto "Il Priore Senza Pace". In più, se mancano personaggi, la lista viene riusata |
| 8. Personaggio e risorse | ✅ | Nuova pagina "Chi sei": personaggio, ruolo, risorse di partenza e testo completo del ruolo (compensa i testi tagliati nelle PNG). Il personaggio viene salvato su Firebase |
| 9. Codice morto | ✅ | CSS ripulito dai doppioni, eliminata la gestione delle vecchie tab |
| 10. Altro | ✅ | Salvataggio in un solo `update()` atomico. Messaggi d'errore in caso di rete assente (timeout di 10 s). La guida, ricaricando la pagina, ritrova i codici della partita attiva |
| 11. Backup online | ✅ nel codice · ⚠️ va pubblicato | `firebase.json` ora pubblica solo `public/`. Backup e doppioni sono spariti dal sito al primo `firebase deploy` |

**Nuovo design (19/10/2026):**
- l'app è stata ridisegnata a partire dal mockup approvato: barra in basso con 4 sezioni, colori dei ruoli presi dalle schede, testi più grandi e più contrastati, codice in 4 caselle;
- la **scheda del ruolo non è più un'immagine unica**: ora è fatta di sfondo damascato nel colore del ruolo, motto, riquadri di testo veri e illustrazioni dei personaggi (`public/img/<ruolo>-personaggi.jpg`). Questo risolve quasi tutta la sezione 6: testi tagliati, frecce mancanti, "plaetale" e "Dané" non esistono più nell'app;
- la pagina Regole riporta anche "Come si guadagnano Monete", presa dal regolamento PDF.

**Il regolamento PDF chiarisce parte della sezione 7.** Il file è `VIZI NASCOSTI — regolamento finale.pdf` (v8.0), sul PC in `D:\Documenti\Dramatrà\Gioco`.
- **Circoli di Tabacco:** 3–4 giocatori in cerchio, ognuno mette 1 Tabacco nel sacchetto e prende 1 Moneta.
- **Piacere Condiviso:** ci si scambia 1 Piacere a testa, lo si mette nel sacchetto e si prende 1 Moneta ciascuno.
- **Il sacchetto** è quello della guida, dove si depositano i token per ottenere Monete.
- **Neo di un'accusa sbagliata:** resta sull'accusato.
- **Guadagno dell'accusa:** si contano i nei "al momento dell'accusa".
- **Il Cicisbeo può convertire** come il Vizioso (ora è scritto nella sua scheda).

Restano aperti i punti 1 (al Vizioso conviene non convertire), 2 (la prima accusa vale 0), 5 (limite alla Dichiarazione di Virtù), 7 (valore del Tabacco a fine partita) e 8. Anche col Piacere Condiviso il punteggio non cambia: si perde 1 Piacere e si guadagna 1 Moneta.

**Regole nuove decise il 19/10/2026** (già scritte nell'app):
- **Gesto segreto:** ogni ruolo ha un gesto, pescato a caso a ogni partita dall'elenco gestito dalla guida, da fare dopo ogni scambio o guadagno di Monete. È vietato imitare quello degli altri ruoli.
- **Piacere infetto:** alcuni token Piacere sono segnati e non vanno mai nel sacchetto. Si rifilano agli altri con uno scambio o con il Piacere Condiviso, oppure si passano a un giocatore a scelta pagando 1 Tabacco. Al Castello valgono −2 ciascuno. Il numero di token infetti lo decide la guida (indicativamente 1 ogni 6–8 giocatori); l'app non li gestisce, perché il gioco resta fisico.

**Non risolti, perché servono decisioni o i file grafici originali:**
- sezione 6: con il nuovo design le PNG non si usano più. Resta da decidere quale versione tenere per l'accusa del Cicisbeo (verso chiunque o solo verso chi ha interagito con lui). Il PDF dice "solo chi ha interagito", come l'app;
- sezione 7, regolamento: sono decisioni di design, da prendere prima di toccare testi e schede.

Ho provato la nuova versione in un browser con schermo da telefono, simulando Firebase. Hanno funzionato:
- login giocatore e rifiuto dei codici vecchi;
- login guida e generazione di 30 codici (8 NOB, 8 VIZ, 7 MOR, 7 CIC, 30 personaggi diversi);
- pulizia dei codici vecchi;
- zoom a due dita e doppio tocco;
- scorrimento tra le quattro pagine;
- messaggio in caso di rete assente.

Non ho ancora provato con il Firebase vero.

---

## 1. Cos'è

Gioco sociale a ruoli segreti per un tour a piedi di 20–30 persone, ambientato nella Milano del Seicento. Si chiude al Castello Sforzesco.

L'app web serve solo a due cose:
1. la **guida** genera un codice segreto per ogni giocatore;
2. ogni **giocatore** inserisce il suo codice sul telefono e vede la propria scheda ruolo.

Il gioco vero e proprio si svolge dal vivo, con token fisici (Monete, Piacere, Tabacco) e adesivi neri (i "nei").

## 2. File pubblicati sul sito

| File | Dimensione | Note |
|---|---|---|
| `index.html` | 2,25 MB | Tutta l'app in un file. Contiene CSS, JS e immagini incorporate in base64 (sfondo damascato, personaggio "Spillo" nella home) |
| `nob.png` | 5,3 MB, 1792×4047 | Scheda del Nobile |
| `viz.png` | 6,0 MB, 1792×4263 | Scheda del Vizioso |
| `mor.png` | 6,2 MB, 1792×3812 | Scheda del Moralista |
| `cic.png` | 6,3 MB, 1792×3976 | Scheda del Cicisbeo |
| `404.html` | 1,8 KB | Pagina 404 standard di Firebase |
| `backup/…` | 2,2–4,5 MB | Vecchie versioni di `index.html`, **anche loro pubblicate** (vedi bug 11) |
| `img/…`, `public/img/…` | — | Copie identiche delle quattro PNG, pubblicate anche queste |

Ho verificato con l'hash SHA-256 che i file in questo repo sono identici byte per byte a quelli pubblicati sul sito. Sul PC il progetto sta in `D:\Documenti\Dramatrà\Gioco\file gioco finale\firebase`.

Non esistono manifest né favicon. Le cartelle `img/` e `public/img/` non sono nel repo, perché sono doppioni delle PNG e `index.html` non le usa. Le librerie esterne sono Google Fonts (IM Fell English, Cinzel, Cinzel Decorative) e Firebase JS SDK 10.12.2, caricato come modulo da gstatic.

## 3. Come funziona l'app

### Schermate (`.page`, una visibile alla volta)
1. **`#p-splash`, la home:** campo codice (`#ci`, `maxlength=6`), pulsante "Entra nella Corte" e link "Alcova Privata →" verso l'area della guida.
2. **`#p-guide`, Alcova della Guida:**
   - selettore del numero di giocatori (20–30);
   - "Genera codici e ruoli": mostra una griglia codice → sigla del ruolo (NOB/VIZ/MOR/CIC);
   - box "Prova un codice".
3. **`#p-player`, la scheda del giocatore:** tre pagine affiancate che si scorrono col dito (`#card-track`, larghezza 300%), con tre pallini in basso.
   - Pagina 0: immagine del ruolo (`#role-img`, PNG), con zoom a due dita (rotto, vedi bug 2).
   - Pagina 1: "Obiettivi & Guida", due blocchi di testo (`op1`, `op2`) generati da `ROLES`.
   - Pagina 2: "Regole Generali", testo fisso e pulsante "← HOME PAGE".

### Generazione dei codici (`#btnGen`)
- **Distribuzione dei ruoli:** `base = floor(n/4)` per ruolo. Il resto va nell'ordine a VIZ, NOB, CIC, quindi il Moralista è sempre il ruolo con meno giocatori.
- I ruoli vengono mescolati. A ogni giocatore viene assegnato anche un personaggio preso da `PERS` (29 voci), che però non viene mai mostrato.
- **Codici:** 4 caratteri dall'alfabeto `ABCDEFGHJKLMNPQRSTUVWXYZ23456789`, senza 0/O/1/I.
- I codici restano in memoria in `gameData` e vengono salvati su Firebase come `codici/{CODICE} = { ruolo: "NOB" }`, uno per volta.

### Login del giocatore (`entra()`)
1. Cerca prima su Firebase con `verificaCodiceOnline(codice)`. Se trova il codice, chiama `mostraSchedeRuolo(ruolo)` e apre la scheda.
2. Altrimenti, se il codice è `SPILLO RENDARD`, apre l'area della guida. Non funziona mai: vedi bug 5.
3. Altrimenti cerca in `gameData` locale, che funziona solo sul telefono che ha generato i codici.

### Firebase
- Configurazione pubblica nel secondo `<script type="module">` (apiKey `AIzaSyAjhTTnq…`, database `degrado-milano-default-rtdb.europe-west1`). Che l'apiKey sia visibile è normale per Firebase e non è un segreto. La protezione dipende dalle regole del database (vedi bug 1).
- Funzioni esposte su `window`: `salvaCodiciOnline(lista)` e `verificaCodiceOnline(codice)`.

## 4. Regole del gioco (come sono scritte oggi)

### Ruoli e risorse di partenza
Le risorse sono definite nel campo `dot` di `ROLES`, ma non vengono mai mostrate.

| Ruolo | Risorse iniziali | Capacità |
|---|---|---|
| **Nobile** | 2 Monete | *Dichiarazione di Virtù*: mostra la carta alla guida davanti a tutti, declama nome e grado e guadagna 1 Moneta. Partecipa ai *Circoli di Tabacco*. Può fare *Piacere Condiviso* con chiunque. Se dichiara Virtù con del Piacere in tasca e viene accusato, prende un neo. |
| **Vizioso** | 3 Piacere, 1 Tabacco | Da solo converte 2 Piacere → 1 Moneta, oppure 1 Piacere + 1 Tabacco → 1 Moneta. In coppia, con il *Piacere Condiviso*, guadagnano 1 Moneta a testa. Non può accusare. Conviene agire durante la narrazione della guida. |
| **Moralista** | 1 Moneta, 2 Tabacco | Può accusare al massimo 2 volte. Non è corruttibile: se qualcuno ci prova, lo accusa subito. Può fare *Piacere Condiviso* solo con il Cicisbeo. |
| **Cicisbeo** | 1 Moneta, 1 Piacere, 1 Tabacco | Non propone mai per primo. Può accusare al massimo 2 volte, ma secondo l'app solo chi ha interagito con lui. Se riceve 1 Tabacco prima di accusare qualcuno, deve rinunciare a quell'accusa. |

### Regole generali
- **Punteggio** = Monete + Piacere rimasti − Nei. Vince il **ruolo** con la media più alta.
- **Scambi:** liberi, a qualsiasi prezzo. Durante gli spostamenti serve almeno un testimone vicino. Durante la narrazione si può agire di nascosto, ma chi viene visto rivela qualcosa di sé.
- **Accuse:** può accusare solo il Moralista o il Cicisbeo.
  1. L'accusatore dichiara a voce alta persona e ruolo.
  2. La guida gli dà un adesivo nero.
  3. L'accusatore lo attacca sul viso dell'accusato.
  - Ruolo giusto: +2 Monete per ogni neo dell'accusato. Ruolo sbagliato: 0.
- **Neo:** resta per tutto il tour, vale −1 punto e non blocca nessuna azione.
- **Fine partita al Castello:** si mostrano le carte, si verificano le accuse, si calcola il punteggio e la guida legge le accuse non consegnate.

---

## 5. BUG E PROBLEMI DELL'APP (in ordine di priorità)

### 🔴 1. Database leggibile da chiunque: i ruoli sono pubblici
Chiunque apra
`https://degrado-milano-default-rtdb.europe-west1.firebasedatabase.app/codici.json`
vede **tutti i codici con il relativo ruolo**. Il 18/10/2026 erano 610. Basta un giocatore curioso per rovinare la partita. Probabilmente il database è anche scrivibile da chiunque: non l'ho provato per non toccare i dati.

**Correzione:** impedire la lettura dell'elenco, lasciare la lettura del singolo codice e riservare la scrittura alla guida autenticata. Regole d'esempio da incollare in *Firebase Console → Realtime Database → Regole*:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "codici": {
      "$codice": {
        ".read": true,
        ".write": "auth != null && auth.token.email == 'EMAIL_DELLA_GUIDA'"
      }
    }
  }
}
```

Poi, nell'area della guida, aggiungere il login Firebase Auth con email e password per la guida, così che solo lei possa generare codici. In alternativa si può usare una Cloud Function. Già solo togliendo la lettura dell'elenco si chiude il problema principale.

### 🔴 2. Zoom a due dita sulla scheda rotto
In `_initPinchZoom()`, le funzioni `apply()` e `reset()` usano `con.querySelector('.card-bg')`. Però `con` (`#pc-image`) **è lui stesso** l'elemento `.card-bg`, e non ha discendenti con quella classe. Il risultato è `null`, e al primo pizzico parte un `TypeError`, quindi lo zoom non funziona.

Il problema è grave perché la scheda è un PNG alto circa 4000 px, mostrato con `height:100dvh; object-fit:contain`. Su un telefono 375×812 diventa larga circa 360 px e il testo è illeggibile senza zoom.

**Correzione:** applicare `transform` direttamente a `img` (`#role-img`), come faceva la v57. Meglio ancora, mostrare l'immagine a larghezza piena con scorrimento verticale (`width:100%; height:auto`) invece di comprimerla a tutto schermo.

### 🟠 3. Immagini troppo pesanti
Le quattro schede pesano 5–6 MB l'una, cioè circa 24 MB in tutto, scaricati con la rete mobile. Inoltre circa un terzo di ogni immagine in basso è sfondo vuoto.

**Correzione:** ritagliare lo spazio vuoto, ridurre a circa 1080 px di larghezza ed esportare in WebP o JPEG qualità 80. L'obiettivo è stare sotto i 400 KB l'una.

### 🟠 4. I codici non scadono mai
Ogni generazione aggiunge codici nuovi senza cancellare i vecchi: oggi sono 610. I codici delle partite passate restano validi.

**Correzione:** salvare i codici sotto un ID partita (`partite/{id}/codici/{codice}`), con data di creazione. Aggiungere un pulsante "Chiudi partita", oppure considerare validi solo i codici della partita attiva.

### 🟠 5. Area della guida aperta a tutti, parola d'ordine impossibile da digitare
- Il link "Alcova Privata →" è visibile a chiunque. Apre la generazione dei codici e mostra il ruolo di ognuno.
- La parola d'ordine `SPILLO RENDARD` ha 14 caratteri, ma il campo `#ci` ha `maxlength="6"`, quindi non si può digitare. In più il controllo avviene solo dopo la ricerca su Firebase.

**Correzione:** togliere il link dalla home e proteggere l'area con Firebase Auth (vedi bug 1). Se si tiene una parola d'ordine, controllarla prima della ricerca su Firebase e togliere il limite di lunghezza. Va comunque ricordato che una parola d'ordine scritta nel JS è visibile a chiunque legga il sorgente.

### 🟡 6. Pezzo di CSS fuori dal tag `<style>`
Subito dopo il `<link>` dei font c'è questo blocco, scritto fuori da `<style>`:
```
img[src$=".png"] { width: 100% !important; height: auto !important; display: block !important; }
```
Il browser lo tratta come testo e lo mostra in cima al `<body>`, ed è probabilmente la striscia scura in alto. **Correzione:** cancellarlo. La stessa regola è già dentro `<style>`.

### 🟡 7. Con 30 giocatori l'ultimo resta senza personaggio
`PERS` ha 29 voci, mentre il massimo è 30. **Correzione:** aggiungere almeno un personaggio.

### 🟡 8. Informazioni utili che non vengono mostrate
- Il **personaggio** assegnato (nome e descrizione, es. "Don Ferrante il Malinconico") non compare da nessuna parte. In più non viene salvato su Firebase, dove c'è solo `ruolo`.
- Le **risorse di partenza** (`ROLES[x].dot`) non sono mai visibili.
- I testi `rp1` e `rp2` di ogni ruolo (descrizioni lunghe) non vengono usati.

**Correzione:** salvare su Firebase `{ruolo, pers}` e mostrare nella scheda il personaggio e le risorse iniziali.

### 🟡 9. Codice morto e duplicato
- La gestione delle tab (`.tbtn`, `.tcont`, `.tabnav`) risale a una versione vecchia: quegli elementi non esistono più.
- Le regole CSS di `#p-player`, `#card-track`, `.pcard` e `#role-img` sono ripetute due o tre volte, con valori diversi.
- La regola `img[src$=".png"]` con `!important` interferisce con lo stile di `#role-img`.

### 🟡 10. Altro
- `salvaCodiciOnline` scrive i codici uno alla volta, con un `await` dentro il ciclo. Funziona, ma un `update()` unico sarebbe più rapido e atomico.
- Se Firebase non risponde, l'utente non vede nessun messaggio: l'errore finisce solo in console.
- Se la guida ricarica la pagina, perde la griglia dei codici, perché non viene riletta da Firebase.
- `maximum-scale=1.0` nel viewport impedisce lo zoom del browser. Insieme al bug 2, sulla scheda non c'è modo di ingrandire.

### 🟠 11. Backup e doppioni pubblicati online
In `firebase.json` c'è `"public": "."`: Firebase pubblica **tutta la cartella del progetto**, tranne i file nascosti. Per questo sono raggiungibili online anche:
- `backup/index PRIMO FUNZIONA.html`, `backup/index no.html` e `backup/index 2.txt` (es. https://degrado-milano.web.app/backup/index%20no.html);
- `img/*.png` e `public/img/*.png`, doppioni delle schede, circa 24 MB in più a ogni deploy.

**Correzione:** spostare i file del sito in una sottocartella, es. `public/` con dentro `index.html`, `404.html` e le PNG, e impostare `"public": "public"` in `firebase.json`. In alternativa, aggiungere `"backup/**"`, `"img/**"` e `"public/**"` alla lista `ignore`. Poi ripubblicare: i file esclusi spariscono dal sito al deploy successivo.

---

## 6. PROBLEMI DELLE SCHEDE RUOLO (immagini PNG)

Sono da correggere nel file grafico originale, non nel codice.

| Scheda | Problema |
|---|---|
| **Nobile** | Testo tagliato: "Attenzione: se dichiari Virtù mentre hai Piacere in tasca e qualcuno ti…" si interrompe. Manca "…accusa, ricevi un neo." |
| **Nobile** | Refuso: "plaetale" invece di "plateale". |
| **Nobile** | Usa la valuta "Dané", mentre tutte le altre schede e l'app usano "Monete". Va scelto un solo nome. |
| **Vizioso** | Mancano le frecce: si legge "2 Piacere   1 Moneta" e "1 Piacere + 1 Tabacco   1 Moneta". Probabilmente il font non ha il carattere →. |
| **Vizioso** | Testo tagliato: "guadagnate entrambi 1 Moneta, ma siete più…". Manca "…visibili." |
| **Moralista** | Aggiunge una regola che non c'è altrove: "accusi quando si riparte". Va inserita nelle regole generali oppure tolta. |
| **Cicisbeo** | Titolo al plurale, "Cicisbei", mentre le altre schede usano il singolare. |
| **Cicisbeo** | Dice che può accusare chiunque ("perché sei il signore del gossip"), mentre l'app dice "solo chi ha interagito con te". Va scelta una versione. |
| **Tutte** | Nessuna indica le **risorse di partenza**. |
| **Tutte** | Circa un terzo dell'immagine in basso è sfondo vuoto. |

---

## 7. PROBLEMI DI REGOLAMENTO (decisioni di design)

Non sono bug di codice. Sono buchi o incoerenze nelle regole, che deve decidere chi ha ideato il gioco.

1. **Al Vizioso conviene non convertire.** Il punteggio è Monete + Piacere − Nei, quindi 1 Piacere vale già 1 punto:
   - 2 Piacere → 1 Moneta: perde 1 punto;
   - 1 Piacere + 1 Tabacco → 1 Moneta: punteggio invariato, perché il Tabacco vale 0.

   L'obiettivo dichiarato ("converti tutto il Piacere") va quindi contro il suo stesso interesse.
   *Possibili soluzioni:* il Piacere vale 0 a fine partita, oppure 1 Moneta vale 2 punti, oppure il Piacere non convertito vale −1.
2. **La prima accusa non rende nulla.** "+2 Monete per ogni neo che aveva l'accusato": all'inizio nessuno ha nei, quindi la prima accusa giusta vale 0 e nessuno ha motivo di accusare per primo.
   *Va chiarito* se conta anche il neo appena messo, oppure va dato un premio fisso (es. +2) più un bonus per ogni neo già presente.
3. **Il neo viene messo prima della verifica.** L'adesivo si attacca subito, ma le accuse si controllano solo al Castello. Va deciso se un innocente accusato a torto tiene il −1 e, se l'accusa era sbagliata, chi toglie il neo.
4. **Meccaniche citate ma mai spiegate:**
   - **Circoli di Tabacco:** come si fanno, quanto rendono, quanti giocatori servono?
   - **Piacere Condiviso:** costa Piacere? A chi? I due giocatori ottengono "1 Moneta ciascuno" dal nulla?
   - **"il sacchetto"**, nominato nel testo del Moralista: cos'è?
   - **Corruzione:** cosa significa "provare a corrompere" il Moralista?
5. **La Dichiarazione di Virtù non ha limiti.** Si può ripetere all'infinito per +1 Moneta ogni volta. Serve un limite, per esempio una volta per tappa o una volta in tutto il tour.
6. **Il Cicisbeo non ha modo di guadagnare.** Non converte, non dichiara e non partecipa ai Circoli. In più si riconosce facilmente, perché non fa mai la prima mossa. Così la regola del Tabacco anti-accusa diventa un'arma facile contro di lui.
7. **Il Tabacco vale 0 a fine partita.** Se è voluto, va scritto esplicitamente.
8. **Il Moralista ha sempre meno giocatori degli altri ruoli**, per come la distribuzione assegna i resti. Se è voluto, va bene; se no, va cambiato l'ordine dei resti.
9. **Regola sulla corruzione del Moralista ("lo accusi immediatamente"):** l'accusa automatica consuma uno dei suoi 2 slot? Vale anche se sbaglia il ruolo?

---

## 8. Ordine di lavoro consigliato

1. **Regole Firebase** (bug 1) e **backup pubblici** (bug 11): da fare subito, prima della prossima partita.
2. **Zoom e visualizzazione della scheda** (bug 2), e **compressione delle immagini** (bug 3).
3. **CSS vagante** (bug 6) e **pulizia del codice morto** (bug 9).
4. **Area della guida protetta** (bug 5) e **partite separate con codici che scadono** (bug 4).
5. **Mostrare personaggio e risorse** (bug 7 e 8).
6. **Rifare le PNG** con i testi corretti (sezione 6), dopo aver deciso le regole (sezione 7).

## 9. Note per chi prende in mano il lavoro
- L'app è un solo `index.html` statico, senza build: si modifica e si ripubblica con `firebase deploy --only hosting`, dalla cartella del progetto Firebase `degrado-milano`.
- Le immagini incorporate in base64 rendono `index.html` pesante, circa 2,2 MB. Conviene estrarle in file separati: `damask.png`, `spillo.png`.
- Il sorgente ora è nel repo GitHub `fox3001/Degrado_Milano-`. Le modifiche vanno fatte lì e poi ripubblicate. Le grafiche originali (PSD delle schede, circa 60 MB l'una) restano sul PC, in `D:\Documenti\Dramatrà\Gioco`.
- Nella cartella del PC ci sono anche un PDF con il regolamento (`VIZI NASCOSTI — regolamento finale.pdf`), versioni compresse delle schede (`schede/clear/ridotte/iloveimg-compressed/`, 360–530 KB l'una) e molte versioni vecchie dell'app. Questa analisi non li ha esaminati.
