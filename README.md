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

1. Users

Visitors access your website via a browser. Requests are automatically routed to the nearest CloudFront edge location.

2. Amazon CloudFront

Acts as a global CDN

Provides caching for faster load times

Serves content via HTTPS using an SSL certificate

Protects against DDoS via AWS Shield (built-in)

3. Amazon S3 Bucket

Stores static website assets (HTML, CSS, JavaScript, images)

Cost-efficient, scalable, and highly durable

Not publicly accessible when using CloudFront + OAC

4. Origin Access Control (OAC)

Ensures CloudFront is the only entity allowed to access the S3 bucket

Prevents direct public access to S3
