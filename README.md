Detecção de Fissuras em Concreto com Machine Learning 🏗️🔍

Este projeto aplica técnicas de Visão Computacional e Machine Learning para detetar fissuras em superfícies de concreto (betão). A automação deste processo é crucial para a inspeção de integridade estrutural na engenharia civil, prevenindo falhas críticas e reduzindo custos de manutenção.

🎯 Objetivo

Construir um pipeline de classificação de imagens capaz de distinguir entre concreto intacto e concreto com rachaduras, justificando matematicamente as decisões do modelo estatístico através de métricas e visualizações gráficas avançadas.

📂 Base de Dados (Dataset)

O projeto utiliza o dataset Surface Crack Detection disponível no Kaggle.
O download é feito diretamente pelo script no Google Colab, através da API do Kaggle, não sendo necessário alojar as imagens localmente neste repositório.

⚙️ Pipeline do Projeto

1. Extração de Características (LBP)

Imagens em bruto não são os melhores dados de entrada para classificadores simples. Para resolver isso, utilizámos o algoritmo Local Binary Pattern (LBP).

O LBP analisa a vizinhança de cada pixel (raio=1, vizinhos=8) e cria um código de microtextura.

O resultado de cada imagem é convertido num histograma de 256 bins (dimensões), representando a "assinatura digital" da textura.

2. Classificação (KNN) e Otimização

O algoritmo escolhido foi o K-Nearest Neighbors (KNN). Para garantir o melhor desempenho, construímos um laboratório de testes executando validações em múltiplas rodadas:

O modelo testou parâmetros de K=1 a K=10 ao longo de 30 provas de divisões aleatórias (train_test_split).

O parâmetro vencedor estatisticamente provado foi K=8.

📊 Resultados e Avaliação Crítica

O modelo alcançou uma acurácia consistente na faixa dos 77,5%. Mais importante do que o número, é a análise clínica dos erros e do comportamento dos dados:

Matriz de Confusão

O modelo (com teste em 20% da base) apresenta uma taxa aceitável de alarmes falsos e falsos negativos. Os erros não se devem a falhas algorítmicas, mas à complexidade inerente do mundo real.

t-SNE: Visualização 3D e a "Sopa de Dados"

Aplicámos o t-SNE para reduzir as 256 dimensões do LBP para apenas 3 (Eixos X, Y, Z).
Ao analisar o gráfico de dispersão gerado pelo Plotly, nota-se que existem agrupamentos isolados de cada classe, mas também uma grande zona central de mistura. Esta "sopa de dados" reflete as situações em que as texturas de superfícies de concreto intacto muito poroso se assemelham matematicamente a fissuras muito finas, justificando visualmente a taxa de erro de ~22% do classificador.

Seleção de Características (ANOVA)

Para eliminar a "caixa preta" da Inteligência Artificial, aplicámos o teste estatístico ANOVA (SelectKBest). O objetivo era descobrir quais das 256 características de textura eram cruciais para o algoritmo.

O teste provou de forma conclusiva que a variância da Barra 14 (Bin 14) do histograma é a principal responsável por delatar uma fissura.

Quando ocorre um pico anómalo na contagem do padrão 14 (uma mudança na frequência, e não apenas a sua presença), o modelo identifica-o como a assinatura máxima de dano estrutural.
