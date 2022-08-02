# CI/CD(Jenkins) for Pet Project (hw22)

To start the app run this coommand in your teminal

```
make up 
```

or

```
docker-compose up --build
```

After build you need navigate to this [link](http://localhost:8080) and make basic setup for Jenkins.

>Possible creds is: 
>login: root
>pwd: Qwerty123

Also you will need to create Node.js config in [Global Tool Configuration](http://localhost:8080/configureTools/).

Then you need setup GitHub [weebhook](https://hookdeck.com/webhooks/platforms/tutorial-github-webhooks#receive-and-inspect-webhooks-and-payload) in your repository.

> To make it work with local jenkins I have used [Ngrok](https://ngrok.com/) tool to make viisble jenkins for Github webhooks 

### Config and Screenshots of system

#### Config for PetProject

``` 
<?xml version='1.1' encoding='UTF-8'?>
<project>
  <actions/>
  <description></description>
  <keepDependencies>false</keepDependencies>
  <properties/>
  <scm class="hudson.plugins.git.GitSCM" plugin="git@4.11.4">
    <configVersion>2</configVersion>
    <userRemoteConfigs>
      <hudson.plugins.git.UserRemoteConfig>
        <url>https://github.com/dmytrox/hw22-pet-project.git</url>
      </hudson.plugins.git.UserRemoteConfig>
    </userRemoteConfigs>
    <branches>
      <hudson.plugins.git.BranchSpec>
        <name>*/master</name>
      </hudson.plugins.git.BranchSpec>
    </branches>
    <doGenerateSubmoduleConfigurations>false</doGenerateSubmoduleConfigurations>
    <submoduleCfg class="empty-list"/>
    <extensions/>
  </scm>
  <canRoam>true</canRoam>
  <disabled>false</disabled>
  <blockBuildWhenDownstreamBuilding>false</blockBuildWhenDownstreamBuilding>
  <blockBuildWhenUpstreamBuilding>false</blockBuildWhenUpstreamBuilding>
  <triggers>
    <com.cloudbees.jenkins.GitHubPushTrigger plugin="github@1.34.5">
      <spec></spec>
    </com.cloudbees.jenkins.GitHubPushTrigger>
  </triggers>
  <concurrentBuild>false</concurrentBuild>
  <builders>
    <hudson.tasks.Shell>
      <command>npm i 
npm run test</command>
      <configuredLocalRules/>
    </hudson.tasks.Shell>
  </builders>
  <publishers/>
  <buildWrappers>
    <jenkins.plugins.nodejs.NodeJSBuildWrapper plugin="nodejs@1.5.1">
      <nodeJSInstallationName>NodeJs</nodeJSInstallationName>
      <cacheLocationStrategy class="jenkins.plugins.nodejs.cache.DefaultCacheLocationLocator"/>
    </jenkins.plugins.nodejs.NodeJSBuildWrapper>
  </buildWrappers>
</project>
```
![image](https://user-images.githubusercontent.com/39772493/182432987-77507fd3-79ef-4734-b998-2eaa0098df8f.png)

#### GitHub Webhook

![image](https://user-images.githubusercontent.com/39772493/182433381-aa1bb0b5-b404-481c-aa1b-01a26f625da1.png)

### Build Triggered by push 

![image](https://user-images.githubusercontent.com/39772493/182434232-c5aebdc9-8b4e-4752-82fc-0335cc35030e.png)
 
```
Started by GitHub push by dmytrox
Running as SYSTEM
Building in workspace /var/jenkins_home/jobs/PetProject/workspace
The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository https://github.com/dmytrox/hw22-pet-project.git
 > git init /var/jenkins_home/jobs/PetProject/workspace # timeout=10
Fetching upstream changes from https://github.com/dmytrox/hw22-pet-project.git
 > git --version # timeout=10
 > git --version # 'git version 2.30.2'
 > git fetch --tags --force --progress -- https://github.com/dmytrox/hw22-pet-project.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://github.com/dmytrox/hw22-pet-project.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision dcf5048c406bf6d70a89f3ed153236380c785c65 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f dcf5048c406bf6d70a89f3ed153236380c785c65 # timeout=10
Commit message: "changed text in response"
First time build. Skipping changelog.
Unpacking https://nodejs.org/dist/v18.7.0/node-v18.7.0-linux-x64.tar.gz to /var/jenkins_home/tools/jenkins.plugins.nodejs.tools.NodeJSInstallation/NodeJs on Jenkins
[workspace] $ /bin/sh -xe /tmp/jenkins17927715815459971043.sh
+ npm i

added 141 packages, and audited 142 packages in 2s

26 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
+ npm run test

> app@1.0.0 test
> mocha --reporter spec



  Basic numbers operations

    ✔ 1 + 1 = 2
    ✔ 10 - 6 = 4
    ✔ 2 * 4 = 8
    ✔ 8 / 2 = 4


  4 passing (4ms)

Finished: SUCCESS
```

