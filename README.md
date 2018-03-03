# 2018-fiware-orion
Cervell central del #hackathonGi 2018 basat en en [Fiware Context Broker](https://github.com/telefonicaid/fiware-orion)

## Ús

Es pot aixecar una instància de fiware orion amb docker:

```bash
$ docker-compose up
```

## Documentació API

El Notebook [README.ipynb](README.ipynb) conté exemples de comunicació amb Fiware Orion

### Per llistar entities:

```bash
$ curl http://84.89.60.4/v2/entities
```

### Per actualitzar el valor d'un atribut

```bash
$ curl http://84.89.60.4/v2/entities/bressol/attrs -s -S --header 'Content-Type: application/json' \
     -X PATCH -d @- <<EOF
{
  "estat": {
    "value": "off",
    "type": "String"
  }
}
EOF
```

amb una sola línea:

```bash
$ curl http://84.89.60.4/v2/entities/bressol/attrs -s -S --header 'Content-Type: application/json' -X PATCH -d "{\"estat\": {\"value\": \"off\",\"type\": \"String\"}}"
```

### Per subscriure's als canvis d'atribut d'una entitat

```bash
curl -v http://84.89.60.4/v2/subscriptions -s -S --header 'Content-Type: application/json' \
    -d @- <<EOF
{
  "description": "Subscripció de so per activar la cuna",
  "subject": {
    "entities": [
      {
        "id": "bressol",
        "type": "bebe"
      }
    ],
    "condition": {
      "attrs": [
        "estat"
      ]
    }
  },
  "notification": {
    "http": {
      "url": "http://192.168.4.28"
    },
    "attrs": [
      "estat"
    ]
  },
  "expires": "2040-01-01T14:00:00.00Z",
  "throttling": 1
}
EOF
```

## Enllaços externs

http://fiware-orion.readthedocs.io/en/latest/quick_start_guide/
https://fiware-orion.readthedocs.io/en/master/user/walkthrough_apiv2/index.html

