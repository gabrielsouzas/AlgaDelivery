# AlgaDelivery - Sistema de Microserviços de Delivery

---

Este projeto, **AlgaDelivery**, é um sistema de delivery desenvolvido como parte do curso "Mergulho Microserviços Spring" da AlgaWorks. Ele visa demonstrar a arquitetura de microserviços utilizando tecnologias modernas do ecossistema Java e Spring.

## Visão Geral do Projeto

O AlgaDelivery é uma rede de delivery que, após uma análise de domínio com **Domain-Driven Design (DDD)**, foi decomposta em microserviços. O foco inicial do desenvolvimento está nos subdomínios de **Entregas** e **Entregadores**, sendo "Entregas" o core do negócio e "Entregadores" um subdomínio de suporte para "Entregas". O subdomínio de "Suporte ao Cliente" foi identificado como genérico e, por ora, não será implementado internamente, podendo ser integrado com soluções de terceiros no futuro.

---

## Estrutura do Projeto

O projeto é composto por dois microserviços principais:

- **Microserviço de Entregas**: Lida com todas as funcionalidades relacionadas ao ciclo de vida das entregas, desde a criação até a finalização.
- **Microserviço de Entregadores**: Gerencia informações e funcionalidades dos entregadores, como cadastro, disponibilidade e atribuição de entregas.

---

## Tecnologias Utilizadas

- **Java 21**
- **Spring Boot**
- **Maven**
- **Spring Web**: Para construção de APIs RESTful.
- **Spring Data JPA**: Para persistência de dados.
- **PostgreSQL**: Banco de dados relacional.
- **Lombok**: Para simplificar o código Java (geração automática de getters, setters, construtores, etc.).
- **Validation**: Para validação de dados de entrada.

---

## Como Gerar o Projeto com Spring Initializr

Você pode gerar a estrutura inicial do seu projeto Spring Boot de duas maneiras: através do site do Spring Initializr ou diretamente no VS Code.

### Gerando via Site do Spring Initializr

