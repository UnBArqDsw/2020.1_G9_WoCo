# Estudo GRASPs

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|06/10/2020|   0.1    | Iniciando o documento       |  Weiller Fernandes  |
|06/10/2020|   0.2    | Adicionando os GRASPs Criador e Especialista | Weiller Fernandes |

## 1. Introdução

Padrões de projetos são princípios e soluções usados durante a criação de um software, codificado em um formato estruturado e bem estabelecido, descrevendo o problema no qual se deseja mitigar, bem como uma solução para esse problema. No caso do software, essa solução pode vir através de modelagem e código.
O Grasp é um padrão de projeto, onde se analisa o desenho do software para que seja possível identificar as entidades envolvidas, bem como suas responsabilidades e dessa forma, seja feita uma solução adequada para a resolução de um determinado problema.

Esse documento busca formalizar o estudo inicial dos GRASPs, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Padrões GRASP

### Criador

Padrão de projeto que representa classes responsáveis por criar instâncias. No caso do aplicativo WoCo, uma classe que pode ser um creator é a classe de Usuário, que é responsável por criar o usuário, seja ele treinador ou atleta, dentro da plataforma. Outra opção de criador é a classe Treinador, pois essa classe é responsável por criar treinos e dietas que ficarão disponíveis para o atleta.

![CREATOR](../img/creator.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Especialista

Padrão de projeto que se preocupa em atribuir responsabilidades para a entidade mais especialista em um dado aspecto. Um exemplo de especialista no WoCo é a Classe Dieta, que é especialista em calcular a quantidade total de calorias de uma dieta, por exemplo. Nesse caso a classe Alimentação deve fornecer o valor das calorias de cada alimento para a classe Dieta, e essa calcula o valor total com base nos dados fornecidos pela classe Alimentação.

![ESPECIALISTA](../img/especialista.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158
