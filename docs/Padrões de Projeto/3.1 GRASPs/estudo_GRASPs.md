# Estudo GRASPs

#### Histórico de revisões
|   Data   |  Versão  |        Descrição       |          Autor(es)          |
|:--------:|:--------:|:----------------------:|:---------------------------:|
|06/10/2020| 0.1 | Iniciando o documento       |  Weiller Fernandes  |
|06/10/2020| 0.2 | Adicionando os GRASPs Criador e Especialista | Weiller Fernandes |
|13/10/2020| 0.3 | Adicionando os GRASPs Alta Coesão e Baixo Acoplamento | Weiller Fernandes|
|13/10/2020| 0.4 | Adicionando os GRASPs Controladora e Polimorfismo | Weiller Fernandes|
|13/10/2020| 0.5 | Adicionando os GRASPs Invenção Pura, Indireção e Variações Protegidas | Weiller Fernandes |
|15/10/2020| 0.6 | Adicionando exemplo de GRASPs Criador, Especialista, Polimorfismo em flask | Davi Alves |

## 1. Introdução

Padrões de projetos são princípios e soluções usados durante a criação de um software, codificado em um formato estruturado e bem estabelecido, descrevendo o problema no qual se deseja mitigar, bem como uma solução para esse problema. No caso do software, essa solução pode vir através de modelagem e código.
O Grasp é um padrão de projeto, onde se analisa o desenho do software para que seja possível identificar as entidades envolvidas, bem como suas responsabilidades e dessa forma, seja feita uma solução adequada para a resolução de um determinado problema.

Esse documento busca formalizar o estudo inicial dos GRASPs, definindo e correlacionando cada um deles com o projeto WoCo, através dos artefatos de modelagem criados no módulo anterior.

## Padrões GRASP

### Criador

Padrão de projeto que representa classes responsáveis por criar instâncias. No caso do aplicativo WoCo, uma classe que pode ser um creator é a classe de Usuario, que é responsável por criar o usuário, seja ele treinador ou atleta, dentro da plataforma. Outra opção de criador é a classe Treinador, pois essa classe é responsável por criar treinos e dietas que ficarão disponíveis para o atleta.

