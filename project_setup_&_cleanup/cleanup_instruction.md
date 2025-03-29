
# **Let Cleanup Our Mess**
## **1. Delete CloudFront Distribution**
1. Go to **CloudFront** service
2. Select your distribution
3. Click **Disable** (wait for status to change)
4. Once disabled, click **Delete** and confirm

 *This may take 5-10 minutes to complete*

---

## **2. Empty & Delete S3 Bucket**
1. Go to **S3** service
2. Select your bucket
3. Click **Empty** → Type "permanently delete" to confirm
4. After emptying, click **Delete** → Enter bucket name to confirm

---

## **3. Delete API Gateway**
1. Go to **API Gateway** service
2. Select your API
3. Click **Actions** → **Delete**
4. Type the API name to confirm deletion

---

## **4. Delete Lambda Functions**
1. Go to **Lambda** service
2. Check both function boxes
3. Click **Actions** → **Delete**
4. Type "delete" to confirm

---

## **5. Delete DynamoDB Table**
1. Go to **DynamoDB** service
2. Select **Tables** from left menu
3. Choose your table
4. Click **Delete table**
5. Type "delete" to confirm

---

## **6. Delete IAM Roles**
For each Lambda role (do both):
1. Go to **IAM** service
2. Select **Roles** from left menu
3. Search for your role name
4. Click the role → **Delete role**
5. Type the role name to confirm

---

***Finally, we are about to say goodbye . It is good to see you. May God Bless you***

