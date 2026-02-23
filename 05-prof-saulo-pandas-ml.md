# Manual Completo de Pandas para Machine Learning

---

## Configuração do ambiente (Poetry + pandas + Jupyter)

Antes de trabalhar com análise de dados e machine learning usando este material, crie um ambiente isolado com Poetry chamado `aula-05`. Siga estes passos:

1. abra um terminal e vá para a pasta de projetos:
   ```bash
   cd ~/projetos/visao
   ```
2. crie o projeto:
   ```bash
   poetry new aula-05
   ```
   A estrutura inicial conterá `pyproject.toml`, diretório `aula-05/` e `tests/`.
3. acesse o diretório:
   ```bash
   cd aula-05
   mkdir notebooks
   ```
4. adicione as dependências necessárias ao projeto em um único comando:
   ```bash
   poetry add pandas numpy matplotlib seaborn scikit-learn
   ```
   e, na sequência, registre um kernel Jupyter instalando o `ipykernel` como dependência de desenvolvimento:
   ```bash
   poetry add --dev ipykernel
   ```
5. registre o kernel com nome amigável:
   ```bash
   poetry run python -m ipykernel install --user --name aula-05 --display-name "aula-05 (poetry)"
   ```
6. recarregue (Ctrl+Shift+P → Developer: Reload Window) o VS Code ou selecione o kernel `aula-05 (poetry)` em seus notebooks. Após isso, todas as instruções deste manual poderão ser executadas com o kernel correto.

---

## Dataset Base

