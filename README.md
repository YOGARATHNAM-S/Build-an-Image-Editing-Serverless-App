# 🖼️ Serverless Image Editing App with Amazon Bedrock

This is a **serverless web application** that uses **Amazon Bedrock** (Generative AI) to edit and generate images. The entire system runs on a **fully serverless AWS architecture** using Amplify, Cognito, API Gateway, and Lambda.

---

## 📌 Architecture Overview

![Architecture Diagram](/assets/arch.png)

### 🔄 Flow of the Application

1. User enters a prompt / uploads an image in the browser  
2. Authentication is handled by **Amazon Cognito**  
3. Request goes through **API Gateway**  
4. **Lambda Function** processes the request  
5. Lambda calls **Amazon Bedrock** for image generation/editing  
6. Result & metadata stored in **DynamoDB**  
7. Final image is returned to the frontend  

---

## 🧰 AWS Services Used

| Service | Purpose |
|------|------|
| **AWS Amplify** | Frontend hosting |
| **Amazon Cognito** | User authentication |
| **API Gateway** | API routing |
| **AWS Lambda** | Backend processing |
| **Amazon Bedrock** | AI image generation/editing |
| **Amazon DynamoDB** | Stores image metadata |
| **Amazon S3 (optional)** | Image storage |

---

## 📁 Project Structure

```

Build an Image Editing Serverless App/
│
├── assets/                 # Diagrams & images
├── config/                 # Configuration files
│
├── index.html              # Main UI file
├── vite.html               # Development preview
│
└── lambda_function.py       # Lambda + Bedrock logic

```

### 📄 File Descriptions

| File / Folder | Description |
|------|------|
| `assets/` | Architecture and output images |
| `config/` | Environment / config files |
| `index.html` | Main web interface |
| `vite.html` | Development version |
| `lambda_function.py` | API → Bedrock → DynamoDB logic |

---

## ✅ Prerequisites

Before deployment, make sure you have:

- ✅ AWS Account
- ✅ AWS CLI installed and configured
- ✅ Python 3.9+
- ✅ Node.js (if needed)
- ✅ Bedrock models enabled in AWS console

---

## 🚀 Deployment Steps

### 1️⃣ DynamoDB

Create a table:

```

ImageGenerationTable

```

Primary Key:
```

id (String)

````

---

### 2️⃣ IAM Role (for Lambda)

Attach this policy:

```json
{
  "Effect": "Allow",
  "Action": [
    "bedrock:InvokeModel",
    "dynamodb:PutItem",
    "dynamodb:GetItem",
    "logs:*"
  ],
  "Resource": "*"
}
````

---

### 3️⃣ Lambda Configuration

Upload **`lambda_function.py`** to AWS Lambda:

| Setting | Value      |
| ------- | ---------- |
| Runtime | Python 3.9 |
| Memory  | 512 MB     |
| Timeout | 30 seconds |

This Lambda:

* Receives user prompt
* Calls Amazon Bedrock
* Saves metadata to DynamoDB
* Returns image

---

### 4️⃣ API Gateway

Create route:

```
POST /generate
```

Attach Lambda → Enable **CORS**

---

### 5️⃣ Amazon Cognito

1. Create **User Pool**
2. Create **App Client** (no secret)
3. Add IDs in `index.html`:

   * UserPoolId
   * ClientId
   * Region

---

### 6️⃣ Frontend Hosting (AWS Amplify)

Upload:

```
index.html
vite.html
assets/
config/
```

Amplify will generate a **Live URL**

---

## 🧪 Example Prompt & Output

**Prompt:**

```
A futuristic cyberpunk city at night with neon lights
```

**Output:**
AI-generated image from Amazon Bedrock


![Sample Output](assets/output.png)


![Sample Output](assets/sample.png)

---

## 🔒 Security Best Practices

* ✅ Use Cognito tokens for every API request
* ✅ Apply least-privilege IAM roles
* ✅ Enable API throttling
* ✅ Use AWS Secrets Manager
* ❌ Never expose keys in frontend

---

## 🔮 Future Improvements

* Image history page
* S3 integration
* Image filters / enhancements
* Real-time editing
* Mobile-friendly UI

---
