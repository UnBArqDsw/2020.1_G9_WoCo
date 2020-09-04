# Política de Branches

As <b>Branches</b> do projeto devem contemplar as seguintes especificações:

* Cada branch é voltada para a solução de uma única issue, após o fechamento da issue especificada a branch também deve ser deletada

* O início do nome da branch deve estar informando a que grupo pertence, sendo eles:

    * wip: (Work In Progress) para trabalhos que não possuem previsão de entrega;

    * feat: Para adição de funcionalidades ou mudanças em alguma já existente; 

    * bug: Para correção de bugs;

    * hotfix: Para correção de bugs em produção que não podem esperar a próxima release.

* O nome da branch deve estar relacionado com a issue, procurando ser o mais objetivo possível, separando o grupo por  barras('/')

````Git
Exemplo: feat/signin-user
````
## Fluxo das branches

O projeto irá adotar um fluxo git baseado no git-flow:

 * master: Código mais estável e atualizado do sistema para produção, aceita apenas adições via Pull Request;

 * develop: Branch para integração, código mais atualizado em desenvolvimento para ser integrado à master, após testado;

 * feature(feat): Branch para desenvolvimento de novas funcionalidades ou para expansão de funcionalidades já existentes;

 * Bugfix(bug): Branch para corrigir bugs em produção;

## Versionamento do documento
| Autor | Data | Versão | Modificação |
|---|---|---|---|
| Ernando Braga | 04/09 | 1.0 | Criação do documento |