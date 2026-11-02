# Random Forest com características linguísticas

## Motivação

Testamos uma Random Forest sobre características linguísticas explícitas, em vez de representações do tipo bag-of-words. O modelo foi escolhido por dois motivos: (i) ele capta relações não lineares e interações entre propriedades do texto e a clareza da resposta; (ii) ele permite analisar a importância de cada característica, o que torna o classificador interpretável.

## Pré-processamento

A limpeza usada no EP1 converte o texto para minúsculas e remove toda a pontuação, o que apaga informações necessárias aqui, como os limites de sentença, as vírgulas e o uso de maiúsculas. Por isso, criamos uma limpeza básica alternativa a partir dos textos originais. Ela remove URLs, e-mails, menções e espaços redundantes, mas **preserva a pontuação e a capitalização**. A limpeza do EP1 não foi alterada, para manter seus resultados reprodutíveis. As instâncias duplicadas do treino foram mantidas, como no EP1.

## Representação: engenharia de características

Diferentemente do EP1, em que a representação TF-IDF é gerada automaticamente (cada palavra vira uma coluna e o modelo descobre quais importam), aqui adotamos **engenharia de características** (*feature engineering*): cada resposta é descrita por um vetor pequeno (48 colunas) de propriedades definidas a partir de hipóteses sobre o que torna uma resposta clara. Os três conjuntos diferem quanto ao grau de decisão manual:

| Conjunto | Nº | Como foi definido | Exemplos |
|---|---|---|---|
| Estilo e legibilidade | 24 | **Manual**, a partir de hipóteses do grupo e da literatura | nº de palavras e de sentenças; palavras por sentença (média e máximo); tamanho médio da palavra; sílabas por palavra; índice de Flesch adaptado ao português (Martins et al., 1996); diversidade vocabular (TTR, TTR raiz, hapax); proporção de palavras raras no treino; conectivos; negações; pontuação; maiúsculas; dígitos |
| Grupos lexicais | 15 | **Manual**, a partir da observação do domínio (respostas do e-SIC) | proporção de expressões ligadas a anexos, links, encaminhamento a outro órgão, inexistência da informação, sigilo/legislação, fórmulas de resposta e de cortesia, referência ao pedido, voz passiva, gerúndio, verbos modais |
| Léxico de clareza | 9 | **Aprendido dos dados**; só o formato foi definido manualmente | para cada classe, média, máximo e mínimo do *log-odds* suavizado das palavras do texto |

- **Estilo e legibilidade:** as ideias gerais (comprimento de sentenças, diversidade vocabular, palavras incomuns, conectivos, pontuação) foram traduzidas em regras concretas escolhidas por nós. Por exemplo, sentenças são delimitadas por `.`, `!`, `?` e `;`; "palavra longa" tem 10 letras ou mais; "palavra rara" aparece em menos de 5 documentos do treino; e a lista de conectivos foi definida manualmente.
- **Grupos lexicais:** as categorias e as expressões de cada uma (por exemplo, "anexo", "encaminhamos", "não possui", "sigilo") foram escolhidas manualmente, com base na intuição sobre as funções discursivas típicas das respostas. Não se trata de um levantamento sistemático, mas de hipóteses testadas pelo modelo.
- **Léxico de clareza:** nenhuma palavra foi escolhida manualmente. Para cada palavra do vocabulário, calcula-se a partir dos rótulos do treino o quanto ela é mais frequente nas respostas de cada classe do que nas demais (*log-odds* suavizado). Apenas a forma de resumir esses valores por texto (média, máximo e mínimo) foi decisão nossa. Como depende dos rótulos, o léxico foi implementado como um *transformer* dentro de um `Pipeline`: em cada fold da validação cruzada, ele é aprendido apenas com os dados de treino daquele fold, o que evita vazamento de informação.

