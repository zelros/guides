<img src="./imgs/zelros.svg" width="250">


# 100: Think as Business API

## Human API

API are written for Human and reflect the business.

Think API as [microservice](https://en.wikipedia.org/wiki/Microservices):
- it is organized around a single domain capability
- it has a well-defined external interface
- it is independently deployable

The domain is very important. It must be:
- not to small, to avoid multiplication of microservices
- not to big, to avoid a monolith application.

Furthermore, API should adopt a [KISS principle](https://en.wikipedia.org/wiki/KISS_principle).


## Methodology

1. Define usecase Key Services for your business (usecase)
2. Identify key services (aka endpoint)
3. Design an API contract with Swagger
4. Develop a mockup API to quickly interact with applications
