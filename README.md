# core-account-cleanroom-create
Create a new, empty mytiki data cleanroom

## API
Request (JSON)

- **requestId**: A unique identifier (uuid) for the transaction.
- **account**: The aws account id to share the cleanroom with.
- **database**: The unique identifier for the cleanroom, follow database naming conventions —lowercase snakecase. Must start with a letter, not a number or symbol. 

```json
{
  "requestId": "string",
  "account": "string",
  "database": "string"
}
```
Response (JSON) - `/api/latest/ocean/cleanroom/create`

- **requestId**: The unique identifier for the transaction.

```json
{
  "requestId": "string"
}
```

Error Response (JSON) - `/api/latest/ocean/error`

- **requestId**: The unique identifier for the transaction.
- **error**: An optional error if the transaction failed.
  - **message**: A description of the error event.
  - **cause**: Additional details describing what created the error event.

```json
{
  "requestId": "string",
  "error": {
    "message": "string",
    "cause": "string"
  }
}
```

## Configuration
Standard SAM CLI flags for build, package, and deploy phases can be provided via command line flags using `"flags="<config>"`.

**example**: `make build flags="--profile aws_profile"`

Standard configuration parameters are provided the same way using the `--parameter-overrides` flag.

### Required
- **ApiAuthorization**: The EventBridge connection id (i.e. name/id)

### Optional
- **Name**: The service name (lowercase pipecase). Default is core-account-cleanroom-create.
- **Workgroup**: The athena workgroup to use. Default is primary.
- **Bucket**: The S3 bucket to host the cleanroom. Default is mytiki-ocean
- **ApiEndpoint**: The base http endpoint to receive the result (without https:// prefix). Default is account.mytiki.com

## Build and Deploy

### Dependencies
- [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html) for step function deployment.
- [JQ](https://jqlang.github.io/jq/) for composite JSON assembly.
- [S3 Buckets](https://aws.amazon.com/s3/) for uploading cloudformation template and hosting the cleanroom.
- [AWS Athena](https://aws.amazon.com/athena/) for glue database creation.
- [AWS Glue](https://aws.amazon.com/glue/) for database metadata catalog.
- [AWS LakeFormation](https://aws.amazon.com/lake-formation/) for database access control.
- [AWS Step Functions](https://aws.amazon.com/step-functions/) for serverless event execution.

### Compile
The project uses a composite JSON assembled at compile time.

```bash
make compile
```

### Build
Requires compile step. At build time the cloudformation template is assembled and validate

```bash
make build flags="--parameter-overrides ApiAuthorization=<xyz/123> LocationRole=<abc>"
```

### Package
Requires compile and build steps.

```bash
make package
```

### Deploy
Requires compile, build, and package steps.

```bash
make deploy flags="--parameter-overrides ApiAuthorization=<xyz/123> LocationRole=<abc>"
```
