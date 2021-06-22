# Desafio Final - Trilha Engenheiro IA Microsoft
Os resultados apresentados neste arquivo foram produzidos através das ferramentas presentes no Microsoft Azure Machine Learning usando o estúdio do Azure Machine Learning para treinar os modelos.

## Realizando a Análise dos Dados do Enem
Para o desafio 1 e 2 foi utilizado o dataset de microdados do ENEM de 2019
https://download.inep.gov.br/microdados/microdados_enem_2019.zip

## Desafio 1 - (3 pontos) Existe alguma correlação entre as notas das provas objetivas?

##### Pré-processamento dos dados:
O dataset foi colocado em um conjunto de dados onde já separei apenas as colunas que vão ser usadas no treinamento (NU_NOTA_CN, NU_NOTA_CH, NU_NOTA_LC e NU_NOTA_MT).
Através do Designer do estúdio do Azure Machine Learning fiz o tratamento do dataset usando o módulo  _**"Clean Missing Data"**_ para limpar os dados que faltavam, mas depois de uma pesquisa sobre pré-processamento de dados decidi fazer uns testes substituindo os valores faltantes pelo módulo, mediana, média e retirando a linha inteira, como resultado obtive melhores valores substituindo com a mediana. Usei o módulo _**"Split Data"**_ com as propriedades de Split Rows com a fração de 0.7, de forma que 70% das linhas são usadas para o treinamento e 30% para avaliar o modelo, as linhas foram separadas de forma aleatória e usando a "Random seed" padrão do módulo.

##### Treinando o modelo: 
Considerando que o enunciado do desafio deseja uma previsão numérica, então, é necessário usar um algoritmo de análise de regressão. Para teste escolhi o algoritmo  _**"Linear Regression"(Parâmetros padrão do Azure)"**_ e o _**"Boosted Decision Tree Regression"**_ (Parâmetros: "Single Parameter"; máximo de 40 "folhas" por árvore; 10; 0.2 taxa de aprendizagem inicial; 100 árvores no total). Ao avaliar os dois modelos o "Boosted Decision Tree Regression" foi melhor avaliado

-------------------|Linear Regression|Boosted Decision Tree Regression
-------------------|-------------------|----------------------------------
Mean_Absolute_Error|0.036465|0.02916039
Root_Mean_Squared_Error|0.05123|0.042682036
Relative_Squared_Error|0.460385|0.431296
Relative_Absolute_Error|0.6635|0.616479
Coefficient_of_Determination|0.539615|0.568704

Então criei uma pipeline de inferência em tempo real onde utilizei minhas notas no ENEM para o modelo prever a nota de ciências. Obtive os seguintes resultados:
NU_NOTA_CH|NU_NOTA_LC|NU_NOTA_MT|NOTA OFICIAL|NOTA PREVISTA _A*_|NOTA PREVISTA _B*_
----------|----------|----------|------------|-------------|-------------
639|648|634|602|573.922325|505.18463
666|602|705|557|606.063713|516.717378
628|645|680|553|581.979772|516.717378

>A* Para prever essa nota foram utilizados todas as notas das provas objetivas (menos as notas de CN)

>B* Para prever essa nota foi utilizado apenas a nota da prova de matemática.

## Desafio 2 - (5 pontos) Escolha um estado da federação e clusterize os estudantes do enade com base em suas respostas ao questionário socioeconômico.

##### Pré-processamento dos dados:
O dataset foi colocado em um conjunto de dados onde já separei apenas as colunas que vão ser usadas no treinamento (Q006;Q025;NU_NOTA_REDACAO).
Através do Designer do estúdio do Azure Machine Learning fiz o tratamento do dataset usando o módulo _**"Clean Missing Data"**_ nesse caso considerei que fosse necessário retirar a linha completa do dataset, caso houvesse a falta de algum dado. O módulo _**"Split Data"**_ foi usado com as propriedades de Split Rows com a fração de 0.7, de forma que 70% das linhas são usadas para o treinamento e 30% para avaliar o modelo, as linhas foram separadas de forma aleatória e usando a "Random seed" padrão do módulo.

