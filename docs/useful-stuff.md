# Useful Commands

The following are a bnch of useful commands that I use
over and over again... No need to remember this stuff anymore!

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