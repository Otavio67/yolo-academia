# Aula 07: Introdução à Detecção de Objetos com YOLO

Bem-vindos à sétima aula! 🕵️‍♀️
    
Até agora, nós manipulamos imagens como matrizes (cortamos, giramos, mudamos cores). Mas o computador ainda não "entendia" o que estava na imagem. Ele via números, não objetos.

Hoje isso muda. Vamos entrar no mundo da **Inteligência Artificial**. Utilizaremos o estado da arte em detecção de objetos: o **YOLO (You Only Look Once)**.

---

## 1. O Mapa do Machine Learning 🗺️

Para entender onde estamos pisando, precisamos olhar para o mapa completo da Inteligência Artificial. Nem tudo é igual!

O Machine Learning (Aprendizado de Máquina) se divide em grandes famílias:

```text
Machine_Learning/
├── Aprendizado_Supervisionado/    <-- Onde nós estamos! 📍
│   ├── Regressao/
│   │   ├── Regressao_Linear/
│   │   └── Ridge_Lasso/
│   └── Classificacao/
│       ├── Regressao_Logistica/
│       ├── SVM/
│       ├── KNN/
│       ├── Arvores_de_Decisao/
│       └── Naive_Bayes/
│
├── Aprendizado_Nao_Supervisionado/
│   ├── Clustering/
│   │   ├── K_Means/
│   │   ├── DBSCAN/
│   │   └── Clustering_Hierarquico/
│   ├── Reducao_de_Dimensionalidade/
│   │   ├── PCA/
│   │   └── t_SNE/
│   └── Regras_de_Associacao/
│
├── Aprendizado_Semi_Supervisionado/
│
├── Aprendizado_por_Reforco/
│   ├── Baseado_em_Modelo/
│   └── Model_Free/
│       ├── Q_Learning/
│       └── Deep_Q_Learning/
│
└── Deep_Learning/                 <-- A Casa do YOLO 🏠
    └── Redes_Neurais/
        ├── ANN/
        ├── CNN/                 <-- Endereço exato do YOLO
        ├── RNN/
        ├── LSTM/
        └── Transformers/
```

### Onde o YOLO se Enquadra?
O YOLO é um algoritmo de **Deep Learning** (Aprendizado Profundo) que usa **CNNs (Redes Neurais Convolucionais)**.
Mas atenção para a pegadinha:
*   Sua **arquitetura** é Deep Learning (ele pensa como um cérebro profundo).
*   Sua **técnica de treino** é Aprendizado Supervisionado (nós damos as imagens e os gabaritos/labels para ele aprender).

---

## 2. Mergulhando em Deep Learning 🧠

### O que é uma Rede Neural?
Imagine que queremos ensinar o computador a diferenciar um Gato de um Cachorro.
*   **Programação Clássica:** Nós escrevemos as regras. "Se tiver orelha pontuda E bigode, é gato". (Muito difícil fazer isso para todas as raças!).
*   **Deep Learning:** Nós damos os dados (fotos) e a resposta (label), e o computador **cria as regras sozinho**.

Uma Rede Neural tenta imitar o cérebro humano. Ela é feita de camadas de neurônios artificiais:

1.  **Camada de Entrada (Input Layer):** São os "olhos" da rede. No nosso caso, são os pixels da imagem.
2.  **Camadas Ocultas (Hidden Layers):** É onde a mágica acontece. Milhares de neurônios conectados matematicamente processam a imagem procurando bordas, depois formas, depois texturas, até formar o conceito de "focinho" ou "orelha".
3.  **Camada de Saída (Output Layer):** É a decisão final. "É um Gato com 98% de certeza".

