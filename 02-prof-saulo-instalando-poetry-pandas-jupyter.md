# Aula 01: Instalando Poetry, pandas e Jupyter 🧪

Este manual resume os passos mínimos que um aluno deve executar para criar um ambiente de desenvolvimento Python usando **Poetry**, instalar `pandas` e configurar um kernel Jupyter que possa ser usado em notebooks. As instruções abaixo são praticamente as mesmas que estão no arquivo `teste.txt`, mas aqui você encontra explicações e detalhes adicionais para cada comando.

## 0. Instalando o Poetry

Se você ainda não tem o Poetry no sistema, faça a instalação antes de prosseguir. Caso o comando `poetry` já funcione, pule diretamente para a seção 1.

1. Abra um terminal (bash ou zsh) e confirme o shell:
   ```bash
   echo $SHELL
   ```
   O resultado deve ser `/bin/bash` ou `/usr/bin/zsh`.

2. Execute o instalador oficial:
   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   ```

3. O instalador informará o diretório onde colocou o executável (geralmente `~/.local/bin`). Adicione esse diretório ao seu `PATH` editando o arquivo de configuração do shell (`~/.bashrc` ou `~/.zshrc`):
   ```bash
   nano ~/.bashrc   # ou ~/.zshrc se estiver usando zsh
   ```

   Ao final do arquivo, acrescente:
   ```bash
   export PATH="$HOME/.local/bin:$PATH"
   ```

   Salve (Ctrl+O, Enter) e saia (Ctrl+X).

4. Aplique as mudanças ao ambiente:
   ```bash
   source ~/.bashrc   # ou source ~/.zshrc
   ```

5. Verifique se a instalação deu certo:
   ```bash
   poetry --version
   ```
   Você deverá ver algo como `Poetry (version 1.8.x)`.

> A partir daqui assumimos que o comando `poetry` está disponível no terminal.

1. Abra um terminal (bash/zsh).
2. Primeiro criamos a pasta 'projetos' e um subdiretório 'visao'

   ```bash   
   mkdir -p ~/projetos/visao
   cd ~/projetos/visao
   ```

   **Atenção:** O nome do pacote deve ser em letras minúsculas e sem acentos.
   **Atenção:** Mantenha esse nome. Nossa estrutura de pastas para as próximas aulas será baseada nesse nome. 

3. Execute o comando abaixo; o Poetry criará a estrutura básica de um pacote Python:

   ```bash
   poetry new aula-02
   ```

   Isso gera:

   ```text
   aula-02/
   .
   ├── README.md
   ├── pyproject.toml
   ├── src
   │   └── aula_02
   │       └── __init__.py
   └── tests
      └── __init__.py
   ```

4. Entre no diretório do projeto:

   ```bash
   cd aula-02
   mkdir -p notebooks
   ```

## 2. Adicione o pandas ao ambiente

O `pandas` é uma biblioteca de manipulação de dados amplamente usada em ciência de dados e análise. Ela será uma dependência de produção do seu pacote.

```bash
poetry add pandas
```

Repare no output: o Poetry criará automaticamente um ambiente virtual isolado (em `.venv/` dentro do projeto ou em outro local conforme sua configuração) e instalará o `pandas` e suas dependências (`numpy`, `python-dateutil`, `six`, etc.).

## 3. Instale o `ipykernel` como dependência de desenvolvimento

Para que o seu ambiente apareça como um **kernel** no Jupyter (e, por consequência, no VS Code), é preciso instalar o pacote `ipykernel` e suas dependências. Elas não são pequenas — o kernel precisa de bibliotecas para comunicação, execução de código, depuração etc., por isso a saída lista várias instalações.

```bash
poetry add --dev ipykernel
```

O comando adiciona `ipykernel` ao grupo de desenvolvimento (`dev-dependencies`). Dependências de desenvolvimento não entram no pacote final; elas são usadas apenas durante o desenvolvimento e testes.

## 4. Registre o kernel Jupyter

Mesmo com `ipykernel` instalado, o Jupyter ainda não conhece o seu ambiente. A etapa a seguir cria um *spec* de kernel no diretório `~/.local/share/jupyter/kernels/` (ou equivalente) lembrando o caminho do Python.

```bash
poetry run python -m ipykernel install --user --name aula-02 --display-name "aula-02 (poetry)"
```

- `--user`: instala no perfil do usuário, sem necessidade de sudo.
- `--name`: identificador interno do kernel.
- `--display-name`: nome amigável que aparecerá na lista de kernels.

Você pode checar a existência com:

```bash
jupyter kernelspec list
```

Saída típica:

```
Available kernels:
  python3          /home/saulo/.local/share/jupyter/kernels/python3
  aula-02 (poetry)    /home/saulo/.local/share/jupyter/kernels/aula-02
```

## 5. Recarregue o VS Code / selecione o kernel

1. Crie um novo notebook na pasta `notebooks` 

```bash
cd ~/projetos/visao/aula-02/notebooks
echo "" > teste.ipynb
```

2. Execute um **Reload Window** (`Ctrl+Shift+P` → *Developer: Reload Window*).

Para escolher o kernel em um notebook:

1. Clique no nome (`Select Kernel`) do kernel no canto superior direito ou no cabeçalho e selecione **`aula-02 (poetry)`**.  
2. Como alternativa, abra o Command Palette e execute:
   - `Python: Select Interpreter` para apontar o VS Code para o Python do ambiente (`.venv/bin/python`); depois o kernel aparecerá na lista.
   - `Jupyter: Select Interpreter to Start Jupyter Server` também funciona.

## 6. Testando o notebook (`teste.ipynb`)

1. No topo do notebook, confirme que o kernel `aula-02 (poetry)` está ativo.
2. Insira uma célula com o código abaixo para testar se o pandas está funcionando:

   ```python
   # Importar a biblioteca pandas
   import pandas as pd

   print("Pandas versão:", pd.__version__)
   ```

4. Execute a célula (Shift+Enter). Se tudo estiver correto, a versão será exibida.

5. Você também pode verificar a versão do Python e do pandas:

   ```python
   import sys, pandas as pd
   print(sys.executable)
   print(pd.__version__)
   ```

   O caminho impresso deve apontar para a virtualenv controlada pelo Poetry.

6. Salve o notebook. Agora você tem um ambiente básico pronto para continuar as aulas.

---

### Observações finais

- O `ipykernel` puxa muitas dependências porque ele não faz só “rodar código”; ele cria um servidor de mensagens, lida com depuração (`debugpy`), integra com o Jupyter frontend (`jupyter-client`, `tornado`), e muito mais. É normal.
- Sempre que adicionar novas dependências no futuro, use `poetry add ...` (padrão) para dependências de produção e `poetry add --dev ...` para ferramentas de desenvolvimento.
- Se você voltar a abrir um notebook e o kernel não estiver na lista, repita o passo de recarregar o VS Code e/ou execute novamente o comando `poetry run python -m ipykernel install ...` caso o ambiente tenha sido recriado/atualizado.

Com esses passos, você montou do zero um ambiente `poetry+pandas+jupyter` e já testou a integração com um notebook. Pronto para seguir com a próxima aula! 🚀

### Resumindo todas as etapas

poetry new aula-02
cd aula-02
poetry add pandas
poetry add --dev ipykernel
poetry run python -m ipykernel install --user --name aula-02 --display-name "aula-02 (poetry)"
### Recarregar o VS Code/Janela
Ctrl+Shift+P → Developer: Reload Window 
