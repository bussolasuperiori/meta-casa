# Metà · spese di casa

Web app installabile (PWA) per segnare le spese di casa di Luca e Claudia, divise sempre 50/50, con il saldo di chi deve a chi aggiornato in tempo reale su entrambi i telefoni.

- **Dati condivisi** tramite Firebase Firestore (piano gratuito), che funziona anche offline e sincronizza appena torna la rete.
- **Nessun login.** Si entra con un "codice casa" di 24 caratteri casuali, inserito una volta sola per telefono.
- **Hosting** su GitHub Pages.

---

## 1. Firebase (circa 10 minuti, si fa una volta sola)

1. Vai su <https://console.firebase.google.com>, clicca **Crea un progetto** e chiamalo per esempio `meta-casa`. Google Analytics non serve: puoi disattivarlo.
2. Nella pagina del progetto clicca l'icona **Web `</>`** per aggiungere un'app web. Dalle un nome (es. `Metà`) e **non** attivare Firebase Hosting.
3. Firebase ti mostra un blocco `const firebaseConfig = { ... }`: copialo.
4. Apri `index.html` e cerca la riga `firebase: null,` (è vicino all'inizio dello script, nella sezione **CONFIGURAZIONE**). Sostituiscila con l'oggetto che hai copiato:

   ```js
   firebase: {
     apiKey: "AIza...",
     authDomain: "meta-casa.firebaseapp.com",
     projectId: "meta-casa",
     storageBucket: "meta-casa.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   },
   ```
5. Nel menu a sinistra apri **Build → Firestore Database** e clicca **Crea database**. Scegli la località `europe-west8 (Milano)` o `eur3`, poi **modalità produzione**.
6. Apri la scheda **Regole**, sostituisci tutto con il contenuto del file `firestore.rules` e clicca **Pubblica**.

> L'`apiKey` di Firebase non è una password: è normale che stia nel codice pubblico. A proteggere i dati sono le regole del punto 6, che senza il codice casa non lasciano leggere nulla.

## 2. GitHub Pages

1. Su GitHub crea un repository nuovo, per esempio `meta-casa`. Può essere pubblico: le spese stanno su Firebase, non nel repository.
2. Carica tutti i file di questa cartella (anche la cartella `icons`) con **Add file → Upload files**. **Non** caricare `storico.json`.
3. Vai in **Settings → Pages**. In *Branch* scegli `main` e la cartella `/ (root)`, poi salva.
4. Dopo un minuto il sito è online su `https://<tuo-utente>.github.io/meta-casa/`.

## 3. Primo avvio sui telefoni

**Luca (Pixel, Chrome)**
1. Apri il link, scegli **Luca** e poi **Crea una nuova casa**.
2. Si aprono le Impostazioni: tocca **Condividi** (o **Copia link**) e manda il link a Claudia su WhatsApp.
3. Nel menu ⋮ di Chrome scegli **Installa app** (oppure **Aggiungi a schermata Home**).
4. Opzionale: in **Impostazioni → Importa storico** scegli `storico.json` per caricare i mesi da aprile a ottobre 2026.

**Claudia (iPhone, Safari)**
1. Apri il link da WhatsApp **in Safari**, scegli **Claudia** e tocca **Entra**.
2. Tocca **Condividi → Aggiungi alla schermata Home**.
3. Apri Metà dall'icona sulla Home. Al primo avvio da lì potrebbe richiedere il codice: lo trova nel messaggio WhatsApp e basta incollarlo. Succede perché iOS tiene separate le app installate e Safari.

## Come funzionano i conti

- Ogni spesa viene divisa a metà. Il **saldo attuale** è la somma di tutte le spese e di tutti i rimborsi, e i residui dei mesi precedenti vengono riportati in automatico: non servono più righe tipo "saldo agosto".
- **Segna rimborso** registra un trasferimento di soldi tra voi due, propone già l'importo che vi rende pari e accetta anche rimborsi parziali.
- I mesi importati dall'Excel (aprile–settembre) erano già chiusi, quindi compaiono nello storico e nei grafici ma non entrano nel saldo. Il saldo parte da ottobre 2026, che a fine mese era: Claudia deve a Luca 227,68 €.

## Aggiornare l'app

Dopo una modifica a `index.html`, nel file `sw.js` cambia `meta-v1` in `meta-v2` (e così via): così i telefoni scaricano la versione nuova invece di usare quella salvata.
