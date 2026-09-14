# 📊 Análise de Confiabilidade de Servidores em TI

Projeto acadêmico desenvolvido em **Python** para aplicar conceitos de **Inferência Estatística e Machine Learning** à análise de telemetria de servidores.

O projeto foi elaborado no contexto da disciplina **Inferência Estatística**, do 6º semestre de Ciência da Computação do Centro Universitário Impacta, e tem como objetivo transformar os conceitos estudados em uma implementação prática sobre um conjunto de dados de servidores.

---

## 🎯 Objetivo do projeto

O objetivo é analisar dados de desempenho de servidores e investigar a relação entre suas características operacionais e a ocorrência de **falhas iminentes**.

O trabalho combina duas áreas:

- **Inferência Estatística**, para estimar parâmetros e testar hipóteses;
- **Machine Learning**, para reduzir a dimensionalidade dos dados e realizar classificação.

A proposta acadêmica solicita a aplicação de estimadores, intervalos de confiança, teste Qui-Quadrado, PCA e KNN sobre um dataset contendo **500 registros de telemetria operacional de servidores**.

---

# 📚 Relação com a atividade da disciplina

Este notebook não foi desenvolvido como uma análise isolada. Ele foi construído para atender às etapas propostas no **Exercício Programa — Inferência Estatística e Machine Learning em TI**, cujo tema é a análise de confiabilidade de servidores em nuvem.

A atividade solicita quatro partes principais:

| Parte | Conteúdo da atividade | Implementação no código |
|---|---|---|
| 1 | Pandas, estimadores e intervalo de confiança | Estatística descritiva e IC de 95% |
| 2 | Teste de hipóteses Qui-Quadrado | Relação entre `tipo_rede` e `status_alerta` |
| 3 | PCA | Redução de 4 variáveis para 2 componentes |
| 4 | KNN | Classificação de servidores quanto ao alerta de falha |

A atividade também determina a utilização das bibliotecas **pandas, scipy e scikit-learn**, além da execução e interpretação dos resultados.

---

# 🗂️ Dataset

O conjunto de dados utilizado é:

```text
servidores_ti.csv
```

O dataset possui **500 registros** e 7 variáveis.

### Variáveis

| Coluna | Tipo | Descrição |
|---|---|---|
| `servidor_id` | Inteiro/String | Identificador do servidor |
| `tipo_rede` | Categórica | Tipo de infraestrutura |
| `uso_cpu` | Contínua | Percentual médio de utilização da CPU |
| `uso_memoria` | Contínua | Percentual médio de utilização da memória |
| `latencia_rede_ms` | Contínua | Latência média da rede |
| `taxa_pacotes_perdidos` | Contínua | Percentual de pacotes perdidos |
| `status_alerta` | Binária | 0 = Saudável; 1 = Falha Iminente |

Os tipos de infraestrutura considerados são:

- AWS;
- Azure;
- On-Premise.

---

# 🧰 Tecnologias utilizadas

O projeto foi desenvolvido utilizando:

- Python
- Pandas
- NumPy
- SciPy
- Scikit-learn
- Matplotlib
- Google Colab / Jupyter Notebook

### Instalação

```bash
pip install pandas numpy scipy scikit-learn matplotlib
```

---

# 📁 Estrutura do projeto

```text
📁 projeto/
│
├── 📓 analise_servidores_revisado (2).ipynb
├── 📄 servidores_ti.csv
└── 📄 README.md
```

---

# 1️⃣ Parte 1 — Manipulação dos dados e Inferência Estatística

A primeira etapa corresponde à aplicação dos conceitos básicos de **estatística descritiva e estimação**.

O código começa carregando o dataset e verificando sua estrutura.

São analisados:

- tipos das colunas;
- quantidade de registros;
- estatísticas das variáveis;
- média;
- variância;
- desvio padrão;
- tamanho da amostra.

## Variáveis analisadas

Nesta etapa são utilizadas principalmente:

```text
uso_cpu
latencia_rede_ms
```

