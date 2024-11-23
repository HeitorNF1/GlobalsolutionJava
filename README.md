# Globalsolution Java

### Integrantes: Gustavo Akio RM:550241;Heitor Farias RM:551539;Yuri Zacarioto RM:550952

Este é um projeto da faculdade, no qual deveríamos recriar o sistema de API de uma empresa de fornecimento de energia limpa

Professor Responsável: [Fellipe Souto](https://github.com/fllsouto)

O desafio: [Global Solution 2SEM24](https://github.com/fllsouto/global_solution_fiap_esp_wx_s2_2024)


## Dependências do Projeto

1. **spring-boot-starter-web**
   - **Group ID:** `org.springframework.boot`
   - **Artifact ID:** `spring-boot-starter-web`
   - **Descrição:** Starter para construir aplicativos web, incluindo aplicações RESTful, usando Spring MVC. Usa Tomcat como o contêiner de servlet padrão.

2. **spring-boot-starter-test**
   - **Group ID:** `org.springframework.boot`
   - **Artifact ID:** `spring-boot-starter-test`
   - **Descrição:** Starter para testes em Spring Boot, que inclui bibliotecas como JUnit, Hamcrest e Mockito.
   - **Escopo:** `test`

3. **spring-boot-starter-data-jpa**
   - **Group ID:** `org.springframework.boot`
   - **Artifact ID:** `spring-boot-starter-data-jpa`
   - **Descrição:** Starter para integração do Spring Data JPA, que facilita o acesso e a persistência de dados usando JPA (Java Persistence API).

4. **spring-boot-devtools**
   - **Group ID:** `org.springframework.boot`
   - **Artifact ID:** `spring-boot-devtools`
   - **Descrição:** Ferramentas de desenvolvimento do Spring Boot que melhoram a experiência de desenvolvimento, incluindo reinicialização automática e depuração.
   - **Escopo:** `runtime`
   - **Opcional:** `true`

5. **mysql-connector-j**
   - **Group ID:** `com.mysql`
   - **Artifact ID:** `mysql-connector-j`
   - **Descrição:** Conector JDBC para MySQL, utilizado para integrar o banco de dados MySQL com a aplicação Spring Boot.
   - **Escopo:** `runtime`
  
## Endpoints da API

### Clientes

- **GET /clientes**  
  Pega todos os clientes disponíveis.
  
- **GET /clientes/{id}**  
  Pega um cliente específico pelo ID.
  
- **DELETE /clientes/{id}**  
  Deleta o cliente com o ID especificado.
  
- **POST /clientes**  
  Adiciona um novo cliente.  
  **Parâmetros**:
  - `name` (String): Nome do cliente
  - `endereco` (String): Endereço do cliente
  - `cpfCnpj` (String): CPF ou CNPJ do cliente
  - `tipoCliente` (String): Tipo do cliente
  - `cep` (String): CEP do cliente

---

### Instalações

- **GET /instalacoes**  
  Pega todas as instalações disponíveis.
  
- **GET /instalacoes/{id}**  
  Pega uma instalação específica pelo ID.
  
- **DELETE /instalacoes/{id}**  
  Deleta a instalação com o ID especificado.
  
- **POST /instalacoes**  
  Adiciona uma nova instalação.  
  **Parâmetros**:
  - `endereco` (String): Endereço da instalação
  - `cep` (String): CEP da instalação

---

### Contratos

- **GET /contratos**  
  Pega todos os contratos disponíveis.
  
- **GET /contratos/{id}**  
  Pega um contrato específico pelo ID.
  
- **DELETE /contratos/{id}**  
  Deleta o contrato com o ID especificado.
  
- **POST /contratos**  
  Adiciona um novo contrato.  
  **Parâmetros**:
  - `codCliente` (UUID): Código do cliente
  - `numeroInstalacao` (UUID): Número da instalação
  - `dataInicio` (Instant): Data de início do contrato
  - `duracaoContrato` (int): Duração do contrato em meses

---

### Consumo

- **GET /consumo**  
  Pega todos os dados de consumo disponíveis.
  
- **GET /consumo/{id}**  
  Pega um dado de consumo específico pelo ID.
  
- **POST /consumo**  
  Adiciona um novo dado de consumo.  
  **Parâmetros**:
  - `numeroInstalacao` (UUID): Número da instalação
  - `consumoKWh` (Double): Consumo em kWh
  - `medicaoTimestamp` (Instant): Data e hora da medição

---

### Produção

- **POST /producao**  
  Adiciona um novo dado de produção.  
  **Parâmetros**:
  - `numeroInstalacao` (UUID): Número da instalação
  - `consumoKWh` (Double): Consumo em kWh
  - `medicaoTimestamp` (Instant): Data e hora da medição

## Referências

Esta aba faremos um pouco diferente, pois iremos explicar o motivo de cada referência, para não ficar nenhuma dúvida do conteúdo entregue

[Fernanda Kipper](https://www.youtube.com/watch?v=lUVureR5GqI&t=1700s) -> Utilizamos o projeto do vídeo como base para o início do nosso, pegando algumas dicas e usando a modelagem dela como referência

[Ordenação](https://www.coffeeandtips.com/post/usando-comparator-comparing-para-ordenar-java-stream) -> Para devolver os valores dentro de uma ordenação pegamos como referência este site que nos ensinou como usar o Comparator.comparing

[ChatGPT](chatgpt.com) -> Utilizamos o chat para a melhora na otimização do código, pesquisa sobre coisas específicas que não achamos no google, mas principalmente para debugar o código

## Trechos que pedimos auxílio do chat

### Código

```Java
List<RegistroConsumo> findByNumeroInstalacaoAndMedicaoTimestampBetween(Instalacao numeroInstalacao, Instant inicio, Instant fim);
```
```Java
//Gerando o mes e o ano atual
        YearMonth mesAtual = YearMonth.now();
        Instant inicioMes = mesAtual.atDay(1).atStartOfDay(ZoneId.systemDefault()).toInstant();
        Instant fimMes = mesAtual.atEndOfMonth().atTime(23, 59, 59).atZone(ZoneId.systemDefault()).toInstant();

```

### Debug

No momento da criação do findByNumeroInstalacaoAndMedicaoTimestampBetween, nós travamos, pois estávamos passando Instalacao como UUID e ele estava quebrando.

O PC do Heitor deu um erro numa classe que nem tem no código e após o auxílio do chat ele testou em outro pc e o código rodou normalmente (posteriormente descobrimos que era por conta da versão do java instalada dentro do pc dele)

E quando criamos as tabelas pelo spring na primeira vez, havíamos passado os valores errados nos atributos das entidades e o chat traduziu este erro para nós (o erro era que basicamente tinha valores indo como nulo)

### Pesquisa

No canal da Fernanda Kipper, nós vimos ela dizendo sobre a utilização do DTO como uma boa prática, após isso, fomos pesquisar os motivos e o chat nos disse que era para não ter a maniupalçao direta dos nossas entidades do lado de fora do nosso código, então montamos os DTOs sem nenhum valor obrigatório para podermos retornar com o mesmo que veio (exceto no de consumo que criamos um específico para devolver do jeito que foi solicitado)

Utilização do responseEntity para colocar o código 201 nos posts quando fossem criados 

E por fim no segundo trecho de código acima, usamos o chat para ele nos ajduar como poderíamos fazer a comparação do melhor jeito possível, já que passávamos um valor em timeStamp e de primeiro momento estavamos usando LocalDateTime, foi nesse momento que ele disse para usarmos Instant que era mais simples de manipular com valores que estavam vindo deste modo, porque não sabiamos o fuso horário correto.


## Pontos de melhora

Por queremos reduzir o uso do GPT, as funções GET ALL retornam uma lista de clientes sem estar dentro do DTO, então poderíamos mudar isso posteriormente

Íamos fazer uma interface front-end assim como vimos no projeto da Fernanda Kipper, mas por conta do tempo e que travamos em certas partes dos códigos, decidimos que era melhor apenas entregar o back-end

Por conta do retorno enorme de erros dentro do SpringBoot, tivemos que recorrer ao chat mais vezes do que queríamos nesse sentido

Poucas validações fora do contexto das regras de negócios




# Obs: *APENAS UTILIZAMOS O GPT PELA LIBERAÇÃO DO ALLEN DENTRO DESTES PEQUENOS CONTEXTOS QUE LEVAMOS PARA ELE* 

