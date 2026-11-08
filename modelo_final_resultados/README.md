
# Modelo Final — EP2

## Arquitetura

Stacking composto por quatro modelos-base:

1. TF-IDF Word + Logistic Regression
2. Qwen3-Embedding-0.6B + Logistic Regression
3. Qwen3-Embedding-4B + Logistic Regression
4. BERTimbau Base Head-Tail original

As probabilidades dos quatro modelos são concatenadas,
produzindo 12 features para um meta-classificador
Logistic Regression.

## Estrutura

- probabilidades/: probabilidades OOF e Teste 1.
- modelos/: modelos-base e meta-classificador.
- embeddings/: embeddings extraídos pelos Qwen.
- validacao/: resultados agregados do Nested Stacking.
- metadados/: configurações e rótulos codificados.

## Validação

Os resultados do Nested Stacking correspondem a uma execução
anterior com 5 folds externos e 4 folds internos.

Foram preservadas as métricas por fold e as métricas agregadas.
Os arrays originais de predições e probabilidades dessa execução
não estão incluídos.

## Observações

- O Teste 1 é utilizado para avaliação e comparação estatística
  com o EP1.
- O Teste 2 é reservado para a avaliação oficial do EP2.
- As predições do Teste 2 serão geradas separadamente.

## Ordem das features do meta-classificador

1. TF-IDF Word: c1, c234, c5
2. Qwen 0.6B: c1, c234, c5
3. Qwen 4B: c1, c234, c5
4. BERT Head-Tail: c1, c234, c5
