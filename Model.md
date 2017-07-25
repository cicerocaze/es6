# ES6 - Orientação a Objeto (Model)

***Declaração de classe***
```javascript
class Pessoa {
    constructor(nome, sobrenome) {
        this.nome = nome;
        this.sobrenome = sobrenome;
    }
}
```

***Declaração de classe, com convenção de atributos privados***
```javascript
class Conta {
    constructor(titular, conta) {
        this._titular = titular;
        this._conta = conta;
        this._saldo = 0.0
    }

    deposita(valor) {
        console.log('Valor depositado: ' + valor);
        this._saldo+=valor; 
    }
}
```

***Declaração de classe, com convenção de atributos privados, métodos Getters e acesso aos Getters***
```javascript
class Conta {
    constructor(titular, conta) {
        this._titular = titular;
        this._conta = conta;
        this._saldo = 0.0
    }

    deposita(valor) {
        console.log('Valor depositado: ' + valor);
        this._saldo+=valor; 
    }

    getSaldo() {
        return this._saldo;
    }

    getTitular() {
        return this._titular;
    }

    getConta() {
        return this._conta;
    }
}

var conta = new Conta('Mingau', 171);
conta.deposita(100);
console.log(conta.getTitular());
console.log(conta.getConta());
console.log(conta.getSaldo());
```

***Declaração de classe, com convenção de atributos privados, métodos Getters e acesso aos Getters como se fossem atributos***
```javascript
class Conta {
    constructor(titular, conta) {
        this._titular = titular;
        this._conta = conta;
        this._saldo = 0.0
    }

    deposita(valor) {
        console.log('Valor depositado: ' + valor);
        this._saldo+=valor; 
    }

    get saldo() {
        return this._saldo;
    }

    get titular() {
        return this._titular;
    }

    get conta() {
        return this._conta;
    }
}

var conta = new Conta('Mingau', 171);
conta.deposita(100);
console.log(conta.titular); (acessando como se fosse uma propriedade)
console.log(conta.conta);
console.log(conta.saldo);
```

***Congelamento de objetos, para o caso de o desenvolvedor não seguir a convenção de atributos com _***
```javascript
class Negociacao {
    constructor(data, quantidade, valor) {
        this._data = data;
        this._quantidade = quantidade;
        this._valor = valor;
    }

    get volume() {
        return this._quantidade * this._valor;
    }

    get data() {
        return this._data;
    }

    get quantidade() {
        return this._quantidade;
    }

    get valor() {
        return this._valor;
    }
}

var negociacao = new Negociacao(new Date(), 1, 100);
Object.freeze(negociacao);
```

***Congelada porém não Imutável***
```javascript
class Negociacao {
    constructor(data, quantidade, valor) {
        this._data = data;
        this._quantidade = quantidade;
        this._valor = valor;

        Object.freeze(this); // congela a instância do objeto
    }

    get volume() {
        return this._quantidade * this._valor;
    }

    get data() {
        return this._data;
    }

    get quantidade() {
        return this._quantidade;
    }

    get valor() {
        return this._valor;
    }
}

var negociacao = new Negociacao(new Date(), 1, 100);
negociacao.data.setDate(negociacao.data.getDate() + 1); //NÃO SE PODE ATRIBUIR NOVOS VALORES, PORÉM É POSSÍVEL CHAMAR MÉTODOS 
```
***Programação Defensiva***
```javascript
class Negociacao {

    constructor(data, quantidade, valor) {
        
        //Para evitar que alterações na referência passada, alterem o estado de nossa classe, criamos uma nova instância 
        this._data = new Date(data.getTime()); // criando uma nova instância a partir do tempo de uma data 
        this._quantidade = quantidade;
        this._valor = valor;
        Object.freeze(this);
    }

    get volume() {
        return this._quantidade * this._valor;
    }
    
    //Para evitar que alterações na referência retornada, alterem o estado de nossa classe, criamos uma nova instância 
    get data() {
        return new Date(this._data.getTime()); //Retornando uma nova instância
    }

    get quantidade() {
        return this._quantidade;
    }

    get valor() {
        return this._valor;
    }
}
```

