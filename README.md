# Teste de Hipótese para a Média com Variância Conhecida (Teste Z)

Script em Python que resolve os exercícios da lista de **Inferência Estatística**
(Centro Universitário Impacta, Prof. Kyung Moo Kim) sobre teste de hipótese para
a média (μ) quando o desvio-padrão populacional (σ) é conhecido.

## O que o código faz

O arquivo `teste_hipotese.py` implementa a função `teste_z`, que recebe os dados
de um problema e devolve o cálculo completo do teste:

```python
teste_z(mu0, sigma, n, x_bar, alpha, tipo)
```

**Parâmetros:**

| Parâmetro | Significado |
|---|---|
| `mu0`   | média sob H0 |
| `sigma` | desvio-padrão populacional |
| `n`     | tamanho da amostra |
| `x_bar` | média amostral |
| `alpha` | nível de significância |
| `tipo`  | `"bilateral"`, `"direita"` ou `"esquerda"` |

**Fórmulas usadas** (as mesmas do guia do PDF):

- Erro padrão: `σ / √n`
- Estatística do teste: `Zcalc = (x̄ − μ0) / (σ/√n)`
- Zcrit obtido pela distribuição normal padrão (`scipy.stats.norm.ppf`), de acordo
  com o tipo de teste (bilateral, unilateral à direita ou à esquerda)

A função retorna um dicionário com o erro padrão, o Zcalc, o Zcrit, a região
crítica e a decisão (rejeita ou não rejeita H0).

## Como usar

```bash
pip install scipy
python teste_hipotese.py
```

Rodando o script direto, ele já calcula e imprime o resultado dos 20 exercícios
da lista, na ordem em que aparecem no PDF.

Para testar um problema novo, é só chamar a função:

```python
from teste_hipotese import teste_z

resultado = teste_z(mu0=355, sigma=5, n=36, x_bar=353, alpha=0.05, tipo="bilateral")
print(resultado)
```

## Conferência com o gabarito

Os 20 resultados do script foram comparados um a um com o gabarito do PDF
(Parte 2) e todos batem — mesmo Zcalc, mesmo Zcrit e mesma decisão sobre H0
em cada exercício.

## Observação

O script cobre apenas o caso de **variância conhecida** (teste Z), que é o
escopo do PDF. Quando o desvio-padrão populacional não é conhecido, o teste
correto é o **teste t de Student**, não coberto aqui.
