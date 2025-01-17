# jtrack

## Overview

This program will: 

1. Get images from GIPHY API using passed in query
2. Try and detect faces on them, if found, draw a box
3. Post results to Slack

I think it's neat because `mvn package` will actually build the JAR into a
Java 12 Docker image (see Dockerfile) that can then be used anywhere. Portability yay!

You should think this is neat because some dependencies are super painful.
Like building OpenCV for your platform. 

You can also get images from Oracle object storage as well.

Example of face detection: 

<img src="https://raw.githubusercontent.com/carimura/jtrack/master/result.gif" /> <br />

<img src="https://raw.githubusercontent.com/carimura/jtrack/master/result.jpg" width=400/> <br />

## To Use

### 1. Configure 
* Create a .env file with GIPHY_TOKEN and SLACK_TOKEN
* Specify your Docker image in pom.xml under properties (ie `<docker.image>carimura/jtrack</docker.image>`)

### 2. Include Dependencies

Only once! (Then each time you change Maven dependencies): 
```
./libs.sh
```

### 3. Compile and Build (Includes Docker build)

```
mvn clean package
```

### 4. Profit
```
docker run -e "QUERY=happy" -e "NUM=3" --env-file=.env <your_docker_name>/jtrack:<version>
```


#### Inputs to container: 

* *QUERY* [string] (ie "excited") -- Query string
* *NUM* [int] (ie "5") -- Number of images to pull
* *PREVIEW* [bool] (ie "false") -- If set to true, a 
smaller image will be pulled from Giphy.


## To Build

### Runtime

You can change the runtime by using jlink as follows:

```
jlink --no-header-files --no-man-pages --compress=2 --strip-debug \
--module-path /Users/chad/lib/jdk-14-loom-linux.jdk/jmods \
--add-modules java.base,java.compiler,java.desktop,java.instrument,java.management,java.naming,java.sql,jdk.attach,jdk.jdi,jdk.unsupported \
--output java-runtime
```

## TODO

1. illegal reflective access message for bouncy castle required by OCI stuff
