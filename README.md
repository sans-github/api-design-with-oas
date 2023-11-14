# API Prototyping

Tools for collaboration on API design and rapid prototyping.

## Design phase

1. Create OAS spec YAML. For example -

* [planets](https://raw.githubusercontent.com/lornajane/flask-planets-and-webhooks/master/openapi.yaml)
* [petstore](https://raw.githubusercontent.com/OAI/OpenAPI-Specification/main/examples/v3.0/petstore.yaml)

## Rapid protytpying and test

1. Generate swagger
[OpenAPI Spec to Swagger](https://petstore.swagger.io/)

2. Create mock server from the spec to test. Docker-compose
```Docker
version: "3.9"

networks:
  elastic:
    external: true

services:
  openapi_mock:
    container_name: openapi_mock
    image: muonsoft/openapi-mock
    volumes:
     - ${DOCKER_VOLUME}/openapi:/etc/openapi
    environment:
      OPENAPI_MOCK_SPECIFICATION_URL: https://raw.githubusercontent.com/lornajane/flask-planets-and-webhooks/master/openapi.yaml
    ports:
      - "8080:8080"
```

3. Using Visual Stuido code for testing
[REST client plugin](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
``` VS REST client
# @name post_company
POST {{host}}/companies
Authorization: {{base64}}
Accept: application/json
Content-Type: application/json

{
  "name": "SMB company name II",
  "description": "Any additional information about the company"
}

###
@company_id = {{post_company.response.body.id}}
@redirect = {{post_company.response.body.redirect}}

# @name echo_post_company
# GET http://httpbin.org/get?company_id={{company_id}}&redirect={{redirect}}

###
# @name get_company
GET {{host}}/companies
Authorization: {{base64}}
Accept: application/json
```