Os resultados encontrados no notebook foram:

| Variável | Média | Variância | Desvio padrão | n |
|---|---:|---:|---:|---:|
| `uso_cpu` | 55,11006 | 227,791307 | 15,092757 | 500 |
| `latencia_rede_ms` | 24,91934 | 559,758393 | 23,659214 | 500 |

Esses cálculos correspondem diretamente à solicitação da atividade de calcular a **média amostral** e a **variância amostral** para `uso_cpu` e `latencia_rede_ms`.

---

# 📐 Intervalo de confiança de 95%

A segunda parte da Parte 1 consiste na construção de um **intervalo de confiança de 95% para a média verdadeira de utilização da CPU**.

O código utiliza a distribuição **t de Student**, conforme solicitado na atividade.

O procedimento considera:

1. média amostral;
2. desvio padrão amostral;
3. tamanho da amostra;
4. erro padrão;
5. graus de liberdade;
6. valor crítico;
7. margem de erro;
8. limites inferior e superior.

A ideia estatística é estimar uma faixa plausível para a média populacional de utilização da CPU a partir dos dados observados.

---

# 2️⃣ Parte 2 — Teste de Hipóteses Qui-Quadrado

A segunda grande etapa investiga uma questão importante para a infraestrutura de TI:

> **Existe relação entre o tipo de rede utilizado pelo servidor e a ocorrência de uma falha iminente?**

Para responder à pergunta, o código utiliza o **Teste Qui-Quadrado de Independência**.

## Variáveis

São utilizadas:

```text
tipo_rede
status_alerta
```

O `status_alerta` possui duas categorias:

```text
0 → Saudável
1 → Falha Iminente
```

---

## 📊 Tabela de contingência

O código cria uma tabela cruzando o tipo de infraestrutura com o status do servidor.

Resultado:

| Tipo de rede | Saudável (0) | Falha Iminente (1) |
|---|---:|---:|
| AWS | 169 | 38 |
| Azure | 133 | 29 |
| On-Premise | 106 | 25 |

Essa tabela permite observar a distribuição dos alertas entre os diferentes tipos de infraestrutura.

---

## 🧪 Hipóteses

O teste estatístico utiliza:

**H₀:** o tipo de rede e o status de alerta são independentes.

**H₁:** o tipo de rede e o status de alerta não são independentes.

O nível de significância utilizado é:

```text
α = 0,05
```

O código calcula:

- estatística Qui-Quadrado;
- graus de liberdade;
- p-valor;
- frequências esperadas.

A decisão estatística é tomada comparando o **p-valor com α = 0,05**.

### Regra de decisão

```text
p-valor < 0,05
        ↓
Rejeitar H₀

p-valor ≥ 0,05
        ↓
Não rejeitar H₀
```

Essa etapa demonstra na prática um dos principais conceitos de **Inferência Estatística: utilizar uma amostra para avaliar uma hipótese sobre uma população**.

---

# 3️⃣ Parte 3 — PCA (Principal Component Analysis)

A terceira etapa introduz um conceito de **Machine Learning e análise multivariada**.

O problema possui quatro métricas quantitativas:

```text
uso_cpu
uso_memoria
latencia_rede_ms
taxa_pacotes_perdidos
```

Trabalhar diretamente com todas essas dimensões pode dificultar a visualização e a interpretação dos dados.

Por isso, o projeto utiliza o **PCA — Principal Component Analysis**.

---

## 📏 Padronização

Antes da aplicação do PCA, os dados são padronizados utilizando:

```python
StandardScaler()
```

A padronização transforma as variáveis para uma escala comparável, com média próxima de zero e variância unitária.

Essa etapa é importante porque as variáveis possuem unidades e escalas diferentes.

---

## 🔽 Redução de dimensionalidade

Depois da padronização, o PCA transforma:

```text
4 variáveis originais
        ↓
PC1 + PC2
```

Ou seja:

