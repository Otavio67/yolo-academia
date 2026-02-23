# Manual de Git e GitHub para Trabalho Colaborativo

## 1. Objetivo do Manual

Este manual tem como objetivo capacitá-lo a trabalhar de forma **colaborativa, organizada e profissional** utilizando **Git** e **GitHub**, ferramentas amplamente adotadas na indústria de software, ciência de dados e projetos acadêmicos.

Ao final do estudo e da prática dos estudos de caso, o aluno deverá ser capaz de:
- Compreender o papel do Git como sistema de controle de versão
- Trabalhar com repositórios locais e remotos
- Colaborar em equipe de forma segura e rastreável
- Resolver conflitos de versionamento
- Adotar boas práticas profissionais

Ref. Bibliográfica:
Aquiles, A.; Ferreira, R. Controlando versões com Git e GitHub. 2. ed. São Paulo: Casa do Código, 2020.

---

## 2. O que é Git

### 2.1 Definição

**Git** é um **sistema de controle de versão distribuído (DVCS – Distributed Version Control System)** criado por Linus Torvalds.

Ele permite:
- Registrar a evolução de arquivos ao longo do tempo
- Recuperar versões anteriores
- Trabalhar simultaneamente com várias pessoas
- Criar ramificações (branches) independentes
- Unir contribuições de diferentes autores

O Git não depende de conexão constante com a internet, pois cada desenvolvedor possui uma cópia completa do histórico do projeto.

---

### 2.2 Problema que o Git resolve

Sem controle de versão, é comum observar:
- Perda de código
- Arquivos duplicados (v1, v2, v3, final_final)
- Conflitos de edição
- Falta de rastreabilidade (quem alterou o quê e quando)

O Git resolve esses problemas ao **versionar o projeto como um todo**, e não apenas arquivos isolados.

---

## 3. Conceitos Fundamentais do Git

### 3.1 Repositório

Um **repositório Git** é o local onde:
- Os arquivos do projeto são armazenados
- O histórico completo de versões é mantido

Pode ser:
- **Local** (na máquina do desenvolvedor)
- **Remoto** (GitHub, GitLab, Bitbucket)

---

### 3.2 Estados dos Arquivos

No Git, um arquivo pode estar em três estados principais:

1. **Untracked** – arquivo novo, ainda não monitorado
2. **Modified** – arquivo alterado
3. **Staged** – arquivo preparado para o próximo commit

---

### 3.3 Commit

Um **commit** representa um **snapshot do projeto em um determinado momento**.

Cada commit possui:
- Um identificador único (hash)
- Autor
- Data
- Mensagem descritiva

Boas mensagens de commit são claras, objetivas e técnicas.

---

### 3.4 Branch

Uma **branch** é uma linha paralela de desenvolvimento.

A branch principal costuma ser chamada de:
- `main` ou `master`

Branches permitem:
- Desenvolver novas funcionalidades
- Corrigir bugs
- Testar ideias sem afetar o código estável

Para alternar entre branches, utilizamos o comando:
```bash
git checkout nome-da-branch
```

---

### 3.5 Merge

**Merge** é o processo de integrar alterações de uma branch em outra.

Pode ocorrer:
- Automaticamente
- Com conflitos, quando duas alterações incompatíveis ocorrem no mesmo trecho de código

---

## 4. O que é GitHub

### 4.1 Definição

**GitHub** é uma plataforma online que hospeda repositórios Git e adiciona recursos colaborativos, tais como:
- Pull Requests
- Issues
- Controle de acesso
- Revisão de código
- Histórico público ou privado

GitHub **não substitui o Git**, ele o complementa.

---

### 4.2 Fluxo Básico Git + GitHub

1. Criar repositório no GitHub
2. Clonar o repositório localmente
3. Criar/editar arquivos
4. Commitar alterações
5. Enviar para o GitHub (push)
6. Integrar contribuições (pull / merge)

---

### 4.3 Configurando Autenticação Segura (Personal Access Token - PAT)

Para enviar códigos para o GitHub (push), não é mais possível utilizar a senha da conta. É obrigatório criar um **Token de Acesso Pessoal (PAT)**.

**Passo a passo para criar seu PAT:**

