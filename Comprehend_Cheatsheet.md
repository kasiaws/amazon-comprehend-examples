# Amazon Comprehend Cheat Sheet

## Service Overview
Amazon Comprehend is a natural language processing (NLP) service that uses machine learning to extract insights from unstructured text data. It enables organizations to discover valuable information within documents, social media posts, customer interactions, and other text-based content without requiring specialized machine learning expertise. 

## Key Features

- **Rich NLP APIs (Text Only)**: Comprehend offers pre-trained APIs for entities, sentiment, key phrases, syntax, language detection, and topic modeling. It supports a broad set of common entity types and the four basic sentiments​ (Positive, Negative, Neutral and Mixed)

- **Custom Model AutoML**: Without needing ML expertise, you can train custom entity recognizers or classifiers by providing labeled examples. The service handles feature engineering and training, delivering ready-to-use models. Custom models include performance metrics (precision/recall) and can be updated via [Flywheels](https://docs.aws.amazon.com/comprehend/latest/dg/flywheels.html) for continuous learning.

- **Real-time & Batch Processing**: Comprehend supports synchronous (real-time) operations for low-latency analysis of individual texts and asynchronous jobs (Batch) for high-volume data in S3. This flexibility lets you pick fast inference for chatbots or scheduled jobs for document archives.

- **Document & Multilingual Support**: Although primarily text-focused, Comprehend can process text extracted from PDFs, Word documents and images (via [Amazon Textract](https://docs.aws.amazon.com/textract/)). It  natively supports many languages (English, Spanish, French, German, Italian, Portuguese, Chinese, Japanese, Korean, and more). Some advanced features (like targeted sentiment) work best in English

- **Scalability & Integration**: As a managed AWS service, Comprehend scales to handle from a few API calls to millions of documents. It integrates with AWS SDKsand many other AWS services to build intelligent document processing pipelines. For example, you can trigger an async analysis job when new documents land in S3, and write results back to a database.

- **Cost Efficiency**: Follows a pay-as-you-go pricing model with no minimum fees or upfront commitments, charging only for the documents analyzed and custom models trained


## Comprehend Terminology
- **Insights**: The results of analyzing text, such as:
    - **Entities**: References to people, places, organizations, items, and locations found in a document. Each detection entity includes a confidence score to guage accuracy.
    - **Key Phrases**: Important phrases or talking points extracted from a document
    - **Sentiment**: The overall emotional tone of a document (positive, negative, neutral, or mixed)
    - **Language**: The dominant language of a document
    - **Syntax**: The grammatical structure of the text, including parts of speech
    - **Events**: Detected occurrences and related details within a document
    - **Targeted Sentiment**: The sentiment expressed towards specific entities within a document
    - **PII**: (Personally Identifiable Information): Personal data that can identify an individual, such as addresses, bank account numbers, or phone numbers
    - **Toxicity**: Identification of toxic content across categories like hate speech, abuse, and profanity
    - **Prompt Safety**: Classification of input prompts as harmful or not
    - **Topic Modeling**: A feature to discover abstract topics within a collection of documents by identifying groups of related keywords
- **Custom Model Concepts**:
    - **Custom Classification**: A feature to create custom models to categorize documents based on user-defined labels
    - **Custom Entity Recognition**: A feature to create custom models to identify specific terms and phrases relevant to a particular domain
- **Document Processing Modes**: The ways in which Comprehend can analyze documents:
    - **Real-time (Synchronous)**: Analyzing single documents or small batches (up to 25) with immediate results
    - **Asynchronous (Batch)**: Analyzing large volumes of documents stored in Amazon S3 with results stored back in S3
- **Endpoints**: For synchronous inference with custom models, endpoints need to be created and managed. These are billed based on active time and provisioned throughput
- **Flywheels**: A feature to help manage custom models and associated data lakes for continuous improvement
- **Document Reader Config**: Configuration parameters to override default text extraction from PDFs and images, allowing the use of Amazon Textract features

## Is Amazon Comprehend a Good Fit?
| **Feature** | **Good Fit** | **Bad Fit** |
|:------------|:-------------|:------------|
| **Entity Recognition** | Extracting names, organizations, dates, etc., from customer reviews, legal documents, support tickets. | Complex nested entities (e.g., extracting full clause structures in legal contracts) needing deep custom parsing. |
| **Sentiment Analysis** | Analyzing social media, product reviews, customer support transcripts for customer emotion trends. | Generating nuanced opinions, subjective summaries, or analyzing sarcasm and humor. |
| **Custom Entity Recognition (AutoML)** | Detecting industry-specific terms like insurance policy numbers, medical codes, or financial instruments. | Extremely complex custom relationships between entities. |
| **Custom Text Classification (AutoML)** | Automatically categorizing documents (e.g., support tickets, resumes, emails) into user-defined classes. | Fine-grained intent detection (e.g., determining if an email expresses confusion vs. frustration) needing sophisticated semantic understanding. |
| **Topic Modeling** | Grouping articles, research papers, customer feedback into major topics for exploration and clustering. | Fine-grained dynamic topic tracking, trend detection over time. |
| **PII Detection & Redaction** | Automatically masking sensitive information (e.g., SSNs, addresses) in customer communications, documents. | Non-standard sensitive fields or very specific compliance cases. |
| **Toxicity Detection** | Moderating chat messages, comments, forums by detecting abusive or toxic content categories. | Context-dependent toxicity detection (e.g., recognizing irony, reclaiming terms) requiring deep understanding. |
| **Prompt Safety Detection** | Screening input prompts for harmful or unsafe intent before sending to LLMs or AI systems. | Detailed multi-layered risk scoring or adversarial prompt defense. |
| **Document Processing (Plain Text, PDF, Images)** | Extracting text insights from PDFs, Word files, images (with Textract pre-processing) for analysis. | Rich forms, complex tables, handwritten forms (better handled with specialized services like Textract Forms/Table Extraction or Bedrock Data Automation). |

## Use Cases
| **Industry/Function** | **Example Use Case** |
|:----------------------|:---------------------|
| **Customer Experience** | Analyze customer reviews, social media, and support emails for overall and targeted sentiment. |
| **Document Management** | Extract entities, key phrases, and classify documents (e.g., support tickets, contracts, insurance claims). |
| **Compliance and Privacy** | Detect and redact PII to comply with regulations like GDPR, HIPAA, and CCPA. |
| **Content Moderation** | Detect toxic language and unsafe prompts in user-generated content or chatbots. |
| **Knowledge Discovery** | Perform topic modeling on surveys, academic papers, or internal reports to uncover emerging trends. |
| **Human Resources** | Extract skills and job titles from resumes; analyze employee feedback sentiment. |
| **Legal Services** | Automate contract review by extracting key clauses, party names, and effective dates. |
| **Healthcare and Life Sciences** | Extract key information from clinical notes, patient records etc. For advanced medical terms and to detect and redact PHI, consider [Comprehend Medical](https://docs.aws.amazon.com/comprehend-medical/latest/dev/comprehendmedical-welcome.html) |
| **Financial Services** | Monitor communications for risk factors and automate extraction from financial documents. |
| **Contact Centers** | Analyze real-time call transcripts for customer sentiment; route support tickets by topic. |
| **Media & Entertainment** | Monitor audience reactions to content across platforms to analyze sentiment |


## Killer Features

- **AutoML + Flywheel**: Build custom NLP models without writing code. Simply label a few dozen examples for a new category or entity, and Comprehend trains the model. Flywheels automate continuous retraining as you gather more data, boosting accuracy over time.

- **Targeted Sentiment**: Get sentiment not just for whole documents but by entity. This lets you see exactly which parts of your product or service customers love or dislike, providing more actionable insights.

- **Built-in PII Redaction**: One-step privacy protection. Comprehend can both identify and mask PII in a text stream or documents, helping you anonymize data without a separate service.

- **Scalable & Managed**: As a fully managed AWS service, Comprehend handles scaling, performance tuning, and reliability. Teams without deep ML expertise can deploy advanced NLP tasks quickly, benefiting from AWS security, encryption, and compliance features by default.

## Benefits

- **No ML Expertise required**: Prebuilt APIs and AutoML workflows make it easy for developers without machine learning backgrounds to extract insights from text.

- **Full-Spectrum NLP Features**: Covers entities, sentiment, syntax, key phrases, language and PII detection, prompt safety, and topic modeling - all in one service

- **Customizable Models with AutoML**: Train custom classifiers and entity recognizers easily by providing labeled examples, with built-in model evaluation metrics

- **Scalable and Serverless**: Fully managed infrastructure automatically scales to your workload without provisioing servers or worrying about performance tuning

## FAQs

### **Q: What languages does Comprehend support?**  
**A:** Comprehend [supports multiple major languages](https://docs.aws.amazon.com/comprehend/latest/dg/supported-languages.html) including English, Spanish, French, German, Italian, Portuguese, Simplified/Traditional Chinese, Japanese, Korean, Hindi, and Arabic. Some advanced features like targeted sentiment or PII detection are primarily available in English.

### **Q: How does Amazon Comprehend handle multilingual documents where multiple languages are present within a single document?**  
**A:** Amazon Comprehend is designed to analyze text written in a *single dominant language* per request. When analyzing multilingual documents, Comprehend will detect and operate on the **primary language** it identifies in the input. It does **not support multiple language segments within the same request**, and results may be inaccurate if the text contains a mix of languages.

📌 **Best practice:** If your document contains multiple languages, split it by language segment and process each part separately, specifying the appropriate `LanguageCode` in each API call.  
🔗 [Learn more about language support →](https://docs.aws.amazon.com/comprehend/latest/dg/how-languages.html)

### **Q: What file formats does Comprehend support?**  
**A:** Comprehend natively processes plain text (UTF-8). For PDFs, Word documents, and image files (JPEG, PNG, TIFF), you can use Amazon Textract first to extract the text before feeding it into Comprehend.

### **Q: How do I feed documents to Comprehend?**  
**A:** You can send raw text through the AWS SDK, CLI, or console APIs. For document files like PDFs or images, use Amazon Textract to extract the text first, then pass the extracted text into Comprehend APIs.

### **Q: What are the input size and usage limits for Amazon Comprehend?**  
**A:** Amazon Comprehend has various service limits depending on the API type, such as synchronous requests, batch jobs, and custom model training. These limits include constraints on input size, number of documents, label count, file formats, and more. It's important to understand these when designing large-scale or high-throughput NLP pipelines.

📌 For the most current and complete list of service guidelines and limitations, refer to the official AWS documentation:  
[Amazon Comprehend Guidelines and Limits →](https://docs.aws.amazon.com/comprehend/latest/dg/guidelines-and-limits.html)

### **Q: Do I need ML expertise to use Comprehend?**  
**A:** No. Comprehend offers ready-to-use APIs and AutoML workflows, making it easy for developers and analysts without machine learning backgrounds to extract insights from text.

### **Q: How does Comprehend differ from SageMaker?**  
**A:** Comprehend provides pre-trained and AutoML models for common text analysis tasks. SageMaker is a fully customizable machine learning platform where you can build, train, and deploy completely custom models for any use case.

### **Q: Are there any best practices for optimizing the performance and cost-effectiveness of custom models in Amazon Comprehend?**  
**A:** Yes, to ensure high-performing and cost-efficient custom models (classification or entity recognition), consider the following best practices:

- **Start with quality training data:** Use clean, representative, and well-labeled samples.
- **Use balanced datasets:** Avoid label imbalance to reduce bias.
- **Limit unnecessary labels:** Keep the label set concise and meaningful.
- **Use Flywheels:** Automate retraining based on evolving data.
- **Monitor metrics:** Check precision, recall, and F1 regularly.
- **Optimize endpoint usage:** Use batch jobs where real-time isn't needed.

📌 [Improve Prediction Quality in Comprehend Custom Models →](https://aws.amazon.com/blogs/machine-learning/improve-prediction-quality-in-custom-classification-models-with-amazon-comprehend/)

### **Q: Can I use Comprehend for real-time streaming data?**  
**A:** Yes. Comprehend’s synchronous APIs can handle real-time analysis for applications like chatbots or social media monitoring. For massive document volumes, it's more efficient to use asynchronous batch processing.

### **Q: How does Comprehend handle PII detection?**  
**A:** Comprehend can automatically detect and optionally redact sensitive personal information like names, addresses, bank details, and phone numbers from text data.

### **Q: How much does it cost to use Comprehend?**  
**A:** Pricing is based on the number of characters analyzed or documents processed. There are additional charges for custom model training and for hosting real-time endpoints. Use the [AWS Pricing Calculator](https://calculator.aws.amazon.com/) to estimate costs.

### **Q: Is Comprehend a good fit for multimodal content (images, audio, video)?**  
**A:** No. Comprehend is specialized for text analysis. For multimodal content extraction across documents, images, audio, and video, consider using Amazon Bedrock Data Automation.



## Comprehend Vs Bedrock LLMs Vs Bedrock Data Automation
| **Criteria** | **Amazon Comprehend** | **Bedrock LLMs (e.g., Claude, Nova)** | **Bedrock Data Automation** |
|:-------------|:----------------------|:--------------------------------------|:-----------------------------|
| **Primary Focus** | In-depth structured text analysis (entities, sentiment, PII, classification, etc.) | General-purpose language understanding, generation, and reasoning | Multimodal data extraction from text, images, audio, video |
| **Best For** | Structured text extraction, document classification, targeted sentiment, compliance (PII) | Flexible text summarization, Q&A, creative writing, dynamic understanding | Automated document processing, structured field extraction across multiple modalities |
| **Strengths** | Fast, reliable, explainable outputs with AutoML customization | Extremely flexible; can follow custom prompts and complex instructions | Handles complex documents with embedded tables, images, and audio/video insights |
| **Custom Models** | AutoML for custom entity recognition and classification | Fine-tuning or customizing LLMs possible (via prompt engineering or retrieval-augmented generation) | Blueprint customization for document/image processing workflows |
| **Input Types** | Plain text, PDFs, Word docs, images (via Textract) | Some models accept multi-modal inputs (Text, Image) | Documents (PDFs, Word), images (JPG, PNG), audio, video |
| **Output Style** | Structured JSON outputs (entities, key phrases, classifications) | Can customize the output using prompt engineering | Structured key-value fields, labeled content, extracted metadata |
| **Real-Time Capability** | Yes (synchronous APIs) | Yes (via Bedrock API) | No (designed for batch-style document/media processing) |
| **Batch Processing Capability** | Yes (async S3 jobs) | Yes (through Bedrock batch mode) | Yes (optimized for large file sets and multimodal content) |
| **When to Use** | You need fast, highly structured text analysis and easy model tuning with minimal setup. | You need flexibility to generate, summarize, or reason creatively from text input. Can perform nuanced reasoning | You need to extract structured data from diverse and complex documents, images, or media files. |
| **When NOT to Use** | Need to deeply summarize, generate new text, reason across documents dynamically. | Need very strict, explainable extraction of structured fields (prefer Comprehend or Data Automation). | Pure text-only analysis or nuanced text generation (prefer Comprehend or LLMs). |

## Technical Enablement Links

- [Amazon Comprehend Documentation](https://docs.aws.amazon.com/comprehend/latest/dg/what-is.html) – Official developer guide, overview, and API references.
- [Amazon Comprehend API Reference](https://docs.aws.amazon.com/comprehend/latest/APIReference/API_Operations.html) – Full details on every Comprehend API operation.
- [Amazon Comprehend Pricing](https://aws.amazon.com/comprehend/pricing/) – Up-to-date pricing details for APIs, custom models, endpoints, and batch processing.
- [Amazon Comprehend Tutorials on AWS Samples GitHub](https://github.com/aws-samples?utf8=%E2%9C%93&q=comprehend&type=&language=) – Example code and workshops for Comprehend.
- [AWS Blog: Improve Prediction Quality in Comprehend Custom Models](https://aws.amazon.com/blogs/machine-learning/improve-prediction-quality-in-custom-classification-models-with-amazon-comprehend/) – Best practices for better custom model performance.
- [AWS Machine Learning Blog - Amazon Comprehend Articles](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-comprehend/) – Real-world use cases and feature announcements.
- [Comprehend Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/b54ae619-4779-409c-9bde-6e9c00edcf0f/en-US) - Explore various Comprehend features by hands-on learning.
- [AWS Re:Post Community – Comprehend Questions](https://repost.aws/tags/TArJuWuDW_RS2Qbz1XXvbVzA/amazon-comprehend) – Community Q&A, troubleshooting tips, and real-world advice.

## Quickstart: Amazon Comprehend

### 1. Prerequisites
- An active AWS account.
- Install the AWS CLI and configure your credentials (`aws configure`).
- Install the AWS SDK for your preferred language (examples here use **Python (boto3)**).

```bash
pip install boto3
```

---

### 2. Detect Sentiment of a Text

```python
import boto3

# Create a Comprehend client
comprehend = boto3.client('comprehend')

# Text to analyze
text = "I love using Amazon Comprehend! It makes text analysis so easy."

# Detect sentiment
response = comprehend.detect_sentiment(
    Text=text,
    LanguageCode='en'
)

# Print results
print("Detected Sentiment:", response['Sentiment'])
print("Sentiment Scores:", response['SentimentScore'])

#Response:
#Detected Sentiment: POSITIVE
#Sentiment Scores: {'Positive': 0.9995021820068359, 'Negative': 0.00012139411410316825, 'Neutral': 0.00015575425641145557, 'Mixed': 0.00022062976495362818}
```

---

### 3. Detect Entities

```python
import boto3

# Create a Comprehend client
comprehend = boto3.client('comprehend')

# Text to analyze
text = "Jeff Bezos founded Amazon in Seattle in 1994."

# Detect entities
response = comprehend.detect_entities(
    Text=text,
    LanguageCode='en'
)

# Print detected entities
for entity in response['Entities']:
    print(f"Type: {entity['Type']}, Text: {entity['Text']}, Score: {entity['Score']:.2f}")

#Response:
#Type: PERSON, Text: Jeff Bezos, Score: 1.00
#Type: ORGANIZATION, Text: Amazon, Score: 0.99
#Type: LOCATION, Text: Seattle, Score: 1.00
#Type: DATE, Text: 1994, Score: 1.00
```

---

### 4. Analyze Batch of Text Files in S3 (Asynchronous Job)

```python
import boto3

# Create a Comprehend client
comprehend = boto3.client('comprehend')

# Start a batch entity detection job
response = comprehend.start_entities_detection_job(
    InputDataConfig={
        'S3Uri': 's3://your-bucket/input-data/',
        'InputFormat': 'ONE_DOC_PER_FILE'
    },
    OutputDataConfig={
        'S3Uri': 's3://your-bucket/output-data/'
    },
    DataAccessRoleArn='arn:aws:iam::your-account-id:role/your-Comprehend-access-role',
    LanguageCode='en',
    JobName='EntityDetectionJob'
)

print("Job ID:", response['JobId'])
print("Job Status:", response['JobStatus'])
```

> ⚡ **Note:**  
> - Replace `your-bucket`, `your-account-id`, and `your-Comprehend-access-role` with your actual values.
> - Results will be stored in your output S3 bucket when the job completes.

---

## 🧪 Practical Exercise: Analyze Customer Reviews with Amazon Comprehend

### Goal:
Use Amazon Comprehend to perform sentiment analysis on a batch of customer reviews.

---

### 🛠️ What You’ll Need:
- AWS account
- AWS CLI and Python (`boto3`) installed
- A small set of sample customer reviews (use your own or the example below)
- S3 bucket to upload input data
- An IAM role that contains the following permissions:
    - comprehend:StartSentimentDetectionJob
    - comprehend:DescribeSentimentDetectionJob
    - s3:GetObject
    - s3:ListBucket
    - s3:PutObject

---

### 📄 Step 1: Create Sample Input File

Save the following text as `reviews.txt` (one review per line):

```
This product exceeded my expectations. The quality is outstanding.
I'm disappointed with this purchase. It broke after a week.
Customer service was very helpful and resolved my issue quickly.
Average product, nothing special but does the job.
Terrible experience, would not recommend to anyone.
```

---

### ☁️ Step 2: Upload to Amazon S3

```bash
aws s3 cp reviews.txt s3://your-bucket-name/input/reviews.txt
```

> Replace `your-bucket-name` with your actual S3 bucket name.

---

### 🚀 Step 3: Start a sentiment detection job

```python
import boto3

comprehend = boto3.client('comprehend')

response = comprehend.start_sentiment_detection_job(
    InputDataConfig={
        'S3Uri': 's3://your-bucket-name/input/',
        'InputFormat': 'ONE_DOC_PER_LINE'
    },
    OutputDataConfig={
        'S3Uri': 's3://your-bucket-name/output/'
    },
    DataAccessRoleArn='arn:aws:iam::your-account-id:role/ComprehendAccessRole',
    LanguageCode='en',
    JobName='KeyPhraseReviewAnalysis'
)

print("Job ID:", response['JobId'])
```

---

### 🧾 Step 4: Analyze the Output

Once the job completes, download the output from your S3 bucket. Each line will contain the sentiment detected for each review.

**Sample Output**
```
Sentiment Analysis Results:
--------------------------
Text: This product exceeded my expectations. The quality is outstanding.
Sentiment: POSITIVE
  Positive: 0.9998
  Negative: 0.0000
  Neutral: 0.0001
  Mixed: 0.0000
--------------------------
Text: I'm disappointed with this purchase. It broke after a week.
Sentiment: NEGATIVE
  Positive: 0.0000
  Negative: 0.9999
  Neutral: 0.0000
  Mixed: 0.0001
--------------------------
Text: The customer service was helpful and resolved my issue quickly.
Sentiment: POSITIVE
  Positive: 0.9994
  Negative: 0.0000
  Neutral: 0.0002
  Mixed: 0.0004
--------------------------
Text: Average product, nothing special but does the job.
Sentiment: MIXED
  Positive: 0.0294
  Negative: 0.0023
  Neutral: 0.0004
  Mixed: 0.9680
--------------------------
Text: Terrible experience, would not recommend to anyone.
Sentiment: NEGATIVE
  Positive: 0.0001
  Negative: 0.9998
  Neutral: 0.0000
  Mixed: 0.0000
--------------------------
```
---

## 🗣️ Customer Testimonials & Case Studies

### 📌 McDonald’s – Voice of the Customer at Scale
McDonald’s uses Amazon Comprehend to extract insights from customer surveys, helping them understand and improve guest experience across thousands of stores.

🔗 [Watch the re:Invent session →](https://www.youtube.com/watch?v=la2dHBYJPoI)

---

### 📌 Amazon Comprehend Customers
> ✨ For  public customer references, visit the official  
> [Amazon Comprehend Customer Success page →](https://aws.amazon.com/comprehend/customers/)

