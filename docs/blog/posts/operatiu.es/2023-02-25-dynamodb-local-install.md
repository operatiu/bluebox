---
title: 'Dynamodb - local install'
date: 
    created: 2023-02-25

author: Joan M.

categories:
    - Informàtica
    - Portada
tags:
    - aws
    - aws-shell
    - dynamodb
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1734029719/dynamodb-icon_jr56vx.png "Imatge inicial entrada")

Per a aquesta instal·lació, farem servir una màquina virtual amb Debian 11 i 2.5GB de RAM

<!-- more -->

### Java

Necessitem instal·lar Java Runtime Environment (JRE)

```shell
sudo apt install openjdk-11-jre
```

Afegirem java path al final de l’arxiu

```shell
sudo nano /etc/bash.bashrc
```

```shell
export PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin:$PATH
```

### DynamoDB

Descarreguem la darrera versió de dynamodb

```shell
wget https://s3.ap-south-1.amazonaws.com/dynamodb-local-mumbai/dynamodb_local_latest.tar.gz
```

#### Instal·lació

Crearem un directori en /usr/lib/ de nom dynamodb on situarem l’arxiu descarregat i el descomprimirem

```shell
sudo mkdir -p /usr/lib/dynamodb
sudo mv dynamodb_local_latest.tar.gz /usr/lib/dynamodb/
cd /usr/lib/dynamodb/
sudo tar xfz dynamodb_local_latest.tar.gz
```

Podem comprovar el funcionament amb:

```shell
sudo java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
```

Per a més comoditat farem que sigui un servei gestionable

```shell
sudo nano /etc/systemd/system/dynamodb.service
```

El contingut de l’arxiu creat, serà el següent:

```shell
[Unit]
Description=Dynamo DB Local Service
[Service]
Type=simple
WorkingDirectory=/usr/lib/dynamodb
ExecStart=/usr/lib/dynamodb/dynamodb
SuccessExitStatus=143
TimeoutStopSec=10
Restart=on-failure
RestartSec=5
KillMode=process
KillSignal=SIGINT
[Install]
WantedBy=multi-user.target
```

Crearem l’arxiu executable:

```shell
sudo nano /usr/lib/dynamodb/dynamodb
```

Li donem permís d’execució

```shell
sudo chmod 744 dynamodb
```

Afegim la comanda que haviem provat en un principi, de forma manual.

```bash
#!/bin/bash
sudo java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
```

Registrem i habilitem el servei: /etc/systemd/system/dynamodb.service

```shell
sudo systemctl daemon-reload
sudo systemctl enable dynamodb
```

Initzialitzem i comprovem el servei

```shell
sudo systemctl start dynamodb
sudo systemctl status dynamodb
```

#### Consola aws

Instal·lació de la consola aws

```shell
sudo pip install --upgrade aws-shell
```

Per entrar

```shell
aws-shell
```

Per sortir

```shell
.quit o .exit
```

##### Configurar aws

En aquest cas, treballarem en local, així doncs no serà necessari introduïr una regió aws real, como ara ‘**eu-south-2**‘ o altres

```shell
aws> configure

AWS Access Key ID [None]: usuari
AWS Secret Access Key [None]: passwdUsuari
Default region name [None]: local
Default output format [None]:
```

Aquesta informació es guardarà als arxius: ~/.aws/config (region) i ~/.aws/credentials (usuari i contrasenya)

Podem provar a entrar amb:

```shell
WS_DEFAULT_REGION=local AWS_ACCESS_KEY_ID=usuari AWS_SECRET_ACCESS_KEY=passwdUsuari aws dynamodb list-tables --endpoint-url http://localhost:8000
```

És important afegir **–endpoint-url http://localhost:8000** ja que estem accedint en local  
Com hem creat els paràmetres de configuració d’usuari, també podem accedir de forma més ràpida amb la comanda:

```shell
aws dynamodb list-tables --endpoint-url http://localhost:8000
```

El que ens hauria de mostrar un llistat buit de taules

```sql
{
    "TableNames": [ ]
}
```

Una altra opció de configuració, és la de crear un perfil i fer-lo servir

