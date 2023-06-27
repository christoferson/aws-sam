
#### AWS::Serverless::Function [link](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-function.html)

##### Basic Example

Basic example using zip

```
Type: AWS::Serverless::Function
Properties:
  Handler: index.handler
  Runtime: python3.9
  CodeUri: s3://bucket-name/key-name
```

Contains a Lambda Function with an API Endpoint

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

