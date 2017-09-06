# medialist-galaxy

> Scripts to deploy Medialist to Meteor Galaxy

Ansible scripts that run on the local machine, to add some consistency around how
Medialist is configured, built and deployed to **Meteor Galaxy**.

For deploying to a VM checkout https://github.com/Medialist/medialist-infrastructure

## Install

This project assumes it'll be run on a developers machine and requires

- `meteor`
- `ansible` >= v2.1.2
- That you know the vault password for the deploy secrets
- That you can clone the Medialist/medialist-app2 repo

## Usage

```
ansible-playbook -i <env> deploy.yml [--limit "<host list>"]
```

Where
- `env` is either `next` or `production`
- `host list` is 1 or more domains to deploy to. Defaults to all for that inventory if omitted.

e.g. Deploying to 1 production domain:

```
ansible-playbook -i production deploy.yml --limit "bm.medialist.io"
```

**Deploy to next**

```sh
ansible-playbook -i next deploy.yml
```

That'll checkout medialist app code at HEAD into ./.build/src, install the npm deps, create a `./.build/settings.next.json` file with all the secrets merged, build the app bundle and deploy it to Galaxy.

**Deploy to all production domains**

```sh
ansible-playbook -i production deploy.yml
```

## Boostrapping a new deployment

You need to sort out the following

- Add DNS for subdomain https://cloud.digitalocean.com/networking/domains/medialist.io?i=d65447
- Add admin and oplog user to db: https://cloud.mongodb.com/v2/593eec469701996e607da978#clusters/security/users
- Create Twitter app credentials
- Create upload care bucket
- Create mailgun log in
- Create a config file in the `host_vars` dir, add in the service details.

After initial deploy:

- Enable "professional" mode in the Galaxy app settings
- Copy the external ip addresses from Galaxy to Mongo Atlas ip whitelist
- Scale the deployment to 2x 1GB containers
