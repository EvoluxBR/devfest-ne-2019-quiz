# Quiz DevFest Nordeste 2019

Respostas comentadas do Quiz Evolux aplicado no DevFest Nordeste 2019.

## JavaScript

### Qual a saída desse código? (Dica: seja `people` uma LIFO, como uma "queue")

```js
const people = ['Marcell', 'David', 'Neto'];
people.unshift('Marcell');
console.log(people);
```

#### Alternativas
- (4)  ["Marcell", "David", "Neto", "Marcell"]
- (2) ["David", "Neto"]
- TypeError: Assignment to constant variable.
- (4) ✔️ ["Marcell", "Marcell", "David", "Neto"]

#### Resposta
Partindo do princípio de que você não sabe de cabeça o que `unshift` faz, basta usar a dica: `people` é uma LIFO (_last in, first out_) e funciona como uma _queue_ (fila). Assim como uma fila indiana no banco ou mercado deveria funcionar, as novas pessoas apenas chegam pelo fim e as pessoas que estão na frente é que vão saindo. Sem furar!

Dito isso, analisaremos cada alternativa:
1. Um novo nome "Marcell" entrou na frente da fila, isso seria furar!
2. O nome "Marcell" no início fugiu e, apesar disso ser aceitável na metáfora da fila de banco, não é um comportamento possível em uma LIFO.
3. Apesar dos elementos da lista estarem variando, a referência para a fila em si é a mesma, então `people` continua constante do ponto de vista mais interno da linguagem.
4. Um novo "Marcell" chegou pelo fim da fila, comportamento completamente esperado de uma LIFO e, portanto, a resposta correta.

### Qual a saída desse código? (Dica: seja `people` uma FIFO, como uma "stack")

```js
const people = ['Adalberto', 'Gustavo', 'Marcos Leo'];  
people.pop();  
console.log(people);
```

#### Alternativas
- (2) ✔️ ["Adalberto", "Gustavo"]
- TypeError: Assignment to constant variable.
- (4) ["Adalberto", "Gustavo", "Marcos Leo", undefined]
- (4) [undefined, "Adalberto", "Gustavo", "Marcos Leo"]

#### Resposta
Os comentários são quase os mesmos da questão anterior. A diferença é que aqui temos uma FIFO (_first in, first out_) como uma _stack_ (pilha). A metáfora agora é uma pilha de pratos em que você coloca um prato novo em cima ("na frente" da lista) e só retira pratos de cima também, porque é potencialmente desastroso tentar tirar algo debaixo ou do meio da pilha de pratos. Assim sendo, o único comportamento em que isso acontece é na primeira alternativa. Talvez alguém se confundisse com o da terceira, mas sempre tem que ter aquela pedra escorregadia pra pegar os espertinhos, né?

### Assumindo que, a princípio, `window.foo === undefined` é expressão verdadeira. Qual o valor retornado por uma chamada dessa função?

```js
function f() {  
  if (window.foo || (window.foo = "bar" )) {  
    return window.foo;  
  }  
}
```

### Alternativas
- ✔️ "bar"
- ReferenceError: Invalid left-hand side in assignment
- undefined
- true

#### Resposta
Essa é de fato pra testar um conhecimento mais intrínseco da linguagem. O primeiro detalhe é notar que dentro desse `if` não há uma comparação de igualdade, mas sim uma atribuição, porque só tem um único `=` ali. O segundo detalhe é saber que o retorno de uma atribuição é o próprio valor atribuído, assim `window.foo = "bar"` é uma expressão que retorna "bar". A utilidade  desse design seria permitir coisas como:

```js
let a;
let b;
const x = a = b = 1;
```

No trecho acima, todas as variáveis e a constante têm valor 1. Não é a mais legível das abordagens, mas é possível.

Logo, a resposta é "bar", porque o retorno da expressão dentro do `if` será verdadeiro, além de ter como efeito colateral a atribuição da string "bar" àquele dado a ser retornado.

### Qual o resultado dessa expressão de redução?
```js
([{ name: 'Dvd' }, { name: 'Lhama' }, { name: 'Pagode' }, { }]  
  .map(({ name }) => name)  
  .filter(it => it)  
  .join(' '));
```

#### Alternativas

- ✔️ "Dvd Lhama Pagode"
- "Dvd Lhama Pagode "
- "Dvd Lhama Pagode undefined"
- "name name name undefined"

