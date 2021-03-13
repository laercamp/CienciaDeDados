Ciência de Dados para Segurança (CI1030) - Trabalho Final

Alunos:
Carlos Humberto Lopes Costa
Láercio Silva de Campos Júnior

I.	INTRODUÇÃO

Esse trabalho foi elaborado como Projeto Final para a disciplina “Ciência de Dados para Segurança”, professor André Grégio (Dtinf/UFPR).  O objetivo desse trabalho é aplicar as técnicas e ferramentas de Ciência de Dados para explorar um conjunto de dados específico.
Foi escolhido para análise o conjunto de dados de ocorrências atendidas pela Guarda Municipal de Curitiba/PR. Esse trabalho abordará todas as etapas do processo de Ciência de Dados – Obtenção de Dados, Limpeza, Exploração, Modelagem e Interpretação. Esse trabalho aplicou ainda os algoritmos de KNN, Random Forest e Support Vector Machine no conjunto de dados.

![image](https://user-images.githubusercontent.com/63817167/111010901-9b4fa600-8376-11eb-937b-3597b76f0622.png)

Fig. 1 - https://towardsdatascience.com/5-steps-of-a-data-science-project-lifecycle-26c50372b492

II.	ANÁLISE DOS DADOS
a)	Fonte de Dados
O conjunto de dados foi obtido do portal de dados abertos da Prefeitura de Curitiba/PR . Esse dataset conta com dados  do sistema “SiGesGuarda” - sistema contendo os dados das ocorrências atendidas pela Guarda Municipal de Curitiba/PR. O dataset está no formato CSV e o espectro temporal é de 2009 até 01/02/2021. O dataset fornece informações como: categoria, subcategoria, data, hora, bairro e origem da ocorrência.

b)	Pré-Processamento
O dataset foi pré-processamento para remoção de registros vazios, remoção de atributos desnecessários, correção de inconsistências e criação de novos atributos. Foi criado o script Python “PreProcessamento.py” para realização dessa tarefa, sendo o script disponibilizado o GitHub “https://github.com/chlcosta/CdadosSeg/tree/main/T3”.
Originalmente o dataset possui 35 colunas/atributos, após o pré-processamento ficou com 11 atributos, apresentados na Tabela 1 a seguir.

Tabela 1 – Colunas/Atributos do Dataset
1	OC_ANO – Ano da Ocorrência
2015, 2016, 2017, 2018, 2019, 2020
2	OC_MES – Mês da Ocorrência
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12
3	OC_DIA_SEMANA – Dia da Semana (Numérico)
1, 2, 3, 4, 5, 6, 7
4	OC_DIA_SEMANA_TXT – Dia da Semana (Textual)
1-DOMINGO', '2-SEGUNDA', '3-TERCA', '4-QUARTA', '5-QUINTA', '6-SEXTA', '7-SABADO'
5	OC_DIA – Dia do mês
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31
6	OC_PERIODO_DIA – Período do Dia (Numérico)
1, 2, 3, 4
7	OC_PERIODO_DIA_TXT – Período do Dia (Textual)
'1-MANHA', '2-TARDE', '3-NOITE', '4-MADRUGADA'
8	OC_BAIRRO – Bairro da Ocorrência (Numérico)
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12,  …,  71, 72, 73, 74
9	OC_BAIRRO_TXT – Bairro da Ocorrência (Textual)
'ABRANCHES', 'ÁGUA VERDE', 'AHÚ', 'ALTO BOQUEIRÃO', 'ALTO DA GLÓRIA', 'ALTO DA RUA XV', 'ATUBA', 'AUGUSTA', 'BACACHERI', 'BAIRRO ALTO', 'BARREIRINHA', 'BATEL'...
10	OC_SUBCATEGORIA – Tipo da Ocorrência (Numérico)
1, 2, 3, 4, 5, 6, 7, 8, 9, 10
11	OC_SUBCATEGORIA_TXT – Tipo da Ocorrência (Textual)
'Arrombamento', 'Cão solto em via pública', 'Desordem', 'Disparo de Alarme (violação)', 'Estacionamento irregular', 'Invasão de equipamento/patrimônio público', 'Pichação', 'Transporte Coletivo', 'Uso de substância ilícita', 'Vandalismo'

O atributo de período do dia (OC_PERIODO_DIA_TXT) foi criado utilizando as seguintes referências, A madrugada vai da zero hora às 6h. A manhã, das 6h às 12h (ou ao meio-dia). A tarde, das 12h às 18h. A noite, das 18h às 24h (ou meia-noite).
Para o estudo optou-se por definir o intervalo dos últimos 6 anos para análise (2020-2015) e as 10 ocorrências de maior frequência no período, totalizando 35.424 registros.
c)	Análise dos Dados

Na sequência são apresentados os gráficos de distribuição do dataset baseado no Ano, Mês e Dia da Ocorrência.

