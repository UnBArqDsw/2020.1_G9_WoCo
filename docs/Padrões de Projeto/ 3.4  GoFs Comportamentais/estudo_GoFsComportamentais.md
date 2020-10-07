# Estudo GoFs Comportamentais

## 1. Introdução

OS Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

Esse documento busca formalizar o estudo inicial dos GoFs, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Padrões GoFs Comportamentais

### Template Method

Esse método tem por característica a definição do esqueleto de um algoritmo em uma operação, adiando algumas etapas para as subclasses. O método de modelo permite que as subclasses redefinam certas etapas de um algoritmo sem alterar a estrutura do algoritmo.

#### Definição

``` Dart
abstract class Alimento {
   double getCalorias();
}

class Whey implements Alimento {
    @override
    double getCalorias() {
      return 100.00;
    }
}

class Arroz implements Alimento {
  @override
  double getCalorias() {
    return 1000.00;
  }
}
```

#### Aplicação no Woco

Dentro do contexto de uso no WoCo, o Template method pode ser utilizado nos requests http, onde a classe abstrata referencia a url base da chamada e os verbos https necessários e cada endpoint implementa propriamente com suas especificações. E em outros casos, onde possa seja aplicável sobrescrita de métodos para classes com tal padrão.


## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|06/10/2020|   0.1    | Iniciando o documento       |  Eugênio Sales  |