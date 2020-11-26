# Seletor de conceito para arranjo submarino
Algoritmo de seleção rápida para sistemas de distribuição em arranjo submarino.
O algoritmo testa o conceito de poços satélite, o uso de manifolds de serviço e gas-lift com compartilhamento de umbilical, o uso de manifolds de injeção com compartilhamento de umbilical e o uso de unidades de distribuição somente de umbilical.
Também é otimizada a posição da plataforma, a posição de cada manifold selecionado e a quais poços estes estão alocados.
**Não se considera compartilhamento de linhas de produção.**

As bibliotecas usadas são:
- *DEAP* para as funções do algoritmo evolutivo;
- *Pandas* para armazenamento das bases de dados;
- *random* para geração de números aleatórios;
- *matplotlib* para geração de gráficos.

### Dados de entrada
Um arquivo chamado "pocos.csv" que contém as coordenadas dos poços (em algum sistema de projeção geográfica) e o seu tipo, se produtor (P), injetor de água (A), injetor de gás (G) ou injetor alternado de água e gás (W).
As linhas devem ser no formato:
Nome do poço, coordenada X, coordenada Y, Tipo do poço

Um arquivo chamado "custoeq.csv" que contém a base de dados de custos dos equipamentos submarinos considerados e o número máximo de poços que cada equipamento consegue atender.
As linhas devem ser no formato:
Nome (tipo) do equipamento, custo, número máximo de poços

Um arquivo chamado "custolin.csv" que contém a base de dados de custos das linhas (dutos) submarinos considerados.
Os custos se dividem entre uma parte fixa e uma parte variável, que deve ser multiplicada pelo comprimento do duto.
Considera-se que cada duto tem um tipo "principal", que se liga a um sistema de distribuição, e um tipo "derivado", que se liga diretamente a um poço.
As linhas do arquivo devem ser no formato:
Nome (tipo) do duto, custo fixo do duto principal, custo variável do duto principal, custo fixo do duto derivado, custo variável do duto derivado

Mais dois parâmetros entram como constantes no código: a lâmina de água média do arranjo e o número máximo de poços produtores atendidos por sistema de controle compartilhado (SDU). O SDU é o único equipamento, considerado nesse algoritmo, que possui limitação específica.

Os tipos de poços e dutos que se ligam a cada sistema de distribuição submarina foram incluídos diretamentee no código do programa.

### Posicionamento da plataforma

Um algoritmo genético é usado para encontrar a melhor posição da plataforma, usando a biblioteca DEAP.
A função objetivo de minimização desse algoritmo é o somatório das distâncias euclidianas da plataforma para os poços.
Há restrição de que todos os poços devem estar fora do raio de ancoragem, definido como **1.4 * LDA**. Essa restrição é modelada, no algoritmo, como uma penalidade fixa.
As variáveis são as coordenadas X e Y da plataforma, portanto um vetor de tamanho 2 e formato *float*.
- A população é de 100 indivíduos (posições)
- O cruzamento é do tipo mistura (*cxBlend*) com *alpha* 0.3 e taxa de cruzamento 60%
- A mutação é do tipo gaussiana (*mutGaussian*), com média 0, desvio-padrão 500, probabilidade independente 100% e taxa de mutação 15%
- A seleção é do tipo torneio (*selTournament*) entre 4 indivíduos
- A evolução leva 40 gerações

### Cálculos

A primeira estimativa do número máximo de sistemas de distribuição é escolhida da seguinte forma:
**1.5 * Número de poços no arranjo que podem atendidos pelo equipamento / número máximo de poços que cada equipamento pode atender.**
O número encontrado é então arredondado para o inteiro imediatamente superior.

A localização de um dado manifold é calculada pela média das posições da plataforma e de todos os poços ligados a este, ponderada pela razão entre o custo variável do duto principal e do derivado.

### Custo total de um arranjo

O custo total do arranjo é um somatório dos custos de equipamentos e dutos selecionados, de acordo com o comprimento, pela função *CustoTotal*
Equipamentos e linhas "inevitáveis", ou seja, presentes em qualquer arranjo e que, por isso, não contribuem para a otimização, não são calculados; por exemplo: árvores de natal e dutos de produção satélite.
Para os poços ligados diretamente à plataforma, calcula-se o custo (com comprimento do poço à plataforma) de todas as linhas que as atendem, de acordo com o tipo do poço e a base de dados de dutos.
Inclui-se também um custo de *riser* definido como *1.5 * LDA*
Para os poços ligados a manifolds, calcula-se, o custo dos dutos derivados (comprimento do poço ao manifold), o custo do próprio manifold e o custo das linhas principais que este distrbui (comprimento do manifold à plataforma, incluindo riser).

## Restrições de um arranjo

A verificação se o arranjo atende às restrições é feita pela função *FuncaoDeRestricao*.
As restrições são:
- Número máximo de poços por manifold, de acordo com a base de dados de equipamentos
- Tipo de poço ligado ao tipo correto de manifold
- Número máximo de poços produtores ligados a um SDU
No algoritmo, utiliza-se uma penalidade na função objetivo, definida como *10 * LDA * número de poços * custo variável da linha mais cara*.
Além disso, a função *DistanciaRestricao* calcula uma penalidade adicional de acordo com o número de restrições que são violadas pelo arranjo.

## Algoritmo genético da alocação de poços

Cada arranjo representa um indivíduo no algoritmo genético, sendo modelado por um vetor de números inteiros.
Cada posição do vetor corresponde a um poço, e seu valor corresponde a qual manifold este está ligado.
A função objetivo, de minimização, é o custo total do arranjo.
Os demais parâmetros do algoritmo evolutivo são:
- População de 120 indivíduos
- Cruzamento uniforme (*cxUniform*) com probabilidade independente de 30% e taxa de cruzamento 60%
- Mutação uniforme de números inteiros (*mutUniformInt*) com probabilidade independente de 15% e taxa de mutação de 30%
- Seleção por torneio (*selTournament*) entre 4 indivíduos
- 40 gerações

## Saída do algoritmo

O algoritmo dá como resposta o arranjo, como vetor de números inteiros, que teve menor custo total na evolução.
Também é gerado um *Dataframe* com as posições da plataforma e de todos os sistemas de distribuição utilizados no arranjo selecionado, no mesmo sistema de coordenadas dos poços.
Otimizações posteriores podem ser realizadas utilizando algoritmos especializados.
