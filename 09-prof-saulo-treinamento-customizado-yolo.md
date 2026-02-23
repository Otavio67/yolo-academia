# Aula 09: Treinando seu Próprio Detector (Custom Training) 🏋️‍♂️

Bem-vindos à aula 09!

Até agora usamos o modelo `yolov8n.pt`, que é "inteligente" para 80 coisas (pessoas, carros, gravatas...). Mas e se quisermos detectar algo que ele **não conhece**? Por exemplo: defeitos em peças mecânicas, logotipos de marcas ou tipos específicos de flores?

Hoje vamos aprender a ensinar coisas novas para o computador.

---

## 1. Preparando o Ambiente

Vamos criar um novo ambiente para organizar os arquivos desta aula.

Execute no terminal:

```bash
# 1. Acessar pasta de projetos
cd ~/projetos/visao/

# 2. Criar o pacote da aula 09
poetry new aula-09
cd aula-09

# um lugar para guardar notebooks
mkdir -p notebooks
```

Instale as bibliotecas necessárias. Note que precisaremos do `requests` para baixar imagens:

```bash
poetry add ultralytics numpy opencv-python requests
poetry add --dev jupyter ipykernel
```

Registre o kernel Jupyter para que ele apareça no VS Code/Jupyter:

```bash
poetry run python -m ipykernel install --user --name aula-09 --display-name "aula-09 (poetry)"
```

Depois disso, abra o VS Code e selecione o kernel (Ctrl+Shift+P → Developer: Reload Window) `aula-09 (poetry)`.

> **Problemas de Conexão? (Dica de Ouro)**
> O download do PyTorch pode travar tentando puxar drivers NVIDIA gigantescos (+2 GB) se você não tiver GPU. Se isso ocorrer, prefira a versão só-CPU:
> ```bash
> poetry source add --priority=explicit pytorch_cpu https://download.pytorch.org/whl/cpu
> poetry add --source pytorch_cpu torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1
> poetry add ultralytics
> ```

---

## 2. Entendendo as Pastas de um Dataset (Teoria) 📂

Ao preparar dados para Inteligência Artificial, o padrão ouro é dividir suas imagens em 3 pastas. Imagine que estamos preparando um aluno para o vestibular:

### 1. `train` (Treino) 📚
*   **O que é:** O livro didático e os exercícios de casa.
*   **Função:** É aqui que o modelo passa 99% do tempo. Ele olha a imagem ("pergunta") e o gabarito ("resposta"). Se errar, ajusta seus neurônios.
*   **Quantidade:** ~70% das imagens.

### 2. `valid` (Validação) 📝
*   **O que é:** O "simulado" semanal.
*   **Função:** O modelo **nunca** aprende com essas imagens de fato. Ele apenas é **avaliado** por elas durante o treino. Serve para sabermos se ele está aprendendo de verdade ou apenas "decorando" as fotos de treino (Overfitting).
*   **Quantidade:** ~20% das imagens.

### 3. `test` (Teste) 🎓
*   **O que é:** A prova final / Vestibular.
*   **Função:** Imagens que ficaram "trancadas no cofre" durante todo o processo. O modelo nunca as viu. Usamos apenas **uma vez** no final de tudo para medir a nota real do sistema no mundo real.
*   **Quantidade:** ~10% das imagens.

O arquivo `data.yaml` que vem na raiz desse formato diz ao YOLO exatamente onde estão cada uma dessas pastas e quais são os nomes das classes que ele precisa prever.

---

## 3. A Estrutura de Pastas e Labels (Prática)

O YOLO requer que seus dados estejam organizados exatamente dessa forma. Se você errar uma pasta, ele não funciona.

Para entender a estrutura, vamos criar as pastas vazias apenas como **exemplo didático**. Você pode executar o seguinte comando no terminal (garanta que está dentro da pasta `aula-09`):

```bash
mkdir -p meu_dataset/train/images meu_dataset/train/labels meu_dataset/valid/images meu_dataset/valid/labels
touch meu_dataset/data.yaml
```

A estrutura padrão criada será:

```
meu_dataset/
├── data.yaml       <-- O "mapa" do dataset configurando as classes
├── train/
│   ├── images/     <-- Fotos de treino
│   └── labels/     <-- Arquivos .txt com as anotações
└── valid/
    ├── images/     <-- Fotos de validação (prova)
    └── labels/     <-- Arquivos .txt com as anotações
```

> ⚠️ **ATENÇÃO - NÃO TREINE NESTA PASTA:** Estas pastas foram criadas VAZIAS apenas para você visualizar a estrutura que o YOLO exige. Se você tentar treinar passando esse `meu_dataset/data.yaml` vazio, **vai dar erro**, pois não há imagens para a rede neural! ⚠️

