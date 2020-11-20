# seletor_subsea
Algoritmo de seleção rápida para sistemas de distribuição em arranjo submarino.
O algoritmo testa o conceito de poços satélite, o uso de manifolds de serviço e gas-lift com compartilhamento de umbilical, o uso de manifolds de injeção com compartilhamento de umbilical e o uso de unidades de distribuição somente de umbilical.
Também é otimizada a posição da plataforma, a posição de cada manifold selecionado e a quais poços estes estão alocados.

### Dados de entrada:
Um arquivo chamado "pocos.csv" que contém as coordenadas dos poços (em algum sistema de projeção geográfica) e o seu tipo, se produtor (P), injetor de água (A), injetor de gás (G) ou injetor alternado de água e gás (W).
As linhas devem ser no formato:
Nome do poço, coordenada X, coordenada Y, Tipo do poço

Um arquivo chamado "custoeq.csv" 
