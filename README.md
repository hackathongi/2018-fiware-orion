# 2018-fiware-orion
Cervell central del #hackathonGi 2018 basat en en [Fiware Context Broker](https://github.com/telefonicaid/fiware-orion)

# Docs

## Probar connectivitat

El servidor está executant-se a la IP pública 84.89.60.4 port 80. Per comprobar la connectivitat:

```bash
$ curl http://84.89.60.4/version

{
"orion" : {
  "version" : "1.11.0-next",
  "uptime" : "0 d, 0 h, 12 m, 30 s",
  "git_hash" : "781d3b621e88a5124be8c6ae3f57a88a3dcc160a",
  "compile_time" : "Wed Feb 14 11:09:08 UTC 2018",
  "compiled_by" : "root",
  "compiled_in" : "88eb02e35773",
  "release_date" : "Wed Feb 14 11:09:08 UTC 2018",
  "doc" : "https://fiware-orion.readthedocs.org/en/master/"
}
}
```

## Registrar entitat

## Actualitzar estat d'una entitat

## Subscriure's a canvis d'estats

