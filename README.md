# PLN - Classificação da Clareza de Respostas do SIC

## Grupo
- Ana Clara Segal Vidal Pessanha - Nº USP: 14677464
- Catarina Macedo Scabelli - Nº USP: 14215555
- Giovanna Almeida Albuquerque - Nº USP: 13687515
- Júlia Du Bois Araújo Silva - Nº USP: 14584360

## Sobre o projeto

Este projeto foi desenvolvido para a disciplina de **Processamento de Linguagem Natural (PLN)** e tem como objetivo classificar respostas de **Sistemas de Informação ao Cidadão (SIC)** de acordo com seu nível de clareza (`c1`, `c234` e `c5`).

Durante o desenvolvimento, foram exploradas diferentes abordagens de processamento e classificação de textos. A descrição detalhada dos modelos testados, experimentos e resultados pode ser encontrada no relatório do projeto.

### Modelo final (EP2)

O modelo final é um **stacking** composto por quatro modelos-base:

1. TF-IDF Word + Logistic Regression
2. Qwen3-Embedding-0.6B + Logistic Regression
3. Qwen3-Embedding-4B + Logistic Regression
4. BERTimbau Base Head-Tail original

As probabilidades dos quatro modelos são concatenadas,
produzindo 12 features para um meta-classificador de 
Regressão Logística.

---

## Estrutura do repositório

```text
plnEp2/
├── analiseCaracteristicas/
│   └── ...
├── data/
│   ├── train.csv
│   ├── train_limpo.csv
│   ├── test1.xlsx
│   ├── test1_limpo.csv
│   ├── test1_rotulado_ep1.xlsx
│   ├── test1-rotulado-para-teste-estatistico.xlsx
│   ├── test2.xlsx
│   └── test2_limpo.csv
│ 
├── McNemar/
│   └── ...
│ 
├── modelo_ep1/
│   └── ...
│ 
├── modelo_final/
│   └── ...
│ 
├── modelos_testados/
│   └── ...
│ 
├── resultados/
│   └── test2_rotulado.xlsx
│ 
├── Limpeza.ipynb
│ 
└── README.md
```

### `analiseCaracteristicas/`

Contém a análise de características dos dados e das predições do modelo final, incluindo a investigação dos casos mais difíceis de classificar.

### `data/`

Contém os conjuntos de dados utilizados no projeto:

- **Treino:** dados originais (`train.csv`), versões sem duplicatas preparadas pelo grupo (`train_noDuplicates.csv` e `train_noSameClassDuplicates.csv`) e a versão limpa (`train_limpo.csv`).
- **Teste 1:** conjunto do EP1 (`test1.xlsx`), sua versão limpa (`test1_limpo.csv`) e o arquivo com os rótulos reais, usado apenas para a comparação estatística com o EP1 (`test1-rotulado-para-teste-estatistico.xlsx`).
- **Teste 2:** conjunto da avaliação oficial do EP2 (`test2.xlsx`) e sua versão limpa (`test2_limpo.csv`).

Os arquivos `*_limpo.csv` são gerados pelo notebook `Limpeza.ipynb`.

### `McNemar/`

Contém o teste de McNemar que compara as predições do modelo do EP2 com as do modelo do EP1 no Teste 1, para verificar se o ganho de desempenho é estatisticamente significativo.

### `modelo_ep1/`

Contém o notebook e a avaliação do desempenho do **modelo selecionado como solução final no EP1**.

### `modelo_final/`

Contém os notebooks do **modelo final do EP2** (stacking descrito acima):

- `modeloFinal.ipynb`: pipeline completo do modelo final, com o treinamento dos modelos-base, o stacking e a avaliação no Teste 1.
- `modeloFinal_com_rotulacao_teste2.ipynb`: o mesmo pipeline acrescido da célula final que rotula o Teste 2.

### `modelos_testados/`

Contém os arquivos notebook de todos os modelos experimentados para o EP2, que não foram selecionados como o melhor modelo.

### `resultados/`

Contém os resultados entregues:

- `test2_rotulado.xlsx`: rótulos de clareza previstos pelo modelo final para cada instância do Teste 2 (colunas `resp_text` e `clarity`).

### `Limpeza.ipynb`

Pré-processamento dos textos de treino e teste: remoção de URLs, de menções e hashtags, de caracteres não alfanuméricos, de espaços em excesso e conversão para letras minúsculas.

---

## Relatório

A descrição completa da metodologia, modelos avaliados, experimentos realizados, métricas obtidas e da escolha do modelo final está disponível no **relatório do projeto**, bem como as instruções para reprodução do resultado.