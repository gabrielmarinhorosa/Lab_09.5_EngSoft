# Laboratório 9.5 — FastAPI e Docker

Engenharia de Software · API FastAPI empacotada em imagem Docker e testada localmente com o Postman.

## Passos

1. **API RESTful em FastAPI:** [`main.py`](main.py), com o CRUD de itens em `/items`.
2. **`.dockerignore`:** [`.dockerignore`](.dockerignore). Deixa fora da imagem o ambiente virtual, os caches,
   o git e a coleção do Postman. Dentro do container, `/app` tem só `main.py` e `requirements.txt`.
3. **Imagem Docker:** [`Dockerfile`](Dockerfile), com base `python:3.14-slim` e o Uvicorn servindo na porta 80.

   ```bash
   docker build -t lab09-fastapi .
   ```

4. **Rodar a imagem localmente:**

   ```bash
   docker run -d --name lab09-fastapi -p 8000:80 lab09-fastapi
   ```

   Depois acesse <http://127.0.0.1:8000/items> ou a documentação em <http://127.0.0.1:8000/docs>.
5. **Teste com o Postman:** use a
   [coleção do Postman](https://go.postman.co/collection/52575572-bc905e7b-89c8-4265-a227-8d7709392449)
   (cópia em [`postman/`](postman/lab_09_5_fastapi_docker.postman_collection.json)). Rode em ordem, com o
   container no ar. Pelo Postman web, selecione o **Desktop Agent**, senão a requisição para `127.0.0.1` falha.
   Rodada duas vezes contra o container: 14 requisições, 23 asserções, nenhuma falha.

Para parar e remover o container:

```bash
docker rm -f lab09-fastapi
```

| Método | Rota | Sucesso | Erros |
|---|---|---|---|
| GET | `/items` | 200 | |
| GET | `/items/{id}` | 200 | 404 |
| POST | `/items` | 201 | 422 |
| PUT | `/items/{id}` | 200 | 404, 422 |
| DELETE | `/items/{id}` | 200 | 404 |

O [9.6](https://github.com/gabrielmarinhorosa/Lab_09.6_EngSoft) leva esta mesma imagem para o Azure App Service
com CI/CD.
