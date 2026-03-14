# Repositório de Amostras de Dados

Este repositório contém amostras de dados em diferentes formatos, como JSON, CSV, entre outros. As amostras de dados são usadas para **testar e validar scripts de importação e exportação** em diferentes cenários.

---

## Documentos

Documentos são objetos **JSON/BSON** que contêm os dados e os metadados associados. Eles são comumente usados em **bancos de dados NoSQL orientados a documentos**, como o **MongoDB**.

### O Básico de Documentos no MongoDB

- Cada documento é um objeto estruturado em pares `chave:valor`.
- Documentos podem ter campos aninhados e arrays.
- Coleções armazenam múltiplos documentos relacionados.
- Exemplo simples de documento:

```json
{
  "nome": "João",
  "idade": 30,
  "endereços": [
    {"rua": "Rua A", "cidade": "São Paulo"},
    {"rua": "Rua B", "cidade": "Rio de Janeiro"}
  ]
}
```

---

## Tabelas / SQL

Tabelas são estruturas de dados que armazenam informações em **linhas e colunas**, típicas de **bancos de dados relacionais**, como o **MySQL**.

### O Básico de Tabelas no MySQL

- Cada linha representa um registro.
- Cada coluna representa um campo do registro.
- As tabelas podem ter **chaves primárias** e **chaves estrangeiras** para relacionamentos.
- Exemplo de criação de tabela:

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    idade INT,
    cidade VARCHAR(50)
);
```

---

## Arquivos CSV

Arquivos **CSV (Comma-Separated Values)** são arquivos de texto que armazenam os dados em formato tabular, separados por vírgulas.

### Uso de Arquivos CSV

- Muito usados para **importação e exportação** de dados entre diferentes sistemas.
- Cada linha do arquivo representa um registro.
- Cada valor separado por vírgula representa um campo.
- Exemplo de arquivo CSV:

```csv
nome,idade,cidade
João,30,São Paulo
Maria,25,Rio de Janeiro
```

---

Este repositório pode ser usado como **base de teste** para scripts que processam dados, garantindo compatibilidade com diferentes formatos e sistemas.
