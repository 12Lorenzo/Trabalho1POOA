#### Lorenzo Correia Maia R.A.: 759502
#### Lucas machado Cid R.A.: 769841

# Single Responsibility Principle
## Definição Rápida

O princípio a ser debatido tem como ideia que, em cada classe ou função, esa deve ter um único motivo para existir e para ser alterada. Em projetos de grande escala isso se torna especialmente relevante, uma vez que partes do código podem ser mantidas por pessoas diferentes de setores diferentes. Neste caso, é interessante que uma classe não assuma funções variadas, já que a alteração em uma função pode impactar diretamente na funcionalidade de outras funções que não tem, logicamente, nenhuma relação com ela.

Um exemplo que podemos trazer do mundo real é um canivete suiço, que possui diversas funcionalidades, desde ser uma lâmina até uma chave de fenda. Apesar dele ter a ideia de prover diversas funções, é um instrumento um tanto quanto estranho à primeira vista, além de possivelmente difícil de ser usado. Aplicando o princípio da responsabilidade única, poderíamos transformar um canivete suiço em uma caixa de ferramenta com suas seções, em que cada uma está bem distribuída com ferramentas específicas, como para parafusos, outra para chaves e assim por diante.

## Exemplo de violação do Princípio

Em linhas de código podemos exemplificar utilizando uma lógica para veículos, na qual um sistema como o DETRAN poderia usufruir da classe veiculo para armazenar atributos importantes, como dono, placa, documento e impostos, e usando de métodos para alterar ou calcular certos cenários. A seguir temos um código referente ao cenário descrito que não aplica o princípio da responsabilidade única:

``` cpp
class Veiculo{
    public:
        void setDono(string nome, int idade);
        void setDocumento(string documento1, string documento2);
        void setPlaca(string placa);
        float calcIpva(); 
}
```

## Exemplo com a aplicação correta do Princípio

Nesta classe temos o método setDono, que muda o dono do veículo (as propriedades foram ocultadas), a propriedade setDocumento que é utilizada para alterar a documentação do veículo, setPlaca para alterar a placa do veículo seguindo o padrão vigente no momento e calcIpva que calcula o IPVA a ser pago naquele ano.

Apesar do agrupamento feito fazer sentido à primeira vista para um veículo, se analisarmos mais cuidadosamente sob a ótica do Princípio de Responsabilidade Única, existem possíveis problemas na forma com que foi implementada a classe. Por exemplo, caso ocorra alguma alteração no modelo padrão que as placas de veículos devem se encaixar, situação que ocorreu, por exemplo, em fevereiro de 2020, teríamos que alterar diretamente a classe. Outra situação bem comum, e que pode acontecer com frequência, é o cálculo do IPVA mudar, assim a mesma classe será alterada, desta forma ferindo o princípio. Além do mais, uma única classe realiza funções que abrangem diversos setores diferentes: certamente o setor responsável pelo cálculo do IPVA não é o mesmo responsável pela parte da documentação. Caso algo tenha que ser alterado em alguma função, esta alteração poderá acabar se propagando para setores não relacionados com o qual a mudança foi feita. Desse modo, foi necessário refatorar o código a fim de respeitar o Princípio da Responsabilidade Única, resultando no seguinte código:


``` cpp
class Veiculo{
    public:
        void setDono(Pessoa dono);
        void setDocumentacao(Documento documento);
        void setPlaca(Placa placa);
}

class Pessoa{
    public:
        setNome(string nome);
        setIdade(int idade);
}

class Documento{
    public:
        void setDocumento(string documento1, string documento2);
}

class Placa{
    public:
        void setPlaca(string newPlaca);   
}

class CalcImposto{
    public:
        float calcIpva(Veiculo veiculo);
}
```

Aqui garantimos que cada classe tem apenas um único motivo para mudar. Caso haja alguma mudança ma documentaçao necessária para um veículo, ou na lógica para a criação de placas ou no cálculo do IPVA, apenas suas respectivas classes serão alteradas, fazendo que tal alteração não seja propagada para outras classe. A classe veiculo apenas armazena referências para estas outras classes. Desse modo, garantimos que o Princípio da Responsabilidade Única fosse respeitado.