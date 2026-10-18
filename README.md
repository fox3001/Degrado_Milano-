# Vizi Nascosti — Milano · Seicento

Web app di supporto a un gioco dal vivo a ruoli segreti (Nobile, Vizioso, Moralista, Cicisbeo) per un tour a piedi di 20–30 persone.

- **Online:** https://degrado-milano.web.app
- **Hosting e database:** Firebase, progetto `degrado-milano`

## File
| File | Cosa contiene |
|---|---|
| `index.html` | L'intera app: home con login, area della guida, schede del giocatore |
| `nob.png`, `viz.png`, `mor.png`, `cic.png` | Schede dei quattro ruoli |
| `404.html` | Pagina 404 di Firebase |
| `firebase.json`, `.firebaserc` | Configurazione per la pubblicazione su Firebase |
| `backup/` | Versioni precedenti di `index.html` (oggi pubblicate anche online, vedi ANALISI.md) |
| `ANALISI.md` | **Analisi completa: bug, problemi delle schede e del regolamento, ordine di lavoro** |

Copia fedele di quello che è pubblicato il 18/10/2026, verificata con l'hash SHA-256.

## Pubblicare
```
firebase deploy --only hosting
```
