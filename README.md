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

- Resources can override the properties that you declare in the Globals section.

- But the resource cannot remove a property that's specified in the Globals section.

-  If some resources use a property but others don't, then you must not declare them in the Globals section.

##### Primitive data types are replaced

Primitive data types include strings, numbers, Booleans, and so on.
The value specified in the Resources section replaces the value in the Globals section.

##### Maps are merged

Maps are also known as dictionaries or collections of key-value pairs.
Map entries in the Resources section are merged with global map entries. If there are duplicates, the Resource section entry overrides the Globals section entry.

##### Lists are additive

Lists are also known as arrays.
List entries in the Globals section are prepended to the list in the Resources section.


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

##### AWS::Serverless::Api

```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: AWS SAM template with a simple API definition
Resources:
  ApiGatewayApi:
    Type: AWS::Serverless::Api
    Properties:
      StageName: prod
  ApiFunction: # Adds a GET api endpoint at "/" to the ApiGatewayApi via an Api event
    Type: AWS::Serverless::Function
    Properties:
      Events:
        ApiEvent:
          Type: Api
          Properties:
            Path: /
            Method: get
            RestApiId:
              Ref: ApiGatewayApi
      Runtime: python3.7
      Handler: index.handler
      InlineCode: |
        def handler(event, context):
            return {'body': 'Hello World!', 'statusCode': 200}

```

#### Serverless - Function - URL

Provision Lambda Function with public URL

[serverless-function-url](serverless-function-url.yaml)