1.  Faça login no GitHub.
2.  Clique na sua foto de perfil (canto superior direito) > **Settings**.
3.  Role a barra lateral esquerda até o final e clique em **Developer settings**.
4.  Clique em **Personal access tokens** > **Tokens (classic)**.
5.  Clique em **Generate new token** > **Generate new token (classic)**.
6.  **Note:** Dê um nome para identificar o computador (ex: "projeto-colaborativo").
7.  **Expiration:** Defina um prazo de validade (ex: 7 days ou 30 days).
8.  **Select scopes:** Marque a opção **repo** (isso garantirá acesso total aos repositórios).
9.  Clique em **Generate token** (final da página).
10. **IMPORTANTE:** Copie o código gerado (ele começa com `ghp_...`). **Você não poderá vê-lo novamente!** Salve-o em um local seguro temporário ou envie para si mesmo.



**Como usar:**
Quando o terminal pedir "Password", cole esse token no lugar da senha.

---

## 5. Fluxo de Trabalho Colaborativo (Visão Geral)

1. Cada desenvolvedor trabalha em sua própria branch
2. Commits pequenos e frequentes
3. Uso de Pull Requests para integrar mudanças
4. Revisão de código antes do merge
5. Branch principal sempre funcional

---

## 6. Estudos de Caso Práticos

Os estudos de caso a seguir devem ser realizados **passo a passo**. Ele foi desenvolvido para simular um cenário real com 2 usuarios (ana-branch2000 e joao-branch2000) trabalhando em um projeto colaborativo. A ideia é que os exercicios sejam desenvolvidos em maquinas diferentes, simulando o trabalho em equipe. No entanto, se voce estiver sozinho, pode simular o trabalho em equipe criando duas contas no github e a mesma maquina, mas alterando de usuario quando indicado no exercício. 

Eu vou apresentar o passo a passo dos exercicios simulando o trabalho de ambos os usuarios na mesma máquina. Essa forma é mais arriscada e suscetível a erros, por isso, recomendo o uso de máquinas diferentes.  

---

### Pré-requisito: Ter o Git e o WSL Ubuntu instalados e configurados em sua máquina

#### Verificação e Troca de Usuário

Antes de iniciar os estudos de caso, verifique qual usuário está configurado na máquina:
```bash
git config user.name
git config user.email
```

Caso precise trocar de usuário (ex: alternar entre os usuários de teste **ana-branch2000** e **joao-branch2000**), utilize os comandos abaixo:

```bash
# Definindo usuário
git config user.name "joao-branch2000"
git config user.email "joao-branch2000@gmail.com"
# ou
git config user.name "anabranch2000"
git config user.email "anabranch2000@gmail.com"
```

### Estudo de Caso 1 – Criando o Primeiro Repositório

**Objetivo:** compreender o ciclo básico do Git.

