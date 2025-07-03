# Sistema de Consulta à Tabela FIPE

## 🎓 Descrição Científica

Este projeto tem como objetivo aplicar conceitos de engenharia de software, integração com APIs REST, serialização de dados em JSON e utilização de recursos avançados da linguagem Java para construir um sistema interativo de consulta à Tabela FIPE. A aplicação foi desenvolvida utilizando o framework **Spring Boot**, implementando a interface `CommandLineRunner` para executar operações via terminal.

A fonte de dados utilizada é a **API pública da Tabela FIPE** ([API FIPE](https://deividfortuna.github.io/fipe/)), que permite consultar valores médios de veículos no mercado nacional brasileiro, categorizados em carros, motos e caminhões.

## 🧪 Objetivos de Aprendizado

- Prática com consumo de APIs externas usando `HttpClient`;
- Uso do padrão de projeto `Service` para modularizar responsabilidades;
- Desserialização de dados JSON com a biblioteca **Jackson** (`ObjectMapper`);
- Implementação de interface para flexibilidade no parse de dados (`IConverteDados`);
- Manipulação de coleções utilizando **Streams API** e **funções lambda**;
- Aplicação de boas práticas com `records`, separação de responsabilidades e leitura dinâmica com `Scanner`.


## 🧱 Estrutura do Projeto

```text
TabelaFipe/
├── model/             # Define os modelos de dados retornados pela API
│   ├── Dados.java
│   ├── Modelos.java
│   └── Veiculo.java
│
├── principal/         # Contém a classe Principal com a lógica interativa
│   └── Principal.java
│
├── service/           # Camada de serviço responsável por requisições e conversão
│   ├── ConverteDados.java
│   ├── IConverteDados.java
│   └── ConsumoApi.java
│
├── TabelaFipeApplication.java  # Classe principal com Spring Boot e CommandLineRunner
```



## 🔧 Tecnologias Utilizadas

- Java 17
- Spring Boot
- API Tabela FIPE (Parallelum)
- Biblioteca Jackson (com `@JsonAlias` e `@JsonIgnoreProperties`)
- Java HTTP Client (`java.net.http.HttpClient`)
- Java Records
- Stream API & Lambdas

## 🔎 Funcionamento da Aplicação

1. **Seleção da Categoria**: O usuário escolhe entre Carro, Moto ou Caminhão.
2. **Listagem das Marcas**: A aplicação consome a API e lista as marcas disponíveis para a categoria.
3. **Filtro de Modelos**: Após escolher a marca, o usuário pode pesquisar por trecho do nome do modelo.
4. **Busca por Anos**: O sistema lista os anos disponíveis para o modelo selecionado.
5. **Consulta de Avaliação**: Para cada ano disponível, a aplicação retorna os dados avaliativos do veículo (valor, tipo de combustível, ano, etc.).


## 📁 Detalhes Técnicos

### `TabelaFipeApplication.java`
- Classe principal da aplicação.
- Anotada com `@SpringBootApplication`.
- Implementa `CommandLineRunner` para iniciar via terminal.
- Executa a lógica definida em `Principal.exibeMenu()`.

---

### `principal/Principal.java`
- Contém o menu de navegação e interação com o usuário.
- Gerencia o fluxo de chamadas à API, filtragem de dados e exibição.
- Utiliza `Scanner` para entrada do usuário.
- Faz uso de `Stream API` para ordenação e filtragem de listas.

---

### `service/ConverteDados.java`
- Implementa a interface `IConverteDados`.
- Usa a biblioteca **Jackson** (`ObjectMapper`) para converter JSON em objetos Java.
- Contém os métodos:
  - `obterDados(String json, Class<T> classe)`: converte um único objeto.
  - `obterLista(String json, Class<T> classe)`: converte uma lista de objetos.

---

### `service/ConsumoApi.java`
- Responsável por realizar chamadas HTTP usando `HttpClient`.
- Método principal:
  - `obterDados(String endereco)`: retorna a resposta JSON como `String`.

---

### `model/Dados.java`
- Representa elementos com código e nome (ex: marca, modelo, ano).
- Utilizado amplamente em diversas respostas da API.
- Implementado como `record`.

---

### `model/Modelos.java`
- Representa uma resposta que contém uma lista de `Dados` referente aos modelos de uma marca.
- Também implementado como `record`.

---

### `model/Veiculo.java`
- Representa os dados detalhados de um veículo (valor, ano, combustível, etc).
- Anotado com:
  - `@JsonAlias` para mapear nomes específicos da API para os atributos.
  - `@JsonIgnoreProperties(ignoreUnknown = true)` para ignorar campos desnecessários.

---
