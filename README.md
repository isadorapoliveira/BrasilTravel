# Brasil Travel

Sistema de reservas de viagens (voos e hotéis) com banco de dados relacional normalizado.

## Sobre a aplicação

O sistema simula uma agência de viagens online, onde o usuário escolhe um destino, seleciona um voo (por companhia aérea) e um hotel (por tipo de quarto), e monta uma solicitação de orçamento ou reserva. Administradores acompanham e tratam essas solicitações.

## Modelo conceitual

7 entidades e 3 tabelas associativas, girando em torno da solicitação de viagem (voo + hotel).

```mermaid
erDiagram
  USUARIOS ||--o{ SOLICITACAO_VIAGEM : faz
  DESTINOS ||--o{ SOLICITACAO_VIAGEM : refere_principal
  DESTINOS ||--o{ AEROPORTOS : possui
  DESTINOS ||--o{ HOTEIS : possui
  COMPANHIAS_AEREAS ||--o{ VOOS : opera
  AEROPORTOS ||--o{ VOOS : origem
  AEROPORTOS ||--o{ VOOS : destino
  SOLICITACAO_VIAGEM ||--o{ SOLICITACAO_VOO : inclui
  VOOS ||--o{ SOLICITACAO_VOO : reservado
  SOLICITACAO_VIAGEM ||--o{ SOLICITACAO_HOTEL : inclui
  HOTEIS ||--o{ SOLICITACAO_HOTEL : reservado
  HOTEIS ||--o{ TIPOS_QUARTO : possui

  USUARIOS {
    int id_usuario PK
    string nome
    string email
    string cpf
    string telefone
    string tipo_usuario
    datetime data_cadastro
  }
  
  DESTINOS {
    int id_destino PK
    string cidade
    string estado
    string pais
    string descricao
    string imagem_url
    boolean ativo
  }
  
  AEROPORTOS {
    int id_aeroporto PK
    int id_destino FK
    string codigo_iata
    string nome
    string endereco
  }
  
  COMPANHIAS_AEREAS {
    int id_companhia PK
    string nome
    string codigo_iata
    string site
    string telefone
  }
  
  VOOS {
    int id_voo PK
    int id_companhia FK
    int id_aeroporto_origem FK
    int id_aeroporto_destino FK
    string numero_voo
    datetime data_hora_partida
    datetime data_hora_chegada
    decimal preco_base
    int vagas_disponiveis
    string classe
    boolean ativo
  }
  
  HOTEIS {
    int id_hotel PK
    int id_destino FK
    string nome
    int categoria_estrelas
    string endereco
    string descricao
    string imagem_url
    boolean ativo
  }
  
  TIPOS_QUARTO {
    int id_tipo_quarto PK
    int id_hotel FK
    string nome
    string descricao
    decimal preco_diaria
    int vagas_disponiveis
    int capacidade_pessoas
    boolean ativo
  }
  
  SOLICITACAO_VIAGEM {
    int id_solicitacao PK
    int id_usuario FK
    int id_destino_principal FK
    datetime data_criacao
    date data_ida
    date data_volta
    string status_solicitacao
    decimal valor_total
    string observacoes
  }
  
  SOLICITACAO_VOO {
    int id_solicitacao_voo PK
    int id_solicitacao FK
    int id_voo FK
    int qtd_passageiros
    int ordem_voo
    decimal preco_unitario
    decimal preco_total
  }
  
  SOLICITACAO_HOTEL {
    int id_solicitacao_hotel PK
    int id_solicitacao FK
    int id_tipo_quarto FK
    date data_checkin
    date data_checkout
    int qtd_quartos
    int qtd_hospedes
    decimal preco_diaria
    decimal preco_total
  }
```
