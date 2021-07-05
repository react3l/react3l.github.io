---
layout: default
title: TypeORM
parent: Database
nav_order: 2
---

# TypeORM

<https://github.com/typeorm/typeorm>

## Installation

```ts
npm install typeorm --save
# or
yarn add typeorm
```

## Directory structure

Create a new folder in the `src` directory with the following files:

```
src
├──  databases
│   ├── config
│   │   └── const.ts
│   ├── helpers
│   ├── models       // place where your entities (database models) are stored
│   │   └── CategoryDAO.ts
│   ├── samples       // sample data
│   ├── services
│   │   └── DAO-to-entity.ts  // Object.assign<DAO, Entity>()
│   │   └── entity-to-DAO.ts  // Object.assign<Entity, DAO>()
│   │   └── data-service.ts     // create seed data
│   │   └── database-service.ts  // connect db
│   ├── index.ts
├── entities       // place where your entities (entity models) are stored
│   │   └── Category  // sample model
│   │   │   └── Category.ts
│   │   │   └── CategoryFilter.ts
│   │   │   └── index.ts
├── repositories       // CRUD methods
└── ...
```

- `Entity`: is a business object (BO) within a multitiered software application that works in conjunction with the data access and business logic layers to transport data.
- `DAO`: Data Access Objects is an abstraction of data persistence. It would be considered closer to the database, often table-centric.

## Usage

### Connection

1. Create the connection

```ts
// create the connection instance
const connection = createConnection({
  name: "default",
  type: "react-native",
  database: "default",
  location: "default",
  synchronize: true,
  logging: ["error"],
  entities: [
    Todo, //... whatever entities you have
  ],
})
```

> Common connection options: https://typeorm.io/#connection-options/common-connection-options

2. Connects the connection.

```ts
await connection.connect()
```

3. Closes the connection.

```ts
await connection.close()
```

### Entity

All active-record entities must extend the BaseEntity class, which provides methods to work with the entity.
Example:

```ts
import { BaseEntity, Column } from "typeorm/browser"

@Entity("Category")
export class CategoryDAO extends BaseEntity {
  @Column("integer", { primary: true, name: "id" })
  id: number

  @Column("text", { name: "code" })
  code: string

  @Column("text", { name: "name" })
  name: string

  @Column("boolean", { name: "isMaster" })
  isMaster: boolean

  @Column("datetime", { name: "startAt", nullable: true })
  startAt: Date

  @Column("integer", { name: "iconId", nullable: true })
  iconId: number
}
```

##### Primary columns

Each entity **must have** at least one primary column. There are several types of primary columns besides the above example:

- `@PrimaryColumn()` creates a primary column which takes any value of any type. You can specify the column type. If you don't specify a column type it will be inferred from the property type. The example below will create id with int as type which you must manually assign before save.
- `@PrimaryGeneratedColumn()` creates a primary column which value will be automatically generated with an auto-increment value. It will create int column with auto-increment/serial/sequence (depend on the database). You don't have to manually assign its value before save - value will be automatically generated.
- `@PrimaryGeneratedColumn("uuid")` creates a primary column which value will be automatically generated with uuid. Uuid is a unique string id. You don't have to manually assign its value before save - value will be automatically generated.

##### Special columns

There are several special column types with additional functionality available:

- `@CreateDateColumn` is a special column that is automatically set to the entity's insertion date. You don't need to set this column - it will be automatically set.
- `@UpdateDateColumn` is a special column that is automatically set to the entity's update time each time you call save of entity manager or repository. You don't need to set this column - it will be automatically set.
- `@DeleteDateColumn` is a special column that is automatically set to the entity's delete time each time you call soft-delete of entity manager or repository. You don't need to set this column - it will be automatically set. If the `@DeleteDateColumn` is set, the default scope will be "non-deleted".
- `@VersionColumn` is a special column that is automatically set to the version of the entity (incremental number) each time you call save of entity manager or repository. You don't need to set this column - it will be automatically set.

### Relations

Don't use relation in my project! You can read more about the Relations on [docs](https://typeorm.io/#/relations).
