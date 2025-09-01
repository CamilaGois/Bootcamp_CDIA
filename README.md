Projeto Final do Bootcamp CDIA - Sistema Inteligente de Manutenção Preditiva

## CONTEXTUALIZAÇÃO ##
Projeto Final do Bootcamp de Ciências de Dados e Inteligência Artificial. O objetivo é criar um sistema capaz de identifi car as falhas que venham a ocorrer, e se possível, qual foi o tipo da falha. Cada amostra no conjunto de dados é composta por 8 atributos que descrevem o comportamento de desgaste da máquina e do ambiente. Além dessas características, cada amostra é rotulada com uma das 5 possíveis classes de defeitos.

## Tratamentos Realizados ##
Limpeza e Conversão de Colunas Binárias: As colunas que representam falhas binárias (como FDF, FDC, FP, FTE, FA e falha_maquina) apresentavam inconsistências. 

Tratamento de Valores Nulos: A coluna desgaste_da_ferramenta continha valores nulos. Para preencher essas lacunas, foi utilizado o SimpleImputer com a estratégia de imputação pela média.

Remoção de Outliers: Utilizando o método do Interquartile Range (IQR), foram identificados e removidos outliers para cada classe de falha, garantindo que o modelo não fosse excessivamente influenciado por pontos de dados atípicos.

Remoção de Colunas Constantes: A coluna umidade_relativa foi identificada como constante, ou seja, todos os seus valores eram idênticos. Colunas com essa característica não fornecem informações relevantes para a modelagem e, portanto, foram removidas do dataset.

Interpretação de Valores Negativos: A coluna temperatura_ar apresentou valores negativos, o que, a princípio, pode parecer um erro. No entanto, após uma análise cuidadosa do contexto, foi interpretado que esses valores poderiam representar anomalias ou falhas de sensores, sendo tratado como um dado válido para o modelo. No caso do modelo XGBoost, ele é robusto a essas variações, o que evita a necessidade de remoção ou imputação de forma drástica.

Transformação para Problema de Classificação Multiclasse: As múltiplas colunas binárias de falhas (FDF, FDC, FP, FTE, FA) foram consolidadas em uma única coluna chamada classe_defeito. Essa abordagem converteu o problema de uma classificação multirrótulo para uma classificação multiclasse, simplificando a modelagem.

Criação de Novas Features: Foram criadas novas features combinadas, como razões e produtos de variáveis existentes (torque e velocidade_rotacional), para capturar interações entre as variáveis e enriquecer a representatividade dos dados.

##Tipo de Modelagem e Modelo Utilizado##
O problema foi abordado como uma tarefa de classificação multiclasse. O modelo escolhido foi o XGBoostClassifier, conhecido por sua alta performance e robustez. O modelo foi integrado em um pipeline completo para automatizar o fluxo de trabalho.

Pipeline de Pré-processamento: Um ColumnTransformer foi utilizado para aplicar transformações específicas a colunas numéricas (por exemplo, padronização) e categóricas (como One-Hot Encoding).

Balanceamento de Classes: Para lidar com o desbalanceamento de classes, a técnica SMOTE (Synthetic Minority Over-sampling Technique) foi aplicada no conjunto de treino. Isso ajudou a criar amostras sintéticas para as classes minoritárias, nivelando a distribuição e melhorando o desempenho do modelo nessas classes.

##Métricas de Avaliação##
As seguintes métricas foram utilizadas para avaliar o desempenho do modelo:

F1-Score por Classe (Micro Análise): Avalia a precisão e a revocação para cada classe individualmente.

F1-Macro (Macro Análise): Considera a média do F1-Score de cada classe. Essa métrica é ideal para conjuntos de dados desbalanceados, pois dá a mesma importância para as classes minoritárias e majoritárias, fornecendo uma visão mais realista do desempenho global do modelo.

Matriz de Confusão: Uma visualização que detalha os acertos e erros do modelo, permitindo identificar quais classes estão sendo confundidas.

A validação foi realizada usando a validação cruzada (cross-validation) com 5 folds e a métrica f1_macro, garantindo que o desempenho do modelo fosse consistente e não dependesse de uma partição específica dos dados.

#Desbalanceamento de Classes#
O dataset original apresentou um forte desbalanceamento, com algumas classes de falha sendo muito mais raras do que outras.
Aplicação de SMOTE: O SMOTE foi fundamental para balancear as classes no conjunto de treino, fornecendo ao modelo dados mais representativos das classes minoritárias.


