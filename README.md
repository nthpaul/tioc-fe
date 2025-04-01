## Getting Started

```bash
# run postgres
docker run -d -e POSTGRES_PASSWORD=your_password -p 5432:5432 --name tioc-dev postgres

# enter postgres container and install pgvector
docker exec -it tioc-dev bash
apt-get update
apt-get install -y build-essential postgresql-server-dev-17 git
git clone --branch v0.7.0 https://github.com/pgvector/pgvector.git
cd pgvector
make
make install
exit

docker exec -it tioc-dev service postgresql restart

# create extension for shadow db (prisma)
docker exec -it tioc-dev psql -U postgres -d template1 -c "CREATE EXTENSION IF NOT EXISTS vector;"

# create extension for main db
docker exec -it tioc-dev psql -U postgres -c "CREATE EXTENSION IF NOT EXISTS vector;"

# could also just pull an image with pgvector already installed if you don't want to install it locally
# still need to create extension for shadow db (prisma)

# create migration
bunx prisma migrate dev --name init --create-only
bunx prisma db push

# initialize prisma
bunx prisma init
bunx prisma migrate dev --name init

bun dev
```
