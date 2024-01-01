---
author: "UmmIt"
title: "Hosting Forgejo Git System, Runner, and Action Testing"
tags: ["Forgejo", "Git"]
date: 2024-01-01T07:53:49+08:00
---

## Introduction

As we step into the world of continuous integration (CI) and embrace the power of Forgejo, let's set up our Forgejo server, configure a runner, and test actions—all within the comfort of our local area network (LAN).

## Prerequisites

Before diving into the setup, ensure you have Go, Docker, and essential build tools installed. Use the following command to install them on Arch Linux:

```shell
sudo pacman -S go docker base-devel
```

Start and enable the Docker service to ensure it runs on system startup:

```shell
sudo systemctl enable docker.service
sudo systemctl start docker.service
```

## Setting Up Forgejo Server

Pull the Forgejo image and create an admin user:

```shell
docker run --name forgejo -e FORGEJO__security__INSTALL_LOCK=true -e FORGEJO__actions__ENABLED=true -d codeberg.org/forgejo-experimental/forgejo:1.19

docker exec --user 1000 forgejo forgejo admin user create --admin --username root --password admin1234 --email root@example.com
```

Retrieve the IP address of your Forgejo instance:

```shell
docker exec --user 1000 forgejo ip a
```

Note the IP address, typically displayed as `http://172.17.0.2:3000/`.

Login to Forgejo with the created admin user (`root` and password `admin1234`), create a new repository, and enable actions in the repository settings.

## Setting Up Forgejo Runner

1. Go to your repository -> Settings -> Runners -> Create new Runner. Note the runner token.

2. Clone the Forgejo runner repository and register the runner:

```shell
git clone https://codeberg.org/forgejo/runner
cd runner
git checkout v1.1.0
make build
./forgejo-runner register --name myrunner --no-interactive --instance http://172.17.0.2:3000 --token <runner_token_here>
./forgejo-runner daemon
```

Keep the runner session running. you need to use another terminal.

## Testing Actions

1. Clone the target repository and create a demo workflow:

```shell
git clone http://172.17.0.2:3000/root/test
cd test
nvim .forgejo/workflows/demo.yaml
```

Insert the following content into `demo.yaml`:

```yaml
name: Demo
run-name: ${{ github.actor }} is testing
on: [push]
jobs:
  Explore-CI:
    runs-on: ubuntu-latest
    steps:
      - run: echo "The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "This job is now running on a ${{ runner.os }} server."
      - run: echo "The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v3
      - run: echo "The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "This job's status is ${{ job.status }}."
```

2. Push the changes:

```shell
git add .
git commit -m "[CI] Testing Forgejo action"
git push origin -u main
```

3. Visit your repository on Forgejo, navigate to Actions, and watch your action come to life. Within seconds, your action should complete.

![done](./featured.png)

## Conclusion

In fact, you've actually self-hosted the entire git system. Well, you can also make your instance public to the internet. I believe all you need to do is open the UDP port of your ip.

Now, with Forgejo, Docker, and your own custom runner, you're set up of CI within your LAN :D

### What Next?

Actually my next tutorial is about Codeberg Action. The Runner is already done, so all that's missing is codeberg action.

## Reference

- [Forgejo gets an integrated CI named Actions](https://forgejo.org/2023-02-27-forgejo-actions/)
