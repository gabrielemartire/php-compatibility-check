# 🐘 PHP Compatibility Check

[![PHP 5.4 Compatibility](https://img.shields.io/badge/PHP-Legacy-blue?logo=php)](https://www.php.net/releases/5_4_0.php)

> A GitHub Actions workflow designed for legacy PHP projects, which checks code compatibility with older PHP versions (e.g., 5.4). Useful for historical repositories or systems that must maintain compatibility with outdated environments.

## How It Works

The workflow:
1. Installs the **specified legacy PHP version** (e.g., 5.4).
2. Verifies that the version is installed correctly.
3. Retrieves `.php` files that have been added, modified, or copied compared to the `main` branch.
```bash 
$(git diff origin/main --name-only --diff-filter=ACM | grep '\.php$' || true)
```
4. Performs a **syntax check (`php -l`)** on each file.
5. If errors or incompatibilities are found, it **reporting the problematic files**.

## 🧩 Method 1 — Reusable Workflow

1. In your repository, create a file like:
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

🔧 Customization:

* `php-version` → with the PHP version you want to test (e.g. `6.2`, `7.1`, etc.)

## 🧠 Method 2 — Direct Use in Your Repository

If you prefer to use it directly in the same legacy PHP repo, just copy the file into the `.github/workflows` folder of your project `php-compatibility-check.yml`

by changing the `on:` block as follows:

```yaml
on:
  push:
    branches-ignore:
      - main
```

> [!NOTE]
> This will run the compatibility check on every push, excluding the main branch.

_and leave everything else unchanged._ If needed, modify the version to be tested by changing this variable at the beginning:

```yaml
env:
  PHP_VERSION: "5.4"
```

---

## ✅ Example Output


```text
From https://github.com/<ORG>/<REPO>
 * [new branch]      add-new-elements    -> origin/add-new-elements
Found PHP files to check:
main/users/new.php
No syntax errors in main/users/new.php
✅ No syntax errors found in any PHP files.
```

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


## 💬 Suggestions

Add to your README.md the badges
_in Action tabs_
<img width="303" height="181" alt="image" src="https://github.com/user-attachments/assets/614e61b5-d758-484a-95a5-89de1a26214f" />

<img width="220" height="30" alt="image" src="https://github.com/user-attachments/assets/b9ba542c-fdde-4ece-a67c-c360912a5347" />
<img width="226" height="28" alt="image" src="https://github.com/user-attachments/assets/7ba8598d-b9ca-4dd8-b12e-be3ba0fc0c9e" />

* Avoid spaces in `.php` file names.
* Even if the code works in modern versions, it may not be compatible with legacy PHP versions like 5.4.
* Perfect for historical repositories, outdated systems, or validation environments.
* The final workflow messages include automatic TIPS with possible causes of error.

## 🙏 Acknowledgments

Special thanks to [shivammathur](https://github.com/shivammathur/setup-php) for `/setup-php` project — one of the fundamental actions that makes it possible to manage legacy PHP versions on GitHub Actions. 💙

$$DON'T·PANIC$$
