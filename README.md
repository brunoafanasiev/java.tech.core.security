# Sumário
- [Sumário](#sumário)
- [Introdução](#introdução)
- [Antes de começar](#antes-de-começar)
    - [Dependência do Spring Boot](#dependência-do-spring-boot)
- [Começando](#começando)
    - [Dependência do projeto](#dependência-do-projeto)
- [Segurança](#segurança)
    - [Configuração de segurança no formato yaml](#configuração-de-segurança-no-formato-yaml)
    - [Configuração de segurança no formato properties](#configuração-de-segurança-no-formato-properties)
- [Contribua](#contribua)

# Introdução
A _lib_ __Java Core Security__ tem o objetivo de agrupar uma série de funcionalidades de segurança comuns para aplicações _Java Spring_.

# Antes de começar
Antes de começar a usar, é importante ter em mente alguns detalhes sobre o suporte e dependências da _lib_.

A _Lib Core Security_ foi desenvolvida em conjunto com outros dois projetos: o projeto POM [Java Parent](https://github.com/brunoafanasiev/java.tech.parent) e o projeto POM [Java Dependencies](https://github.com/brunoafanasiev/java.tech.dependencies).

Para o usar a _Lib Core_, seu projeto maven deve ter como _parent_ o projeto [Java Parent](https://github.com/brunoafanasiev/java.tech.parent). Adicione o _parent_ no pom.xml do seu projeto, conforme exemplo abaixo:

```xml
<parent>
    <groupId>com.bruno.afanasiev</groupId>
    <artifactId>java.tech.parent</artifactId>
    <version>${java.parent.version}</version>
</parent>
```
Ao usar o _parent_ acima, todas as dependências necessárias para o uso da _Lib Core_ serão automaticamente resolvidas nas versões ideais para o funcionamento correto.

Ademais, a _Lib Core Security_ utiliza a [Lib Core Exception](https://github.com/brunoafanasiev/java.tech.core.exception), para implementar as funcionalidades de exceções para a aplicação.

## Dependência do Spring Boot
O projeto [Java Parent](https://github.com/brunoafanasiev/java.tech.parent) importa o projeto [Java Dependencies](https://github.com/brunoafanasiev/java.tech.dependencies), que por sua vez, usa a versão base do _Spring Boot_ e do _Spring Cloud_.

Principalmente para a versão do Spring Security, que é herdada a partir da versão gerenciada do _Spring Boot_, o cuidado em utilizar a versão correta do Spring garante o funcionamento da _Lib Core_ e seus componentes. O uso de outra versão do Spring Boot a partir de um _parent_ que não seja o [Java Parent](https://github.com/brunoafanasiev/java.tech.parent) **é de total responsabilidade do time de desenvolvimento e não existe garantia do funcionamento correto da _Lib Core_**.


# Começando
A partir daqui, você terá os detalhes de como usar a biblioteca em seu projeto.

## Dependência do projeto
Para usar a _Lib Core Security_ no seu pojeto, além de definir o _parent_ destacado acima, é necessário adicionar a dependência da _lib_ no `pom.xml` do seu projeto, conforme exemplo abaixo:
```xml
<dependency>
    <groupId>com.bruno.afanasiev</groupId>
    <artifactId>java.tech.core.security</artifactId>
    <version>${java.core.security.version}</version>
</dependency>
```



# Segurança

Na _Lib Core Security_ o suporte a autenticação baseada em JWT por padrão. O suporte foi pensado de modo genérico, para que seja possível trabalhar com qualquer IDP (_Identity Provider_). Entretanto, os IDPs usados devem servir por padrão o suporte a [OpenID Connect 1.0](https://openid.net/connect/) ou [OAuth 2.0](https://oauth.net/2/) e, como resultado, emitirem um JWT após o processo de autenticação do usuário.

Abaixo estão os documentadas as propriedades disponíveis para usar a segurança da _Lib Core Security_.

| Propriedade                                                                    | Descrição                                                                                                                 | Valor padrão                        | Obrigatório                                                           |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|-------------------------------------|-----------------------------------------------------------------------|
| tech.security.auth.enabled                                                     | Liga ou desliga o mecanismo de segurança                                                                                  | `true`                              | Sim                                                                   |
| tech.security.auth.jwt.issuer-uri                                              | URI do emissor (Auth Server) de tokens                                                                                    | Não possui                          | Depende                                                               |
| tech.security.auth.jwt.jwk-set-uri                                             | URI do JWK Set do emissor (Auth Server) de tokens.                                                                        | Não possui                          | Depende                                                               |
| tech.security.auth.jwt.public-key.verification-key                             | Chave pública em formato PEM a ser usada na validação de assinatura do JWT                                                | Não possui                          | Depende                                                               |
| tech.security.auth.jwt.public-key.key-factory-algorithm                        | Algoritmo a ser utilizado na criação da instância da chave pública                                                        | RSA                                 | Não                                                                   |
| tech.security.auth.jwt.public-key.signature-algorithm                          | Algoritmo que será utilizado para validação da assinatura do JWT                                                          | RS256                               | Não                                                                   |
| tech.security.auth.jwt.principal-claim-name                                    | Nome da _claim_ do JWT da qual o _Principal_ será extraído para ser utilizado no contexto de segurança do Spring Security | preferred_username                  | Não                                                                   |
| tech.security.auth.jwt.roles-claim-path                                        | Caminho das claims a ser seguido pelo Spring Security para a extração do _array_ de _roles_ (RBAC) do JWT                 | resource_access.\<client-id\>.roles | Não                                                                   |
| tech.security.auth.jwt.scopes-claims                                           | Nome das _claims_ do JWT de onde os _scopes_ serão extraídos                                                              | scope e scp                         | Não                                                                   |
| tech.security.auth.resource-server.client-id                                   | _Client ID_ do _Resource Server_                                                                                          | Não possui                          | Não                                                                   |
| tech.security.auth.resource-server.client-secret                               | _Client secret_ do _Resource Server_                                                                                      | Não possui                          | Não                                                                   |
| tech.security.auth.actuator-roles-allowed                                      | Nome das _roles_ que serão utilizadas para proteger os endpoints sensíveis do Spring Boot Actuator                        | Actuator.Monitor e ActuatorMonitor  | Não                                                                   |
| tech.security.auth.custom-header                                               | _Header_ alternativo para extração do _Bearer Token_ (JWT) da requisição HTTP de entrada                                  | Authorization                       | Não                                                                   |
| tech.security.auth.authorization.\<authz-id\>.resource-paths.paths             | Caminhos da APIs do _Resource Server_ onde serão aplicadas as regras de autorização                                       | Não possui                          | Não                                                                   |
| tech.security.auth.authorization.\<authz-id\>.resource-paths.http-methods      | Método/verbo HTTP dos paths informados para o qual a regra de autorização será aplicada                                   | Não possui                          | Não. Se não informado, será aplicado a todos os métodos (verbos) HTTP |
| tech.security.auth.authorization.\<authz-id\>.roles-alowed                     | _Roles_ que serão exigidas para acesso aos caminhos (_paths_) informados                                                  | Não possui                          | Não                                                                   |
| tech.security.auth.authorization.\<authz-id\>.scopes-alowed                    | _Scopes_ que serão exigidos para acesso aos caminhos (_paths_) informados                                                 | Não possui                          | Não                                                                   |
| tech.security.auth.authentication.\<integration-id\>.endpoint-uri              | URI da integração onde a autenticação será realizada automaticamente                                                      | Não possui                          | Não                                                                   |
| tech.security.auth.authentication.\<integration-id\>.auth-strategy             | Estratégia de autenticação a ser utilizada para a URI informada                                                           | send-received-jwt                   | Não                                                                   |
| tech.security.auth.authentication.\<integration-id\>.credentials.client-id     | _Client ID_ a ser utilizado para autenticação sistêmica na URI informada                                                  | Não possui                          | Não                                                                   |
| tech.security.auth.authentication.\<integration-id\>.credentials.client-secret | _Client secret_ a ser utilizada para autenticação sistêmica na URI informada                                              | Não possui                          | Não                                                                   |
| tech.security.auth.insecure-paths.\<insecure-path-id\>.paths                   | _Paths_ que serão liberados para acesso sem autenticação e autorização para a integração especificada                     | Não possui                          | Não                                                                   |
| tech.security.auth.insecure-paths.\<insecure-path-id\>.http-methods            | Método HTTP que será liberado para os _paths_ informados                                                                  | Não possui                          | Não                                                                   |

---
**NOTA:**
Se o componente de segurança estiver habilitado, pelo menos uma das propriedades `issuer-uri`, `jwk-set-uri` e `public-key.verification-key` deve estar preenchida.

---


As configurações podem ser realizadas no formato `yaml` ou `properties`. Abaixo estão alguns exemplos de configuração.

## Configuração de segurança no formato yaml
```yaml
tech:
  security:
    auth:
      enabled: true
      resource-server:
        client-id: ${spring.application.name}
      jwt:
        # Config básica apontando para o endereço do IDP.
        issuer-uri: https://accounts.google.com

      # Regras de autorização!
      authorization:
        write-filial:
          resource-paths:
            paths:
            - /filial/v1
            - /filial/v2
            http-methods: post, put, delete
          roles-alowed:
          - Admin.Access
          - Manager.Access
          scopes-alowed:
          - scope:write
          - scope:update
        read-filial: 
          resource-paths:
            paths:
            - /filial/v1
            - /filial/v2
          roles-alowed:
          - User.Access
          scopes-alowed:
          - scope:read
      
      insecure-paths:
        h2-database:
          paths:
          - /h2-console/**
        gets-filial-v2:
          paths:
          - /filial/v2/** 
          http-methods: get
```

## Configuração de segurança no formato properties
```properties
tech.security.auth.enabled=true
tech.security.auth.resource-server.client-id=${spring.application.name}

# Config básica apontando para o endereço do IDP.
tech.security.auth.jwt.issuer-uri=https://accounts.google.com

# Regras de autorização!
tech.security.auth.authorization.write-filial.resource-paths.paths=/filial/v1, /filial/v2
tech.security.auth.authorization.write-filial.resource-paths.http-methods=post, put, delete
tech.security.auth.authorization.write-filial.roles-alowed=Admin.Access, Manager.Access
tech.security.auth.authorization.write-filial.scopes-alowed=scope:write, scope:update
tech.security.auth.authorization.read-filial.resource-paths.paths=/filial/v1, /filial/v2
tech.security.auth.authorization.read-filial.roles-alowed=User.Access
tech.security.auth.authorization.read-filial.scopes-alowed=scope:read

# Regras de liberação!
tech.security.auth.insecure-paths.h2-database.paths=/h2-console/**
tech.security.auth.insecure-paths.gets-filial-v2.paths=/filial/v2/**
tech.security.auth.insecure-paths.gets-filial-v2.http-methods=get
```
