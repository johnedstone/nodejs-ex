Node.js sample app on OpenShift!
-----------------

### This branch demonstrates the following ###
The template `openshift/templates/nodejs.json` has been modified. To handle the proxy variable.

- using http proxy
- using branch
- using GIT keys
- Updated s2i http_proxy in bc to NPM_HTTP_PROXY

### Creating a new project in Origin/Openshift ###

```
oc login
oc new-project simple-nodejs

oc secrets new-sshauth sshsecret --ssh-privatekey=$HOME/<path to key>
oc secret add serviceaccount/builder secrets/sshsecret

oc new-app -f openshift/templates/nodejs-simple-example.yaml \
--param=MEMORY_LIMIT=${MEMORY_LIMIT} \
--param=SOURCE_REPOSITORY_URL=${SOURCE_REPOSITORY_URL} \
--param=SOURCE_REPOSITORY_REF=${SOURCE_REPOSITORY_REF} \
--param=NPM_HTTP_PROXY=${NPM_HTTP_PROXY} \
--param=NPM_HTTPS_PROXY=${NPM_HTTPS_PROXY}
```
