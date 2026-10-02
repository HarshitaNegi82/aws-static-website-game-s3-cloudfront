# aws-static-website-game-s3-cloudfront
Static Snake Game deployed on AWS using Amazon S3 and CloudFront, with documented architecture, deployment steps, and AWS lab troubleshooting.

# Snake Game — AWS S3 + CloudFront

A classic Snake Game deployed as a static web application using Amazon S3 for object storage and Amazon CloudFront for content delivery.

This project demonstrates the deployment of a static web application on AWS using a private S3 origin and CloudFront as the delivery layer.

<p align="center">
  <img src="images/game_display.png" alt="Snake Game deployed through Amazon CloudFront" width="750">
</p>

## Live Demo

[Play the Snake Game](https://d9rdsyjskw9cj.cloudfront.net/index.html)

## Project Overview

The objective of this project was to deploy a simple static web application on AWS and understand the relationship between object storage and content delivery.

The application files are stored in an Amazon S3 bucket and delivered to users through an Amazon CloudFront distribution.

### Architecture

```text
                    User Browser
                         |
                         | HTTPS
                         v
                 Amazon CloudFront
                  (Content Delivery)
                         |
                         | Origin Request
                         v
                    Amazon S3
                  (Static Assets)
                         |
                  +------+------+
                  |             |
              index.html    style.css
