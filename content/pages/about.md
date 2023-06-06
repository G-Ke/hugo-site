---
title: "About"
date: 2023-05-18T16:55:48-04:00
draft: false
---
# About this site
Thanks for visiting my personal site. This is a project that I undertook to help learn about Hugo, get some more experience working with AWS, and most importantly to try out using OIDC to securely access AWS resources by creating a trusted relationship between AWS and my GitHub repo. I'll likely use the site to experiment with new things in the future.

## Overview
The site is built using Hugo. I use GitHub actions to build and deploy the site to AWS S3 as a static website. The S3 bucket is cached using AWS Cloudfront. Instead of using the built in `Hugo deploy` command I use OIDC to authorize and generate short-lived credentials to assume an IAM Role.Then I can use the AWS CLI to perform an `S3 sync --delete` and Cloudfront distribution invalidation.

## Hugo
The site is made using an amazing SSG (static site generator) [Hugo](https://gohugo.io/) and the [hugo-blog-awesome](https://github.com/hugo-sid/hugo-blog-awesome) theme with some modifications. I chose Hugo because it seemed like a great way to host a simple static personal page and blog. I like using toml/yaml files for configuration and writing markdown for content instead of using a full CMS w/ UI like I might build with [Wagtail](https://wagtail.org/).

Working with Hugo was fairly straightforward. There was certainly a learning curve, and I still have a ton to learn, but being able to spin up a website was mostly drama free. I didn't make any major changes with Hugo aside from the theme and some accompanying styling changes. I may write a blog post about my process from start to finish, but there are already a lot of great resources on building a basic Hugo site.

## OIDC with GitHub and AWS
One of the big things I changed since my first build of the site was to implement OIDC between GitHub and AWS. Most tutorials will show you how to deploy your site using your AWS IAM User Access Keys as secrets and providing them as enviornment variables. This is probably okay for most basic websites, but is not the best practice for accessing your AWS resources from a security perspective.

The recommended route is to configure an OIDC IdP in AWS, and use IAM roles and short-lived credentials to authenticate, instead of user access keys. This is achieved by setting up a trusted relationship between GitHub and AWS, using GitHub's OIDC provider as a federated identity in AWS.

For more information on setting up OIDC and IAM Roles for GitHub Actions and AWS, checkout the resources below:

- AWS | [Use IAM roles to connect GitHub Actions to actions in AWS](https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/)
- GitHub | [Configuring OpenID Connect in Amazon Web Services](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
- AWS | [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
- AWS | [Creating a role for web identity or OpenID Connect Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html#idp_oidc_Create_GitHub)
- AWS | [Creating OpenID Connect (OIDC) identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)