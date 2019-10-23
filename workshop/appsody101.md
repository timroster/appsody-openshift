# Getting started with Appsody

Appsody has a CLI tool that you need to install. There are [more details](https://appsody.dev/docs/getting-started/installation) on installation available, but the key steps are as follows by platform:

## install for MacOS

1. If you don't have the Xcode command line tools installed on your system, install them by running `xcode-select --install`
2. If you don't have Homebrew installed on your system, install it by running `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
3. To install Appsody, run:

    `brew install appsody/appsody/appsody`

## install for Windows 10

1. Create a directory for Appsody on your Windows 10 system.
2. Download the Appsody binaries for Windows from the [Appsody releases page](https://github.com/appsody/appsody/releases) into the directory. The file is named `appsody-v.r.m-windows.tar.gz`, where v.r.m indicates the release tag.
3. Extract the files by running `tar -xvf appsody-v.r.m-windows.tar.gz`.
4. To install Appsody, run the following setup command:

    `appsody-setup.bat`

## install for Ubuntu and other .deb minded distributions

1. Your user account must be a member of the docker group, which you can configure by running `sudo usermod -aG docker <username>`
2. Download the latest Debian install package from the [Appsody releases page](https://github.com/appsody/appsody/releases). The file is named `appsody_v.r.m_amd64.deb`, where v.r.m indicates the release tag.
3. To install the package, run:

    `sudo apt install -f <path>/appsody_v.r.m_amd64.deb`

## install for RHEL and other .rpm minded distributions

1. Your user account must be a member of the docker group, which you can configure by running `sudo usermod -aG docker <username>`
2. Download the latest Debian install package from the [Appsody releases page](https://github.com/appsody/appsody/releases). The file is named `appsody-v.r.m-1.x86_64.rpm`, where v.r.m indicates the release tag.
3. To install the package, run:

    `sudo apt install -f <path>/appsody-v.r.m-1.x86_64.rpm`

## Really getting started with Appsody

With appsody installed, it's time to create your first application using a stack. Explore the stacks available:

```text
appsody list

REPO        	ID                       	VERSION  	TEMPLATES        	DESCRIPTION                                                 
*appsodyhub 	java-microprofile        	0.2.18   	*default         	Eclipse MicroProfile on Open Liberty & OpenJ9 using Maven   
*appsodyhub 	java-spring-boot2        	0.3.15   	*default, kotlin 	Spring Boot using OpenJ9 and Maven                          
*appsodyhub 	nodejs                   	0.2.5    	*simple          	Runtime for Node.js applications                            
*appsodyhub 	nodejs-express           	0.2.7    	scaffold, *simple	Express web framework for Node.js                           
*appsodyhub 	nodejs-loopback          	0.1.5    	*scaffold        	LoopBack 4 API Framework for Node.js                        
*appsodyhub 	python-flask             	0.1.4    	*simple          	Flask web Framework for Python                              
*appsodyhub 	starter                  	0.1.0    	*simple          	starter sample stack that can be run by appsody, to help    
            	                         	         	                 	creation of more stacks                                     
*appsodyhub 	swift                    	0.1.4    	*simple          	Runtime for Swift applications                              
experimental	java-spring-boot2-liberty	0.1.10   	*default         	Spring Boot on Open Liberty & OpenJ9 using Maven            
experimental	nodejs-functions         	0.1.4    	*simple          	Serverless runtime for Node.js functions                    
experimental	quarkus                  	0.1.5    	*default         	Quarkus runtime for running Java applications               
experimental	rust                     	0.1.1    	*simple          	Runtime for Rust applications                               
experimental	vertx                    	0.1.3    	*default         	Eclipse Vert.x runtime for running Java applications
```

### Create your first Appsody application

Start off with the `nodejs-express` stack which will provide a runtime for node.js, the express framework and a simple application template that can be easily modified by adding additional code. To create the application, you will need to make a new directory, change into that directory, and then run the `appsody init` command.

```text
mkdir mynode
cd mynode
appsody init nodejs-express

Running appsody init...
Downloading nodejs-express template project from https://github.com/appsody/stacks/releases/download/nodejs-express-v0.2.7/incubator.nodejs-express.v0.2.7.templates.simple.tar.gz
Download complete. Extracting files from /Users/timro/msdemo/mynode/nodejs-express.tar.gz
Setting up the development environment
Your Appsody project name has been set to mynode
Pulling docker image appsody/nodejs-express:0.2
Running command: docker pull appsody/nodejs-express:0.2
0.2: Pulling from appsody/nodejs-express
Digest: sha256:36a60fa077a5e38d5275665f666c00f93e64896434d84efe8ebeab87b8c51210
Status: Image is up to date for appsody/nodejs-express:0.2
docker.io/appsody/nodejs-express:0.2
[Warning] The stack image does not contain APPSODY_PROJECT_DIR. Using /project
Running command: docker run --rm --entrypoint /bin/bash appsody/nodejs-express:0.2 -c find /project -type f -name .appsody-init.sh
Successfully initialized Appsody project
```

Use your text editor / IDE of preference to take a look through the contents of the `mynode` folder. Note the absence of a `Dockerfile` - an Appsody stack has a standardized process, set by the *stack architect* to take the application and configure it to run in a container environment with the runtime, or to build the application into a container for transfer to a container image registry.

To start the application, use the `appsody run` command from that directory:

```text
appsody run
Running development environment...
Pulling docker image appsody/nodejs-express:0.2
Running command: docker pull appsody/nodejs-express:0.2
0.2: Pulling from appsody/nodejs-express

...

[Container] Running command:  npm start
[Container] 
[Container] > nodejs-express@0.2.7 start /project
[Container] > node server.js
[Container] 
[Container] [Tue Oct 22 13:23:22 2019] com.ibm.diagnostics.healthcenter.loader INFO: Node Application Metrics 5.0.3.201908230949 (Agent Core 4.0.3)
[Container] [Tue Oct 22 13:23:23 2019] com.ibm.diagnostics.healthcenter.mqtt INFO: Connecting to broker localhost:1883
[Container] App started on PORT 3000
```

The terminal session will show any output on standard out for the application. Note that in this case, the *stack architect* has not configured any logging in addition to the express framework. Start another terminal and try this command:

```text
curl http://localhost:3000/
Hello from Appsody!
```

With no logging configured, notice that there's no output in the terminal window. You will fix this in a moment. First up, stop the `appsody run` session by typing **Ctrl-C** in the terminal:

```text
[Container] [Tue Oct 22 13:23:23 2019] com.ibm.diagnostics.healthcenter.mqtt INFO: Connecting to broker localhost:1883
[Container] App started on PORT 3000
^CRunning command: docker stop mynode-dev
Closing down, development environment was interrupted.
```

### Add logging to the application

As an example of updating the framework for the application with a developer opinion (as the stack maintainer has not provided one) let's add logging using `morgan` to this application template. Update the application dependencies (for both development and runtime) with this command:

```text
npm install morgan
+ morgan@1.9.1
added 178 packages from 578 contributors and audited 304 packages in 3.528s
found 0 vulnerabilities
```

With the dependency installed, add the logging component to the application. Open up the `app.js` file in an IDE and update it adding lines to import and apply morgan as middleware to the express app. The final version will look like:

```node
const app = require('express')()
const morgan = require('morgan')

app.use(morgan('combined'))

app.get('/', (req, res) => {
  res.send("Hello from Appsody!");
});
 
module.exports.app = app;
```

Save the file in the IDE and then from the terminal start the application again with appsody:

```text
appsody run
0.2: Pulling from appsody/nodejs-express
Digest: sha256:36a60fa077a5e38d5275665f666c00f93e64896434d84ef
...
[Container] App started on PORT 3000
```

In another terminal, repeat the `curl http://localhost:3000` command . Now you will see appended to the application output something like:

```
[Container] ::ffff:172.17.0.1 - - [22/Oct/2019:16:08:00 +0000] "GET / HTTP/1.1" 200 19 "-" "curl/7.54.0"
```

cool huh?

### Local application edits and live updates

Just as you are accustomed to live editing and application updates in some IDEs with local runtimes, you can have the same experience when using `appsody run`. It's time to extend this application with an additional endpoint. To keep things simple,we'll just add this to the `app.js` module, but this is the point where you could start extending the node.js application by adding other modules, etc.

Keeping the `appsody run` session active (no **Ctrl-C** this time), open up the `app.js` file in an IDE or editor and add the following code right after the `app.get('/'...` line (starting at line 9 or 10):

```node
app.get('/foo', (req, res) => {
  res.json({msg: 'bar'});
});
```

Save the file in the editor and look over at the terminal with `appsody run`. You'll see something like:

```text
Container] Running command:  npm start
[Container] Wait received error on APPSODY_RUN/DEBUG/TEST signal: interrupt
[Container] 
[Container] > nodejs-express@0.2.7 start /project
[Container] > node server.js
[Container] 
[Container] [Tue Oct 22 16:17:10 2019] com.ibm.diagnostics.healthcenter.loader INFO: Node Application Metrics 5.0.3.201908230949 (Agent Core 4.0.3)
[Container] [Tue Oct 22 16:17:11 2019] com.ibm.diagnostics.healthcenter.mqtt INFO: Connecting to broker localhost:1883
[Container] App started on PORT 3000
```

Test out the new endpoint:

```text
curl http://localhost:3000/foo
{"msg":"bar"}
```

As we would expect, and there's also an update to the log output from the container from the new request.

### How Appsody supports changes in local loop development

Appsody uses containers to provide the runtime and the stack for running an application. But how does this work in practice when a developer changes local files as you just did? One option would be to build a new container image combining the stack components with the application under development, but with `appsody run` the approach is more simple and efficient. Scroll up in the terminal where you executed `appsody run` and you will see a line like:

```text
Running docker command: docker run --rm -p 3000:3000 -p 9229:9229 --name mynode-dev -v /Users/timro/msdemo/mynode/:/project/user-app -v mynode-deps:/project/user-app/node_modules -v /Users/timro/.appsody/appsody-controller:/appsody/appsody-controller -t --entrypoint /appsody/appsody-controller appsody/nodejs-express:0.2 --mode=run
```

This commands starts up a container image `appsody/nodejs-express:0.2` and includes a volume mount from the local path where the application code resides into that container. (The `-v /Users/timro/msdemo/mynode/:/project/user-app` argument to the docker command). This means that any changes in the local workstation filesystem are immediately available in the running container. Code within the container provided by the *stack architect* is responsible for taking the appropriate action to rebuild/relaunch the application when there is a change by the developer.


### Appsody stack status endpoints

At this point, it's important to note that in addition to support for developers to extend the appsody application with code of their own, appsody stacks are built with a number of pre-defined endpoints that provide health, liveness, readiness, and metrics out-of-the-box. These endpoints are available during development and also when deployed to a cluster and implement standard semantics so that you don't need to write your own checks if the defaults meet your needs. The Node.js Express stack provides the following endpoints:

- Health endpoint: http://localhost:3000/health
- Liveness endpoint: http://localhost:3000/live
- Readiness endpoint: http://localhost:3000/ready
- Metrics endpoint: http://localhost:3000/metrics
- Dashboard endpoint: http://localhost:3000/appmetrics-dash (development only)

Take a momment now to either `curl` or open these endpoints within a browser. You'll want to use a browser for the Dashboard. Notice that the code that's being used to provide these endpoints is not a concern for the developer, nor is it visible within the code template we are using. It's just bundled in by the Node.js Express stack.

## Summary

You have now created a basic application starting from a simple template based on the Node.js Express appsody stack. Up to this point, the code has been executing in local mode, through a volume mount into a container with the appsody stack. For deployment to a Kubernetes cluster as an application or serverless instance, the appsody cli provides commands to use to build the application into a production container image with best practices and then deploy to Kubernetes. You'll do this next.

Continue on to [deploying Appsody apps to Kubernetes](appsody-deploy.md)
