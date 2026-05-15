# YOLO Academia - Detecção de Equipamentos de Musculação

Projeto desenvolvido para a disciplina de **Visão Computacional** do curso de 
Engenharia da Computação - UNICEP, sob orientação do **Prof. Saulo Santos**.

## Descrição

Este projeto implementa um detector de equipamentos de academia utilizando a 
arquitetura **YOLOv8 (You Only Look Once)**, passando por todo o ciclo de vida 
de um projeto de Inteligência Artificial: obtenção de dados, treinamento do 
modelo e inferência em vídeo real.

## Classes Detectadas

| Classe | Descrição |
|---|---|
| `bicep_curl_machine` | Máquina de rosca bíceps |
| `hip_adduction_machine` | Máquina de adução de quadril |

## Estrutura do Projeto
yolo-academia/
├── dataset_academia/ ← Dataset baixado do Roboflow (YOLOv8)
│ ├── train/ ← Imagens de treinamento
│ ├── valid/ ← Imagens de validação
│ ├── test/ ← Imagens de teste
│ └── data.yaml ← Mapa do dataset
├── runs/detect/ ← Resultados gerados pelo YOLO
│ ├── train-2/ ← Resultado do treino (notebook 10)
│ ├── train-3/ ← Resultado do treino (notebook 09)
│ └── predict/ ← Vídeo com inferência
├── 09.ipynb ← Aula 09: Custom Training
├── 10.ipynb ← Aula 10: Treinamento YOLOv8
├── 11.ipynb ← Aula 11: Inferência em vídeo
├── modelo_academia.pt ← Modelo final treinado
└── yolov8n.pt ← Modelo base YOLOv8 Nano

text

## 🗃️ Dataset

- **Fonte:** [Roboflow Universe](https://universe.roboflow.com)
- **Nome:** Gym Machine Detector
- **Formato:** YOLOv8
- **Total de imagens:** ~182 imagens
- **Classes:** 2 (bicep_curl_machine, hip_adduction_machine)

## ⚙️ Configuração do Ambiente

### Pré-requisitos
- Python 3.11+
- Git

### Instalação

```bash
# Clone o repositório
git clone https://github.com/Otavio67/yolo-academia.git
cd yolo-academia

# Crie o ambiente virtual
python -m venv venv

# Ative o ambiente virtual (Windows)
venv\Scripts\activate

# Instale as dependências
pip install ultralytics jupyter ipykernel
```

## Como Usar

### 1. Treinamento (Notebook 10)
Abra o `10.ipynb` e execute todas as células. O modelo será treinado com 
Transfer Learning usando o `yolov8n.pt` como base.

Parâmetros utilizados:
- `epochs=5`
- `imgsz=416`
- `batch=2`

### 2. Inferência em Vídeo (Notebook 11)
Abra o `11.ipynb`, adicione um vídeo `.mp4` na pasta raiz e execute todas 
as células. O vídeo com as detecções será salvo em `runs/detect/predict/`.

## Resultados do Treinamento

| Métrica | Valor |
|---|---|
| mAP50 | ~0.70 |
| Precision | ~0.70 |
| Recall | ~0.75 |
| Epochs | 5 |

As losses de treinamento e validação apresentaram queda consistente ao longo 
das epochs, indicando aprendizado efetivo do modelo.

## Tecnologias Utilizadas

- [YOLOv8 (Ultralytics)](https://github.com/ultralytics/ultralytics)
- [Python 3.11](https://www.python.org/)
- [Roboflow](https://roboflow.com/)
- [PyTorch](https://pytorch.org/)
- [Jupyter Notebook](https://jupyter.org/)
- [VS Code](https://code.visualstudio.com/)

## 👤 Autor

**Otavio** — Engenharia da Computação - UNICEP  
GitHub: [@Otavio67](https://github.com/Otavio67)

## 📄 Licença

Projeto acadêmico desenvolvido para fins educacionais.
