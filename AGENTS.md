# AGENTS.md

Portfólio estático em HTML/CSS/JS (Davi Miguel), em português (`pt-BR` — conteúdo e atributos `lang`). Sem passo de build, sem package.json, sem testes, sem CI.

## Estrutura
- Todo o fonte fica em `portifolio/` (6 páginas: `index`, `sobre`, `projetos`, `habilidades`, `certificacoes`, `contato`; mais `css/style.css`, `js/main.js`, `assets/`).
- Ponto de entrada: `portifolio/index.html`. Visualize abrindo esse arquivo direto no navegador — não precisa servidor, instalação nem build.
- Os caminhos entre páginas e assets são relativos e resolvidos dentro de `portifolio/`. Não mova arquivos de lugar sem ajustar os caminhos.

## Tema claro/escuro
- `js/main.js` grava `data-theme="dark"|"light"` em `<html>` e persiste no `localStorage` com a chave `dm-theme`. Na primeira visita (sem valor salvo), o tema escuro segue `prefers-color-scheme`.
- Use as variáveis CSS `--*` de `:root` / `[data-theme="dark"]` em vez de cores fixas.

## Animações `.reveal` (guard de progressive enhancement)
- O CSS esconde `.reveal` **apenas sob `.js`** (`.js .reveal { opacity: 0 }`), porque um `<script>` inline em cada `<head>` adiciona `document.documentElement.classList.add("js")`.
- Nunca estilize `.reveal` fora do escopo `.js` — sem isso, usuários sem JS veriam conteúdo invisível. Não remova o script inline nem a classe `.reveal` de conteúdos animados.
- `IntersectionObserver` adiciona `.in` — é só assim que o fade dispara. Um fallback em `window.load` exibe o que estiver visível após 400 ms.

## Filtros (projetos e certificações)
- Botões `.filter-chip` com `data-category`; cards com `[data-category]`. Categorias válidas — projetos: `Todas`/`Todos` (ativa por padrão), `Tecnologia`, `Edição`, `UI/UX`, `Criação`, `Projetos de Pesquisa e Extensão`; certificações: `Todas` (ativa por padrão), `Tecnologia`, `Desenvolvimento Profissional e Pessoal`, `Design`, `Formação Acadêmica`.
- Os IDs `NN / Categoria` dos cards de projeto são **totalmente automáticos** (bloco em `js/main.js`): o número vem da ordem no DOM e a categoria vem do `data-category` do card. O primeiro `.card-meta span` fica **vazio** no HTML — nunca escreva número nem categoria nele.
- Ao adicionar cards, inclua sempre o `data-category` — cards sem o atributo desaparecem ao filtrar.
- "Todas" embaralha a ordem dos cards a cada aplicação do filtro; clicar num chip já ativo volta para "Todas". O botão `#removeFilter` (o `×`) faz o mesmo. Mantenha esses comportamentos.

## Pontos de atenção
- `js/main.js` define a função `aplicarFiltro` duas vezes (a segunda sobrescreve a primeira). Só a segunda roda — não perca tempo editando a cópia inútil; um refactor pode remover a primeira.
- `certificacoes.html` (linha 39) linka `Curriculo_Davi_Miguel.pdf`, que não existe. O PDF real é `assets/Curriculo_DaviMiguel.pdf` (usado corretamente nas outras 5 páginas). Link quebrado pré-existente — padronize ao tocar nesse trecho.
- Em `index.html`, os botões "Baixar currículo" (hero) e "Ver estudo de caso" apontam para `#` — placeholders pré-existentes.
- Contato usa apenas links de e-mail e WhatsApp (sem backend de formulário). O card de e-mail em `contato.html` abre a janela "Escrever" do Gmail com o destinatário preenchido; `index.html` ainda usa `mailto:contato@davimiguel.com`. WhatsApp: `wa.me/5521996778007` com mensagem pré-preenchida ("Olá! Vim do seu portfólio...") — botão ao lado de "Começar uma conversa" no `index.html` e linha `social-row` no `contato.html`. E-mails diferem entre páginas: `contato@davimiguel.com` (index) e `davimiguel.tech@gmail.com` (contato).
- As fontes (Google Fonts) carregam do CDN — requer rede.
- `projetos.html` linka para a demo externa do Éden em `eden.gabsprojects.uk`.

## Comandos
Nenhum. Não há toolchain de teste/lint/build. Não invente comandos `npm` nem CI.
