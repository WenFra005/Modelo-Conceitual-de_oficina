# **🛠 Modelo Conceitual e Relacional de Banco de Dados - Oficina Mecânica**  
Este projeto apresenta um **modelo conceitual e físico** de banco de dados relacional para um **sistema de controle e gerenciamento de ordens de serviço em uma oficina mecânica**. O modelo foi elaborado seguindo as 3 Formas Normais (3FN) para garantir consistência, integridade e performance dos dados.  

## **🚀 Objetivo do Projeto**  
O objetivo deste banco de dados é organizar e armazenar informações necessárias para o funcionamento de uma oficina mecânica, com as seguintes funcionalidades:  
1. Cadastro de clientes, veículos e ordens de serviço.  
2. Associação de ordens de serviço a equipes e mecânicos.  
3. Registro de peças e serviços realizados na ordem de serviço.  
4. Cálculo de valores de mão de obra e peças nas ordens de serviço.  
5. Acompanhamento de status e data de conclusão das ordens.  

---

## **🧩 Descrição das Entidades**  

1. **Cliente**  
   Representa os clientes que levam veículos à oficina.  
   - **Atributos principais**:  
      - `codigo_cliente` (INT, PK)  
      - `nome_cliente` (VARCHAR)  
      - `email_cliente` (VARCHAR)  
      - `telefone_cliente` (VARCHAR)  
      - `Endereco_codigo_Endereco` (FK)  
   - **Relacionamento**:  
      - **1:N** com `Veiculo` (um cliente possui vários veículos).  
      - **N:1** com `Endereco`.  

2. **Veiculo**  
   Representa os veículos trazidos pelos clientes.  
   - **Atributos principais**:  
      - `placa_veiculo` (VARCHAR, PK)  
      - `modelo_veiculo` (VARCHAR)  
      - `marca_veiculo` (VARCHAR)  
      - `ano_veiculo` (YEAR)  
      - `cliente_codigo_cliente` (FK)  
   - **Relacionamento**:  
      - **N:1** com `Cliente` (um veículo pertence a um cliente).  
      - **1:N** com `OrdemDeServico`.  

3. **Endereco**  
   Representa os endereços dos clientes e mecânicos.  
   - **Atributos principais**:  
      - `codigo_Endereco` (INT, PK)  
      - `logradouro` (VARCHAR)  
      - `numero` (INT)  
      - `cep` (VARCHAR)  
      - `bairro` (VARCHAR)  
   - **Relacionamento**:  
      - **1:N** com `Cliente`.  
      - **1:N** com `Mecanico`.  

4. **OrdemDeServico**  
   Representa as ordens de serviço emitidas pela oficina.  
   - **Atributos principais**:  
      - `NumeroOS` (INT, PK)  
      - `DataEmissao` (DATE)  
      - `DataConclusao` (DATE)  
      - `ValorTotal` (DECIMAL)  
      - `Status` (ENUM)  
      - `veiculo_placa_veiculo` (FK)  
      - `Equipe_codigo_Equipe` (FK)  
   - **Relacionamento**:  
      - **N:1** com `Veiculo`.  
      - **N:1** com `Equipe`.  
      - **1:N** com `ItensOS` (ordem pode conter vários itens).  

5. **Equipe**  
   Representa as equipes de mecânicos responsáveis pelas ordens de serviço.  
   - **Atributos principais**:  
      - `codigo_Equipe` (INT, PK)  
      - `nome_Equipe` (VARCHAR)  
   - **Relacionamento**:  
      - **1:N** com `OrdemDeServico`.  
      - **1:N** com `Mecanico`.  

6. **Mecanico**  
   Representa os mecânicos da oficina.  
   - **Atributos principais**:  
      - `Codigo_Mecanico` (INT, PK)  
      - `nome_Mecanico` (VARCHAR)  
      - `Especialidade_Mecanico` (VARCHAR)  
      - `Equipe_codigo_Equipe` (FK)  
      - `Endereco_codigo_Endereco` (FK)  
   - **Relacionamento**:  
      - **N:1** com `Equipe` (mecânico pertence a uma equipe).  
      - **N:1** com `Endereco`.  

7. **Servico**  
   Representa os serviços executados nos veículos.  
   - **Atributos principais**:  
      - `Codigo_Servico` (INT, PK)  
      - `Descricao` (VARCHAR)  
      - `Valor_Mao_Obra` (DECIMAL)  
   - **Relacionamento**:  
      - **1:N** com `ItensOS`.  

8. **Peca**  
   Representa as peças utilizadas nos serviços.  
   - **Atributos principais**:  
      - `Codigo_Peca` (INT, PK)  
      - `Descricao` (VARCHAR)  
      - `nome_Peca` (VARCHAR)  
      - `valor` (DECIMAL)  
   - **Relacionamento**:  
      - **1:N** com `ItensOS`.  

9. **ItensOS**  
   Representa os itens (serviços e peças) incluídos em cada ordem de serviço.  
   - **Atributos principais**:  
      - `Numero_Item_OS` (INT, PK)  
      - `quantidade` (INT)  
      - `subtotal` (DECIMAL)  
      - `OrdemDeServico_NumeroOS` (FK)  
      - `Peca_Codigo_Peca` (FK)  
      - `Servico_Codigo_Servico` (FK)  
   - **Relacionamento**:  
      - **N:1** com `OrdemDeServico`.  
      - **N:1** com `Servico`.  
      - **N:1** com `Peca`.  

---

## **🔗 Relacionamentos entre as Entidades**  

| **Relacionamento**               | **Descrição**                                        |  
|:---------------------------------|:----------------------------------------------------|  
| `Cliente` ↔ `Endereco`           | Um cliente possui um endereço (N:1).                |  
| `Cliente` ↔ `Veiculo`            | Um cliente possui vários veículos (1:N).            |  
| `Veiculo` ↔ `OrdemDeServico`     | Um veículo pode ter várias ordens de serviço (1:N). |  
| `OrdemDeServico` ↔ `ItensOS`     | Uma ordem pode conter vários itens (1:N).           |  
| `Equipe` ↔ `OrdemDeServico`      | Uma equipe realiza várias ordens de serviço (1:N).  |  
| `Equipe` ↔ `Mecanico`            | Uma equipe possui vários mecânicos (1:N).           |  
| `Endereco` ↔ `Mecanico`          | Um mecânico possui um endereço (N:1).               |  
| `Servico` ↔ `ItensOS`            | Um serviço pode ser associado a vários itens (1:N). |  
| `Peca` ↔ `ItensOS`               | Uma peça pode ser associada a vários itens (1:N).   |  

---

## **📈 Diagrama do Banco de Dados (Modelo ER)**  
O diagrama abaixo apresenta o modelo atualizado, com as entidades, relacionamentos e suas chaves primárias/estrangeiras:  

<div align="center">  
   <img src="coloque_o_link_aqui" alt="Diagrama ER Oficina Mecânica" width="800"/>  
</div>  

---

## **💻 Tecnologias Utilizadas**  
- **MySQL** 🐬: Sistema de gerenciamento de banco de dados relacional.  
- **MySQL Workbench** 🛠: Ferramenta de modelagem e criação do banco de dados.  
- **ChatGPT** 🤖: Auxílio na documentação e descrição do diagrama.  

---

Caso tenha dúvidas ou sugestões, sinta-se à vontade para abrir uma *issue* ou entrar em contato!
