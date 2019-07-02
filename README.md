# README

## Apagar

ao iniciar o projeto, podemos apagar os diretórios em _src/main/resources_:

* static

* templates

Sobre os arquivos de configuração:

* application.properties: configurações do Spring que podem ser substituídas.

* bootstrap.properties: arquivo utilizado para configuração dentro do ecosistema do spring cloud. Podemos definir um nome para identificar uma aplicação dentro do sprint cloud.

## Config Server

Colocar a anotação __@EnableConfigServer__ na classe principal.

Ao configurarmos o repositorio onde estarão as configurações, podemos iniciar o serviço o chamar na url pela configuração que queremos.

No repositório

```
https://github.com/demo-spring-cloud/demo-config
```

Existem os arquivos:

```
demo-config-server-dev.properties
demo-config-server-prd.properties
sample-config-app.properties
```

Para conseguirmos acessar cada um deles, fazemos a chamada no modelo:

```
http://hostname:port/[config-file-name]/[profile]
```

Sendo assim, podemos fazer as seguintes chamadas:

```
http://localhost:9090/demo-config-server/dev
http://localhost:9090/demo-config-server/prd
http://localhost:9090/sample-config-app/anything
```

Interessante observar que no caso de ter apenas um arquivo para uma configuração (_sample-config-app.properties_), não precisamos definir um profile exato.
Isso quer dizer que se passarmos qualquer String, ele vai retornar o único arquivo que existe. Se colocarmos '__batata__', vai funcionar

Exemplo de resposta:

```json
{
    "name": "sample-config-app",
    "profiles": [
        "batata"
    ],
    "label": null,
    "version": "4e492e35f0621c838d6d2171cfac09e25a85629b",
    "state": null,
    "propertySources": [
        {
            "name": "https://github.com/demo-spring-cloud/demo-config/sample-config-app.properties",
            "source": {
                "container.min": "1",
                "container.max": "5"
            }
        }
    ]
}
```

No caso de existir mais de um arquivo, é necessário que exista uma distinção por profile.
Podemos ver no exemplo que estão definidos por ambiente (__dev/prd__) e na URI de chamada definimos qual o profile que queremos.

Exemplo de resposta:

```json
{
    "name": "demo-config-server",
    "profiles": [
        "dev"
    ],
    "label": null,
    "version": "4e492e35f0621c838d6d2171cfac09e25a85629b",
    "state": null,
    "propertySources": [
        {
            "name": "https://github.com/demo-spring-cloud/demo-config/demo-config-server-dev.properties",
            "source": {
                "container.min": "2",
                "container.max": "6"
            }
        }
    ]
}
```

```json
{
    "name": "demo-config-server",
    "profiles": [
        "prd"
    ],
    "label": null,
    "version": "4e492e35f0621c838d6d2171cfac09e25a85629b",
    "state": null,
    "propertySources": [
        {
            "name": "https://github.com/demo-spring-cloud/demo-config/demo-config-server-prd.properties",
            "source": {
                "container.min": "5",
                "container.max": "10",
                "provider.endpoint": "https://hostname/some/uri/parameters",
                "provider.timeout": "5000"
            }
        }
    ]
}
```