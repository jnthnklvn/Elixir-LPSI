# Relatório Elixir
## Ferramentas de desenvolvimento
  * Elixir é uma linguagem dinâmica e funcional projetada para construir aplicações escaláveis e de fácil manutenção, baseada em Erlang, roda na Virtual Machine (VM) da mesma (BEAM).

## Paradigmas
  * Suporta multiparadigma, com conceitos de Orientação a Objetos (OO), Funcional e Concorrente.

## Simplicidade geral
  * Possui uma multiplicidade de recursos que possibilita haver mais de uma maneira de realizar uma operação e certa liberdade com relação a indentação. Contudo, os desenvolvedores da linguagem promovem bastante o chamado Estilo Elixir, estilo esse que segue padrões que visam melhorar a legibilidade. Além disso, o compilador faz alguns alertas quanto a ambiguidades que prejudicam a legibilidade, exemplo trecho de código abaixo:
```
  # verifica se a string termina com o arg
  "elixir" |> String.ends_with? "ixir" # compiler ambiguidade
  "elixir" |> String.ends_with?("ixir")
  String.ends_with? "elixir", "ixir"
  String.ends_with?("elixir", "ixir")
```
  * No fim, a responsabilidade de manter a legibilidade fica toda a cargo do programador, diferente de linguagens como Python por exemplo, em Elixir é possível escrever todo o código sem recuos, mesmo em módulos e funções.

## Tipos de dados Primitivos

  * Integer: números inteiros (suporte para números binários, octais e hexadecimais);
  * Float: valores de ponto flutuante (64 bits e suporta “e” para números exponenciais);
  * String: uma sequência de bytes (codificadas em UTF-8);
  * Char: caractere representado pelo Unicode;
  * Outros tipos de dados:
  * Boolean: booleanos (true or false);
  * Atom: constante cujo nome é seu valor (symbol em Ruby).

## Tipos de dados Compostos

  * List: lista de valores que podem incluir múltiplos tipos (são encadeadas);
  * Tuple: como o List, mas é armazenado de maneira contígua (rápido acesso aos elementos e imutabilidade);
  * Charlist: lista de caracteres representados pelo Unicode;
  * Keyword List: lista de tuplas de dois valores onde o primeiro elemento - o chave - é um Atom (ordenadas pela chave);
  * Map: lista de pares chave-valor.

## Declaração de tipos

  * Elixir possui uma tipagem forte e dinâmica, e não aceita declaração de tipos, como é o caso de Python.
```
  x = "String"
  IO.puts x # “String”
  x = 1
  IO.puts x # 1
  x = true
  IO.puts x # true
  x^ = "String"
  IO.puts x # true
```
  * No exemplo acima não só é  possível ver que uma variável pode ter sua referência mudada, como também pode ser para outro tipo. No último caso o “^” depois da variável, diz que se já houver uma referência naquela variável, ela permanece com  a mesma referência.

## Efeitos colaterais e mutabilidade

  * Elixir possui uma sintaxe rígida e não permite duplicação de significados em seus operadores ou átomos. Além disso, conta com a imutabilidade das variáveis, o código abaixo mostra um exemplo:
```
  muda = 1

  defmodule Mutacao do
    def mutar(valor) do
      valor = 0
      IO.puts valor # Será impresso 0 ou 1?
      valor
    end
  end

  Mutacao.mutar muda
  IO.puts muda # E aqui? 0 ou 1?
  muda = Mutacao.mutar muda
  IO.puts muda # E agora, 0 ou 1?
```
  * Os valores que são imprimidos na tela, no caso acima, são 0, 1, 0 e 0. Isso se dá porque em Elixir uma variável não pode ter sua referência trocada fora do escopo no qual foi declarada, e somente na última, quando recebe o retorno da função, a referência muda, pois as variáveis podem ter suas referências mudadas dentro de seu contexto, podendo inclusive receber referência a um outro tipo diferente do atual.

## Portabilidade

  * Elixir compila para bytecode, aproveita a BEAM, conhecida por executar sistemas de baixa latência - basicamente, sistemas com um menor tempo de respota a requisições - assim, o código roda em qualquer máquina com a BEAM e Elixir instaladas. Além disso, por ser uma linguagem recente (2012), ela foi influenciada por várias outras linguagens além da própria Erlang, assim a sintaxe e estrutura dos códigos soa como uma mistura entre Ruby, Java, Python e outras, o que faz com que seja mais familiar trabalhar com ela e/ou fazer portabilidade.
  