**Passo a passo: para criar um repositório localmente gerenciado por "anabranch2000"/"anabranch2000@gmail.com"**
1. Criar diretório do projeto (Simulando o PC da Ana)
```bash
mkdir ana-workspace
cd ana-workspace
mkdir projeto-colaborativo
cd projeto-colaborativo
```
2. Inicializar o Git (`git init`)
3. Voce receberá um aviso pedindo para você trocar o nome da branch principal para main (`git branch -M main`)
4. Se estiver no linux, execute o comando `ls -la` para verificar a pasta `.git` que foi criada.
5. Verificar o status (`git status`). Você verá que não há arquivos para serem adicionados (comitados).
6. Inicializar o repositório e configurar identidade
```bash
git init
git config user.name "anabranch2000"
git config user.email "anabranch2000@gmail.com"
```
*(Dica: Rode `git config user.email` para confirmar se funcionou)*
7. Criar o primeiro arquivo e commitar
```bash
echo "# Projeto Colaborativo" > README.md
git add README.md

```
8. Execute novamente `ls -la` para verificar que o arquivo README.md foi criado.
9. Verificar o status (`git status`). Você verá que o arquivo README.md está listado como "untracked" (em vermelho).
10. Adicionar arquivo ao stage (`git add README.md`). Agora o arquivo README.md está listado como "staged" (em verde).
11. Para retornar ao modo de "untracked", execute o comando `git rm --cached README.md`. O arquivo README.md será removido do stage e voltará ao modo de "untracked" (vermelho).
12. Repita a etapa 10 para adicionar o arquivo README.md ao stage novamente.
13. Realizar o primeiro commit (`git commit -m "Commit inicial: Ana cria o projeto"`).
14. Verificar o status (`git status`). Você verá que não há arquivos para serem adicionados (comitados).
15. Conectar ao GitHub (`git remote add origin https://github.com/anabranch2000/projeto-colaborativo.git`)
16. Enviar os arquivos (`git push -u origin main`)

    > **Errou? "Password authentication is not supported"?**
    > O GitHub removieu o suporte a senhas de conta para HTTPS.
    > **Solução:** Você deve usar o **Personal Access Token (PAT)** que criamos na seção 4.3 como senha.
    > Quando o terminal pedir "Password", cole o seu PAT (aquele código que começa com `ghp_...`).
    >
    > **Errou? Permission denied (403)?**
    > Se apareceu `Permission ... denied to [Outro-Usuario]`, é porque o computador lembrou de uma senha antiga.
    > O Git não pediu senha porque ele achou que já sabia!
    > **Solução:** Force o Git a usar o usuário correto colocando-o no link:
    > `git remote set-url origin https://anabranch2000@github.com/anabranch2000/projeto-colaborativo.git`
    >
    > **Ainda deu erro?** O computador pode estar "teimoso" segurando a senha na memória.
    > Rode este comando para limpar a memória de senhas do Git:
    > ```bash
    > git credential-cache exit
    > ```
    > **Ainda deu erro? (VS Code / Terminais Integrados)**
    > Se você está usando o terminal do VS Code, ele pode estar "roubando" a autenticação.
    > Force o terminal a ignorar o VS Code e pedir a senha na tela preta:
    > ```bash
    > GIT_ASKPASS= git push -u origin main
    > ```
    > *Sim, tem um espaço entre o `=` e o `git`.*
    >
    > **Atenção Máxima:** O que define quem você é para o GitHub é o **TOKEN** que você cola, e não o nome que aparece na tela.
    > Se você colar o token do *Usuario A*, o GitHub vai te logar como *Usuario A*, mesmo que você esteja tentando acessar a conta do *Usuario B*.
    > **Verifique se você copiou o token correto!**

**Resultado esperado:**
- Repositório local versionado
- Código sincronizado com o GitHub
- Histórico com pelo menos um commit

---

### Estudo de Caso 2 – Versionando um Projeto Simples

*(Ana continua trabalhando em sua pasta `ana-workspace/projeto-colaborativo`)*

**Objetivo:** Compreender a importância de realizar commits pequenos e frequentes (granularidade).
Em vez de salvar o projeto inteiro apenas no final, o Git permite salvar cada "peça de lego" que você adiciona ao projeto. Isso permite voltar no tempo com precisão caso algo dê errado em uma etapa específica.

Neste estudo, você criará um arquivo `app.py`, fará o versionamento inicial e, em seguida, realizará uma alteração para entender o fluxo de atualização.

**Passo a passo:**
1. Crie um arquivo de script Python:
   ```bash
   echo "print('Ola Mundo - Versao 1')" > app.py
   ```
2. Adicione e commite a primeira versão:
   ```bash
   git add app.py
   git commit -m "Adiciona app.py na versao 1"
   ```
3. Modifique o código (simulando uma atualização):
   ```bash
   echo "print('Ola Mundo - Versao 2')" > app.py
   ```
4. Verifique o que mudou antes de salvar (`git diff`):
   ```bash
   git diff
   ```
   *Você verá a linha antiga em vermelho (-) e a nova em verde (+).*
5. Realize o segundo commit (usando o atalho `-am` para adicionar e commitar ao mesmo tempo):
   ```bash
   git commit -am "Atualiza app.py para versao 2"
   ```
6. Consulte o histórico de versões:
   ```bash
   git log
   ```
   *Para uma visão resumida, tente:* `git log --oneline`

**Resultado esperado:**
- Histórico com múltiplas versões

---

### Estudo de Caso 3 – Uso de Branches

*(Ana continua trabalhando em sua pasta `ana-workspace/projeto-colaborativo`)*

