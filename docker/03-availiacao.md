# Docker compose com Django e banco de dados

## Informações gerais

- Assunto: Docker, conteinizar aplicativos
- Disciplina: *sistemas operacionais*
- **Tarefa**:
  1. Criar um projeto django com uma aplicação web
    - alternativa, criar uma _branch_ no repositório do projeto integrador para configurar o acesso ao repositório de dados
  2. Criar um `Dockerfile` para o projeto django
  3. Criar a imagem e testar o conteiner para testar
  4. OPCIONAL, porque dependendo de como pergunta ao assistente de IA; criar um `Dockerfile` para o repositório de banco de dados
  5. Criar um `docker-compose.yml` e configurar para 2 serviços: `webapp` e `db`
  6. Configurar o arquivo django de acesso ao repositório de dados para usar o serviço docker `db`
  7. Testar o `docker-compose.yml`
  8. Relatar minimamente o que foi feito.
- **Entrega**: copia desse aquivo markdown preenchido, no seu repositório _fork_ de https://github.com/sistemas-operacionais/2024.2


## Relato

### 2. Criando o Dockerfile para o projeto django do PDS
Criei um arquivo chamado `Dockerfile` na raiz do projeto com o seguinte conteúdo:

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

### 3. Criando a Imagem e Testando o Contêiner
Para criar a imagem Docker, executei o comando:

```bash
docker build -t RnSemFila-django .
```

Depois, rodei o contêiner com:

```bash
docker run -p 8000:8000 myproject-django
```

Acessei `http://localhost:8000` no navegador para verificar se o Django estava funcionando.

### 4. Criando o Dockerfile para o Banco de Dados
Para o banco de dados, optei por usar o PostgreSQL. Criei um arquivo `Dockerfile.db`:

```Dockerfile
# Dockerfile.db
FROM postgres:13

ENV POSTGRES_DB=mydatabase
ENV POSTGRES_USER=myuser
ENV POSTGRES_PASSWORD=mypassword
```

### 5. Criando o `docker-compose.yml`
Criei um arquivo `docker-compose.yml` para orquestrar os serviços:

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    environment:
      POSTGRES_DB: mydatabase
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
    volumes:
      - postgres_data:/var/lib/postgresql/data/

  webapp:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:
```

### 6. Configurando o Django para Acessar o Banco de Dados
No arquivo `settings.py` do Django, configurei o acesso ao banco de dados:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'db',
        'PORT': '5432',
    }
}
```

### 7. Testando o `docker-compose.yml`
Para testar, executei o comando:

```bash
docker-compose up
```

Acessei `http://localhost:8000` e verifiquei se o Django estava funcionando corretamente com o banco de dados.

## 8. Relato Final
- Criei um projeto Django e conteinerizei ele usando Docker.
- Configurei um banco de dados PostgreSQL em um contêiner separado.
- Usei o `docker-compose.yml` para orquestrar os serviços.
- Testei a aplicação e tudo funcionou conforme o esperado.
