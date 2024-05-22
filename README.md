# Gen-AI-Conversational-Co-pilot

### Assumptions for a Conversational Gen-AI Co-Pilot for Engineers Wiki Agent

The following assumptions are made to design a high-level architecture for a conversational Gen-AI co-pilot that will serve as an internal Wiki Agent for engineers. This solution aims to assist product engineers globally by providing quick, relevant information derived from internal documentation and other knowledge sources.

#### Assumptions:

1. **User Base and Interaction:**
   - **Target Users:** Engineers
   - **Interaction Mode:** Users will interact with the co-pilot through a web-based application interface.

2. **Documentation and Knowledge Base:**
   - **Document Sources:** Internal documentation, wiki pages, technical manuals, and best practices guides stored as PDF files.
   - **Document Storage:** These documents are stored in Amazon S3 buckets for scalability and easy access.
   - **Document Size:** Approximately 500 pages of PDF documents, split into manageable chunks for processing.

3. **Embedding and Semantic Search:**
   - **Embedding Model:** Pre-trained embedding models such as Sentence Transformers will be used to convert text into vector representations.
   - **Vector Database:** A vector database service like Qdrant will be used to store and query embeddings.

4. **Large Language Model (LLM):**
   - **Model:** GPT-4 will be used for generating responses based on the retrieved chunks of information and user queries.

5. **Prompt Template and Response Generation:**
   - **Prompt Template:** The input to GPT-4 will be structured using a prompt template that includes the user query, retrieved document chunks, and any relevant chat history.
   - **Response:** The LLM will generate a relevant response that is delivered back to the user interface.


-----------------------------------

AWS Architecture:

Network Setup:

VPC: A Virtual Private Cloud with subnets across two Availability Zones.
Subnets: Private subnets in each AZ.
NAT Gateway: Provides internet access to private subnet resources.
Load Balancer: Distributes incoming traffic across multiple AZs for high availability.
ECS and ECR:

ECS Cluster: Hosts the Docker containers.
ECR (Elastic Container Registry): Stores Docker images for the frontend, backend, and vector database.
Fargate: Manages container deployment without needing to manage servers.
Compute and Storage:

Frontend Container: Hosts the user interface.
Backend Container: Handles application logic and API requests.
Vector Database Container: Stores and queries document embeddings.
Amazon S3: Stores the internal wiki documents and other relevant files.
Load Balancer:

Ensures even distribution of traffic and high availability across ECS tasks in different AZs.
API Gateway and Cognito:

API Gateway: Routes user queries to backend services.
Cognito: Manages user authentication and authorization.
Certificate Manager:

Manages SSL/TLS certificates for secure communication.
Lambda Functions and Secrets Manager:

Lambda: Handles serverless compute tasks for specific operations.
Secrets Manager: Manages sensitive information such as API keys and database credentials.

Monitoring and Logging:
CloudWatch: Monitors performance metrics and logs.


GitHub Actions: Manages the CI/CD pipeline for deploying updates to the containers in ECS.

#### High-Level Design:
!My Image

1. **User Query:**
   - Users interact with the co-pilot via a web interface.
   - User queries are sent to an API Gateway.

2. **API Gateway:**
   - Receives user queries and routes them to the backend services.

3. **Embedding Models:**
   - User queries are converted into vector embeddings using pre-trained models.

4. **Vector Database:**
   - Embeddings of internal documents are stored in a vector database.
   - A semantic search is performed to retrieve the most relevant document chunks.

5. **Document Retrieval:**
   - Relevant document chunks are retrieved from the vector database.
   - Retrieved chunks are combined with the original query and chat history.

6. **Prompt Template:**
   - A prompt template structures the input for GPT-4.

7. **Large Language Model (GPT-4):**
   - Processes the structured input and generates a response.

8. **Response Delivery:**
   - The generated response is sent back to the user via the API Gateway.

9. **Security Measures:**
   - AWS Cognito for user authentication.

10. **Scalability and Availability:**
    - Deployed across multiple AWS regions.
    - Regular backups and cross-region replication.
    - Auto-scaling and load balancing for high availability.

11. **Operational and Monitoring:**
    - Monitoring using AWS CloudWatch.
    - Centralized logging with AWS CloudTrail and CloudWatch Logs.
    - Automated alerts for performance and security issues.


`