### O Arquivo de Anotação (.txt)
Para cada imagem (ex: `foto1.jpg`), deve haver um arquivo de texto com o mesmo nome (ex: `foto1.txt`) na pasta `labels`.

Dentro do `.txt`, cada linha é um objeto:
```
0 0.50 0.50 0.20 0.40
1 0.10 0.60 0.10 0.10
```

O formato é: `class_id x_center y_center width height` (valores normalizados de 0.0 a 1.0).

---

## 4. Obtendo um Dataset Real (Obrigatório) 🛑

Como vimos acima, precisamos de dados reais para treinar a IA. Felizmente, existem plataformas com datasets prontos. Nos passos abaixo, você irá baixar um repositório já organizado com fotos, labels e um `data.yaml` populado.

Aqui estão 2 sugestões excelentes e gratuitas do **Roboflow Universe** (já no formato YOLOv8):

1.  **Vida Marinha (Aquário) 🐠**
    *   **Descrição:** 600+ imagens de peixes, tubarões e pinguins.
    *   **Link:** [Aquarium Dataset](https://universe.roboflow.com/roboflow-lab/aquarium-combined)
2.  **Segurança do Trabalho (EPI/PPE) 👷**
    *   **Link:** [PPE / Hard Hat Dataset](https://universe.roboflow.com/roboflow-universe-projects/construction-site-safety)

### Como baixar do Roboflow:
1.  Acesse um dos links acima e clique em **"Download Dataset"** (canto superior direito).
2.  Na tela de exportação, selecione o formato **YOLOv8** (não escolha o OBB!). *(Se YOLOv8 não aparecer, escolha YOLOv5 PyTorch).*
3.  Marque a opção **"Download zip to computer"** e baixe.
4.  Crie uma pasta chamada `dataset_aquarium` (ou `dataset_capacete`) dentro da pasta principal de projetos (`~/projetos/visao/`), e **extraia os arquivos do zip nela**.

Ao terminar a extração, certifique-se de que se entrar na pasta extraída, você verá o arquivo `data.yaml` e as pastas `train` e `valid` repletas de imagens.

---

## 5. O Treinamento no Jupyter Notebook 🔥

Agora que você tem um dataset com imagens reais, vamos treinar! Mantenha o arquivo `09-prof-saulo-treinamento-customizado-yolo.ipynb` acompanhando o seu dataset.

Abra este notebook e siga os passos interativos:

1.  **Aponte para o Dataset Correto:** Na célula que tem `dataset_yaml = 'meu_dataset/data.yaml'`, mude o caminho para a pasta real que você acabou de extrair!
    *   Exemplo: `dataset_yaml = 'dataset_aquarium/data.yaml'`
2.  **Execute a verificação:** Ele deve imprimir que o arquivo foi encontrado e está acessível.
3.  **Inicie o Treino:** Execute a célula `model.train(data=dataset_yaml, ...)`.

Diferente de antes, agora você verá o download se inicializar (`yolov8n.pt` ou similar) e, logo após as checagens normais de AMP, uma bela barra de progresso do treinamento de _Época em Época_ (Epochs). Se os dados estiverem lá, o treinamento prossegue lindamente sem dar erros e encerrar sumariamente!

---

## 6. Analisando os Resultados 📊

Depois do treino, as células finais do notebook vão ajudá-lo a exibir os resultados gerados.

Você verá arquivos como:
*   **results.png**: Gráficos mostrando que o erro (loss) caiu a cada época de estudo.
*   **val_batch0_pred.jpg**: Uma colagem de fotos do conjunto de _validação_, que o próprio modelo tentou prever desenhando as `bounding boxes`.

Se conseguir ver as predições coloridas nas fotos, parabéns! Você virou oficialmente um "Treinador de IA". 🧢🔴⚪

---

## 7. Treinamento via Terminal (Alternativa)

Se você não quiser abrir todo o ambiente do VS Code com Jupyter Notebook, mas quiser treinar sua rede num servidor ou PC "direto ao ponto", pode usar a CLI incrível do pacote `ultralytics`.

Abra o terminal, vá para a pasta onde você baixou o dataset (ex: `~/projetos/visao`) e rode:

```bash
# Lembre-se de apontar para o dataset real extraído!!
poetry run yolo detect train data=dataset_aquarium/data.yaml model=yolov8n.pt epochs=5 imgsz=640
```

Se tudo der certo, todas aquelas barras de progresso que você viu no Jupyter vão ser impressas direto no seu terminal em um fluxo de log.