#### Resposta

Aqui temos o que alguns chamam de _chain_ (encadeamento) de funções: o retorno de uma vira o argumento da próxima, o produto disso vira argumento de uma próxima e assim por diante.

Analisaremos por partes. Primeiro, tem um array de 4 objetos. Na segunda linha, existe um mapeamento de todos esses objetos para um array de nomes (e há um bom uso da sintaxe de  "destructuring" no parâmetro para extrair o valor de `name` de cada um): `["Dvd", "Lhama", "Pagode", undefined]`. Observe que sobrou um `undefined` no último elemento, porque era um objeto sem chave `name`. Na terceira linha, o `filter` analisa cada objeto e só deixa passar as expressões verdadeiras, e nesse momento o `undefined` desaparece porque seu correspondente booleano é `false`. Chamamos o `undefined` de "falsy value" por isso. Na quarta e última linha, os objetos restantes são concatenados entre si através do método `join` que liga os valores colocando o espaço em branco entre eles.

A resposta é, então: "Dvd Lhama Pagode", produto de um mapeamento de nomes devidamente filtrado e concatenado.

Dica de ninja: a combinação de um `map` com um `filter` podia ser feita com uma única linha de `reduce`. 😉

### Assumindo que o array `people` é indeterminado, qual opção melhor descreve, em Big O, a complexidade do algoritmo da função abaixo no pior caso?

```js
const people = [  
  { name: 'Alice', phone: '1234-4321' },  
  { name: 'Virna', phone: '8888-1269' },  
  // ...  
  { name: 'Xia', phone: '1111-4321' },  
];  
const findByName = name => people.find(it => it.name === name);
```

#### Alternativas

- O(3)
- O(3n)
- ✔️ O(n)
- O(n^2)

#### Resposta

Essa é bem contraditória e mais difícil do que se pensou durante sua elaboração.

Se você souber a notação de Big O, de cara já exclui a primeira e a segunda alternativa, pois pra esta notação a gente simplifica as constantes. Fica a dúvida quanto as duas últimas.

A função em questão é apenas `findByName`. Dentro dela, há comparação de strings. As strings, no JavaScript, são comparadas pelo valor de cada um de seus caracteres (em algumas linguagens como Java seria diferente). Então, aqui, se um `name` tiver 10 caracteres, então a comparação no pior caso fará 10 tarefas, mas se tiver 20 caracteres então a comparação pode ir até 20 caracteres, e assim por diante, de forma proporcional e linear.

Só pelo trecho de código dado, você pode chegar e dizer que a resposta é linear, O(n).

Há uma linha de raciocínio que nota a existência da iteração, através do `find`, em função do tamanho do array `people`, assim ficaria um conjunto de no máximo P tarefas do `find` multiplicado por um conjunto de no máximo Q tarefas de comparação com `name`. 

Apesar de ter elementos "indeterminados", em nenhum momento foi dito se os elementos de `people` variam por algum efeito colateral externo. E aí que causa a dúvida diante da imprevisibilidade das mutações. Mas como não há alternativas suficientes pra suportar essa divagação (ou outras quaisquer), então somos forçados a assumir que o tamanho de `people` é constante e podemos dar a resposta por O(n) mesmo.

### Qual opção melhor descreve a complexidade do algoritmo da função abaixo no pior caso? (Dica: considere `Math.pow` e `parseInt` como sendo O(1) para sua análise)

```js
const sqrt5 = Math.sqrt(5);  
const nthFibonacci = n => parseInt((1 / sqrt5) * (Math.pow((1 + sqrt5) / 2, n) - Math.pow((1 - sqrt5) / 2, n)), 10);
```

#### Alternativas
- Exponencial
- Logarítmica
- Linear
- ✔️ Constante

#### Resposta

Assustadora, a formula fechada para encontrar o enésimo valor de Fibonacci é esta função acima: `nthFibonacci`. Essa função tem diversas implementações, cada uma com complexidade diferente. Mas esta aqui é constante. Um erro comum é ter decorado uma das implementações e usar a análise dela como resposta aqui.

Seja qual for o tamanho do inteiro `n`, a função fará uma quantidade bem específica (e independente) de operações mesmo em seu pior caso. Aqui, nem precisa ler muito a fórmula, muito menos entender! E, pela dica do enunciado, nenhuma dessas operações internas faz tarefas variáveis tampouco.

