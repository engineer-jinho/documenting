# Mkdocs

## Reference Site
- https://www.mkdocs.org/
- https://squidfunk.github.io/mkdocs-material/
- https://demun.github.io/mkdocs-tuts/



## Git Setup
----------------

- Git global setup
```
git config --global user.name "이름"
git config --global user.email "이메일"
```

- Git Clone
```
git clone https://gitlab.com/engineer-jinho/documenting.git
```

- Git Check
```
cd gitlab-and-github
cat .\.git\config
```

- Create ".gitignore", "README.md"
  - README.md
  ```
  ```
  - .gitignore
  ```
  *.log
  *.bak
  myvenv/
  wiki/site/
  ```

## Python Setup
----------------

- python venv
```
C:\Users\[유저이름]\AppData\Local\Programs\Python\[파이썬버전]\python.exe -m venv myvenv
.\myvenv\Scripts\activate
```

- Create "requirements.txt"
```
mkdocs-material==9.5.3
markdown_grid_tables==0.3.1
pandas==2.2.0
numpy==1.26.4
openpyxl==3.1.2
tabulate==0.9.0
```

- Install python python packageds
```
pip install -r requirements.txt
```

## Mkdocs 설정
----------------

- mkdocs 만들기
```
mkdocs new wiki
```

- mkdocs.yml 파일
```
site_name: Gitlab and Github Tutorial
site_url: ""
site_dir: ../public
use_directory_urls: false
nav:
  - Home: index.md
  - 초기설정: initialization.md

plugins: []

theme:
  name: material

markdown_extensions:
  - tables
  - markdown_grid_tables
```

- Mkdos build
```
mkdocs build -f .\wiki\mkdocs.yml
```

- Excel to Mkdocs
```
python3 .\wiki_table\convert_excel_to_markdown.py "이상치 vs 이상.xlsx"
```
- convert_excel_to_markdown.py
``` python
import pandas as pd
import numpy as np
import argparse   

parser = argparse.ArgumentParser(description='Excel to Markdown')
parser.add_argument('file_name', help='파일명')    
args = parser.parse_args()   

FILE_PATH = "wiki_table/" + args.file_name
try:
    df = pd.read_excel(FILE_PATH, engine = "openpyxl")
    df = df.apply(lambda x: x.str.replace('\n', '<br>') if x.dtype == 'object' else x)
    df = df.apply(lambda x: x.str.replace('-', '\-') if x.dtype == 'object' else x)
    md_table = df.to_markdown(index=False)

    f = open("wiki_table.txt", 'w',  encoding='utf-8')
    f.write(md_table)
    f.close()
except:
    print(args.file_name, " file does not exist")
```


## Initial commit
----------------
```
git switch --create main
git add .
git status
git commit -m "Initialization"
git push --set-upstream origin main
```


## Publishing
----------------

- Github
```
cd wiki
mkdocs gh-deploy --force
```

- Gitlab ".gitlab-ci.yml"

```
image: python:latest
before_script:
  - pip install -r requirements.txt

pages:
  stage: deploy
  script:
  - mkdocs build -f wiki/mkdocs.yml --strict --verbose
  artifacts:
    paths:
    - public
  rules:
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH

```
