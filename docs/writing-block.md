# RELATÓRIO — ANÁLISE DE AGRUPAMENTO COM K-MEANS

**Trabalho:** TDE 4 — Matemática Aplicada à Computação  
**Tema:** Aplicação do algoritmo K-Means ao conjunto de dados Dow Jones Index  
**Integrantes:** Rafael Bernardo e Paulo Tavares

---

## 1. Introdução

A análise de agrupamento, também conhecida como *clustering*, é uma técnica de aprendizado de máquina não supervisionado utilizada para identificar grupos de observações com características semelhantes.

Neste trabalho, foi aplicado o algoritmo K-Means ao conjunto de dados Dow Jones Index, disponibilizado pelo UCI Machine Learning Repository. O objetivo foi identificar padrões entre observações semanais de ações de empresas que compõem o índice Dow Jones, considerando características relacionadas ao volume de negociação, variação dos preços e dividendos.

## 2. Base de dados

O conjunto Dow Jones Index possui 750 observações semanais e 16 atributos, referentes a 30 empresas.

Para a análise, foram selecionados quatro atributos numéricos:

- **Volume (`volume`):** quantidade de ações negociadas.
- **Variação percentual do preço (`percent_change_price`):** variação percentual do preço da ação.
- **Dias até o próximo dividendo (`days_to_next_dividend`):** quantidade de dias restantes até o próximo pagamento de dividendos.
- **Retorno percentual dos dividendos (`percent_return_next_dividend`):** retorno percentual associado aos dividendos.

Após a seleção das variáveis, foi realizada a verificação de valores ausentes e a preparação dos dados para aplicação do algoritmo.

## 3. Preparação dos dados

Como os atributos possuem escalas e unidades diferentes, foi utilizada a técnica de padronização `StandardScaler`, disponibilizada pela biblioteca Scikit-learn.

Essa transformação coloca as variáveis em escalas comparáveis, evitando que atributos com valores numericamente maiores tenham influência desproporcional no cálculo das distâncias.

A análise foi realizada utilizando os quatro atributos selecionados e as 750 observações disponíveis.

## 4. Aplicação do algoritmo K-Means

O algoritmo K-Means foi aplicado com inicialização K-Means++, utilizando 10 inicializações para cada quantidade de grupos testada.

Foram avaliados valores de K entre 1 e 10 por meio da inércia, e valores entre 2 e 10 pelo coeficiente médio de Silhouette.

### 4.1 Método do Cotovelo

O método do Cotovelo utiliza a inércia, que representa a soma das distâncias quadráticas entre as observações e os centroides de seus respectivos grupos.

Os valores obtidos foram:

| K | Inércia |
|---|---:|
| 1 | 3000,00 |
| 2 | 2416,53 |
| 3 | 1922,46 |
| 4 | 1540,51 |
| 5 | 1244,70 |
| 6 | 1117,40 |
| 7 | 998,80 |
| 8 | 924,55 |
| 9 | 873,71 |
| 10 | 820,67 |

Observa-se que a inércia diminui conforme a quantidade de clusters aumenta. Entretanto, a redução da inércia, isoladamente, não determina a melhor quantidade de grupos. Por isso, também foi utilizado o coeficiente de Silhouette.

### 4.2 Coeficiente de Silhouette

O coeficiente de Silhouette avalia a qualidade dos agrupamentos considerando a proximidade das observações dentro do próprio cluster e sua separação em relação aos demais grupos.

Os resultados obtidos foram:

| K | Silhouette médio |
|---|---:|
| 2 | 0,4447 |
| 3 | 0,4651 |
| 4 | 0,2303 |
| 5 | 0,2543 |
| 6 | 0,2258 |
| 7 | 0,2535 |
| 8 | 0,2322 |
| 9 | 0,2365 |
| 10 | 0,2337 |

O maior valor foi obtido com K = 3, apresentando Silhouette médio de aproximadamente 0,4651.

O resultado para K = 2 também apresentou um valor próximo, de 0,4447. Dessa forma, K = 3 foi selecionado por apresentar o melhor resultado entre as configurações avaliadas, embora a diferença em relação a K = 2 seja relativamente pequena.

## 5. Resultado do agrupamento

Com base na avaliação realizada, foi treinado o modelo final utilizando três clusters.

A distribuição das observações foi:

| Cluster | Quantidade | Percentual |
|---|---:|---:|
| Cluster 0 | 25 | 3,33% |
| Cluster 1 | 663 | 88,40% |
| Cluster 2 | 62 | 8,27% |
| **Total** | **750** | **100%** |

A distribuição demonstra que os grupos possuem tamanhos bastante diferentes, com concentração significativa das observações no Cluster 1.

