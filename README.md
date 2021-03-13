# AWS Lambda com Quarkus (e Serverless Framework)

Função Lambda simples usando Quarkus e Java 11. A função pode ser construída e implementada em JVM e também em modo nativo. Implantação via script de shell ou estrutura sem servidor.

Mais detalhes aqui: https://quarkus.io/guides/amazon-lambda

A função não pode ser chamada via HTTP devido a um erro desconhecido do Cloud Front.

## Preparação

Precisamos criar uma função de execução do AWS Lambda usando o AWS CLI.

```
$ aws iam create-role --role-name lambda-execution --assume-role-policy-document file: //trust-policy.json
$ aws iam attach-role-policy --role-name lambda-execute --policy-arn arn: aws: iam :: aws: policy / service-role / AWSLambdaBasicExecutionRole

$ export LAMBDA_ROLE_ARN = " arn: aws: iam :: 450802564356: função / execução de lambda "
```

## Função de construção e implantação (modo JVM)

```
$ ./gradlew build
$ sam local invoke --template build / sam.jvm.yaml --event payload.json

$ export LAMBDA_ROLE_ARN = " arn: aws: iam :: 450802564356: função / execução de lambda "
$ sh build / manage.sh create

$ cp payload.json build /
$ sh build / manage.sh invoke

# using Serverless framework 
# in serverless.yaml o runtime deve ser definido para java11
$ serverless deploy
$ serverless invoke
```

## Função de construção e implantação (modo nativo)

```
$ ./gradlew build -Dquarkus.package.type = native -Dquarkus.native.container-build = true
$ sam local invoke --template build / sam.native.yaml --event payload.json

$ export LAMBDA_ROLE_ARN = " arn: aws: iam :: 450802564356: função / execução de lambda "
$ sh build / manage.sh native create

$ cp payload.json build /
$ sh build / manage.sh invoke

# usando a estrutura sem servidor 
# em serverless.yaml, o tempo de execução deve ser definido como fornecido
$ serverless deploy
$ serverless invoca --function serverlessQuarkus --log
```