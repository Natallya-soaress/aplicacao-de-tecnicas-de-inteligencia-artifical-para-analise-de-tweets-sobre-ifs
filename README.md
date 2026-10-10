# Aplicação de Técnicas de Inteligência Artificial para Análise de Tweets sobre Institutos Federais Brasileiros

Este repositório contém o código, os artefatos e as coleções de dados do Trabalho de Conclusão de Curso (TCC) do Bacharelado em Ciência da Computação do **Instituto Federal de Goiás (IFG) – Câmpus Anápolis (2023)**. O projeto foi desenvolvido a partir de pesquisas de Iniciação Científica (PIBIC).

- **Autora:** Nátallya Soares Costa
- **Orientador:** Prof. Dr. Sérgio Daniel Carvalho Canuto

---

## Sobre o Projeto

O trabalho investiga tarefas de **Processamento de Linguagem Natural (PLN)** aplicadas a tweets em português que mencionam os Institutos Federais (IFs) do Brasil, focando em duas frentes principais:

1. **Análise de Sentimento (Positivo, Negativo, Neutro):** Comparação de 11 abordagens, incluindo métodos baseados em dicionários (léxicos), aprendizado de máquina clássico e modelos avançados baseados em *Transformers*.
2. **Modelagem de Tópicos com BERTopic:** Identificação de temas e discussões recorrentes tanto em nível nacional quanto regionalizado por regiões do país.

O fluxo metodológico seguiu as etapas de **Descoberta de Conhecimento em Bases de Dados (KDD)**: coleta, pré-processamento, mineração e validação.

---

## Estrutura do Repositório

| Pasta | Conteúdo |
| :--- | :--- |
| `colecoes/` | Coleções de tweets utilizadas nos experimentos |
| `pre-processamento/` | Scripts e etapas de preparação dos dados textuais |
| `algoritmos-nao-supervisionados/` | Métodos baseados em dicionários léxicos (*Sentilex-PT* e *Vader*) |
| `algoritmos-classicos/` | Modelos de Machine Learning clássico (*Random Forests, Multinomial Naive Bayes, SVM e kNN*) |
| `algoritmos-baseados-em-transformers/` | Modelos de Deep Learning (*BERTweet, RoBERTa, BERTimbau, m-BERT e XLM-RoBERTa*) |
| `modelagem-topicos/` | Aplicação do *BERTopic* para extração de tópicos globais e regionais |

---

## Coleta e Dados

Foram utilizadas duas coleções distintas de tweets em português, ambas higienizadas (sem retweets e sem mensagens dos setores de comunicação oficiais para priorizar o discurso da comunidade acadêmica):

| Coleção | Período | Finalidade | Tamanho |
| :--- | :--- | :--- | :--- |
| **Rotulada** *(termos: "IFG", "Instituto Federal", "Institutos Federais")* | Out/2021 a Fev/2022 | Avaliação de técnicas de sentimento | ~2.500 tweets |
| **Regional** *(siglas de todos os IFs do país)* | Jan/2023 a Nov/2023 | Modelagem de tópicos | ~18.000 tweets |

* **Coleção 1:** Obtida via API v2 do Twitter (*filtered stream*). Rotulada manualmente por três estudantes (conflitos descartados).
* **Coleção 2:** Coleta periódica (horária) dividida em cinco bases geográficas (Centro-Oeste, Nordeste, Norte, Sudeste e Sul).

### Distribuição da Coleção Rotulada
* **Positivos:** Menoritária (gerou desafios de desbalanceamento).
* **Negativos / Neutros:** Maior representatividade.

---

## Configuração dos Experimentos

* **Pré-processamento:** Normalização (minúsculas), tokenização e representação textual (*TF-IDF*, *GloVe* e *FastText* — com o *TF-IDF* apresentando o melhor desempenho).
* **Tradução Automática:** Aplicada via API do Google Translation para os modelos nativos em inglês (*Vader*, *BERTweet* e *RoBERTa*).

| Abordagem | Configuração Principal |
| :--- | :--- |
| **Sentilex-PT / Vader** | Sem parametrização (dicionários e regras fixas) |
| **Random Forests** | 200 árvores sem poda, $\sqrt{n}$ termos por divisão |
| **Multinomial Naive Bayes** | Versão multinomial padrão |
| **SVM** | Kernel linear; parâmetro $C$ escolhido por validação cruzada ($10^{-3}$ a $10^3$) |
| **kNN** | $k \in \{2, 5, 10, 20, 30\}$ via validação cruzada em 5 partes |
| **Transformers** | Ajuste fino (*Fine-Tuning*) com 30 épocas, *learning rate* $10^{-5}$ e *batch size* 30 |
| **BERTopic** | Configuração multilíngue (uni, bi e trigramas), 30 termos por tópico; número de tópicos ($2$ a $20$) otimizado por coerência (10 execuções) |

* **Validação:** Validação cruzada de 5 partes (5-fold), avaliada pelas métricas **Macro-F1**, **Micro-F1** e Matriz de Confusão (com prioridade para o *Macro-F1* devido ao desbalanceamento de classes).

---

## Principais Resultados

### 1. Análise de Sentimento (Macro-F1 / Micro-F1)

| Abordagem | Coleção IFG | Coleção Instituto Federal |
| :--- | :---: | :---: |
| **Sentilex-PT** | 0,37 / 0,43 | 0,39 / 0,45 |
| **Vader** | 0,47 / 0,48 | 0,54 / 0,55 |
| **Random Forests** | 0,56 / 0,49 | 0,58 / 0,52 |
| **Multinomial Naive Bayes** | 0,56 / 0,51 | 0,62 / 0,57 |
| **SVM** | **0,58 / 0,54** | **0,63 / 0,59** |
| **kNN** | 0,52 / 0,41 | 0,57 / 0,48 |
| **BERTweet** | 0,73 / 0,68 | 0,74 / 0,69 |
| **RoBERTa** | 0,54 / 0,44 | 0,76 / 0,73 |
| **BERTimbau** | **0,78 / 0,75** | **0,86 / 0,84** |
| **m-BERT** | 0,53 / 0,38 | 0,68 / 0,60 |
| **XLM-RoBERTa** | 0,39 / 0,20 | 0,41 / 0,19 |

> **Destaques:**
> * Os modelos baseados em *Transformers* superaram claramente os métodos léxicos e clássicos.
> * O **BERTimbau** (pré-especificamente treinado em português) foi o mais efetivo, alcançando **Macro-F1 de 0,86** na coleção "Instituto Federal".
> * Métodos léxicos e clássicos ficaram com pontuações abaixo de 0,64 de Macro-F1.
> * Ruídos decorrentes de tradução automática prejudicaram o desempenho de modelos como *Vader*, *BERTweet* e *RoBERTa*. O desvio padrão dos *Transformers* foi mais elevado na coleção IFG devido ao menor volume amostral.

### 2. Modelagem de Tópicos
* A coerência média ideal na coleção completa foi obtida com **4 tópicos principais**.
* **Tópicos Globais:** Comunicação aluno-instituição (editais, vagas, inscrições) e ansiedade/medo em relação a avaliações e provas.
* **Tópicos Regionais Exemplares:** * *Centro-Oeste:* Discussões sobre processos/votações do IFB.
  * *Nordeste:* Repercussão do eclipse anular.
  * *Norte:* Rotina e vivências estudantis.
  * *Sudeste:* Expectativas e preocupações com processos seletivos.
  * *Sul:* Avisos e impactos de suspensão de aulas por condições climáticas (chuvas).

---
