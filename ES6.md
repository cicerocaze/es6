# ES6 - Novidades do ES6

### Escopo de variáveis LET x VAR
* LET - Possue escopo de bloco, se for declarada mais de uma vez o JavaScript lançará um erro
* VAR - Possue escopo de função ou global


* **Evitando que variáveis do tipo VAR, vaze de um escopo**:
```javascript
var idade = 18;
var temCarteira = true;

(function() {
    if(idade >= 18 && temCarteira) {
        var msg = 'Pode dirigir';
        console.log(msg);
    }
})();

alert(msg); // exibe undefined
```

### Arrow Function
* Em algumas situações precisamos criar funções anônimas, imagine que precisamos iterar um array e para cada elemento do array vamos precisar executar um processamento qualquer. Este é um exemplo de onde podemos utilizar funções anônimas, veja o exemplo:
```javascript
let versions = [5, 6, 7, 8];
versions.map(function (item) {
    return 'es' + item;
});
// > ['es5', 'es6', 'es7', 'es8']
```
* Para este mesmo exemplo podemos utilizar a Arrow Function, que é uma nova maneira de declarar funções anônimas, veja como ficaria o exemplo acima com uso de Arrow Function:
```javascript
let versions = [5, 6, 7, 8];
versions.map((item) => {
    return `es${item}`;
});
```
* Caso nossa Arrow Function seja apenas de uma linha, podemos escreve-la sem as {} e caso receba apenas um parâmetro, podemos escreve-la sem o ().
```javascript
let versions = [5, 6, 7, 8];

//Arrow functions com parâmetro
versions
    .filter(item => item > 5)
    .map(item => `es${item}`);

// > ['es6', 'es7', 'es8']
```
* Mas se minha Arrow Function não receber parâmetro? Vou precisar deixar o () vazio? Sim 
```javascript
let versions = [5, 6, 7, 8];
document.addEventListener('click', () => console.log('click'));
```

**É importante citar também, caso vocês não tenham notado, que arrow functions, quando são de uma linha só, não precisam do return para indicar o que ela está retornando, o valor já é retornado automaticamente**

** Explicar o escopo do this em Arrow Function (escopo léxico - isto é, seu this é estático e não muda)
O this de uma arrow function é léxico, isto é, seu valor é determinado no local onde a arrow function for definida, ela não cria um novo this. O this de uma arrow function não pode ser alterado, mesmo se usarmos recursos da linguagem, como a API Reflect.

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>

    <script>

        class SistemaImpressao {

            constructor() {
                this._codigo = 2;
            }
            
            /*
            imprime(nomes) {
                nomes.forEach(function(nome) {
                    console.log(this);
                    console.log(`${this._codigo}: ${nome}`);
                });
            }
            */
               
            imprime(nomes) {
                // usando arrow function.
                nomes.forEach(nome => {
                    console.log(this);
                    console.log(`${this._codigo}: ${nome}`);
                });
            }
        }

        let nomes = ['Flávio', 'Nico', 'Douglas'];
        let si = new SistemaImpressao();
        si.imprime(nomes);

    </script>
</body>
</html>
```
* Na função imprime que está comentada o código não funciona, pois dentro da forEach o this é undefined.
* Já na função imprime que não está comentada, o fato es utilizarmos arrow function faz com que o this dentro de forEach seja o mesmo this da classe. Já que arrow functions utilizam-se do mesmo this existente no momento da declaração da arrow function.

** Cuidado com o escopo da tag <script>
* Quando estamos escrevendo códio dentro da tag <script>, lembre-se que o this está no escopo de window. 
Então um código que aparentemente devria funcionar, pode não funcionar. Veja o exemplo abaixo:

```javascript
<script>
    let carro = {
        velocidade: 100,
        acelera : () =>  {
            console.log(this);
            console.log(`Carro a ${this.velocidade} km por hora!`);
        }
    };
    carro.acelera();
</script>
```
* O código acima não funciona pois no momento da criação da arrow function o escopo de this é window....

* Já o código abaixo que aparentemente poderia dar erro, se estivéssemo dentro de uma classe, irá funciona
```javascript
<script>
    let carro = {
        velocidade: 100,
        acelera :function() {
            console.log(this);
            console.log(`Carro a ${this.velocidade} km por hora!`);
        }
    };
    carro.acelera();
</script>
```
* Como nossa função está sendo chamada a partir de um objeto, por padrão, o this dessa função será o objeto. E no escopo deste objeto existe a propriedade velocidade


### Spread Operator
* O Spread Operator basicamente converte um array em argumentos, ele é muito útil quando se precisar quebrar um array para passar seus valores para uma função ou construtor de um objeto como argumentos de valores separados.
```javascript
function soma(a, b) {
    return a + b;
}