```text
uso_cpu
uso_memoria
latencia_rede_ms
taxa_pacotes_perdidos
        ↓
      PCA
        ↓
PC1          PC2
```

O código utiliza:

```python
PCA(n_components=2)
```

---

## 📊 Cargas dos componentes

O notebook também analisa o peso de cada variável nos componentes principais.

Os resultados registrados foram:

| Componente | uso_cpu | uso_memoria | latencia_rede_ms | taxa_pacotes_perdidos |
|---|---:|---:|---:|---:|
| PC1 | -0,097 | 0,261 | 0,700 | -0,658 |
| PC2 | 0,704 | 0,660 | 0,084 | 0,248 |

Essas cargas permitem interpretar quais variáveis possuem maior contribuição para cada componente.

Por exemplo, `latencia_rede_ms` apresenta forte contribuição positiva para o **PC1**, enquanto `uso_cpu` e `uso_memoria` possuem contribuições importantes para o **PC2**.

---

# 4️⃣ Parte 4 — Classificação com KNN

A última etapa utiliza **Machine Learning supervisionado**.

O objetivo é utilizar as informações dos servidores para classificar seu estado:

```text
0 → Saudável
1 → Falha Iminente
```

O modelo utilizado é o:

**K-Nearest Neighbors (KNN)**.

---

## ✂️ Divisão dos dados

O conjunto é dividido em:

```text
70% → Treinamento
30% → Teste
```

A atividade determina uma semente aleatória fixa, sendo utilizada:

```python
random_state = 42
```

Essa configuração permite reproduzir a divisão dos dados.

---

## 🤖 Configuração do KNN

O modelo é configurado com:

```python
KNeighborsClassifier(n_neighbors=5)
```

Portanto:

```text
K = 5
```

O algoritmo procura os **5 vizinhos mais próximos** e utiliza essas observações para determinar a classe do servidor.

---

# 📊 Avaliação do modelo

O modelo é avaliado utilizando:

### Matriz de confusão

Permite verificar:

- verdadeiros positivos;
- verdadeiros negativos;
- falsos positivos;
- falsos negativos.

### Classification Report

O código também calcula métricas como:

- **Precision**;
- **Recall**;
- **F1-Score**.

Essas métricas permitem avaliar o desempenho do classificador além de simplesmente observar a quantidade de acertos.

---

# ⚠️ Importância dos falsos negativos

No contexto deste projeto, os **falsos negativos** possuem uma importância operacional especial.

Um falso negativo acontece quando:

```text
Servidor com falha iminente
          ↓
Modelo classifica como
"Saudável"
```

Em um ambiente corporativo, isso pode significar que uma falha potencialmente importante não seja identificada antecipadamente.

As consequências podem incluir:

- indisponibilidade de serviços;
- interrupção de sistemas;
- perda de produtividade;
- necessidade de manutenção emergencial;
- impacto sobre usuários e clientes.

Por isso, a análise do **Recall da classe "Falha Iminente"** é especialmente relevante.

---

# 🔗 Relação entre Inferência Estatística e Machine Learning

Uma das principais características deste projeto é justamente a integração entre os dois conteúdos estudados.

```text
             DADOS DOS SERVIDORES
                     │
                     ▼
          ┌──────────────────────┐
          │ Estatística          │
          │ descritiva           │
          └──────────┬───────────┘
                     │
                     ▼
          Intervalo de confiança
                     │
                     ▼
          Teste Qui-Quadrado
                     │
                     ▼
          ┌──────────────────────┐
          │ Machine Learning     │
          └──────────┬───────────┘
                     │
                     ▼
              Padronização
                     │
                     ▼
                   PCA
                4 → 2
                     │
                     ▼
                   KNN
                  K = 5
                     │
                     ▼
          Avaliação da classificação
```

A **Inferência Estatística** é utilizada para compreender os dados e verificar relações estatísticas.

O **Machine Learning** é utilizado posteriormente para representar os dados em menor dimensionalidade e realizar uma classificação preditiva.

---

# 📚 Conceitos da matéria aplicados no código

