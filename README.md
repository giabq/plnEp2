# PLN - Classificação da Clareza de Respostas do SIC

## Grupo
- Ana Clara Segal Vidal Pessanha - Nº USP: 14677464
- Catarina Macedo Scabelli - Nº USP: 14215555
- Giovanna Almeida Albuquerque - Nº USP: 13687515
- Júlia Du Bois Araújo Silva - Nº USP: 14584360

## Sobre o projeto

Este projeto foi desenvolvido para a disciplina de **Processamento de Linguagem Natural (PLN)** e tem como objetivo classificar respostas de **Sistemas de Informação ao Cidadão (SIC)** de acordo com seu nível de clareza.

Durante o desenvolvimento, foram exploradas diferentes abordagens de processamento e classificação de textos. A descrição detalhada dos modelos testados, experimentos e resultados pode ser encontrada no relatório do projeto.

<!-- --- -->

## Estrutura do repositório

```text
plnEp2/
├── data/
│   ├── train.csv
│   ├── train_noDuplicates.csv
│   ├── train_noSameClassDuplicates.csv
│   └── test1.xlsx
│   └── test2.xlsx
│   └── test1-rotulado-para-teste-estatistico.xlsx
├── modelo_ep1/
│   └── ...
├── modelos_testados/
│   └── ...
└── Limpeza.ipynb
└── README.md
└── ...


```

### `data/`

Contém os conjuntos de dados utilizados no projeto, incluindo os dados de treinamento originais, os dados sem duplicatas preparados pelo grupo e o conjunto de teste fornecido para classificação.


### `modelo_ep1/`

Contém o notebook e os arquivos relacionados ao **modelo selecionado como solução final no EP1**.

### `modelos_testados/`

Contém os arquivos notebook de todos os modelos experimentados para o ep2, que não foram selecionados como o melhor modelo.

### `Limpeza.ipynb`

Pré-processamento dos textos de treino e teste: remoção de URLs, de menções e hashtags, de caracteres não alfanuméricos, de espaços em excesso e conversão para letras minúsculas

<!-- --- -->

## Relatório

A descrição completa da metodologia, modelos avaliados, experimentos realizados, métricas obtidas e da escolha do modelo final está disponível no **relatório do projeto**, bem como as intruções para reprodução do resultado.
