### 🌐 Static Website Hosting with AWS S3 + CloudFront + SSL + GoDaddy

This project hosts a static website using:
- 📦 Amazon S3 (for storage)
- 🚀 Amazon CloudFront (for global CDN and HTTPS)
- 🔐 AWS Certificate Manager (for free SSL)
- 🌍 GoDaddy (for domain DNS management)

---

## 🛠 Deployment Steps

### 1. 🪣 Create an S3 Bucket

- Name your bucket **exactly** like your domain (e.g. `www.gopendra2311.xyz`)
- Enable **Static Website Hosting**
- Upload your files (`index.html`, etc.)
- Disable “Block all public access” if using public read
- Make files publicly readable or use CloudFront OAC

### 2. 🔒 Create an SSL Certificate in ACM

- Go to AWS Certificate Manager (ACM)
- Request a **public certificate** in **us-east-1**
- Add domain names:
  - `blog.gopendra2311.xyz`
  - (or `www.gopendra2311.xyz`)
- Use **DNS validation**
- Add CNAME record in **GoDaddy DNS**
- Wait for the certificate status to become **Issued**

### 3. 🚀 Create a CloudFront Distribution

- Origin domain: `www.gopendra2311.xyz.s3.ap-south-1.amazonaws.com`
- Viewer Protocol Policy: Redirect HTTP to HTTPS
- SSL Certificate: Choose the one from ACM
- Alternate domain name (CNAME): `blog.gopendra2311.xyz`
- Default root object: `index.html`
- (Optional) Enable **Origin Access Control (OAC)** for private S3 access

### 4. 🌐 Configure GoDaddy DNS

- Go to your GoDaddy account > DNS Management
- Add a CNAME record:
  - **Name**: `blog`
  - **Type**: `CNAME`
  - **Value**: `your-cloudfront-domain.cloudfront.net`

Example:

Name: blog
Type: CNAME
Value: d2x7kxcq6c1uq7.cloudfront.net

yaml
Copy
Edit

---

## ✅ Test Your Site

After DNS propagation (can take a few minutes):

Visit:

https://blog.gopendra2311.xyz

pgsql
Copy
Edit

You should see your static site served securely over HTTPS 🚀

---

## 🔐 Optional: S3 Bucket Policy (Public Access)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::www.gopendra2311.xyz/*"
    }
  ]
}
