# Adozione del collegamento seriale per renovate-config

**Stato: Active**

## Autorità e incarico

Missione approvata dal Product Owner: [Homelab #1265](https://github.com/skunklabs-uk/homelab/issues/1265), continuazione della #1252 per tutti i 32 repository. Questo incarico riguarda soltanto l'adozione documentale di `skunklabs-uk/renovate-config`; non modifica i preset o il comportamento di Renovate.

Leggi integralmente la RFC-0001 corrente fornita dal parent e `AGENTS.md`. La richiesta verificata dal parent contiene branch, head, assignment e generation: usa quella revisione, senza ricostruire i parametri da GitHub. Non avviare altri consumer o processi modello.

## Fonti da leggere nel checkout

- `README.md`.
- `default.json` e `automerge.json`, per capire cosa possiede il repository.
- `.github/workflows/validate-renovate.yml`, per distinguere il controllo producer dai test del collegamento.

Il runbook autorevole del collegamento è [WORKSPACE-HANDOFF.md](https://github.com/skunklabs-uk/developer-workspace/blob/main/docs/WORKSPACE-HANDOFF.md); il lifecycle runtime appartiene al [README Homelab](https://github.com/skunklabs-uk/homelab/blob/main/gitops/apps/developer-workspace/README.md). Il coordinatore ha verificato queste fonti sul producer `599dbc40d18892865443bfe9fb2606237b3c06c8` e Homelab `008506bc4e2853a96eff247221b77ef5b77be42c`. Non interrogare fonti esterne dalla sandbox.

## Modifica richiesta

Modifica soltanto `README.md`, aggiungendo una sezione breve in italiano per l'operatore del collegamento. Conserva le informazioni esistenti sui preset e sull'ownership. La sezione deve spiegare:

1. Un incarico richiede repository e thread ammessi, branch/head esatti e prompt corrente; un solo consumer seriale esegue il task in un checkout isolato.
2. Il report è distinto dalla pubblicazione. Una modifica richiede `publish_paths` con file esatti e una PR Draft nello stesso repository; il parent pubblica e il coordinatore rilegge SHA/diff e completa RETURN. Il child non esegue commit, push, merge o rollout.
3. La validazione dei preset appartiene al producer e usa il comando effettivamente presente nel workflow. Descrivi i trigger osservati senza inventare controlli PR. Non eseguire npx o installazioni durante questo incarico documentale.
4. Il repository produce configurazioni, quindi non richiede un'applicazione o una preview HTTP. Questo non elimina il controllo dei preset né il RETURN.
5. Rimanda ai due runbook proprietari per enrollment, selezione GitOps, recupero e stato persistente; non copiare un catalogo dei progetti, configurazioni operative o credenziali nel README.

Non dichiarare già completati la review finale, il merge o la CI di questo incarico. La prova di consegna verrà acquisita dal parent dopo il risultato; il coordinatore completerà il closeout e rimuoverà questo prompt.

## Confini e verifica

- Nessuna modifica a `default.json`, `automerge.json`, workflow, AGENTS o policy centrali.
- Nessuna rete dei comandi, installazione di tool, esecuzione di Renovate, credenziale, API esterna o filesystem fuori dal checkout.
- Nessun nuovo test per documentazione non consumata da codice.
- Verifica la sezione rispetto ai file letti e controlla che il diff riguardi soltanto `README.md`. Applica una review tecnica e una revisione della chiarezza del testo; usa `humanize-writing` solo se disponibile nel perimetro, senza installarla o fingere una review indipendente.

Restituisci in italiano: modifica effettuata, fonti e head esaminato, verifiche realmente eseguite, limiti e gate ancora spettanti al coordinatore. Non inventare output, URL di risultato o commit di pubblicazione.
