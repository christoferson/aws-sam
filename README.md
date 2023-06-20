# aws-sam

[Official Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)

### AWS SAM template anatomy | [link](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-template-anatomy.html)

#### Transform declaration (required)
The declaration Transform: AWS::Serverless-2016-10-31 is required for AWS SAM template files. 

#### Globals section (optional)
The Globals section is unique to AWS SAM. It defines properties that are common to all your serverless functions and APIs. All the AWS::Serverless::Function, AWS::Serverless::Api, and AWS::Serverless::SimpleTable resources inherit the properties that are defined in the Globals section.

#### Resources section (required)
In AWS SAM templates the Resources section can contain a combination of AWS CloudFormation resources and AWS SAM resources.

#### Parameters section (optional)
Objects that are declared in the Parameters section cause the sam deploy --guided command to present additional prompts to the user.

#### Skeleton

```
Transform: AWS::Serverless-2016-10-31

Globals:
  set of globals
Description:
  String
Metadata:
  template metadata
Parameters:
  set of parameters
Mappings:
  set of mappings
Conditions:
  set of conditions
Resources:
  set of resources
Outputs:
  set of outputs
```

#### Globals

##### Globals Example

```
Globals:
  Function:
    Runtime: nodejs12.x
    Timeout: 180
    Handler: index.handler
    Environment:
      Variables:
        TABLE_NAME: data-table

Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      Environment:
        Variables:
          MESSAGE: "Hello From SAM"

  ThumbnailFunction:
    Type: AWS::Serverless::Function
    Properties:
      Events:
        Thumbnail:
          Type: Api
          Properties:
            Path: /thumbnail
            Method: POST
```

##### Global Template

```
Globals:
  Function:
    Handler:
    Runtime:
    CodeUri:
    DeadLetterQueue:
    Description:
    MemorySize:
    Timeout:
    VpcConfig:
    Environment:
    Tags:
    Tracing:
    KmsKeyArn:
    Layers:
    AutoPublishAlias:
    DeploymentPreference:
    PermissionsBoundary:
    ReservedConcurrentExecutions:
    ProvisionedConcurrencyConfig:
    AssumeRolePolicyDocument:
    EventInvokeConfig:
    Architectures:
    EphemeralStorage:

  Api:
    Auth:
    Name:
    DefinitionUri:
    CacheClusterEnabled:
    CacheClusterSize:
    Variables:
    EndpointConfiguration:
    MethodSettings:
    BinaryMediaTypes:
    MinimumCompressionSize:
    Cors:
    GatewayResponses:
    AccessLogSetting:
    CanarySetting:
    TracingEnabled:
    OpenApiVersion:
    Domain:

  HttpApi:
    Auth:
    AccessLogSettings:
    StageVariables:
    Tags:

  SimpleTable:
    SSESpecification:
```
##### Implicit API
  AWS SAM creates implicit APIs when you declare an API in the Events section. You can use Globals to override all properties of implicit APIs.

#### Overridable properties
-------

Example SAM Template

```
AWSTemplateFormatVersion: 2010-09-09
Transform: AWS::Serverless-2016-10-31
Resources:
  getAllItemsFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: src/get-all-items.getAllItemsHandler
      Runtime: nodejs12.x
      Events:
        Api:
          Type: HttpApi
          Properties:
            Path: /
            Method: GET
    Connectors:
      MyConn:
        Properties:
        Destination:
          Id: SampleTable
          Permissions:
            - Read
  SampleTable:
    Type: AWS::Serverless::SimpleTable
    
```

SAM CLI

#### Initialize a new project 
sam init

#### Build your application for deployment
sam build

Perform local debugging and testing
sam local invoke

Deploy your application
sam deploy --guided

Configure CI/CD deployment pipelines
sam pipeline init --bootstrap

Monitor and troubleshoot your application in the cloud
sam list

Sync local changes to the cloud as you develop
sam sync --watch



