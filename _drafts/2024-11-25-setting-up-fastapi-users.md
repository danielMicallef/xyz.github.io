---
draft: true
layout: post
title: FastAPI Users
lead: Step by Step setup of FastAPI Users
---

# An overview of FastAPI Users

FastAPI Users is a Python library that simplifies the process of adding user authentication to your FastAPI application. It provides a set of tools to manage user registration, login, and authentication. In this post, we will go through the process of setting up FastAPI Users in a FastAPI application.

## The different Components of FastAPI Users

FastAPI Users consists of the following components:
1. User Model: An SQLAlchemy or Beanie model that represents a user.
2. User Manager: A class that provides methods to manage users, such as creating, updating, and deleting users. It also provides different handlers for user registration, login, and authentication.
3. Authentication Backend: A class that provides methods to authenticate users based on their credentials. The authentication backend requires a Transport and a Strategy to communicate with the database and validate user credentials.
   - Transport: A way to pass the user token between the client and the server. This can be either through cookies or Bearers.
   - Strategy: A way to validate user credentials. This can be either through a Database or a JWT. The database can either be a table (called AccessToken) in your current database, or a Redis collection.
4. Routers: These are the endpoints that handle user registration, login, authentication, user verification, and password reset. For the Routers to work we need to define Pydantic Schemas for the different requests and responses.

In this post we are going to create a step-by-step example on how to setup fastapi-users with SQLAlchemy and using a JWT token as a transport.

# Setting up FastAPI Users

## Step 1: Install FastAPI Users

To install FastAPI Users, assuming we are going to use SQLAlchemy, run the following command:

```bash
pip install 'fastapi-users[sqlalchemy]'
```

## Step 2: Create a User Model

Create a User model that inherits from `fastapi_users_db_sqlalchemy.SQLAlchemyBaseUserTable`. In the example below we are using a UUID as the primary key (hence inheriting from `SQLAlchemyBaseUserTableUUID`) and adding two fields - `first_name` and `last_name`.

```python
class User(SQLAlchemyBaseUserTableUUID):
    first_name: Mapped[str | None] = Column(String, nullable=False)
    last_name: Mapped[str | None] = Column(String, nullable=False)
```

Implement `get_user_db` to create a SQLAlchemy database instance. This function is going to be required inorder to initialize the UserManager.

```python
from fastapi import Depends
from fastapi_users.db import SQLAlchemyUserDatabase
from sqlalchemy.ext.asyncio import AsyncSession, async_sessionmaker, create_async_engine

from settings import database_url

engine = create_async_engine(database_url)

async def get_session() -> AsyncSession:
    async_session_maker = async_sessionmaker(engine, expire_on_commit=False)
    async with async_session_maker() as session:
        yield session


async def get_user_db(session: AsyncSession = Depends(get_session)):
    yield SQLAlchemyUserDatabase(session, User)
```

## Step3: Create a User Schemas


## Step 4: The User Manager

### Step 4.1: Create User Manager

### Step 4.2: Create User Manager retrieval function

Inorder to initialize `FastAPIUsers` we need to create a function that retrieves the user manager (`get_user_manager`).

```python
async def get_user_manager(
    user_db: SQLAlchemyUserDatabase = Depends(get_user_db),
) -> UserManager:
    yield UserManager(user_db)
```

## Step 5: Create Authentication Backend
The final step is to set up an AuthenticationBackend. To set this up we need to decide two things:
1. Transport: How are we going to pass the user token between the client and the server? This can be either through cookies or Bearers.
2. Strategy: How are we going to validate user credentials? This can be either through a Database or a JWT. The database can either be a table (called AccessToken) in your current database, or a Redis collection.


## Step 6: Initialize the FastAPIUsers application and Create Routers


# Other considerations
Using FastAPI Users with SQLAdmin is a great way to add user authentication to your FastAPI application. However, there are a few things to consider when using FastAPI Users with SQLAdmin:
