
# Rest-API-Challenge

**All requisites where completed**

Requisitos Funcionais:
+ ~~REST API que expõe as operações de soma, subtracção, multiplicação e divisão.~~
+ ~~Suporte para dois operandos apenas (a e b, por simplicidade).~~
+ ~~Suporte para arbitraryprecision signed decimal numbers.~~

Requisitos Não Funcionais:
+ ~~Projecto Gradle ou Maven com pelo menos dois módulos — rest e calculator.~~
+ ~~Utilização de Spring Boot 2.2.6 como foundation de ambos os módulos.~~
+ ~~Utilização de RabbitMQ e Spring AMQP para comunicação intermódulo.~~
+ ~~Configuração via application.properties (default do Spring Boot).~~
+ ~~Nenhuma configuração XML (com excepção, eventualmente, da de logging).~~
+ ~~Versionamento do trabalho em Git.~~

Bonus Points (Opcional):
+ ~~Utilização de logback-access para logging do tráfego HTTP (+2).~~
+ ~~Atribuição a cada pedido REST individual de um identificador único e comunicação aos clientes do mesmo através de um response header (+3).~~
+ ~~Propagação deste identificador de request através do MDC na comunicação intermódulo e inclusão do mesmo em cada linha de logging que diga respeito a um pedido HTTP em ambos os módulos (+5).~~

## Project

+ 2 Modules - Rest and Calculator
+ Git used for version control
+ Maven
+ Spring boot 2.2.6
+ RabbitMq, Spring AMQP, lombok and logback access
+ RabbitMQ Config
    + One exchange
    + One Queue
    + One routing key
    + Docker container
+ Postman used for testing endpoints

## Rest

+ Handle API Endpoints with the four basic ones and accepts the parameters as Request parameters
+ All endpoints produce a json with the result
+ Has a WebMVCConfigurer that create a Handler interceptor to manage the identifier stored in the MDC and the response-headers
+ Uses BigDecimal for arbitraryprecision signed decimal numbers
+ Configuration data inside application.properties
+ Response-header with identifier
+ Logs (Both with request identifier so we can easily search for a specific request)
    + logback-access.xml to log the http traffic
    + logback.xml to log the logs added in the code
+ RabbitMQ to send messages to Calculator module sending the necessary parameters and request-identifier
+ Handles failed calculation results
+ Has a `TomcatServletWebServerFactory` that instanciates a `LogbackValve` to allow http logging

## Calculator

+ Configuration data inside application.properties
+ Handles the calculation
    + If success send back result
    + If fail send back result with null that is handled in rest side
+ Logs added in code
+ RabbitMq to send messages back to the rest