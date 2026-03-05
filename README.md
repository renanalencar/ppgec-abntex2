# PPGEC-abnTeX2

Este projeto é um fork de [PPGEC-abnTeX2](https://github.com/victormelo/ppgec-abntex2) desenvolvido por [victormelo](https://github.com/victormelo)

Modelo LaTeX para dissertacoes de mestrado do **PPGEC** (Programa de Pos-Graduacao em Engenharia de Computacao) da **UPE** (Universidade de Pernambuco), baseado no [abnTeX2](https://github.com/abntex/abntex2) — classe padrao para documentos academicos em conformidade com as normas da ABNT.

## Pre-requisitos

- Distribuicao LaTeX completa ([TeX Live](https://www.tug.org/texlive/) ou [MiKTeX](https://miktex.org/))

> **Nota:** O pacote abnTeX2 ja esta incluido localmente no diretorio `abntex2/`, portanto **nao e necessario instala-lo separadamente**. A classe `ppgec-abntex2.cls` configura automaticamente o caminho para os arquivos locais.

## Estrutura do Projeto

```
ppgec-abntex2-modelo.tex   # Documento principal (ponto de entrada)
ppgec-abntex2.cls          # Classe customizada (capa, folha de rosto, rotulos)
referencias.bib            # Base de dados bibliografica (BibTeX)
Makefile                   # Alvo 'clean' para remover arquivos auxiliares

abntex2/                   # Copia local do abnTeX2 (classe, estilos, bib)
  latex/abntex2/           #   Classe base abntex2.cls e pacotes (.sty)
  bibtex/bst/abntex2/      #   Estilos bibliograficos (abntex2-alf, abntex2-num)
  bibtex/bib/abntex2/      #   Opcoes bibliograficas (abntex2-options.bib)

pretextuais/               # Elementos pre-textuais
  capa.tex                 #   Metadados (titulo, autor, orientador, instituicao)
  dedicatoria.tex          #   Dedicatoria
  agradecimentos.tex       #   Agradecimentos
  epigrafe.tex             #   Epigrafe
  resumos.tex              #   Resumos (portugues, ingles, frances, espanhol)
  siglasesimbolos.tex      #   Lista de siglas e simbolos

conteudo/                  # Capitulos
  introducao.tex           #   Introducao
  fundamentacao.tex        #   Fundamentacao teorica
  desenvolvimento.tex      #   Desenvolvimento (exemplos de uso do LaTeX/abnTeX2)
  resultados.tex           #   Resultados e discussao
  conclusao.tex            #   Conclusao

imagens/                   # Recursos de imagem
  brasao.pdf               #   Brasao da universidade (capa/folha de rosto)
  abntex2-modelo-img-*.pdf #   Figuras de exemplo
```

## Compilacao

### Compilacao completa (multi-passo, necessario para referencias cruzadas)

```bash
pdflatex ppgec-abntex2-modelo.tex
bibtex ppgec-abntex2-modelo.aux
makeindex ppgec-abntex2-modelo.idx
makeindex ppgec-abntex2-modelo.nlo -s nomencl.ist -o ppgec-abntex2-modelo.nls
pdflatex ppgec-abntex2-modelo.tex
pdflatex ppgec-abntex2-modelo.tex
```

### Usando latexmk (recomendado)

```bash
latexmk -pdf ppgec-abntex2-modelo.tex
```

### Verificacao rapida de sintaxe (passo unico, referencias quebradas)

```bash
pdflatex -interaction=nonstopmode -halt-on-error ppgec-abntex2-modelo.tex
```

### Limpeza de arquivos auxiliares

```bash
make clean
```

## Como Usar o Modelo

1. **Clone o repositorio**
   ```bash
   git clone https://github.com/renanalencar/ppgec-abntex2.git
   ```

2. **Edite os metadados** em `pretextuais/capa.tex` (titulo, autor, orientador, data)

3. **Escreva o conteudo** nos arquivos dentro de `conteudo/`

4. **Adicione referencias** em `referencias.bib` e cite com `\cite{chave}` ou `\citeonline{chave}`

5. **Compile** usando `latexmk -pdf ppgec-abntex2-modelo.tex`

### Adicionando um novo capitulo

1. Crie o arquivo `conteudo/<nomedocapitulo>.tex` (minusculo, sem acentos)
2. Inicie com `\chapter{Titulo}\label{cap_<nome>}`
3. Inclua no documento principal com `\include{conteudo/<nomedocapitulo>}` na secao textual de `ppgec-abntex2-modelo.tex`

## Configuracoes do Documento

| Configuracao | Valor |
|---|---|
| Tamanho da fonte | 12pt |
| Papel | A4 |
| Fonte | Times New Roman |
| Estilo de citacao | Numerico ABNT com colchetes |
| Recuo de paragrafo | 1.3cm |
| Espacamento entre paragrafos | 0.2cm |
| Idioma principal | Portugues (brasil) |

## Licenca

Este projeto esta licenciado sob a [LPPL v1.3+](https://www.latex-project.org/lppl/) (LaTeX Project Public License).

Baseado no modelo canonico do abnTeX2 (v1.9.6) de Lauro Cesar Araujo, customizado para o PPGEC-UPE.
