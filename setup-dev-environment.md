This page will guide you through the process of setting up the plugin development environment on your local machine. It should work on Linux, macOS, or Windows 10 with the Linux Subsystem.

## Pre-requisites
Before starting, make sure you have the following software installed and working on your machine:

1. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) to clone the plugin repository (or your fork of the plugin repository).
2. [NPM](https://www.npmjs.com/get-npm) to install Node packages used to build assets and other tasks. (*v12.18.3 at the time of last update.*)
    - *NOTE*: A Node version manager, such as NVM, can be used to easily switch between versions as needed by different projects.
3. [Docker](https://docs.docker.com/get-docker/) & Docker Compose(separate [download on Linux](https://docs.docker.com/compose/install/))

## Clone the repository
In your terminal, replacing USER_NAME with your GitHub username::
```
$ git clone git@github.com:USER_NAME/openid-connect-generic.git
```

## Setup using Visual Studio Code
In your VS Code, install [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension.

After installing the extension, open the `openid-connect-generic` repo in VS Code:

- VS Code will detect the `.devcontainer/devcontainer.json` file and prompt you to "Reopen in Container." Click open and wait for it to setup.
- If the prompt doesn't appear, you can manually trigger it from the VS Code Command Palette (**Ctrl+Shift+P** or **Cmd+Shift+P** on macOS). Search for "Dev Containers: Reopen in Container" and select this option.

Once VS Code finishes setting up and opens the project in the development container, it will automatically run the `preinstall` in `package.json` and the `postCreateCommand` scripts in `devcontainer.json`

You have successfully set up your development environment!

## Run and test the project

1. Open your WordPress admin page in a web browser: http://localhost:8080/wp/wp-login.php

The admin account credentials can be found in the `.devcontainer/setup.sh` file:
```bash
# Setup the WordPress environment.
cd "/app"
if ! wp core is-installed 2>/dev/null; then
	echo "Setting up WordPress at $SITE_HOST"
	wp core install --url="$SITE_HOST" --title="OpenID Connect Development" --admin_user="admin" --admin_email="admin@example.com" --admin_password="password" --skip-email --quiet
fi
```

2. Access phpMyAdmin: http://localhost:8081/

The admin account credentials can be found in the `docker-compose.yml` file:
```yml
    environment: &env
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_TEST_DB_NAME: wordpress_test
```

3. Run unit tests:

The unit test scripts are defined in the package.json file. You can run them using NPM:
```bash
npm run test
```