O único objeto de dúvida seria o cálculo de raíz de 5, mas mesmo assim o resultado do `Math.sqrt` nunca mudará porque o parâmetro 5 é constante e `sqrt` é uma função matemática pura, ou seja, se aplicado um parâmetro 5 então a função apontará ao mesmo ponto na imagem, sem efeitos colaterais. De qualquer forma, o cálculo de raíz está fora do escopo da função, e seu valor definido como constante pelo próprio modificador `const` da linguagem.

### (Extra de React 1) A qual método do ciclo de vida do React esse hook `useEffect` abaixo melhor corresponderia?

```jsx
import React from 'react';  
import { fetchPeople } from './sync-people-lib';  
  
const PeopleList = () => {  
  const [people, setPeople] = React.useState([]);  
  React.useEffect(() => setPeople(fetchPeople()), []);  
  
  return (  
    <ul>  
      {people.map(person => ( // warning  
        <li>{person.name}</li>  
      )}  
    </ul>  
  );  
};  

export default PeopleList;
```

#### Alternativas
- componentDidUpdate
- ✔️ componentDidMount
- componentWillReceiveProps
- componentShouldUpdate

#### Resposta
O segundo parâmetro do `useEffect` é um array de valores em que o "efeito observa". Como não há nenhum valor sendo observado, ou seja, o array está explicitamente vazio, então o efeito daquela função anônima só é executado uma única vez e nunca mais reagirá a algo. Assim, se aproxima mais ao `componentDidMount`, método de ciclo de vida que só executa uma vez na vida do componente.

### (Extra de React 2) O seguinte código (semelhante ao acima) está imprimindo warnings no console e às vezes renderiza dados incorretos quando a lista `people` é reorganizada ou filtrada, então qual seria solução mais adequada dentre estas abaixo?

```jsx
import React from 'react';  
import connect from 'react-redux';  
  
const PeopleList = ({ people }) => (  
  <ul>  
    {people.map(person => (  
      <li>{person.name}</li>  
    )}  
  </ul>  
);  

const mapStateToProps = ({ people }) => ({ people });  
export default connect(mapStateToProps)(PeopleList);
```

#### Alternativas
- Usar o índice desse `map` como atributo de `key` para cada item de `li`
- Gerar um identificador único universal (UUID) baseado no endereço MAC em cada iteração desse `map` para atribuir às `key`de `li`
- ✔️ Gerar um UUID qualquer para cada item de `people` durante a criação/dispatch da action que atribuiria esse array ao estado, para usar os UUIDs como as `key` de `li`
- Gerar um UUID baseado em timestamp para cada item de `people` durante o reducer que atribuiria esse array ao estado, para usar os UUIDs como as `key` de `li`

#### Resposta

O Virtual DOM é uma entidade pouco compreendida e, por se tratar de um _pattern_ para otimização do DOM, facilmente vira uma faca de dois gumes. A fonte silenciosa de bugs mais comum é o mal (ou ausência de) uso de `key` formar no JSX conjunto de elementos. A propriedade `key` é usada como identificação única de cada item da lista mapeada, assim o React irá saber quando cada coisa foi alterada e invocar a renderização somente da alteração ao invés de repetir a renderização da lista inteira. O próprio React irá disparar um aviso no console do navegador a respeito, caso haja um `map` cujos elementos resultantes não tenham `key` em seu JSX.

A princípio, qualquer uma das alternativas fará o React parar de reclamar. Mas a melhor solução, dentre estas, é a terceira, porque as outras são _anti-patterns_  já documentados. Analisaremos cada uma.

A primeira não é adequada nesse contexto. Imagine que o React vê uma lista e cada chave é o índice dela. Se você colocar um novo elemento no meio do array, então o React irá tentar assumir, ao ver que a key é igual, que a renderização não deve mudar porque o item é o mesmo de antes.

A segunda é bem doida, mas também é ruim porque UUIDs são únicos e mudando a cada vez que o `map` rodar, então o React vai achar que são sempre novos componentes chegando e sempre tentar atualizar a renderização, mesmo que os itens sejam os mesmos.

A terceira e a correta, é um caso comum: um efeito colateral (assim como um `fetch` a APIs web) no momento de criação da `action` que vai ao Redux.

A quarta também funcionaria bem, exceto pelo fato de que um reducer é definido como uma função pura que combina estado atual com uma ação para criar um novo estado. E adicionar um UUID cujo padrão varia a cada milisegundo implicaria no reducer sofrendo efeitos colaterais do mundo externo, não sendo mais função pura e, assim, quebrando uma premissa muito importante de sua definição.

## Python

### Qual o resultado da expressão abaixo?

```py
f = lambda end: {n for n in range(end) if n}  
f(3)
```

#### Alternativas

- [1, 2]
- {'1': 1, '2': 2}
- ✔️ {1, 2}
- [1, 2, 3]

#### Resposta

Essa linha declara uma função que retorna um conjunto não-enumerávei de inteiros "verdadeiros" de 0 até `n` (intervalo aberto em `n`), ou seja, é o `set` de valores `{1, 2}`.

Por eliminação, caso tenha feito uma leitura atenta e saiba que Python não possui apenas _list comprehension_, você já excluiria as alternativas que contém colchetes ao invés de chaves, sobrando as do meio pra ficar na dúvida. A segunda alternativa seria possível somente em caso de _dict comprehension_ na forma `{str(n): n for n in range(end) if n}` que possui a notação de chave (conertida para string) e valor ao invés de somente valor.

### O que é verdade sobre o script abaixo?

```py
def add_contact(name, people=[]):  
  people.append(name)  
  return people  

people = add_contact('Marcell')  
people = add_contact('Sergio', people)  

other_people = add_contact('Evangilo')  
add_contact('Genilson', other_people)

print(', '.join(people))
```

#### Alternativas

- Dá erro, pois não é possível efetuar reatribuição nos parâmetros de uma função
- ✔️ people == other_people
- O output será: "Marcell, Sergio"
- O output será: "Evangilo, Genilson"

#### Resposta

A primeira alternativa não é verdade, mas, embora não dê erro de interpretação, é considerado má prática reatribuir argumentos recebidos. De qualquer forma, o `=`ali, apesar de tecnicamente ainda poder se considerar "atribuição", é a abstração que o Python possui para atribuir valores aos argumentos em caso de omissão durante a chamada da função.

Para entender por que a segunda é a verdadeira, façamos uma análise de cada chamada da função supracitada. Na primeira vez, `add_contact`  não recebe parâmetro `people`, então uma lista vazia é associada, recebe o `name` fornecido e tem sua referência retornada.

Na segunda vez, é passado a explicitamente mesma `people` como parâmetro, e assim a `add_contact` apenas inclui o novo nome e retorna a mesma referência.

Agora começa a parte confusa. Na terceira vez, o `people` é omitido, então o Python usa a mesmíssima referência dessa lista usada na chamada anterior. O retorno desta terceira chamada será uma lista com já dois nomes e incluindo agora o terceiro `name`.

Por consequência desse problema, na quarta e última chamada de `add_contact` a `people` passada acaba sendo a mesma que a terceira, que é a mesma referência da primeira e segunda chamada de `add_people`. Logo, são todas iguais e a saída seria "Marcell, Sergio, Evangilo, Genilson".

Caso essa explicação tenha sido difícil de digerir, ao menos guarde na cabeça de nunca usar valores mutáveis como parâmetros _default_ de funções, ou seja, apenas `None`, nùmeros, strings, tuplas e qualquer outro dado imutável "passado como valor" e não "passado como referência". Dentre exemplos de dados passados como referência, temos listas, dicionários e instâncias de classes

### Qual a saída desse script? (Considere apenas Python 3)

```py
people = ['wallysson', 'bruno', 'zenobio']  
names = map(lambda person: person.capitalize(), people)  

if names[0] == 'Wallysson':  
  print('It worked')  
elif names[0] == 'wallysson':  
  print('Not worked')  
else:  
  print('Not sure what happened')
``` 

#### Alternativas

- "It worked"
- "Not worked"
- "Not sure what happened"
- ✔️ Uma exceção é lançada

#### Resposta

O `map` aplica a função anônima para deixar maiúsculas as iniciais uma coleção `people` de itens. É uma estrutura de repetição comum em várias linguagens, com um apelo ao Paradigma Funcional.

