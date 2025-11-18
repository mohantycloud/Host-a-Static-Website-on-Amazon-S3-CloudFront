# Host-a-Static-Website-on-Amazon-S3-CloudFront


                       +----------------------------+
                       |    Users / Web Browsers    |
                       +-------------+--------------+
                                     |
                                     | HTTPS Request
                                     v
                         +-----------+------------+
                         |     Amazon CloudFront  |
                         |   (Global CDN Layer)   |
                         +-----------+------------+
                                     |
                                     | Fetches content from origin
                                     v
                +--------------------+----------------------+
                |  Amazon S3 Bucket (Static Website Files)  |
                |  HTML | CSS | JS | Images | Assets        |
                +--------------------+----------------------+
                                     |
                                     | Secure Access via
                                     | Origin Access Control (OAC)
                                     v
                       +-------------------------------+
                       |  S3 Bucket Policy (OAC-based) |
                       +-------------------------------+


## Key Components Explained

`1. Users`

Visitors access your website via a browser. Requests are automatically routed to the nearest CloudFront edge location.

`2. Amazon CloudFront`

Acts as a global CDN

Provides caching for faster load times

Serves content via HTTPS using an SSL certificate

Protects against DDoS via AWS Shield (built-in)

`3. Amazon S3 Bucket`

Stores static website assets (HTML, CSS, JavaScript, images)

Cost-efficient, scalable, and highly durable

Not publicly accessible when using CloudFront + OAC

`4. Origin Access Control (OAC)`

Ensures CloudFront is the only entity allowed to access the S3 bucket

Prevents direct public access to S3


# PART 1 — Create S3 Static Website Bucket

## Step 1. Create an S3 bucket

Bucket name must be globally unique.

Example: `mohanty-static-website-demo-2025`

### AWS Console Steps

Go to S3 Console

Click Create bucket

Enter a unique name

Region: your choice (e.g., us-east-1)

Uncheck “Block all public access”

Check I acknowledge this bucket will be public

Create bucket

## Step 2. Enable static website hosting

Open bucket → Properties tab

Scroll to Static website hosting

Enable

Choose:

Index document: index.html

Error document: error.html

## Step 3. Upload website files

index.html

error.html

style.css

images/

scripts/

## Step 4. Add bucket policy (public read)

Replace <bucket-name>:

Bucket Policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-website-demo-2025/*"
    }
  ]
}
```



PART 2 — Create CloudFront Distribution (Recommended)

CloudFront gives:
✔ HTTPS (SSL)
✔ CDN caching
✔ Faster global speed
✔ Hide S3 bucket URL

Step 1. Create CloudFront Distribution
Console Steps

Go to CloudFront

Create Distribution

Origin Domain → choose your S3 bucket

Origin access control (OAC) → Create new OAC

Click Update bucket policy

Set:

Viewer Protocol Policy: Redirect HTTP to HTTPS

Allowed HTTP Methods: GET, HEAD

Cache Policy: CachingOptimized

Scroll down:

Default Root Object
index.html


Create distribution.

Step 2. Wait for CloudFront deployment

It may take 5–10 minutes.

You’ll get a URL like:

https://d1ab23xyz.cloudfront.net


Your static site is now online.

🟧 PART 3 — (Optional) Configure Custom Domain with Route 53

If you own a domain like mywebsite.com:

Steps:

Go to ACM (Certificate Manager)

Request certificate:

mywebsite.com

www.mywebsite.com

Validate via DNS

In CloudFront → Edit Distribution

Add Alternate Domain Names (CNAMEs)

Attach the SSL certificate

Go to Route 53 → Hosted zone

Create A Record → Alias → CloudFront distribution

Your site will load at:

https://mywebsite.com
