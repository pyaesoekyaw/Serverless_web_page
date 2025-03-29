
# **Serverless Student List Webpage Deployment**  
![AWS Architecture](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/diagram.png)  
*Figure 1: End-to-end serverless architecture using DynamoDB, Lambda, API Gateway, S3, and CloudFront*

---


## 🌟 **Project Overview**  
This project implements a **fully serverless data management system** that allows users to securely submit and retrieve data through a web interface. Designed for scalability and cost-efficiency, it leverages AWS serverless technologies to eliminate infrastructure management overhead while ensuring high availability and automatic scaling under variable workloads. I know you will go through each step with me and it is not a big deal for someone like you. Just a beginner level project but well organized architecture for serverless infrastructure. GoodLuck With That, Mate.

---

## ✨ **Key Features**  
- **Secure Data Transmission**: All communications are encrypted via HTTPS using CloudFront  
- **Serverless Backend**: Lambda functions handle data processing with zero server management  
- **Scalable Storage**: DynamoDB provides single-digit millisecond latency at any scale  
- **CDN Accelerated**: Global content delivery via CloudFront edge locations  
- **CORS Enabled**: Pre-configured for cross-origin access from web applications  

---

## 🚀 **Deployment Steps**  

### **Step 1: DynamoDB Table Setup**  
Let create a highly available database table for user data storage.  
1. Navigate to **DynamoDB Console** → **Create Table**  
2. Configure with:  
   - **Table name**: `PskTable`  
   - **Partition key**: `studentid` (as a **String**)
   - Leave default settings for other configuration.
   - After Deployment, click on **PskTable** and click on **explore table items** to view every item we created or added. Initially it will be empty table as we known.

![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0000.png)

---

### **Step 2: Lambda Functions Deployment**  
We gonna deploy serverless functions for data processing with Python language but don't worry I already provided the code just for **You**.  
  
1. **Create Function**:  
   - Name the function : `GetPsk` 
   - Runtime should be : **Python 3.13** (prefer the latest version)
   - Change default execution setting :choose the **Create new IAM role** and create function.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0001.png)
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0002.png)

2. **Add Inline Policy to our new IAM role**:  
   - Go to IAM → Roles → Select `GetPsk-role-XXXX`. 
   - Under Permission tab, **add permission**.
   - **Create inline policy** with json format.
   - The [code](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/policies/iam_policy_for_GET.txt) is in my repository.
   - **Save** the rule and go back to the function.
   - Under **Code** tab, apply the [python code](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/function_codes/get_function.txt) for scan the data in DynamoDB table.
   - Don't forget to update your region and Table name that you created.
   ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0003.png)
   - Click on **Deploy** and wait a little moment.
   - Create new **test event** with your desired name and you don't need to add any parameters in the query because we are going to Get the data from table.
   - Click on **Test**, you answer should be like my output.
   - It means there is no data inside the table.
    ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0004.png)

3. **PostPsk Function Setup**  
Repeat with these changes:  
   - Namethe function : `PostPsk`. 
   - Runtime should be : **Python 3.13** (prefer the latest version)
   - Change default execution setting :choose the **Create new IAM role** and create function.
2. **Add Inline Policy to PostPsk Role**:  
   - Go to IAM → Roles → Select `PostPsk-role-XXXX`.
   - Under Permission tab, **add permission**.
   - Create inline policy with json format.
     
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0005.png)
   
   - The [code](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/policies/iam_policy_for_POST.txt) is in my repository.
   - **Save** the rule and go back to the function.
   - Under **Code** tab, apply the [python code](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/function_codes/post_function.txt) for insert the data in DynamoDB table.
   - Don't forget to update Table name that you created.
   - Save the code and click **Deploy**.
   - Create new **test event** with your desired name and you need to add parameters in the **Event JSON** to add some data for sample.
     
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0006.png)

   - Click on **Test**, you answer should be like my output.
   - Now, the output only show completion but you can view within DynamoDB **explore table items**.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0007.png)

