Save the currect mortgage interest rate (swedbank). Use github action to schedule this daily. 

```sh
# project tree

project/
|-- .github
|   `-- workflows
|       `-- schedule.yml
|-- bin/webscrape.py
|-- env/requirements.txt
|-- swedbank.csv
`-- README.md
```

---

The `requirements.txt` contains all the libraries used for the project. 

```sh
# To generate requirements.txt

pip3 install pipreqs
python -m pipreqs.pipreqs .
```

The `webscrape.py` extract the current mortgage interest rate and saves it into `swedbank.csv`, that can be used for further analysis.

The `.github/workflows/schedule.yml` checks the homepage every work day (twice to be sure). 

>Github Actions has a built in function for schedule executable actions; CI/CD (Continous Integration and Continuous Deployment).

```sh
# generate the file

mkdir -p .github/workflows
touch .github/workflows/schedule.yml
```

Paste the the following into it:

```yml
name: check homepage

on:
  schedule:
    - cron: '0 15 * * 1-5' # mon - fri @ 5 pm (stockholm)

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-python@v5
      with:
        python-version: '3.12'
        cache: 'pip' # caching pip dependencies
    - run: pip install -r requirements.txt
    
    - name: check homepage
      run: |
        python webscrape.py
```