## Sintaxe, Semântica e Léxico

  * Elixir suporta os operadores básicos +, -, * e /. É importante ressaltar que / sempre retornará um número ponto flutuante, veja:
```
  x = 1 + 3 # x = 4 (int)
  x = x * 2 # x = 8 (int)
  x = x / 2 # x = 4.0 (float)
  x = x - 4 # x = 0.0 (float)
```
  * Caso seja preciso realizar divisões e manter partes inteiras, existem duas funcionalidades para isso div e rem, a parte inteira da divisão e o resto, respectivamente.
```
x = div(8, 2) # 4
x = rem(x, 3) # 1
```
  * Os operadores booleanos também  são os comuns ||, &&, e !. Mas, temos 3 funcionalidades a mais and, or e not, os quais é necessário um booleano como argumento da esquerda.
```
  true and 42 # true
  42 && true # true
  42 and true # false
```
  * Elixir vem com todos os operadores de comparação que estamos acostumados a usar: ==, !=, ===, !==, <=, >=, < e >. Contudo traz duas a mais, veja a diferença no trecho abaixo:
```
  2 == 2.0 # true
  2 !== 2.0 # true
  2 != 2.0 # false
  2 === 2.0 # false
```
  * A inclusão de mais um “=” é para comparações de tipo e valor.

  * Elixir permite que criemos alguns códigos com a mesma semântica e diferente sintaxe. Entretanto, oficialmente as diferentes sintaxe são  recomendadas a serem usadas em casos distintos para melhorar legibilidade, assim como a indentação. Abaixo temos um exemplo:
```
  x = true
  if (x==true), do: IO.puts "Verdadeiro"

  if (x==true) do
    IO.puts "Verdadeiro"
  end
```
  * No trecho acima, as duas condições fazem a mesma coisa, porém o Estilo Elixir define que o primeiro deve ser usado apenas quando houver 1 instrução, caso haja mais que uma a segunda forma deve ser adotada. Nesse caso o segundo caso não seria recomendável.
```
  teste(teste1(teste2(teste3(teste4()))))
  teste4() |> teste3() |> teste2() |> teste1() |> teste()
  teste4 |> teste3 |> teste2 |> teste1 |> teste
```
  * Nesse outro caso a semântica também é a mesma, porém o uso da primeira sintaxe é desaconselhável, o da terceira até o compilador te “repreende” - o compilador tem alertas para alguns desvios comuns que causam dano à legibilidade - e o segundo é o recomendável. Tanto o segundo quanto o terceiro utilizam o operador pipe, ele consiste no encadeamento de funções (é como o “.” em Haskell), onde o resultado da chamada da esquerda é passado como parâmetro para a função a direita.

  * A build tool de Elixir (Mix), na hora da compilação, faz primeiro a análise léxica verificando as variáveis e átomos e o encoding do código, se houver algum caractere não pertencente ao Unicode ou algum átomo ou variável fora de contexto, ele mostra um erro e o caractere corresponde.

## Tratamento de exceções

  * Elixir suporta o tratamento de exceções, convencionalmente se cria uma função que retorna uma 2-tupla com o status e o resultado e outra com o erro e a razão, e outra função que retorna a trilha de erros, segue o  exemplo abaixo:

```
  try do
    raise "Oh naoh!"
  rescue
    e in RuntimeError -> IO.puts("An error occurred: " <> e.message)
  end
```

  * A função raise cria um erro com a mensagem passada como parâmetro, daí o try-rescue imprime a concatenação das mensagens de erro, criando assim a trilha.

## Modificabilidade

  * Não dá pra declarar variáveis como constantes, por isso alguns programadores utilizam alguns métodos alternativos como o de criar uma função que retorna um valor. Um exemplo de uma dessas funções pode ser visto abaixo:
```
  defmodule Exemplo do
    def minha_const, do: 10
  end
```

