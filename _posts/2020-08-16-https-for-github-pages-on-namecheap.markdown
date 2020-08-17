---
title:  "https on .me custom domain - github pages"
tags: [https, nc, namecheap, me, github pages]
style: fill
color: primary
description: If you've acquired a .me domain from namecheap.com and configured it point to your github pages, making the website use https will require some additional steps.
---

### 1. Follow the steps in the below.

a. [Name Cheap KB to connect NC to github pages](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages)

b. Now, in your github repository, add a file "CNAME" the contents of which are your .me domain name. 
```
$ cat CNAME
nithintsk.me
```

c. [Github KB to make the domain use https instead of http](https://docs.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https)

d. Try accessing your .me domain, if it successfully redirects to https, you can stop reading further.

### 2. If you weren't able to enable https redirection in github settings

#### Did you get the following error?
![Github Error](/img/github-error.jpg)

This is likely because you have two extra A records in you advanced DNS settings on NameCheap.com

Delete the A records with the ips below:
- 192.30.252.153
- 192.30.252.154

After deletion, try to reenable https redirection in the repository settings on Github. It worked for me.

Now you should be able to use your .me domain to access your github pages website.

---
