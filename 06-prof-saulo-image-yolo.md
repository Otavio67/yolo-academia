# Aula 06: Manipulação de Imagens como Dados (PIL + OpenCV)

Bem-vindos a sexta aula! 📸

Hoje vamos desmistificar como os computadores "enxergam" o mundo. Para você, uma imagem é uma selfie bonita. Para o computador, é apenas uma **matriz gigante de números**.

Nesta aula, vamos aprender a manipular essas matrizes usando as duas bibliotecas mais importantes do ecossistema Python: **Pillow (PIL)** e **OpenCV**.

---

## 1. O que é uma Imagem Digital?

Antes de codar, precisamos entender a estrutura.
Uma imagem colorida padrão é composta por **Pixels**. Cada pixel é formado pela combinação de três cores primárias:
-   **R** (Red - Vermelho)
-   **G** (Green - Verde)
-   **B** (Blue - Azul)

Isso significa que uma imagem de 100x100 pixels é, na verdade, uma matriz (tabela) de dimensões `100 x 100 x 3`.

---

## 2. As Ferramentas

### 2.1 Pillow (PIL)
A **Pillow** é a biblioteca "amigável". Ela é excelente para operações básicas: abrir, cortar, redimensionar, converter formatos (JPG -> PNG) e colar imagens umas sobre as outras.
*   **Quando usar?** Edição simples de imagens, web development, scripts rápidos.

### 2.2 OpenCV
A **OpenCV** (Open Computer Vision Library) é a "ferramenta pesada". É o padrão da indústria para Visão Computacional em tempo real. Ela é feita em C++ (super rápida) e possui bindings para Python.
*   **Quando usar?** Detecção de objetos, reconhecimento facial, processamento de vídeo, filtros complexos, robótica.

---

## 3. Preparando o Ambiente

Antes de prosseguir com os exemplos, vamos criar e configurar um projeto Python usando o **Poetry**, instalar um kernel Jupyter e adicionar as dependências necessárias: `numpy`, `pillow` e `opencv-python`. Estas instruções são uma variação do que foi mostrado na **aula 01** (`aula_01_instalando_poetry_pandas_jupyter.md`) e funcionam em qualquer ambiente com shell Bash.

1. Se ainda não possui o Poetry instalado, siga os passos da aula 01 para instalá‑lo (`curl -sSL https://install.python-poetry.org | python3 -` e ajuste o `PATH`).

2. Acesse a pasta `visao`:
   ```bash
   cd ~/projetos/visao

   # agora geramos o pacote `aula-06` com o Poetry
   poetry new aula-06
   cd aula-06

   # dentro do projeto, crie também uma pasta para notebooks interativos
   mkdir -p notebooks
   ```

   *Essa pasta `notebooks/` é onde você guardará arquivos Jupyter (`.ipynb`). Você pode usar ela no VS Code ou Jupyter sem misturar com o código fonte.*

3. Instale as dependências principais (produção):
   ```bash
   poetry add numpy pillow opencv-python
   ```
   - `numpy` será útil para manipular matrizes/arrays de pixels
   - `pillow` é a biblioteca PIL já mencionada
   - `opencv-python` traz o OpenCV para Python

4. Para trabalhar com notebooks Jupyter, adicione o pacote de kernel como dependência de desenvolvimento:
   ```bash
   poetry add --dev ipykernel jupyter
   ```

5. Registre o kernel Jupyter do ambiente:
   ```bash
   poetry run python -m ipykernel install --user --name aula-06 --display-name "aula-06 (poetry)"
   ```

6. Para facilitar os exemplos práticos, crie um notebook chamado `teste-pil-cv.ipynb` dentro da pasta `notebooks/` que acabamos de criar. Esse arquivo servirá para combinar trechos de código com explicações durante o uso de Pillow e OpenCV.

7. Selecionar o kernel (Ctrl+Shift+P → Developer: Reload Window) `aula-06 (poetry)`.

---

## 4. Mão na Massa: Exemplos Básicos

Para treinar, faça o download do notebook 06-prof-saulo-image-yolo.ipynb para dentro da pasta `projetos/visao/aula-06/notebooks`.

---

## 5. Projeto 1: "Transforme sua Selfie" 🤳

Agora é sua vez de aplicar o que aprendeu!

**Objetivo**: Criar um script Python que processe uma foto sua (selfie).

**Requisitos**:
1.  Tire uma selfie com seu celular e envie para a pasta do projeto (nomeie como `minha_selfie.jpg`).
2.  Crie um script chamado `projeto_selfie.py`.
3.  O script deve usar **OpenCV** ou **Pillow** (sua escolha) para:
    *   Carregar a imagem.
    *   Aplicar uma **escala de 50%** (reduzir tamanho pela metade).
    *   Aplicar uma **rotação de 90 graus**.
    *   Converter para **escala de cinza**.
    *   Salvar o resultado final como `selfie_transformada.jpg`.

Boa sorte! 🚀
