---
layout: default
title: Active Record, Data Mapper and Query Builder
parent: Database
---

# Active Record, Data Mapper and Query Builder

## Active Record
The **Active Record** pattern is an approach to access your database within your models. [Read more](https://en.wikipedia.org/wiki/Active_record_pattern).
```ts
// example how to save AR entity
    const categoryDAO = new CategoryDAO();
    user.code = "ABC";
    user.name = "Saw";
    user.isMaster = true;
    await categoryDAO.save(); // or await CategoryDAO.save(categoryDAO);

// example how to remove AR entity
    await user.remove();

// example how to load AR entities
    const categories: CategoryDAO[] = await CategoryDAO.find({
      where: {code: Equal('ABC')},
    });
```

## Data Mapper
**Data Mapper** is an approach to access your database within repositories instead of models. [Read more](https://en.wikipedia.org/wiki/Data_mapper_pattern).
```ts
const categoryDAORepository = connection.getRepository(CategoryDAO);

// example how to save AR entity
    const categoryDAO = new CategoryDAO();
    user.code = "ABC";
    user.name = "Saw";
    user.isMaster = true;
    await categoryDAORepository.save(); 

// example how to remove AR entity
    await categoryDAORepository.remove();

// example how to load AR entities
    const categories: CategoryDAO[] = await categoryDAORepository.find({
      where: {code: Equal('ABC')},
    });
```

## Query Builder
Query builder is used build complex SQL queries in an easy way. It is initialized from Connection method and QueryRunner objects. We can create QueryBuilder in three ways.
**Connection**
Consider a simple example of how to use QueryBuilder using connection method.
```ts
import {getConnection} from 'typeorm/browser';

const category = await getConnection()
  .createQueryBuilder()
  .select('Category')
  .from(CategoryDAO, 'Category')
  .where('Category.id = :id', {id: 1})
  .getOne();
```
**Entity manager**
Let’s create a query builder using entity manager as follows −
```ts
import {getManager} from 'typeorm/browser';

const category = await getManager()
  .createQueryBuilder(CategoryDAO, 'Category')
  .where('Category.id = :id', {id: 1})
  .getOne();
```
**Repository**
We can use repository to create query builder. It is described below,
```ts
import {getRepository} from 'typeorm/browser';

const category = await getRepository(CategoryDAO)
  .createQueryBuilder('Category')
  .where('Category.id = :id', {id: 1})
  .getOne();
```

## Which one should I choose?
One thing we should always keep in mind with software development is how we are going to maintain our applications.
|  Active Record | Data Mapper  | Query Builder  |
|---|---|---|
| The `Active record` approach helps keep things simple which works well in smaller apps. And simplicity is always a key to better maintainability.  |  The `Data Mapper` approach helps with maintainability, which is more effective in bigger apps.  | A better approach would be to build up the join table yourself by adding a custom join table entity. Then use `Query Builder` to replace the old foreign ky to the old channel with the is of the new one  | 