**Objetivo:** Trabalhar em paralelo sem "quebrar" o código principal.
Branches (ramificações) são universos paralelos onde você pode testar novas ideias à vontade. Se funcionar, você traz para o oficial (Merge). Se der errado, basta apagar a branch e o projeto principal continua perfeito.

> **Contexto Profissional:**
> Dentro de um contexto profissional, cada colaborador cria a sua própria branch e trabalha nela localmente em sua própria máquina fazendo diversos commits para salvar a evolução de sua feature. Somente após a conclusão de sua feature, ele faz o merge e em seguida o push no main.

Para apagar uma branch (o que não faremos agora), o comando seria: `git branch -D nome-da-branch`.

**Passo a passo:**
1. Crie uma nova branch chamada `feature-login` e mude para ela:
   ```bash
   git checkout -b feature-login
   ```
2. Execute `git branch` para checar as branchs criadas. 
  
3. Crie um novo arquivo simulando uma nova funcionalidade:
   ```bash
   echo "print('Login v1')" > login.py
   git add login.py
   git commit -m "Implementa login basico"
   ```
> **O que aconteceu aqui?**
> O arquivo `login.py` agora existe **apenas** na branch `feature-login`. A branch `main` continua intacta (sem esse arquivo). Isso permite que Ana trabalhe sem medo de estragar o projeto principal.

4. Volte para a branch `main` e verifique que o arquivo sumiu:
   ```bash
   git checkout main
   ls
   ```
   *Note que `login.py` não existe aqui. A `main` está intacta.*

5. Execute `git status`:
   ```bash
   git status
   ```
   *Você verá: `Your branch is ahead of 'origin/main' by 2 commits.`*
   
   > **O que isso significa?**
   > A branch `main` no seu computador tem 2 commits a mais que a do GitHub.
   > O GitHub está "no passado". A mensagem sugere `git push` para publicar.

   *Você verá: (use "git push" to publish your local commits). *
   > Isso indica que você pode enviar os commits para o GitHub para que eles sejam publicados.

6. Execute `git diff --name-only main feature-login` para ver as diferenças entre as duas branches. Serão mostrados todos os arquivos da branch `feature-login` que não existem na branch `main`.   

   ```bash
   git diff --name-only main feature-login
   ```
   *Você verá: `login.py`*

7. Agora faça o merge da branch `feature-login` para a branch `main`. O arquivo login.py será adicionado à branch `main`:
   ```bash
   git merge feature-login
   ```
   *Você verá: `Already up to date.`*

8. Para ver quais arquivos existem na sua máquina (main local) mas ainda não existem no GitHub (origin/main) é:
   ```bash
   git diff --name-only main origin/main
   ```
   *Você verá: `login.py`*

9. Volte para a branch `main` e verifique que o arquivo sumiu:
   ```bash
   git checkout main
   ls (ou git ls-files)
   ```
   *Note que `login.py` não existe aqui. A `main` está intacta.*

10. Verifique o resultado e atualize o GitHub:
   ```bash
   git log --oneline
   git push
   ```

**Resultado esperado:**
- Branch integrada corretamente

---

### Estudo de Caso 4 – Colaboração em Equipe (GitHub)

**Objetivo:** Simular um ambiente de trabalho profissional onde múltiplos desenvolvedores contribuem para o mesmo projeto.
Neste cenário, **João** (`joaobranch2000`) atuará como colaborador no projeto criado originalmente pela **Ana** (`anabranch2000`), enviando novas funcionalidades através de suas próprias branches.

**Passo a passo:**
1. Criar repositório no GitHub (O repositório já existe. Usaremos o repositório criado anteriormente)

2. Adicionar colaboradores (Ação crítica no Site):
   *   **Ana (Dona):** Clique no repositório que voce deseja compartilhar. Vá em **Settings > Collaborators** > **Add people**.
       *   Digite o usuário `joaobranch2000` e clique em **Add to this repository**.
   *   **João (Colaborador):** Você receberá um **e-mail** ou uma **notificação** (sininho no canto superior direito do GitHub).
   *   **Importante:** João DEVE clicar em **Accept invitation** antes de prosseguir. Sem isso, o Git bloqueará o envio (Erro 403).

