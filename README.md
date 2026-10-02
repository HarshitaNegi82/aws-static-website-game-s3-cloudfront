# aws-static-website-game-s3-cloudfront
Static Snake Game deployed on AWS using Amazon S3 and CloudFront, with documented architecture, deployment steps, and AWS lab troubleshooting.


<p align="center">
  <img src="images/game_display.png" alt="Snake Game deployed through Amazon CloudFront" width="750">
</p>
## Architecture

```text
                    User Browser
                         |
                         | HTTPS
                         v
                 Amazon CloudFront
                  (CDN / Delivery)
                         |
                         | Origin Request
                         v
                    Amazon S3
                 (Static Assets)
                         |
                 +-------+-------+
                 |               |
             index.html      style.css
```

### Request Flow

1. The user requests the game through the CloudFront distribution URL.
2. CloudFront receives the request and acts as the content delivery layer.
3. CloudFront retrieves the requested object from the S3 origin.
4. The browser receives the static files and renders the game.

## AWS Services Used

| Service | Role |
|---|---|
| Amazon S3 | Stores the static website files |
| Amazon CloudFront | Delivers the website content through a CDN |

## Implementation

### 1. Amazon S3

A general-purpose S3 bucket named `cfgame1234` was created for the project.

The bucket contains the static website assets:

```text
index.html
style.css
```

The S3 configuration used in the lab included:

- Block all public access enabled
- Static website files uploaded to the bucket
- S3 used as the origin for the CloudFront distribution

### 2. Amazon CloudFront

A CloudFront distribution was created with the S3 bucket configured as the origin.

The deployed distribution is:

```text
https://d9rdsyjskw9cj.cloudfront.net
```

The game is accessed using:

```text
https://d9rdsyjskw9cj.cloudfront.net/index.html
```

## Security Configuration

The S3 bucket was configured with **Block all public access enabled** in the lab environment.

This keeps the S3 bucket from being directly exposed through public bucket access. The application is accessed through the CloudFront distribution.

The exact CloudFront-to-S3 authorization mechanism should be documented according to the configuration actually enabled on the distribution. This repository does not claim OAI/OAC unless it is verified from the distribution configuration.

## Engineering Challenge: Restricted CloudFront Permissions

### Problem

The AWS Skill Builder lab environment used a restricted IAM identity.

When attempting to configure the CloudFront **Default Root Object** as `index.html`, the operation was blocked because the lab identity did not have the required `cloudfront:UpdateDistribution` permission.

As a result, the root URL:

```text
https://d9rdsyjskw9cj.cloudfront.net/
```

could not be configured to automatically resolve to `index.html`.

### Workaround

Instead of modifying IAM permissions or attempting to bypass the lab restrictions, the exact object path was requested directly:

```text
https://d9rdsyjskw9cj.cloudfront.net/index.html
```

This allowed the application to be successfully delivered through CloudFront without requiring a distribution update.

## Screenshots

### Deployed Game

<p align="center">
  <img src="docs/images/game_display.png" alt="Snake Game deployed through Amazon CloudFront" width="750">
</p>

### CloudFront Distribution

<p align="center">
  <img src="docs/images/cloudfront.png" alt="Amazon CloudFront distribution configuration" width="750">
</p>

### S3 Bucket

<p align="center">
  <img src="docs/images/bucket_created.png" alt="Amazon S3 bucket created for the project" width="750">
</p>

### S3 Objects

<p align="center">
  <img src="docs/images/objects_created.png" alt="Static website objects stored in Amazon S3" width="750">
</p>

## Project Structure

```text
snake-game-s3-cloudfront/
├── website/
│   ├── index.html
│   └── style.css
├── docs/
│   └── images/
│       ├── bucket_created.png
│       ├── cloudfront.png
│       ├── game_display.png
│       └── objects_created.png
├── .gitignore
└── README.md
```

## Key Takeaways

- Practiced creating and configuring an Amazon S3 bucket.
- Stored static website assets in S3.
- Created an Amazon CloudFront distribution with S3 as the origin.
- Understood the relationship between object storage and CDN-based content delivery.
- Worked with S3 Block Public Access settings.
- Troubleshot a CloudFront configuration issue caused by restricted IAM permissions.
- Used a direct object path as a practical workaround without changing restricted lab permissions.

## Future Improvements

- Configure a custom domain for the CloudFront distribution.
- Configure the CloudFront Default Root Object in an account with the required permissions.
- Evaluate AWS WAF for a production-oriented deployment.
- Enable appropriate CloudFront logging and monitoring.
- Explore automated deployment through CI/CD.

## Learning Environment

This project was implemented as hands-on practice using AWS Skill Builder.

The configuration and permissions available in a learning lab can differ from those available in a production AWS account.