![image](https://user-images.githubusercontent.com/63817167/111010949-bb7f6500-8376-11eb-83c2-3ecfe10af589.png)
(a)

![image](https://user-images.githubusercontent.com/63817167/111010957-c0441900-8376-11eb-8f97-21265e8d4897.png)
(b)

![image](https://user-images.githubusercontent.com/63817167/111010960-c508cd00-8376-11eb-9803-1b96030f0516.png)
(c)

Fig. 2 – Distribuições por (a) Ano, (b) Mês e
(c) Dia da Semana

	Ano	Mês	Dia Semana
Terceiro Quartil	6.797	3.141	5.305
Mediana	5.306	2.984	4.837
Primeiro Quartil	5.011	2.873	4.731

![image](https://user-images.githubusercontent.com/63817167/111010989-d4881600-8376-11eb-8ac4-12a15eef6e8f.png)
Fig. 3 – Número de Ocorrências por Ano

![image](https://user-images.githubusercontent.com/63817167/111011002-d9e56080-8376-11eb-896b-844142050020.png)
Fig. 4 – Tendências das “Top 10” ocorrências

A partir da análise das principais ocorrências pode-se observar um aumento de quase 1.000% nas ocorrências de “Estacionamento Irregular” entre 2019 e 2020. Com exceção da ocorrência da “Estacionamento Irregular” as demais ocorrências sofreram uma leve queda de 2019 para 2020, possivelmente devido ao efeito da pandemia do COVID-19. No entanto, o “Uso de Substância Ilícita” teve uma queda aproximada de 31% no mesmo período.

![image](https://user-images.githubusercontent.com/63817167/111011014-e23d9b80-8376-11eb-966c-bacac7435892.png)
Fig. 5 – Número de ocorrências por Tipo

Observou-se através das Figuras 5 e 6 que a distribuição da quantidade de ocorrências é bastante irregular entre os tipos de ocorrências e entre os bairros.

![image](https://user-images.githubusercontent.com/63817167/111011018-e8337c80-8376-11eb-971e-e060e37095de.png)
Fig. 6 – Número de ocorrências por Bairro

![image](https://user-images.githubusercontent.com/63817167/111011027-ef5a8a80-8376-11eb-8369-8eac4759fd5a.png)
Fig. 7 – Número de ocorrências por Período do Dia

Conforme a Figura 7, as ocorrências atendidas pela Guarda Municipal de Curitiba, para os “Top 10” tipos de ocorrências, seguem a tendência de aumento ao longo do dia, no entanto, com redução considerável no período da madrugada.
Para essa etapa de exploração dos dados foi criado o  script Python “ExploracaoDados.py”, que também foi disponibilizado no endereço do GitHub informado.

III.	CONSTRUÇÃO DO MODELO

Considerando que o propósito do presente trabalho é a classificação dos tipos de ocorrências atendidas pela Guarda Municipal de Curitiba/PR, utilizou-se para construção dos modelos os algoritmos de classificação    K-Nearest Neighbour (KNN), Random Forests e Support Vector Machine (SVM). Cada algoritmo tem suas próprias vantagens e desvantagens em termos de complexidade, precisão e tempo de treinamento, podendo prover diferentes resultados para um mesmo conjunto de dados de entrada.
Antes da construção dos modelos, os dados precisaram ser alterados para formatos compatíveis com os modelos. Os atributos categóricos foram convertidos em atributos numéricos com um único ID. Todos os tipos de ocorrências e bairros possuem um diferente ID. Os bairros são representados através de 74 IDs e os tipos de ocorrências através de 10 IDs. A relação entre os atributos textuais e numéricos podem ser visualizados na Tabela 1, apresentada anteriormente.
A etapa de pré-processamento realizou a divisão do dataset em conjunto de treinamento e teste na proporção “80/20”, respectivamente. Os algoritmos foram aplicados sobre esses mesmos conjuntos de dados. Realizou-se também a validação cruzada (Cross-Validation) com 5 pastas para cada algoritmo. A validação cruzada previne o problema de overfitting e assegura que a predição do modelo possui performance satisfatória para dados ainda não vistos.
Antes da aplicação de cada modelo, o dataset foi novamente divido na proporção “80/20”.
Para aplicação dos modelos no dataset foi criado o  script Python “Modelos.py”, que está disponível no endereço do GitHub informado.

a) K-Nearest Neighbour (KNN)

O KNN foi aplicado com os parâmetros 1, 3 e 5, sendo os resultados coletados e apresentados abaixo:

Tabela 2 – Resultados KNN
	K =1	K = 3	K = 5
Precisão	0.195	0.203	0.217
Acurácia	0.256	0.266	0.296
Erro	2.565	2.847	2.695



[[ 62   4  12  14  31  23  16  11  22  10]
 [  2   3   0   4   2   1   3   1   7   1]
 [ 16   1   7   8  14  21  10   4   7   2]
 [ 16   2   4  11   8  12  10   8  13  10]
 [ 27   2  12  14  21  17  21   9  26  15]
 [ 25   1   6  12  20  16  18   7  21   8]
 [ 16   1   8  14  19  24  18   8  20  26]
 [  9   0   3   5   8  12   9   1  18   9]
 [ 33   2   1  12  21  15  25  12  63  34]
 [ 13   0   4   7  19  11  12  12  21 161]]



Nova divisão

b) Random Forests



c) Support Vector Machine (SVM)


IV.	CONCLUSÃO



V.	REFERÊNCIAS

[1]	Ceschin, F.; Oliveira, L. E. S.; Grégio, A. R. A. Aprendizado de Máquina para Segurança: Algoritmos e Aplicações. Capítulo 2 do Livro de Minicursos do XIX Simpósio Brasileiro de Segurança da Informação e de Sistemas Computacionais, 2019.
https://sbseg2019.ime.usp.br/minicursos.pdf





