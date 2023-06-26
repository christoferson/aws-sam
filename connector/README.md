
#### AWS::Serverless::Connector [link](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-api.html)

contains a Lambda Function with an API endpoint

```
Transform: AWS::Serverless-2016-10-31
...
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Connectors:
      MyConn:
        Properties:
          Destination:
            Id: MyTable
          Permissions:
            - Read
            - Write
  MyTable:
    Type: AWS::DynamoDB::Table

```

