# 🐘 PHP Compatibility Check

[![PHP 5.4 Compatibility](https://img.shields.io/badge/PHP-Legacy-blue?logo=php)](https://www.php.net/releases/5_4_0.php)

> Un workflow GitHub Actions pensato per progetti **PHP legacy**, che controlla la compatibilità del codice con versioni più vecchie di PHP (es. 5.4). Utile per repository storiche o sistemi che devono mantenere la compatibilità con ambienti datati.

---

## 🚀 Come funziona

Il workflow:

1. Installa **la versione di PHP legacy** specificata (es. 5.4).
2. Verifica che la versione sia installata correttamente.
3. Recupera i file `.php` aggiunti, modificati o copiati rispetto al branch `main`.
4. Esegue un controllo di **sintassi (`php -l`)** su ogni file.
5. Se trova errori o incompatibilità, **blocca il merge** segnalando i file problematici.

Include anche diversi **TIP utili** e commenti esplicativi nel codice, per aiutare chi lavora su progetti PHP legacy.

---

## 🧩 Modalità 1 — Reusable Workflow (consigliata)

1. Dalla tua repository, crea un file come:
   `.github/workflows/php-check.yml`

```yaml
name: Run PHP Compatibility Check

on:
  push:
    branches-ignore:
      - main

jobs:
  call-compatibility:
    uses: gabrielemartire/php-compatibility-check/.github/workflows/php-compatibility.yml@main
    with:
      php-version: '5.4'
```

🔧 Personalizzazione:

* `php-version` → con la versione di PHP che vuoi testare (es. `6.2`, `7.1`, ecc.)

---

## 🧠 Modalità 2 — Uso diretto nella propria repository

Se vuoi invece usarlo direttamente nella stessa repo PHP legacy, ti basta copiare il file in `.github/workflows` della tua cartella progetto `php-compatibility-check.yml`

cambiando il blocco `on:` in questo modo:

```yaml
on:
  push:
    branches-ignore:
      - main
```

> [!NOTE]
> questo avvira' il check compatibili ad ogni push, escluso su main

e lasciare tutto il resto invariato. Se serve modifica la versione da testare cambiando questa variabile all’inizio:

```yaml
env:
  PHP_VERSION: "5.4"
```

---

## ✅ Esempio di output

```text
From https://github.com/<ORG>/<REPO>
 * [new branch]      add-new-elements    -> origin/add-new-elements
Found PHP files to check:
main/users/new.php
No syntax errors in main/users/new.php
✅ No syntax errors found in any PHP files.
```

#### ERRORE
```bash 
From https://github.com/<ORG>/<REPO>
 * [new branch]      add-new-elements        -> origin/add-new-elements
Found PHP files to check:
main/users/edit.php
main/items/show.php
PHP Fatal error:  Can't use function return value in write context in main/items/show.php on line 218
Error: Process completed with exit code 255.
```

---

## 💬 Core

```bash 
$(git diff origin/main --name-only --diff-filter=ACM | grep '\.php$' || true)
```

---

## 💬 Suggerimenti

* Evita spazi nei nomi dei file `.php`.
* Anche se il codice funziona in versioni moderne, può non essere compatibile con versioni PHP legacy come la 5.4.
* Perfetto per repository storiche, sistemi datati o ambienti di validazione.
* I messaggi finali del workflow includono **TIP automatici** con possibili cause d’errore.

---

## Badge

<img width="220" height="30" alt="image" src="https://github.com/user-attachments/assets/b9ba542c-fdde-4ece-a67c-c360912a5347" />


<img width="226" height="28" alt="image" src="https://github.com/user-attachments/assets/7ba8598d-b9ca-4dd8-b12e-be3ba0fc0c9e" />

---

🙏 Ringraziamenti

Un grazie speciale al progetto shivammathur/setup-php — una delle azioni fondamentali che rendono possibile gestire versioni di PHP legacy su GitHub Actions. 💙

👷‍♂️ Made for developers who still use PHP legacy.
