# MongoDB - Operações CREATE (INSERT)

## 📝 Introdução
As operações CREATE no MongoDB são realizadas através dos comandos `insertOne()` e `insertMany()`. Estes comandos permitem inserir novos documentos (registros) em uma coleção (equivalente a uma tabela em SQL).

## 🎯 Comandos Básicos

### insertOne() - Inserir um único documento
Insere um único documento em uma coleção.

**Sintaxe:**
```javascript
db.colecao.insertOne({
  campo1: valor1,
  campo2: valor2,
  ...
})
Exemplo:

db.pacientes.insertOne({
  "nome": "João Silva",
  "data_nascimento": ISODate("1990-05-15"),
  "altura": 1.80,
  "documentos": {
    "CPF": "12345678900",
    "RG": "MG1234567"
  }
})Retorno:

{
  "acknowledged": true,
  "insertedId": ObjectId("507f1f77bcf86cd799439011")
}insertMany() - Inserir múltiplos documentos

Insere vários documentos de uma vez em uma coleção.

Sintaxe:

db.colecao.insertMany([
  { campo1: valor1, campo2: valor2 },
  { campo1: valor3, campo2: valor4 },
  ...
])


Exemplo:

db.pacientes.insertMany([
  {
    "nome": "Maria Santos",
    "data_nascimento": ISODate("1985-03-20"),
    "altura": 1.65
  },
  {
    "nome": "Pedro Oliveira",
    "data_nascimento": ISODate("1992-07-10"),
    "altura": 1.75
  }
])


Retorno:

{
  "acknowledged": true,
  "insertedIds": [
    ObjectId("507f1f77bcf86cd799439012"),
    ObjectId("507f1f77bcf86cd799439013")
  ]
}

🔑 Tipos de Dados no MongoDB
Tipo	Descrição	Exemplo
String	Texto	"João Silva"
Number	Números (inteiros ou decimais)	1.75, 30
Boolean	Verdadeiro ou falso	true, false
Date	Data e hora	ISODate("2024-01-15")
ObjectId	Identificador único do MongoDB	ObjectId("507f1...")
Array	Lista de valores	["Cardiologia", "Clínica Geral"]
Object	Documento aninhado	{cidade: "SP", estado: "SP"}
Null	Valor nulo/ausente	null

Exemplo combinando tipos:

db.exemplo.insertOne({
  nome: "Maria Silva",
  idade: 30,
  altura: 1.65,
  ativo: true,
  data_cadastro: ISODate("2024-01-15"),
  especialidades: ["Cardiologia", "Clínica Geral"],
  endereco: {
    rua: "Av. Paulista",
    numero: "1000",
    cidade: "São Paulo"
  },
  observacoes: null
})

🏗️ Estruturas de Dados
Documentos Simples
db.medicos.insertOne({
  "nome": "Dr. Carlos Silva",
  "CRM": "SP123456",
  "especialidade": "Cardiologia"
})

Documentos com Subdocumentos (Objetos Aninhados)
db.pacientes.insertOne({
  "nome": "Ana Costa",
  "endereco": {
    "logradouro": "Rua das Flores",
    "numero": "100",
    "bairro": "Centro",
    "cidade": "São Paulo",
    "estado": "SP",
    "CEP": "01000000"
  },
  "contato": {
    "telefone": "11999999999",
    "email": "ana@email.com"
  }
})

Documentos com Arrays
db.medicos.insertOne({
  "nome": "Dra. Beatriz Santos",
  "especialidades": ["Pediatria", "Infectologia", "Clínica Geral"],
  "telefones": ["11988888888", "11977777777"]
})

Arrays de Objetos
db.consultas.insertOne({
  "paciente_id": ObjectId("507f1f77bcf86cd799439011"),
  "data": ISODate("2024-01-15"),
  "receitas": [
    { "medicamento": "Paracetamol", "dosagem": "500mg", "frequencia": "8 em 8 horas" },
    { "medicamento": "Amoxicilina", "dosagem": "875mg", "frequencia": "12 em 12 horas" }
  ]
})

🎨 Boas Práticas

Use _id Customizado (Opcional)

db.pacientes.insertOne({
  "_id": "PAC001",
  "nome": "José Santos",
  "CPF": "12345678900"
})


Valide Dados Antes de Inserir

// ❌ Evite dados incompletos
db.pacientes.insertOne({ "nome": "Maria" })

// ✅ Prefira documentos completos
db.pacientes.insertOne({
  "nome": "Maria Silva",
  "data_nascimento": ISODate("1990-05-20"),
  "CPF": "12345678900",
  "telefone": "11999999999"
})


Mantenha Estrutura Consistente

// ✅ Boa prática
db.pacientes.insertMany([
  { nome: "João", idade: 30, endereco: { cidade: "SP", estado: "SP" } },
  { nome: "Maria", idade: 25, endereco: { cidade: "RJ", estado: "RJ" } }
])

// ❌ Evite inconsistência
db.pacientes.insertMany([
  { nome: "João", idade: 30, endereco: { cidade: "SP" } },
  { nome: "Maria", anos: 25, end: "Rua X" }
])


Use ISODate para Datas

// ✅ Correto
db.consultas.insertOne({ data: ISODate("2024-01-15T10:30:00Z") })

// ❌ Evite strings
db.consultas.insertOne({ data: "15/01/2024" })

🔗 Relacionamentos entre Coleções
Abordagem 1: Referência (Reference)
db.medicos.insertOne({ "_id": ObjectId("507f1f77bcf86cd799439011"), "nome": "Dr. Carlos Silva" })

db.consultas.insertOne({
  "data": ISODate("2024-01-15"),
  "medico_id": ObjectId("507f1f77bcf86cd799439011"),
  "paciente_id": ObjectId("507f1f77bcf86cd799439012")
})

Abordagem 2: Embedding (Documentos Aninhados)
db.consultas.insertOne({
  "data": ISODate("2024-01-15"),
  "medico": { "nome": "Dr. Carlos Silva", "CRM": "SP123456", "especialidade": "Cardiologia" },
  "paciente": { "nome": "João Santos", "CPF": "12345678900" }
})

💡 Comparação com SQL
SQL	MongoDB
INSERT INTO pacientes (nome, idade) VALUES ...	db.pacientes.insertOne({nome: "João", idade: 30})
INSERT INTO ...	db.pacientes.insertMany([{...}, {...}])
Tabela	Coleção
Linha/Registro	Documento
Coluna	Campo
Chave Primária	_id
Chave Estrangeira	Referência (ObjectId) ou Embedding
🚀 Comandos Úteis
const resultado = db.pacientes.insertOne({nome: "João"})
console.log(resultado.acknowledged)  // true
console.log(resultado.insertedId)    // ObjectId inserido


Tratamento de erro:

try {
  db.pacientes.insertOne({ "_id": "PAC001", "nome": "Maria Silva" })
} catch (error) {
  console.log("Erro ao inserir:", error.message)
}


Inserir múltiplos com ordered:

db.pacientes.insertMany([{nome: "João"}, {_id:1,nome:"Maria"}, {_id:1,nome:"Pedro"}], {ordered:true})
db.pacientes.insertMany([{nome: "João"}, {_id:1,nome:"Maria"}, {_id:1,nome:"Pedro"}, {nome:"Ana"}], {ordered:false})

📚 Operadores Especiais em Inserção

$currentDate - Data/hora atual

db.logs.insertOne({ evento: "Login", usuario: "joao", timestamp: new Date() })


$inc não se aplica a INSERT (usado em UPDATE)

🎯 Exercícios Práticos

Inserir um Paciente Simples:

db.pacientes.insertOne({ /* Seu código aqui */ })


Inserir Múltiplos Médicos:

db.medicos.insertMany([ /* Seu código aqui */ ])


Inserir Documento com Subdocumentos:

db.pacientes.insertOne({
  nome: "Seu Nome",
  endereco: { /* Complete aqui */ },
  contato: { /* Complete aqui */ }
})

🔍 Verificando Suas Inserções
db.pacientes.find()
db.pacientes.find().pretty()
db.pacientes.countDocuments()
db.pacientes.find().sort({_id: -1}).limit(1)

📖 Recursos Adicionais

Documentação Oficial: MongoDB Insert

Schema Design: Estrutura de documentos

Validação de Schema: Garantir regras específicas

🎓 Resumo

insertOne(): Insere um documento

insertMany(): Insere múltiplos documentos

Documentos: Estruturas flexíveis

Subdocumentos: Objetos aninhados

Arrays: Listas de valores ou objetos

_id: Identificador único

Referências vs Embedding: Relacionamentos entre coleções

Agora você está pronto para criar documentos no MongoDB! 🚀
