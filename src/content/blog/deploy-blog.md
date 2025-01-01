---
author: Nan Lin # should be replaced
pubDatetime: 2025-01-01T00:00:00Z # should be replaced
modDatetime: 2025-01-01T00:00:00.000Z
title:  Deploy a Personal Website Based on Astro # should be replaced
slug: deploy-blog # should be replaced
featured: false # check
draft: false # check
tags:
  - test # should be replaced
description:
  test description # should be replaced
---

## Table of contents

## Basic Operations

### How to Quickly Deploy a Personal Blog Like This One

1. **Prerequisites**: A Xnix machine (I personally use a MacBook).
2. Follow the [Install and set up Astro](https://docs.astro.build/en/install-and-setup/) tutorial from the [Astro official site]. Use `npm run dev` to run the site locally.
3. I use the [astro-paper](https://github.com/satnaing/astro-paper) template. Download and replace the files with content customized to your personal information.
4. Create a GitHub repository to save the code, and deploy the site on GitHub Pages by following the official guide: [Deploy your Astro Site to GitHub Pages](https://docs.astro.build/en/guides/deploy/github/).
5. Commit your work!


## Optional

### Image Hosting

Set up a (free) image hosting system based on Cloudflare and PicGo. Refer to this article for details: [Set up your free image hosting system from scratch (Cloudflare R2 + WebP Cloud + PicGo)](https://sspai.com/post/90170).


### Deploying to a Custom Domain

1. **Purchase a domain**: I use [Alibaba Cloud Domain Service](https://wanwang.aliyun.com/domain/tld?spm=5176.8048432.J_6007716750.9.1db669a3fSNiKa#.com.cn).
2. **Add DNS Records**[^1]:
   ![](https://pub-f4fb14aad5ef4ee6a83bd71292941254.r2.dev/202408141143898.png)
   - **Record Type**: Select CNAME.
   - **Host Record**: Recommend using `www`.
   - **Record Value**: Use `<username>.github.io`.
3. Go back to GitHub, open your repository's Settings, and under Pages -> Build and deployment, select **GitHub Actions**. Then, enter your custom domain in **Custom Domain**.
4. Perform a DNS check and wait a few minutes.
5. (Optional) Force HTTPS connections.
   ![](https://pub-f4fb14aad5ef4ee6a83bd71292941254.r2.dev/202408141151157.png)


## Q&A

### Potential Issues
- **`npm` Network Problems**: Refer to this article to configure a proxy: [View and change npm source addresses](https://blog.csdn.net/weixin_37861326/article/details/107064092).
- **Error with Astro's default workflow**:
  ```text
  Error: No pnpm version is specified.
  Please specify it by one of the following ways:
      - in the GitHub Action config with the key "version"
      - in the package.json with the key "packageManager"
  ```
  **Solution**: Add the following to the workflow YAML file:
  ```yaml
  - name: Install, build, and upload your site
        uses: withastro/action@v2
        with:
          path: . # The root location of your Astro project inside the repository. (optional)
          node-version: 20 # The specific version of Node that should be used to build your site. Defaults to 20. (optional)
          package-manager: pnpm@latest # The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile. (optional)
  ```

[^1]: [GitHub Docs - Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#dns-records-for-your-custom-domain)