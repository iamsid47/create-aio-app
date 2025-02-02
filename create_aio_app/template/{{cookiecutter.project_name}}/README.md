# {{ cookiecutter.project_name }}

___

## Requirements
- docker-compose

___

## Features

- aiohttp
- mypy
- pytest
- flake8
- trafaret
- docker-compose
- aio devtools
- black
- aiohttp debug toolbar
{%- if cookiecutter.use_postgres == 'y' %}
- postgres
- alembic
- aiopg
- sqlAlchemy
{%- endif %}


## Local development
All the development settings are stored in `/config/api.dev.yml`.

### Run
To start the project in development mode, run the following command:

```
make run
```

or just

```
make
```

To stop docker containers:

```
make stop
```

To clean up docker containers (removes containers, networks, volumes, and images created by docker-compose up):

```
make clean
```

Shell inside the running container

```
make bash # the command can be executed only if the server is running e.g. after `make run`
```

## Managing Dependencies

### Using `.in` Files and `pip-compile`

To manage project dependencies and ensure consistent versions, we use `.in` files and the `pip-compile` tool. This approach allows us to pin package versions and simplify the process of updating dependencies across different environments.

- `development.in`: Development dependencies.
- `documentation.in`: Documentation dependencies.
- `production.in`: Production dependencies.

#### Updating Dependencies

1. **Edit `.in` Files**: Open the relevant `.in` file and specify dependencies and version constraints.

2. **Compile Dependencies**: Run `make compile` to generate/update `.txt` files with pinned versions.

3. **Review and Test**: Check the updated `.txt` files for your dependencies.

4. **Commit Changes**: Commit both `.in` and `.txt` files for consistent dependencies across environments.

Using `.in` files simplifies dependency management, ensuring your project stays up-to-date with controlled versions.

### Upgrade
To upgrade dependencies:

```
make upgrade
```

### Help

List available `Makefile` commands
```
make help
```

### Docs

To generate sphinx docs
```
make doc
```

### Linters
To run flake8:

```
make lint
```

All the settings for `flake8` can be customized in `.flake8` file

### Type checking
Run mypy for type checking:

```
make mypy
```

Settings for `mypy` can be customized in the `mypy.ini` file.

___

{%- if cookiecutter.use_postgres == 'y' %}

### Testing
```
make test
```

### Database
Database migrations are managed with [alembic](http://alembic.zzzcomputing.com/en/latest/).

New migrations should be added to `{{ cookiecutter.project_name }}/migrations/versions/`:

```
make migrations # the command can be executed only if the server is running e.g. after `make run`
```

Apply migrations:

```
make migrate # the command can be executed only if the server is running e.g. after `make run`
```

If u want to create a new file with tables u should import tables to `{{ cookiecutter.project_name }}/migrations/env.py`

```
import {{ cookiecutter.project_name }}.users.tables # import new files here
```

If u need to make downgrade or execute other alembic specific commands, use `make bash`
to get the shell inside the container and run alembic commands as you would normally do.

```
make bash
alembic downgrade -1
```

You can connect to the postgres inside running container with the command:

```
make psql # the command can be executed only if the database server is running e.g. after `make run` 
```
{%- endif %}

## Software

- python3.7
