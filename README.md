# Rendering-your-README-with-GitHub-Actions

The details of the codeset and plots are included in the attached Microsoft Word Document (.docx) file in this repository. 
You need to view the file in "Read Mode" to see the contents properly after downloading the same.

pkgdown.yaml
=============

on:
  push:
    branches: [main, master]
    tags: ['*']

name: pkgdown

jobs:
  pkgdown:
    runs-on: ubuntu-latest
    env:
      GITHUB_PAT: ${{ secrets.GITHUB_TOKEN }}
    steps:
      - uses: actions/checkout@v2

      - uses: r-lib/actions/setup-pandoc@v1

      - uses: r-lib/actions/setup-r@v1
        with:
          use-public-rspm: true

      - uses: r-lib/actions/setup-r-dependencies@v1
        with:
          extra-packages: pkgdown
          needs: website

      - name: Deploy package
        run: |
          git config --local user.name "$GITHUB_ACTOR"
          git config --local user.email "$GITHUB_ACTOR@users.noreply.github.com"
          Rscript -e 'pkgdown::deploy_to_branch(new_process = FALSE)'
