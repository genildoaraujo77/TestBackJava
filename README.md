# Show me the code

### # DESAFIO:EMPRESA QUE ESTOU ME CANDIDATANDO ALTRAN

API REST para Gestão de Gastos!

Baixando imagem no docker
sudo docker pull redis:[version] --- linux
docker pull redis:[version] --- windows pelo assistente do docker (Docker ToolBox)

-------------------------------------------------------------------------------------------------------------------------------

Iniciando container
docker run -it --name redis -p 6379:6379 redis:[version]

-------------------------------------------------------------------------------------------------------------------------------

No application.properties coloquei as informações para conectar com a machine do docker

spring.jpa.hibernate.ddl-auto=none
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true

spring.redis.host=192.168.99.100
spring.redis.database=0
redis.maxTotal=5
redis.maxIdle=5
redis.testOnBorrow=true
server.port=9090

spring.main.allow-bean-definition-overriding=true

-------------------------------------------------------------------------------------------------------------------------------

URLs
-------------------------------------------------------------------------------------------------------------------------------
//URL METHOD=POST http://localhost:9090/registros <-------Para registrar um usuário
Enviar dados no body, exemplo abaixo:
{
    "nome": "nome do user",
    "login": "login user 1",
    "password": "senha user 1"
}
Exemplo de resposta abaixo:
Usuário registrado com sucesso!

-------------------------------------------------------------------------------------------------------------------------------
//URL METHOD=POST http://localhost:9090/authuser <--------Para fazer login e gerar um token valido por 30 minutos
Enviar dados no body, exemplo abaixo:
{
    "login": "login user 1",
    "password": "senha user 1"
}
Exemplo de resposta abaixo:
Token gerado com sucesso!

--------------------------------------------------------------------------------------------------------------------------------
//URL METHOD=POST http://localhost:9090/gastos <---------Para salvar um gasto 
Enviar dados no body, exemplo abaixo:
{
  "idgasto": 3,
  "descricao": "descricao gasto 3",
  "valor": 10.0,
  "data": "18/06/2020",
  "coduser": 1
}
POSSÍVEL ERRO CASO O TOKEN ESTEJA COM O TEMPO EXPIRADO 
mensagem no body da response "Token inválido ou usuário sem autorização"

-------------------------------------------------------------------------------------------------------------------------------
//URL METHOD=GET http://localhost:9090/gastos/listgastobydate?date={DATA} <----Para filtrar gastos por data
Enviar dados na url, exemplo abaixo:
http://localhost:9090/gastos/listgastobydate?date=27/03/1992
Exemplo de resposta abaixo:
[
    {
        "idgasto": 3,
        "descricao": "descricao gasto 3",
        "valor": 30.0,
        "data": "21/06/2020",
        "coduser": 1
    },
    {
        "idgasto": 2,
        "descricao": "descricao gasto 2",
        "valor": 20.0,
        "data": "21/06/2020",
        "coduser": 1
    },
    {
        "idgasto": 1,
        "descricao": "descricao gasto 1",
        "valor": 10.0,
        "data": "21/06/2020",
        "coduser": 1
    }
]

--------------------------------------------------------------------------------------------------------------------------------

//URL METHOD=GET http://localhost:9090/gastos/listlastgasto?limit={LIMIT} <----Para filtrar ultimos lançamentos dos gastos
Enviar dados na url, exemplo abaixo:
http://localhost:9090/gastos/listgastobydate?limit=5
Exemplo de resposta abaixo:
[
    {
        "idgasto": 3,
        "descricao": "descricao gasto 3",
        "valor": 30.0,
        "data": "21/06/2020",
        "coduser": 1
    },
    {
        "idgasto": 2,
        "descricao": "descricao gasto 2",
        "valor": 20.0,
        "data": "21/06/2020",
        "coduser": 1
    },
    {
        "idgasto": 1,
        "descricao": "descricao gasto 1",
        "valor": 10.0,
        "data": "21/06/2020",
        "coduser": 1
    }
]

--------------------------------------------------------------------------------------------------------------------------------

//URL METHOD=GET http://localhost:9090/gastos/gastodetail <----Para buscar detalhes do gasto
Enviar dados no body, exemplo abaixo:
{
        "idgasto": 1,
        "descricao": "descricao gasto 1",
        "valor": 10.0,
        "data": "21/06/2020"
}
Exemplo de resposta abaixo:
{
    "idgasto": 1,
    "descricao": "descricao gasto 1",
    "valor": 10.0,
    "data": "21/06/2020",
    "coduser": 1,
    "codcategoria": 0,
    "nomecategoria": "sem categoria"
}
OU APÓS SALVAR UMA NOVA CATEGORIA
{
    "idgasto": 1,
    "descricao": "descricao gasto 1",
    "valor": 10.0,
    "data": "21/06/2020",
    "coduser": 1,
    "codcategoria": 1,
    "nomecategoria": "categoria 1 para o gasto 1 do user 1"
}

--------------------------------------------------------------------------------------------------------------------------------

//URL METHOD=POST http://localhost:9090/gastos/gastodetail <----------Para salvar os detalhes do gasto
Enviar dados no body, exemplo abaixo:
{
    "idgasto": 1,
    "descricao": "descricao gasto 1",
    "valor": 10.0,
    "data": "21/06/2020",
    "codcategoria": 1,
    "nomecategoria": "categoria 1 para o gasto 1 do user 1"
}
Exemplo de resposta abaixo:
Dados do gasto atualizados com sucesso

--------------------------------------------------------------------------------------------------------------------------------

OBS: A funcionalidade abaixo não foi implementada, pois o creio que seja melhor implementa-la no front end, buscando um json de todas as
categorias pela URL METHOD=GET http://localhost:9090/categorias e criar um evento para ouvir as teclas e ir carregando as possiveis categorias para sugestão, caso esteja errado, pode 
considerar que eu não soube implementar no back end.

Funcionalidade: Sugestão de categoria
  Dado que acesso como um cliente autenticado
  Quando acesso o detalhe do gasto que não possui categoria
  E começo a digitar a categoria que desejo
  Então uma lista de sugestões de categoria deve ser exibida, estas baseadas em categorias já informadas por outro usuários.
  
URL METHOD=GET http://localhost:9090/categorias <----Para buscar todas as categorias
Exemplo de resposta abaixo:
[
    {
        "idcategoria": 1,
        "nome": "categoria 1 para o gasto 1 do user 1"
    },
    {
        "idcategoria": 2,
        "nome": "categoria 2 para o gasto 2 do user 2"
    }
]
