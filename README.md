## IndianRecipes - GDG Mumbai Demo

<img width="1135" alt="app_screenshot" src="https://github.com/YugabyteDB-Samples/yugarecipes/assets/2041330/1bcdec08-b11c-4a58-900a-e136070202a8">

## Prerequisites

- Install Python 3+
- Install Node.js 18+
- Install Ollama or obtain OpenAI API Key
- Install Docker
- setup Gemini Foundation Model Integration

## Set up the Database

AlloyDB Omni Docker Image

### Set up PostgreSQL with pgvector



### Set up AlloyDB Omni


```

The database connectivity settings are provided in the `.env` file and do not need to be changed if you started the cluster with the preceding command.

## Load the schema and seed data

This application requires a database table with information about news stories. This schema includes a `news_stories` table.

### PostgreSQL

1. Make database directory.
   ```sh
   docker exec -it postgres mkdir /home/database
   ```
2. Copy the schema to the first node's Docker container.

   ```sh
   docker cp database/schema.sql postgres:/home/database
   ```

3. Copy the seed data file to the Docker container.

   ```sh
   docker cp database/output_with_embeddings.csv postgres:/home/database
   ```

4. Execute the SQL files against the database.

   ```sh
   docker exec -it postgres bin/psql -U postgres -f /home/database/schema.sql
   docker exec -it postgres bin/psql -U postgres -c "\COPY recipes(name,image_url,description,cuisine,course,diet,prep_time,ingredients,instructions,embeddings) from '/home/database/output_with_embeddings.csv' DELIMITER ',' CSV HEADER;"
   ```


## Starting the Application

This application is comprised of a Flask API Server and a React Frontend.

### Backend

1. Install backend dependencies.
   ```
   python -m venv venv
   source venv/bin/activate
   pip install -r ../requirements.txt
   ```
2. Start the server.
   ```
   python server.py
   ```

### Frontend

1. Install frontend dependencies.
   ```
   npm install
   ```
2. Start frontend.
   ```
   npm run dev
   ```

## Sample multipart form CURL request

```
curl -X POST http://127.0.0.1:5000/api/search -H "Content-Type: multipart/form-data" -F 'data={"query": "tomato"}' -F "image=@/Users/bhoyer/Projects/indian-recipes/backend/static/images/1.Thayir_Curd_Semiya_recipe_Yogurt_Vermicelli_South_indian_Lunch_recipe-4.jpg"
```
