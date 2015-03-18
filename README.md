Buildpack for meteor on cloudcontrol
====================================

This is a [buildpack](https://www.cloudcontrol.com/dev-center/Platform%20Documentation#buildpacks-and-the-procfile) for
[meteor apps](http://www.meteor.com).

It does expect one of the mongo database ADDONs to be added to deploy correctly.

Usage
-----

This is a buildpack for meteor applications. In case you want to introduce some changes, fork the buildpack,
apply changes and test it via [custom buildpack feature](https://www.cloudcontrol.com/dev-center/Guides/Third-Party%20Buildpacks/Third-Party%20Buildpacks):

~~~bash
$ cctrlapp APP_NAME create custom --buildpack https://github.com/aheissenberger/cloudcontrol-buildpack-meteor.git
~~~

Example
-------
~~~bash
$ meteor create --example wordplay
$ cd wordplay
$ cctrlapp APP_NAME create custom --buildpack https://github.com/aheissenberger/cloudcontrol-buildpack-meteor.git
$ git init && git add . && git commit -m 'init'
$ cctrlapp APP_NAME/default addon.add mongosoup.sandbox
$ cctrlapp APP_NAME/default push
$ cctrlapp APP_NAME/default deploy
~~~

oplog support
-------------
oplog support is only available with the [MongoLab's ADDON](https://www.cloudcontrol.com/add-ons/mongolab) except for the sandbox version.

1. [Create a user with oplog read access](http://docs.mongolab.com/oplog/)
2. add the credetials USERNAME:PASSWORD to the deployment
~~~bash
$ cctrlapp APP_NAME/default addon.add mongolab.sharedsinglesmall
$ cctrlapp APP_NAME/default config.add MONGO_OPLOG_CRED=USERNAME:PASSWORD
$ cctrlapp APP_NAME/default deploy
~~~
do not forget to remove all other mongo database addons - e.g.
~~~bash
$ cctrlapp APP_NAME/default addon.remove mongosoup.sandbox
~~~
if you switch to a MonoLab Sandbox remove the config:
~~~bash
$ cctrlapp APP_NAME/default config.remove MONGO_OPLOG_CRED
~~~

change ROOT_URL (OPTIONAL - defaults to http://APPNAME.cloudcontrolled.com )
------------------------------------

from inside meteor [http://docs.meteor.com/#meteor_absoluteurl]:
```javascript
Meteor.absoluteUrl.defaultOptions.rootUrl = "http://mydomain.com"
```
or by overiding ENVIROMENT Variable ROOT_URL

add ENVIROMENT Variables (OPTIONAL - defaults to stable version)
------------------------------------

you can add them by creating e.g. a file with appname.sh in a directory under your app root directory.
e.g.:
```bash
mkdir APPDIR/.profile.d
echo "export MAIL_URL='smtp://user:password@mailhost:port/'" > APPDIR/.profile.d/APPNAME.sh
```

Node.js and npm versions (OPTIONAL - defaults to stable version)
------------------------------------

You can specify the versions of Node.js and npm your application requires using `package.json`

```json
{
  "engines": {
    "node": "~0.10.13",
    "npm": "~1.3.2"
  }
}
```

To list the available versions of Node.js and npm, see these manifests:

- [node.js verions](http://cloudcontrolled.com.packages.s3.amazonaws.com/buildpack-nodejs/manifest.nodejs)
- [npm.js verions](http://cloudcontrolled.com.packages.s3.amazonaws.com/buildpack-nodejs/manifest.npm)


This is a [buildpack](https://www.cloudcontrol.com/dev-center/Platform%20Documentation#buildpacks-and-the-procfile) for
[meteor apps](http://www.meteor.com).