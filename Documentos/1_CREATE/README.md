# MongoDB - Operações CREATE (INSERT)

📝 Introdução  
As operações CREATE no MongoDB são realizadas através dos comandos insertOne() e insertMany(). Estes comandos permitem inserir novos documentos (registros) em uma coleção (equivalente a uma tabela em SQL).

🎯 Comandos Básicos

insertOne() - Inserir um único documento  
Insere um único documento em uma coleção.

Sintaxe:

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
})

Retorno:

{
  "acknowledged": true,
  "insertedId": ObjectId("507f1f77bcf86cd799439011")
}

insertMany() - Inserir múltiplos documentos  
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

Tipos Básicos

Tipo	Descrição	Exemplo
String	Texto	"João Silva"
Number	Números (inteiros ou decimais)	1.75, 30
Boolean	Verdadeiro ou falso	true, false
Date	Data e hora	ISODate("2024-01-15")
ObjectId	Identificador único do MongoDB	ObjectId("507f1...")
Array	Lista de valores	["Cardiologia", "Clínica Geral"]
Object	Documento aninhado	{cidade: "SP", estado: "SP"}
Null	Valor nulo/ausente	null

Exemplos de Tipos:

db.exemplo.insertOne({
  // String
  nome: "Maria Silva",

  // Number
  idade: 30,
  altura: 1.65,

  // Boolean
  ativo: true,

  // Date
  data_cadastro: ISODate("2024-01-15"),

  // Array
  especialidades: ["Cardiologia", "Clínica Geral"],

  // Object (subdocumento)
  endereco: {
    rua: "Av. Paulista",
    numero: "1000",
    cidade: "São Paulo"
  },

  // Null
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

Vantagem: Agrupa informações relacionadas em um único documento, facilitando consultas.

Documentos com Arrays

db.medicos.insertOne({
  "nome": "Dra. Beatriz Santos",
  "especialidades": [
    "Pediatria",
    "Infectologia",
    "Clínica Geral"
  ],
  "telefones": [
    "11988888888",
    "11977777777"
  ]
})

Vantagem: Permite armazenar múltiplos valores do mesmo tipo sem criar coleções separadas.

Arrays de Objetos

db.consultas.insertOne({
  "paciente_id": ObjectId("507f1f77bcf86cd799439011"),
  "data": ISODate("2024-01-15"),
  "receitas": [
    {
      "medicamento": "Paracetamol",
      "dosagem": "500mg",
      "frequencia": "8 em 8 horas"
    },
    {
      "medicamento": "Amoxicilina",
      "dosagem": "875mg",
      "frequencia": "12 em 12 horas"
    }
  ]
})

🎨 Boas Práticas

Use _id Customizado (Opcional)

db.pacientes.insertOne({
  "_id": "PAC001",  // ID customizado
  "nome": "José Santos",
  "CPF": "12345678900"
})

⚠️ Atenção: Se você definir um _id que já existe, a operação falhará.

Valide Dados Antes de Inserir

// ❌ Evite inserir dados incompletos
db.pacientes.insertOne({
  "nome": "Maria"
})

// ✅ Prefira inserir documentos completos
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

// ❌ Evite estruturas inconsistentes
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

// Inserir médico
db.medicos.insertOne({
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "nome": "Dr. Carlos Silva"
})

// Inserir consulta que referencia o médico
db.consultas.insertOne({
  "data": ISODate("2024-01-15"),
  "medico_id": ObjectId("507f1f77bcf86cd799439011"),  // Referência
  "paciente_id": ObjectId("507f1f77bcf86cd799439012")
})

Vantagens:

Evita duplicação de dados

Facilita atualizações (mudar nome do médico em um só lugar)

Desvantagens:

Requer múltiplas consultas ou uso de $lookup (JOIN)

Abordagem 2: Embedding (Documentos Aninhados)

db.consultas.insertOne({
  "data": ISODate("2024-01-15"),
  "medico": {
    "nome": "Dr. Carlos Silva",
    "CRM": "SP123456",
    "especialidade": "Cardiologia"
  },
  "paciente": {
    "nome": "João Santos",
    "CPF": "12345678900"
  }
})

Vantagens:

Uma única consulta retorna todos os dados

Melhor performance para leitura

Desvantagens:

Duplicação de dados

Se o médico mudar de nome, precisa atualizar em vários lugares

💡 Comparação com SQL

SQL	MongoDB
INSERT INTO pacientes (nome, idade) ...	db.pacientes.insertOne({nome: "João", idade: 30})
INSERT INTO ...	db.pacientes.insertMany([{...}, {...}])
Tabela	Coleção
Linha/Registro	Documento
Coluna	Campo
Chave Primária	_id
Chave Estrangeira	Referência (ObjectId) ou Embedding

🚀 Comandos Úteis

const resultado = db.pacientes.insertOne({nome: "João"})
console.log(resultado.acknowledged)  // true se sucesso
console.log(resultado.insertedId)     // ObjectId do documento inserido

Inserir com tratamento de erro

try {
  db.pacientes.insertOne({
    "_id": "PAC001",
    "nome": "Maria Silva"
  })
} catch (error) {
  console.log("Erro ao inserir:", error.message)
}

Inserir múltiplos documentos com opção ordered

// ordered: true (padrão) - para no primeiro erro
db.pacientes.insertMany([
  {nome: "João"},
  {_id: 1, nome: "Maria"},
  {_id: 1, nome: "Pedro"}  // Vai falhar (ID duplicado)
], {ordered: true})

// ordered: false - continua mesmo com erros
db.pacientes.insertMany([
  {nome: "João"},
  {_id: 1, nome: "Maria"},
  {_id: 1, nome: "Pedro"},  // Vai falhar
  {nome: "Ana"}  // Mas este será inserido
], {ordered: false})

📚 Operadores Especiais em Inserção

$currentDate - Data/hora atual
db.logs.insertOne({
  evento: "Login",
  usuario: "joao",
  timestamp: new Date()  // Data/hora atual
})

$inc - Não aplicável diretamente em INSERT
Operadores como $inc, $set, $push são usados em UPDATE, não em INSERT.

🎯 Exercícios Práticos

Exercício 1: Inserir um Paciente Simples

db.pacientes.insertOne({
  // Seu código aqui
})

Exercício 2: Inserir Múltiplos Médicos

db.medicos.insertMany([
  // Seu código aqui
])

Exercício 3: Inserir Documento com Subdocumentos

db.pacientes.insertOne({
  nome: "Seu Nome",
  endereco: {
    // Complete aqui
  },
  contato: {
    // Complete aqui
  }
})

🔍 Verificando Suas Inserções

db.pacientes.find()
db.pacientes.find().pretty()
db.pacientes.countDocuments()
db.pacientes.find().sort({_id: -1}).limit(1)

📖 Recursos Adicionais

Documentação Oficial: MongoDB Insert
Schema Design: Como estruturar seus documentos
Validação de Schema: Como garantir que documentos sigam regras específicas

🎓 Resumo

insertOne(): Insere um documento
insertMany(): Insere múltiplos documentos
Documentos: Estruturas flexíveis (não precisam seguir um schema rígido)
Subdocumentos: Objetos aninhados para agrupar dados relacionados
Arrays: Listas de valores ou objetos
_id: Identificador único (criado automaticamente ou customizado)
Referências vs Embedding: Duas formas de relacionar dados

Agora você está pronto para criar documentos no MongoDB! 🚀
