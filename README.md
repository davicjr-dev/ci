# ci

Checagens que os repositórios desta conta herdam, definidas num lugar só.

Um repositório não copia estas checagens: ele as chama. Mudar uma regra aqui muda em todos
de uma vez, e nenhum repositório carrega cópia que possa envelhecer.

## Como usar

```yaml
# .github/workflows/ci.yml do repositório que chama
name: ci

on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

jobs:
  ci:
    uses: davicjr-dev/ci/.github/workflows/ci.yml@main
    with:
      natureza: codigo   # codigo | dados | conteudo | ferramental
      executor: davi-ci  # opcional; usa o runner efêmero local do repositório
```

Sem `executor`, o contrato continua usando `ubuntu-latest`. Como os repositórios
pertencem a uma conta pessoal, cada repositório privado que informar `davi-ci`
precisa ter seu próprio runner registrado com esse rótulo.

## O que roda

Em qualquer natureza: varredura de segredo no histórico completo, README que se sustenta
sozinho e aviso de arquivo grande versionado.

Em `conteudo`: frontmatter válido e link interno que não aponta para arquivo inexistente.
Texto não tem build nem teste — o que quebra em repositório de conteúdo é link morto.

Nas outras: detecção de Python e de Node pelos manifestos, com lint e teste do ecossistema
encontrado. Stack ausente não falha, avisa que pulou — melhor um aviso honesto que um passo
verde que não verificou nada.

## Por que um job só

Minuto de Actions em repositório privado é arredondado para cima por job. Quatro checagens
em quatro jobs custam quatro minutos, mesmo levando quinze segundos cada. Em sequência,
dentro de um job, custam um.
