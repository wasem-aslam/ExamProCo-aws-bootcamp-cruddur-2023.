# Week 1 — App Containerization

## Technical Task
```
✅ Create a new GitHub repo
✅ Launch the repo within a Gitpod workspace
✅ Configure Gitpod.yml configuration, eg. VSCode Extensions
✅ Clone the frontend and backend repo
✅ Explore the codebases
✅ Ensure we can get the apps running locally
✅ Write a Dockerfile for each app
✅ Ensure we get the apps running via individual container
✅ Create a docker-compose file
✅ Ensure we can orchestrate multiple containers to run side by side
✅ Mount directories so we can make changes while we code
```
## Homework Challenges    

```
✅ Run the dockerfile CMD as an external script
✅ Push and tag a image to DockerHub (they have a free tier)
✅ Use multi-stage building for a Dockerfile build
✅ Implement a healthcheck in the V3 Docker compose file
✅ Research best practices of Dockerfiles and attempt to implement it in your Dockerfile
✅ Learn how to install Docker on your localmachine and get the same containers running outside of Gitpod / Codespaces
✅ Launch an EC2 instance that has docker installed, and pull a container to demonstrate you can run your own docker processes. 
```

Week1 was tough as i have not worked before with docker  but i spend quite much time by watching all the homework videos and also watched live cloud job role video which will definitely help me to decide where i want to work in cloud. By completing technical task and homework challange i got good understandong of how docker, images and container works.As i also pushed my image to dockerhub so now i understand the purpose of docker registry repository. 


## Technical Task

I have completed all the task mentioned above in 'Technical Task' and run individual containers using Andrew 
[Guideline](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1/journal/week1.md)
All my code is committed in my [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023) as a proof.



After completing individual running container, app was working fine but i forgot to take screenshot. I also asked this question to Andrew in office hours and he said its ok as i have been went through this process by running individual container apps which can be also checked by my code Repo.

After this step, wrote docker compose yml file where i also added enviromnets,network and ports.

Here is how compose file looks like and code is already in [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)

![](assets/Docker%20compose%20yaml.png)


I also added npm install in gitpod.yml file so i dont need to do everytime.

Gitpod.yml screenshot in [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023/blob/main/.gitpod.yml)
![](assets/Gitpod.png)


Docker images screenshot.
![](assets/Docker%20Containers%20Images%20.png)


Make sure to opened the ports.

![](assets/Opened%20the%20ports.png)

Docker copmose container running now as showing in screenshot.

![](assets/Docker%20Containers%20Images%20.png)

