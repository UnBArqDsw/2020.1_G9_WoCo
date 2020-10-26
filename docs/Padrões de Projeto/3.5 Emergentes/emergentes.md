# Padrões de projeto emergentes

## 1. Introdução

Além dos tradicionais GoFs e GRASPs utilizados no projeto, foram utilizados padrões de projeto emergentes que não foram abordados nos padrões citados. Tendo em isto em vista, esse documento tem por objetivo a exploração de algum desse padrões.

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

## Referências

[1] Flutter BLoC. Disponível em: https://pub.dev/packages/flutter_bloc

#### Histórico de revisões
|   Data   |  Versão  |           Descrição          |       Autor(es)       |
|:--------:|:--------:|:----------------------------:|:---------------------:|
|25/10/2020|   0.1    | Iniciando o documento  |   Eugênio Sales   |
|25/10/2020|   0.2    | Adicionando o BloC  |   Eugênio Sales   |
