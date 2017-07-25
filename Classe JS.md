# ES6 - Herança

*Classe principal*
```javascript
class Conta {
    constructor(saldo) {
        //Atributos com prefixo _ por convenção não devem ser acessados diretamente pelos desenvolvedores e sim através de getters e setters
        this._saldo = saldo;
    }
    
    //Com a utilização do prefixo _ antes dos atributos privados, devemos criar métodos getters conforme abaixo:
    getSaldo() {
        return this._saldo;
    }
    
    //Podemos criar uma espécie de método (video abaixo), que é acessado como se fosse uma propriedade
    get saldo() {
        return this._saldo;
    }

    atualiza(taxa) {
        throw new Error('Você deve sobrescrever o método ');
    }
}

var conta = new Conta(100);
console.log(conta.saldo); //acessando como se fosse uma propriedade!
```

*Classe com uso de herança que reescreve o método atualiza para somar a taxa ao saldo*
```javascript
class ContaCorrente extends Conta {
    atualiza(taxa) {
        this._saldo = this._saldo + taxa;
    }
}
```

*Classe com uso de herança que reescreve o método atualiza para somar o dobro da taxa ao saldo*
```javascript
class ContaPoupanca extends Conta {

    atualiza(taxa) {
        this._saldo = this._saldo + taxa * 2;
    }
}
```