For DynamoDB local followed following [Guideline](https://github.com/100DaysOfCloud/challenge-dynamodb-local) and DynamoDB added in compose file [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)

As table is created with items and showing table in screenshot.
![](assets/DynamoDB%20LOcal.png)

PostgreSQL connected successfully by running:  ```psql -Upostgres --host localhost``` added code in comppose.yml [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml)

![](assets/PostgreSQL%20connected.png)


Final cruddur GUI is showing running in composer container.
![](assets/Final%20cruddur%20after%20login.png)

As I also aded Notification functionality so working fine [REPO](https://github.com/wasem-aslam/aws-bootcamp-cruddur-2023/blob/main/backend-flask/services/notifications_activities.py)

![](assets/Notifications.png) 


## Homework Challenges 

✅ Run the dockerfile CMD as an external script

Cretaed app.py and Dockerfile in Visualstudio locally at my macbook.

Dockerfile

```
FROM python:3.8-alpine
COPY . /app
WORKDIR /app
CMD ["python","app.py"]
```

app.py
```
print("Hello World")
```

Run the command to build image
```
docker build -t helloworld .
```

At my local terminal at mac showing the program written helloword and dockerfile.
![](assets/Script%20program.png)


Docker file for script.

![](assets/Dockerfile%20for%20script.png)

Docker command build script.

![](assets/Docker%20command%20build%20script.png)

After build step run the docker.

![](assets/Docker%20run%20command%20showing%20reesult.png)

Showing image also in docker local app.

![](assets/Docker%20image%20created%20and%20showing%20in%20docker%20app%20in%20MAC%20.png)


✅ Push and tag a image to DockerHub (they have a free tier)

Image taged and pushed to dockerhub registry repository.

![](assets/Docker%20push%20command%202.png)

As a result of push my image is uploaded to dockerhub repository as showing in under my user 'wasemaslam' and repository 'aws-bootcamp-cruddur-2023'

![](assets/Image%20uploaded%20at%20docker%20up.png)


✅ Implement a healthcheck in the V3 Docker compose file

I put the code for healthcheck in docker-compose.yml file.

```
 healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 2
      start_period: 20s
```
This is how looks like in gitpod and when run the 'docker compose up' works fine and showing healthy containers. 

![](assets/showing%20healthy.png)

After that i chnaged the port to wrong port like 5000 instead 3000 and run the copose up and can see all other containers are up but frontend is unhealthy which can be seen in docker tab and also in ps command in terminal.

![](assets/Shiwing%20unhealthy.png)




✅ Use multi-stage building for a Dockerfile build

I followed the following   
[Guideline](http://100daysofdevops.com/use-multi-stage-builds-with-dockerfile/) for multistage.

First wrote dokerfile as single stage as showing in screenshot and also wrote simple helloworld program in go lang langauge.

Helloworld.go
```
package main
import "fmt"
func main() {
fmt.Println("hello world")
}
```

Dockerfile code for single-stage.
```
FROM golang:1.13.1
WORKDIR /tmp
COPY helloworld.go .
RUN GOOS=linux go build -a -installsuffix cgo -o helloworld .
CMD ["./helloworld"]%
```

Dockerfile code for multi-stage.

```
FROM golang:1.13.1 as multistage
WORKDIR /tmp
COPY helloworld.go .
RUN GOOS=linux go build -a -installsuffix cgo -o helloworld .
CMD ["./helloworld"]

FROM alpine:3.10.2
WORKDIR /root
COPY --from=multistage /tmp/helloworld .
CMD ["./helloworld"]%
```

After saving code build comands used.

```
docker build -t single.stage .
```
```
docker build -t multi.stage .
```

Here single stage build done.
![](assets/Single%20stage%20build%20example.png)

As showing imgae is created with size ``` 719 MB ```.

![](assets/Single%20stage%20example.png)


Now Docker file is updated with multi stage as showing in screenshot.

Here we run the docker.

![](assets/Docker%20multi%20stage%20run.png)

Multi stage docker image created with only ```7.42 MB ``` as previously with single it was ``` 719 MB ``` so this is the beauty of multistage.

![](assets/multi%20stage%20docker%20file.png)

After creating multi stage image run the image to container and its hwoing in my local docker app.

![](assets/Docker%20container%20running%20from%20local%20.png)


✅ Research best practices of Dockerfiles and attempt to implement it in your Dockerfile

I read [following article](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) and tried to use some of the attributes but it seems more technical so i simple used as in multi stage.

Dockerfile multistage.
```
FROM golang:1.13.1 as multistage
WORKDIR /tmp
COPY helloworld.go .
RUN GOOS=linux go build -a -installsuffix cgo -o helloworld .
CMD ["./helloworld"]

FROM alpine:3.10.2
WORKDIR /root
COPY --from=multistage /tmp/helloworld .
CMD ["./helloworld"]%
```

![](assets/multi%20stage%20docker%20file.png)


✅ Learn how to install Docker on your localmachine and get the same containers running outside of Gitpod / Codespaces

I downloaded docker app from dockerhub website and installed at my macbook and its working fine.

As you can see in screenshot I pulled all the code from my github locally to my visual studio and run the composer composer.


Docker compose up run the command.

![](assets/Dokcer%20compose%20up.png)


![](assets/Multi%20container%20running.png)

Docker local app showing container running.

![](assets/Docker%20local%20app%20showing%20container.png)

One container frontend was not running and complaining about environment flast. I did troubleshooting and tried to update environment of flask like et FLASK_ENV="development" or set FLASK_ENV=development. but not helped.

[For example1](https://stackoverflow.com/questions/52162882/set-flask-environment-to-development-mode-as-default)
[For example2](https://stackoverflow.com/questions/54794134/flask-env-seems-to-be-ignored-cant-enter-debug-environment)

I have less time so i will troubleshoot this later after submitting my homework.


✅ Launch an EC2 instance that has docker installed, and pull a container to demonstrate you can run your own docker processes. 


