# i18n-coverage

![.github/workflows/ci.yml](https://github.com/alexkiro/i18n-coverage/workflows/.github/workflows/ci.yml/badge.svg)

Github action that can be used to measure how many of you translatable strings 
have a translation available. Currently supported formats:

 - [gettext .po files](https://www.gnu.org/software/gettext/manual/html_node/PO-Files.html)

The action can also attach a neutral/success/failure check to the head commit that triggered 
the workflow, with the coverage results for each found language.
  
## Example usage

Note, that you must include a checkout step before using this action in order to have the 
translations available. (Or any other alternative method ot get the translation files) 

```yaml
on: [push]

jobs:
  i18n-coverage:
    runs-on: ubuntu-latest
    name: Check i18n coverage
    steps:
      - uses: actions/checkout@v2
      - name: Check i18n coverage
        uses: alexkiro/i18n-coverage@v1.0.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          translations-path: 'translations/**/*.po'
          ignore-languages: 'en'
```

---

The total coverage percentage can also be retrieved from the output. E.g:

```yaml
on: [push]

jobs:
  i18n-coverage:
    runs-on: ubuntu-latest
    name: Check i18n coverage
    steps:
      - uses: actions/checkout@v2
      - name: Check i18n coverage
        id: i18nCoverage
        uses: alexkiro/i18n-coverage@v1.0.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          translations-path: 'translations/**/*.po'
          ignore-languages: 'en'
      - name: Get the coverage
        run: echo "The coverage was ${{ steps.i18nCoverage.outputs.coverage }}"
```

## Check results

The check details will be added to the triggered commit and will be displayed in GitHub. E.g:

![Check summary](docs/imgs/check-summary.png)

Including the full details. E.g:

![Check details](docs/imgs/check-details.png)

## Inputs

### `translations-path`

The glob patterns to the translations files to check. Defaults to "translations/**/*.po". 

### `token`

Github secret token, usually set to `${{ secrets.GITHUB_TOKEN }}`. If not specified the check details
are not attached to the commit, and therefore can only be found and used in the action workflow.

### `min-coverage`

The minimum coverage required for this check to be marked as success. If not set or set to 0 the 
check is marked as neutral. Float values are supported.

 - See sample pr for success: https://github.com/alexkiro/i18n-coverage/pull/1 
 - See sample pr for failure: https://github.com/alexkiro/i18n-coverage/pull/2 

### `ignore-languages`

Comma separated list of languages codes; if specified these languages will be excluded from the check.

### `only-languages`

Comma separated list of languages codes; if specified only these languages will be checked instead of all. 

## Outputs

### `coverage`

Total percentage of translated strings, across all checked languages.
