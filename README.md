# VPS-SITE-BDE-FRONT

<p align="center">
  <!-- PATTERN="![GitHub latest release](https://img.shields.io/github/v/release/${GITHUB_REPOSITORY})" -->![GitHub latest release](https://img.shields.io/github/v/release/${GITHUB_REPOSITORY})<!-- PATTERN_END -->
  <!-- PATTERN="![License](https://img.shields.io/github/license/${GITHUB_REPOSITORY})" -->![License](https://img.shields.io/github/license/${GITHUB_REPOSITORY})<!-- PATTERN_END -->
</p>

Front-end repository of BDE website.

---

## Table of Contents

- [Usage](#usage)
  - [Node.JS](#nodejs)
  - [Docker](#docker)
- [Structure](#structure)
- [Commands](#commands)
- [Git conventions](#git-conventions)
  - [Branches](#branches)
  - [Commits](#commits)
  - [PR](#pr)
- [Git Commands](#git-commands)
- [GitHub](#github)
  - [Actions](#actions)
  - [Secrets](#secrets)
  - [Variables](#variables)
  - [Releases](#releases)
- [License](#license)

---

## Usage

Next steps define how to download the project and run it under a port/path, you will have to configure a webserver on your machine to access it from internet (using nginx, apache2, etc.).

- ### Node.JS
  - need to clone the repository 
  - need to have Node.JS installed with npm ([download link](https://nodejs.org/en/download) and choose `using nvm with npm` option)
  1. open a terminal
  2. clone the repository with <!-- PATTERN="`git clone https://github.com/${GITHUB_REPOSITORY}.git <path>`" -->`git clone https://github.com/${GITHUB_REPOSITORY}.git <path>`<!-- PATTERN_END -->
  3. navigate to the cloned folder with `cd <path>`
  4. run `npm ci`
  5. Depends on your needs:
      - for development: run `npm run dev:run -- --port=<number>` and replace `<port>` with the port you want
      - for production: run `npm run build:run -- --output-path=<path>` and replace `<path>` with the path of the folder you want (it needs to be created before)
  6. configure your webserver to serve the port/path you chose
      - for development: `http://localhost:<port>` (replace `<port>` with the port you chose)
      - for production: the path you chose in step 5

- ### Docker
  - NO need to clone the repository
  - image: [![Docker Image Version]<!-- PATTERN="(https://img.shields.io/docker/v/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE}?sort=semver)" -->(https://img.shields.io/docker/v/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE}?sort=semver)<!-- PATTERN_END --> ![Docker Image Size]<!-- PATTERN="(https://img.shields.io/docker/image-size/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE}?sort=semver)" -->(https://img.shields.io/docker/image-size/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE}?sort=semver)<!-- PATTERN_END -->]<!-- PATTERN="(https://hub.docker.com/r/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE})" -->(https://hub.docker.com/r/${DOCKERHUB_USERNAME}/${DOCKERHUB_IMAGE})<!-- PATTERN_END -->
  - be sure you installed [Docker](https://docs.docker.com/engine/install/) and [Docker Compose](https://docs.docker.com/compose/install/)
  1. open a terminal and navigate to the folder where you want to save your configuration with `cd <path>`
  2. create a folder where the container will save its data with `mkdir <data-folder>`
  3. create a `docker-compose.yml` file
      - you can find an example of `docker-compose.yml` [here](./.docker/front/docker-compose.yml)
      - you can copy it with <!-- PATTERN="`curl -o docker-compose.yml https://raw.githubusercontent.com/${GITHUB_REPOSITORY}/main/${DOCKER_CONFIG_FOLDER}/docker-compose.yml`" -->`curl -o docker-compose.yml https://raw.githubusercontent.com/${GITHUB_REPOSITORY}/main/${DOCKER_CONFIG_FOLDER}/docker-compose.yml`<!-- PATTERN_END -->
  4. create a `.env` file
      - you can find an example of `.env` [here](./.docker/front/example.env)
      - you can get it with <!-- PATTERN="`curl -o .env https://raw.githubusercontent.com/${GITHUB_REPOSITORY}/main/${DOCKER_CONFIG_FOLDER}/example.env`" -->`curl -o .env https://raw.githubusercontent.com/${GITHUB_REPOSITORY}/main/${DOCKER_CONFIG_FOLDER}/example.env`<!-- PATTERN_END -->
  5. edit them to your needs
  6. run `docker compose up -d --force-recreate --pull always` to start the container
  7. configure your webserver to serve the port you chose in the `.env` file

---

## Structure

- `.docker`: Docker-related files (Dockerfile, etc.)
- `.github`: GitHub-related files (workflows, issue templates, etc.)
- `.ssh`: local folder for your ssh keys to auth and sign commits (ignored by git)
- `assets`: static files but local (ignored by git)
- `build`: built files for production (after running `npm run build`) (ignored by git)
- `public`: static files (HTML template, images, etc.)
- `scripts`: scripts used by `npm run <script>` command
- `src`: source code (React components, styles, etc.)
- `.dockerignore`: files to ignore when building the Docker image
- `.gitignore`: files to ignore by git
- `eslint.config.mjs`: ESLint configuration
- `LICENSE`: license file (MIT)
- `package-lock.json`: npm package lock file (exact versions of dependencies)
- `package.json`: npm package configuration (dependencies, scripts, etc.)
- `postcss.config.mjs`: PostCSS configuration
- `README.md`: this file
- `setup.sh`: setup script (run after cloning the repository)
- `tsconfig.json`: TypeScript configuration
- `webpack.config.mjs`: Webpack configuration

---

## Commands

- `npm install`: install dependencies
- `npm install -E -D <package>...`: install development dependencies (with exact version)
- `npm install -E -P <package>...`: install production dependencies (with exact version)
- `npm run lint`: lint the code with ESLint (automatically run before `dev` and `build`)
  - options (`npm run lint -- <option>...`):
    - `--lint-skip`: skip linting
    - `--lint-fix`: automatically fix problems
    - `--lint-nibble`: format output to more readable format
- `npm run dev`: run the application in development mode (with hot-reloading)
  - options (`npm run dev -- <option>...`):
    - every those of `lint` command
    - `--dev-port=<number>`: specify the port (default: 3000)
- `npm run build`: build the application for production
  - options (`npm run build -- <option>...`):
    - every those of `lint` command
    - `--build-output-path=<path>`: specify the output path (default: `./build`)

---

## Git conventions

- ### Branches:
  - `main`: production branch
  - `dev`: development branch (all features must be merged here first)
  - `<service>`: service branches (must be created from `dev` and merged back to `dev`)
  - `<service>-<id>`: task branches (must be created from `<service>` and merged back to `<service>`)

- ### Commits:
  - `<type>(<scope>): <comment>`
    - `<type>`: type of the commit (must be one of the following, can be combined with `|`):
      - `feat`: new feature
      - `fix`: bug fix
      - `docs`: documentation only changes
      - `style`: changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
      - `refactor`: code change that neither fixes a bug nor adds a feature
      - `perf`: code change that improves performance
      - `test`: adding missing tests or correcting existing tests
      - `chore`: changes to the build process or auxiliary tools and libraries such as documentation generation
      - `merge`: merges branches
    - `<scope>`: name of the branch (`service-id`)
    - `<comment>`: short description of the commit
  - example: `feat(user-42): add login feature`
  - example: `feat|perf|style(home-21): add last event feature, improve performance for render and fix styles of first section`
  - every commit must be signed with a GPG/SSH key (set it up with `./setup.sh`)
  - make reasonable sized commits (do not commit everything in one commit, but do not make too many small commits either)
  - write meaningful commit messages (do not use `fix` or `update` as comment, be more specific)

- ### PR:
  - title: `merge(<source> -> <target>): <comment>`
    - `<source>`: source branch
    - `<target>`: target branch
    - `<comment>`: short description of the PR
  - description
    - optional: follow the template provided by GitHub
    - if you did not use the template, at least provide a description of the changes
  - for the merge commit, let GitHub generate it automatically (do not edit it)
  - reviewers: at least one reviewer must be assigned (preferably the repository admin or the service manager)
  - In case of a release PR (from `dev` to `main`):
    - title: `r/<version> - <name>`
      - `<version>`: version of the release (must follow [semantic versioning](https://semver.org/), e.g. `1.2.3`)
      - `<name>`: name of the release
    - description:
      - follow the template provided by GitHub
      - if you did not use the template, at least provide a description of the changes
      - you can omit the "Changelog" section
      - do not use "---" in the PR description, it breaks the formatting, only let the one by default
      - a link to compare the changes since last release will be automatically generated by GitHub in the release description

---

## Git Commands:

- `git status`: check the status of the repository
- `git fetch origin`: fetch changes from remote
- `git checkout <branch>`: switch to an existing branch
- `git pull origin`: pull changes from remote branch
- `git rebase origin/<branch>`: rebase current branch onto remote branch, if you want to get latest changes of another branch (must be done after `git fetch origin`)
- `git add .`: stage all changes
- `git add <file>...`: stage specific file (can use glob patterns)
- `git commit -m "<commit message>"`: commit staged changes with a message
- `git push origin`: push changes to remote branch
- `git stash -u`: stash changes (to be reapplied later, for example when switching branch)
- `git stash pop`: reapply stashed changes

---

## GitHub
- ### Actions:
  - `CI`: runs on every push and PR, lints and builds the project to make sure everything is fine
  - `CD`: runs on every release from `main`, builds the Docker image and pushes it to Docker Hub
  - `release`: runs on every release PR from `dev` to `main`, generates the release notes and creates a GitHub release
- ### Secrets:
  - `DOCKERHUB_TOKEN`: Docker Hub token for pushing images (make it inside an environment named `production` to restrict access only to the main branch and version tags)
  - `DOCKERHUB_TOKEN`: Admin GitHub user token with contents write permission on that repo (make it inside an environment named `release` to restrict access only to the main branch)
- ### Variables:
  - `DOCKERHUB_USERNAME`: Docker Hub username
  - `DOCKERHUB_IMAGE`: Docker Hub image name
  - `DOCKER_BUILD_CONTEXT`: Docker build context (default: `.`)
  - `DOCKER_CONFIG_FOLDER`: Docker config folder (default: `./.docker`)
- ### Releases:
  - must have a different tag for every version (used by CD action to tag Docker image)
  - must be created from `main` branch
  - tag must be prefixed with `v` (e.g. `v1.2.3`)
  - tag must follow semantic versioning (https://semver.org/)
  - When a new version is pushed to `main`, the Docker image will be tagged with:
    - the full version (e.g. `v1.2.3`)
    - the major and minor version (e.g. `v1.2`)
    - the major version (e.g. `v1`)
    - the `latest` tag (always points to the latest version)

---

## License

This software is distributed under the [MIT License](https://opensource.org/licenses/MIT), see [LICENSE](./LICENSE) for more information.
