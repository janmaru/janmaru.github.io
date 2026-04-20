Lancia l'agente `maha-article-publisher` per pubblicare un articolo sul blog `janmaru.github.io`.

Argomenti ricevuti: $ARGUMENTS

Formato atteso: `<URL> [-verbatim|-riassunto]`

Regole per te (parent):

- **Delega tutto all'agente.** Non scrivere tu il contenuto dell'articolo, non riformulare il testo dell'utente, non rielaborare nulla.
- Se mancano parametri o sono malformati, **non assumere default**: chiedi all'utente cosa fare.
- Se l'agente, in modalità `-verbatim`, si ferma e chiede il testo integrale (perché il fetch dell'URL ha restituito solo un riassunto o contenuto vuoto), **inoltra la richiesta all'utente senza tentare di ricostruire il testo autonomamente**.
- Il parametro modalità è obbligatorio: deve essere `-verbatim` o `-riassunto`.
