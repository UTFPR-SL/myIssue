# Arquitetura de Software: JabRef 2022

Neste documento é apresentada uma descrição arquitetônica do sistema de software de código aberto chamado JabRef. Este sistema é responsável por criar uma interface para organização de referências utilizadas no software LaTeX.

### Autores 

- Rafael Rampim Soratto;
- Victor Daniel Pires; 
- Emica.

### Resumo sobre o projeto

O [JabRef](https://github.com/JabRef/jabref) é um sistema multi-plataforma que oferece ferramentas para gerenciar refêrencias em divervos sistemas operacionais: Windows, MacOS e Linux. Esse sistema é implementado em Java e necessita do jdk 18 para ser executado localmente. Todo projeto é bem descrito no [README](https://github.com/JabRef/jabref/blob/main/README.md): as funcionalidades, a instalação, e guia para contribução. 

### Execução do projeto

- Garanta que a versão do jdk da sua máquina é igual ou superior a versão 18.
- Clone o projeto.
- Execute o comando `gradlew run`

### Como contribuir

Existe uma comunidade engajada de contribuidores neste projeto. As contribuições são bem-vindas em diversos níveis do projeto, são eles:

- Contribuições de tradução
- Implementação de funcionalidades e solução de problemas. Para facilitar este processo é fornecido um guia de arquitetura do sistema chamado de [High-level documentation](https://devdocs.jabref.org/getting-into-the-code/high-level-documentation). E também uma diretriz de contribuição.
- Tarefas para iniciantes. existe uma label chamda 'good first issue' para contribuidores recém chegados ao projeto.

Para efetuar uma contruição é utilizado o fluxo padrão em repositórios Github: realizar as alterações em uma ramificação atualizada com a ramificação principal e após finalizar a tarefa deve-se criar um pull request, onde o código inserido será revisado e testado.

## Arquitetura do sistema

### MVC

Na documentação fornecida para os desenvolvedores é dito que a arquitetura 'espaguete' vem sendo migrada com sucesso para a famosa arquitetura MVC (model, view, controller). Portanto, para implementar novas funcionalidades é necessário obedecer a nova arquitetura proposta. Isso é bom pois evidencia quais são as entidades presentes no projecto chamadas de modelos. Definindo as entidades nas quais serão implementadas as funcionalidades, toda a lógica é implementada em uma camada lógica, que é consumida pela interface para apresentar os dados.
A lógica é responsável por ler/escrever/importar/exportar e manipular o modelo, e geralmente é estruturada como uma API que o gui pode chamar e usar. Resumindo: o MVC separa as entidades da regra de negócio e da visualização dos dados. Isto é positivo para identificar em qual camada o dado está, e facilita a manutenção do projeto. São exemplos de modelos do projeto: BibDatases, BibEntries, Events, etc.

Apenas o a camada de interface conhece o usuário e suas preferências e pode interagir com ele para ajudá-lo a resolver tarefas. Para cada camada do MVC, forma-se pacotes de acordo com sua responsabilidade, ou seja, estruturação vertical. O modelo não deve ter dependências com outras classes do JabRef e a lógica deve depender apenas das classes do modelo. O pacote 'cli' agrupa classes que são responsáveis pela interface de linha de comando do JabRef. As preferências representam todas as informações personalizáveis por um usuário para suas necessidades pessoais.

### Dependências permitidas entre camadas

Note que todo o fluxo de depência começa com a interface. A interação com a interface começa todo o fluxo de dependências.

- GUI -> logic -> Model
- GUI -> Model;
- GUI -> Preferences;
- GUI -> CLI
- GUI -> Classes globais

Note também que o modelo é a base da arquitetura, e ela nunca dependerá de outra camada:

- Logic -> Model
- Classes globais -> todo projeto
- cli -> Todo projeto

### Classes importantes

Classes mais importantes e sua relação Ambos GUI e CLI são iniciados através do JabRefMain que por sua vez chamará JabRef que então decide se o GUI (JabRefFrame) ou o CLI (JabRefCLI e muito código no JabRef) serão iniciados. O JabRefFrame representa a janela que contém um SidePane à esquerda usado para os buscadores

### Howtos

TODO: Fazer um RESUMO sobre esta página  [HOWTO](https://devdocs.jabref.org/getting-into-the-code/code-howtos.html)