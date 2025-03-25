Here’s a professional **GitHub README.md** in the exact style of your reference repository, with enhanced explanations and the same structural elements:

---

# **Serverless Data Management Portal with AWS**  
![AWS Architecture](assets/architecture.png)  
*Figure 1: End-to-end serverless architecture using DynamoDB, Lambda, API Gateway, S3, and CloudFront*

---


## 🌟 **Project Overview**  
This project implements a **fully serverless data management system** that allows users to securely submit and retrieve data through a web interface. Designed for scalability and cost-efficiency, it leverages AWS serverless technologies to eliminate infrastructure management overhead while ensuring high availability and automatic scaling under variable workloads.  

---

## ✨ **Key Features**  
- **Secure Data Transmission**: All communications are encrypted via HTTPS using CloudFront  
- **Serverless Backend**: Lambda functions handle data processing with zero server management  
- **Scalable Storage**: DynamoDB provides single-digit millisecond latency at any scale  
- **CDN Accelerated**: Global content delivery via CloudFront edge locations  
- **CORS Enabled**: Pre-configured for cross-origin access from web applications  

---

## 🚀 **Deployment Steps**  

### **1. DynamoDB Table Setup**  
Let create a highly available database table for user data storage.  
1. Navigate to **DynamoDB Console** → **Create Table**  
2. Configure with:  
   - **Table name**: `PskTable`  
   - **Partition key**: `studentid` (as a **String**)
   - Leave default settings for other configuration.
   - After Deployment, click on **PskTable** and click on **explore table items** to view every item we created or added. Initially it will be empty table as we known.

!0 

---

### **2. Lambda Functions Deployment**  
We gonna deploy serverless functions for data processing with Python language but don't worry I already provided the code just for **You**.  
  
1. **Create Function**:  
   - Namethe function : `GetPsk` 
   - Runtime should be : **Python 3.13**    
   - Change default execution setting : **Create new IAM role** 

2. **Add Inline Policy to our new IAM role**:  
   - Go to IAM → Roles → Select `GetPsk-role-XXXX`  
   - Create inline policy with json format.
   - The code is in my repository.(code)!
   - save the code and click **Deploy**.
!1
!2  
   - Create new test event and click on **Test**, you answer should be like my output.
   - It means there is no data inside the table.
!4

3. **PostPsk Function Setup**  
Repeat with these changes:  
 - Namethe function : `GetPsk` 
   - Runtime should be : **Python 3.13**    
   - Change default execution setting : **Create new IAM role** 

2. **Add Inline Policy**:  
   - Go to IAM → Roles → Select `PostPsk-role-XXXX`
!5
   - Create inline policy with json format.
   - The code is in my repository.(code)!
   - save the code and click **Deploy**.
!6
   - click on **Test**, you answer should be like my output.
   - Now, the output only show completion but you can view within DynamoDB **explore table items**.
!7

---

### **3. API Gateway Configuration**  
The next step is to create secure API endpoints with CORS support.  
1. Create **REST API** (Edge-optimized) named `DataAPI`  
2. Under `/data` resource , click on **create method**,  
   - Choose method type as `GET` → choose the function named `XXX:GetPsk`and **create method**.
!8 
   - Choose method type as `GET` → choose the function named `XXX:PostPsk` and **create method**.

3. **Enable CORS**
   - Click on **enable CORS**.
!9
   - Check the **GET** and **POST**.
!10
   - May be you notice there is an **Option** method appear between two method.
   - Finally click on **Deploy API** and pick **new stage** and name something and deploy it. 
!11

---

### **4. Frontend Hosting (S3)**  
We are now on board, let deploy static website with secure access controls.  
1. Create S3 bucket with the name : `your-psk-buddy` and uncheck the **block public access** and check the **acknowledge** because we are going to access for verifying Webpage functionality.
!12 
3. Under the properties tab, enable **Static Website Hosting** with `index.html`.
4. Under the permission tab, edit the bucket policy for all user access.[code]!
   - don't forget to change the bucket url and `/*`because we are under the object level of the bucket.
!13
5. Before uploading JS file , you need to update the invoke Url from API Gateway.
   - save the javascript after you update the file.
!14
7. click on **Upload** and **add files**.
   - `index.html`  
   - JavaScript files (with updated API endpoint)  
8. Apply bucket policy restricting access to CloudFront only.  

---

### **5. CloudFront Distribution**  
Enable HTTPS and global content delivery.  
1. Create distribution with S3 as origin
!17 18 19
   
3. Critical settings:  
   - **Origin Access Identity (OAI)**: Create new  
   - **Viewer Protocol Policy**: Redirect HTTP to HTTPS  
   - **Default Root Object**: `index.html`  
4. Wait 15-20 minutes for global deployment
   copy policy and attach s3 
!20
![CloudFront](assets/cloudfront-distro.png)  

---
