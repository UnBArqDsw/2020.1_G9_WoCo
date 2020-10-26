# Estudo GoFs Comportamentais

## 1. Introdução

Os Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

Esse documento busca formalizar o estudo inicial dos GoFs, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Facade
O Facade é um padrão de projeto estrutural que fornece uma interface simplificada para uma biblioteca, um framework, ou qualquer conjunto complexo de classes. Sua finalidade pode ser resumida na seguinte frase

> ***"Facade visa fornecer uma interface unificada para um conjunto de interfaces em um subsistema. Facade define uma interface de nível superior que torna o subsistema mais fácil de usar."***

No contexto da aplicação WoCo podemos exemplificar os subsistemas em classes a respeito dos treinos, modularizando assim uma tarefa em classes.


```Dart
class AdicionarTreino implements ListItem {
  final String name;
  final String description;

  MessageItem(this.sender, this.body);

  Widget buildTitle(BuildContext context) => Text(sender);

  Widget buildSubtitle(BuildContext context) => Text(body);
}
```

```Dart
    class EditarTreino(Context name) {
        final newValue = name;
        final updateIndex = 3; //exemplo de index treino
        
        setState(() {
            data[updateIndex] = newValue;
        });
}
```

```Dart
    class ExcluirTreino {
    
        final length = _trainings.length;
        for (int i = length - 1; i >= 0; i--) {
            String removedItem = _data.removeAt(i);
            AnimatedListRemovedItemBuilder builder = (context, animation) {
        
        return _buildItem(removedItem, animation);
  };
    _listKey.currentState.removeItem(i, builder);
}
}
```

## Composite
Composite é um padrão de design estrutural que permite compor objetos em estruturas de árvores e trabalhar com essas estruturas como se fossem objetos individuais. O padrão composite compõe objetos em termos de uma estrutura em árvore para representar partes e hierarquias inteiras.

No contexto do Woco podemos exemplificar com os seguintes trechos de códigos. Onde cada ação como verificar login, deletar usuário, e atualizar informações do usuário.<br>

> Treino e exercícios
```Dart
    class Exercise {
        String name;
        String description;
        void isDone(exercise);
    }

    class Workout implements Exercise {
        String name;
        Set<Exercicio> _exercises = Set();

        Treino(BuildContext name);

        void isDone(exercise) {
            print("exercise is done")
            const exerciseIndex = _exercises.indexOf(exercise)
            _exercises[exerciseIndex].isDone = true
        }
    }
```

## Adapter

Adapter é um design pattern que permite objectos com iterfaces imcompatíveis possam se comunicar e colaborar.

```python
class MotorCycle: 
  
    def __init__(self): 
        self.name = "MotorCycle"
  
    def TwoWheeler(self): 
        return "TwoWheeler"
  
  
class Truck: 
  
    def __init__(self): 
        self.name = "Truck"
  
    def EightWheeler(self): 
        return "EightWheeler"
  
  
class Car: 
  
    def __init__(self): 
        self.name = "Car"
  
    def FourWheeler(self): 
        return "FourWheeler"
  
class Adapter: 
  
    def __init__(self, obj, **adapted_methods): 
        self.obj = obj 
        self.__dict__.update(adapted_methods) 
  
    def __getattr__(self, attr): 
        return getattr(self.obj, attr) 
  
    def original_dict(self): 
        return self.obj.__dict__ 
  
  
""" main method """
if __name__ == "__main__": 
  
    objects = [] 
  
    motorCycle = MotorCycle() 
    objects.append(Adapter(motorCycle, wheels = motorCycle.TwoWheeler)) 
  
    truck = Truck() 
    objects.append(Adapter(truck, wheels = truck.EightWheeler)) 
  
    car = Car() 
    objects.append(Adapter(car, wheels = car.FourWheeler)) 
  
    for obj in objects: 
       print("A {0} is a {1} vehicle".format(obj.name, obj.wheels()))
```

## Decorator

Decorator é um design pattern que permite adicionar novos comportamentos de forma dinâmica.

>Decorator de login usando Flask
```python
from functools import wraps
from flask import g, request, redirect, url_for

def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if g.user is None:
            return redirect(url_for('login', next=request.url))
        return f(*args, **kwargs)
    return decorated_function
```

## Proxy

Proxy é um padrão de design estrutural que permite fornecer um substituto ou espaço reservado para outro objeto. Um proxy controla o acesso ao objeto original, permitindo que você execute algo antes ou depois que a solicitação chega ao objeto original.

```python
from flask import Flask
from requests import get

app = Flask('__main__')
SITE_NAME = 'https://Woco.com'

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def proxy(path):
  return get(f'{SITE_NAME}{path}').content

app.run(host='0.0.0.0', port=8080)

```

O padrão Proxy sugere que você crie uma nova classe de proxy com a mesma interface de um objeto de serviço original. Em seguida, você atualiza seu aplicativo para que ele passe o objeto proxy para todos os clientes do objeto original. Ao receber uma solicitação de um cliente, o proxy cria um objeto de serviço real e delega todo o trabalho para ele.

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

[2] Refactoring Guru. Disponível em: https://refactoring.guru/pt-br/design-patterns/behavioral-patterns

[3] Documentação do Flask sobre decorators. Disponível em: https://flask.palletsprojects.com/en/1.1.x/patterns/viewdecorators/

[4] Source Making - Design Patterns / Structural patterns: https://sourcemaking.com/design_patterns/proxy

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|10/10/2020|   0.1    | Iniciando o documento     | Bruno Duarte e Ernando Braga|
|10/10/2020|   0.2    | Adicionando Composite  | Bruno Duarte|
|12/10/2020|   0.3    | Refatorando o Composite, e adicionando Adapter e Decorator  | Bruno Duarte e Ernando Braga|
|26/10/2020|   0.4    | Adicionado Proxy  | Davi Alves |
