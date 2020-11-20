## Histórico de Revisão

| Data | Versão | Descrição | Autor |
|------|--------|-----------|-------|
| 20/11/2020 | 1.0 | Criação do documento | Davi Alves |
| 20/11/2020 | 1.1 | Adição da reutilização do Docker | Davi Alves, Bruno, Ernando Braga|
| 20/11/2020 | 1.2 | Refatoração do documento | Weiller, Eugenio|

# Reutilização do Docker no Backend

 A ideia do reuso é evitar retrabalho no desenvolvimento de um novo projeto, sempre levando em consideração trabalhos anteriores, fazendo com que soluções previamente desenvolvidas sejam aproveitadas e implementadas em novos contextos.
 A reutilização de software se baseia no uso de conceitos, produtos ou soluções previamente elaboradas ou adquiridas para criação de um novo software, visando melhorar significativamente a qualidade e a produtividade. Reusar um produto significa poder reusar partes de um sistema desenvolvido anteriormente como: especificações, módulos de um projeto, arquitetura e código fonte. É a reaplicação de informações e artefatos de um sistema já definido, em outros sistemas semelhantes. O termo reuso pode ser considerado uma denominação genérica para uma série de técnicas utilizadas, que vão desde a etapa de modelagem de um projeto até a implementação.

## Docker
Docker é um conjunto de produtos de plataforma como serviço que usam virtualização no nível do sistema operacional para entregar software em pacotes chamados contêineres. Os contêineres são isolados uns dos outros e agrupam seus próprios softwares, bibliotecas e arquivos de configuração; eles podem se comunicar uns com os outros por meio de canais bem definidos.
O uso do Docker simplifica e acelera o fluxo de trabalho, dando aos desenvolvedores a liberdade de inovar com sua escolha de ferramentas, pilhas de aplicativos e ambientes de implantação para cada projeto. Ele possui muitos hot spots, e alguns frozen spots, ambos estão sendo utilizados em nossa aplicação. Utilizamos o Docker Compose para orquestrar os containers do Docker.

## Reuso Composicional
Reuso Composicional é a utilização de componentes existentes como blocos para a construção de um novo sistema. 
A característica principal é que um novo sistema é construído a partir da composição de componentes existentes.
Optamos por usar o reuso composicional pois tanto o docker file quanto o doker compose são modelos que podem ser usados para varios outros tipo de projetos ou até mesmo novos modulols dentro do nosso projeto.


* **Frozen Spots**:
    * Formatação do arquivo;
    * Nome dos arquivos.

* **Hot Spots**:
    * Imagem para construir o container;
    * Dependências;
    * Comandos que o container executa ao iniciar;
    * Network;
    * Variáveis de ambiente.


*docker-compose*

```docker
version: "3.7"
services:
  app:

    # Use latest Python Docker image
    image: "python:latest"

    # Set container name
    container_name: flask-woco

    # Set environment variables
    environment:
      - FLASK_ENV=development
      - FLASK_APP=/repo/app/__init__.py
      - DATABASE=sqlite:////repo/app/db/sqlite.db

    # Mount entire project into docker container under /repo
    volumes:
      - ./:/repo

    # Make all ports accessible on host
    network_mode: host

    # Install requirements and start flask app
    entrypoint: >
      bash -c "pip install -r /repo/requirements.txt
      && flask run"
```

### Docker Backend
O docker usado no backend monta imagem do flask e da base de dados com SQLite. Caso a equipe quisesse trocar a base de dados ou o flask para uma outra ferramenta de API seria facilmente feito apenas substituindo os que são atualmente usados.
O arquivo usado para instalação das dependencias no python, o "requirements.txt" também entra como um reuso. 

## Referências

* Videoaulas e materiais complementares presentes no moodle da disciplina Arquitetura e Desenho de Software. Disponível em: https://aprender3.unb.br/course/view.php?id=158
* Engenharia de Software, Ian Sommerville, 9 Edição, Pearson (pág. 296–300).
