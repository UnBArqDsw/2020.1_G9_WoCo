# Estudo GoFs Cracionais

## 1. Introdução

Os Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

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

#### Aplicação no WoCo

Dentro do contexto de uso no WoCo, o Factory Method pode ser utilizado em widgets onde a instanciação do objeto seja responsabilidade de um factory widget como nesse exemplo de botões, onde o facory Btn instancia o raised button ou o icon button dependendo do parâmetro passado.

### Abstract Factory

A ideia por trás do Abstract Factory é prover uma interface para criação de família de objetos dependentes ou relacionados. É bastante similar ao Factory Method visto anteriormente, porém a diferença principal entre os dois é que o Abstract Factory delega a criação de novas instâncias a outro objeto via composição, enquanto o Factory Method utiliza Herança, e depende de uma subclasse para lidar com a instanciação do objeto.

#### Definição

```Python
class Course:
  def __init__(self, coursesFactory = None):
    self.course_factory = coursesFactory

  def showCourse(self)
    course = self.courseFactory()
    print(f'We have a course named {course}') 
    print(f'its price is {course.Fee()}')

class SoftwareDevelopment:
  def price(self):
    return 8000

class DataStructureAndAlgorithms:
  def price(self):
    return 11000

def randomCourse():
  return random.choice([SDE, STL, DSA])()

if __name__ = "__main__":
  course = Course(randomCourse)

  for i in range(5):
    course.showCourse()

```

#### Aplicação no WoCo

No contexto do WoCo, podemos usar o Abstract Factory para a criação dos nossos usuários, já que teremos dois tipos de usuários: Treinador e Atleta, que terão diferentes objetivos e responsabilidades dentro do nosso sistema.

### Singleton

O Singleton é um design pattern que permite garantir que uma classe vai gerar apenas uma única instância dela e prover um acesso global a aquela instância. É útil quando queremos ter o controle de acesso a recursos compartilhados, como a base de dados ou um arquivo.

#### Definição

```Python
# singleton.py
def Singleton:
  _instance = None

  def __new__(cls):
    if cls._instance is None:
      cls._instance = super().__new__(cls)
    return cls._instance

# main.py
from singleton import Singleton
foo = Singleton()
bar = Singleton()
foo is bar
>>> True
foo.some_attribute = 'some value'
bar.some_attribute
>>> 'some value'
```

#### Aplicação no WoCo

Podemos utilizar no back-end do WoCo para se conectar a base de dados, e garantir que sempre que em algum lugar da aplicação precisar de comunicar com a base de dados, terá um objeto compartilhado.

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|11/10/2020|   0.1    | Iniciando o documento       |  Eugênio Sales  |
|12/10/2020|   0.2    | Adicionado Abstract Factory e Singleton patterns  |  Ernando braga  |
