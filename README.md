# Installation Nexus

## 1. How to install and run Nexus using docker command

Docker Pull Command
```
$ docker pull sonatype/nexus3 
```

To run, binding the exposed port 8081 to the host, use:
```
$ docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

To test:
```
$ curl http://localhost:8081/
```
What we will do:
- Create a private (hosted) repository for our own packages
- Create a proxy repository pointing to the official registry
- Create a group repository to provide all the above repos under a single URL

## 2. Configuring your clients and projects to use your Nexus repos
If you have a project where you only want to download dependencies from Nexus, create a ***.npmrc*** file at your project’s root with:

```
registry=http://your-host:8081/repository/npm-group/
_auth=YWRtaW46YWRtaW4xMjM=
```

_auth=YWRtaW46YWRtaW4xMjM= is the base64 hash for the credentials (admin/admin123). If you use a different set of credentials, you should compute your own hash with:

```
echo -n 'myuser:mypassword' | openssl base64
```

If you have a project that you want to publish to your Nexus, put this in package.json:

```
{
  ...

  "publishConfig": {
    "registry": "http://your-host:8081/repository/npm-private/"
  }
}
```

Now if you run in your projects:
```
npm install
```
or
```
npm publish
```

Your npm will point to your Nexus instance.

**Original page**: https://blog.sonatype.com/using-nexus-3-as-your-repository-part-2-npm-packages
# Installing npm packages

## 2. Configuring your clients and projects to use your Nexus repos
You can replace your host and your auth in this command and then create a *.npmrc* file at your project’s root with:

```
echo -e "registry=http://your-host:8081/repository/npm-group/\n_auth=$(echo -n 'myuser:mypassword' | openssl base64)" >> .npmrc
```

Run to install your package:
```
npm install your-package
```