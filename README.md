# Go RAG Api

A minimal Retrieval-Augmented Generation (RAG) service in Go that ingests text documents, performs semantic retrieval with a vector database, and generates answers using an LLM with the retrieved context.

The main.go is a example from https://github.com/golang/example/tree/master/ragserver/ragserver-langchaingo, repository of langchain with examples and tests.

## Setup

First, start Weaviate database instance to insert your embeddings.

```bash
docker-compose up -d
```

Run project with your GEMINI_API_KEY

```bash
GEMINI_API_KEY="your_api_key" go run .
```

Good! Your api is running on port 9020.

## Routes

This route add texts on weaviate embedded to query your questions.

POST /add

```bash
echo '{
        "documents": [
        {"text": "RAG is very easy to learn"}
]}' | tr -d "\n" | curl \
                -X POST \
    -H 'Content-Type: application/json' \
    -d @- \
    http://localhost:9020/add/

```

With this route you can ask anything or specific things of your context recently add using /add route.

POST /query

```bash
curl -X POST http://localhost:9020/query/ -H "Content-Type: application/json" -d '{"content":"Learn RAG is easy?"}' | jq
```

To verify your database documents, try:

```bash
echo '{
  "query": "{
    Get {
      Document {
        text
      }
    }
  }"
}' | tr -d "\n" | curl \
    -X POST \
    -H 'Content-Type: application/json' \
    -d @- \
    http://localhost:8080/v1/graphql | jq .
```

To delete all documents, try:

```bash
curl \
  -X DELETE \
  http://localhost:8080/v1/schema/Document

```
