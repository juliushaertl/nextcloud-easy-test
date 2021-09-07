# Nextcloud easy test instance
This is a one-command Nextcloud instance that makes it possible to test different branches and apps with only one command.

## How to use this?

### Preparation (only needed if not yet done):
Install Docker on your OS:
- On Linux via:
    ```shell
    curl -fsSL get.docker.com | sudo sh
    ```
- On macOS and Windows via Docker Desktop:
https://www.docker.com/products/docker-desktop 

### Execution
Run the container:  
(You can change the branch by changing `master` in `SERVER_BRANCH=master` to the branch that you want to test during the initial container creation.)

On Linux and macOS:
```
docker run \
-e SERVER_BRANCH=master \
--name nextcloud-easy-test \
-p 127.0.0.1:8443:443 \
--user=www-data \
ghcr.io/szaimen/nextcloud-easy-test:latest
```

<details>
<summary>On Windows</summary>

```
docker run ^
-e SERVER_BRANCH=master ^
--name nextcloud-easy-test ^
-p 127.0.0.1:8443:443 ^
--user=www-data ^
ghcr.io/szaimen/nextcloud-easy-test:latest
```

</details>

<details>
<summary>Explanation of the command</summary>

`docker run \`  
This command creates a new docker container.

`-e SERVER_BRANCH=master \`  
This inserts the environment variable `SERVER_BRANCH` into the container and sets it to the value `master`. 

`--name nextcloud-easy-test \`  
This gives the container a distinct name `nextcloud-easy-test` so that you are able to easily run other docker commands on the container.

`-p 127.0.0.1:8443:443 \`  
This makes the container listen on `localhost` and maps the host port `8443` to the container port `443` so that you are able to access the container by opening https://localhost:8443.

`--user=www-data \`  
This makes sure that every task inside the container runs with the `www-data` user which allows you to run the container securely on your PC. (The default is `root` which is insecure.)

`ghcr.io/szaimen/nextcloud-easy-test:latest`  
This is the image name that you will use as base for the container. `latest` is the tag that will be used.

---

</details>

After the initial startup you will be able to access the instance via https://localhost:8443 and using `admin` as username and `nextcloud` as password.

### Follow up

**After you are done testing**, you can simply stop the container by pressing `[CTRL] + [c]` and delete the container by running:
```
docker stop nextcloud-easy-test
docker rm nextcloud-easy-test
```

### Running in a VM
If you want to run this in a VM, you need to change the port in the initial command from to `-p 127.0.0.1:8443:443` to `-p 8443:443` and add the following flag: `-e TRUSTED_DOMAIN=ip.of.the.VM` in order to automatically make it work.

### Additionally Available Environmental Variables
Additionally, the container currently reacts on the following variables:
```
ACTIVITY_BRANCH
APPROVAL_BRANCH
BOOKMARKS_BRANCH
CALENDAR_BRANCH
CIRCLES_BRANCH
CONTACTS_BRANCH
DECK_BRANCH
E2EE_BRANCH
FIRSTRUNWIZARD_BRANCH
FORMS_BRANCH
GROUPFOLDERS_BRANCH
GUESTS_BRANCH
IMPERSONATE_BRANCH
LOGREADER_BRANCH
MAIL_BRANCH
MAPS_BRANCH
NEWS_BRANCH
NOTES_BRANCH
NOTIFICATIONS_BRANCH
PDFVIEWER_BRANCH
PHOTOS_BRANCH
POLLS_BRANCH
SERVERINFO_BRANCH
TALK_BRANCH
TASKS_BRANCH
TEXT_BRANCH
VIEWER_BRANCH
```

<details>
<summary>For easy copy and paste</summary>

```
-e ACTIVITY_BRANCH=master \
-e APPROVAL_BRANCH=master \
-e BOOKMARKS_BRANCH=master \
-e CALENDAR_BRANCH=master \
-e CIRCLES_BRANCH=master \
-e CONTACTS_BRANCH=master \
-e DECK_BRANCH=master \
-e E2EE_BRANCH=master \
-e FIRSTRUNWIZARD_BRANCH=master \
-e FORMS_BRANCH=master \
-e GROUPFOLDERS_BRANCH=master \
-e GUESTS_BRANCH=master \
-e IMPERSONATE_BRANCH=master \
-e LOGREADER_BRANCH=master \
-e MAIL_BRANCH=master \
-e MAPS_BRANCH=master \
-e NEWS_BRANCH=master \
-e NOTES_BRANCH=master \
-e NOTIFICATIONS_BRANCH=master \
-e PDFVIEWER_BRANCH=master \
-e PHOTOS_BRANCH=master \
-e POLLS_BRANCH=master \
-e SERVERINFO_BRANCH=master \
-e TALK_BRANCH=master \
-e TASKS_BRANCH=master \
-e TEXT_BRANCH=master \
-e VIEWER_BRANCH=master \
```

</details>

If one of the above variables are set via e.g. `-e CALENDAR_BRANCH=master` during the initial container creation, then will the container automatically get the chosen branch from github and compile and enable the chosen apps on the instance during the startup.

## How does it work?
The docker image comes pre-bundled with all needed dependencies for a minimal instance of Nextcloud, has the master branch already included and allows to define a target branch via environmental variables that will automatically be switched to and installed during the container startup. For a refresh, you need to recreate the container by first removing it and then running the same command again.
