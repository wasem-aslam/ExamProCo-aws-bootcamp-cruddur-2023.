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

## Technical Task

I have completed all the tasks mentioned above in 'Technical Task' and run individual containers using Andrew 
[Guideline](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1/journal/week1.md)
All my code is committed in my Repo as a proof.


After completing this step, wrote docker compose yml file where i also added enviromnets,network and ports.

![](assets/Docker%20compose%20yaml.png)


I also added npm install in gitpod.yml file so i dont need to do everytime.

Gitpod.yml screenshot.
![](assets/Gitpod.png)

Opened the ports.

![](assets/Opened%20the%20ports.png)

Docker Container 

![](assets/Docker%20Containers%20Images%20.png)



postgreSQL connected

![](assets/PostgreSQL%20connected.png)


Final 
![](assets/Final%20cruddur%20after%20login.png)



✅ Run the dockerfile CMD as an external script

Cretaed app.py and Dockerfile in Visualstudio locally.
Dockerfile
,,,
FROM python:3.8-alpine
COPY . /app
WORKDIR /app
CMD ["python","app.py"]
,,,

app.py
```
print("Hello World")
```

Run the command to build image
```
docker build -t helloworld .
```


✅ Use multi-stage building for a Dockerfile build

I followed the following   
[Guideline](http://100daysofdevops.com/use-multi-stage-builds-with-dockerfile/) for multistage.

First wrote dokerfile as single stage as showing in screenshot and also wrote simple helloworld program in go lang langauge.

![]()

```
docker build -t single.stage .
```


![]()

```
docker build -t multi.stage .
```



