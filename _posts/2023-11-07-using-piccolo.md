# I used PiccoloORM for the fist time

## What Happened

# The Bigger Picture

# What they said

# What Happens Next?

### Install 

`pip install 'piccolo[postgres]'`

### Getting a project started

Creating a FastAPI project is as straight forward as it gets:

`piccolo asgi new`

The upcoming prompt allows you to select which framework you'd like to use. Starlette, FastAPI, Blacksheep and Litestar are all an option. In my case I selected FastAPI.

### Installing Requirements
To install using poetry, after running `poetry init` you can install requirements via:
```bash
cat requirements.txt | xargs poetry add
```

### Creating a dabatbase in Postgresql

To access `psql` shell in Ubuntu we can run `sudo su postgres -c psql` (if you're using mac you can go ahead with `psql` only).

1. Create a User

`CREATE User smart_crawler with password 'smart_crawler';`

2. Create a Datbase

`CREATE DATABASE smart_crawler owner smart_crawler;`

3. Verify

`\du` will list your users, `\l+` will list you databases.

4. Go to `main.py` and update your database name, username and password. These should be retrieved through environment variables.

### Running the website

We can sanity check we are on the right direction:

`poetry run python main.py`

The website should run correctly.

### Creating and running migrations

Run the commands as shown by the index page. Make sure that your poetry shell is active before running the below commands. Otherwise you will get an error: "ModuleNotFoundError: No module named 'piccolo_admin'"

`piccolo migrations forwards session_auth`
`piccolo migrations forwards user`

### Create your first user

`piccolo user create`