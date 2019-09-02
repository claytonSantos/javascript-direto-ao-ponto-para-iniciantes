
### JAVASCRIPT DIRETO AO PONTO PARA INICIANTES

##### MOTIVAÇÃO
**Este não é um tutorial de javascript**. É um compilado de particularidades da linguagem que encontrei dificuldade no início do aprendizado:

- [Diferentes maneiras de criar funções](#highlights)
- [Como "this" funciona](#highlights)
- [callbacks](#highlights)
- [Diferentes maneiras de criar objetos javascript](#highlights)
- [Basico conceito de referência de objetos](#highlights)
- [Links pra tópicos mais aprofundados](#highlights)

#### ESCREVER FUNÇÕES

##### Função declarada (Function Declaration)

Sintaxe
```javascript

function functionName(parameters) {
  code to be executed
}
```
Uso:
```javascript
function soma (a,b){
  return a+b;
}

var result = soma(5,5);

console.log(result); // 10
```

##### Função de Expressão (Function Expression)

Uma referência da função pode ser atribuida a uma variável

```javascript
var soma = function(a,b){
 return a + b;
}
```
Se imprimirmos a "variável" soma, veremos que na verdade ela é uma **função**

```
console.log(soma);
// function(a,b){
// return a + b;
// }

var result = soma(5,5);

console.log(result); // 10
```

##### Funções que retornam outra função


Funções que são declaradas dentro de outras funções são chamadas de funções internas ou aninhadas.

```javascript
var calc = function(x){
	return function(y){
		return x * y;
	}
}
var dobraValor = calc(2);
```
A função calc é chamada e o retorno é a função interna que é atribuída a variável dobraValor.
```javascript
var res = dobraValor(5);
console.log(res); //10
```

#### ESCOPO DE FUNÇÕES 

Escopo pode ser definido em dicionários como: “Limite ou abrangência de uma operação”

Quando uma função é criada ela cria um escopo, no qual seu conteúdo só é visível por ela.

exemplo:
```javascript
function foo(){
 let name = "Clayton";
  return name;
}
console.log(name); 
//undefined
```

name é definida dentro da função foo é só é visível de dentro dela. Em outras palavras, a variável name pertence ao escopo de foo e não pode ser acessada de funções externas.

O contrário é permitido: Funções internas tem acesso a variáveis de funções externas, das quais ela reside. 

```javascript
function a(){
	let name = "Clayton";
 	return function b(){
		return name;
	}
}

let b = a();
let res = b();
console.log(res); //clayton
```

Outra coisa sobre o escopo é que ele é criado no momento da chamada e guardado na memória.

No tópico anterior quando calc foi chamado passamos o 2 por parâmetro. Mas ao imprimirmos dobraValor recebemos uma função que parece ter pedido o valor de x.

```javascript
console.log(dobraValor);
// function(y){
// return x * y;
// }
```

Pense um pouco… 

No momento em que calc foi chamada a função retornada cria e armazena seu escopo na memória. Como o escopo das funções internas abrange as funções na qual ela reside, no momento da chamada o escopo é armazenado na memória e na próxima chamada a função interna acessa o valor de x e pode fazer o cálculo.

#### OBJETOS

Bom, quando iniciei JavaScript isso era algo que eu sempre tive problemas. JavaScript é muito versátil e te permite criar objetos de algumas maneiras diferentes.
Vamos lá.

##### Objeto Literal

Esse tipo é bem comum. É um objeto pronto pra uso. Amplamente usado em parâmetros de funções para configurações de plugins ou outras funções, por exemplo. Essencialmente esse objeto é representado por {}. 

Vamos lá;
Criar um objeto com duas variáveis e atribuir a uma variável config.

```javascript
var config = {
  Identacao: “tabs”,
  espacos: 4
}
```

Note, a última variável não pode conter vírgulas. 

Você poderia passar esse objeto para uma função de duas maneiras:

```javascript
meuframework({
  Identacao: “tabs”,
  espacos: 4
});
```
Ou:

```javascript
meuframework(config);
```

Um objeto literal pode conter métodos também.

```javascript
var pessoa = {
    firstName:"Clayton",
    lastName: "Santos",
    fullName: function( ) {
        return this.firstName + " " + this.lastName;
    }
}
console.log( pessoa.fullName ( ) );  // Clayton Santos
```


##### Também Podemos Construir Objetos Dessa Maneira
```javascript
var pessoa = { };   // criar um obj vazio
pessoa.name;    // atribui um atributo ao obj
pessoa.setName = function ( name ) {
	this.name = name;
}
pessoa.getName = function () {
	return this.name;
}
pessoa.setName('Clayton');
alert( pessoa.getName() ); // Clayton
```
 Usamos a palavra reservada this para fazer referência ao objeto atual e acessar suas variáveis. Não se preocupe, o this será mencionado posteriormente.

A maneira que criamos objetos acimas são pouco flexíveis e não nos permite criar novos objetos com atributos diferentes. Abaixo veremos duas maneiras de criar objetos que podem gerar instâncias diversas. 

##### Construtor de objetos new

```javascript
function person( firstName , lastName , age , eyeColor ) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
    this.eyeColor = eyeColor;
    this.getFullName = function ( ) {
         return this.firstName + this.lastName;
    }
}
var pessoa1 = new person("Clayton", "Santos", 48, "green");
var pessoa2 = new person("João", "Santos", 48, "green");

alert( pessoa1.getFullName() );    // Clayton Santos
alert( pessoa2.getFullName() );    // João Santos
```

#### OBJETOS EM JAVASCRIPT SÃO ABORDADOS POR REFERÊNCIA

Objetos em javascrip são abordados por referência, não por valor.
Se foo é um objeto, a seguinte instrução não vai criar uma cópia de foo:
```javascript
var foo = {};
var bar = foo;  // Isso não criará uma cópia de foo.
```
A variável bar não é uma cópia de foo. Ela é foo. bar e foo apontam para o mesmo objeto.
Quaisquer alterações em bar também mudará foo, porque foo e bar apontam para o mesmo objeto.
```javascript
var pessoa1 = {nome:"John", idade:50}
var pessoa2 = pessoa1;
pessoa2.idade = 10;
alert(pessoa1.idade); // 10
alert(pessoa2.idade); // 10
```

#### CALLBACKS

Pra resumir, calbacks são funções passadas por parâmetros em outras funções.

Vamos criar uma função que verifica se uma imagem foi carregada e permitir que o desenvolvedor faça algo depois que a imagem carregar:

Vamos supor que a função seja essa: 
```javascript
let loadImg = function( imagem, calback){
  //faça algo para verificar se a imagem carregou
  If( imagem carregou ){
    calback();
  }
}
```
Usando essa função, você pode fazer qualquer coisa depois que a imagem for carregada. 

```javascript
loadImg( imagem, function(){
  console.log("imagem carregou");
});
```
Ou:

```javascript
let hello = function(){
  console.log("imagem carregou");
}
loadImg(imagem, hello);
```

O javascript está cheio de funções dessa maneira.

Há um artigo muito bom sobre programação funcional no médium e um parte trata de callbacks: [Entendendo Programação Funcional em JavaScript de uma vez.](https://medium.com/tableless/entendendo-programa%C3%A7%C3%A3o-funcional-em-javascript-de-uma-vez-c676489be08b)

#### THIS EM JAVASCRIPT

Algo muito confuso em JavaScript é o this. Em linguagens como Java por exemplo, o this representa o objeto que chamou o método. Como aqui em JavaScript o this é definido no momento da chamada e não quando é definido o objeto/função é fácil cometer erros.

Mas vou contar um segredo que aprendi no curso do udacity que vai te ajudar em 90% dos casos: Quando você vè um ponto à esquerda de uma chamada de função, significa que ela foi vista como propriedade de um objeto.
```javascript
let pessoa = {
 getThis(){
  return this;
 }
}

console.log( pessoa.getThis())
// pessoa
```

Antes de prosseguir preciso explicar sobre funções criadas no espaço global. Logo voltaremos ao this.
Em JavaScript há uma função chamada setTimeout que recebe dois parâmetros: uma função e o tempo. Quando setTimeout é chamado ele aguarda o tempo que passamos e quando esse tempo expira ele chama a função. 
setTimeout (funcao, tempo em milissegundos);

note a ausência de um ponto antes da chamada de setTimeout. Por motivos didáticos, não falei que funções chamadas sem um ponto antes, na verdade são chamadas implicitamente por um objeto **global** chamado **window**.

window.setTimeout();
Outras funções como console.log e alert funcionam da mesma maneira. 
Não só essas, mas todas funções criadas diretamente no espaço global pertencem ao objeto window.


Voltando ao this .. 
Agora a cereja do bolo;
```javascript
let pessoa = {
  name: 'Clayton',
 imprimeName(){
  console.log(this);
 }
}

Se executarmos pessoa.imprimeName(); /// this = pessoa
```
E se passarmos a função para setTimeOut?
```javascript
setTimeout(pessoa.imprimeName, 2000);
```

Note a ausencia de parenteses, pois queremos passar a função. Se colocassemos parenteses a função seria lida e executada imediatamente.

Após um segundo qual o valor de this que pessoa.imprimeName imprimirá?
Pense um pouco…
Lembra o que eu disse que quando você vè um ponto à esquerda de uma chamada de função, significa que ela foi vista como propriedade de um objeto? Então você poderia dizer que o valor de this retornado é pessoa. No entanto se você respondeu pessoa você errou. 

Vamos olhar a função setTimeout() mais de perto. 
Talvez ela fosse implementada dessa maneira.

```javascript
var setTimeout = function(callback, time){
	espere(time);
	callback();
}
```

Para fim de didática, considere a chamada de função dessa maneira:
```javascript
setTimeout( imprimeName, 2000) {
	espere(2000);
	imprimeName();
}
```

E ai, já descobriu qual o valor de this? Vou repetir:

1 - O this é avaliado sempre no momento de execução e não no momento onde ele foi definido.

2 - quando você vè um ponto à esquerda de uma chamada de função, significa que ela foi vista como propriedade de um objeto. 

No caso do setTimeout embora a função imprimeName() seja definida dentro do objeto pessoa, ela é passada e executada posteriormente. Quando o callback é chamado note que **não há ponto** antes. O window implicitamente chama a função.

Na maioria das vezes, quando se trata de callbacks, é comum perder o valor de this. Há maneiras de garantir que o this seja exatamente onde a função foi criada, mas isso é tópico mais adiante.  


#### MANIPULANDO O HTML

Em frontend é comum manipularmos o dom anexando eventos ou alterando. Eu chamo de dom todo o documento com todas as tags html. Tags eu chamo de **elementos do dom.** 

O princípio é sempre o mesmo:

1. Obter elementos do dom no javascript e armazená-los numa variável.
2. Anexar eventos a esses elementos.
3. E fazer algo quando um evento nestes elementos acontecerem. 

Para obter elementos do dom podemos utilizar dois métodos bastante versáteis: **document.querySelector()** e **document.querySelectorAll()**. Esses métodos recebem por parametro um selector css e retorna os elementos que correspondem com o seletor. **querySeletorAll** retorna um array com todos os elementos do dom que correspondem com o seletor. 
**querySelector** retorna apenas a primeira correspondencia do dom, mesmo que mais elementos correspondam a pesquisa.

**document.querySelector()**

```javascript
1. var elem = document.querySelector("#code"); 
// 1. elem agora contém um elemento(tag) do dom que contém o Id #code

2. elem.addEventListener(“click”, function(){
	console.log(“o elemento foi clicado”);
);
//2. Aqui eu adiciono ao elemento, através da função addEventLister,
// um ouvinte de clique. 
// Quando o clique ocorrer a função de calback é chamada e podemos 
// fazer algo nessa função. Nesse caso apenas mostramos um log.
```
**document.querySelectorAll()**

```javascript
1 - var elems = document.querySelectorAll(“.elementos”);
// querySelectorAll retorna um array* de elementos.

```
```javascript
// precisamos percorrer esse array e acessar cada elemento.
// para cada elemento adicionar o evento
2 -  for (i = 0; i < elems .length; i++) { 
      elem = elems[i];
      elem.addEventListener(“click”, function(){
      console.log(“o elemento foi clicado”);
      });
}
```
É necessário percorrer cada elemento. Se você tentar anexar um 
evento diretamente:
**Elems**.addEventListener(ev,fn); você vai obter um erro no console e o JavaScript vai falhar 


Se de **elems** você quiser apenas o primeiro elemento você pode fazer:

var primeiroElem = elems[0];

E assim por diante…
Há outros métodos para obter elementos do dom. Mas acho que te expliquei o mínimo pra você fazer o que imaginar.
Há outras opções e você pode ver aqui:
[JavaScript HTML DOM Elements.](https://www.w3schools.com/js/js_htmldom_elements.asp)

Para referência de quais eventos você pode anexar nos elementos(tag) pode consultar:[HTML DOM Events](https://www.w3schools.com/jsref/dom_obj_event.asp)

#### Recapitulando callbacks, this numa mesma função e prototype

Como dito, JavaScript possui várias funções que usam nativamente callbacks. 
Já vimos como percorrer um array com o for basicão. Mas JavaScript nos dá uma maneira elegante de percorrer elementos com a função forEach() que recebe um callback por parâmetros que é chamada para cada item do array. 
Note que cada item do array é passado para o callback.

```javascript
var arr = [2,4,6,8];

arr.forEach(function(item){
	console.log(item);
});
// 2 4 6 8
```
Quando comecei, me intrigava imaginar como essa função foi implementada. Eu me perguntava:
 
1. Como a função de calback tem acesso a cada item?
Como a função de calback é chamada em cada interação?

2. Para responder a essas perguntas eu implementei a minha própria função pra tentar entender e cheguei nessa solução:


```javascript
arr = [1,2,3,4,5];

Array.prototype.myForEach = function(calback){
	for (var i=0;i<this.length;i++) {
	   let item = this[i];
                 calback(item);
	}
}

arr.myForEach(function(x){
	console.log(x);
});
```
Temos uma coisa nova nessa função: **prototype**. Pra ficar básico, prototype nesse caso anexa ao objeto Array nativo a função myForEach. Isso faz com que qualquer array a partir de agora herde essa função do Array nativo. Se você vem de outras linguagens orientadas a objetos, esse conceito é muito parecido com herança. 

Quem chama a função é o array, então **this = arr**

A partir daí fazermos um for basicão, chamamos o calback pra cada item, pois dentro da função temos acesso ao array e consequentemente seus itens. 