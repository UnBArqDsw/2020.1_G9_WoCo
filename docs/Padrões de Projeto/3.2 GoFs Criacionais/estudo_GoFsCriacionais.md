# Estudo GoFs Cracionais

## 1. Introdução

OS Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

Esse documento busca formalizar o estudo inicial dos GoFs criacionais, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Padrões GoFs Comportamentais

### Factory Method

Uma vez definida uma interface para criar um objeto, as subclasses decidem qual classe instanciar. O Factory Method permite que uma classe adie a instanciação para as subclasses. Ou seja, o padrão de design Factory Method define uma interface para uma classe responsável por criar um objeto, e com isso, adia a instanciação para classes específicas que implementam essa interface.

#### Definição

``` Dart
enum ButtonType {
  raised,
  icon
}

abstract class Btn {
  factory Btn(ButtonType type) {
    switch (type) {
      case ButtonType.raised: return Raised();
      case ButtonType.icon: return Icon();
      default: return null;
    }
  }

 Widget _builBtn();
}

class Raised implements Btn {
  @override
  Widget build() {
    return RaisedButton.icon(
      color: Colors.black,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(18.0),
        side: BorderSide(color: Colors.white)
      )
    )
  }
}

class Icon implements Btn {
  @override
  Widget build() {
    return IconButton(
      icon: Icon(Icons.refresh), 
      onPressed: () {}
    )
  }
}

```

#### Aplicação no Woco

Dentro do contexto de uso no WoCo, o Factory Method pode ser utilizado em widgets onde a instanciação do objeto seja responsabilidade de um factory widget como nesse exemplo de botões, onde o facory Btn instancia o raised button ou o icon button dependendo do parâmetro passado.


## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|11/10/2020|   0.1    | Iniciando o documento       |  Eugênio Sales  |
