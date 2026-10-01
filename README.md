# BrasilTravel

Sistema web de agência de viagens aéreas nacionais desenvolvido em **Java Spring Boot**, **Thymeleaf** e **PostgreSQL**.

![Java 21](https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.16-6DB33F?logo=springboot&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-view-005F0F?logo=thymeleaf&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-relacional-4169E1?logo=postgresql&logoColor=white)

---

## Sobre o projeto

O **BrasilTravel** é uma aplicação web desenvolvida como Fase 1 do projeto da disciplina de **Banco de Dados II**, com foco em banco de dados relacional.

O sistema representa uma agência de viagens aéreas nacionais. Ele permite o gerenciamento de destinos, aeroportos, companhias aéreas, voos e solicitações de viagem. Também possui relatórios gerenciais construídos a partir das informações cadastradas no banco de dados.

A modelagem completa, incluindo esquema conceitual, esquema lógico e dicionário de dados, está disponível no documento da disciplina.

---

## Funcionalidades

| Categoria | Funcionalidades |
| --- | --- |
| Destinos | Cadastro, consulta, atualização e remoção |
| Aeroportos | Cadastro, consulta, atualização e remoção |
| Companhias aéreas | Cadastro, consulta, atualização e remoção |
| Voos | Cadastro, consulta, atualização e remoção |
| Solicitações de viagem | Registro de solicitação, seleção de voos de ida e volta, acompanhamento e atualização de status |
| Relatórios | Viagens por companhia aérea, ocupação e disponibilidade de voos, demanda por destino |
| Usuários | Acesso de administrador e cliente |

---

## Tecnologias utilizadas

| Item | Tecnologia / Versão |
| --- | --- |
| Linguagem | Java JDK 21 |
| Framework | Spring Boot 3.5.16 |
| Interface | Thymeleaf |
| Banco de dados | PostgreSQL |
| Persistência | Spring Data JPA / Hibernate |
| Gerenciador de dependências | Maven Wrapper incluso no projeto |
| Servidor embutido | Apache Tomcat |

> Não é necessário instalar o Maven manualmente, pois o projeto já possui Maven Wrapper (`mvnw` e `mvnw.cmd`).

---

## Pré-requisitos

Antes de executar o projeto, é necessário ter instalado:

- Java JDK 21;
- PostgreSQL;
- Git;
- PowerShell, Prompt de Comando ou terminal equivalente.

Para conferir a versão do Java:

```powershell
java -version
```

O resultado deve indicar uma versão Java 21.

Exemplo:

```text
java version "21.x.x"
```

---

## Banco de dados local

O projeto utiliza PostgreSQL.

Antes de executar a aplicação, é necessário criar o banco de dados local chamado:

```text
brasiltravel
```

No PostgreSQL, execute:

```sql
CREATE DATABASE brasiltravel;
```

Também é possível criar pelo PowerShell:

```powershell
psql -U postgres -c "CREATE DATABASE brasiltravel;"
```

---

## Configuração do banco

As configurações principais estão no arquivo:

```text
src/main/resources/application.properties
```

Por padrão, o projeto utiliza:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/brasiltravel
spring.datasource.username=postgres
spring.datasource.password=udesc
```

Caso o PostgreSQL local use outra senha, há duas opções.

### Opção 1 — Alterar o arquivo `application.properties`

Altere a linha:

```properties
spring.datasource.password=udesc
```

para a senha do seu PostgreSQL local.

### Opção 2 — Usar variáveis de ambiente no PowerShell

Antes de rodar o projeto, execute:

```powershell
$env:SPRING_DATASOURCE_URL="jdbc:postgresql://localhost:5432/brasiltravel"
$env:SPRING_DATASOURCE_USERNAME="postgres"
$env:SPRING_DATASOURCE_PASSWORD="SUA_SENHA_DO_POSTGRES"
```

Depois rode a aplicação normalmente.

---

## Como executar o projeto

Abra o PowerShell na pasta onde o projeto foi salvo.

Exemplo:

```powershell
cd D:\Faculdade\BAN2\Trabalho01\BrasilTravel
```

Execute:

```powershell
.\mvnw.cmd spring-boot:run
```

Em Linux ou Mac:

```bash
./mvnw spring-boot:run
```

Aguarde aparecer no terminal uma mensagem semelhante a:

```text
Started BrasiltravelApplication
```

Depois acesse no navegador:

```text
http://localhost:8080
```

> O terminal precisa permanecer aberto enquanto o sistema estiver em uso. Se o terminal for fechado, a aplicação será encerrada.

---

## Usuários de acesso

| Perfil | E-mail | Senha |
| --- | --- | --- |
| Administrador | `admin@brasiltravel.com` | `Admin123` |
| Cliente | `cliente@brasiltravel.com` | `Cliente123` |

---

## Dados de demonstração

Por padrão, a aplicação recria dados comerciais de demonstração ao iniciar.

Essa configuração está definida em:

```properties
app.dados-exemplo.recriar=${APP_DADOS_EXEMPLO_RECRIAR:true}
```

Com isso, o sistema gera automaticamente:

- destinos brasileiros;
- aeroportos nacionais;
- companhias aéreas;
- voos futuros;
- solicitações históricas;
- dados para os relatórios.

Os voos futuros são gerados no período de:

```text
27/09/2026 a 27/10/2026
```

As solicitações históricas são distribuídas no período de:

```text
01/01/2026 a 27/09/2026
```

Os relatórios foram pensados para utilizar esse período histórico como base de demonstração.

---

## Relatórios disponíveis

O sistema possui três relatórios principais:

1. Viagens por companhia aérea;
2. Ocupação e disponibilidade de voos;
3. Demanda por destino.

Todos os relatórios possuem filtro obrigatório por status, com valor padrão:

```text
Finalizada
```

Também há filtros por período, companhia aérea e aeroportos de origem e destino, conforme o tipo de relatório.

---

## Regras principais do sistema

### Solicitações de viagem

O cliente pode criar uma solicitação selecionando:

- aeroporto de origem;
- destino;
- data de ida;
- voo de ida;
- data de volta;
- voo de volta;
- quantidade de passageiros.

A data de volta deve ser posterior à data de ida.

### Voos

A área administrativa permite cadastrar, editar, consultar e remover voos.

A listagem administrativa de voos exibe apenas voos futuros e utiliza paginação de 20 registros por página, preservando os filtros de aeroporto de origem e destino.

### Status das solicitações

As solicitações podem ser acompanhadas por status, permitindo controle administrativo do andamento de cada pedido.

---

## Backup e restauração do banco

Para gerar um dump do banco local pelo PowerShell:

```powershell
pg_dump -U postgres -d brasiltravel -F p -f brasiltravel_dump.sql
```

Para restaurar o dump em outro ambiente local:

```powershell
psql -U postgres -d brasiltravel -f brasiltravel_dump.sql
```

---

## Estrutura básica do projeto

```text
src/
 └── main/
     ├── java/
     │   └── br/com/brasiltravel/brasiltravel/
     │       ├── config/
     │       ├── controller/
     │       ├── model/
     │       ├── repository/
     │       └── service/
     └── resources/
         ├── templates/
         ├── static/
         └── application.properties
```

---

## Vídeo de demonstração

Link do vídeo de apresentação:

```text
https://youtu.be/ZvkA1D1h6o0
```

---

## Equipe

| Integrante |
| --- |
| Isadora Pimenta de Oliveira |
| Luís Felipe dos Anjos de Carvalho |

**Professora:** Rebeca Schroeder Freitas

**Disciplina:** Banco de Dados II

**Curso/Turma:** TADS 2026/02