Especificamente no Python 3, ao contrário de Python 2 ou de linguagens como JavaScript, esse produto retornado `names` não possui o exatamente mesmo tipo que sua entrada `people`. Isso porque `names` não será lista, mas sim um _iterator_ (iterador ou gerador) conforme é o tipo hoje esperado de retorno para `map`.

Basicamente, isso significa que `names[0]` (ou qualquer outro elemento do produto `names`) ainda não existe e precisa ser consumido por ferramentas de iteração, como um loop `for` ou qualquer outra função que precise fazer o consumo desses dados. E isso também significa que a instância `names` de _iterator_, por ainda não ser uma lista, lançará exceção ao tentar ser usada como tal. Portanto a última resposta ė a correta.

### Qual o resultado da expressão abaixo?

```py
class Person():
  pass

class Tester(Person):
  pass

class Coder(Person):
  pass

Tester.name = 'Jaon'
Person.name = 'Rodrigo'

Person.name, Tester.name, Coder.name
``` 

####  Alternativas

- ✔️ ('Rodrigo', 'Jaon', 'Rodrigo')
- ('Jaon', 'Rodrigo', 'Jaon')
- ('Jaon', 'Jaon', 'Jaon')
- ('Rodrigo', 'Rodrigo', 'Rodrigo')

#### Resposta

Um problema clássico de modelagem pelo Paradigma de Orientação a Objetos. Temos três classes, sendo a primeira delas a superclasse das seguintes.

Após a declaração das classes, há uma subclasse `Tester` tendo seu atributo `name` padronizado em "Jaon"; na linha seguinte há a superclasse `Person` tendo 
o seu como "Rodrigo".

Ao formar a expressão final de tupla, os valores serão os mesmos conforme explicado no parágrafo anterior. Deve-se observar que `Coder`, por não ter `name` próprio, irá recorrer à implementação de sua superclasse `Person` e, portanto, teremos o terceiro valor da tupla como "Rodrigo" também. 

### Assumindo que a lista `people` é indeterminada, qual opção melhor descreve, em Big O, a complexidade do algoritmo da função abaixo no pior caso?

```py
people = [  
  dict(name='Iuri', phone='1234-4321'),  
  dict(name='Italo', phone='8888-1269'),  
  # ...
  dict(name='Alice', phone='1111-4321'),  
]  

def find_by_name(name, people):  
  for person in people:  
    if person.get('name') == name:  
      return person  
  return None
```

#### Explicação

As alternativas e respostas são praticamente as mesmas da penúltima pergunta (não-extra) da seção de JavaScript.

### Qual opção melhor descreve a complexidade do algoritmo da função abaixo no pior caso? (Dica: considere `math.pow` e `int` como sendo O(1) para sua análise)

```py
import math  
sqrt5 = math.sqrt(5)  
nthFibonacci = lambda n: int((1 / sqrt5) * (math.pow((1 + sqrt5) / 2, n) - math.pow((1 - sqrt5) / 2, n)))
```

#### Explicação

As alternativas e respostas são praticamente as mesmas da última pergunta (não-extra) da seção de JavaScript.


### (Extra de GNU/Linux 1) No GNU/Linux, qual dos seguintes comandos mostra o ring buffer de mensagens de operações do kernel?

#### Alternativas

- kmesg
- ✔️ dmesg
- cat /var/xmesg
- cat /var/run/zmesg

#### Respostas

Pergunta bem frequente de simulados para ceriticaçöes GNU/Linux e uma boa ferramenta de investigação. Quem "sabe, então sabe":`dmesg`mostra essas mensagens do sistema.

### (Extra de GNU/Linux 2) No GNU/Linux, quando você precisa achar um arquivo de configurações de certo serviço, qual seria o primeiro diretório a se procurá-lo?

#### Alternativas

- /opt
- /var
- ✔️ /etc
- /dev

#### Resposta

Outra pergunta quente para certificaçōes e uma das informaçōes clássicas que se aprende ao estudar algum GNU/Linux ou sistema do tipo Unix é sobre a organizaçäo dos diretórios da raíz, ou FHC (_Filesystem Hierarchy Standard__ traduzido como "padrão hierárquico de sistema de arquivos"). O "opt" guarda os estados dos processos em execução na máquina; o "var" é para arquivos muito instáveis como _logs_ e _locks_; o "etc" (resposta correta) guardaria os arquivos de configuração do sistema; e por fim o "dev" guardando representações dos dispositivos. 

