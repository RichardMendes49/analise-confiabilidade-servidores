# 📊 Análise de Confiabilidade de Servidores em TI

Projeto acadêmico desenvolvido em **Python** para aplicar conceitos de **Inferência Estatística e Machine Learning** na análise de dados de servidores.

O projeto utiliza um dataset com informações de telemetria de servidores e investiga a relação entre suas características operacionais e a ocorrência de **falhas iminentes**.

O trabalho foi desenvolvido na disciplina de **Inferência Estatística**, do 6º semestre de Ciência da Computação do Centro Universitário Impacta.

---

## 🎯 Objetivo

O projeto combina conceitos de **Inferência Estatística** e **Machine Learning** para analisar dados de servidores.

As principais etapas são:

* calcular estatísticas descritivas;
* construir um intervalo de confiança de 95% para a média de utilização da CPU;
* verificar a relação entre `tipo_rede` e `status_alerta` utilizando o teste Qui-Quadrado;
* reduzir as variáveis numéricas utilizando PCA;
* classificar servidores entre **Saudável** e **Falha Iminente** utilizando KNN;
* avaliar o desempenho do classificador por meio da matriz de confusão e das métricas de classificação.

---

## 📁 Dataset

O projeto utiliza o arquivo:

```text
servidores_ti.csv
```

O dataset possui **500 registros** e as seguintes variáveis:

| Variável                | Descrição                                             |
| ----------------------- | ----------------------------------------------------- |
| `servidor_id`           | Identificador do servidor                             |
| `tipo_rede`             | Tipo de infraestrutura utilizada                      |
| `uso_cpu`               | Percentual de utilização da CPU                       |
| `uso_memoria`           | Percentual de utilização da memória                   |
| `latencia_rede_ms`      | Latência da rede em milissegundos                     |
| `taxa_pacotes_perdidos` | Percentual de pacotes perdidos                        |
| `status_alerta`         | Status do servidor: 0 = Saudável / 1 = Falha Iminente |

Os tipos de infraestrutura presentes nos dados são:

* AWS
* Azure
* On-Premise

---

## 🛠️ Tecnologias utilizadas

* Python
* Pandas
* NumPy
* SciPy
* Scikit-learn
* Matplotlib
* Jupyter Notebook / Google Colab

### Instalação

```bash
pip install pandas numpy matplotlib scipy scikit-learn
```

---

# 📊 Parte 1 — Estimadores e Intervalo de Confiança

A primeira etapa trabalha com **estatística descritiva** das variáveis:

```text
uso_cpu
latencia_rede_ms
```

São calculados:

* média;
* variância amostral;
* desvio padrão amostral;
* tamanho da amostra.

A variância e o desvio padrão são calculados com `ddof=1`, tratando os dados como uma **amostra**.

### Intervalo de confiança

Em seguida, é calculado um **intervalo de confiança de 95% para a média de utilização da CPU**.

Como o desvio padrão populacional é desconhecido, o código utiliza a **distribuição t de Student**.

O cálculo considera:

```text
média amostral
desvio padrão amostral
erro padrão
graus de liberdade
t crítico
margem de erro
limite inferior
limite superior
```

O resultado fornece uma estimativa da faixa em que a média populacional de utilização da CPU pode estar, considerando os dados da amostra.

---

# 🧪 Parte 2 — Teste Qui-Quadrado

A segunda etapa investiga:

> **Existe relação entre o tipo de rede utilizado pelo servidor e o status de alerta?**

Para isso, são utilizadas as variáveis:

```text
tipo_rede
status_alerta
```

O código começa criando uma **tabela de contingência** com `pd.crosstab()`.

Depois é aplicado o:

**Teste Qui-Quadrado de Independência**

### Hipóteses

**H₀:** `tipo_rede` e `status_alerta` são independentes.

**H₁:** `tipo_rede` e `status_alerta` não são independentes.

O nível de significância utilizado é:

```text
α = 0,05
```

O código calcula:

* estatística Qui-Quadrado;
* graus de liberdade;
* p-valor;
* frequências esperadas.

A decisão é feita comparando o p-valor com `α`.

```text
p-valor < 0,05
→ rejeitar H₀

p-valor ≥ 0,05
→ não rejeitar H₀
```

> O teste identifica uma possível **associação estatística** entre as variáveis, mas não estabelece, sozinho, uma relação de causa e efeito.

---

# 📉 Parte 3 — Redução de Dimensionalidade com PCA

A terceira etapa utiliza **Machine Learning** para reduzir a quantidade de variáveis utilizadas na análise.

São consideradas quatro variáveis numéricas:

```text
uso_cpu
uso_memoria
latencia_rede_ms
taxa_pacotes_perdidos
```

