# Introdução-a-Programação-com-Blockchain-e-Ethereum-Smart-Contracts

Desenvolver um token baseado na blockchain Ethereum, seguindo o padrão ERC20, é uma excelente maneira de explorar os fundamentos da programação com blockchain e smart contracts. Abaixo está a estrutura do projeto e exemplos de código para cada parte essencial.

### Estrutura do Projeto

1. **Contrato Inteligente (Smart Contract)**
   - **TokenERC20.sol**: O contrato inteligente que implementa as funcionalidades do token ERC20.

2. **Configuração e Deployment**
   - **Truffle Framework**: Utilizado para compilar, testar e deploy dos contratos na blockchain Ethereum.
   - **Ganache**: Uma blockchain Ethereum local para desenvolvimento e testes.
   
3. **Frontend Simples (Opcional)**
   - Uma interface básica para interagir com o contrato inteligente através de transações Ethereum.

### Exemplo de Código

#### TokenERC20.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TokenERC20 {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals,
        uint256 _initialSupply
    ) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply * 10**uint256(_decimals);
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address _to, uint256 _value) external returns (bool success) {
        require(_to != address(0), "Transfer to the zero address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) external returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external returns (bool success) {
        require(_to != address(0), "Transfer to the zero address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Insufficient allowance");
        
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        
        emit Transfer(_from, _to, _value);
        return true;
    }
}
```

#### Explicação do Contrato Inteligente (TokenERC20.sol)

- **Variáveis de Estado**: `name`, `symbol`, `decimals`, `totalSupply` definem os detalhes do token e seu fornecimento total.
- **Mapeamentos**: `balanceOf` rastreia o saldo de cada endereço; `allowance` rastreia permissões para transferir tokens.
- **Eventos**: `Transfer` e `Approval` são emitidos para notificar transferências e aprovações.
- **Construtor**: Inicializa o contrato com o nome, símbolo, número de casas decimais e fornecimento inicial, distribuindo todos os tokens para o criador inicialmente.

### Considerações Finais

Este projeto aborda os conceitos fundamentais de desenvolvimento de tokens na blockchain Ethereum usando o padrão ERC20. O código apresentado implementa as funcionalidades básicas de um token, incluindo transferências de saldo, aprovações e controle de permissões. Ao explorar este projeto, teremos uma compreensão sólida de como os contratos inteligentes podem ser utilizados para criar ativos digitais interoperáveis e negociáveis na blockchain Ethereum.
