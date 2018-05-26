# ci-cd-tutorial

## Set up build and release job

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
