# Padrões de projeto emergentes

## 1. Introdução

Além dos tradicionais GoFs e GRASPs utilizados no projeto, foram utilizados padrões de projeto emergentes que não foram abordados nos padrões citados. Tendo em isto em vista, esse documento tem por objetivo a exploração de algum desse padrões. Chamamos de padrões emergentes estes casos em que o comportamento de um determinado padrão está presente, mesmo que certa região do código apresente estruturação livre. Por exemplo, a essência do padrão de projeto Singleton está presente em uma classe qualquer quando esta possui apenas uma única instância durante as execuções de um programa, mesmo que não haja restrição sintática para que isso seja possível; ou seja, o padrão Singleton emerge deste comportamento de um determinado elemento do programa. Ao auxiliar o desenvolvedor na detecção de situações de projeto como esta, pode-se enriquecer o seu conhecimento sobre as consequências de suas decisões, além de propiciar a estruturação explícita do padrão como conhecida, facilitando assim a documentação e comunicação do projeto. Este trabalho explora o conceito de padrões emergentes através das seguintes contribuições: 
* uma revisão sistemática sobre abordagens automáticas de detecção de padrões de projeto, 
* conceitos de padrões emergentes para vários padrões de projeto bem conhecidos, 
* uma proposta de abordagem semi-automática de detecção de padrões emergentes, 
* sua utilização para uma análise de ferramentas de detecção existente acerca de sua capacidade de identificação de padrões emergentes em alguns projetos de código aberto

## Padrões emergentes

### BLoC

No padrão BLoC, podemos distinguir quatro camadas principais de aplicação:

* UI:  Contém todos os componentes do aplicativo que são visíveis para o usuário e com os quais podem interagir.

* Bloco: Essas são as classes que atuam como uma camada entre os dados e os componentes da IU. O BLoC escuta os eventos transmitidos por ele e, após receber uma resposta, emite um estado apropriado.

* Repositório. Ele é responsável por buscar informações de uma ou várias fontes de dados e processá-las para as classes de UI.

* Fontes de dados. São classes que fornecem os dados para a aplicação, de todas as fontes de dados, incluindo o banco de dados, rede, preferências compartilhadas, etc.

Dado como se divide são representadas as camadas, elas se comunicam entre si através:

* Eventos: Dão passados ​​da IU, que contém informações sobre uma ação específica que deve ser tratada pelo BLoC.

* Estados: Mostram como a IU deve reagir à mudança de dados. Cada BLoC tem seu estado inicial, que é definido na criação.

No aplicativo WoCo, na implementação da tela de login, precisaremos passar LoginEvent com detalhes de login quando o usuário clicar no botão apropriado. Depois de receber uma resposta, o BLoC deve mostrar o LogInLoaded quando o login foi concluído com sucesso, ou LogInError quando o usuário inseriu as credenciais erradas ou ocorreu um erro diferente.


* Authentication Bloc

```Dart
import 'dart:async';

import 'package:bloc/bloc.dart';
import 'package:WoCo/blocs/authenticationBloc/authentication_event.dart';
import 'package:WoCo/blocs/authenticationBloc/authentication_state.dart';
import 'package:WoCo/models/authentication.dart';
import 'package:WoCo/repository/authentication/authenticationRepository.dart';

class AuthenticationBloc
    extends Bloc<AuthenticationEvent, AuthenticationState> {
  AuthenticationBloc() : super(SignUpInitial());

  final AuthenticationRepository authenticationRepository =
      new AuthenticationRepository();

  @override
  Stream<AuthenticationState> mapEventToState(
    AuthenticationEvent event,
  ) async* {
    if (event is SignUp) {
      yield* mapSignUpToEvent(event);
    } else if (event is LogIn) {
      yield* mapLogInToEvent(event);
    }
  }

  Stream<AuthenticationState> mapSignUpToEvent(SignUp event) async* {
    try {
      yield SignUpLoading();

      Authentication auth =
          await authenticationRepository.signUp(event.email, event.password);

      yield SignUpLoaded(token: auth.token, id: auth.id);
    } catch (e) {
      yield SignUpFailure(message: e.toString());
    }
  }

  Stream<AuthenticationState> mapLogInToEvent(LogIn event) async* {
    try {
      yield LogInLoading();

      Authentication auth =
          await authenticationRepository.logIn(event.email, event.password);

      yield LogInLoaded(token: auth.token);
    } catch (e) {
      yield LogInFailure(message: e.toString());
    }
  }
}
```

* Widget (UI) responsável por alterar o estado de acordo os eventos emitidos pelo BloC e reagir de acordo com BlocConsumer

