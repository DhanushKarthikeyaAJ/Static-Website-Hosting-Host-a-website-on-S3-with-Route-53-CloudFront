# 🚀 Static Website Hosting on AWS with Custom Domain

This guide outlines how to host a static website on **Amazon S3** and serve it through **CloudFront** with a custom domain secured via **SSL/HTTPS** using **Route 53** and **ACM**.

---

## ✅ Prerequisites

- An AWS account
- A registered domain (in Route 53 or any other provider)

---

## 📦 Step 1: Create an S3 Bucket for Website Hosting

1. Go to the **AWS S3 Console** and click **Create bucket**.
2. **Bucket name**: Use your domain name (e.g., `example.com`).
3. **Region**: Select your preferred region.
4. **Disable Block Public Access**:
   - Uncheck "Block all public access" and acknowledge the warning.
5. **Enable Static Website Hosting**:
   - Go to the **Properties** tab → Click **Edit** under **Static website hosting**.
   - Select **Enable** → Choose **Host a static website**.
   - Set **index document** to `index.html`.
   - Note the **Bucket Website Endpoint URL**.
6. Click **Create bucket**.

---

## 📤 Step 2: Upload Website Files

1. Open your newly created S3 bucket.
2. Click **Upload** → Add your `index.html`, `style.css`, `script.js`, etc.
3. Click **Upload** to finish.

---

## 🔐 Step 3: Configure S3 Bucket Policy for Public Access

1. Go to the **Permissions** tab of the bucket.
2. Under **Bucket Policy**, click **Edit** and paste the following:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example.com/*"
    }
  ]
}
```

> Replace `example.com` with your actual bucket name.

3. Click **Save changes**.

---

## 🌐 Step 4: Register a Domain (Optional)

1. Go to **Route 53 Console** → **Domains** → **Register Domain**.
2. Search and register your domain (e.g., `example.com`).
3. Wait for AWS to finish registration.

---

## 🧭 Step 5: Create a Hosted Zone in Route 53

1. Go to **Hosted Zones** → Click **Create hosted zone**.
2. Enter your domain name.
3. Choose **Public Hosted Zone** and click **Create**.

---

## ☁️ Step 6: Set Up CloudFront for Content Delivery

1. Open the **CloudFront Console** → Click **Create distribution**.
2. Configure the **Origin**:
   - Origin Domain: Select your S3 bucket.
   - Origin Access: Set to **Public**.
   - Viewer Protocol Policy: **Redirect HTTP to HTTPS**
3. Set **Alternate Domain Names (CNAMEs)**: Add your domain (`example.com`).
4. Configure **SSL**:
   - Request or import a certificate from **AWS Certificate Manager (ACM)**.
   - Validate using **email or DNS**.
   - Attach the certificate once it’s issued.
5. Click **Create distribution** and wait for deployment.

---

## 🧾 Step 7: Update Route 53 DNS Records

1. Go to **Route 53 → Hosted Zone → [your domain]**.
2. Click **Create record**.
3. Choose **Simple routing** → Define simple record.
4. Record Name: Leave empty for root domain.
5. Record Type: `A – IPv4 Address`.
6. Route traffic to: **Alias to CloudFront distribution**.
7. Choose your CloudFront distribution from the dropdown.
8. Click **Create record**.

---

## 🔍 Step 8: Verify and Test the Website

- Visit your domain in a browser: `https://example.com`
- DNS propagation may take **30 mins to a few hours**

---

## ✅ Summary

- Created S3 bucket and hosted static website files.
- Set public access and bucket policy.
- Registered and/or configured domain in Route 53.
- Created CloudFront distribution with HTTPS.
- Configured DNS to point domain to CloudFront.
- Website is live with SSL, performance, and global availability!

---

**You're all set!** 🎉 Your static site is now live using AWS best practices!
