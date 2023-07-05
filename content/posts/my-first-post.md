---
title: "Welcome! Some notes about the site."
date: 2023-05-16T03:24:11-04:00
draft: false
---

## Welcome

I'm George and thanks for checking out my site!

I'm not going to talk about myself here, except to say I appreciate you taking time out of your day to visit. I'll be using this website to post about any projects I'm working on, and other stuff of interest. The rest of this post will be an overview of the site and the decisons I made when constructing it.

## About the site

I set out to replace my old site and after some looking I found Hugo. Hugo seemed like an obvious choice for a lightweight personal site. I've used heavier frameworks for my site in the past, but I always felt like it was overkill, even if it was helpful practice. So I chose the best tool for the job: a Static Site Generator like Hugo.

This is a simple static site hosting my personal web page, built using Hugo and hosted on AWS S3. It uses the hugo-blog-awesome theme by hugo-sid along with some styling modifications. It is built and deployed to S3 using GitHub actions, triggered by pushes to the repo. The GitHub Actions and some other notes used are listed below in the mentions.


## GitHub Actions

To keep with the theme of simplicity I opted to deploy the site to AWS using GitHub Actions. Hugo is commonly deployed using Actions, so there was already a common workflow and plenty of examples to reference.

**The stages of the workflow are as follows:**

1. Checkout the repo
2. Setup Hugo
3. Build Hugo
4. Configure AWS credentials *(notes below on OIDC)*
5. Push the site to S3 and invalidate the Cloudfront cache

### Configuring OIDC for AWS & GitHub

Initially I had set this project up to utilize GitHub Secrets to provide the build enviornment with my AWS IAM user access keys as environment variables.

Despite this being a low stakes personal website, I decided to implement the best practice instead of the just good enough option. The recommended route is to configure an OIDC IdP in AWS IAM, and use IAM roles and short-lived credentials to authenticate, instead of those user access keys.

This is achieved by setting up a trusted relationship between GitHub and AWS, using GitHub's OIDC provider as a federated identity. After configuration, I added the [aws-actions/configure-aws-credentials@v2](https://github.com/aws-actions/configure-aws-credentials) GitHub action to my main.yml, along with region, IAM role to assume, and session name.

#### OIDC Resources

For more information on setting up OIDC and IAM Roles for GitHub Actions and AWS, checkout the resources below:

- GitHub | [Configuring OpenID Connect in Amazon Web Services](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)

- AWS | [Use IAM roles to connect GitHub Actions to actions in AWS](https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/)

- AWS | [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)

- AWS | [Creating a role for web identity or OpenID Connect Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html#idp_oidc_Create_GitHub)

- AWS | [Creating OpenID Connect (OIDC) identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)

## AWS

Hugo has built in support for deploying to AWS S3. I opted to use the AWS CLI and the OIDC credentials in the Actions workflow to perform an `S3 Sync` and cloudfront `create-invalidation`. I found the CLI to be a bit more reliable and customizable.

### S3

The site is hosted in a public S3 bucket with Static Website Hosting Enabled. Nothing too special here.

### Route53

Another straight forward step. I purchased a domain in Route53 and created an A record pointing to the Cloudfront distribution as noted below.

### Cloudfront

In Cloudfront I created a distribution with the S3 bucket as the origin and S3 Static Website as the Origin Type. To enable a custom domain I added an Alternate Domain Name (CNAME record), created a custom SSL certificate in ACM, and set HTTP requests to route to HTTPS. I had originally used AWS WAF and configured that here as well, but it was too expensive for my needs.

## Roadmap?

I will likely tweak the theme and css to hone in on something that both looks and feels approachable, but scratches my current itch when it comes to design.

In addition to look and feel tweaks, I'll be adding more pages and expanding my knowledge of Hugo.
