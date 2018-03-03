# Aixecar servei fiware orion

$docker-compose up

# References
http://fiware-orion.readthedocs.io/en/latest/quick_start_guide/
https://fiware-orion.readthedocs.io/en/master/user/walkthrough_apiv2/index.html

# Token 
# wget --no-check-certificate https://raw.githubusercontent.com/fgalan/oauth2-example-orion-client/master/token_script.sh
    bash token_script.sh

# Set AUTH_TOKEN environment variable
    AUTH_TOKEN='OhTQlD3qDmi6o1k7Z5JmQPVxY1gUdL'

Ask for a longer expire periode for the token 
    
# Orion
http://130.206.120.79:1026/version

# Identify the Orion version
    curl http://130.206.120.79:1026/version \
       -X GET -s -S --header 'Accept: application/json' \
       --header  "X-Auth-Token: $AUTH_TOKEN" | python -mjson.tool

# Generate random unique ID
    RANDOM_NUMBER=$(cat /dev/urandom | tr -dc '0-9' | fold -w 10 | head -n 1)
    ID=MyEntity-$RANDOM_NUMBER
    echo $ID

# Create entity
curl http://130.206.120.79:1026/v2/entities -X POST -s -S \
    --header 'Content-Type: application/json' \
    --header "X-Auth-Token: $AUTH_TOKEN" -d @- <<EOF
{
    "id": "$ID",
    "type": "User",
    "city_location": {
    "value": "Girona",
    "type": "City"
    },
    "temperature": {
    "value": 23.8,
    "type": "Number"
    }
}
EOF

# Create entity
RANDOM_NUMBER=$(cat /dev/urandom | tr -dc '0-9' | fold -w 10 | head -n 1)
ID=MyEntity-$RANDOM_NUMBER
echo $ID

curl http://130.206.120.79:1026/v2/entities -X POST -s -S \
    --header 'Content-Type: application/json' \
    -d @- <<EOF
{
    "id": "$ID",
    "type": "User",
    "city_location": {
    "value": "Girona",
    "type": "City"
    },
    "temperature": {
    "value": 23.8,
    "type": "Number"
    }
}
EOF

# Check entity
curl http://130.206.120.79:1026/v2/entities/$ID -X GET -s -S \
--header 'Accept: application/json'\
--header "X-Auth-Token: $AUTH_TOKEN" | python -mjson.tool

curl http://130.206.120.79:1026/v2/entities/$ID -X GET -s -S \
--header 'Accept: application/json'\
| python -mjson.tool
    
# Modify entity
curl http://130.206.120.79:1026/v2/entities/$ID/attrs/temperature \
    -X PUT -s -S --header  'Content-Type: application/json' \
    --header "X-Auth-Token: $AUTH_TOKEN" -d @- <<EOF
{
    "value": 18.4,
    "type": "Number"
}
EOF

# Check new value entity
    curl http://130.206.120.79:1026/v2/entities/$ID -X GET -s -S \
        --header 'Accept: application/json'\
        --header "X-Auth-Token: $AUTH_TOKEN" | python -mjson.tool


The basic patterns for all the curl examples in this document are the following:

# For POST:
    curl localhost:1026/<operation_url> -s -S [headers]' -d @- <<EOF
    [payload]
    EOF

# For PUT:
    curl localhost:1026/<operation_url> -s -S [headers] -X PUT -d @- <<EOF
    [payload]
    EOF

# For PATCH:
    curl localhost:1026/<operation_url> -s -S [headers] -X PATCH -d @- <<EOF
    [payload]
    EOF

# For GET:
    curl localhost:1026/<operation_url> -s -S [headers]

# For DELETE:
    curl localhost:1026/<operation_url> -s -S [headers] -X DELETE

Regarding [headers] you have to include the following ones:

# Accept header to specify which payload format you want to receive in the response. You should explicitly specify JSON.
    curl ... --header 'Accept: application/json' ...

# Only in the case of using payload in the request (i.e. POST, PUT or PATCH), you have to use Context-Type header to specify the format (JSON).
    curl ... --header 'Content-Type: application/json' ...

Some additional remarks:

* Most of the time we are using multi-line shell commands to provide the input to curl, using EOF to mark the beginning and the end of the multi-line block (here-documents). In some cases (GET and DELETE) we omit -d @- as they don't use payload.

* In our examples we assume that the broker is listening on port 1026. Adjust this in the curl command line if you are using a different setting.

* In order to pretty-print JSON in responses, you can use Python with msjon.tool (examples along with tutorial are using this style):

    (curl ... | python -mjson.tool) <<EOF
    ...
    EOF
    

curl $HGIORION:1026/v2/entities -s -S --header 'Content-Type: application/json' -d @- <<EOF
{
  "id": "Room1",
  "type": "Room",
  "temperature": {
    "value": 23,
    "type": "Float"
  },
  "pressure": {
    "value": 720,
    "type": "Integer"
  }
}
EOF

# Check entity
curl $HGIORION:1026/v2/entities/Room1?type=Room -X GET -s -S \
    --header 'Accept: application/json'\
    --header "X-Auth-Token: $AUTH_TOKEN" | python -mjson.tool
    
# Request KeyValues
curl $HGIORION:1026/v2/entities/Room1?options=keyValues -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Request values and attrs
curl 'http://130.206.120.79:1026/v2/entities/Room1?options=values&attrs=temperature,pressure' -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Request attrs list
curl 'http://130.206.120.79:1026/v2/entities/Room1?options=values&attrs=pressure,temperature' -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Request single attrs
curl $HGIORION:1026/v2/entities/Room1/attrs/temperature -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Request only the value
curl $HGIORION:1026/v2/entities/Room1/attrs/temperature/value -s -S  \
    --header 'Accept: text/plain' | python -mjson.tool

# Error entity
curl $HGIORION:1026/v2/entities/Room5 -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Error attrs 
curl $HGIORION:1026/v2/entities/Room1/attrs/humidity -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# GET /v2/entities
curl $HGIORION:1026/v2/entities -s -S --header 'Accept: application/json' | python -mjson.tool

# GET /v2/entities by type
curl $HGIORION:1026/v2/entities?type=Room -s -S  \
    --header 'Accept: application/json' | python -mjson.tool

# Short update
curl $HGIORION:1026/v2/entities/Room1/attrs/temperature/value -s -S \
    --header 'Content-Type: text/plain' \
    -X PUT -d 28.5

# Formal update
# double check
curl $HGIORION:1026/v2/entities/Room1/attrs -s -S --header 'Content-Type: application/json' \
    -X PATCH -d @- <<EOF
{
  "temperature": {
    "value": 26.5,
    "type": "Float"
  },
  "pressure": {
    "value": 763,
    "type": "Float"
  }
}
EOF

    
