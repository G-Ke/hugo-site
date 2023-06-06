
# Intro

![Build and Deploy](https://github.com/g-ke/hugo-site/actions/workflows/main.yml/badge.svg)

This is a simple static site hosting my personal web page, built using [Hugo](https://github.com/gohugoio/hugo) and hosted on AWS S3. It uses the [hugo-blog-awesome](https://github.com/hugo-sid/hugo-blog-awesome) theme by hugo-sid along with some styling modifications. It is built and deployed to S3 using GitHub actions, triggered by pushes to the repo. The GitHub Actions and some other notes used are listed below in the mentions.

## OIDC AWS >< GitHub

Initially I had set this up to utilize GitHub Secrets to provide the build enviornment with my AWS IAM user access keys as environment variables. Despite this being a low stakes personal website, I decided to implement the best practice instead of the just good enough option. The recommended route is to configure an OIDC IdP in AWS IAM, and use IAM roles and short-lived credentials to authenticate, instead of those user access keys. This is achieved by setting up a trusted relationship between GitHub and AWS, using GitHub's OIDC provider as a federated identity. After configuration, I added the [aws-actions/configure-aws-credentials@v2](https://github.com/aws-actions/configure-aws-credentials) GitHub action to my main.yml, along with region, IAM role to assume, and session name.

For more information on setting up OIDC and IAM Roles for GitHub Actions and AWS, checkout the resources below:

- AWS | [Use IAM roles to connect GitHub Actions to actions in AWS](https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/)

- GitHub | [Configuring OpenID Connect in Amazon Web Services](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)

- AWS | [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)

- AWS | [Creating a role for web identity or OpenID Connect Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html#idp_oidc_Create_GitHub)

- AWS | [Creating OpenID Connect (OIDC) identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)

# Mentions

Thanks to these great people and resources for making this a breeze:

- [hugo-blog-awesome theme](https://github.com/hugo-sid/hugo-blog-awesome) by hugo-sid

- [actions-hugo](https://github.com/peaceiris/actions-hugo) by peaceiris

- [aws-actions/configure-aws-credentials by AWS](https://github.com/aws-actions/configure-aws-credentials)

- [@JohnHammond](https://twitter.com/_johnhammond) for his video with the team from [Halborn](https://www.halborn.com/) called [How Can CI/CD Go Horribly Wrong?](https://www.youtube.com/watch?v=IzdWk6nA_Ho) for the OIDC best practice information.
