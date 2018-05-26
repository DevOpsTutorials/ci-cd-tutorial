# ci-cd-tutorial

## Set up build and release job on Jenkins

Click on `New Item`

Enter `docker-build` as the job name and select `Freestyle project`.

In `Copy from` enter `simple-build` and click `OK` button.

In `Source Code Management` -> `Git` -> `Repositories` -> `Repository URL`, change repo to `https://github.com/kurianinc/docker-curriculum.git`.

In `Build` -> `Execute shell` -> `Command` enter the following commands, make changes specific to your setup as needed:
```
cd flask-app
sudo docker build -t thomastk/catnip .
sudo docker login --username=thomastk --password=PASS
sudo docker push thomastk/catnip
```

Click `Save`.

This job will ensure that everytime a change is checked in the Docker image is rebuilt and pushed that to Docker Hub.

## Set up app deployment job Jenkins

Click on `New Item`

Enter `deploy-app` as the job name and select `Freestyle project`.

Under `General` -> `This project is parameterized`:
- `Add Parameter` -> pull down `String Parameter`, enter `Name` as `appserver` and `Default Value` as `54.186.127.91`.
- `Add Parameter` -> pull down `String Parameter`, enter `Name` as `portnum`.

Under `Build` -> `Execute shell` -> `Command`, enter following commandline with correct image name:
```
sudo -u devops ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no ${appserver} docker run -d -P -p ${portnum}:5000 thomastk/catnip
```

Click `Save`.
