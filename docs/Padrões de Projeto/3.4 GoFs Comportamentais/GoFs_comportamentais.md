# GoFs Comportamentais

## 1. Introdução

Os padrões GoFs podem ser descritos como soluções consolidadas para um problema recorrente no desenvolvimento e manutenção de software orientado a objetos.
No caso dos GoFs comportamentais, pode-se dizer que eles são padrões voltados para alterações no nível do comportamento dos objetos. Eles são fundamentais, por exemplo, quando se precisa usar vários algoritmos diferentes, onde cada um se encaixa melhor em um determinado contexto diferente. Dessa forma, a decisão de qual algoritmo usar, de acordo com um contexto específico, é feita em tempo de execução, o que o torna em algo bastante dinâmico.

## GoFs Comportamentais no Projeto WoCo

### Observer

O Observer é responsável por definir um mecanismo eficiente para reagir às alterações realizadas em determinados objetos. Assim, um objeto é observado pelo observer, que faz constantes atualizações dependendo das mudanças que ocorrem com o objeto, como uma mudança de estados, por exemplo. Assim, os objetos dependentes do objeto observado, são notificados automaticamente a cada mudança realizada.

No aplicativo WoCo, o responsável por implementar o Observer é o gerenciador de estados ChangeNotifier, que é uma classe simples incluída no Flutter SDK que fornece notificação de alteração para seus ouvintes. Um exemplo dessa implementação do Observer pode ser visto no trecho de código a seguir:


```Dart
class WorkoutProvider with ChangeNotifier {

  final Map<String, Workout> _items = {...DUMMY_WORKOUTS};

  void put(Workout workout) {
    if (workout == null) {
      return;
    }

    if (workout.id != null &&
        workout.id.trim().isNotEmpty &&
        _items.containsKey(workout.id)) {
      _items.update(workout.id, (_) => workout);
    } else {
      final id = Random().nextDouble().toString();
      _items.putIfAbsent(
        id,
        () => Workout(
          id: id,
          title: workout.title,
          description: workout.description,
        ),
      );
    }
    notifyListeners();
  }

  void remove(Workout workout) {
    if (workout != null && workout.id != null) {
      _items.remove(workout.id);
      notifyListeners();
    }
  }
}
```

[Link para o código](https://github.com/UnBArqDsw/2020.1_G9_WoCo_Frontend/blob/main/lib/provider/workout_provider.dart)

O notifyListeners() é um método específico do ChangeNotifier e deve ser chamado sempre que houverem mudanças que possam alterar a IU do aplicativo. No WoCo, ele é chamado sempre quando um treino é adicionado, editado ou removido da lista de treinos do usuário, dessa forma a mudança é processada e exibida na tela.

### Template Method

O pattern template method consiste em montar o esqueleto de um algoritmo de uma forma abstrata para que as classes concretas realizem as devidas implementações. O Template Method utiliza uma classe abstrata base, que vai encapsular o template do algoritmo em um método, para que as classes concretas possam herdar desta classe e realizar a implementação de determinados passos deste algoritmo.

No aplicativo WoCo, o o Template Method é utilizado junto a outro design pattern denominado Repository, cujo objetivo é prover um ponto de acesso único a camada de dados. Dessa forma, é implementado uma classe base abstrata com os métodos e o repository se encarrega de fazer a implementação concreta dos métodos. No exemplo abaixo referente ao módulo de autenticação, é possível verificar a utilização:

* Classe Abstrata Base
```Dart
import 'package:WoCo/models/authentication.dart';

abstract class Base {
  Future<Authentication> signUp(String email, String password);
  Future<Authentication> logIn(String email, String password);
}
```

* Implementação concreta (Repository)
```Dart

import 'package:WoCo/repository/authentication/base.dart';
import 'package:WoCo/models/authentication.dart';
import 'package:WoCo/services/authenticationApi.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class AuthenticationRepository implements Base {
  AuthenticationApi api = AuthenticationApi();

  Future<Authentication> signUp(String email, String password) async {
    var response = await http.post(
      '${api.baseUrl}/api/register',
      headers: {
        'Content-Type': 'application/json',
      },
      body: jsonEncode({'email': email, 'password': password}),
    );

    if (response.statusCode == 200 || response.statusCode == 201) {
      var data = json.decode(response.body);

      return Authentication.fromJson(data);
    } else {
      throw Exception(
          {'status': response.statusCode, 'message': 'Registro inválido'});
    }
  }

  Future<Authentication> logIn(String email, String password) async {
    var response = await http.post(
      '${api.baseUrl}/api/login',
      headers: {
        'Content-Type': 'application/json',
      },
      body: jsonEncode({'email': email, 'password': password}),
    );

    if (response.statusCode == 200 || response.statusCode == 201) {
      var data = json.decode(response.body);

      return Authentication.fromJson(data);
    } else {
      throw Exception(
          {'status': response.statusCode, 'message': 'Login inválido'});
    }
  }
}
```

[Link para o código](https://github.com/UnBArqDsw/2020.1_G9_WoCo_Frontend/blob/main/lib/repository/authentication/authenticationRepository.dart)

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158
[2] Patterns: Template Method. Disponível em: https://www.devmedia.com.br/patterns-template-method/18953
[3] Repositoy Pattern - Projetar a camada de persistência da infraestrutura. Disponível em: https://docs.microsoft.com/pt-br/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design

#### Histórico de revisões
|   Data   |  Versão  |           Descrição          |       Autor(es)       |
|:--------:|:--------:|:----------------------------:|:---------------------:|
|22/10/2020|   0.1    |    Iniciando o documento     |   Weiller Fernandes   |
|22/10/2020|   0.2    | Adicionando Padrão Observer  |   Weiller Fernandes   |
|25/10/2020|   0.3    | Adicionando Template Method  |   Eugênio Sales   |
|20/11/2020| 0.4 | Adicionando links para o código | Weiller Fernandes |