1.  Acesse o site: [https://start.spring.io/](https://start.spring.io/)
2.  Preencha os campos conforme abaixo:
    - **Project**: Maven Project
    - **Language**: Java
    - **Spring Boot**: Escolha a versão mais recente estável (ex: 3.x.x)
    - **Project Metadata**:
      - **Group**: `com.algaworks.algadelivery` (ou um nome de grupo de sua preferência)
      - **Artifact**: `delivery.tracking` (para o microserviço de Entregas) ou `courier.management` (para o microserviço de Entregadores)
      - **Name**: `delivery.tracking` ou `courier.management`
      - **Description**: `API de entregas para o AlgaDelivery` ou `API de entregadores para o AlgaDelivery`
      - **Package Name**: `com.algaworks.algadelivery.entregas` ou `com.algaworks.algadelivery.entregadores`
    - **Packaging**: Jar
    - **Java**: 21
3.  **Adicione as dependências**:
    - `Spring Web`
    - `Spring Data JPA`
    - `PostgreSQL Driver`
    - `Lombok`
    - `Validation`
4.  Clique no botão "Generate" para baixar o projeto compactado (ZIP).

### Gerando via VS Code

1.  Abra o VS Code.
2.  Certifique-se de ter a extensão "Spring Boot Extension Pack" instalada.
3.  Pressione `Ctrl+Shift+P` (ou `Cmd+Shift+P` no macOS) para abrir a Paleta de Comandos.
4.  Digite `Spring Initializr` e selecione "Spring Initializr: Generate a Maven Project".
5.  Siga as instruções na tela, selecionando as opções equivalentes às do site (Maven, Java, Spring Boot versão, Group, Artifact, Java 21) e adicionando as mesmas dependências (`Spring Web`, `Spring Data JPA`, `PostgreSQL Driver`, `Lombok`, `Validation`).
6.  Escolha o diretório onde deseja salvar o projeto.

---

## Como Importar o Projeto no VS Code (se gerado pelo site)

Se você gerou o projeto Spring Boot pelo site do Spring Initializr, siga estes passos para importá-lo no VS Code:

1.  Descompacte o arquivo ZIP que você baixou em um diretório de sua preferência.
2.  Abra o VS Code.
3.  Vá em `File > Open Folder...` (ou `Arquivo > Abrir Pasta...`).
4.  Navegue até o diretório onde você descompactou o projeto (a pasta raiz do projeto, que contém o arquivo `pom.xml`) e clique em "Select Folder" (ou "Selecionar Pasta").
5.  O VS Code, com as extensões Spring Boot instaladas, detectará automaticamente que é um projeto Maven/Spring Boot e configurará o ambiente. Aguarde a resolução das dependências.

---

## Análise de Domínio com Domain-Driven Design (DDD)

A fase de análise de domínio com DDD foi crucial para a identificação e decomposição dos microserviços. O DDD nos ajuda a construir software que reflete a complexidade e as nuances do negócio.

### Etapas da Análise com DDD para Decomposição de Microserviços

1.  **Entendimento do Domínio e Contexto Geral**:

    - Começamos entendendo o negócio "AlgaDelivery" como um todo: o que é uma entrega, quem são os participantes (clientes, entregadores, restaurantes), quais são os processos (fazer pedido, atribuir entrega, acompanhar entrega, finalizar entrega, suporte).
    - O objetivo principal é entregar algo do ponto A ao ponto B.

2.  **Identificação de Subdomínios e Contextos Delimitados (Bounded Contexts)**:

    - Com base no entendimento do domínio, começamos a identificar as grandes áreas de responsabilidade e conhecimento dentro do negócio. Essas áreas são os **Subdomínios**.
    - Para o AlgaDelivery, os subdomínios iniciais identificados foram:
      - **Entregas**: O coração do negócio, lidando com o fluxo principal de uma entrega.
      - **Entregadores**: Gerencia a força de trabalho que realiza as entregas.
      - **Suporte ao Cliente**: Lida com o atendimento e resolução de problemas para clientes.
    - Cada subdomínio se torna um **Contexto Delimitado (Bounded Context)**, onde termos e conceitos têm um significado específico e consistente dentro de suas fronteiras. Isso evita ambiguidades e conflitos de modelos entre diferentes partes do sistema. Por exemplo, um "Pedido" no contexto de "Entregas" pode ter atributos diferentes de um "Pedido" no contexto de "Vendas" (que não foi foco inicial).

3.  **Classificação dos Subdomínios**:

    - Após identificar os subdomínios, classificamos cada um deles quanto à sua importância estratégica para o negócio:
      - **Core Domain (Domínio Core)**: Representa o diferencial competitivo da empresa, onde está o valor agregado principal. É aqui que devemos investir mais esforço e inteligência.
        - No AlgaDelivery, **Entregas** foi classificado como **Core Domain**. A forma como as entregas são gerenciadas, otimizadas e executadas é o que define o sucesso do serviço de delivery.
      - **Supporting Domain (Domínio de Suporte)**: Subdomínios que são específicos da empresa, mas não representam seu diferencial competitivo principal. Eles dão suporte ao Core Domain.
        - **Entregadores** foi classificado como **Supporting Domain**. Embora essencial para a execução das entregas, o gerenciamento de entregadores em si (cadastro, localização) é um suporte para o core, não a essência da entrega.
      - **Generic Domain (Domínio Genérico)**: Subdomínios que são comuns a muitas empresas e podem ser facilmente adquiridos ou terceirizados (ex: autenticação, notificações, pagamentos). Não há valor em construí-los internamente do zero se já existem boas soluções no mercado.
        - **Suporte ao Cliente** foi classificado como **Generic Domain**. A funcionalidade de suporte ao cliente é genérica e pode ser atendida por ferramentas de CRM (Customer Relationship Management) de terceiros, sem a necessidade de um desenvolvimento interno complexo no início do projeto.

4.  **Decomposição em Microserviços**:
    - A classificação dos subdomínios nos guiou na decomposição em microserviços. Cada **Bounded Context** geralmente se traduz em um ou mais microserviços.
    - **Core Domain** e **Supporting Domain** são candidatos ideais para serem implementados como microserviços internos, pois contêm lógica de negócio específica.
    - **Generic Domain** pode ser um microserviço separado (se implementado) ou, preferencialmente, integrado com soluções externas.
    - Neste projeto, optamos por desenvolver microserviços para os subdomínios **Entregas** e **Entregadores**, refletindo a importância estratégica e de suporte ao Core Domain.

---

## Design Tático e Domain Model

Com a decomposição em microserviços definida, a próxima fase é o **Design Tático** dentro de cada **Bounded Context**. Nesta etapa, aprofundamos nos detalhes da implementação do modelo de domínio.

Para cada microserviço (Entregas e Entregadores), definiremos os seguintes blocos construtivos do DDD:

- **Entidades (Entities)**: Objetos que possuem uma identidade e um ciclo de vida contínuo. Ex: `Clientes`, `Pedidos`, `Produtos`.
- **Objetos de Valor (Value Objects)**: Objetos que descrevem algo, mas não possuem uma identidade única. São imutáveis e comparados por seus atributos. Ex: `Endereco`, `Cpf`.
- **Agregados (Aggregates)**: Um cluster de Entidades e Objetos de Valor que são tratados como uma única unidade para fins de persistência e consistência transacional. Há uma **Raiz do Agregado (Aggregate Root)** que é a única entidade externa que pode ser referenciada. Ex: O Agregado `Entrega` pode ter `ItemEntrega` como Entidade interna e `Endereco` como Objeto de Valor. A `Entrega` seria a Raiz do Agregado.
- **Serviços de Domínio (Domain Services)**: Operações que não se encaixam naturalmente em uma Entidade ou Objeto de Valor, mas que expressam uma lógica de negócio importante. Ex: Um serviço para calcular a rota mais eficiente para uma entrega.
- **Repositórios (Repositories)**: Abstrações para a persistência de Agregados. Permitem que o domínio "recupere" e "persista" Agregados sem se preocupar com os detalhes técnicos do banco de dados.

Nesta fase, desenhamos as classes e interfaces que compõem o modelo de domínio de cada microserviço, garantindo que eles reflitam as regras de negócio e a Ubiquitous Language (Linguagem Ubíqua) estabelecida na fase de análise.

---

## Escolha da Arquitetura: Camadas (Layered Architecture)

Para cada microserviço do AlgaDelivery, adotamos a **Arquitetura em Camadas (Layered Architecture)**, também conhecida como arquitetura N-camadas. Essa escolha é fundamental para organizar o código de forma coesa, modular e com responsabilidades bem definidas, promovendo a manutenibilidade, testabilidade e escalabilidade.

A arquitetura em camadas organiza o software em camadas horizontais, onde cada camada tem uma responsabilidade específica e depende apenas da camada abaixo dela (ou não depende de nenhuma, como a camada de domínio). As camadas típicas que utilizaremos em cada microserviço são:

1.  **Camada de Apresentação/API (Presentation Layer)**:

    - **Responsabilidade**: Lida com a interação externa do sistema, recebendo requisições e enviando respostas. É a porta de entrada para o microserviço.
    - **Tecnologias**: Spring Web (Controladores REST).
    - **Exemplo**: Controladores REST que recebem requisições HTTP para criar uma entrega, buscar entregadores, etc.

2.  **Camada de Aplicação (Application Layer)**:

    - **Responsabilidade**: Orquestra as operações de negócio, coordenando o fluxo de dados entre a camada de apresentação e a camada de domínio. Não contém regras de negócio complexas, mas sim a sequência de passos para executar uma funcionalidade. É aqui que transações são gerenciadas.
    - **Exemplo**: Serviços de aplicação que chamam serviços de domínio para executar uma operação, como `CriarEntregaService` ou `AtribuirEntregaService`.

3.  **Camada de Domínio (Domain Layer)**:

    - **Responsabilidade**: O coração da aplicação, contendo toda a **lógica de negócio (Domain Model)**. É independente de frameworks e tecnologias de infraestrutura. Aqui residem as **Entidades**, **Objetos de Valor**, **Agregados** e **Serviços de Domínio**.
    - **Exemplo**: Classes como `Entrega`, `Entregador`, `Endereco`, e métodos que definem o comportamento do negócio, como `Entrega.confirmar()`, `Entregador.ficarDisponivel()`.

4.  **Camada de Infraestrutura (Infrastructure Layer)**:
    - **Responsabilidade**: Lida com os detalhes técnicos de persistência de dados, comunicação com outros sistemas, envio de e-mails, etc. Implementa as interfaces definidas nas camadas superiores (principalmente repositórios).
    - **Tecnologias**: Spring Data JPA para persistência no PostgreSQL.
    - **Exemplo**: Implementações de `Repositorios` que interagem com o banco de dados (ex: `EntregaRepositoryImpl` que usa `JpaRepository`), classes de configuração de banco de dados.

### Benefícios da Arquitetura em Camadas:

- **Separação de Preocupações**: Cada camada tem uma responsabilidade clara, tornando o código mais fácil de entender e manter.
- **Testabilidade**: Permite testar cada camada isoladamente, especialmente a camada de domínio, que é a mais importante.
- **Flexibilidade**: Alterações em uma camada (ex: mudar o banco de dados na camada de infraestrutura) têm impacto limitado nas outras camadas.
- **Reusabilidade**: Lógicas de negócio e componentes de infraestrutura podem ser reutilizados dentro do mesmo microserviço ou, em alguns casos, compartilhados (com cuidado) entre microserviços.
- **Organização**: Fornece uma estrutura bem definida para organizar o código-fonte, facilitando o desenvolvimento e a colaboração em equipe.

Essa abordagem nos permite construir microserviços robustos, com um domínio bem modelado e uma clara separação entre a lógica de negócio e os detalhes técnicos de implementação.

---

### Aplicação do Princípio de Inversão de Dependência (DIP)

Dentro da nossa **Arquitetura em Camadas**, o **Princípio de Inversão de Dependência (DIP)** é crucial para garantir a flexibilidade, testabilidade e manutenibilidade dos nossos microserviços. O DIP, parte dos princípios SOLID, afirma que:

1.  Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.
2.  Abstrações não devem depender de detalhes. Detalhes (implementações concretas) devem depender de abstrações.

Em termos práticos, isso significa que as camadas superiores (como a Camada de Aplicação e a Camada de Domínio) não devem ter dependências diretas de implementações concretas de camadas inferiores (como a Camada de Infraestrutura). Em vez disso, elas dependerão de **abstrações (interfaces)**, e as implementações dessas interfaces serão fornecidas pelas camadas de baixo nível.

**Como aplicamos o DIP em cada microserviço:**

- **Camada de Domínio e Aplicação dependem de Interfaces (Abstrações):**

  - Nossa **Camada de Domínio** define as **interfaces dos repositórios** (ex: `EntregaRepository`, `EntregadorRepository`). Essas interfaces especificam os contratos para persistir e recuperar os agregados.
  - A **Camada de Aplicação** usa essas interfaces de repositórios para interagir com os dados, sem saber como esses dados são realmente persistidos (se é PostgreSQL, MongoDB, etc.).
  - Da mesma forma, quaisquer **Serviços de Domínio** ou lógicas de negócio que precisem interagir com "fora" do domínio (ex: enviar uma notificação) fariam isso através de interfaces definidas no próprio domínio ou na camada de aplicação.

- **Camada de Infraestrutura implementa as Abstrações (Detalhes):**
  - A **Camada de Infraestrutura** é responsável por fornecer as **implementações concretas** das interfaces definidas nas camadas superiores.
  - Por exemplo, `EntregaRepositoryImpl` (ou o que o Spring Data JPA gera automaticamente) implementará a interface `EntregaRepository`, utilizando o PostgreSQL como banco de dados.
  - Essa separação garante que, se precisarmos mudar o banco de dados de PostgreSQL para outro, apenas a camada de infraestrutura será afetada, e as camadas de domínio e aplicação permanecerão inalteradas.

**Benefícios da Aplicação do DIP:**

- **Acoplamento Fraco**: As camadas ficam menos acopladas umas às outras, pois dependem de interfaces e não de implementações concretas.
- **Facilidade de Teste**: Podemos facilmente substituir as implementações reais por _mocks_ ou _stubs_ nas interfaces durante os testes unitários e de integração, isolando as camadas e tornando os testes mais rápidos e confiáveis.
- **Flexibilidade e Manutenção**: Facilita a substituição de tecnologias de infraestrutura (ex: trocar de banco de dados, ou de um sistema de mensageria) sem impactar a lógica de negócio principal.
- **Clareza e Organização**: Promove uma estrutura de código mais limpa e compreensível, onde as responsabilidades são claramente segregadas.

Ao aplicar o DIP, invertemos o controle das dependências: em vez de módulos de alto nível controlarem seus módulos de baixo nível, o controle é "invertido" e as dependências são injetadas (geralmente por um _framework_ como o Spring) em tempo de execução, permitindo que a lógica de negócio permaneça focada no domínio, sem se preocupar com os detalhes de implementação.

**Arquitetura**

![alt text](/images/architecture.png)

---

## Contextos

### Delivery Tracking

![alt text](/images/delivery-tracking-uml.png)

### Courier Management

![alt text](/images/courier-management-uml.png)

## Implementação

Detalhes importantes sobre a implementação utilizando DDD.

### Criação Controlada de Entidades com Factory Method

No projeto **AlgaDelivery**, seguimos os princípios do **Domain-Driven Design (DDD)** e, por isso, optamos por utilizar métodos de fábrica estáticos para instanciar nossas entidades, ao invés de construtores públicos ou anotações como `@AllArgsConstructor`.

#### Por que não usamos `@AllArgsConstructor`?

Embora o uso de anotações como `@AllArgsConstructor` (do Lombok) seja comum em aplicações Java para gerar construtores com todos os atributos, essa prática não é recomendada em **modelos de domínio ricos**. Os motivos são:

* Expor todos os atributos do domínio ao construtor viola o **encapsulamento** e **as invariantes do modelo**.
* Permite a criação de objetos inválidos ou incompletos fora do controle da própria entidade.
* Dificulta a evolução do domínio ao longo do tempo (ex: se novas regras de negócio surgirem).

#### Por que usar métodos de fábrica (factory methods)?

Ao usar um **factory method**, como o método `draft()` da classe `Delivery`, podemos centralizar e controlar a criação da entidade com valores padrão e regras iniciais:

```java
public static Delivery draft() {
  Delivery delivery = new Delivery();
  delivery.id = UUID.randomUUID();
  delivery.status = DeliveryStatus.DRAFT;
  delivery.totalItems = 0;
  delivery.totalCost = BigDecimal.ZERO;
  delivery.courierPayout = BigDecimal.ZERO;
  delivery.distanceFee = BigDecimal.ZERO;
  return delivery;
}
```

Essa abordagem nos dá vantagens como:

* **Claridade semântica**: `Delivery.draft()` expressa claramente a intenção de criar uma entrega em estado inicial de rascunho.
* **Segurança do domínio**: evita a criação incorreta ou incompleta da entidade.
* **Flexibilidade para evoluir o processo de criação**: novas regras, logs, ou validações podem ser facilmente adicionadas ao método de fábrica sem afetar outras partes do sistema.

#### Uso do `@NoArgsConstructor(access = AccessLevel.PACKAGE)`

O construtor padrão foi mantido **com visibilidade de pacote** (`AccessLevel.PACKAGE`) para:

* **Restringir a criação de objetos fora do domínio** (ex: por código externo ou de camadas de aplicação).
* **Permitir o uso por ferramentas como JPA/Hibernate**, que exigem um construtor padrão acessível.

Isso reforça o **encapsulamento** e mantém a entidade sob o controle do próprio domínio.

---

## Como Executar o Projeto

1. Certifique-se de ter o **PostgreSQL** rodando.
2. Crie um banco de dados com o nome desejado.
3. Configure o `application.properties` com:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/courierdb
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

4. No terminal:

```bash
./mvnw spring-boot:run
```

---

## 📌 Considerações Finais

Este projeto é um excelente ponto de partida para entender como estruturar um sistema real utilizando **DDD**, **Spring Boot** e **arquitetura de microserviços**. A modularização por subdomínios estratégicos facilita a escalabilidade e manutenção de grandes sistemas.

---

> Projeto em andamento. Mais módulos e funcionalidades serão adicionados conforme avanço no curso.
