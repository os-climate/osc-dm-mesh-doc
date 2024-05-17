# Useful Commands

The following are a bnch of useful commands that I use
over and over again... No need to remember this stuff anymore!

## Git Feature / Pull Request / Merge Flow

Step 1: Create a Pull Request

First, make sure your local repository is up-to-date and create a new branch for your changes:

Navigate to your repository
~~~~
cd /path/to/your/repository
~~~~

Ensure the local main branch is up-to-date
~~~~
git checkout main
git pull origin main
~~~~

Create and switch to a new branch
~~~~
git checkout -b new-feature-branch
~~~~

Step 2: Make your changes, add them, and commit:

Add changes to staging
~~~~
git add .
~~~~

Commit changes
~~~~
git commit -m "Description of changes"
~~~~

Step 3: Push your new branch to GitHub:

Push branch to GitHub
~~~~
git push origin new-feature-branch
~~~~

Step 4: Create a pull request (PR) on GitHub.

Go to your repository on GitHub.
Click on the "Compare & pull request" button next to your new branch.
Fill in the details for the pull request and create it.

Step 5: Merge the pull request branch:

Switch to main branch
~~~~
git checkout main
~~~~

Pull the latest changes
~~~~
git pull origin main
~~~~

Merge the branch (replace 'new-feature-branch' with your branch name)
~~~~
git merge new-feature-branch
~~~~

Step 6: Cleanup

After merging, it's a good practice to delete the branch both locally
and remotely to keep your repository clean.

Delete the branch locally:
~~~~
git branch -d new-feature-branch
~~~~

Delete the branch remotely:
~~~~
git push origin --delete new-feature-branch
~~~~

## Cleanup after GitHub Merge

After you have merged a pull request on GitHub, it is
time to remove to branch on your local machine:
~~~~
git fetch
git checkout main
git pull origin main

git branch -d <branch-name>
~~~~

## Create a new Branch

~~~~
BRANCH_NAME="feature/issue-1-some-description"
git checkout -b "$BRANCH_NAME"
~~~~

## Rename a branch

(Assuming you have not commited anything yet)
~~~~
BRANCH_NAME="feature/issue-1-some-description"
git branch -m "$BRANCH_NAME"
~~~~

## Push All Code to GitHub Branch

~~~~
MESSAGE="Convert/add logging/tracing middleware" ;
BRANCH="main" ;
git add * ;
git commit -m "$MESSAGE" ;
git push -u origin "$BRANCH" ;
~~~~

## Creating a Release/Tag

Create a new release/tag, push to GitHub, and checkout main
~~~~
git add * ;
git commit -m "Convert BGS/bgs references to OSC/osc" ;
git push -u origin main ;

VERSION="v0.1.1" ;
MESSAGE="Release version $VERSION" ;
echo "Creating release version: $VERSION (commit message: $MESSAGE)" ;
git tag -a "$VERSION" -m "$MESSAGE" ;
git push origin "$VERSION" ;
git checkout main ;
~~~~

## Publishing the Docker Image

Build the docker image and publish to Docker Hub
(requires DOCKERHUB_USERNAME and DOCKERHUB_TOKEN)
to be set:
~~~~
$PROJECT_DIR/bin/dockerize.sh --publish ; date
~~~~

## Clean out Local Docker Environment

~~~~
docker system prune -a --volumes
~~~~