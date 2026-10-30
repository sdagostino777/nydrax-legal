# Pagine legali di NYDRAX — come pubblicarle

Questa cartella contiene le pagine legali dell'app, in HTML statico: un solo `style.css`
accanto, nessuna risorsa esterna, nessuno script. Si aprono anche da `file://` e si possono
ospitare ovunque.

Servono perché **App Store Connect pretende un URL pubblico** per l'informativa sulla privacy
(obbligatorio in ogni localizzazione) e per l'URL di supporto. Le pagine dentro l'app non
bastano: Apple vuole poterle aprire dal browser.

---

## I file

Per **ognuna delle cinque lingue** (it, en, es, de, fr) ci sono cinque pagine. L'italiano non ha
suffisso; le altre lingue hanno il codice ISO. **I nomi sono in inglese e i file sono piatti**,
come in `Aequor/Store/legal/`: l'indirizzo che finisce nella scheda è uno solo e cambia il
suffisso.

| Lingua | Indice | Privacy | Termini | Avvertenza | Crediti |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Italiano | `index.html` | `privacy.html` | `terms.html` | `disclaimer.html` | `credits.html` |
| Inglese | `index.en.html` | `privacy.en.html` | `terms.en.html` | `disclaimer.en.html` | `credits.en.html` |
| Spagnolo | `index.es.html` | `privacy.es.html` | `terms.es.html` | `disclaimer.es.html` | `credits.es.html` |
| Tedesco | `index.de.html` | `privacy.de.html` | `terms.de.html` | `disclaimer.de.html` | `credits.de.html` |
| Francese | `index.fr.html` | `privacy.fr.html` | `terms.fr.html` | `disclaimer.fr.html` | `credits.fr.html` |

**`credits.html` è l'unica aggiunta rispetto alle altre app**, e ha un motivo: i suoni hanno una
provenienza (dodici da Pixabay, tre generati con ffmpeg) e la pagina dei crediti esiste già
dentro l'app. Se restasse solo lì, chi pubblica l'app non avrebbe un indirizzo da mostrare in
caso di contestazione sui diritti.

## Come si generano

**Non si scrivono a mano.** Le parole vengono dal catalogo dell'app, le stesse chiavi `legal.*`
che si leggono dentro le Impostazioni:

```bash
python3 Scripts/genera-pagine-legali.py
```

Così il testo pubblicato e quello dentro l'app sono lo stesso testo. Se cambia una frase
nell'app, si rigenerano le pagine — e il sito non può dire il contrario dell'app.

## Come si pubblicano

1. Repository GitHub dedicato, per esempio `nydrax-legal`.
2. Contenuto del repository: **tutto** il contenuto di questa cartella, `style.css` compreso,
   nella radice.
3. GitHub → Settings → Pages → *Deploy from a branch* → `main` / `/ (root)`.
4. Gli indirizzi diventano:

   ```
   https://sdagostino777.github.io/nydrax-legal/index.html                     ← indice (URL di supporto)
   https://sdagostino777.github.io/nydrax-legal/privacy.html         ← privacy (italiano)
   https://sdagostino777.github.io/nydrax-legal/privacy.en.html      ← privacy in inglese
   ```

   Sono gli stessi che il codice dell'app usa (`LegalLinks` in
   `Features/Settings/LegalViews.swift`) e che i testi della scheda riportano. **Non inventarne
   altri**: un indirizzo in due posti diversi è un link che si rompe in uno solo dei due.

## Verifica

```bash
# ogni lingua deve avere le sue cinque pagine
for l in "" .en .es .de .fr; do
  for p in index privacy terms disclaimer credits; do
    test -f "Store/legal/$p$l.html" || echo "MANCA $p$l.html"
  done
done
```

E `./Scripts/verify-all.sh` controlla che le pagine ci siano per tutte e cinque le lingue e che
il testo di ciascuna sia **davvero nella sua lingua** (non l'italiano copiato).
