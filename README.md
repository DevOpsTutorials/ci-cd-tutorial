# ci-cd-tutorial

## Set up build and release job on Jenkins

Click on `New Item`

Enter `docker-build` as the job name and select `Freestyle project`, and in `Copy from` at the bottom, enter `simple-build` and click `OK` button.

In `Source Code Management` (second tab on left) -> `Git` -> `Repositories` -> `Repository URL`, change repo to `https://github.com/kurianinc/docker-curriculum.git`. In `Branches to build`, also in `Source Code Management`, change branches to build to `*/master`.

In `Build` (second rightmost tab) -> `Execute shell` -> `Command` enter the following commands, make changes specific to your setup as needed:

Edit the following with your Docker Username and Password
```
cd flask-app
sudo docker build -t YOUR-DOCKER-USERNAME/catnip .
sudo docker login --username=YOUR-DOCKER-USERNAME --password=YOUR-DOCKER-PASSWORD
sudo docker push YOUR-DOCKER-USERNAME/catnip
```

Click `Save`.

This job will ensure that everytime a change is checked in the Docker image is rebuilt and pushed that to Docker Hub.

## Set up app deployment job Jenkins

Click on `New Item`

Enter `deploy-app` as the job name and select `Freestyle project`. Click `OK` button.

Under `General` -> `This project is parameterized`:
- `Add Parameter` -> pull down `String Parameter`, enter `Name` as `appserver` and `Default Value` as `54.186.127.91`.
- `Add Parameter` -> pull down `String Parameter`, enter `Name` as `portnum`.

Under `Build` (second rightmost tab) -> `Execute shell` -> `Command`, enter following commandline with correct image name:
```
sudo -u devops ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no ${appserver} docker run -d -P -p ${portnum}:5000 YOUR-DOCKER-USERNAME/catnip
```

Click `Save`.

And now to test it, from the Dashboard, click `deploy-app`, on the left sidebar, go to `Build with Parameters`, then specify a portnum (ex. a portnum between `9000-9050`), and click the `Build` button and a build should be started in your Build History. You can also check testing it in the browser at `http://54.186.127.91:PORTNUM/`
