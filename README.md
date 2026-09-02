# Arqueiro vs Zumbis: 50 Fases

Jogo web (HTML/CSS/JS puro, sem dependências) pronto para publicar como
**PWA instalável** — funciona offline e pode ser adicionado à tela inicial
em Android, iOS e Desktop.

## Estrutura dos arquivos

```
index.html      → o jogo em si
manifest.json   → configuração do app instalável (nome, ícones, orientação)
sw.js           → service worker (cache offline)
icons/          → ícones do app em vários tamanhos
```

Todos os caminhos são relativos, então essa pasta pode ser publicada em
qualquer subpasta ou domínio sem precisar editar nada.

---

## 1. Publicar no GitHub Pages

1. Crie um repositório novo no GitHub (pode ser público ou privado, desde
   que o GitHub Pages esteja disponível no seu plano).
2. Envie **todos os arquivos desta pasta** para a raiz do repositório
   (mantendo a pasta `icons/` como está), por exemplo:
   ```bash
   git init
   git add .
   git commit -m "Primeira versão do jogo"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
   git push -u origin main
   ```
3. No GitHub, vá em **Settings → Pages**.
4. Em "Build and deployment", selecione **Deploy from a branch**, escolha a
   branch `main` e a pasta `/ (root)`. Salve.
5. Aguarde 1–2 minutos. O GitHub mostrará o endereço do site, algo como:
   ```
   https://SEU-USUARIO.github.io/SEU-REPOSITORIO/
   ```

O GitHub Pages já serve o site via **HTTPS**, o que é obrigatório para o
Service Worker e para a instalação como app funcionarem.

> Se preferir usar um domínio próprio, configure-o em Settings → Pages →
> Custom domain — nenhum outro arquivo precisa mudar.

---

## 2. Testar antes de publicar (opcional)

Service workers não funcionam abrindo o `index.html` direto pelo
`file://`. Para testar localmente, sirva a pasta com um servidor simples:

```bash
# Python 3
python3 -m http.server 8000
```

Depois abra `http://localhost:8000` no navegador.

---

## 3. Instalar em cada dispositivo

### Android (Chrome)
Abra o link do jogo → toque no menu (⋮) → **"Instalar app"** (ou aguarde o
banner automático de instalação aparecer). O ícone vai para a tela
inicial e o jogo abre em tela cheia, direto em modo paisagem.

### iPhone / iPad (Safari)
Abra o link do jogo → toque no ícone de **Compartilhar** (□↑) → **"Adicionar
à Tela de Início"**. O iOS não permite travar a orientação da tela via
navegador, então o jogo usa um recurso interno que **gira o conteúdo
automaticamente** para preencher a tela quando o aparelho está na vertical
— a experiência final ainda é em paisagem, mesmo sem suporte nativo do iOS.

### Desktop (Chrome / Edge)
Abra o link do jogo → clique no ícone de instalação (⊕) que aparece na
barra de endereço → **"Instalar"**. O jogo abre como uma janela própria,
sem as abas do navegador.

---

## 4. Sobre o modo paisagem/tela cheia

- Em **Android**, o jogo pede tela cheia real e tenta travar a orientação
  em paisagem automaticamente (usando as APIs `Fullscreen` e
  `Screen Orientation` do navegador) assim que o jogador toca em
  **"Iniciar Defesa"**.
- Em **iOS**, essas APIs de bloqueio de orientação não existem no Safari,
  então o jogo usa um efeito CSS que gira a tela do jogo para o formato
  paisagem mesmo com o iPhone na vertical. É um recurso "melhor esforço":
  funciona bem na maioria dos aparelhos, mas vale testar no seu modelo
  específico antes de divulgar o link.
- Os botões de flecha (Normal / Explosiva / Tripla) ficam numa faixa fixa
  **abaixo** da área de jogo, então nunca cobrem a cena.

---

## 5. Atualizando o jogo depois de publicado

Sempre que enviar uma nova versão do `index.html`, aumente o número da
versão do cache em `sw.js` (linha `CACHE_NAME`), por exemplo de
`arqueiro-vs-zumbis-v1` para `arqueiro-vs-zumbis-v2`. Isso garante que os
dispositivos que já instalaram o app baixem a versão nova em vez de
continuar usando o cache antigo.
