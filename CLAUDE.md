# atualia-site

Site estático do Atualia, o bot de artigos médicos no Telegram. Fica no GitHub Pages, no domínio atualia.com.br. Quem decide o conteúdo e o visual é o Roberto, dermatologista e dono do projeto; ele não é programador de front-end, então explique cada passo antes de rodar comando, em português.

## O que existe

- `vozes/` - a página das vozes (atualia.com.br/vozes): a amostra de cada voz ativa do bot, com o mesmo trecho, a família, o gênero, o custo em cifrões e a nota do curador.

## Regras do site

- Português do Brasil, com acento. Texto curto e direto.
- HTML, CSS e JavaScript puros, sem framework e sem etapa de build: o GitHub Pages serve os arquivos como estão.
- Leve e bom no celular primeiro; abre rápido mesmo com muitas vozes.
- Acessível: cada player com rótulo, contraste bom, navegação por teclado.
- Nada de rastreador, analytics ou recurso de terceiros sem o Roberto pedir.

## O que o bot escreve - nunca editar à mão

- `vozes/vozes.json` e `vozes/amostras/*.mp3` são gravados pelo bot artigos, pela API do GitHub, numa rodada diária e quando uma voz muda. Edição à mão nesses arquivos some na rodada seguinte.
- O bot faz commits neste repositório. Antes de mudar qualquer coisa, rode `git pull`; antes de publicar, `git pull` de novo.
- Enquanto o bot não publica a primeira lista, `vozes/vozes.json` é um exemplo, marcado com `"exemplo": true`.

## O contrato do vozes.json (versão 1)

```json
{
  "versao": 1,
  "exemplo": true,
  "gerado_em": "2026-11-19T12:00:00-03:00",
  "texto_amostra": "o trecho que todas as vozes leem",
  "grupos": [
    { "grupo": "economica", "rotulo": "Econômica", "cifroes": "$" }
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
      "amostra": "amostras/37.mp3"
    }
  ]
}
```

- `versao`: muda quando o formato muda; a página confere e avisa se não conhece a versão.
- `exemplo`: só no arquivo de exemplo; com ele verdadeiro, a página mostra um aviso discreto de que os dados são de exemplo.
- `gerado_em`: quando o bot gerou a lista, com fuso.
- `texto_amostra`: o trecho que todas as vozes narram; a página o mostra uma vez, acima das vozes.
- `grupos`: os grupos de custo, já em ordem do mais barato para o mais caro; `cifroes` vai de `$` a `$$$$`. Quanto mais cifrões, mais créditos o áudio gasta. A legenda dos cifrões sai daqui.
- `vozes`: só as vozes ativas, na ordem em que a página deve mostrar.
  - `rotulo`: o nome da voz para quem lê.
  - `familia`: a família da voz (Standard, Chirp 3, Gemini Pro, Fish Audio...).
  - `provedor`: a chave técnica do provedor; mostrar pequeno, ou não mostrar.
  - `genero`: `F` ou `M`.
  - `nota_curador`: a nota do curador, ou `null`; quando houver, aparece marcada como nota do curador.
  - `amostra`: o caminho do MP3, relativo à pasta `vozes/`.

## A página das vozes

- Lê `vozes.json` da própria pasta, com caminho relativo.
- Um cartão por voz, na ordem do arquivo: rótulo, família, gênero, cifrões, a nota do curador quando houver, e o player da amostra com `preload="none"`.
- Mostra o trecho da amostra e a legenda dos cifrões.
- Filtros simples por gênero e por grupo de custo.
- Se o JSON não carregar, ou se um MP3 não existir, uma mensagem curta no lugar, sem quebrar a página.
- Para ver no computador, a página precisa de um servidor local: o navegador bloqueia a leitura do JSON aberta direto do arquivo. Use o que já existir na máquina antes de instalar qualquer coisa, e pergunte ao Roberto antes de instalar.

## Publicação

- Repositório no GitHub, publicado pelo GitHub Pages a partir da branch `main`, pasta raiz.
- Domínio atualia.com.br: arquivo `CNAME` na raiz com `atualia.com.br`, e o DNS no registro.br apontando para o GitHub Pages. Confira na documentação atual do GitHub os registros de DNS antes de passar ao Roberto.
- O bot escreve no repositório com um token que só tem acesso a ele; o token não fica neste repositório.

## De onde vêm as mudanças

- Visual e textos da página: aqui, com o Roberto.
- Formato do vozes.json, quais vozes entram e o trecho da amostra: no bot artigos, no chat do projeto; só depois a página muda para acompanhar.
