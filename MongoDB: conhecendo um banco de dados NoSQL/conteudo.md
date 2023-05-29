# MongoDB: conhecendo um banco de dados NoSQL

## 1. NoSQL

   1. Entender o que é o NoSQL;
      1. Necessidades para permitir novos modelos de aplicação
         1. mais recursos de *armazenamento* e *leitura* de dados.
      2. **NoSQL:** Não apenas SQL.
   2. Identificar os tipos de banco de dados NoSQL;
      1. Grafos
      2. Chave / valor
      3. Colunar
      4. Documento
   3. Conhecer o MongoDB;
      1. Estrutura de Documentos
      2. Esquema não é fixo. Podemos ter, dentro de uma mesma **coleção**, vários **documentos** que possuem diversos **campos diferentes**. Com essa estrutura, o MongoDB consegue **facilitar a reestruturação de um Banco de Dados**.
   4. Reconhecer as versões MongoDB existentes.
      1. **Community edition** é a edição **gratuita**
      2. **Enterprise edition** é a edição **comercial**
      3. **MongoDB Atlas** é uma versão hospedada na **nuvem**

## 2. Banco de dados e coleções

   1. Instalar o MongoDB, Compass e Mongosh;
   2. Criar banco de dados e coleções pelo Compass e pela linha de comando;
   3. Restrições: Os nomes das coleções **devem começar** com um **sublinhado** ou um caractere de **letra**. Não podem: *$*; Ser uma *string vazia* (por exemplo "", ); Conter o caractere *nulo*; Começar com o *system.prefixo*. (Reservado para uso interno).
   4. Remover banco de dados: Estando conectado ao BD `use DB`, executamos o comando `db.dropDatabase()`; Excluir todas as coleções de um banco de dados ele também remove o BD.;

## 3. Documentos

   1. Tipos de dados
      1. [Link da lista de titpos de dados](https://www.mongodb.com/docs/manual/reference/bson-types/)
   2. Restrições: O nome do campo **_id** é reservado para uso como chave primária. Seu valor deve ser **único** na coleção, é **imutável** e pode ser de **qualquer tipo que não seja um array**; Os nomes dos campos não podem conter o caractere **NULL**; Tamanho máximo de um documento BSON é **16 megabytes**.
   3. Importar de arquivo:
      1. **No MongoDB Compass:** Acesse a coleção que deseja importar os dados e clique no botão **ADD DATA** -> **Import File**
      2. **MongoDB Shell (mongosh):** Use o comando `use <nome-do-banco-de-dados>` -> Use o comando `load()` seguido do caminho para o arquivo JSON. Por exemplo: `load('caminho/para/arquivo.json')`;

## 4. Realizando consultas

   1. No **MongoDB Compass:**, para isso, temos o botão **FIND** que possui diversas opções: **FILTER**, **PROJECT**, **SORT**, **MAX TIME MS**, **COLLATION**, **SKIP**, **LIMIT**;
   2. Com o **Mongosh:**
      1. Modelo:

         ```javascript
         db.users.find(               ← collection
         { age: { $gt: 18  } },       ← query criteria
         { name: 1, address: 1 }      ← projection
         ).limit(5)                   ← cursor modifier
         ```

      2. Exemplos:

         ```javascript
         db.series.find()

         db.series.find({"Ano de lançamento": 2018})

         db.series.find({},{"Série":1, "Ano de lançamento": 1, "_id":0})

         db.series.find({"Ano de lançamento": {$in: [2019,2020]}})

         db.series.find().limit(5)

         db.series.find().sort({"Série":1})

         db.series.find({"Temporadas disponíveis": {$gte: 4}})

         db.series.find({"Ano de lançamento": {$ne: 2020}})

         db.series.find({"Genero": {$all:["Ação", "Comédia"]}})
         ```

## 5. Atualizando e removendo dados

   1. No **MongoDB Compass** temos alguns **botões** que podemos utilizar para: **alterar**, **editar**, **copiar**, **clonar** e **Deletar**
   2. No **Mongosh:** temos os comandos `updateOne`, `updateMany` [`ReplaceOne`](https://www.mongodb.com/docs/v6.0/reference/operator/aggregation/replaceOne/):
      1. Modelo do comando de **update**, é:

      ```javascript
      db.users.updateMany(               ← collection
      { age:  { $lt: 18 } },             ← update filter
         { $set:  { status: "reject" } } ← update action
      )
      ```

      2. Exemplos:

      ```javascript
      db.series.updateOne({"Série": "Grimm"},{$set: {"Temporadas disponíveis": 6}})

      db.series.updateOne({"Série": "Grimm"},{$set: {"Classificação": "16+"}})

      db.series.updateMany({"Série":{$in:["Four More Shots Please", "Fleabag"]}},{$set: {"Classificação": "18+"}})

      db.series.updateOne({"Série": "Grey's Anatomy"},{$set: {"IMDb Avaliação": 8.1}})
      ```

   3. Excluindo dados no **Mongosh** temos os comandos `deleteOne` e `deleteMany`. **Atenção** Se passarmos o `deleteMany()` vazio, removeremos todos os documentos da coleção.
      1. Modelo do comando de **deleteOne** e **deleteMany**, é:

      ```javascript
      db.users.deleteMany(  ← collection
      { status: "reject" }  ← delete filter)
      ```

      2. Exemplos:

      ```javascript
      db.series.deleteOne({"Série": "The Boys"})

      db.series.deleteMany({"Temporadas disponíveis": 1})

      db.series.deleteMany({"Ano de lançamento ": 2017})

      db.series.deleteMany({"Ano de lançamento ": { $gt :2017 }})

      db.series.deleteMany({})
      ```
