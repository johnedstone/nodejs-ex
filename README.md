Node.js sample app on OpenShift!
-----------------

### branch001 ###
 - Cloned from: https://github.com/openshift/nodejs-ex

Demonstrating ...

- using a template to start a new project: `openshift/templates/nodejs-simple-example.yaml`
- using http proxy
- using a git branch
- using GIT keys
- Updated s2i http_proxy in the custom assemble script (Build Config) to use NPM_HTTP_PROXY instead of http_proxy, for the sake of clarity.

### Creating a new project in Origin/Openshift ###

```
oc login
oc new-project simple-nodejs

oc secrets new-sshauth sshsecret --ssh-privatekey=$HOME/<path to key>
oc secret add serviceaccount/builder secrets/sshsecret

source env_vars.sh

oc new-app -f openshift/templates/nodejs-simple-example.yaml \
--param=SOURCE_REPOSITORY_URL=${SOURCE_REPOSITORY_URL} \
--param=SOURCE_REPOSITORY_REF=${SOURCE_REPOSITORY_REF} \
--param=NPM_HTTP_PROXY=${NPM_HTTP_PROXY} \
--param=NPM_HTTPS_PROXY=${NPM_HTTPS_PROXY}

# watch pods
oc get pods -w

# watch build config
oc logs -f bc/nodejs-simple-example

# watch deploy config
oc logs -f dc/nodejs-simple-example

# get URL
oc get route

## Let's update index.html and rebuild
sed -i 's/Node.js application/very special Node.js application/' views/index.html
git commit -am "updated index.html"
git push
oc start-build nodejs-simple-example

# Or after a local change w/o pushing to git, do a start-build
sed -i 's/very special Node.js application/very very special Node.js application/' views/index.html
oc start-build nodejs-simple-example --from-dir=.

# Clear most everything in project except secrets and then do oc new-app ...
oc delete all --all
oc new-app -f .... <see above>

# Debug
oc get events
```
