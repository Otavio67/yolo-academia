# Introdução ao Pandas - Guia para Iniciantes

Bem-vindo! Este notebook foi desenvolvido para ensinar os conceitos básicos de pandas, uma das bibliotecas mais importantes para análise de dados em Python.

---

## Configuração do ambiente (Poetry + pandas + Jupyter)

Antes de começar a trabalhar com os exemplos abaixo, é recomendável criar um ambiente isolado usando **Poetry** e instalar `pandas` e um kernel Jupyter. Nós fizemos isso na aula-02, mas recomendo que você repita esse processo para demais aulas (aula-03 até aula-09). Não reutilize a aula-02!

Siga os passos abaixo para montar o projeto `aula-03`:

1. Abra um terminal e navegue até a pasta onde guarda seus projetos:
   ```bash
   cd ~/projetos/visao
   ```

3. Crie o novo projeto com Poetry:
   ```bash
   poetry new aula-03
   ```
   Isso criará a estrutura básica (`pyproject.toml`, `intro_pandas/`, `tests/`).

4. Entre no diretório:
   ```bash
   cd aula-03
   mkdir -p notebooks
   ```

5. Adicione o `pandas` como dependência de produção:
   ```bash
   poetry add pandas
   ```

6. Instale o `ipykernel` como dependência de desenvolvimento para registrar o kernel Jupyter:
   ```bash
   poetry add --dev ipykernel
   ```

7. Registre o kernel com um nome amigável:
   ```bash
   poetry run python -m ipykernel install --user --name aula-03 --display-name "aula-03 (poetry)"
   ```

8. Recarregue o VS Code ou selecione o kernel `aula-03 (poetry)` em qualquer notebook.

9. Copie o arquivo `03-prof-saulo-intro-pandas.ipynb` para a pasta `aula-03/notebooks/`.

---