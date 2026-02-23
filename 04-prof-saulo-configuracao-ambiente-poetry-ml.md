# Aula 01: Configuração de Ambiente com Poetry 🚀

Nesta aula, vamos construir a fundação do nosso trabalho. Vamos configurar um ambiente **profissional**, robusto e escalável. Vamos esquecer ferramentas ultrapassadas como `venv` e `requirements.txt`. O mercado de trabalho exige domínio de ferramentas modernas de gerenciamento e qualidade de código.

Nosso objetivo hoje é configurar do zero um ambiente Linux pronto para Computação Gráfica e Visão Computacional, garantindo reprodutibilidade e qualidade desde a primeira linha de código.

Nesse exemplo o foco não será o Jupyter Notebook, mas sim a configuração do ambiente de desenvolvimento

## O Que Vamos Usar?

1.  **Poetry**: O gerenciador de dependências e empacotamento do Python.
2.  **Ruff**: Ele garante que seu código siga padrões de estilo e qualidade instantaneamente.
3.  **Pydantic**: Biblioteca essencial para validação de dados. Indispensável em projetos.
4.  **Pytest**: O framework de testes padrão da indústria. Sem testes, não existe software confiável.
5.  **Pytest-cov**: Plugin do Pytest para medir a cobertura de testes do seu código.
6.  **Pre-commit**: Garante que nada "sujo" entre no seu repositório Git. Roda verificações automáticas antes de cada commit.
7.  **Bump2version**: Automatiza o versionamento do seu software (v0.1.0 -> v0.1.1).
8.  **Makefile**: Um arquivo mágico para criar atalhos para comandos complexos.
9.  **Jupyter**: Um ambiente interativo para desenvolvimento e experimentação.

---

## Passo 1: Criando Seu Primeiro Projeto

Vamos criar a estrutura do nosso projeto de aula. Não crie pastas manualmente! O Poetry faz isso da forma correta para nós.

1.  Navegue até:
    ```bash
    cd ~/projetos/visao
    ```

2.  Crie o projeto "aula-04":
    ```bash
    poetry new aula-04
    ```

3.  Entre na pasta:
    ```bash
    cd aula-04
    mkdir notebooks
    ```

    Se você listar os arquivos (`ls -R`), verá uma estrutura assim:
    ```text
    .
    ├── README.md
    ├── notebooks
    ├── pyproject.toml
    ├── src
    │   └── aula_04
    │       └── __init__.py
    └── tests
        └── __init__.py
    ```

---

## Passo 4: Instalando as Ferramentas (Dependências)

Agora vamos dar superpoderes ao nosso projeto. O Poetry separa dependências de **produção** (o que seu código precisa para rodar) das de **desenvolvimento** (o que você usa para programar e testar).

1.  **Dependências Principais**:
    Vamos instalar o `pydantic`.
    ```bash
    poetry add pydantic
    ```
    *Observe o terminal: o Poetry cria um ambiente virtual isolado automaticamente e instala tudo lá.*

2.  **Dependências de Desenvolvimento**:
    Ferramentas de qualidade de código não vão para o produto final, então usamos o grupo `--group dev`.
    ```bash
    poetry add --group dev pytest pytest-cov pre-commit ruff bump2version jupyter
    ```

---

## Passo 5: Configuração Profissional

Instalar não é configurar. Vamos criar os arquivos de configuração para que essas ferramentas trabalhem para nós.

### 5.1 Configurando o Ruff e Versão (pyproject.toml)

O arquivo `pyproject.toml` é onde tudo acontece. Vamos editá-lo para configurar nossas regras de qualidade.
Abra o arquivo `pyproject.toml` no seu editor (pode ser o VS Code).
No final do arquivo, adicione/edite as seções:

```toml

# --- CONFIGURAÇÕES NOVAS ABAIXO ---

[tool.ruff]
line-length = 88
target-version = "py310"

[tool.ruff.lint]
select = ["E", "F", "I"] # E: Errors, F: Pyflakes, I: Imports (isort)

[tool.pytest.ini_options]
addopts = "-ra -q"
testpaths = [
    "tests",
]
```

> **Importante**: Sempre que editar o `pyproject.toml` manualmente, é necessário atualizar o arquivo de trava das dependências. Execute:
> ```bash
> poetry lock
> ```

### 5.2 Configurando o pre-commit (.pre-commit-config.yaml)

Crie um arquivo chamado `.pre-commit-config.yaml` na raiz do projeto:

echo "" > .pre-commit-config.yaml


Abra o arquivo `.pre-commit-config.yaml` e adicione:

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [ --fix ]
      - id: ruff-format
```
Isso garante que, antes de qualquer commit, o Ruff vai verificar e formatar seu código automaticamente.

Para ativar, primeiro inicie o repositório git e depois instale o pre-commit no terminal:
```bash
git init
poetry run pre-commit install
```

### 5.3 O Poderoso Makefile

Para não termos que decorar comandos longos, vamos criar um `Makefile`. Crie um arquivo chamado `Makefile` (sem extensão) na raiz:

```bash
echo "" > Makefile
```
Adicione o seguinte conteúdo ao arquivo `Makefile`:

```makefile
.PHONY: install test lint format clean

install:
	@echo "Instalando dependencias com Poetry..."
	poetry install

test:
	@echo "Rodando testes..."
	poetry run pytest

lint:
	@echo "Verificando código com Ruff..."
	poetry run ruff check .

format:
	@echo "Formatando código..."
	poetry run ruff format .

clean:
	@echo "Limpando arquivos temporários..."
	rm -rf .pytest_cache
	rm -rf .ruff_cache
	find . -type d -name "__pycache__" -exec rm -rf {} +
```

---

## Passo 6: Testando Tudo

Agora seu ambiente está pronto. Vamos testar o fluxo de trabalho.

1.  **Verificar instalação**:
    ```bash
    make install
    ```

2.  **Criar um código simples**:

    ```bash
    echo "" > main.py
    ```


Edite o arquivo `aula-04/main.py` (crie se não existir):
```python
from pydantic import BaseModel

class Aluno(BaseModel):
    nome: str
    nota: float

def main():
    aluno = Aluno(nome="Maria", nota=9.5)
    print(f"Aluno: {aluno.nome}, Nota: {aluno.nota}")

if __name__ == "__main__":
    main()
```

3.  **Rodar o Lint/Format**:
    ```bash
    make format
    make lint
    ```
    Veja como o Ruff arruma seu código se tiver algo fora do padrão!

4.  **Rodar o código**:
    Lembre-se: o Python do seu sistema não conhece as bibliotecas, só o do Poetry.
    ```bash
    poetry run python main.py

    # ou simplesmente execute o makefile
    make run
    ```

Parabéns! Você tem um ambiente de Machine Learning profissional, configurado com as melhores práticas do mercado. Na próxima aula, começaremos a exploração de dados!
