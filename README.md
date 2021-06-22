# Desafio Final - Trilha Engenheiro IA Microsoft
Os resultados apresentados neste arquivo foram produzidos através das ferramentas presentes no Microsoft Azure Machine Learning usando o estúdio do Azure Machine Learning para treinar os modelos.

## Realizando a Análise dos Dados do Enem
Para o desafio 1 e 2 foi utilizado o dataset de microdados do ENEM de 2019
https://download.inep.gov.br/microdados/microdados_enem_2019.zip

### Desafio 1 - (3 pontos) Existe alguma correlação entre as notas das provas objetivas?
Pré-processamento dos dados:
O dataset foi colocado em um conjunto de dados onde já separei apenas as colunas que vão ser usadas no treinamento (NU_NOTA_CN, NU_NOTA_CH, NU_NOTA_LC e NU_NOTA_MT).
Através do Designer do estúdio do Azure Machine Learning fiz o tratamento do dataset usando o módulo "Clean Missing Data" para limpar os dados que faltavam, mas depois de uma pesquisa sobre pré-processamento de dados decidi fazer uns testes substituindo os valores faltantes pelo módulo, mediana, média e retirando a linha inteira, como resultado obtive melhores valores substituindo com a mediana. Usei o módulo "Split Data" com as propriedades de Split Rows com a fração de 0.7, de forma que 70% das linhas são usadas para o treinamento e 30% para avaliar o modelo.

Treinando o modelo: 
Considerando que o enunciado do desafio deseja uma previsão numérica, então, é necessário usar um algoritmo de análise de regressão. Para teste escolhi o algoritmo "Linear Regression"(Parâmetros padrão do Azure) e o "Boosted Decision Tree Regression" (Parâmetros: "Single Parameter"; máximo de 40 "folhas" por árvore; 10; 0.2 taxa de aprendizagem inicial; 100 árvores no total). Ao avaliar os dois modelos o "Boosted Decision Tree Regression" foi melhor avaliado

-------------------|"Linear Regression"|"Boosted Decision Tree Regression"
-------------------|-------------------|----------------------------------
Mean_Absolute_Error|0.036465|0.02916039
Root_Mean_Squared_Error|0.05123|0.042682036
Relative_Squared_Error|0.460385|0.431296
Relative_Absolute_Error|0.6635|0.616479
Coefficient_of_Determination|0.539615|0.568704

Então criei uma pipeline de inferência em tempo real onde utilizei minhas notas no ENEM para o modelo prever a nota de ciências. Obtive os seguintes resultados:
NU_NOTA_CH|NU_NOTA_LC|NU_NOTA_MT|NOTA OFICIAL|NOTA PREVISTA A*|NOTA PREVISTA B*
----------|----------|----------|------------|-------------|-------------
639|648|634|602|573.922325|505.18463
666|602|705|557|606.063713|516.717378
628|645|680|553|581.979772|516.717378

A* Para prever essa nota foram utilizados todas as notas das provas objetivas (menos as notas de CN)
B* Para prever essa nota foi utilizado apenas a nota da prova de matemática.

### Desafio 2 - (5 pontos) Escolha um estado da federação e clusterize os estudantes do enade com base em suas respostas ao questionário socioeconômico.




