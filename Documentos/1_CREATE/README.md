# MongoDB - Operações CREATE (INSERT)
📝 Introdução
As operações CREATE no MongoDB usam os comandos insertOne() e insertMany(). Eles inserem documentos em uma coleção (equivalente a tabela em SQL).

🎯 Comandos Básicos
insertOne() - Inserir um único documento
db.colecao.insertOne({campo1: valor1, campo2: valor2})
Exemplo:
db.pacientes.insertOne({
  "nome": "João Silva",
  "data_nascimento": ISODate("1990-05-15"),
  "altura": 1.80,
  "documentos": { "CPF": "12345678900", "RG": "MG1234567" }
})

insertMany() - Inserir múltiplos documentos
db.colecao.insertMany([{campo1: valor1, campo2: valor2}, {campo1: valor3, campo2: valor4}])
Exemplo:
db.pacientes.insertMany([
  {"nome": "Maria Santos","data_nascimento": ISODate("1985-03-20"),"altura":1.65},
  {"nome": "Pedro Oliveira","data_nascimento": ISODate("1992-07-10"),"altura":1.75}
])

🔑 Tipos de Dados
String: "João Silva"
Number: 1.75, 30
Boolean: true, false
Date: ISODate("2024-01-15")
ObjectId: ObjectId("507f1...")
Array: ["Cardiologia","Clínica Geral"]
Object: {cidade: "SP"}
Null: null

Exemplo combinando tipos:
db.exemplo.insertOne({
  nome: "Maria Silva",
  idade: 30,
  altura: 1.65,
  ativo: true,
  data_cadastro: ISODate("2024-01-15"),
  especialidades: ["Cardiologia","Clínica Geral"],
  endereco: {rua:"Av. Paulista", numero:"1000", cidade:"São Paulo"},
  observacoes: null
})

🏗️ Estruturas de Dados
Documentos Simples:
db.medicos.insertOne({"nome": "Dr. Carlos Silva","CRM":"SP123456","especialidade":"Cardiologia"})

Subdocumentos (Objetos Aninhados):
db.pacientes.insertOne({
  "nome": "Ana Costa",
  "endereco": { "logradouro":"Rua das Flores","numero":"100","bairro":"Centro","cidade":"São Paulo","estado":"SP","CEP":"01000000" },
  "contato": { "telefone":"11999999999","email":"ana@email.com" }
})

Arrays:
db.medicos.insertOne({
  "nome": "Dra. Beatriz Santos",
  "especialidades": ["Pediatria","Infectologia","Clínica Geral"],
  "telefones": ["11988888888","11977777777"]
})

Arrays de Objetos:
db.consultas.insertOne({
  "paciente_id": ObjectId("507f1f77bcf86cd799439011"),
  "data": ISODate("2024-01-15"),
  "receitas": [
    {"medicamento":"Paracetamol","dosagem":"500mg","frequencia":"8 em 8 horas"},
    {"medicamento":"Amoxicilina","dosagem":"875mg","frequencia":"12 em 12 horas"}
  ]
})

🎨 Boas Práticas
1. _id Customizado (Opcional):
db.pacientes.insertOne({"_id":"PAC001","nome":"José Santos","CPF":"12345678900"})
2. Valide dados antes de inserir
3. Estrutura consistente:
db.pacientes.insertMany([
  {nome:"João", idade:30, endereco:{cidade:"SP",estado:"SP"}},
  {nome:"Maria", idade:25, endereco:{cidade:"RJ",estado:"RJ"}}
])
4. Use ISODate para datas:
db.consultas.insertOne({data: ISODate("2024-01-15T10:30:00Z")})

🔗 Relacionamentos entre Coleções
Referência (Reference):
db.medicos.insertOne({"_id": ObjectId("507f1f77bcf86cd799439011"), "nome":"Dr. Carlos Silva"})
db.consultas.insertOne({"data": ISODate("2024-01-15"), "medico_id": ObjectId("507f1f77bcf86cd799439011")})

Embedding (Documentos Aninhados):
db.consultas.insertOne({
  "data": ISODate("2024-01-15"),
  "medico": {"nome":"Dr. Carlos Silva","CRM":"SP123456","especialidade":"Cardiologia"},
  "paciente": {"nome":"João Santos","CPF":"12345678900"}
})

💡 Comparação com SQL
SQL | MongoDB
INSERT INTO pacientes ... | db.pacientes.insertOne({ ... })
Tabela | Coleção
Linha/Registro | Documento
Coluna | Campo
Chave Primária | _id
Chave Estrangeira | Referência (ObjectId) ou Embedding

🚀 Comandos Úteis
Verificar inserção:
const resultado = db.pacientes.insertOne({nome:"João"})
console.log(resultado.acknowledged)  // true
console.log(resultado.insertedId)    // ObjectId inserido

Inserção com tratamento de erro:
try { db.pacientes.insertOne({"_id":"PAC001","nome":"Maria Silva"}) }
catch (error) { console.log("Erro:", error.message) }

Inserir múltiplos com ordered:
db.pacientes.insertMany([{nome:"João"}, {_id:1,nome:"Maria"}, {_id:1,nome:"Pedro"}], {ordered:true})
db.pacientes.insertMany([{nome:"João"}, {_id:1,nome:"Maria"}, {_id:1,nome:"Pedro"}, {nome:"Ana"}], {ordered:false})

📚 Recursos Adicionais
Documentação Oficial: MongoDB Insert
Schema Design: Estrutura de documentos
Validação de Schema: Garantir regras específicas

🎓 Resumo
insertOne(): insere um documento
insertMany(): insere múltiplos documentos
Documentos: flexíveis, sem schema rígido
Subdocumentos: agrupar dados relacionados
Arrays: listas de valores ou objetos
_id: identificador único
Referências vs Embedding: relacionamentos entre coleções

> Agora você está pronto para criar documentos no MongoDB! 🚀
