# Bootcamp 2020 Class 1
This repository shows the work done in the first class of [Bootcamp 2020 by PANACLOUD](https://github.com/panacloud/bootcamp-2020)

## Summary of steps to follow along

### Creating a Simple Webpage

1. Create a [GitHub](https://github.com/) account
2. Create a new repo, name it appropriately. Check the option of creating a readme file and confirm
3. Download and install [GitHub Desktop](https://desktop.github.com/)
4. Open GitHub desktop
	- Click on **Add** and select **clone repository**
	- In the search bar, enter the name of newly created repo and select the correct one
	- Finally click on **clone** at the bottom of the dialogue box
5. Now click on **Open in Visual Studio Code** or press <kbd>Ctrl</kbd><kbd>Shift</kbd><kbd>A</kbd>
6. In visual studio code, create a new `html` file `index.html`
	- Inside this `html` file, type `!` and wait until visual studio code completion suggestions are visible. Press <kbd>Enter</kbd> on the option with single exclamation mark.
	- Alternatively, you may also achieve the same result with typing `html:5` and pressing <kbd>Enter</kbd>.
	- **Side Note**: The text entering technique above uses `emmet` plugin builtin many code editors, and the resultant code we got is known as HTML Boilerplate
7. In the body tag, create a new `div` and enter `Hello world from {your name}` and save the file
8. Now in the GitHub Desktop, you will see that `index.html` has been detected and the whole text is highlighted green
	- Green highlight indicates new addition
	- Red highlight indicates deletion (this will be seen when pre-existing code is altered)
9. On bottom left, there is an option to commit the new changes. Above commit button there is an option to add summary and description of new changes. It is considered good practice to always document new changes. Fill the feilds with appropriate info and commit the changes.
10. The changes have been commited on local machine, but the repo in GitHub server has not been changed. To update the GitHub repo, click on **Push origin** or press <kbd>Ctrl</kbd><kbd>P</kbd>

Now the repo on GitHub server has also been updated.

### Deploying on Surge
1. Download and install [Node.js](https://nodejs.org/en/)
2. Open cmd and install surge using following command: `npm install --global surge`
3. Copy the path to the local copy of your repo and enter the following command: `cd {path\to\local\repo}`
	- In linux, mac and GitBash(all OS), the folders are separated by either `/` or `\\` in the directory path
	- In windows, the folders are separated by either `\` or `\\` in the directory path
4. Once in the repo directory, enter the command `surge` and press <kbd>Enter</kbd>
	- Upon first usage, you will be asked to enter email and password to register your account
5. Now you will be prompted to enter the project directory. If the surge command was entered in the project directory, then there is no need to modify anything, otherwise, you must enter the project directory in the prompt. Finally press <kbd>Enter</kbd>
6. Now, you will be prompted to enter domain at which you would like to deploy your webpage. Enter something like `web-address.surge.sh` and press <kbd>Enter</kbd>
7. After few moments you should see

```
	upload: [====================] 100% eta: 0.0s (2 files, 300 bytes)
	CDN: [====================] 100%

             IP: XXX.XXX.XXX.XXX

Success! - Published to web-address.surge.sh
```
### Continuous Integration/Continuous Deployment (CICD)
> Continuous Integration (CI) is a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build (CD), allowing teams to detect problems early. By integrating regularly, you can detect errors quickly, and locate them more easily.
1. In cmd, enter the command `surge token` and copy the token returned by the surge
2. Go to your repo settings and click on **Secrets**
3. On top right, click on **New secret**
	- Enter any suitable name (e.g. `SURGE_KEY` or `SURGE_SECRET` or `SURGE_TOKEN`)
	- Enter the key returned by the surge in cmd
	- Click on **Add secret**
4. On the repo page, click on **Actions**, then click on **set up a workflow yourself**
5. Name the file *deploy_surge.yml* and enter the code below in the code editor:
	- Ensure that you have changed the web-address and token name to address and names you have selected for your particular project
```
# This is a basic workflow to help you get started with Actions

name: Surge Deploy Action

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Install Node.js
        uses: actions/setup-node@v2-beta
        with:
          node-version: 12
      - name: Install Surge
        run: npm install -g surge
      - name: Deploy using surge
        run: surge ./ web-address.surge.sh --token ${{secrets.SURGE_KEY}}
```

6. Commit the creation of the action
7. Go to **Actions** page to verify that the action script runs correctly and that there are no errors


Now any changes made to the repo (committed and pushed) will automatically be updated and deployed to surge
