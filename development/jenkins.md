# Jenkins

## Script Console <a id="script-console"></a>

**List pending script approvals**

[https://github.com/cloudbees/jenkins-scripts/blob/master/pipeline-approval-scripts.groovy](https://github.com/cloudbees/jenkins-scripts/blob/master/pipeline-approval-scripts.groovy)

**Get secret token set as environment variable**

```text
def envVars = Jenkins.instance.getGlobalNodeProperties()[0].getEnvVars() 
println envVars['MY_SECRET_TOKEN']
```

**Decrypt tokens**

1. Go to the "Configure System" screen.
2. Right click on a password field, Inspect element, then change to "text" instead of "password".
3. Copy that field and then go to the Script Console. Paste the below into the field, replace with your encrypted password, and then hit "run".

```text
encrypted_pw = '{your_encrypted_password_with_brackets_around_it}'
passwd = hudson.util.Secret.decrypt(encrypted_pw)
println(passwd)
```

## Jenkins in Docker <a id="jenkins-in-docker"></a>

### Remove plugins from pre-baked Jenkins docker image <a id="remove-plugins-from-pre-baked-jenkins-docker-image"></a>

Three steps:

1. `remove_plugins.sh`
2. `remove_plugins_list.txt`
3. add to `Dockerfile`

**Step 1**:

```text
#!/usr/bin/env bash
set -x

#JENKINS_PLUGINS_PATH="${JENKINS_PLUGINS_PATH:-/usr/share/jenkins/ref/plugins}"
JENKINS_PLUGINS_PATH="${JENKINS_PLUGINS_PATH:-/usr/share/jenkins/ref/plugins}"
REMOVE_PLUGINS_LIST_PATH="${REMOVE_PLUGINS_LIST_PATH:-/usr/share/jenkins/ref/remove_plugins_list.txt}"
echo $JENKINS_PLUGINS_PATH
echo $REMOVE_PLUGINS_LIST_PATH

for line in `cat ${REMOVE_PLUGINS_LIST_PATH}`
do
    echo "$line"
    find ${JENKINS_PLUGINS_PATH}/ -name ${line}\* | xargs rm -rf
done

# testing locally:
# JENKINS_PLUGINS_PATH=./test REMOVE_PLUGINS_LIST_PATH=./remove_plugins_list.txt ./remove_plugins.sh
```

**Step 2**:

Include this in a file titled `remove_plugins_list.txt`:

```text
active-directory
anchore
checkmarx
gatling
google
hipchat
kubernetes
ldap
p4
```

**Step 3**:

Add this to the Dockerfile:

```text
RUN sh /usr/share/jenkins/ref/remove_plugins.sh
```

## Keeping Jenkins Plugins up to date <a id="keeping-jenkins-plugins-up-to-date"></a>

```text
jenkins.model.Jenkins.getInstance().getUpdateCenter().getSites().each { site ->
  site.updateDirectlyNow(hudson.model.DownloadService.signatureCheck)
}

hudson.model.DownloadService.Downloadable.all().each { downloadable ->
  downloadable.updateNow();
}

def plugins = jenkins.model.Jenkins.instance.pluginManager.activePlugins.findAll {
  it -> it.hasUpdate()
}.collect {
  it -> it.getShortName()
}

println "Plugins to upgrade: ${plugins}"
long count = 0

jenkins.model.Jenkins.instance.pluginManager.install(plugins, false).each { f ->
  f.get()
  println "${++count}/${plugins.size()}.."
}

if(plugins.size() != 0 && count == plugins.size()) {
  jenkins.model.Jenkins.instance.safeRestart()
}
```

