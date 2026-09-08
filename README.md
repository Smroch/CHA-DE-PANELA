# Chá da Érica — Lista de Presentes

Site com lista de presentes compartilhada em tempo real: quando um convidado escolhe um item, todos os outros veem a atualização na hora, sem precisar recarregar a página.

## Estrutura de pastas

```
cha-da-erica/
├── index.html
├── README.md
├── vercel.json
├── .gitignore
├── assets/
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── app.js
└── icons/
    └── favicon.svg
```

## Passo 1 — Criar o banco de dados grátis (Firebase)

1. Acesse **https://console.firebase.google.com** e faça login com sua conta Google.
2. Clique em **"Adicionar projeto"**, dê um nome (ex: `cha-da-erica`) e siga os passos (pode desativar o Google Analytics, não é necessário).
3. No menu lateral, clique em **Build → Realtime Database**.
4. Clique em **"Criar banco de dados"**.
   - Escolha a localização (qualquer uma, ex: `us-central1`).
   - Selecione **"Iniciar no modo de teste"** (mais fácil de configurar; ajustamos as regras no passo 3 abaixo).
5. Copie a **Database URL** que aparece no topo da página (algo como `https://cha-da-erica-default-rtdb.firebaseio.com`).
6. No menu lateral, clique no ⚙️ (Configurações do projeto) → role até **"Seus apps"** → clique no ícone `</>`  (Web) para criar um app da Web.
   - Dê um apelido (ex: `site`) e clique em **"Registrar app"**.
   - O Firebase vai mostrar um bloco `firebaseConfig = {...}`. **Copie esse bloco inteiro.**

## Passo 2 — Colar a configuração no arquivo

Abra `assets/js/app.js`, procure pelo topo do arquivo:

```js
const firebaseConfig = {
  apiKey: "COLE_AQUI",
  authDomain: "COLE_AQUI.firebaseapp.com",
  databaseURL: "https://COLE_AQUI-default-rtdb.firebaseio.com",
  projectId: "COLE_AQUI",
  storageBucket: "COLE_AQUI.appspot.com",
  messagingSenderId: "COLE_AQUI",
  appId: "COLE_AQUI"
};
```

Substitua pelos valores reais que o Firebase te deu no passo 1.6. Salve o arquivo.

## Passo 3 — Ajustar as regras de segurança (importante!)

O "modo de teste" do Firebase expira em 30 dias e, além disso, é bom deixar as regras corretas desde já:

1. No Firebase, vá em **Realtime Database → Regras**.
2. Substitua o conteúdo por:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> Isso deixa qualquer pessoa com o link ler e marcar itens — o que é o comportamento desejado para uma lista de presentes pública entre convidados. Não coloque informações sensíveis nesse banco.

Clique em **"Publicar"**.

## Passo 4 — Subir para o GitHub

1. Crie um repositório novo no GitHub (ex: `cha-da-erica`) — pode ser público ou privado, tanto faz para o Vercel.
2. Envie **a pasta inteira** (`index.html`, `vercel.json`, `.gitignore`, `README.md`, `assets/`, `icons/`) para o repositório, mantendo essa mesma estrutura de subpastas:
   ```bash
   git init
   git add .
   git commit -m "Lista de presentes"
   git branch -M main
   git remote add origin https://github.com/SEU_USUARIO/cha-da-erica.git
   git push -u origin main
   ```
   Ou arraste a pasta inteira pelo site do GitHub (botão "Add file → Upload files"), mantendo as subpastas.

## Passo 5 — Publicar no Vercel

1. Acesse **https://vercel.com** e faça login (pode entrar direto com sua conta do GitHub).
2. Clique em **"Add New..." → "Project"**.
3. Selecione o repositório `cha-da-erica` na lista (o Vercel pede autorização para acessar seus repositórios do GitHub na primeira vez — autorize).
4. Na tela de configuração do projeto:
   - **Framework Preset:** deixe em **"Other"** (é um site estático, sem build).
   - **Build Command:** deixe em branco.
   - **Output Directory:** deixe em branco (ou `.`).
5. Clique em **"Deploy"**. Em menos de um minuto o Vercel te dá um link público, algo como:
   `https://cha-da-erica.vercel.app`

Esse é o link que você compartilha com os convidados. A partir de agora, todo `git push` para o `main` atualiza o site automaticamente. 🎉

### Quer um domínio próprio (ex: chadaerica.com.br)?

No painel do projeto no Vercel, vá em **Settings → Domains**, digite o domínio que você comprou e siga as instruções de DNS que o próprio Vercel mostra (é mais simples que configurar no GitHub Pages, e o Vercel já cuida do certificado HTTPS). Isso é opcional — sem isso o link `.vercel.app` já funciona normalmente.

> O `vercel.json` incluído já configura cache de longa duração para os arquivos em `assets/` — não precisa mexer nele.

## Testando

Abra o link em duas abas (ou peça para alguém abrir no celular). Marque um item como escolhido em uma aba — a outra deve atualizar sozinha em segundos.

## Observações

- A lista inicial (itens e quem já reservou) está fixa no código, em `assets/js/app.js`, na variável `defaultItems`. Ela só é usada para popular o banco na primeira vez que alguém abre o site; depois disso, quem manda é o Firebase.
- Se quiser resetar tudo, vá em Firebase → Realtime Database → Dados, e apague o nó `presentes`. Na próxima visita, ele repopula com `defaultItems`.
- O plano gratuito (Spark) do Firebase é mais do que suficiente para esse uso.
