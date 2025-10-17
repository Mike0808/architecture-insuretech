# Задание 5. Проектирование GraphQL API

Контракт REST API Swagger:
```YAML
swagger: '2.0'
info:
 description: API сервиса управления клиентскими данными
 version: 1.0.0
 title: Клиентский Сервис
host: api.client-service.com
basePath: /v1
schemes:
 - https
paths:
 /clients/{id}:
   get:
     tags:
       - Клиент
     summary: Получить информацию о клиенте по ID
     description: Возвращает информацию о клиенте.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           $ref: '#/definitions/Client'
 /clients/{id}/documents:
   get:
     tags:
       - Документы
     summary: Список документов клиента
     description: Возвращает список документов клиента по ID.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента для поиска его документов
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           type: array
           items:
             $ref: '#/definitions/Document'
 /clients/{id}/relatives:
   get:
     tags:
       - Родственники
     summary: Информация о родственниках клиента
     description: Возвращает информацию о родственниках клиента по ID.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           type: array
           items:
             $ref: '#/definitions/Relative'
definitions:
 Client:
   type: object
   properties:
     id:
       type: string
     name:
       type: string
     age:
       type: integer
 Document:
   type: object
   properties:
     id:
       type: string
     type:
       type: string
     number:
       type: string
     issueDate:
       type: string
     expiryDate:
       type: string
 Relative:
   type: object
   properties:
     id:
       type: string
     relationType:
       type: string
     name:
       type: string
     age:
       type: integer
```

Контракт GraphQL

```graphql
# Типы данных 
type Client {
    id: ID!
    name: String
    age: Int
    documents: [Docement]
    relatives: [Relative]
}

type Document {
    id: ID!
    type: String
    number: String
    issueDate: String
    expireDate: String
}

type Relative {
    id: ID!
    relationType: String
    name: String
    age: Int
}

# Запросы

type Query {
    # Получить клиента по id
    client(id: ID!): [Client]

    # Получить документы клиента
    clientDocuments(id:ID!): [Document]

    # Получить родственников клиента
    clientRelatives(id:ID!): [Relative]
}

```
Пример вызова в все одном запросе
```graphql
query GetClientFull {
    client(id: "123") {
        id
        name
        age
        documents {
            type
            number
        }
        relatives {
            name
            relationType
        }
    }
}
```
Пример конкретного вызова
```graphql
query GetClientShort {
    client(id: "123") {
        name
        documents {
            number
            expiryDate
        }
    }

}
```