# AWS Lambda Mind Map

## 1. Introduction to AWS Lambda
- **Definition**: 
  - AWS Lambda is a serverless compute service that automatically runs code in response to events and manages the underlying compute resources.
- **Use Cases**:
  - Event-driven workloads (e.g., S3, DynamoDB triggers).
  - APIs using Amazon API Gateway.
  - Real-time file processing, data pipelines, or IoT backend.

---

## 2. Steps to Create a Lambda Function
1. **Access Lambda Console**:
   - Go to AWS Management Console → AWS Lambda.
2. **Choose a Blueprint**:
   - Use existing templates or start from scratch.
3. **Configure the Function**:
   - Define a **name**, runtime (e.g., Python, Node.js), and execution role.
4. **Write or Upload Code**:
   - Inline editor, .zip file, or via AWS CLI.
5. **Test the Function**:
   - Use test events to simulate invocations.
6. **Deploy and Monitor**:
   - Use CloudWatch Logs for debugging and performance insights.

---

## 3. Lambda Layers
- **What Are Layers?**
  - Reusable components shared across functions (e.g., libraries, dependencies, utilities).
- **Steps to Create Layers**:
  1. Package dependencies into a `.zip` file.
  2. Upload the `.zip` to the Lambda console under **Layers**.
  3. Add the layer to one or more Lambda functions.
- **Use Cases**:
  - Shared libraries (e.g., NumPy for Python).
  - Reduce deployment package size.
  - Centralize updates for dependencies.

---

## 4. Adding Triggers
- **Supported Triggers**:
  - **Amazon S3**:
    - Triggers: `ObjectCreated`, `ObjectRemoved`, etc.
    - **Steps**:
      1. Open S3 in the AWS Console.
      2. Go to the bucket → **Properties** → **Event Notifications**.
      3. Add a new notification and specify the Lambda function.
      4. Save and test by uploading/removing files.
    - **Why**: Automate file processing, e.g., image resizing, data validation.

  - **Amazon DynamoDB**:
    - Triggers: DynamoDB Streams.
    - **Steps**:
      1. Enable DynamoDB Streams on your table.
      2. Go to the Lambda console → Add trigger → Select DynamoDB.
      3. Configure batch size and starting position.
      4. Save and test by updating items in the table.
    - **Why**: Automate data pipelines or real-time analytics.

  - **Amazon SNS**:
    - Triggers: Notifications from SNS topics.
    - **Steps**:
      1. Create an SNS topic and add a subscription for the Lambda function.
      2. Go to the Lambda console → Add trigger → Select SNS.
      3. Configure the topic and test by publishing messages.
    - **Why**: Notify systems or process incoming notifications.

  - **Amazon SQS**:
    - Triggers: Messages in SQS queues.
    - **Steps**:
      1. Create an SQS queue.
      2. Go to the Lambda console → Add trigger → Select SQS.
      3. Configure batch size and test by sending messages to the queue.
    - **Why**: Queue-based processing, e.g., decoupling systems.

---

## 5. Limits, Restrictions, and Throttling
- **Execution Limits**:
  - Max execution time: 15 minutes.
  - Memory: 128 MB to 10 GB.
  - Max disk space (/tmp): 512 MB.
- **Concurrency**:
  - Default account-wide concurrency limit: 1,000 invocations.
- **Throttling**:
  - Excess invocations are queued and retried.

- **Reasons for Restrictions**:
  - To prevent misuse or abuse of shared resources.
  - To ensure high availability and fairness across accounts.

- **Solutions**:
  - Use **Reserved Concurrency** to ensure critical functions have dedicated capacity.
  - Enable **Provisioned Concurrency** for predictable scaling.
  - Optimize memory allocation to improve execution performance.

---

## 6. Deployment with Docker Images
- **Steps**:
  1. **Create a Dockerfile**:
     - Include your code, dependencies, and runtime base image.
     - Example:
       ```dockerfile
       FROM public.ecr.aws/lambda/python:3.9
       COPY app.py .
       CMD ["app.lambda_handler"]
       ```
  2. **Build the Image**:
     - Use `docker build -t my-lambda-image .` to build the image locally.
  3. **Push to Amazon ECR**:
     - Authenticate with ECR using `aws ecr get-login-password`.
     - Create an ECR repository and push the image using `docker push`.
  4. **Deploy via Lambda Console**:
     - Select **Image** as the deployment type.
     - Choose the image from your ECR repository.
  5. **Test and Monitor**:
     - Invoke the Lambda function and use CloudWatch Logs for debugging.

---

## 7. Scaling
- **Automatic Scaling**:
  - AWS Lambda automatically scales horizontally based on the number of requests.
  - No manual intervention required.

- **Concurrency Model**:
  - Each invocation runs in its own isolated environment.
  - Concurrent executions = Active requests being processed.

- **Burst Concurrency Limits**:
  - AWS allows a burst of up to 3,000 concurrent executions (varies by region).
  - Requests above the limit are throttled and retried.
  - **Handling Burst Limits**:
    - Use **Reserved Concurrency** to guarantee capacity.
    - Use SQS to queue incoming requests.

---

## 8. Advanced Scenario-Based Questions
1. **Scenario**: How would you handle a Lambda function that frequently hits concurrency limits?
   - **Solution**:
     1. Monitor usage in CloudWatch.
     2. Use reserved concurrency to prioritize critical functions.
     3. Queue incoming requests with SQS to handle bursts.

2. **Scenario**: A function has high cold start latency. What would you do?
   - **Solution**:
     1. Enable Provisioned Concurrency.
     2. Optimize package size and reduce dependencies.
     3. Use lightweight runtimes like Node.js or Python.

3. **Scenario**: How to implement a retry mechanism for a failed Lambda invocation?
   - **Solution**:
     1. Enable DLQs (Dead Letter Queues) for the function.
     2. Use retry settings for event sources (e.g., SQS, DynamoDB).
     3. Monitor failures with CloudWatch.

4. **Scenario**: Your Lambda function needs more than 15 minutes to process a task. What’s the solution?
   - **Solution**:
     1. Split the task into smaller, parallel invocations.
     2. Use Step Functions to orchestrate tasks.
     3. Consider running the task on EC2 or Fargate instead.

5. **Scenario**: How do you debug a failing Lambda function?
   - **Solution**:
     1. Use CloudWatch Logs to inspect error messages and stack traces.
     2. Enable AWS X-Ray for tracing execution flow.
     3. Test the function locally with AWS SAM or Docker.

---