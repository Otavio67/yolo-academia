# Aula 11: Colocando seu Modelo em Produção (Inferência) 🚀

Parabéns! Você treinou o modelo. Agora você tem um arquivo chamado `best.pt` escondido nas pastas do projeto. Esse arquivo é o "cérebro" que aprendeu a diferenciar Tubarões de Peixes (ou o que você tenha treinado).

Mas e agora? Como usamos isso?

---

## 1. O Conceito de "Especialista" 🧠

Você fez uma pergunta excelente: *"Se o modelo foi treinado para reconhecer peixes, como ele conseguiria reconhecer pistolas?"*

**A resposta é: Ele não consegue!**

Quando fazemos o *Fine-tuning* (o que fizemos anteriormente), transformamos um modelo "Generalista" (que sabia 80 coisas genéricas) em um "Especialista" (que sabe MUITO bem apenas as coisas do seu dataset).

*   O `yolov8n.pt` sabe: pessoa, carro, cachorro, copo...
*   O seu `best.pt` sabe: Peixe, Tubarão, Pinguim... (e esqueceu o resto!).

Se você mostrar uma arma para o modelo do aquário, ele vai ignorar ou achar que é um peixe muito estranho. Para ter um modelo que sabe TUDO, você precisaria de um dataset que tivesse TUDO misturado (o que é bem difícil de fazer).

---

## 2. A Caça ao Tesouro: Onde está o `best.pt`? 🗺️

Quando o treino termina, o YOLO **sempre** salva dois arquivos principais automaticamente (esse nome é padrão da biblioteca, não fomos nós que escolhemos):

1.  **`best.pt`**: É o check-point onde o modelo teve a **melhor nota** na prova de validação. (É esse que queremos!).
2.  `last.pt`: É o modelo exatamente como estava no último segundo do treino. Nem sempre é o melhor, pois ele pode ter "piorado" um pouquinho no final  (overfitting), então geralmente ignoramos esse.

Eles ficam em uma pasta parecida com esta:
`runs/detect/trainX/weights/best.pt`

*   `trainX`: Pode ser `train`, `train2`, `train3`... dependendo de quantas vezes você rodou o comando. Sempre pegue a pasta com o **maior número** (a mais recente).

Vamos localizar esse arquivo e, para facilitar nossa vida, **copiá-lo** para a raiz do projeto com o nome `modelo_aquario.pt`.

Você pode fazer isso manualmente pelo explorador de arquivos do VS Code ou pelo terminal:

```bash
# Exemplo (ajuste o 'train14' para o número da sua pasta correta!)
cp runs/detect/train/weights/best.pt modelo_aquario.pt
```

---

## 3. Script de Teste (Vento no Rosto) 🏍️

Agora vamos criar um script Python (aula-09/notebooks11-prof-saulo-inferencia-yolo.ipynb)que carrega ESSE modelo específico e roda na webcam ou num vídeo.

Crie o arquivo `teste_modelo_customizado.py`:

```python
from ultralytics import YOLO
import cv2

# --- CONFIGURAÇÃO ---
# Em vez de 'yolov8n.pt', carregamos O NOSSO ARQUIVO!
nome_modelo = 'modelo_aquario.pt' 

# Fonte de vídeo (0 para webcam, ou nome de um arquivo mp4)
source = 'video_teste.mp4' 
# --------------------

print(f"Carregando modelo: {nome_modelo}...")
model = YOLO(nome_modelo)

# Verificar quais classes esse modelo conhece
print("Classes que este modelo conhece:", model.names)

# Rodar inferência no vídeo (show=True abre a janelinha)
# conf=0.5 significa que só mostra se tiver 50% de certeza
model.predict(source=source, show=True, conf=0.5)
```

### Desafio 
1.  Baixe um vídeo do YouTube que tenha peixes (ex: "Aquarium 4k") usando algum site de download.
2.  Salve como `video_teste.mp4`.
3.  Rode o script e veja se ele detecta os peixes!

---

## 4. O Resultado

Você notará que agora, nas caixinhas, não aparece mais "book" ou "person", e sim as classes do seu dataset (ex: `fish`, `shark`, `stingray`).

Isso é a prova final de que a IA aprendeu o que você ensinou. Você criou tecnologia! 🤖✨
