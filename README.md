# Ginestal Machado — React SPA

Aplicação migrada de PHP para uma SPA em React. Pode ser alojada em ambientes estáticos (Netlify, Vercel, GitHub Pages).

O projeto usa localStorage como 'DB' para persistência simples (ou Supabase, se configurar as chaves). Para começar:

- Instalar dependências: `npm install`
- Executar em desenvolvimento: `npm run dev`
- Build: `npm run build`

Notas:
- Componentes principais: `src/pages/*`
- Funções de dados: `src/utils/db.js` (fallback para localStorage; com `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY` usa Supabase)
 - Rotas SPA definidas em `src/App.jsx`

Deploy no Netlify
1. Faça push do repositório para GitHub.
2. No Netlify, crie um novo site a partir do repositório.
3. Build command: `npm run build` e publish directory: `dist`.
4. Para suportar SPA, já existe `public/_redirects` com `/* /index.html 200`.

Se quiser um backend:
- Use Netlify Functions, Vercel Serverless ou um pequeno API (Express) para CRUD sobre MySQL/Postgres. Atualize `src/utils/db.js` para chamar a API.

Estado da migração:
- Navegação, pesquisa, registos (livro, autor, utente, género, editora, exemplar, empréstimo) e listagens portadas para React.
- Páginas PHP antigas permanecem no repositório apenas como referência e podem ser removidas após validação.

PWA / App-like
----------------

O projecto inclui um `manifest.json` e um `service-worker.js` no `public/`.

Como testar localmente:

1. `npm install`
2. `npm run dev`
3. Abrir `http://localhost:5173` no Chrome.

Instalar no Chrome/Edge: clicar no botão "Install" na omnibox ou no menu do navegador > Install app.

Auditoria PWA: abrir DevTools > Lighthouse > Run audit (seleccionar PWA checklist).

Configurar Supabase (produção/local)
-----------------------------------

Se pretende usar Supabase em vez do localStorage, configure as variáveis de ambiente com as suas chaves (NUNCA comite as chaves no repositório):

- Local (desenvolvimento): crie um ficheiro `.env.local` na raiz e adicione:

```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=eyJ... (sua anon key)
```

- Netlify (produção): no painel do site em Netlify, vá a Site settings -> Build & deploy -> Environment -> Edit variables e adicione `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`. Depois re-deploy do site.

Notas de segurança:

- A `VITE_SUPABASE_ANON_KEY` é pensada para chamadas cliente (anon), mas assegure que definiu regras RLS (Row Level Security) no Supabase para limitar operações sensíveis.
- Nunca comite `.env.local` ou chaves API no repositório público. `.env.local` já está listado em `.gitignore`.

Testar a ligação localmente:

1. Adicione `.env.local` com as variáveis.
2. `npm install` (se ainda não o fez)
3. `npm run dev`
4. Abrir `http://localhost:5173` e testar funções de criar/listar livros. Se as chamadas falharem, verifique o console para mensagens de erro do Supabase.



