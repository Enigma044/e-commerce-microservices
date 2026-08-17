# E-commerce microservices

This project demonstrates a small microservices architecture built with Go, gRPC, GraphQL, PostgreSQL, Elasticsearch, and Docker Compose.

## Services

- Account service: manages customer accounts and stores data in PostgreSQL.
- Catalog service: manages products and stores data in Elasticsearch.
- Order service: creates orders and stores data in PostgreSQL.
- GraphQL gateway: exposes one API over the account, catalog, and order services.

## Project Structure

```text
.
├── account/              # Account gRPC service
├── catalog/              # Catalog gRPC service
├── order/                # Order gRPC service
├── graphql/              # GraphQL API gateway
├── assets/               # README assets
├── docker-compose.yaml   # Local service orchestration
├── go.mod
└── go.sum
```

## Requirements

- Go
- Docker
- Docker Compose

## Getting Started

Start all services:

```bash
docker-compose up -d --build
```

Open the GraphQL playground:

```text
http://localhost:8000/playground
```

Stop the services:

```bash
docker-compose down
```

## GraphQL Examples

Query accounts:

```graphql
query {
  accounts {
    id
    name
  }
}
```

Create an account:

```graphql
mutation {
  createAccount(account: { name: "New Account" }) {
    id
    name
  }
}
```

Query products:

```graphql
query {
  products {
    id
    name
    price
  }
}
```

Create a product:

```graphql
mutation {
  createProduct(product: { name: "New Product", description: "A new product", price: 19.99 }) {
    id
    name
    price
  }
}
```

Create an order:

```graphql
mutation {
  createOrder(order: { accountId: "account_id", products: [{ id: "product_id", quantity: 2 }] }) {
    id
    totalPrice
    products {
      name
      quantity
    }
  }
}
```

Query an account with orders:

```graphql
query {
  accounts(id: "account_id") {
    name
    orders {
      id
      createdAt
      totalPrice
      products {
        name
        quantity
        price
      }
    }
  }
}
```

## Regenerating Protobuf Files

Install the protobuf compiler and Go plugins, then run generation from each service directory:

```bash
protoc --go_out=./pb --go-grpc_out=./pb account.proto
protoc --go_out=./pb --go-grpc_out=./pb catalog.proto
protoc --go_out=./pb --go-grpc_out=./pb order.proto
```
