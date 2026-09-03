# Brasil Travel

Sistema de reservas de viagens (voos e hotéis) com banco de dados relacional normalizado.

## Sobre a aplicação

O sistema simula uma agência de viagens online, onde o usuário escolhe um destino, seleciona um voo (por companhia aérea) e um hotel (por tipo de quarto), e monta uma solicitação de orçamento ou reserva. Administradores acompanham e tratam essas solicitações.

## Modelo conceitual

7 entidades e 3 tabelas associativas, girando em torno da solicitação de viagem (voo + hotel).

```mermaid
erDiagram
  USUARIOS ||--o{ SOLICITACOES : faz
  DESTINOS ||--o{ SOLICITACOES : refere
  DESTINOS ||--o{ AEROPORTOS : possui
  DESTINOS ||--o{ HOTEIS : possui
  COMPANHIAS_AEREAS ||--o{ VOOS : opera
  AEROPORTOS ||--o{ VOOS : origem
  AEROPORTOS ||--o{ VOOS : chegada
  HOTEIS ||--o{ TIPOS_QUARTO : possui
  SOLICITACOES ||--o{ SOLICITACAO_VOO : inclui
  VOOS ||--o{ SOLICITACAO_VOO : reservado
  SOLICITACOES ||--o{ SOLICITACAO_HOTEL : inclui
  TIPOS_QUARTO ||--o{ SOLICITACAO_HOTEL : reservado

  USUARIOS {
    int id_usuario PK
    string nome
    string email
    string cpf
    string tipo_usuario
  }
  DESTINOS {
    int id_destino PK
    string cidade
    string estado
    string descricao
  }
  AEROPORTOS {
    int id_aeroporto PK
    int id_destino FK
    string codigo_iata
    string nome
  }
  COMPANHIAS_AEREAS {
    int id_companhia PK
    string nome
    string codigo_iata
  }
  VOOS {
    int id_voo PK
    int id_companhia FK
    int id_aeroporto_origem FK
    int id_aeroporto_destino FK
    string numero_voo
    datetime data_hora_partida
    decimal preco_base
  }
  HOTEIS {
    int id_hotel PK
    int id_destino FK
    string nome
    int categoria_estrelas
  }
  TIPOS_QUARTO {
    int id_tipo_quarto PK
    int id_hotel FK
    string nome
    decimal preco_diaria
  }
  SOLICITACOES {
    int id_solicitacao PK
    int id_usuario FK
    int id_destino FK
    string categoria
    string tipo_solicitacao
    date data_ida
    date data_volta
    string status_solicitacao
  }
  SOLICITACAO_VOO {
    int id_solicitacao_voo PK
    int id_solicitacao FK
    int id_voo FK
    string tipo_trecho
    int qtd_passageiros
  }
  SOLICITACAO_HOTEL {
    int id_solicitacao_hotel PK
    int id_solicitacao FK
    int id_tipo_quarto FK
    date data_checkin
    date data_checkout
  }
```
