---
published: false
---

I used this quide to install node and npm on fedora 23 box: [Installing Node.js via package manager | Node.js](https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora)

```
sudo su -
curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -
dnf install nodejs
```

On 2016-Apr-13, the result was installation of the `nodejs x86_64 5.10.1-1nodesource.fc23` package from the `nodesource` repository. `npm` version is `3.8.3`. `node` version is `5.10.1`.