## Semântica Estática e Dinâmica Operacional (SDO)

  * Em tempo de compilação o Mix faz a verificação das regras semânticas - parâmetros utilizados na chamada de uma função têm o tipo correto, limite de loops, etc - e segundo PROUNTZOS, Dimitrios (2012), em tempo de execução faz a verificação SDO.

## Checagem e equivalência de tipos

   * Como a linguagem  é fortemente tipada, a Análise Estática (AE) do programa em tempo de compilação determina se há violações na estrutura de tipos.         Se uma função é definida (implícita ou explicitamente) com um parâmetro do tipo T e houver chamadas com parâmetros diferentes de T no programa, um erro de compilação será apresentado.
```
  defmodule Teste2 do
    def teste(lista), do: for x <- lista, do: x*x
  end

  IO.puts(Teste2.teste([])) # imprimi uma lista vazia
  IO.puts(Teste2.teste(1)) # Enumerable not implemented for 1
```
  * Contudo, é possível fazer operações entre alguns tipos diferentes, não só operações como também procedures como o de conversão de alguns tipos, vide exemplo:
```
  s = "5" # String
  x = String.to_integer(s) # int
```
## Contextos, Vínculos, Escopo e Declarações

  * Como já mencionados, brevemente, antes Elixir possui escopos estáticos e não dá pra alterar ou acessar uma variável fora de contexto, o que se pode fazer é receber uma cópia da variável passando ela como parâmetro de uma função.

 * As declarações são feitas a partir do operador de Pattern Matching (=).
 
  * As definições são feitas em funções, structs e módulos a partir dos Atomos defmodule, defstruct e def.
  
  |**Identificador/Symbol**|        **Entidade**       	|**Tempo de vínculo**|
  |:---------------------:	|:-------------------------:	|:-----------------:	|
  |           *           	| Operação de Multiplicação 	|      Projeto      	|
  |        Variável       	|            Tipo           	|      Execução     	|
  |         Função        	|      Código da função     	|      Ligação      	|
  |    Variável Global    	|          Memória          	| Carga do Programa 	|
  |     Variável Local    	|          Memória          	|   Execução Local  	|

## Abstração de Funções e Ordem das Funções

  * Como Elixir é uma linguagem funcional e promove o desacoplamento de códigos, abstrações são implementadas com módulos ao invés de classes. E diferentemente das linguagens OO são chamadas as funções de módulos e não de objetos. Exemplo:
```
  String.upcase("uma string")
```
  * E como foi mostrado anteriormente os dados são imutáveis em Elixir, sendo assim temos que atribuir a referência retornada da função à uma variável.

  * Elixir suporta funções de alta ordem, em que uma função pode ser passada como argumento de outra. Também suporta compreensão de listas.

## Estratégias de Avaliação Preguiçosa
  * Assim como Haskell, também temos a avaliação preguiçosa em Elixir, o exemplo abaixo utiliza os módulos Enum e Stream
  ```
  x = [1..5] # x = [1..5]
  Enum.filter(1..1000000000, &(rem(&1, 3) == 0 || rem(&1, 5) == 0)) |> Enum.take 5
  Stream.filter(1..1000000000, &(rem(&1, 3) == 0 || rem(&1, 5) == 0)) |> Enum.take 5
  ```

## Persistência de Dados

  * O módulo Record.defrecord permite criar um conjunto de macros para criar e acessar uma gravação. Para tanto, define-se a forma da estrutura, a qual será preenchida ao chamar a função. Ex:

    * defrecord Planemo, nome: :nil,         gravidade: 0, diametro: 0, distancia_do_sol: 0

    * Também é possível utilizar o Erlang Term Storage (ETS) que persiste dados através de tabelas.

## Correspondência e Passagem de Parâmetros
  * Em Elixir a passagem os parâmetros de uma função são lidos na ordem em que estão dispostos, se a ordem estiver diferente da esperada o compilador apresenta um erro. 

## Polimorfismo (Coerção, Sobrecarga, Paramétrico)

  * Não existe coerção ou sobrecarga em Elixir, os parâmetros das funções só aceitam o tipo definido e a criação de novas funções com o mesmo nome não caracteriza sobrecarga pois são funções diferentes (que serão acessadas na ordem independente do tipo do parâmetro), embora tenham o mesmo nome. E justamente por conta da tipagem Polimorfismo Paramétrico também não é aceitavél.
  ```
  defmodule Teste do
    def teste(1), do: 2
    def teste(x), do: x*2 + teste(x-1)
    def teste([head|tail]), do: tail ++ head
  end
  ```
  * No caso acima, diferente de Java, se uma lista é passada como parâmetro da função teste, ela da erro, pois entra na primeira possibilidade e da erro de compatibilidade.
  