##### Treinando o modelo: 
O enunciado do desafio pede a previsão do cluster de um estudante com base nos dados de Renda Mensal (Q006), acesso a internet(Q025) e nota final da redação(NU_NOTA_REDACAO) informados. Usando o Designer do estúdio do Azure Machine Learning utilizei o módulo _**"Train Clustering Model"**_ e o algoritmo de clustering disponível _**"K-Means Clustering"**_ (Parâmetros padrão do módulo) o resultado foi ligado ao módulo _**"Assign Data to Cluster"**_ e então o módulo _**"Evaluate Model"**_ para avaliar o modelo. 

Foi retornada a seguinte tabela:
Result Description|Average Distance to Other Center|Average Distance to Cluster Center|Number of Points|Maximal Distance to Cluster Center
------------------|--------------------------------|----------------------------------|----------------|-----------------------------------
Evaluation For Cluster No.0|1.42721|0.951839|702258|1.204265
Evaluation For Cluster No.1|1.074436|0.127362|223031|0.551924
Evaluation For Cluster No.2|1.607219|0.81604|251654|1.257748
Combined Evaluation|1.398849|0.766564|1176943|1.257748

Foi então criada uma pipeline de inferência em tempo real com as seguintes entradas e saídas:
NU_NOTA_REDACAO|Q006|Q025|Assignments|DistancesToClusterCenter no.0|DistancesToClusterCenter no.1|DistancesToClusterCenter no.2
---------------|----|----|-----------|-----------------------------|-----------------------------|-------------------------------
820|H|B|0|1.017651|1.439397|1.847701
760|F|A|2|1.732325|2.010795|1.16811
720|F|B|0|0.994871|1.424166|1.829158
420|H|B|0|1.009844|1.420353|1.823016
540|J|B|0|1.032852|1.414264|1.823316

## Realizando a Análise dos Dados do ENADE
Para o desafio 3 e 4 foram utilizados datasets de microdados do ENADE. Disponível em: 
https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/enade

## Desafio 3 - (3 pontos) Clusterizando os dados do ENADE

##### Pré-processamento dos dados:
O dataset foi colocado em um conjunto de dados sendo utilizado apenas as seguintes colunas: QE_I08, QE_I23, NT_GER.
Através do Designer do estúdio do Azure Machine Learning fiz o tratamento do dataset usando o módulo _**"Clean Missing Data"**_ aqui também considerei que fosse necessário a retirada das linhas que estivessem com dados vazios.  O módulo _**"Split Data"**_ foi usado novamente com as propriedades de Split Rows com a fração de 0.7, de forma que 70% das linhas são usadas para o treinamento e 30% para avaliar o modelo, as linhas foram separadas de forma aleatória e usando a "Random seed" padrão do módulo.

##### Treinando o modelo: 
Foi pedido uma previsão do cluster de um estudante com base nos dados de Renda Mensal (QE_I08), suas horas de estudo (QE_I23) e sua nota geral (NT_GER). Foi usado o módulo o  _**"Train Clustering Model"**_ e o algoritmo _**"K-Means Clustering"**_ (Parâmetros padrão do módulo), o resultado foi ligado ao módulo _**"Assign Data to Cluster"**_ e então o módulo _**"Evaluate Model"**_ para avaliar o modelo. 

Obtive os seguintes resultados:
Result Description|Average Distance to Other Center|Average Distance to Cluster Center|Number of Points|Maximal Distance to Cluster Center
------------------|--------------------------------|----------------------------------|----------------|-----------------------------------
Evaluation For Cluster No.0|1.961089|1.587573|81667|1.823156
Evaluation For Cluster No.1|1.763879|1.338415|48512|1.47732
Combined Evaluation|1.887598|1.494723|130179|1.823156

Foi então criada uma pipeline de inferência em tempo real com as seguintes entradas e saídas:
NU_NOTA_REDACAO|Q006|Q025|Assignments|DistancesToClusterCenter no.0|DistancesToClusterCenter no.1|DistancesToClusterCenter no.2
---------------|----|----|-----------|-----------------------------|-----------------------------|-------------------------------

