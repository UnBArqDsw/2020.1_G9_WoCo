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
O notifyListeners() é um método específico do ChangeNotifier e deve ser chamado sempre que houverem mudanças que possam alterar a IU do aplicativo. No WoCo, ele é chamado sempre quando um treino é adicionado, editado ou removido da lista de treinos do usuário, dessa forma a mudança é processada e exibida na tela.

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158
[2] Design Patterns - Observer Pattern. Disponível em: https://www.tutorialspoint.com/design_pattern/observer_pattern.htm
[3] Simple app state management - ChangeNotifier. Disponível em: https://flutter.dev/docs/development/data-and-backend/state-mgmt/simple

#### Histórico de revisões
|   Data   |  Versão  |           Descrição          |       Autor(es)       |
|:--------:|:--------:|:----------------------------:|:---------------------:|
|22/10/2020|   0.1    |    Iniciando o documento     |   Weiller Fernandes   |
|22/10/2020|   0.2    | Adicionando Padrão Observer  |   Weiller Fernandes   |
