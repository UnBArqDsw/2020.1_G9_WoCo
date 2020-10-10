# Estudo GoFs Comportamentais

## 1. Introdução

OS Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

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

> Verificar Login
```Dart

    Future<bool> isLogged(){
        var user = firebase.auth().currentUser;

        if (user) 
            return true;
        else 
            return false; 
    }

```


> Deletar Usuário
```Dart

    Future<bool> deleteUser(){
        var user = firebase.auth().currentUser;
        user.delete().then(function() {
        // User deleted.
        }).catch(function(error) {
        // An error happened.
        });
        }

```

> Setar/Cadastrar Usuário
```Dart

Future<bool> setUser(){
    var user = firebase.auth().currentUser;
    var name, email, photoUrl, uid, emailVerified;

    if (user != null) {
    name = user.displayName;
    email = user.email;
    photoUrl = user.photoURL;
    emailVerified = user.emailVerified;
    uid = user.uid;  }
        }

```
> Atualizar Usuário
```Dart
Future<void> updateProfile{

    var user = firebase.auth().currentUser;
    user.updateProfile({
    displayName: "Jane Q. User",
    photoURL: "https://example.com/jane-q-user/profile.jpg"
    }).then(function() {
    // Update successful.
    }).catch(function(error) {
    // An error happened.
    });
    }
```

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|10/10/2020|   0.1    | Iniciando o documento     | Bruno Duarte e Ernando Braga|
|10/10/2020|   0.1    | Adicionando Composite  | Bruno Duarte|
