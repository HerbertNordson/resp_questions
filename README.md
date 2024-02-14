## Teste para Desenvolvedor Flutter Pleno

### Parte 1: Conhecimentos Técnicos Básicos
**Instruções:** Responda às seguintes perguntas teóricas.

1. **Flutter e Dart:**
   - Explique a diferença entre `StatelessWidget` e `StatefulWidget`.
      
      Resposta: 
        Um StatelessWidget nada mais é do que um widget que não tem o seu estado alterado. Ex: Texto, imagem; Já um StateFulWidget é um widget que por sua vez permite alteração em seu estado.

        Para uma melhor compreensão podemos dizer que o StatelessWidget é um quadro onde o seu conteúdo se mantém o mesmo ao longo do tempo de forma estática, enquanto o StatefulWidget é uma TV, que a qualquer momento tem o seu conteúdo alterado, sua função alterada e um comportamento dinâmico.
   
   
   - Descreva o ciclo de vida de um `StatefulWidget`.
      
      Resposta: 
        *Create*: Cria o Widget; -> 
        *InitState*: É chamado uma vez, no momento da inserção do widget na árvore. Sendo sobrescrito com o @override de acordo com a necessidade de inicialização de propriedades, registrar stream, conectar com o banco ou uma API. ->
        *Build*: Responsável pela contrução/renderização da árvore de widgets e pelo contexto do widget, observando o estado da aplicação. Alguns eventos como setState, initState ou uma necessidade de uma nova renderização pode chamar o Build. ->
        *Dispose*: Quando um widget é removido permanentemente da árvore de widgets o dispose é disparado. O método é chamado automaticamente quando a página é removida da pilha de navegação, podendo ser sobrescrito para executar novas ações.

2. **Gerenciamento de Estado:**
   - Compare os métodos `setState`, `Provider`, e `BLoC` para gerenciamento de estado no Flutter. Quais são as vantagens e desvantagens de cada um?

      Resposta: 
        setState -> É um método fornecido pelo StateFullWidget, sendo responsável pela alteração de estado e comportamento do widget. 
          Vantagem: É um método nativo do StatefulWidget Flutter, o seu custo em desempenho é mínimo e permite tornar o nosso widget dinâmico e a sua aplicação é simples.
          Desvantagem: Ele é limitado a gerenciar o estado do widget em que ele está inserido. 

        Provider -> Ele permite compartilhar dados para toda a aplicação com injeção e resolução de depedências seguindo o padrão Singleton. Muito utilizado por desenvolvedores mobile flutter e em geral combinado ao BloC Pattern.
          Vantagem: Compartilhar informações por toda a aplicação com injeção de dependências e aplicação do padrão singleton.
          Desvantagem: Por ser um singleton, no geral, existe uma dificuldade em aplicar teste unitários quando mais de um singleton precisa conversar entre si. Dificuldade no controle de memória quando associado ao BloC que utiliza StreamController e necessita ser encerrado manualmente com o close().

        BloC -> Biblioteca flutter que aplica o gerenciamento de estado no padrão BloC (Business Logic Object Components). Separando a regra de negócios da interface. Este mesmo padrão pode facilemente ser aplicado sem a necessidade do usa da biblioteca.
          Vantagens: Separação de responsabilidades, facilidade em teste, fluxo de dados unidirecional e reatividade.
          Desvantagens: Por utilizar StreamController ele precisa ser encerrado manualmente com o close(), caso não seja utilizado devidamente gera um grande consumo e descontrole de memória.

### Parte 2: Programação e Resolução de Problemas
**Instruções:** Desenvolva um aplicativo Flutter com as seguintes especificações.

- **Aplicativo de Lista de Tarefas:**
  - Permitir adicionar, remover e marcar tarefas como concluídas.
  - Incluir uma tela para adicionar detalhes da tarefa.
  - Implementar persistência de dados (pode ser local).

      Resposta: https://github.com/HerbertNordson/init


### Parte 3: Conhecimento Avançado e Melhores Práticas
**Instruções:** Responda à seguinte pergunta teórica e desenvolva um exemplo prático.

1. **Pergunta Teórica:**
   - Como você implementaria o padrão BLoC para gerenciamento de estado em um aplicativo Flutter? Explique com exemplos.

      Resposta: Pensando em uma simples aplicação de count para dar mais contexto a explicação, começaria criando o nossa classe repository (count_repository). Nele eu criaria nossa regra de negócio recebendo uma variavel int count e possuindo os métodos de incremment e decremment.

      Após a criação do repositorio, iniciaria a construção do bloco de eventos, que teria uma classe abstrata CountEvent, uma classe GetCount, a classe incremmente e a decremment ambas recebendo o valor que será utilizando no cálculo, as três classes herdando a CountEvent.

      Seguindo para o nosso estado, mantendo a mesma ideia, uma classe abstrata que recebe o nome de CountState e mais três classes que levraram no seu nome o comportamento de execução atual, sendo eles: initial, loading e loaded; Herdando de CountState. Esses estados serviram para acompanhar o processo de envio e recebimento dos dados.

      Por fim, montar o arquivo BloC. Nele nós utilizaremos as regras de négocio para dar vida as nossas ações. Criamos os StreamController's  event e state (input e output); Criamos nosso Sink e Stream, input.sink e output.tream. Montamos o construtor CountBloc que utilizará o input.stream.listen para escutar as ações da função que será responsável por executar os métodos do nosso repository que será validado em cunjunto de if e else, que determinará a partir do evento escolhido. Retornando o dado atualizado no output.add, passando o estado final de loaded.

      Aplicando o Bloc em nossa view: 
        Inicio a instancia do CountBloc em nosso initState e inicio nosso envio de dados com o evento GetCount() no nosso input.add(GetCount());

        Feito isso, envolvo o widget pai da view em uma StreamBuilder passando o output no stream que ficará escutando os novos dados recebidos e no build monto o comportamento da minha tela de acordo com o evento e dados retornados no snapshot.


2. **Exemplo Prático:**
   - Implemente um exemplo simples usando BLoC para gerenciar o estado de um formulário de login.

      Resposta: https://github.com/HerbertNordson/login_pattern_bloc


### Parte 4: Testes e Debugging
**Instruções:** Escreva testes para o código fornecido.

- **Desafio de Teste:**
  - Escreva testes unitários para uma função de validação de email.
  - Escreva testes de widgets para um botão de login que se habilita apenas quando o formulário é válido.

      Resposta: 


### Parte 5: Avaliação de Código e Análise Crítica
**Instruções:** Revise o seguinte trecho de código Flutter.

- **Revisão de Código:**
  - Identifique e corrija erros no código fornecido.
  - Sugira melhorias para otimização de desempenho e legibilidade.

```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('Widget tapped');
      },
      child: Container(
        padding: EdgeInsets.all(10),
        decoration: BoxDecoration(
          color: Colors.blue,
          borderRadius: BorderRadius.circular(5),
        ),
        child: Text('Tap Me'),
      ),
    );
  }
}
```

    
Resposta: Não possui erro no código fornecido. O seu funcionamento está correto, a utilização do widget também. Como melhoria, envolveria o GestureDetector em um widget pai, por exemplo um Center, que determinaria um tamanho limite para ele, evitando que o mesmo ocupe toda a tela.


Este teste foi projetado para avaliar o conhecimento técnico, a habilidade prática, e a capacidade de resolução de problemas de um desenvolvedor Flutter Pleno.
