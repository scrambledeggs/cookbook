# API Consolidation Documentation Policy for Services

## Objective:

To ensure that API documentation is created, maintained, and regularly updated to provide clear and accurate information about the services offered. This policy outlines the steps for creating, updating, and uploading API documentation to an S3 bucket using GitHub workflows.

## Policy:

### 1. Documentation Creation

- API documentation must be written for all services.
- It should follow **OpenAPI (Swagger)** or another relevant standard for consistency.
- The documentation should include detailed descriptions of endpoints, request/response structures, authentication methods, and error codes.
- Every change to the service that affects the API must be accompanied by an update to the documentation.

Example Documentation

```yaml
openapi: 3.0.0
info:
  title: User Management API
  description: API for managing users in the system, including creation, retrieval, update, and deletion of users.
  version: 1.0.0

servers:
  - url: https://api.example.com/v1
    description: Production server
  - url: https://staging-api.example.com/v1
    description: Staging server

paths:
  /users:
    get:
      summary: Get a list of users
      description: Retrieve a list of all registered users.
      responses:
        "200":
          description: A JSON array of user objects
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/User"
        "500":
          description: Internal Server Error

    post:
      summary: Create a new user
      description: Create a new user with the provided details.
      requestBody:
        description: User object that needs to be created
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/NewUser"
      responses:
        "201":
          description: User successfully created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/User"
        "400":
          description: Bad request (e.g., invalid input)

  /users/{userId}:
    get:
      summary: Get a specific user
      description: Retrieve details of a user by user ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: ID of the user to retrieve
          schema:
            type: string
      responses:
        "200":
          description: A user object
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/User"
        "404":
          description: User not found
        "500":
          description: Internal Server Error

    put:
      summary: Update an existing user
      description: Update the details of a specific user by their user ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: ID of the user to update
          schema:
            type: string
      requestBody:
        description: Updated user object
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/UpdateUser"
      responses:
        "200":
          description: User successfully updated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/User"
        "400":
          description: Invalid input
        "404":
          description: User not found
        "500":
          description: Internal Server Error

    delete:
      summary: Delete a user
      description: Delete a user by their user ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: ID of the user to delete
          schema:
            type: string
      responses:
        "204":
          description: User successfully deleted
        "404":
          description: User not found
        "500":
          description: Internal Server Error

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          description: Unique identifier for the user
        name:
          type: string
          description: Full name of the user
        email:
          type: string
          description: Email address of the user
        createdAt:
          type: string
          format: date-time
          description: Timestamp of when the user was created

    NewUser:
      type: object
      required:
        - name
        - email
      properties:
        name:
          type: string
          description: Full name of the user
        email:
          type: string
          description: Email address of the user

    UpdateUser:
      type: object
      properties:
        name:
          type: string
          description: Full name of the user
        email:
          type: string
          description: Email address of the user
```

### 2. Documentation Maintenance

- API documentation must be reviewed and updated regularly or when a new version of the API is released.
- Old or deprecated APIs should be clearly marked in the documentation, with migration guidelines provided where necessary.

### 3. Review Process

- Before merging any code that changes the API, a review of the documentation must be conducted by a peer or senior team member.
- Pull requests that involve API changes will not be merged unless corresponding updates to the API documentation are present.

### 4. Versioning

- API documentation must follow the same versioning system as the API. Any major, minor, or patch release should have its own corresponding documentation.
- All versions of the API documentation should be archived and accessible.

### 5. Hosting and Access

- API documentation must be accessible to authorized users and hosted in a secure environment (e.g., **booky-docs**) bucket.
- Permissions must be set on the S3 bucket to ensure that only authorized personnel have write access, while read access is available to relevant users and services.

---

# GitHub Workflow for Uploading API Documentation to S3 Bucket

To automate the process of uploading API documentation to an S3 bucket, we can use GitHub Actions in the workflow. The following steps explain how to set up a workflow to upload documentation files to S3 whenever changes are made.

## Step 1: Set Up AWS Credentials in GitHub

- Go to your GitHub repository.
- In the repository settings, navigate to **Secrets and Variables**.
- Add the following secrets:
  - `AWS_ACCESS_KEY_ID`
  - `AWS_SECRET_ACCESS_KEY`
  - `AWS_REGION` (optional, if using multiple regions)

Note: Use booky-docs bucket for any documentation to be uploaded

## Step 2: GitHub Workflow YAML File

In the workflow file `.github/workflows/deploy-staging.yml` or similar in your repository:

```yaml
# ... code before DEPLOY steps NOTE: please check github workflows for reference
DEPLOY:
    steps:
      # ... remaining code after deployment steps - name: Deploy

      - name: Upload API Documentation to S3
        uses: keithweaver/aws-s3-github-action@v1.0.0
        with:
          command: cp
          destination: 's3://booky-docs/${{ env.APP_NAME }}.yml'
          source: ./docs/api_contract.yml
```

### 2. Booky Rosetta Manual Deployment:
- If a manual redeployment is required, follow these steps:
  1. Log in to the AWS Amplify console.
  2. Navigate to the **booky-Rosetta** project.
  3. In the "All apps" section, select **booky-Rosetta**.
  4. Click the **Redeploy** button in the "Deployments" tab. This will trigger a new deployment based on the current `main` branch state.

### to reflect documentation on frontend NOTE: currently exploring ways to automatically reflect documentation without redeploying

## Accessing the documentation

go to https://api-docs.bky.ph/ to access the consolidated API documentation

## Note

Documentation Review: Before the workflow is triggered, ensure documentation is reviewed and validated.
