# Estudo GoFs Comportamentais

## 1. Introdução

OS Padrões de projetos são princípios e soluções usados durante a criação de um software que resolvem um problema específico e são generalizados, codificado em um formato estruturado e bem estabelecido. Os padrões de design comportamental (GoFs) preocupam-se com algoritmos e a atribuição de responsabilidades entre objetos baseando-se no comportamento.

Esse documento busca formalizar o estudo inicial dos GoFs, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Padrões GoFs Comportamentais

### Template Method

Esse método tem por característica a definição do esqueleto de um algoritmo em uma operação, adiando algumas etapas para as subclasses. O método de modelo permite que as subclasses redefinam certas etapas de um algoritmo sem alterar a estrutura do algoritmo.

#### Definição

``` Dart
import 'package:http/http.dart' as http;


abstract class Request {
    String baseUrl = "https://jsonplaceholder.typicode.com";
    dynamic get();
}

class Todo extends Request {
    @override
    dynamic get() async {
      return await http.get('${BaseUrl}/todos/1');
    }
}

class Photo extends Request {
    @override
    dynamic get() async {
      return await http.get('${BaseUrl}/photos/1');
    }
}
```

#### Aplicação no Woco

Dentro do contexto de uso no WoCo, o Template method pode ser utilizado nos requests http, onde a classe abstrata referencia a url base da chamada e os verbos https necessários e cada endpoint implementa propriamente com suas especificações. E em outros casos, onde possa seja aplicável sobrescrita de métodos para classes com tal padrão.

### Observer
Defina uma dependência um-para-muitos entre os objetos para que, quando um objeto mudar de estado, todos os seus dependentes sejam notificados e atualizados automaticamente. Tendo em mente esta definição, uma aplicação plausível para a aplicação WoCo é na forma de resertar senha que ao solicitar ***alterar sua senha*** seu email de cadastrado é notificado e consequentemente seus dados de cadastro no banco são atualizados para então confirmarmos tal ação.


```Dart
@override
    Future<void> resetPassword(String email) async {
      await _firebaseAuth.sendPasswordResetEmail(email: email);
    }
```


## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|06/10/2020|   0.1    | Iniciando o documento       |  Eugênio Sales  |