var arr = [1, 2];
soma(...arr); // retorna: 3
```

Outro Exemplo:
```javascript
let lista1 = ['banana', 'laranja', 'mamão'];
let lista2 = ['caju', 'tangerina', 'abacaxi'];

lista1.push(...lista2);
console.log(lista1); //["banana", "laranja", "mamão", "caju", "tangerina", "abacaxi"]
```

Exemplo Real:
* Imagine que você tem uma funão que recebe 3 parâmetros em caixa alta. E você recebe os 3 parâmetros em um único input de seu formulário em caixa baixa, separados por uma barra (/). Veja como o spread operator pode nos ajudar:
```javascript
//Primeiro utilizamos o split para quebrar as 3 informaões separadas por uma barra (/) em um array e depois criamos um novo array através do map convertendo-os para caixa alta.
let dados = this._inputDados.value.split('/').map(item => item.toUpperCase());

//Ou podemos já transformar a informaão em caixa alta e depois quebrar  as 3 informações separadas por uma barra () através do split, evitando o uso desnecessário do map
let dados = this._inputDados.value.toUpperCase().split('/');

//Por fim enviamos o array dados com uso do spread operator, que irá quebrar cada item do array como um parametro
let arquivo = new Arquivo(...dados);
```

### Template String
* O Template String é a utilização do backtick (crase) para concatenação de Strings com variáveis. Com a estrtura do template string facilitamos o processo de concatenação entre strings e variáveis, sendo menos sujeita a erros de concatenção. Vejamos um exemplo:
```javascript
let idade = 18;
let nome = "Daniel";

console.log(`A idade de ${nome} é ${idade}`);
```

### O uso do bind()
* O uso do bind permite indicar qual será o valor de this, quando a função que recebe o bind for invocada. Veja o exemplo abaixo:

```javascript
class Pessoa {
    constructor(nome) {
        this.nome = nome;
    }
}

function exibeNome() {
    alert(this.nome);
}

let pessoa = new Pessoa('Salsifufu');
exibeNome = exibeNome.bind(pessoa);
exibeNome();
```

* No exemplo acima, a função exibeNome recebe como valor (através do uso do bind) para o this da pŕopria função o objeto pessoa. Por isso quando é executado ( exibeNome(); ), a função exibeNome consegue imprimir o nome 'Salsifufu' corretamente. Pois o valor de this é o objeto pessoa e em pessoa existe o atributo nome.

### O uso do map()
* O método map nos permite percorrer cada elemento do array aplicando alguma transformação, e guardando os valores processados/transformados em um novo array. Veja o exemplo:

```javascript
let numeros = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100, 121];

//Utilização do map para dobrar o valor de cada item do array, guarando esse novo valor em um novo array
let dobro = numeros.map(function(num) {
    return num * 2;
});

//Utilização do map para dividir cada elemento do array por 2 (gerar a metadade do valor), guarando esse novo valor em um novo array
let metade = numeros.map(function(num) {
    return num/2;
});

//Utilização do map para gerar a raiz quadrada do valor de cada item do array, guarando esse novo valor em um novo array
let raiz = numeros.map(function(num) {
    return Math.sqrt(num);
});
```

* Utilizando Map + Arrow Function:

```javascript
let dobro = numeros.map(num => num * 2);
let metade = numeros.map(num => num/2);
let raiz = numeros.map(num => Math.sqrt(num));
```
Por ser apenas um parâmetro, não precisamos colocar os parênteses. De:(item) => Para: item => 

### O uso do filter()

### O uso do reduce()
* O método reduce nos permite aplicar uma função para cada item de um array, reduzindo o resultado para uma única variável. Ou seja, percorremos cada elemento do array aplicando algum processamento e acumulando o resultado em uma única variável. Veja o exemplo abaixo: 

```javascript
let numeros = [1, 2, 3, 4];
let resultado = numeros.reduce(function(total, num) {
    return total * num;
}, 1);
```
* Na função interna ao reduce, o primeiro parâmetro é o valor da última iteração, que neste caso é o total. O segundo parâmetro é o valor da iteração atual.

* O total se inicia com o valor 1, definido pelo segundo parâmetro da função reduce. É feita a primeira iteração, pegando o primeiro valor do array (1) :

```javascript
return total * num; // Leia-se: return 1 * 1 e coloque este valor em total.
```

* Na segunda iteração, com o segundo valor do array (2):

```javascript
return total * num; // Leia-se return 1 * 2 e coloque este valor em total, que agora vale 2;
```

* O mesmo código com adição de Arrow Function:

```javascript
numeros.reduce((total, num) => total * num , 1);
```

### Proxy

### Promises

### API Reflection
* Alteração do this em execução
* Reutilização de código através de mixin, com api reflection