A variável utilizada como alvo é:

```text
status_alerta
```

## Divisão dos dados

Antes da padronização e do PCA, os dados são separados em:

```text
70% → treinamento
30% → teste
```

A divisão utiliza:

```python
random_state=42
stratify=y
```

O `stratify` mantém a proporção das classes de `status_alerta` nos conjuntos de treinamento e teste.

## Padronização

As variáveis são padronizadas utilizando:

```python
StandardScaler()
```

O `StandardScaler` é ajustado **somente com os dados de treinamento** e depois aplicado tanto ao treinamento quanto ao teste.

Isso evita que informações do conjunto de teste sejam utilizadas durante o ajuste do modelo.

## PCA

Após a padronização, é aplicado:

```python
PCA(n_components=2)
```

Assim, as quatro variáveis originais são transformadas em dois componentes principais:

```text
4 variáveis
     ↓
    PCA
     ↓
 PC1 + PC2
```

O notebook também calcula:

* variância explicada por cada componente;
* variância explicada acumulada;
* cargas das variáveis originais em PC1 e PC2.

As cargas permitem observar a contribuição de cada variável para os componentes principais.

---

# 🤖 Parte 4 — Classificação com KNN

Na última etapa, os componentes obtidos pelo PCA são utilizados para treinar um classificador **K-Nearest Neighbors (KNN)**.

O objetivo é classificar os servidores em:

```text
0 → Saudável
1 → Falha Iminente
```

O modelo é configurado com:

```python
KNeighborsClassifier(n_neighbors=5)
```

Portanto:

```text
K = 5
```

O treinamento é realizado utilizando os dados de treinamento transformados pelo PCA.

Depois, o modelo realiza previsões sobre o conjunto de teste.

---

# 📊 Avaliação do modelo

O desempenho do KNN é analisado utilizando duas abordagens.

## Matriz de Confusão

O código gera uma matriz de confusão com as classes:

```text
Saudável (0)
Falha Iminente (1)
```

Ela permite visualizar:

* verdadeiros positivos;
* verdadeiros negativos;
* falsos positivos;
* falsos negativos.

## Classification Report

Também é utilizado:

```python
classification_report()
```

São apresentadas métricas como:

* **Precision**
* **Recall**
* **F1-Score**

A análise dá atenção especial à classe **Falha Iminente**, pois um falso negativo representa um servidor que apresentava uma falha iminente, mas foi classificado pelo modelo como saudável.

---

# 🔄 Fluxo do projeto

```text
servidores_ti.csv
       │
       ▼
Carregamento dos dados
       │
       ▼
Estatística descritiva
       │
       ▼
Intervalo de confiança
       │
       ▼
Teste Qui-Quadrado
       │
       ▼
Divisão treino / teste
       │
       ▼
StandardScaler
       │
       ▼
PCA (4 → 2)
       │
       ▼
KNN (K = 5)
       │
       ▼
Previsões
       │
       ├── Matriz de Confusão
       │
       └── Classification Report
```

---

## 📚 Conceitos aplicados

### Inferência Estatística

* Média amostral
* Variância amostral
* Desvio padrão
* Erro padrão
* Intervalo de confiança
* Distribuição t de Student
* Teste de hipótese
* Hipótese nula e alternativa
* p-valor
* Teste Qui-Quadrado
* Independência estatística

### Machine Learning

* Separação entre treino e teste
* Padronização
* PCA
* Redução de dimensionalidade
* Classificação supervisionada
* KNN
* Matriz de confusão
* Precision
* Recall
* F1-Score

---

## ▶️ Como executar

### 1. Clone o repositório

```bash
git clone URL_DO_REPOSITORIO
cd NOME_DO_REPOSITORIO
```

### 2. Instale as dependências

```bash
pip install pandas numpy matplotlib scipy scikit-learn
```

### 3. Mantenha os arquivos no mesmo diretório

```text
projeto/
├── AnaliseServidores.ipynb
├── servidores_ti.csv
└── README.md
```

### 4. Abra o notebook

O notebook pode ser executado utilizando:

* Google Colab;
* Jupyter Notebook;
* JupyterLab;
* Visual Studio Code.

### 5. Execute as células em ordem

O notebook foi organizado para que as etapas sejam executadas sequencialmente, desde o carregamento do dataset até a avaliação do modelo KNN.

---

## 🎓 Contexto acadêmico

**Curso:** Ciência da Computação — 6º semestre
**Instituição:** Centro Universitário Impacta
**Disciplina:** Inferência Estatística
**Tema:** Análise de Confiabilidade de Servidores em TI

## 👨‍💻 Autor

**Richard**

Estudante de Ciência da Computação.

Projeto desenvolvido para fins acadêmicos.
