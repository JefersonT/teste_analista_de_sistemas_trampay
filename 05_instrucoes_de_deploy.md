# Instruções de Deploy e Demonstração
*Não foi possível desenvolver o projeto devido a falta de equipamento qualificado para programação, portanto as instruções a seguir são exemplos meramente ilustrativos, passo-a-passos que geralmente são feito.*

Vou dividir em duas partes: deploy local com Docker e deploy em um provedor gratuito (Render).

1. Deploy com Docker (Local)
    - Pré-requisitos
        - Docker e Docker Compose instalados.
        - Código do projeto configurado em um repositório.
    - Criar o Dockerfile
        - Crie um arquivo Dockerfile na raiz do projeto com o seguinte conteúdo:

            ```dockerfile
            # Base image
            FROM node:18-alpine

            # Set working directory
            WORKDIR /app

            # Copy package.json and install dependencies
            COPY package*.json ./
            RUN npm install

            # Copy application code
            COPY . .

            # Expose application port
            EXPOSE 3000

            # Start the application
            CMD ["npm", "run", "start:prod"]
            ```
    - Criar o docker-compose.yml
        - Crie o arquivo docker-compose.yml para configurar o container do PostgreSQL e do aplicativo NestJS.

            ```yaml
            version: '3.8'
            services:
            app:
                build: .
                ports:
                - "3000:3000"
                environment:
                DATABASE_URL: postgres://user:password@db:5432/nestdb
                depends_on:
                - db

            db:
                image: postgres:14-alpine
                container_name: postgres
                environment:
                POSTGRES_USER: user
                POSTGRES_PASSWORD: password
                POSTGRES_DB: nestdb
                ports:
                - "5432:5432"
                volumes:
                - postgres_data:/var/lib/postgresql/data

            volumes:
            postgres_data:
            ```
    - Executar o Ambiente
        - No terminal, execute:
            ```bash
            docker-compose up --build
            ```
        - O serviço estará disponível em `http://localhost:3000`.
    - Testar Endpoints
        - Use o Postman ou curl para testar os endpoints, como:
            ```bash
            curl -X POST http://localhost:3000/accounts -H "Content-Type: application/json" -d '{
            "name": "João",
            "cpf": "12345678901",
            "email": "joao@gmail.com",
            "password": "senha123"
            }'
            ```
2. Deploy em Render
    - Configuração do Render
        1. Acesse Render e crie uma conta.
        2. Clique em **New + > Web Service**.
        3. Conecte o repositório GitHub contendo o projeto.
    - Configurar o Deploy
        - Branch: Escolha a branch principal (ex.: `main`).
        - Start Command: `npm run start:prod`.
        - Environment Variables:
            - `DATABASE_URL`: Configurar a URL do banco PostgreSQL.
            - `NODE_ENV`: production.

### Como Rodar o Projeto Localmente

1. Instale Docker e Docker Compose.
2. Clone o repositório:
    ```bash
    git clone git@github.com:JefersonT/teste_analista_de_sistemas_trampay.git
    cd teste_analista_de_sistemas_trampay
    ```
3. Execute:
    ```bash
    docker-compose up --build
    ```
4. O serviço estará disponível em http://localhost:3000.

### Testando os Endpoints
Use Postman ou cURL para testar os endpoints, segue um exemplo ilustrativo:

1. Criar conta:
    ```bash
    POST /accounts
    Body: { "name": "João", "cpf": "12345678901", "email": "joao@gmail.com", "password": "senha123" }
    ```
2. Consultar saldo:
    ```bash
    GET /accounts/:id/balance
    ```