A validação cruzada (apenas no treino) também foi usada para decidir quais conjuntos manter. Por exemplo, um conjunto de características estruturais (contagem de URLs, e-mails e aspas) foi descartado por não melhorar a acurácia.

A vantagem dessa abordagem é a **interpretabilidade**: cada coluna tem significado explícito, o que permite verificar, pela análise de importância, quais hipóteses se confirmam. A desvantagem é que o desempenho fica limitado pelas hipóteses de quem projetou as características.

## Experimentos

Os hiperparâmetros foram escolhidos por *grid search* com validação cruzada estratificada de 5 folds no conjunto de treino, usando a acurácia como métrica.

- **Experimento 1 (24 características de estilo e legibilidade):** foram 18 combinações de `max_depth`, `min_samples_leaf` e `class_weight`. Um teste preliminar também variou `max_features`. Todas as combinações ficaram entre 0,418 e 0,423, o que indica que o limite estava na representação, e não nos hiperparâmetros.
- **Experimento 2 (48 características):** foram 192 combinações, envolvendo os parâmetros do léxico (`min_freq` ∈ {3, 10}, `alpha` ∈ {0,5; 1,0}) e da Random Forest (`n_estimators`, `max_depth`, `min_samples_leaf`, `max_features`, `class_weight`). A melhor configuração foi `min_freq=10`, `alpha=0,5`, 300 árvores, `max_depth=20`, `min_samples_leaf=5`, `max_features="sqrt"` e `class_weight="balanced"`.

| Modelo | Acurácia (CV, 5 folds) | Acurácia (teste1) |
|---|---|---|
| EP1: Regressão Logística + TF-IDF | 0,4632 | **0,4967** |
| RF: estilo e legibilidade (24) | 0,4219 | — |
| RF: + grupos lexicais (39)* | 0,4374 | — |
| RF: + grupos lexicais + léxico de clareza (48) | **0,4506** | 0,4756 |

\* Avaliação preliminar com hiperparâmetros fixos.

No teste1, a Random Forest final obteve F1-macro de 0,4751. Como no EP1, a classe intermediária (c234) foi a mais difícil (F1 = 0,43), enquanto c1 e c5 ficaram com F1 de 0,48 e 0,51.

**Teste de McNemar (EP1 x RF, teste1):** o EP1 acertou 125 instâncias em que a RF errou, e a RF acertou 106 em que o EP1 errou. O resultado foi χ² = 1,40 e p = 0,236. A diferença entre os modelos **não é estatisticamente significativa** (p ≥ 0,05).

## Análise de características

Pela importância por permutação no teste1, o léxico de clareza (o único conjunto aprendido dos dados) é de longe o fator mais relevante: embaralhá-lo reduz a acurácia em 13,6 pontos percentuais. Nenhuma outra característica, individualmente, passa de 1 ponto. As seguintes foram, em ordem, a proporção de maiúsculas, o tamanho da maior sentença, a referência ao pedido, a diversidade vocabular (TTR raiz) e o número de palavras. Os grupos lexicais ligados a fórmulas de resposta, cortesia e anexos também contribuíram.

Na comparação entre classes, as respostas c5 tendem a ser mais curtas, ter vocabulário mais variado e mencionar anexos com mais frequência.

## Conclusão

Usar só características de legibilidade e estilo não é suficiente para prever a clareza das respostas: a acurácia na validação cruzada ficou em 0,42. O desempenho melhorou ao incorporar informação lexical, chegando a 0,45 na validação cruzada e 0,476 no teste1, mas ficou abaixo do modelo do EP1, ainda que sem diferença significativa. Entre as características definidas manualmente, as de estilo e legibilidade contribuíram pouco, e a parte aprendida dos dados (léxico de clareza) foi a mais relevante. A análise de importância indica, portanto, que a clareza, como percebida pelos anotadores, depende mais do **conteúdo lexical** da resposta (o que é dito) do que de propriedades de legibilidade (como é dito).
