  

Meteorupdev is a meteor enhancement tools.

[中文使用说明](https://github.com/meteorup/meteorupdev/blob/master/README.zh_CN.md)

Starting an application in production mode is as easy as:

```bash
$ meteorupdev push
```

## Install Meteorupdev

```bash
$ npm install meteorupdev -g
```

*npm is a builtin CLI when you install Node.js - [Installing Node.js with NVM](https://keymetrics.io/2015/02/03/installing-node-js-and-io-js-with-nvm/)*

## Configuration runtime environments on your server

```bash
$ meteorupdev setup
```

install on your server NVM, nodejs, pm2, mongodb

## Deployment a project to your server

```bash
$ meteorupdev push
or 
$ meteorupdev push -s // meteor build --server-only
```

Visit ROOT_URL after a successful deployment

## Setup and Deploy to your server for config file
append in package.json file.
```js

    "server": {
        "host": "182.92.11.131",
        "username": "root",
        "//password": "password",
        "//":" or pem file (ssh based authentication)",
        "//": "WARNING: Keys protected by a passphrase are not supported",
        "pem": "~/.ssh/id_rsa",
        "//":" Also, for non-standard ssh port use this",
        "sshOptions": { "port" : 22 },
        "//":" server specific environment variables",
        "env": {}
    },
    "setup": {
		"//": "Install MongoDB on the server. Does not destroy the local MongoDB on future setups",
		"mongo": true,
		"//": "Application server path .  must in /usr /opt /home /alidata directory.",
		"path": "/usr/local/meteorupdev"
    },
    "deploy": {
		"//": "Application name (no spaces).",
		"appName": "best",
		"//": "Configure environment",
		"//": "ROOT_URL must be set to your correct domain (https or http)",
	    "env": {
			"YJENV": "test", // customize environment
            "MONGO_URL": "mongodb://127.0.0.1:27017/best",
			"PORT": 8181,
			"ROOT_URL": "http://182.92.11.131:8181"
		}
    },
    "notice": "Well done"

```

## Print logs on server

```bash
$ meteorupdev logs
or
$ meteorupdev logs -l 100
```

## Connection to a remote mongo database

```bash
$ meteorupdev mongo
```


## Custom completion notice voice
config in package.json file. default would say “finished”
```js
"notice": "Well done"
```


## Update Meteorupdev

```bash
# reinstall latest meteorupdev version
$ npm install meteorupdev -g
```

#FAQ

### sudo: sorry, you must have a tty to run sudo

```
sudo vi /etc/sudoers
#Default requiretty
```
either comment it out the line or delete the line


## License

Meteorupdev is made available under the terms of the MIT License (MIT)