### 5.1 Interpretação dos centroides

Para facilitar a interpretação, os centroides foram convertidos novamente para as unidades originais dos atributos.

| Atributo | Cluster 0 | Cluster 1 | Cluster 2 |
|---|---:|---:|---:|
| Volume | 47,44 milhões | 82,73 milhões | 518,17 milhões |
| Variação do preço (%) | 0,175 | 0,205 | -1,651 |
| Dias até próximo dividendo | 252,04 | 46,02 | 41,66 |
| Retorno dos dividendos (%) | 0,972 | 0,719 | 0,293 |

A partir desses valores, é possível observar diferenças entre os grupos:

**Cluster 0:** apresenta o menor volume médio de negociação, maior distância média até o próximo dividendo e maior retorno percentual dos dividendos.

**Cluster 1:** concentra a maior parte das observações. Apresenta volume médio intermediário, variação média positiva do preço e características de dividendos próximas às observadas no conjunto geral.

**Cluster 2:** apresenta volume médio de negociação significativamente superior aos demais grupos e variação média negativa do preço. Também possui o menor retorno percentual médio dos dividendos.

Essas características demonstram que o algoritmo identificou grupos com comportamentos distintos considerando os atributos selecionados.

### 5.2 Análise complementar das empresas

Após a formação dos clusters, foi realizada uma análise complementar utilizando a identificação das empresas presentes na base original.

Essa análise indicou que as 25 observações do Cluster 0 pertencem à empresa Disney (DIS). O Cluster 2 apresentou observações de empresas como Bank of America (BAC) e Cisco (CSCO), entre outras.

Essa verificação foi utilizada apenas para auxiliar a interpretação dos grupos, não fazendo parte das variáveis utilizadas no treinamento do K-Means.

## 6. Visualização dos agrupamentos

Para facilitar a interpretação visual dos resultados, foi utilizada a técnica de Análise de Componentes Principais (PCA).

A PCA permite representar os dados em duas dimensões, preservando parte da informação presente nos quatro atributos originais.

As duas componentes principais explicaram aproximadamente 59,41% da variância dos dados.

A visualização permite observar a distribuição das observações e a separação entre os três clusters. Entretanto, como existe perda de informação ao reduzir quatro dimensões para duas, a representação gráfica deve ser interpretada como uma aproximação dos agrupamentos originais.

## 7. Limitações da análise

Apesar dos resultados obtidos, algumas limitações devem ser consideradas:

- **Desbalanceamento dos clusters:** o Cluster 1 concentra 88,40% das observações, enquanto o Cluster 0 possui apenas 3,33%.
- **Distribuição do volume:** o atributo volume apresenta grande variação entre as observações, podendo influenciar a formação dos grupos.
- **Características do K-Means:** o algoritmo utiliza distâncias euclidianas e tende a funcionar melhor em agrupamentos com formatos relativamente compactos.
- **Observações repetidas por empresa:** a base contém registros semanais das mesmas 30 empresas. Portanto, as 750 observações não representam 750 empresas independentes.
- **Escolha de K:** embora K = 3 tenha apresentado o maior Silhouette médio, o resultado de K = 2 foi próximo, indicando que a escolha não deve ser considerada absoluta.

Essas limitações demonstram a importância de interpretar os resultados considerando as características da base e do algoritmo utilizado.

## 8. Conclusão

A aplicação do algoritmo K-Means ao conjunto Dow Jones Index permitiu identificar três grupos de observações com características distintas relacionadas ao volume de negociação, à variação dos preços e aos dividendos.

A avaliação por meio do método do Cotovelo e do coeficiente de Silhouette indicou K = 3 como a configuração de melhor desempenho entre as avaliadas, com Silhouette médio de aproximadamente 0,4651.

A interpretação dos centroides permitiu identificar diferenças entre os grupos, especialmente em relação ao volume de negociação e ao comportamento dos preços.

Entretanto, o desbalanceamento dos clusters e as características das observações semanais demonstram que os resultados devem ser interpretados com cautela.

Conclui-se que o K-Means foi capaz de identificar padrões na base analisada, demonstrando a aplicação prática de técnicas de aprendizado de máquina não supervisionado na análise de dados financeiros.

## 9. Referências

UCI MACHINE LEARNING REPOSITORY. **Dow Jones Index Data Set**. Disponível em: https://archive.ics.uci.edu/dataset/312/dow+jones+index.

SCIKIT-LEARN. **KMeans**. Documentação oficial. Disponível em: https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html.

SCIKIT-LEARN. **Silhouette Score**. Documentação oficial. Disponível em: https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html.