---

### **Step 3: API Gateway Configuration**  
The next step is to create secure API endpoints with CORS support.  
1. Create **REST API** with the name `DataAPI`.
   - Deploy with **Edge Optimized** Endpoint type.
   - Click on **create API**.
2. Under `/` resource , click on **create method**. 
   - Choose method type as `GET` → choose the function named `XXX:GetPsk`and **create method**.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0008.png)
   - Choose method type as `POST` → choose the function named `XXX:PostPsk` and **create method**.

3. **Enable CORS**
   - Click on **enable CORS**.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0009.png)
   - Check the **GET** and **POST** and enable it.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0010.png)
   - May be you notice that there is an **Option** method appear between two methods.
   - Finally click on **Deploy API** and pick **new stage** and name something and deploy it. 
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0011.png)
   - After API deployment, you have to copy the **Invoke Url** which will require to interact between webpage backend and Gateway API.
     
---

### **Step 4: Frontend Hosting (S3)**  
We are now on board, let deploy static website with secure access controls.  
1. Create S3 bucket with the name : `your-psk-buddy` and uncheck the **block public access** and check the **acknowledgement** because we are going to access for verifying Webpage availability.
 ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0012.png)
 - Under the properties tab, enable **Static Website Hosting** with `index.html`.
 - Under the permission tab, edit **the bucket policy** with this [policy](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/policies/Initial_S3_policy.txt) for all user access.
 - Don't forget to change the **Bucket ARN** and `/*`because we are under the object level under the bucket.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0013.png)
2. Before uploading JS file , you need to update the **Invoke Url** from API Gateway.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0014.png)
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0023.png)
 - Save the javascript after you update the file.
 - Click on **Upload** and **add files** to store [html and javascript file](https://github.com/pyaesoekyaw/Serverless_web_page/tree/main/code_for_webpage) into S3 bucket.
 - `index.html`  
 - `scripts.js` files (with updated API endpoint) and click on **Upload**.
 - Test the S3 Url from **Static Website Hosting**. You will see the same out like mine.
 - You will see this is not secure because the traffic is sending over plaintext with HTTP protocol.
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0015.png)
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0016.png)
---

### **Step 5: CloudFront Distribution**  
To enable HTTPS and global content delivery service, we are going to need Cloudfront configuration.  
1. Go to CloudFront Console → **Create Distribution**.
- Origin Domain: Select your S3 bucket (my-cloudfront-oac-bucket.s3.amazonaws.com).
- Origin Access: Origin Access Control Settings (OAC) → **Create new OAC**.
- Default Root Object: type `index.html`.
- Check **Do not enable security protection** because we don't want to donate AWS like usual.
- Leave other configuration setting and create distribution.
   ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0017.png)
   ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0018.png)
   ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0019.png)
   
 2. Let attach Cloudfront policy to S3 in order to access traffic from CloudFront.
- Wait 5-10 minutes for Distribution global deployment.
- copy Yellow policy and navigate to S3 bucket.

  ![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0020.png)
  
- Go to your S3 bucket → under Permissions tab → edit **Bucket Policy**.
- Paste the copied policy and save it.
3. Enforce Strict S3 Blocking from public access.
- In your S3 bucket →under Permissions → edit **Block Public Access**.
- Confirm "Block all public access" is **ON**.

4. Finally, We have done this far, I hope you enjoy full steps and let test our result.
- Test the Cloudfront Url with adding student data and getting data from Web page UI.

![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0021.png)
![image](https://github.com/pyaesoekyaw/Serverless_web_page/blob/main/images/0022.png)

- I am pretty sure you got this. Congratulation and now we are on the state of [cleaning up the resource](https://github.com/pyaesoekyaw/dynamodb-quicksigth-project/blob/main/project_setup_%26_cleanup/cleanup_instruction.md).
  
---
