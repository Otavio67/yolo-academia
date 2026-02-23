# Aula 08: Por Dentro da Caixa (Bounding Boxes) 📦

Bem-vindos!

Na aula passada, tudo parecia mágica: passávamos uma imagem e o YOLO desenhava os quadrados. Mas em ciência de dados e engenharia de ML, não gostamos de "mágica". Gostamos de **matemática**.

Hoje vamos entender o que o YOLO realmente nos devolve e como usar esses números para criar nossas próprias visualizações.

---

## 1. O que é uma Bounding Box (Caixa Delimitadora)?

Quando o YOLO detecta um objeto, ele não retorna "uma imagem com um quadrado". Ele retorna **números** que descrevem onde esse quadrado está.

Existem dois formatos principais para representar esses quadrados:

### 1.1 Formato XYXY (Padrão do YOLOv8 quando usamos `boxes.xyxy`)
É o formato mais fácil de plotar:
*   **x1 (xmin)**: Coordenada X do canto **esquerdo superior**.
*   **y1 (ymin)**: Coordenada Y do canto **esquerdo superior**.
*   **x2 (xmax)**: Coordenada X do canto **direito inferior**.
*   **y2 (ymax)**: Coordenada Y do canto **direito inferior**.

### 1.2 Formato XYWH (Comum para cálculos de centro)
*   **x_centro**: O meio da caixa no eixo X.
*   **y_centro**: O meio da caixa no eixo Y.
*   **width (largura)**: Largura total da caixa.
*   **height (altura)**: Altura total da caixa.

---

## 2. Confiança e Classes

Além da posição, cada detecção traz:
*   **Confiança (conf)**: Um número entre 0.0 e 1.0 (ou 0% a 100%) que diz o "quanto" o modelo tem certeza. Geralmente filtramos tudo abaixo de 0.5 (50%).
*   **Classe (cls)**: Um número inteiro (ID) que representa o objeto (0=pessoa, 1=bicicleta, 2=carro...). O modelo tem uma lista interna de nomes (`names`) para traduzir isso.

---

## 3. Preparando o Ambiente

Vamos criar um novo ambiente para organizar os arquivos desta aula.

Execute no terminal:

```bash
# 1. Acessar pasta de projetos
cd ~/projetos/visao/

# 2. Criar o pacote da aula 08
poetry new aula-08
cd aula-08

# um lugar para guardar notebooks
mkdir -p notebooks
```

Instale as bibliotecas necessárias. Note que precisaremos do `requests` para baixar imagens:

```bash
poetry add ultralytics numpy opencv-python requests
poetry add --dev jupyter ipykernel
```

Registre o kernel Jupyter para que ele apareça no VS Code/Jupyter:

```bash
poetry run python -m ipykernel install --user --name aula-08 --display-name "aula-08 (poetry)"
```

Depois disso, abra o VS Code e selecione o kernel (Ctrl+Shift+P → Developer: Reload Window) `aula-08 (poetry)`.

> **Problemas de Conexão? (Dica de Ouro)**
> O download do PyTorch pode travar tentando puxar drivers NVIDIA gigantescos (+2 GB) se você não tiver GPU. Se isso ocorrer, prefira a versão só-CPU:
> ```bash
> poetry source add --priority=explicit pytorch_cpu https://download.pytorch.org/whl/cpu
> poetry add --source pytorch_cpu torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1
> poetry add ultralytics
> ```


---

## 4. Prática: Desenhando "Na Mão"

Toda a prática de desenhar as bounding boxes manualmente, analisar os resultados e a discussão sobre a importância dessa técnica foram movidas para um notebook interativo.

👉 **Execute o notebook:** `notebooks/08-prof-saulo-bbox-yolo.ipynb`

Para abrir, certifique-se de que o kernel `aula-08 (poetry)` esteja selecionado no VS Code ou Jupyter.