3. João clona o repositório (no computador dele):
   ```bash
   mkdir joao-workspace
   cd joao-workspace
   git clone https://github.com/anabranch2000/projeto-colaborativo.git
   cd projeto-colaborativo
   
   # Configurando a identidade do João APENAS nesta pasta:
   git config user.name "joaobranch2000"
   git config user.email "joaobranch2000@gmail.com"
   ```
   Quando João clonou o repositorio, o git criou automaticamente o `git remote add origin https://github.com/anabranch2000/projeto-colaborativo.git`. Portanto, João não precisa fazer `git remote add origin https://github.com/anabranch2000/projeto-colaborativo.git`, antes do push (vide passo 6 abaixo).

4. João cria sua própria branch `feature-login` e muda para ela:
   ```bash
   git checkout -b feature-login
   git branch
   ```

5. João cria uma nova funcionalidade (arquivo):
   ```bash
   echo "print('Login do Joao')" > login-joao.py
   ```

6. João commita e envia as alterações:
   ```bash
   git status
   git add .
   git status
   git commit -m "Joao implementa login basico"
   git push origin feature-login
   ```  
   > **Atenção:** Neste momento o Git pedirá login.
   > Você deve usar o usuário **`joaobranch2000`** e o **TOKEN do João**.
   > *Não use os dados da Ana, pois é o João quem está fazendo o envio.*  

   > **Atenção:** Se der erro de negação de usuário (denied to <nome-do-usuario >) você deve forçar o Git a "esquecer" esse usuário e pedir o token do joaobranch2000. Rode este comando para forçar:

   ```bash   
   GIT_ASKPASS= git push origin feature-login
   ```
   *Isso vai abrir o prompt de senha. Aí você cola o Token do João.*

   ```bash
   git push origin feature-login
   ```

7. João abre o Pull Request no site do GitHub:
   Ao acessar a página inicial do repositório, João verá uma barra amarela com o botão verde **"Compare & pull request"**.
   > Isso significa que o GitHub detectou que João enviou uma branch nova (`feature-login`) e está sugerindo que ele a una com a principal.
   
   Clique nesse botão. Você verá uma tela "Comparing changes":
   *   Verifique se a seta aponta de `base: main` **←** `compare: feature-login`.
   *   Adicione um título e descrição (ex: "Adicionando login").
   *   Clique no botão verde **Create pull request**.