## Inferência Estatística

- População e amostra;
- Média amostral;
- Variância amostral;
- Desvio padrão;
- Estimação;
- Intervalo de confiança;
- Distribuição t de Student;
- Hipótese nula;
- Hipótese alternativa;
- Nível de significância;
- p-valor;
- Teste Qui-Quadrado;
- Independência estatística.

## Machine Learning

- Variáveis preditoras;
- Variável-alvo;
- Treinamento e teste;
- Padronização;
- Redução de dimensionalidade;
- PCA;
- Classificação supervisionada;
- KNN;
- Matriz de confusão;
- Precision;
- Recall;
- F1-Score.

---

# 🧪 Relação com os exercícios de Teste de Hipótese

Além do exercício programa de servidores, o conteúdo utilizado neste projeto está relacionado à lista de exercícios de **Teste de Hipótese com Variância Conhecida (σ conhecido)** da disciplina.

A lista apresenta a estatística:

```text
Zcalc = (X̄ - μ₀) / (σ / √n)
```

e trabalha diferentes tipos de hipóteses:

- bilateral;
- unilateral à direita;
- unilateral à esquerda.

Também são utilizados diferentes níveis de significância, como:

```text
α = 10%
α = 5%
α = 1%
```

A lista contém 20 situações práticas envolvendo controle de qualidade, engenharia, salários, logística, TI, consumo energético, saúde, educação, automóveis, delivery, tecnologia e satisfação de clientes.

---

# 💻 Como executar o projeto

### 1. Baixar os arquivos

É necessário possuir:

```text
analise_servidores_revisado (2).ipynb
servidores_ti.csv
```

### 2. Instalar as bibliotecas

```bash
pip install pandas numpy scipy scikit-learn matplotlib
```

### 3. Abrir o notebook

O projeto pode ser executado em:

- Google Colab;
- Jupyter Notebook;
- JupyterLab;
- Visual Studio Code.

### 4. Carregar o dataset

O arquivo `servidores_ti.csv` deve estar disponível no caminho utilizado pelo notebook.

### 5. Executar as células

Execute as células do notebook na ordem para reproduzir:

1. carregamento dos dados;
2. análise estatística;
3. intervalo de confiança;
4. teste Qui-Quadrado;
5. PCA;
6. KNN;
7. avaliação do modelo.

---

# 📌 Conclusão

O projeto apresenta uma aplicação prática dos conteúdos estudados na disciplina de **Inferência Estatística**, integrando conceitos estatísticos com técnicas de Machine Learning.

Na primeira etapa, os dados dos servidores são explorados por meio de estatística descritiva e estimação. Em seguida, o teste Qui-Quadrado é utilizado para investigar a possível relação entre o tipo de infraestrutura e a ocorrência de falhas.

Na etapa de Machine Learning, as quatro métricas quantitativas são padronizadas e reduzidas para dois componentes principais por meio do PCA. Esses componentes são então utilizados como entrada para um classificador KNN configurado com cinco vizinhos.

Assim, o notebook percorre todo o processo:

```text
Dados
 ↓
Estatística descritiva
 ↓
Intervalo de confiança
 ↓
Teste de hipótese
 ↓
PCA
 ↓
KNN
 ↓
Avaliação
```

O projeto demonstra, portanto, como conceitos teóricos de **Inferência Estatística** podem ser implementados computacionalmente e integrados a técnicas de **Ciência de Dados e Machine Learning** em um problema relacionado à confiabilidade de infraestrutura de TI.

---

# 🎓 Contexto acadêmico

**Curso:** Ciência da Computação — 6º semestre  
**Instituição:** Centro Universitário Impacta  
**Disciplina:** Inferência Estatística  
**Tema:** Inferência Estatística e Machine Learning em TI  
**Aplicação:** Análise de Confiabilidade de Servidores em Nuvem

## 👨‍💻 Autor

**Richard**

Estudante de Ciência da Computação.

Projeto desenvolvido para fins acadêmicos.