> **Imagem para Visualizar:** Imagine vários pontos ligados por linhas.
> ![Rede Neural](https://upload.wikimedia.org/wikipedia/commons/3/3d/Neural_network.svg)

### Conceitos Chave do Treinamento 🏋️‍♂️

#### 1. Epoch (Época) 🔄
Uma época é **uma passada completa** por todo o seu material de estudo.
*   Se você tem 1000 fotos e treinou por **10 epochs**, significa que o modelo olhou cada uma das 1000 fotos exatamente 10 vezes.
*   Poucas épocas? O modelo não aprende (Underfitting).
*   Muitas épocas? O modelo decora (Overfitting).

#### 2. Weights (Pesos) ⚖️
São as conexões entre os neurônios. Aprender, para o computador, nada mais é do que **ajustar esses pesos**.
*   No começo, os pesos são aleatórios (o modelo é burro).
*   A cada erro que ele comete no treino, ele volta ajustando os pesos (um processo chamado *Backpropagation*) para errar menos na próxima vez.

#### 3. CNN (Rede Neural Convolucional) 📸
É o tipo especial de rede usada para imagens. Em vez de olhar todos os pixels de uma vez, ela passa um "filtro" (como uma pequena lanterna) escaneando a imagem pedaço por pedaço para entender o contexto visual. O YOLO é uma CNN extremamente rápida.

---

## 3. Como sabemos se o modelo aprendeu? (Métricas) 📊

Na escola, temos "nota 0 a 10". Na Ciência de Dados, o buraco é mais embaixo.

Não adianta olhar e dizer "acho que ficou bom". Precisamos de matemática. As três métricas sagradas que todo engenheiro de ML deve ter tatuado no braço são:

### 1. Precision (Precisão) - "A Qualidade"
Responde à pergunta: **De tudo que o modelo "disse" que era cachorro, quantos eram realmente cachorro?**
*   Exemplo: O modelo detectou 10 objetos na foto. Desses 10, acertou 9.
*   Precisão = 90%.
*   **Alta Precisão** = O modelo é chato, só fala quando tem certeza. Pouco "Alarme Falso".

### 2. Recall (Revocação) - "A Quantidade"
Responde à pergunta: **De todos os cachorros que EXISTIAM na foto, quantos o modelo conseguiu achar?**
*   Exemplo: Tinha 100 cachorros na foto. O modelo achou só os 10 do exemplo acima.
*   Recall = 10% (Ele é míope).
*   **Alto Recall** = O modelo é paranoico, sai marcando tudo para não perder nada.

> **O Dilema:** É um cobertor curto. Se você quer muita precisão, o recall cai (ele deixa de marcar os duvidosos). Se quer muito recall, a precisão cai (ele começa a marcar gato como cachorro só para garantir).

### 3. mAP (Mean Average Precision) - "A Nota Final"
Como medimos o equilíbrio perfeito? Com o mAP.
Ele é uma média complexa que considera tanto a Precisão quanto o Recall.
*   **mAP50:** Considera acerto se a caixinha cobrir pelo menos 50% do objeto. (Professor Bonzinho).
*   **mAP50-95:** É a média de várias exigências, desde 50% até 95% de perfeição no desenho da caixa. (Professor Rigoroso).

---

## 4. Matriz de Confusão: O Raio-X do Modelo 😵

Todas as métricas acima saem daqui. A Matriz de Confusão mostra exatamente onde o modelo está errando.

**1. O Conceito Básico (Binário):**
Imagine uma tabela 2x2 para "É Cachorro" ou "Não é":

|                        | **É Cachorro (Real)** | **NÃO é Cachorro (Real)**  |
|------------------------|-----------------------|----------------------------|
| **Disse que É**        | **True Positive (TP)**<br>*(Acertou na mosca)* | **False Positive (FP)**<br>*(Alarme Falso/Mentiroso)* |
| **Disse que NÃO é**    | **False Negative (FN)**<br>*(Míope/Deixou passar)* | **True Negative (TN)**<br>*(Acertou ignorando)* |

*   **TP:** Era cachorro e ele disse cachorro. (Ótimo!)
*   **FP:** Era gato e ele gritou "Cachorro!". (Erro Tipo 1 - O Mentiroso).
*   **FN:** Era cachorro e ele ficou quieto. (Erro Tipo 2 - O Cego).

> **A Regra de Ouro:** Queremos números altos na **diagonal principal** (TP e TN) e zeros no resto.

**2. Na Prática (Múltiplas Classes + Fundo):**
Suponha que queremos identificar peixes, tubarões e água viva em um vídeo. 
Quando temos vários objetos (Peixe, Tubarão, Água Viva), a matriz vira um **Mapa de Calor (NxN)**.
Olhe para a imagem que o YOLO gera:

![Exemplo de Matriz de Confusão Multiclasse](confusion_matrix.png)
*   **Linhas (True):** O que realmente existe na imagem.
*   **Colunas (Predicted):** O que o modelo achou.
*   **DIAGONAL PRINCIPAL:** É onde estão os acertos. (Era Peixe -> Disse Peixe). Queremos essa linha bem azul escura!
*   **FORA DA DIAGONAL:** São as confusões. Se o quadrado (Tubarão, Peixe) estiver azul, significa que o modelo está chamando muitos Tubarões de Peixe!

**3. O Mistério do "Background" (Fundo):**
No YOLO, existe uma classe extra chamada "Background".
*   **Background FN:** Tinha um objeto lá, mas o modelo achou que era só água (Fundo). Ele não viu nada.
*   **Background FP:** Não tinha nada lá (era só água), mas o modelo alucinou um objeto.

> **A Regra de Ouro:** Queremos números altos na **diagonal principal** e zeros no resto.

---

## 5. Classificação vs. Detecção: Qual a diferença?

Muitos iniciantes confundem esses dois conceitos. Vamos esclarecer:

### 1.1 Classificação de Imagens
É quando queremos responder: **"O que tem nesta imagem?"**
*   **Entrada**: Uma foto de um gato.
*   **Saída**: "Gato" (99% de certeza).
*   **Limitação**: Se houver um gato E um cachorro, classificadores simples podem se confundir ou dizer apenas o objeto dominante. Eles não dizem *onde* o objeto está.

### 1.2 Detecção de Objetos (O que faremos hoje)
É quando queremos responder: **"Onde estão os objetos e quais são eles?"**
*   **Entrada**: Uma foto com um gato, um cachorro e uma pessoa.
*   **Saída**:
    *   "Gato" na posição (x=10, y=20) (Caixa delimitadora / Bounding Box).
    *   "Cachorro" na posição (x=50, y=80).
    *   "Pessoa" na posição (x=90, y=10).

O YOLO é famoso porque ele consegue fazer isso extremamente rápido (em tempo real), olhando para a imagem apenas uma vez (daí o nome *You Only Look Once* - Você Só Olha Uma Vez).

---

## 6. Preparando o Ambiente

Vamos usar a biblioteca `ultralytics`, que é a implementação moderna do YOLO (v8).

Para manter este trabalho separado do projeto anterior, criaremos um novo ambiente Poetry. Execute no terminal:

```bash
# criar diretório específico para a aula 07
cd ~/projetos/visao/

# inicializar o projeto Poetry
poetry new aula-07
cd aula-07

# um lugar para guardar notebooks
mkdir -p notebooks
```

Adicione as dependências necessárias para rodar o YOLO e manipular imagens:

```bash
poetry add ultralytics numpy opencv-python pillow
poetry add --dev jupyter ipykernel
```

Registre o kernel Jupyter para que ele apareça no VS Code/Jupyter:

```bash
poetry run python -m ipykernel install --user --name aula-07 --display-name "aula-07 (poetry)"
```

Depois disso, abra o VS Code e selecione o kernel (Ctrl+Shift+P → ) `aula-07 (poetry)`.


> **Problemas de Conexão? (Dica de Ouro)**
> O download do PyTorch pode travar tentando puxar drivers NVIDIA gigantescos (+2 GB) se você não tiver GPU. Se isso ocorrer, prefira a versão só-CPU:
> ```bash
> poetry source add --priority=explicit pytorch_cpu https://download.pytorch.org/whl/cpu
> poetry add --source pytorch_cpu torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1
> poetry add ultralytics
> ```


---

## 7. Prática 1: Detectando objetos em imagens e vídeos.

Execute o notebook 07-prof-saulo-introducao-yolo.ipynb