# atualia-site

Site estático do Atualia, o bot de artigos médicos no Telegram. Fica no GitHub Pages, em vozes.atualia.com.br (sem o domínio, em robertodermato.github.io/atualia-site). Quem decide o conteúdo e o visual é o Roberto, dermatologista e dono do projeto; ele não é programador de front-end, então explique cada passo antes de rodar comando, em português.

## O que existe

- `index.html` - a página das vozes: a amostra de cada voz ativa do bot, com o mesmo trecho, em faixas de custo e, dentro delas, por família, com o gênero, o custo em cifrões, a nota do curador e o selo das vozes em fase de teste.
- `vozes.json` e `amostras/` - os dados que o bot grava, na raiz, junto da página.
- `_config.yml` - deixa o CLAUDE.md fora do site publicado.

## Regras do site

- Português do Brasil, com acento. Texto curto e direto.
- HTML, CSS e JavaScript puros, sem framework e sem etapa de build: o GitHub Pages serve os arquivos como estão.
- Leve e bom no celular primeiro; abre rápido mesmo com muitas vozes.
- Acessível: cada player com rótulo, contraste bom, navegação por teclado.
- Nada de rastreador, analytics ou recurso de terceiros sem o Roberto pedir.

## O que o bot escreve - nunca editar à mão

- `vozes.json` e `amostras/*.mp3`, na raiz do repositório, são gravados pelo bot artigos, pela API do GitHub, numa rodada diária e quando uma voz muda. Edição à mão nesses arquivos some na rodada seguinte.
- O bot faz commits neste repositório, e o GitHub também (o do `CNAME`). Antes de mudar qualquer coisa, rode `git pull`; antes de publicar, `git pull` de novo.
- Enquanto o bot não publica a primeira lista, `vozes.json` é um exemplo, marcado com `"exemplo": true`.

## O contrato do vozes.json (versão 1)

```json
{
  "versao": 1,
  "exemplo": true,
  "gerado_em": "2026-11-19T12:00:00-03:00",
  "texto_amostra": "o trecho que todas as vozes leem",
  "grupos": [
    { "grupo": "economica", "rotulo": "Muito econômica", "cifroes": "$" }
  ],
  "vozes": [
    {
      "id": 37,
      "rotulo": "Standard-B",
      "familia": "Standard",
      "provedor": "google",
      "genero": "M",
      "idioma": "pt-BR",
      "grupo": "economica",
      "cifroes": "$",
      "nota_curador": null,
      "em_teste": false,
      "amostra": "amostras/37.mp3"
    }
  ]
}
```

- `versao`: muda quando o formato muda; a página confere e avisa se não conhece a versão.
- `exemplo`: só no arquivo de exemplo; com ele verdadeiro, a página mostra um aviso discreto de que os dados são de exemplo.
- `gerado_em`: quando o bot gerou a lista, com fuso.
- `texto_amostra`: o trecho que todas as vozes narram; a página o mostra uma vez, acima das vozes.
- `grupos`: os grupos de custo, já em ordem do mais barato para o mais caro; `rotulo` é o nome do custo (Muito econômica, Econômica, Intermediária, Cara e Muito cara, desde 19/11/2026), e `cifroes` vai de `$` a `$$$$$`. Quanto mais cifrões, mais créditos o áudio gasta. A legenda dos cifrões e o título de cada faixa saem daqui.
- `vozes`: só as vozes ativas, na ordem em que a página deve mostrar: por grupo de custo, por família em ordem alfabética e pela ordem da voz.
  - `rotulo`: o nome da voz para quem lê.
  - `familia`: a família da voz (Standard, Chirp 3, Gemini Pro, Fish Audio...).
  - `provedor`: a chave técnica do provedor; mostrar pequeno, ou não mostrar.
  - `genero`: `F` ou `M`.
  - `nota_curador`: a nota do curador, ou `null`; quando houver, aparece marcada como nota do curador.
  - `em_teste`: `true` para as vozes que, no bot, só o plano God usa, de qualquer grupo de custo; `false` para as outras. Com ele verdadeiro, o cartão mostra o selo "Em fase de teste"; os filtros valem para essas vozes como para as outras.
  - `amostra`: o caminho do MP3, relativo à raiz do repositório, onde fica o `vozes.json` (por exemplo, `amostras/37.mp3`).

## A página das vozes

- Fica na raiz, junto do `vozes.json` e da pasta `amostras/`. Usa só caminhos relativos, sem barra no começo, para funcionar em vozes.atualia.com.br e em robertodermato.github.io/atualia-site.
- Uma faixa por grupo de custo, na ordem de `grupos`, separada da anterior por uma linha: no título, os cifrões, o nome do custo e quantas vozes a faixa tem. Voz de grupo que não está em `grupos` vai para a faixa "Outras vozes", no fim, e não some.
- Dentro da faixa, um subtítulo por família, na ordem do arquivo, e um cartão por voz: rótulo, gênero, cifrões, o selo "Em fase de teste" quando `em_teste` for verdadeiro, a nota do curador quando houver, e o player da amostra com `preload="none"`. A família fica no subtítulo, não no cartão.
- Mostra o trecho da amostra e a legenda dos cifrões.
- Filtros simples por gênero e por grupo de custo; as vozes de teste entram neles como as outras. O filtro esconde a família e a faixa que ficam sem voz, e a conta da faixa acompanha.
- Se o JSON não carregar, ou se um MP3 não existir, uma mensagem curta no lugar, sem quebrar a página.
- Para ver no computador, a página precisa de um servidor local: o navegador bloqueia a leitura do JSON aberta direto do arquivo. Use o que já existir na máquina antes de instalar qualquer coisa, e pergunte ao Roberto antes de instalar.

## Publicação

- Repositório `github.com/robertodermato/atualia-site` (público), publicado pelo GitHub Pages a partir da branch `main`, pasta raiz.
- A página fica em vozes.atualia.com.br e, sem o domínio, em robertodermato.github.io/atualia-site.
- O arquivo `CNAME` quem cria é o GitHub, quando o Roberto põe o domínio nas configurações do Pages; por isso o `git pull` vem antes de qualquer mudança. Nunca crie o `CNAME` à mão.
- atualia.com.br, www.atualia.com.br, atualia.app e www.atualia.app são do web app Atualia, na VPS de sites, e nunca apontam para o GitHub Pages.
- O DNS fica no Cloudflare, aos cuidados do chat de infra (inclusive o registro de vozes.atualia.com.br), e este repositório não mexe nele.
- O bot escreve no repositório com um token que só tem acesso a ele; o token não fica neste repositório.

## De onde vêm as mudanças

- Visual e textos da página: aqui, com o Roberto.
- Formato do vozes.json, quais vozes entram e o trecho da amostra: no bot artigos, no chat do projeto; só depois a página muda para acompanhar.