```Dart
@override
  Widget build(BuildContext context) {
    _height = MediaQuery.of(context).size.height;
    _width = MediaQuery.of(context).size.width;

    return Scaffold(
      appBar: AppBar(
        title: Text('Log In'),
        centerTitle: true,
      ),
      body: SafeArea(
        child: Container(
          height: _height,
          width: _width,
          child: BlocConsumer<AuthenticationBloc, AuthenticationState>(
            builder: (context, state) {
              if (state is LogInInitial) {
                return _logInForm();
              } else if (state is LogInLoading) {
                return Loading();
              } else if (state is LogInFailure) {
                return ErrorMessage(message: state.message);
              }
              return _logInForm();
            },
            listener: (context, state) {
              if (state is LogInLoaded) {
                Navigator.of(context)
                    .pushNamedAndRemoveUntil(AppRoutes.HOME, (route) => false);
              }
            },
          ),
        ),
      ),
    );
  }
```

### Injeção de dependências (Dependency injection)

Dependency Injection (DI) é um padrão de projeto usado para implementar IoC. Ele permite a criação de objetos dependentes fora de uma classe e fornece esses objetos a uma classe de diferentes maneiras. Usando DI, movemos a criação e vinculação dos objetos dependentes para fora da classe que depende deles. Também com o uso Injeção de dependências  conseguimos desacoplar o uso de um objeto de sua criação. Isso ajuda você a seguir os princípios de inversão de dependência e responsabilidade única do SOLID.

Você pode introduzir interfaces para quebrar as dependências entre classes de nível superior e inferior. Se você fizer isso, ambas as classes dependerão da interface e não mais uma da outra.Esse princípio melhora a capacidade de reutilização do seu código e limita o efeito cascata se você precisar alterar as classes de nível inferior. Mas mesmo que você o implemente perfeitamente, você ainda mantém uma dependência da classe de nível inferior. A interface apenas desacopla o uso da classe de nível inferior, mas não sua instanciação. Em algum lugar do seu código, você precisa instanciar a implementação da interface. Isso evita que você substitua a implementação da interface por uma diferente.

O objetivo da técnica de injeção de dependência é remover essa dependência, separando o uso da criação do objeto. Isso reduz a quantidade de código clichê necessário e melhora a flexibilidade. Além disso há 4 regras importantes que devem ser verificadas antes da aplicação da injeção de dependências:

1. O serviço que você deseja usar.
2. O cliente que usa o serviço.
3. Uma interface que é usada pelo cliente e implementada pelo serviço.
4. O injetor que cria uma instância de serviço e a injeta no cliente.

Python é uma linguagem interpretada com tipagem dinâmica. Há indícios de que a injeção de dependência não funciona para ele tão bem quanto para Java. Grande parte da flexibilidade já está incorporada. Alguns desenvolvedores Python dizem que a injeção de dependência pode ser implementada facilmente usando os fundamentos da linguagem.
Na aplicação Woco é possivel ultilizar a classe container e assim desacoplar a partir da dependency injector e deixar com alta coesão.

```python
from dependency_injector import containers, providers
from dependency_injector.wiring import Provide


class Container(containers.DeclarativeContainer):

    config = providers.Configuration()

    coach = providers.Singleton(
        Coach,
        api_key=config.api_key,
        timeout=config.timeout.as_int(),
    )

    exercise = providers.Factory(
        Exercise,
        coach=coach,
    )


def main(service: Service = Provide[Container.service]):
    ...


if __name__ == '__main__':
    container = Container()
    container.config.api_key.from_env('API_KEY')
    container.config.timeout.from_env('TIMEOUT')
    container.wire(modules=[sys.modules[__name__]])

    main()  # <-- dependency is injected automatically

    with container.coach.override(mock.Mock()):
        main()  # <-- overridden dependency is injected automatically
```

## Referências

[1] Flutter BLoC. Disponível em: https://pub.dev/packages/flutter_bloc

[2] JOB, Ricardo de Sousa. Uma abordagem para detecção de padrões emergentes. 2014. 92f. (Dissertação de Mestrado em Ciência da Computação) Programa de Pós-graduação em Ciência da Computação, Centro de Engenharia Elétrica e Informática, Universidade Federal de Campina Grande - Paraiba - Brasil, 2014.

[3] Tutorials Teacherh - ttps://www.tutorialsteacher.com/ioc/dependency-injection

#### Histórico de revisões
|   Data   |  Versão  |           Descrição          |       Autor(es)       |
|:--------:|:--------:|:----------------------------:|:---------------------:|
|25/10/2020|   0.1    | Iniciando o documento  |   Eugênio Sales   |
|25/10/2020|   0.2    | Adicionando o BloC  |   Eugênio Sales   |
|26/10/2020|   0.3    | Refatorado e Adição de Injeção de dependências  |   Davi Alves   |