```shell
aws configure --profile proot
AWS Access Key ID [None]: root
AWS Secret Access Key [None]: passwdRoot
Default region name [None]: local
Default output format [None]:
```

#### Crear una taula i importar un arxiu .json

La mateixa documentació d’AWS ens mostra el següent exemple:

```shell
aws --profile proot --endpoint-url http://localhost:8000 dynamodb create-table \
    --table-name GameScores \
    --attribute-definitions AttributeName=UserId,AttributeType=S AttributeName=GameTitle,AttributeType=S AttributeName=TopScore,AttributeType=N AttributeName=Date,AttributeType=S \
    --key-schema AttributeName=UserId,KeyType=HASH AttributeName=GameTitle,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=5 \
    --global-secondary-indexes file://gsi.json
```

[ [gsi.json](https://drive.google.com/file/d/1Kdw2s17bzz-a4Od5RfukP96z67JGiqW4/view?usp=sharing) ]

Podem comprovar el llistat de taules novament

```shell
aws --profile proot dynamodb list-tables --endpoint-url http://localhost:8000

{
    "TableNames": [
        "GameScores"
    ]
}
```

O veure la seva estructura i contingut

```shell
aws --profile proot dynamodb describe-table --table-name GameScores --endpoint-url http://localhost:8000
```

```shell
{
    "Table": {
        "AttributeDefinitions": [
            {
                "AttributeName": "UserId",
                "AttributeType": "S"
            },
            {
                "AttributeName": "GameTitle",
                "AttributeType": "S"
            },
            {
                "AttributeName": "TopScore",
                "AttributeType": "N"
            },
            {
                "AttributeName": "Date",
                "AttributeType": "S"
            }
        ],
        "TableName": "GameScores",
        "KeySchema": [
            {
                "AttributeName": "UserId",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "GameTitle",
                "KeyType": "RANGE"
            }
        ],
        "TableStatus": "ACTIVE",
        "CreationDateTime": 1677344659.024,
        "ProvisionedThroughput": {
            "LastIncreaseDateTime": 0.0,
            "LastDecreaseDateTime": 0.0,
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 10,
            "WriteCapacityUnits": 5
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:ddblocal:000000000000:table/GameScores",
        "GlobalSecondaryIndexes": [
            {
                "IndexName": "GameDateIndex",
                "KeySchema": [
                    {
                        "AttributeName": "GameTitle",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "Date",
                        "KeyType": "RANGE"
                    }
                ],
                "Projection": {
                    "ProjectionType": "ALL"
                },
                "IndexStatus": "ACTIVE",
                "ProvisionedThroughput": {
                    "ReadCapacityUnits": 5,
                    "WriteCapacityUnits": 5
                },
                "IndexSizeBytes": 0,
                "ItemCount": 0,
                "IndexArn": "arn:aws:dynamodb:ddblocal:000000000000:table/GameScores/index/GameDateIndex"
            },
            {
                "IndexName": "GameTitleIndex",
                "KeySchema": [
                    {
                        "AttributeName": "GameTitle",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "TopScore",
                        "KeyType": "RANGE"
                    }
                ],
                "Projection": {
                    "ProjectionType": "ALL"
                },
                "IndexStatus": "ACTIVE",
                "ProvisionedThroughput": {
                    "ReadCapacityUnits": 10,
                    "WriteCapacityUnits": 5
                },
                "IndexSizeBytes": 0,
                "ItemCount": 0,
                "IndexArn": "arn:aws:dynamodb:ddblocal:000000000000:table/GameScores/index/GameTitleIndex"
            }
        ]
    }
}
```

### Bibliografia

[https://github.com/awslabs/aws-shell](https://github.com/awslabs/aws-shell)

[https://docs.aws.amazon.com/es\_es/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html](https://docs.aws.amazon.com/es_es/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

[http://docs.aws.amazon.com/cli/latest/reference/dynamodb/create-table.html](http://docs.aws.amazon.com/cli/latest/reference/dynamodb/create-table.html)

[https://docs.aws.amazon.com/cli/latest/userguide/cli-services-dynamodb.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-dynamodb.html)
