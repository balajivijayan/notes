docker network create kong-net

docker run -d --name kong-database \
 --network=kong-net \
 -p 5432:5432 \
 -e "POSTGRES_USER=kong" \
 -e "POSTGRES_DB=kong" \
 -e "POSTGRES_PASSWORD=kongpass" \
 postgres:13

 docker run --rm --network=kong-net \
-e "KONG_DATABASE=postgres" \
-e "KONG_PG_HOST=kong-database" \
-e "KONG_PG_PASSWORD=kongpass" \
kong:3.5.0 kong migrations bootstrap


docker run -d --name kong-gateway \
--network=kong-net \
-e "KONG_DATABASE=postgres" \
-e "KONG_PG_HOST=kong-database" \
-e "KONG_PG_USER=kong" \
-e "KONG_PG_PASSWORD=kongpass" \
-e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
-e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
-e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
-e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
-e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
-e "KONG_ADMIN_GUI_URL=http://localhost:8002" \
-p 4000:8000 \
-p 4443:8443 \
-p 127.0.0.1:8001:8001 \
-p 127.0.0.1:8002:8002 \
-p 127.0.0.1:8444:8444 \
kong:3.5.0


Kong Manager:
http://localhost:8002/

Kong ADMIN API
curl -i -X GET --url http://localhost:8001/services

curl -X GET http://localhost:4000/mock/anything

Ports:
4000, 4443 - Gateway Port, SSL
8001, 8444 - Admin API, SSL
8002 - Kong Manager UI




Clean Up
docker kill kong-gateway
docker kill kong-database
docker container rm kong-gateway
docker container rm kong-database
docker network rm kong-net


curl -i -s -X POST http://localhost:4001/services \
 --data name=example_service \
 --data url='http://httpbin.org'