**[Titanic - Machine Learning from Disaster (Kaggle)](https://www.kaggle.com/c/titanic)**

O objetivo é usar _Machine Learning_ para criar um modelo que faça previsões sobre quais passageiros sobreviveram ao naufrágio do Titanic.

### Dicionário de Dados
Compreender os dados é o primeiro passo antes de qualquer análise. Segundo o Kaggle, temos as seguintes variáveis:

| Variável | Definição | Chave |
|---|---|---|
| `survival` | Sobrevivência (Target) | 0 = Não, 1 = Sim |
| `pclass` | Classe da passagem | 1 = 1ª, 2 = 2ª, 3 = 3ª (Proxy para Status Socioeconômico) |
| `sex` | Sexo | |
| `Age` | Idade em anos | Fracionada se < 1. Estimada em formato `xx.5` |
| `sibsp` | Nº de irmãos/cônjuges a bordo | Cônjuge, irmão, irmã, meio-irmão/irmã |
| `parch` | Nº de pais/filhos a bordo | Pai, mãe, filho, filha, enteado(a) |
| `ticket` | Número da passagem | |
| `fare` | Tarifa do passageiro | |
| `cabin` | Número da cabine | |
| `embarked` | Porto de embarque | C = Cherbourg, Q = Queenstown, S = Southampton |

---

# PARTE 1 — Fundamentos

## Importação

```python
import pandas as pd
import numpy as np
```

## Leitura de CSV

```python
df = pd.read_csv("train.csv")
```

## Exploração Inicial

```python
df.head()
df.tail()
df.shape
df.columns
df.info()
df.describe()
```

---

# PARTE 2 — Manipulação Essencial

## Seleção

```python
df["Age"]
df[["Age", "Fare"]]
```

## Filtros

```python
df[df["Age"] > 30]
df[(df["Age"] > 30) & (df["Sex"] == "female")]
```

## Criando Features

> [!NOTE]
> De acordo com o Dicionário de Dados, `SibSp` representa irmãos/cônjuges e `Parch` representa pais/filhos. Somar ambos (+1 para o próprio passageiro) nos dá o **Tamanho da Família**, uma nova *feature* (variável) muito mais útil para o modelo.

```python
df["FamilySize"] = df["SibSp"] + df["Parch"] + 1
```

## Ordenação

```python
df.sort_values("Fare", ascending=False)
```

## Valores Nulos

```python
df.isnull().sum()
df["Age"].fillna(df["Age"].median(), inplace=True)
df["Embarked"].fillna(df["Embarked"].mode()[0], inplace=True)
```

---

# PARTE 3 — GroupBy e Análise

```python
df.groupby("Sex")["Survived"].mean()
df.groupby(["Sex", "Pclass"])["Survived"].mean()

pd.pivot_table(df, values="Survived", index="Sex", columns="Pclass")
```

---

# PARTE 4 — Transformações

## Apply

```python
df["AgeGroup"] = df["Age"].apply(lambda x: "Child" if x < 12 else "Adult")
```

## Encoding

```python
df["Sex"] = df["Sex"].map({"male": 0, "female": 1})
df = pd.get_dummies(df, columns=["Embarked"], drop_first=True)
```

## Drop

> [!NOTE]
> Estamos descartando temporariamente o `Name`, `Ticket` e `Cabin` para simplificar nosso primeiro modelo. No entanto, o `Name` contém pronomes de tratamento (Mr., Mrs., Master) e o `Cabin` tem letras que indicam o convés do navio. Em projetos mais avançados, extraímos essas sub-informações em vez de apagá-las simplesmente.

```python
df.drop(["Name", "Ticket", "Cabin"], axis=1, inplace=True)
```

---

# PARTE 5 — Pipeline Profissional

```python
df = pd.read_csv("train.csv")

# Tratar Nulos
df["Age"].fillna(df["Age"].median(), inplace=True)
df["Embarked"].fillna(df["Embarked"].mode()[0], inplace=True)

# Feature Engineering
df["FamilySize"] = df["SibSp"] + df["Parch"] + 1
df["IsAlone"] = (df["FamilySize"] == 1).astype(int)

# Encoding
df["Sex"] = df["Sex"].map({"male": 0, "female": 1})
df = pd.get_dummies(df, columns=["Embarked"], drop_first=True)

# Features Descartáveis (por enquanto) e Separação X/y
cols_to_drop = ["Survived", "PassengerId", "Name", "Ticket", "Cabin"]
X = df.drop(columns=cols_to_drop, errors="ignore")
y = df["Survived"]
```

---

# PARTE 6 — Integração com Machine Learning

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

model.score(X_test, y_test)
```

---

# PARTE 7 — Técnicas Avançadas

## Query

```python
df.query("Age > 30 and Sex == 1")
```

## Assign

```python
df = (
    df
    .assign(FamilySize=lambda x: x.SibSp + x.Parch + 1)
)
```

## Value Counts Normalizado

```python
df["Survived"].value_counts(normalize=True)
```

## Category Type

```python
df["Pclass"] = df["Pclass"].astype("category")
```

## Uso de Memória

```python
df.memory_usage(deep=True)
```

---

# PARTE 8 — Submissão para o Kaggle

Para avaliar seu modelo oficialmente no Kaggle, temos que ler o arquivo `test.csv`, submetê-lo às **mesmas transformações do pipeline** e gerar o arquivo de submissão (ex: `submission.csv`). O objetivo final do Kaggle é prever a coluna "Survived" que *não* existe no arquivo de teste!

```python
# 1. Carregar os dados de teste
df_test = pd.read_csv("test.csv")
passageiros_id = df_test["PassengerId"] # Salvar para a submissão final

# 2. Aplicar Pipeline de Tratamento (Mesmas lógicas do treino)
df_test["Age"].fillna(df["Age"].median(), inplace=True)
df_test["Fare"].fillna(df["Fare"].median(), inplace=True) # Teste possui 1 Fare nulo
df_test["Embarked"].fillna(df["Embarked"].mode()[0], inplace=True)

df_test["FamilySize"] = df_test["SibSp"] + df_test["Parch"] + 1
df_test["IsAlone"] = (df_test["FamilySize"] == 1).astype(int)

df_test["Sex"] = df_test["Sex"].map({"male": 0, "female": 1})
df_test = pd.get_dummies(df_test, columns=["Embarked"], drop_first=True)

# 3. Remover Colunas Inúteis para o Modelo
X_pred = df_test.drop(columns=["PassengerId", "Name", "Ticket", "Cabin"], errors="ignore")

# 4. Realizar a Previsão no modelo treinado
previsoes = model.predict(X_pred)

# 5. Montar Arquivo de Submissão final
submission = pd.DataFrame({
    "PassengerId": passageiros_id,
    "Survived": previsoes
})
submission.to_csv("minha_primeira_submissao.csv", index=False)
```

> [!TIP]
> Agora basta ir ao Kaggle, procurar pelo botão **Submit Predictions** e enviar o arquivo `minha_primeira_submissao.csv` gerado na sua pasta. Veja sua posição no ranking!

---

# Roadmap de Evolução

- **Semana 1**: Manipulação básica e filtros
- **Semana 2**: Feature engineering e encoding
- **Semana 3**: Pipeline e otimização
- **Semana 4**: Novos datasets e desafios Kaggle

---

# Conclusão

Domine:

- Seleção avançada
- GroupBy complexo
- Feature engineering
- Encoding
- Pipeline profissional

Você estará pronto para aplicar Pandas em projetos reais de Machine Learning.
