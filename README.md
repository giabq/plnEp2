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
plnEp1/
├── data/
│   ├── train.csv
│   ├── train_noDuplicates.csv
│   ├── train_noSameClassDuplicates.csv
│   └── test1.xlsx
│
├── baseline/
│   └── ...
│
├── modelos_testados/
│   └── ...
│
├── modelo_final/
│   └── ...
│
├── resultados/
│   └── ...
│
└── README.md
```

### `data/`

Contém os conjuntos de dados utilizados no projeto, incluindo os dados de treinamento originais, os dados sem duplicatas preparados pelo grupo e o conjunto de teste fornecido para classificação.

### `baseline/`

Contém os notebooks e arquivos relacionados aos modelos utilizados como **baseline** (Dummy Classifier e TF-IDF + Regressão Logística) para comparação.

### `modelos_testados/`

Contém os notebooks das diferentes abordagens exploradas durante o desenvolvimento do projeto.

### `modelo_final/`

Contém o notebook e os arquivos relacionados ao **modelo selecionado como solução final**.

### `resultados/`

Contém os resultados gerados pelo modelo final, incluindo o conjunto de teste rotulado após a classificação.

<!-- --- -->

## Modelo final

O modelo final selecionado para a classificação das respostas foi:

**TF-IDF + sublinear_tf + Regressão Logística**

O treinamento, a avaliação e a aplicação do modelo podem ser encontrados no notebook disponível em: `modelo_final/`

Após o treinamento e a avaliação, o modelo foi utilizado para classificar as respostas presentes no conjunto `test1.xlsx`.

O arquivo resultante está disponível em: `resultados/test1_rotulado.xlsx`

<!-- Os resultados detalhados dos experimentos e a justificativa para a escolha do modelo final estão apresentados no relatório do projeto. -->

<!-- --- -->

## Relatório

A descrição completa da metodologia, modelos avaliados, experimentos realizados, métricas obtidas e da escolha do modelo final está disponível no **relatório do projeto**, bem como as intruções para reprodução do resultado.
