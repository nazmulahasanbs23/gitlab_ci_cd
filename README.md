## Prerequisites
I have used vagrant to calibrate this test installation for gitlab & gitlab runner
Please find the Vagrantfile in the resource section of the repo.

I have used two servers, one for gilab & other for gitlab runner

* gitlab-server ip: 10.2.3.4
* gitlab-runner ip: 10.2.3.5

### Machine configurations are as follows:

* 4 GB of RAM
* 4 GB of SWAP space
* 4 Cores of CPU
* 40 GB of HDD spaces

## Install and configure the necessary packages/dependencies on the server (10.2.3.4)

```
sudo yum install -y curl policycoreutils-python openssh-server perl
Enable OpenSSH server daemon if not enabled: sudo systemctl status sshd
sudo systemctl enable sshd
sudo systemctl start sshd

```

## Adding repository & package installation 
```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
```
## Browsing and login using creds
Open the browser & browse with the ip http://10.2.3.4
A randomly generated password will be available in /etc/gitlab/initial_root_password . Use this password with username root to login. It will be stored for 24 hours at that location.

## GitLab runner
GitLab Runner is an application that works with GitLab CI/CD to run jobs in a pipeline.
You can choose to install the GitLab Runner application on infrastructure that you own or manage. If you do, you should install GitLab Runner on a machine that’s separate from the one that hosts the GitLab instance for security and performance reasons. When you use separate machines, you can have different operating systems and tools, like Kubernetes or Docker, on each.

## Gitlab runner executor

An executor determines the environment each job runs in.

If you want your CI/CD job to run commands in a custom Docker container, you might install GitLab Runner on a Linux server and register a runner that uses the Docker executor.
These are only a few of the possible configurations. You can install GitLab Runner on a virtual machine and have it use another virtual machine as an executor.

When you install GitLab Runner in a Docker container and choose the Docker executor to run your jobs, it’s sometimes referred to as a “Docker-in-Docker” configuration.


## Runner Execution flow

![alt text](https://github.com/nazmulahasanbs23/gitlab_ci_cd/blob/main/gilab_with_runner.png)


## Install Gitlab runner with docker on 10.2.3.5

Refresh the local repository & install/configure docker on the runner machine

### Create the Docker volume:
```
docker volume create gitlab-runner-config
```
### Start the GitLab Runner container using the volume we just created:

```
docker run -d --name gitlab-runner --restart always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v gitlab-runner-config:/etc/gitlab-runner \
    gitlab/gitlab-runner:latest
```

We can create as many gitlab-runners as we want.

### Register gitlab runner 
```
docker run --rm -it -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner:latest register
```
Go to the gitlab web interface (10.2.3.4 in my case) create a project & go to the settings>CI/CD>Expand the runners section. You will find the registered runner there.

Run a Test CI/CD

Add a file (.gitlab-sample.yml) to the repository & click on the CI/CD pipeline on the left side (or you can turn on auto devops to initiate pipeline automatically). On Successful run, you will get the 3 stages passed successfully.
Please find the .gitlab-sample.yml file in the resource section of the repo.

