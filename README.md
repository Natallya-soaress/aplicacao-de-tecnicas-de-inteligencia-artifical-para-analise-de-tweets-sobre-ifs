Aplicação de Técnicas de Inteligência Artificial para Análise de Tweets sobre Institutos Federais Brasileiros

Código e coleções do trabalho de conclusão de curso (Bacharelado em Ciência da Computação, Instituto Federal de Goiás, Câmpus Anápolis, 2023), desenvolvido a partir da pesquisa de iniciação científica (PIBIC).

Autora: Nátallya Soares Costa
Orientador: Prof. Dr. Sérgio Daniel Carvalho Canuto
Sobre o projeto

O trabalho investiga duas tarefas de Processamento de Linguagem Natural sobre tweets em português que mencionam os Institutos Federais (IFs) do Brasil:

Análise de sentimento (positivo, negativo, neutro), comparando 11 técnicas: métodos léxicos, aprendizado de máquina clássico e modelos baseados em transformers.
Modelagem de tópicos com BERTopic, em nível nacional e por região do país.

O processo seguiu as etapas de descoberta de conhecimento em bases de dados (KDD): coleta, pré-processamento, mineração e validação.

Principais resultados
Os modelos baseados em transformers superaram claramente os métodos léxicos e os clássicos.
O BERTimbau (pré-treinado em português) foi o mais efetivo, com Macro-F1 de 0,86 na coleção "Instituto Federal".
Léxicos e métodos clássicos ficaram abaixo de 0,64 de Macro-F1.
A modelagem de tópicos revelou temas globais (comunicados institucionais, ansiedade com provas) e regionais (por exemplo, eclipse anular no Nordeste e suspensão de aulas por chuvas no Sul).
Estrutura do repositório
Pasta	Conteúdo
colecoes/	Coleções de tweets utilizadas nos experimentos
pre-processamento/	Etapas de preparação dos dados textuais
algoritmos-nao-supervisionados/	Métodos baseados em dicionários léxicos (Sentilex-PT e Vader)
algoritmos-classicos/	Random Forests, Multinomial Naive Bayes, SVM e kNN
agoritmos-baseados-em-transformers/	BERTweet, RoBERTa, BERTimbau, m-BERT e XLM-RoBERTa
modelagem-topicos/	Aplicação do BERTopic (tópicos globais e regionais)
Dados

Foram usadas duas coleções, ambas em português e sem retweets:

Coleção	Período	Finalidade	Tamanho
Rotulada (termos "IFG", "Instituto Federal" e "Institutos Federais")	out/2021 a fev/2022	Avaliação das técnicas de sentimento	~2.500 tweets coletados
Regional (siglas de todos os IFs do país)	jan/2023 a nov/2023	Modelagem de tópicos	~18.000 tweets

A primeira coleção foi obtida pela API v2 do Twitter (filtered stream). A segunda, por coleta periódica (a cada hora) de tweets que continham a sigla de algum IF, dividida em cinco bases, uma por região (Centro-Oeste, Nordeste, Norte, Sudeste e Sul).

Rotulação: cada tweet foi rotulado manualmente por três estudantes; mensagens sem consenso foram descartadas. Mensagens dos setores de comunicação dos institutos foram removidas para avaliar apenas o discurso da comunidade.

Coleção rotulada	Positivos	Negativos	Neutros
IFG	128	271	269
Instituto Federal	263	613	580

Aviso: o uso e a redistribuição de textos de tweets estão sujeitos aos termos da plataforma. [Confirme se a pasta colecoes/ contém o texto completo ou apenas identificadores, e ajuste esta seção.]

Pré-processamento
Normalização (minúsculas) e tokenização
Representações de texto: TF-IDF, GloVe e FastText (TF-IDF teve o melhor resultado e foi usado nas tabelas abaixo)
Tradução automática para inglês (API Google Translation Basic) para os modelos nativos em inglês: Vader, BERTweet e RoBERTa
Configuração dos experimentos
Método	Configuração
Sentilex-PT, Vader	Sem parametrização (dicionário e regras fixas)
Random Forests	200 árvores sem poda, √n termos por divisão
Multinomial Naive Bayes	Versão multinomial
SVM	Kernel linear; C escolhido por validação cruzada entre 6 valores de 10⁻³ a 10³
kNN	k escolhido em {2, 5, 10, 20, 30} por validação cruzada em 5 partes
Transformers	Ajuste fino com 30 épocas, learning rate 10⁻⁵ e batch size 30
BERTopic	Configuração padrão multilíngue (uni, bi e trigramas), 30 termos por tópico; número de tópicos (2 a 20) escolhido pelo escore de coerência, com 10 execuções por valor

Validação: validação cruzada em 5 partes (5-fold), com Macro-F1, Micro-F1 e matriz de confusão. O Macro-F1 foi priorizado por penalizar modelos que favorecem a classe majoritária.

Resultados: análise de sentimento

Macro-F1 / Micro-F1 por coleção.

Abordagem	IFG	Instituto Federal
Sentilex-PT	0,37 / 0,43	0,39 / 0,45
Vader	0,47 / 0,48	0,54 / 0,55
Random Forests	0,56 / 0,49	0,58 / 0,52
Multinomial Naive Bayes	0,56 / 0,51	0,62 / 0,57
SVM	0,58 / 0,54	0,63 / 0,59
kNN	0,52 / 0,41	0,57 / 0,48
BERTweet	0,73 / 0,68	0,74 / 0,69
RoBERTa	0,54 / 0,44	0,76 / 0,73
BERTimbau	0,78 / 0,75	0,86 / 0,84
m-BERT	0,53 / 0,38	0,68 / 0,60
XLM-RoBERTa	0,39 / 0,20	0,41 / 0,19

Observações:

Ruídos de tradução prejudicaram Vader, BERTweet e RoBERTa.
A classe positiva é minoritária (desbalanceamento), o que dificultou os métodos clássicos.
O desvio padrão dos transformers entre as partes da validação cruzada foi alto na coleção IFG (menos dados).
Resultados: modelagem de tópicos
Na coleção completa, a maior coerência média foi obtida com 4 tópicos.
Tópicos globais de destaque: comunicação aluno-instituição (editais, vagas, inscrições) e ansiedade/medo em relação a provas.
Tópicos regionais: votação do IFB (Centro-Oeste), eclipse anular (Nordeste), rotina dos estudantes (Norte), preocupação com processos seletivos (Sudeste) e suspensão de aulas por chuvas (Sul).
