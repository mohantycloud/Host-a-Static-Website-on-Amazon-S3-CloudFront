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
