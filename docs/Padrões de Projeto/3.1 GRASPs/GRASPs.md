# GRASPs

## 1. Introdução

O Grasp é um padrão de projeto, onde se analisa o desenho do software para que seja possível identificar as entidades envolvidas, bem como suas responsabilidades e dessa forma, seja feita uma solução adequada para a resolução de um determinado problema. Nesse documento, temos o objetivo de listar vários GRASPs identificados dentro da aplicação WoCo.

## GRASPs no Projeto WoCo

### Alta Coesão

O conceito da coesão está intimamente ligado ao Princípio da Responsabilidade Única (SRP) do SOLID. O SRP aplica-se a qualquer artefato contido no modelo do software, como classes e objetos. Um componente com Alta Coesão é um componente que possui apenas uma única responsabilidade, que possui em seu conteúdo ou funções, apenas aquilo que realmente deve fazer.
No aplicativo WoCo, a classe WorkoutList é um exemplo da aplicação do padrão de Alta Coesão, afinal essa classe é responsável unicamente pela tela de listagem de treinos para o usuário, deixando outras funcionalidades como editar, adicionar e excluir treinos sendo implementadas por outras classes.

```Dart
class WorkoutList extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final WorkoutProvider workouts = Provider.of(context);

    return Scaffold(
      appBar: AppBar(title: Text('Lista de Treinos')),
      body: ListView.builder(
        itemCount: workouts.count,
        itemBuilder: (ctx, i) => WorkoutTile(workouts.byIndex(i)),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          Navigator.of(context).pushNamed(AppRoutes.WORKOUT_FORM);
        },
        child: Icon(Icons.add),
        backgroundColor: Color(0XFF1D3075),
      ),
    );
  }
}
```

[Link para o código](https://github.com/UnBArqDsw/2020.1_G9_WoCo_Frontend/blob/main/lib/screens/workout_list.dart)

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158

#### Histórico de revisões
|   Data   |  Versão  |           Descrição          |       Autor(es)       |
|:--------:|:--------:|:----------------------------:|:---------------------:|
|24/10/2020|   0.1    |    Iniciando o documento     |   Weiller Fernandes   |
|24/10/2020|   0.2    |Adicionando Padrão Alta Coesão|   Weiller Fernandes   |
|20/11/2020| 0.3 | Adicionando link para o código | Weiller Fernandes|
