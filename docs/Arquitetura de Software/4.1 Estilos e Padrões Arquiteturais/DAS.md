# Documento de Arquitetura de Software


#### Histórico de revisões
|    Data    | Versão |       Descrição       |    Autor(es)     |
| :--------: | :----: | :-------------------: | :--------------: |
| 29/10/2020 | 0.1 | Iniciando o documento e adicionando o Sumário | Weiller Fernandes |
| 29/10/2020 | 0.2 | Adicionando Representação Arquitetural | Weiller Fernandes |
| 03/11/2020 | 0.3 | Adicionando Visão de Casos de Uso | Weiller Fernandes |
| 03/11/2020 | 0.4 | Adicionando Visão lógica | Eugênio Sales |
| 03/11/2020 | 0.5 | Adicionando Visão de Implementação | Davi Alves |
|03/11/2020|0.6|Adicionando Introdução e Visão de Deploy| Bruno Duarte|
|20/11/2020|1.0|Atualizando a Introdução de forma coerente| Bruno Duarte|


## Sumário
  - [1. Introdução](#1-introducao)
    - [1.1 Objetivo](#11-objetivo)
    - [1.2 Escopo](#12-escopo)
    - [1.3 Definições, Acrônimos e Abreviações](#13-definicoes-acronimos-e-abreviacoes)
    - [1.4 Referências](#14-referencias)
    - [1.5 Visão Geral](#15-visao-geral)
  - [2. Representação Arquitetural](#2-representacao-arquitetural)
  - [3. Restrições e Metas Arquiteturais](#3-restricoes-e-metas-arquiteturais)
  - [4. Visão de Casos de Uso](#4-visao-de-casos-de-uso)
  - [5. Visão Lógica](#5-visao-logica)
  - [6. Visão de Processo](#6-visao-de-processo)
  - [7. Visão de Implementação](#7-visao-de-implementacao)
  - [8. Visão de Deploy](#8-visao-de-deploy)
  - [9. Qualidade](#9-qualidade)

## 1. Introdução

### 1.1 Objetivo
Este documento visa esclarecer, especificar e documentar as decisões de arquitetura na produção e implementação do projeto WoCo - Workout Controller relatando também os aspectos da aplicação e comunicação de serviços operantes.


### 1.2 Escopo
O WoCo é uma aplicação voltada para o monitoramento e acompanhamento de treino físico dos usuários. Aplicação desenvolvida na disciplina Arquitetura e Desenho de Software, na Universidade de Brasília Campus Gama (UnB).

### 1.3 Definições, Acrônimos e Abreviações
* **DAS**: Desenho e Arquitetura de Software
* **UnB**: Universidade de Brasília
* **PaaS**: Plataform as a Service

### 1.4 Referências

- [DAS Template](http://sce.uhcl.edu/helm/RationalUnifiedProcess/webtmpl/templates/a_and_d/rup_sad.htm)

### 1.5 Visão Geral
A arquitetura utilizada foi a arquiteura ***N Camadas***, ou do inglês, ***Multitier Architecture*** que por sua vez é uma arquitetura ***client-server*** na qual as funções de apresentação, processamento e gerenciamento de dados são fisicamente separadas. A aplicação será realizada utilizando comunicação entre cada componente, onde cada evento irá realizar uma requisição de acesso/escrita no banco através do backend e o feedback será entregue ao usuário em sua tela de aplicação. Escolhemos esta arquitetura por uma série de fatores sendo um deles a possibilidade de criar aplicações flexíveis e reutilizaveis. No decorrer do documento iremos abordar cada uma das camadas presentes na aplicação, a camada lógica, camada de apresentação e a de armazenamento de dados.
 
## 2. Representação Arquitetural

Este tópico descreve a arquitetura de software da aplicação e como ela é representada, através das tecnologias escolhidas para cada uma das três partes principais do app, são elas: O Back-end, Front-end e o Banco de Dados. 
## 2.1. Front-end

Essa é parte visual da aplicação. É responsável pela comunicação com o Back-end e é através dela que o usuário é capaz de interagir com o aplicativo em si. Para o desenvolvimento do WoCo, a equipe optou por desenvolver uma aplicação mobile, pois esse tipo de aplicação se adequa melhor ao escopo do projeto.

### 2.1.1 Tecnologias

[![Flutter](../img/flutter.png)](../img/flutter.png)

A definição do Flutter em seu site oficial é: "Flutter é um kit de ferramentas do Google para construir aplicações lindas, nativamente compiladas para mobile, web, desktop à partir de um único código-base."

O Flutter é um framework construído pela Google para facilitar o desenvolvimento mobile multiplataforma (Android/iOS) que tem o Dart como principal linguagem de desenvolvimento. Ele utiliza uma abordagem até então única para lidar com os componentes nativos de cada plataforma, em que cada um deles é implementado pelo próprio framework e apresentado ao usuário por um motor de renderização próprio.

O Flutter foi escolhido por ser uma tecnologia familiar para parte da equipe de desenvolvimento do projeto e por permitir um aprendizado relativamente rápido para aqueles que ainda não dominavam o framework. Outro aspecto levado em consideração nessa tomada de decisão é que o Flutter é mais rápido que o React Native, seu concorrente direto, a partir do momento em que ele não exige que o Javascript faça uma ponte para interagir com os componentes nativos do sistema.

## 2.2. Back-end

Essa é a camada de integração entre o Front-end e o Banco de Dados. Sua função é armazenar e disponibilizar os dados da aplicação para a visualização por parte do usuário.

### 2.2.1 Tecnologias

[![Flask](../img/flask.png)](../img/flask.png)

O Flask é um micro-framework web escrito em Python e baseado na biblioteca WSGI Werkzeug e na biblioteca de Jinja2. Ele tem a flexibilidade da linguagem de programação Python e provê um modelo simples para desenvolvimento web.
Ele é chamado de micro-framework porque mantém um núcleo simples mas extensível. Não há uma camada de abstração do banco de dados, validação de formulários, ou qualquer outro componente onde bibliotecas de terceiros existem para prover a funcionalidade, porém o Flask suporta extensões capazes de adicionar tais funcionalidades na aplicação final. Há uma vasta coleção de bibliotecas para resolver essas questões em Python, isso simplifica o framework e torna sua curva de aprendizado mais suave.

Esses fatores inclusive, foram determinantes para a escolha do Flask, sua simplicidade e curva de aprendizado suave tornam o seu uso no projeto WoCo algo bastante tranquilo e satisfatório.

## 2.3. Banco de Dados

Essa é a camada de persistência de informações. É ela que fornece para o back-end as informações e dados que serão exibidos na tela do usuário através do front-end.

### 2.3.1 Tecnologias

[![SQLite](../img/sqlite.png)](../img/sqlite.png)

SQLite é uma biblioteca de código aberto (open source) desenvolvido na linguagem C que permite a disponibilização de um pequeno banco de dados na própria aplicação, sem a necessidade de acesso a um SGDB separado. A estrutura de banco junto com a aplicação é denominada de “banco de dados embutido” e é indicada para aplicações de pequeno porte, que utilizam poucos dados.

O SQLite foi escolhido pelo fato do WoCo ser uma aplicação simples, um projeto de pequena escala. Dessa forma, o SQLite satisfaz todas as necessidades do projeto.

## 3. Restrições e Metas Arquiteturais

### 3.1 Metas Arquiteturais
| Metas |  |    
|:-------:|:--------:|
| Usabilidade | O aplicativo deve ser interativo e deve ser responsivo |
| Portabilidade | O aplicativo deve ser multiplataforma (Android e IOS) |
| Segurança | A aplicação deve prover confidencialidade e privacidade dos dados sensíveis dos usuários |

### 3.2 Restrições Arquiteturais
| Restrições |  |    
|:-------:|:--------:|
| Acesso | O aplicativo deve possuir acesso a internet para acesso dos recursos |
| Acesso | O aplicativo não possuirá acesso a dados offline com banco de dados local |
| Plataforma | O aplicativo não possuirá portabilidade para outras plataformas além de Android e IOS |
| Linguagem | A aplicação possuíra, apenas, a língua portuguesa como opção |


## 4. Visão de Casos de Uso

### 4.1 Atores

|    Atores    |       Descrição       |
| :--------:   |:-------------------:  |
| Treinador   |  Usuário responsável por criar e adicionar os treinos e exercícios que serão usados pelos Usuários Atletas da plataforma.   |
| Atleta      | Usuário que deseja usar a plataforma para monitorar e controlar seus treinos do dia a dia.|
| WoCo        | Responsável por autenticar os usuários e apresentar treinos e exercícios cadastrados na plataforma. |

### 4.2 Diagrama de Casos de uso

**Versão 1.0**

[![Diagrama de Casos de Uso](../img/use_case_v1.png)](../img/use_case_v1.png)

### 4.3 Descrição dos casos de Uso

| Caso de Uso | Descrição |
| :-------:   | :-------: |
| UC01 - Realizar cadastro/login no app   | O usuário se cadastra na aplicação ou realiza o login se já possuir cadastro. |
| UC02 - Deslogar do app| O usuário logado "desloga" da aplicação.|
| UC03 - Criar/editar exercício| O treinador cria um exercício novo ou edita um existente. |
| UC04 - Adicionar nome | O treinador adiciona um nome ao exercício que está sendo criado/editado.|
| UC05 - Selecionar grupo muscular| O treinador seleciona um grupo muscular para o exercício que está sendo criado/editado.|
| UC06 - Adicionar imagem de execução correta| O treinador adiciona uma imagem ao exercício que está sendo criado/editado, demonstrando como é a forma correta de realizar esse exercício.|
| UC07 - Salvar exercício| O treinador salva o exercício que criou/editou.|
| UC08 - Excluir exercício| O treinador exclui um exercício que havia criado.|
| UC09 - Criar/editar treino | O treinador cria um treino novo ou edita um existente.|
| UC10 - Adicionar título|O treinador adiciona um título ao treino que está sendo criado/editado.|
| UC11 - Adicionar descrição| O treinador adiciona uma descrição ao treino que está sendo criado/editado.|
| UC12 - Adicionar/excluir exercícios| O treinador adiciona ou exclui exercícios do treino que está sendo criado/editado.|
| UC13 - Salvar treino| O treinador salva o treino que criou/editou.|
| UC14 - Excluir treino | O treinador exclui um treino que havia criado.|
| UC15 - Listar exercícios| O atleta lista todos os exercícios cadastrados na aplicação.|
| UC16 - Filtrar exercícios por grupo muscular| O atleta filtra os exercícios da aplicação para visualizar apenas os que pertencem a um ou mais grupos musculares.|
| UC17 - Listar treinos| O atleta lista todos os treinos cadastrados na aplicação.|
| UC18 - Ver treinos sugeridos| O atleta visualiza os treinos sugeridos para o seu perfil (iniciante, intermediário ou avançado).|
| UC19 - Iniciar treino | O atleta escolhe um treino listado para poder iniciar.|
| UC20 - Interromper treino| O atleta interrompe um treino que havia iniciado.|

## 5. Visão Lógica
A visão lógica é uma organização conceitual de um software em termos de camadas, subsistemas, pacotes, frameworks, classes, interfaces e realizaçõesde caso de uso. Dentre possíveis representações, segue:

### 5.1 Diagrama de pacotes
![Diagrama de Pacote](../img/package-diagram-v2.png)

## 6. Visão de Processo
A <i>Visão de Processos</i> mostra como será feito o modelo de projeto, tendo como base uma visualização em sequência.

### Diagrama de Sequência

![Diagrama de Sequencia](../img/diagrama_sequenciav2.png)

## 7. Visão de Implementação

Uma visualização arquitetural chamada visão de implementação ilustra a distribuição do processamento em um conjunto de nós do sistema, incluindo a distribuição física dos processos e encadeamentos. Ela mostra como o sistema proposto será implementado, ou seja, uma das suas principais características é a visão geral do Diagrama de Classes do projeto. A finalidade da visão de implementação é captar as decisões de arquitetura tomadas para a implementação.

A visão da implementação é uma das cinco visões de arquitetura de um sistema. As outras visões de arquitetura são a visão lógica, a visão de caso de uso, a visão de processos e a visão de implementação.

### 7.1 Diagrama de Classes

![Diagrama de Classes](../img/umlClass3.png)

## 8. Visão de Deploy

### 8.1 Heroku
Heroku é um dos primeiros provedores de PaaS, caracterizado por suportar muitas outras linguagens de programação populares, por exemplo, Java, Node.js, Scala, Python, PHP e Go. Nosso deploy visa utilizar a plataforma como ponto de acesso aos serviços e requisições da aplicação.
### 8.2 Flask
Flask é um pequeno framework web escrito em Python e baseado na biblioteca WSGI Werkzeug e na biblioteca de Jinja2, possui a flexibilidade da linguagem de programação Python e provê um modelo simples para desenvolvimento web. Uma vez importando no Python, Flask pode ser usado para economizar tempo construindo aplicações web.
### 8.3 Exemplificando
Utilizando as tecnologias citadas iremos através da criação de API, efetuar comunicações web utilizando o Heroku com o backend baseado na tecnologia do framework flask em python. A imagem abaixo tenta ilustrar essas idealizaçoes:

![Image](./../img/deploy.png)

## 9. Qualidade

## 9.1 NFR

O <i>NFR</i> rastreia os requisitos não funcionais, e mostra o impacto que um <i>hardgoal</i> causa em um <i>softgoal</i>. Sempre deixando claro a rastreabilidade e o propósito de um requisito. Tendo como padrão a <i>ISO 9241</i> e as <i>Heurísticas de Nielsen</i>

![NFR](../img/NFR.png)
