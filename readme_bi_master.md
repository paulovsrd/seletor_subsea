# Seletor de arranjo submarino por Algoritmo Evolutivo

#### Aluno: [Paulo Vinicius Soares Ramalho Domingos](https://github.com/paulovsrd)
#### Orientadora: [Ana Carolina Alves Abreu](https://github.com/acarolina1612)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/paulovsrd/seletor_subsea). <!-- caso não aplicável, remover esta linha -->

---

### Resumo

<!-- trocar o texto abaixo pelo resumo do trabalho -->

Algoritmo de seleção rápida para sistemas de distribuição em arranjo submarino através de uma otimização de custo total. O algoritmo testa o conceito de poços satélite, o uso de manifolds de serviço e gas-lift com compartilhamento de umbilical, o uso de manifolds de injeção com compartilhamento de umbilical e o uso de unidades de distribuição somente de umbilical. Também é otimizada a posição da plataforma, a posição de cada manifold selecionado e a quais poços estes estão alocados. Não se considera compartilhamento de linhas de produção.

As entradas são as coordenadas espaciais e tipos dos poços, bem como bases de dados de custos de equipamentos e linhas submarinas. Cada tipo de sistema de distribuição pode compartilhar linhas específicas, que servem poços de tipos específicos. A função objetivo da otimização é o custo total do arranjo, considerando apenas os custos que mudam entre uma opção e outra, portanto, os custos de linhas de produção satélite não estão inclusos. O método de solução é um Algoritmo Evolucionário. Cada indivíduo é um vetor de números inteiros, no qual cada posição representa um poço e seu número associado representa a qual sistema de distribuição submarina este está conectado.

Ao final, a melhor solução que atende a todas as restrições pode ser utilizada em outros algoritmos para um refinamento do arranjo submarino.

---

Matrícula: 192.190.065

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