![CREATOR](../img/creator.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

Dentre os beneficios temos o baixo acoplamento que é suportado, o que implica menores dependências de manutenção e maiores oportunidades de reutilização. O acoplamento provavelmente não é aumentado porque a classe criada provavelmente já está visível para a classe criadora, devido às associações existentes que motivaram sua escolha como criador.
 
Exemplo inicial:

```
from app import db
from flask_login import UserMixin

class Role(db.Model):
    __tablename__ = 'roles'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    permissions = db.Column(db.Integer)
    users = db.relationship('User',
                            backref='role', lazy='dynamic')


class User(UserMixin, db.Model): 
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    type = db.Column(db.String(50))
    name = db.Column(db.String(64), index=True)
    email = db.Column(db.String(64), unique=True, index=True)

    password_hash = db.Column(db.String(128))
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

```

### Especialista

Padrão de projeto que se preocupa em atribuir responsabilidades para a entidade mais especialista em um dado aspecto. Um exemplo de especialista no WoCo é a Classe Dieta, que é especialista em calcular a quantidade total de calorias de uma dieta, por exemplo. Nesse caso a classe Alimentacao deve fornecer o valor das calorias de cada alimento para a classe Dieta, e essa calcula o valor total com base nos dados fornecidos pela classe Alimentacao.

![ESPECIALISTA](../img/especialista.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

Exemplo inicial:

```
class Diet(Form):
    def __init__(self):
        pass

    LengthVal = FloatField('Length:', validators=[validators.required()])
    WidthVal = FloatField('Width:', validators=[validators.required()])
    ThickVal = FloatField('Thickness:', validators=[validators.required()])

    @app.route('/', methods = ['POST', 'GET'])

    def errors(self):
        return "these are some errors"

    def CalculateKCal(self):
        form = ReusableForm(request.form)

        print (form.errors)
        if request.method == 'POST':
            KCalinit = request.form['KCalinit']
            KCalfood = request.form['KCalfood']
            print (KCalinit, " ", KCalfood, " ")

        if form.validate():
            FinalCalc = KCalinit - KCalfood
            FinalCalc = round(FinalCalc,2)
            flash('Total Board Feet: ' + FinalCalc)
     
        else:
            flash('Error: All form fields required')

        return render_template('boards.html')
```

### Alta Coesão

O conceito da coesão está intimamente ligado ao Princípio da Responsabilidade Única (SRP) do SOLID. O SRP aplica-se a qualquer artefato contido no modelo do software, como classes e objetos. Um componente com Alta Coesão é um componente que possui apenas uma única responsabilidade, que possui em seu conteúdo ou funções, apenas aquilo que realmente deve fazer. No WoCo, um bom exemplo de alta coesão é a classe Exercicio, que possui apenas um identificador e a instrução de como o atleta deve executar aquele exercício para que ela seja feito de forma correta, ou seja, essa classe apresenta apenas as informações necessárias à ela.

![ALTA COESÃO](../img/alta_coesao.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Baixo Acoplamento

O acoplamento pode ser entendido como a união ou ligação entre dois ou mais corpos, formando um único conjunto. Em um software, o acoplamento acontece no relacionamento entre classes, tabelas, domínios, etc. O ideal é que esse acoplamento seja baixo, para que sua manutenção possa ser realizada de forma mais simples, caso contrário, se uma classe, função ou método precisar ser modificado, inúmeros outros também podem precisar passar por alterações em sua estrutura. No aplicativo WoCo, podemos ver um exemplo de baixo acoplamento entre as classes Alimentacao e Dieta. A classe Alimentacao fornece os valores de calorias de cada alimento para a classe Dieta, e essa faz o cálculo do total de calorias de toda a Dieta. Porém, se a forma como o total de calorias é calculado, precisar ser modificado no software ao longo do tempo, a classe Alimentacao não será afetada, pois ela não lida com o cálculo em si, apenas com o valor de calorias de cada alimento, isso demonstra um baixo acoplamento entre essas classes.

![BAIXO ACOPLAMENTO](../img/especialista.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Controladora

O padrão controlador atribui a responsabilidade de manipular eventos do sistema para uma classe que não seja de interface do usuário (UI) que representa o cenário global ou cenário de caso de uso. Um objeto controlador é um objeto de interface não-usuário, responsável por receber ou manipular um evento do sistema. Dentro do WoCo, um exemplo de controladora pode ser dado pela classe Autenticacao, pois essa é responsável por controlar e tratar os eventos relacionados à autenticação do usuário, como o login, signUp e recuperação de senha.

![CONTROLADORA](../img/controladora.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Polimorfismo

Polimorfismo é um princípio a partir do qual as classes derivadas de uma única classe base são capazes de invocar os métodos que, embora apresentem a mesma assinatura, comportam-se de maneira diferente para cada uma das classes derivadas. Com o polimorfismo, os mesmos atributos e métodos podem ser utilizados em objetos distintos, porém, com implementações lógicas diferentes. No aplicativo WoCo, as classes Treinador e Atleta herdam da classe abstrata Usuario, as informações necessárias para cada instância de seus objetos. Sendo assim, essas classes exemplificam a aplicação do polimorfismo no app.

![POLIMORFISMO](../img/polimorfismo.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

```python
class Role(db.Model):
    __tablename__ = 'roles'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    permissions = db.Column(db.Integer)
    users = db.relationship('User',
                            backref='role', lazy='dynamic')


class User(UserMixin, db.Model): 
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    type = db.Column(db.String(50))
    name = db.Column(db.String(64), index=True)
    email = db.Column(db.String(64), unique=True, index=True)

    password_hash = db.Column(db.String(128))
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

    __mapper_args__ = {
        'polymorphic_identity': 'users',
        'with_polymorphic': '*',
        "polymorphic_on": type
    }

class Coach(User):
    __tablename__ = 'coach'
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    phone_number = db.Column(db.Integer)
    other_Coach_data = db.Column(db.String)

    __mapper_args__ = {
        'polymorphic_identity': 'coach',
        'with_polymorphic': '*'
    }


class Athlete(User):  
    __tablename__ = 'athlete'
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    other_athlete_data = db.Column(db.String)

    __mapper_args__ = {
        'polymorphic_identity': 'athlete',
        'with_polymorphic': '*'
    }
```

### Invenção Pura

Uma invenção pura é uma classe artificial que não representa um conceito no domínio do problema, especialmente feito para conseguir baixo acoplamento, alta coesão e o potencial de reutilização derivado. No WoCo, a classe Pesquisa pode ser considerado um exemplo de invenção pura, já que ela é responsável por realizar as pesquisas de treinos e dietas. Outra classe de invenção pura é a já citada classe de Autenticacao, que é responsável por autenticar o usuário dentro da plataforma.

![INVENÇÃO PURA](../img/invencao_pura.png)

![INVENÇÃO PURA 2](../img/controladora.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Indireção

O padrão indireção ajuda a manter o baixo acoplamento, através de delegação de responsabilidades através de uma classe mediadora, ou seja, o objetivo do padrão indireção é atribuir responsabilidade a um objeto intermediário para ser o mediador entre outros componentes ou serviços, para que eles não sejam diretamente acoplados. O intermediário cria uma indireção entre os outros componentes. No WoCo, a classe de Pesquisa também é um exemplo de indireção, pois ela funciona como um intermediário entre as classes Treino e Dieta e a camada de persistência.

![INDIREÇÃO](../img/invencao_pura.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

### Variações Protegidas

O padrão variações protegidas protege elementos das variações em outros elementos (objetos, sistemas, subsistemas) envolvendo o foco de instabilidade com uma interface e usando polimorfismo para criar várias implementações desta interface. No WoCo, temos o exemplo de variações protegidas nas classes Treinador e Atleta, pois ambas se relacionam com as classes Treino e Dieta, porém cada uma da sua maneira, seja criando, editando, deletando, iniciando e interrompendo esses treinos e dietas.

![VARIAÇÕES PROTEGIDAS](../img/polimorfismo.png)

[Diagrama de Classes Completo](../../Modelagem/2.1%20M%C3%B3dulo%20Projeto%20Orientado%20a%20Abordagens%20Tradicionais/Diagramas%20Est%C3%A1ticos/umlClasses.md#vers%C3%A3o-20-com-deped%C3%AAncia-e-associa%C3%A7%C3%A3o)

## Referências

[1] Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158
[2] Acoplamento e Coesão. Disponível em: https://www.ateomomento.com.br/acoplamento-e-coesao/
[3] GRASP (padrão orientado a objetos), Padrão Controladora. Disponível em: https://pt.wikipedia.org/wiki/GRASP_(padr%C3%A3o_orientado_a_objetos)#Controller_(controlador)
[4] Conceitos e Exemplos – Polimorfismo: Programação Orientada a Objetos. Disponível em: https://www.devmedia.com.br/conceitos-e-exemplos-polimorfismo-programacao-orientada-a-objetos/18701
[5] GRASP (padrão orientado a objetos), Padrão Invenção Pura. Disponível em: https://pt.wikipedia.org/wiki/GRASP_(padr%C3%A3o_orientado_a_objetos)#Pure_fabrication_(inven%C3%A7%C3%A3o_pura)