## Orientação a objetos (OO) - Herança e polimorfismo
  
  * Em Elixir, não há o polimorfismo ou a herança que vemos nas linguagens OO tradicionais. O meio que a linguagem encontrou para alcançar o polimorfismo foi o dos protocolos, ela já vem com um número de protocolos incorporados, por exemplo String.Chars que contém a função to_string. Além disso, protocolos adicionais podem ser implementados ou baixados, assim como as bibliotecas.
  
  * Já a herança, o que chega mais perto disso é o macro *use*. Ele é parecido com o macro import, porém ele dá a opção de editar o que está sendo importado, na pratica ele cria uma cópia do módulo importado e faz as "modificações".
  
  ```
  defmodule Hi do
    defmacro __using__(opts) do
      greeting = Keyword.get(opts, :greeting, "Hi")

      quote do
        def hello(name), do: unquote(greeting) <> ", " <> name
      end
    end
  end
  ```
  * No módulo acima é definido um macro com __using__(opts) que permite a "modificação" a seguir.
  ```
    defmodule Example do
      use Hi, greeting: "Olah"
    end
  
    IO.puts(Example.hello("Kelvin")) # "Olah, Kelvin"
  ```
  
## Concorrência

  * Implementação de algoritmo de concorrência Produtor-ProdutorConsumidor-Consumidor.
  ```
  defmodule Producer do
    use GenStage

    def start_link(inicio) do
      GenStage.start_link(Producer, inicio, name: Producer)
    end

    def init(cont), do: {:producer, cont}

    def handle_demand(demanda, estado) do
      eventos = Enum.to_list(estado..(estado + demanda - 1))
      {:noreply, eventos, estado + demanda}
    end
  end
  ```
  * O Produtor produz uma lista de inteiros de acordo com a demanda dos consumidores e a envia para o ProdutorConsumidor.
  ```
  defmodule ProducerConsumer do
    use GenStage

    require Integer

    def start_link, do: GenStage.start_link(__MODULE__, :ok, name: __MODULE__)

    def init(estado), do: {:producer_consumer, estado, subscribe_to: [Producer]}

    def handle_events(eventos, _from, estado) do
      num =
        eventos
        |> Enum.filter(&Integer.is_even/1)

      {:noreply, num, estado}
    end
  end
  ```
  * O ProdutorConsumidor filtra a lista de inteiros recebida, mantendo apenas os pares e a envia para os Consumidores.
  ```
  defmodule Consumer do
    use GenStage

    def start_link, do: GenStage.start_link(Consumer, :consumer)

    def init(estado), do: {:consumer, estado, subscribe_to: [ProducerConsumer]}

    def handle_events(events, _from, estado) do
      for event <- events, do: IO.inspect({self(), event, estado})

      {:noreply, [], estado}
    end
  end
  ```
  * O Consumidor imprimi sua identificação, os elementos da lista e seu *atom* até esvaziar a lista e poder demandar novamente.
  ```
  defmodule GenstageExample.Application do
    use Application

    def start(_type, _args) do
      import Supervisor.Spec, warn: false

      chineseChildren = [
        worker(Producer, [0]),
        worker(ProducerConsumer, []),
        worker(Consumer, [], id: 1),
        worker(Consumer, [], id: 2),
        worker(Consumer, [], id: 3)
      ]

      opts = [strategy: :one_for_one, name: GenstageExample.Supervisor]
      Supervisor.start_link(chineseChildren, opts)
    end
  end
  ```
  * Aplicação que cria os Produtores e Consumidores, define o tipo de alternancia e inicia o programa.
   
## Referências
  * PROUNTZOS, Dimitrios; MANEVICH, Roman; PINGALI, Keshav. Elixir: A system for synthesizing concurrent graph programs. ACM SIGPLAN Notices, v. 47, n. 10, p. 375-394, 2012.
  * https://elixirschool.com/pt/
  * https://hexdocs.pm/elixir/
