# Graninha Bot Render - Hospedagem no Render.com

## 📦 Estrutura do Projeto

Este projeto combina a interface do **Graninha Bot v3.1** com a infraestrutura de deployment do **Young Money Tasks** para hospedar no Render.com.

### Arquivos Principais

```
graninha-bot-render/
├── server/
│   └── index.js              # Backend Express com proxy para API Graninha
├── public/
│   ├── index.html            # Interface do bot
│   ├── style.css             # Estilos
│   ├── script_optimized.js   # Script otimizado do bot
│   └── assets/               # Recursos estáticos
├── package.json              # Dependências Node.js
├── render.yaml               # Configuração de deployment Render
└── README.md                 # Este arquivo
```

## 🚀 Como Fazer Deploy no Render.com

### Pré-requisitos

- Conta no GitHub com repositório criado
- Conta no Render.com (https://render.com)
- Git instalado localmente

### Passo 1: Preparar o Repositório GitHub

1. Crie um novo repositório no GitHub (ex: `graninha-bot-render`)
2. Clone o repositório localmente:
   ```bash
   git clone https://github.com/seu-usuario/graninha-bot-render.git
   cd graninha-bot-render
   ```

3. Copie todos os arquivos do projeto para a pasta
4. Faça o commit e push:
   ```bash
   git add .
   git commit -m "Initial commit: Graninha Bot Render"
   git push origin main
   ```

### Passo 2: Conectar ao Render.com

1. Acesse [https://dashboard.render.com](https://dashboard.render.com)
2. Clique em **"New +"** → **"Web Service"**
3. Selecione **"Deploy an existing repository from GitHub"**
4. Conecte sua conta GitHub (se não estiver conectada)
5. Selecione o repositório `graninha-bot-render`
6. Configure as seguintes opções:

| Campo | Valor |
|-------|-------|
| **Name** | `graninha-bot-render` |
| **Environment** | `Node` |
| **Build Command** | `npm install` |
| **Start Command** | `npm start` |
| **Plan** | `Free` (ou pago, conforme necessário) |

### Passo 3: Configurar Variáveis de Ambiente

No Render Dashboard, vá para **Environment** e adicione as seguintes variáveis:

```
NODE_ENV = production
PORT = 3000
CORS_ORIGIN = *
GRANINHA_API_URL = https://painel.graninha.com.br/api/v1
```

### Passo 4: Deploy

1. Clique em **"Create Web Service"**
2. O Render iniciará o build automaticamente
3. Aguarde a conclusão (geralmente 2-5 minutos)
4. Acesse a URL gerada (ex: `https://graninha-bot-render.onrender.com`)

## 🔄 Atualizações Automáticas

Após o deployment inicial, qualquer push para o repositório GitHub acionará um novo build automaticamente no Render.

```bash
# Para atualizar o projeto
git add .
git commit -m "Update: descrição das mudanças"
git push origin main
```

## 📝 Variáveis de Ambiente

### NODE_ENV
- **Produção**: `production`
- **Desenvolvimento**: `development`

### PORT
- Porta padrão: `3000`
- Render atribui automaticamente

### CORS_ORIGIN
- Permite requisições de qualquer origem: `*`
- Para restringir: `https://seu-dominio.com`

### GRANINHA_API_URL
- URL da API Graninha: `https://painel.graninha.com.br/api/v1`
- Pode ser alterada se necessário

## 🤖 Endpoints Disponíveis

O servidor proxy fornece os seguintes endpoints:

### GET /health
Verifica o status do servidor.

```bash
curl https://graninha-bot-render.onrender.com/health
```

### POST /api/get_user
Obtém informações do usuário.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/get_user \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token"}'
```

### POST /api/play_scratch
Joga uma raspadinha.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/play_scratch \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token", "ex_id": "seu_ex_id"}'
```

### POST /api/play_roulette
Joga a roleta.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/play_roulette \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token", "ex_id": "seu_ex_id"}'
```

### POST /api/play_quiz
Responde um quiz.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/play_quiz \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token", "ex_id": "seu_ex_id"}'
```

### POST /api/play_game
Joga um dos 33 jogos disponíveis.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/play_game \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token", "ex_id": "seu_ex_id", "game_id": 1}'
```

### POST /api/get_spin
Obtém o resultado do spin da roleta.

```bash
curl -X POST https://graninha-bot-render.onrender.com/api/get_spin \
  -H "Content-Type: application/json" \
  -d '{"bearer_token": "seu_token", "ex_id": "seu_ex_id"}'
```

## 🌐 Acessar a Interface

Após o deploy, acesse a URL do Render no navegador:

```
https://graninha-bot-render.onrender.com
```

A interface do Graninha Bot estará disponível com:

1. **Configuração**: Insira seu Bearer Token e EX ID
2. **Status**: Acompanhe saldo, ganhos e ações executadas
3. **Logs**: Veja todos os eventos em tempo real
4. **Loop Infinito**: Ative para execução contínua

## 🔧 Solução de Problemas

### Erro: "Build failed"
- Verifique se o `package.json` está correto
- Confirme que o `render.yaml` está no root do projeto
- Verifique os logs no Render Dashboard

### Erro: "Port already in use"
- O Render atribui a porta automaticamente
- Não é necessário configurar manualmente

### Erro: "CORS Error"
- Verifique se `CORS_ORIGIN` está configurado como `*`
- Limpe o cache do navegador
- Tente em uma aba anônima

### Bot não conecta à API
- Verifique o Bearer Token (tokens expiram)
- Confirme que o EX ID está correto
- Verifique os logs do servidor no Render Dashboard

## 📊 Monitoramento

No Render Dashboard, você pode:

1. **Ver Logs**: Em tempo real do servidor
2. **Monitorar Performance**: CPU, memória, requisições
3. **Gerenciar Deploys**: Histórico e rollback
4. **Configurar Alertas**: Notificações de erros

## 🔐 Segurança

- Nunca compartilhe seu Bearer Token
- Use variáveis de ambiente para dados sensíveis
- Mantenha o repositório privado se necessário
- Revise os logs regularmente

## 📞 Suporte

Se encontrar problemas:

1. Verifique os logs no Render Dashboard
2. Abra o console do navegador (F12)
3. Verifique a conectividade com a API Graninha
4. Confirme que todas as variáveis de ambiente estão configuradas

## 📄 Licença

MIT

## 🎯 Versão

- **Graninha Bot**: v3.1
- **Young Money Template**: Render Ready
- **Data**: Dezembro 2025
- **Status**: ✅ Pronto para Deploy

---

**Desenvolvido com ❤️ para automação legítima do Graninha Bot**