8. Ana aceita o Pull Request (Fuse as branches):
   *   **Ana (Dona):** Vá na aba **Pull Requests** no GitHub.
   *   Clique no PR aberto pelo João ("Implementa login basico").
   *   Clique no botão verde **Merge pull request** e depois em **Confirm merge**.
   *   *A mágica acontece: O código do João agora faz parte da `main` oficial.*

   # Volte para a pasta da Ana (se estiver na do João)
   trocar usuario para ana: 
   # Configurando a identidade do João APENAS nesta pasta:
   git config user.name "anabranch2000"
   git config user.email "anabranch2000@gmail.com"
   git checkout main
   git pull origin main
   ```
   > **O que esse comando faz?**
   > O `git pull` é o "download". Ele vai até o GitHub (`origin`), pega as novidades que a Ana aceitou no PR (o código do João) e mistura (`merge`) com os arquivos que estão na pasta do computador dela. Agora a máquina da Ana tem a versão mais recente do projeto.
- Histórico com múltiplos autores
- Uso correto de Pull Requests

---

### Estudo de Caso 5 – Resolução de Conflitos

**Objetivo:** Aprender a lidar com situations onde o Git não consegue decidir sozinho o que fazer (Conflitos de Merge).
Isso acontece quando duas pessoas alteram **a mesma linha do mesmo arquivo** de formas diferentes. O Git entra em pânico e pede ajuda humana para decidir qual versão é a correta (ou se devemos criar uma mescla das duas).
*Sim, vamos "quebrar" o repositório de propósito para consertá-lo depois.*

**Passo a passo:**

1. **Ana** altera o arquivo primeiro (Simulando um trabalho concluído):
   *   Vá para a pasta da Ana.
   *   Edite o arquivo `README.md` trocando o título.
   ```bash
   cd ../projeto-colaborativo (garanta que está na pasta da Ana)
   # Confirme a identidade
   git config user.name "anabranch2000"
   
   echo "# Projeto Colaborativo - Versão 1.0 (Ana)" > README.md
   git add README.md
   git commit -m "Ana define o titulo oficial"
   git push origin main
   ```
   *Agora o GitHub tem a versão da Ana.*

2. **João** altera o MESMO arquivo (Simulando desatualização):
   *   **Importante:** NÃO faça `git pull` na pasta do João. Queremos justamente que ele esteja desatualizado.
   *   Vá para a pasta do João.
   *   Edite a **mesma linha** do `README.md`.
   ```bash
   
   cd ../../joao-workspace/projeto-colaborativo
   # Confirme a identidade
   git config user.name "joaobranch2000"
   
   git checkout main
   # (Não faça git pull aqui! Queremos simular que ele está desatualizado)
   
   echo "# Projeto Top do João (Conflito)" > README.md
   git add README.md
   git commit -m "João muda o titulo tambem"
   ```
3. **João Tenta Enviar (O Erro):**
   João, inocente, tenta enviar suas mudanças para o servidor.
   ```bash
   git push origin main
   ```
   *Se houver erro de negacao (denied) execute: `GIT_ASKPASS= git push -u origin main`*

   **Resultado:** 🛑 O Git vai bloquear com uma mensagem vermelha!
   > *error: failed to push some refs to...*
   > *hint: Updates were rejected because the remote contains work that you do not have.*
   
   **Tradução:** "João, a Ana enviou novidades para a nuvem que você ainda não tem. Eu não posso aceitar seu código por cima do dela. Baixe as mudanças primeiro!"

4. **João Tenta Atualizar (O Conflito):**
   João obedece e tenta baixar (`pull`) as mudanças da Ana para misturar com as dele.
   ```bash
   git config pull.rebase false  # Configuração essencial para evitar erro de 'divergent branches'
   git pull origin main
   ```
   **Resultado:** 💥 BUM! O terminal mostra:
   > *CONFLICT (content): Merge conflict in README.md*
   > *Automatic merge failed; fix conflicts and then commit the result.*

   > **Nota Técnica:** Versões modernas do Git exigem que você escolha uma estratégia quando as histórias se separam.
   > O comando `pull.rebase false` diz: "Git, prefiro o comportamento clássico de criar um Commit de Merge para unir os trabalhos."

   **O que aconteceu?**
   O Git tentou juntar "Versão 1.0 (Ana)" (que veio da nuvem) com "Projeto Top do João" (que estava no PC), mas ambos mexeram na **mesma linha**. O Git não sabe qual texto manter, então ele **paralisou tudo** e deixou o arquivo marcado para você resolver.
5. **Resolvendo a Bagunça ("Cirurgia" no Código):**
   Abra o arquivo `README.md` no seu editor (VS Code, Bloco de Notas ou Nano).
   Você verá algo bizarro assim:

   ```text
   <<<<<<< HEAD
   # Projeto Top do João (Conflito)
   =======
   # Projeto Colaborativo - Versão 1.0 (Ana)
   >>>>>>> [algum-codigo-hash]
   ```
   **O que é isso?**
   *   `<<<<<<< HEAD`: Daqui até o `=======` é o que **VOCÊ (João)** tem no computador.
   *   `=======`: É a fronteira dividindo os dois mundos.
   *   `>>>>>>> ...`: Daqui até o final é o que a **ANA** escreveu (que veio da nuvem).

   **Sua Missão:**
   Apague "na mão" os marcadores feios (`<<<`, `===`, `>>>`) e as linhas que você não quer. Deixe o texto bonitinho, como ele deve ficar no final.
   
   *Exemplo de como deixar o arquivo final:*
   ```text
   # Projeto Top do João (Conflito)
   # Projeto Colaborativo - Versão 1.0 (Ana)
   ```
   *(Salve o arquivo!)*

6. **Finalizando o Merge:**
   Agora que você "limpou" o arquivo, avise o Git que o conflito acabou.
   ```bash
   git status
   # O arquivo ainda estará vermelho (both modified)
   
   git add README.md
   # Agora ficou verde!
   
   git commit -m "Conflito resolvido: uniao dos titulos"
   git push origin main
   ```
   **Sucesso!** O terminal aceitará o envio. O conflito é passado.

**Resultado esperado:**
- Conflito resolvido conscientemente
- Código final consistente

---
