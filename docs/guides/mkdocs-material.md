# Setting Up MKDocs-Material
If you like the look and feel of The Villains' Vault, you can make your own version by taking advantage of MKDocs-Material, the free and open source project we use to produce this site.

In this tutorial we'll be using docker & docker-compose to spin up our instance of MKDocs-Material. Hit up [Setting up Docker](https://vault.villains.live/guides/docker/) if you need help getting started there.

## Step 1 - Get Familiar
First make yourself familiar with the two projects, everything you'll find in our guide is more or less taken from these two resources, however we think it's helpful having steps easily spelled out, hence we still produced this guide.

[mkdocs.org](https://www.mkdocs.org)

[squidfunk.github.io/mkdocs-material/](https://squidfunk.github.io/mkdocs-material/)


## Step 2 - docker-compose.yml
Create a docker-compose.yml using the example below on your system wherever you plan to store your container. Make sure to update `line 11` with the path that you're planning to store MKDocs-Material project.

``` yaml title="docker-compose.yml" linenums="1" hl_lines="0"
version: '3'

services:
  mkdocs:
    container_name: mkdocs
    image: squidfunk/mkdocs-material
    restart: unless-stopped
    ports:
      - "8000:8000"
    volumes:
      - /your/path/goes/here:/docs
```
## Step 3 - New project
Setup your project by running this command at the path you defined in your docker-compose file.
```
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .
```
This will create your basic project structor, including a blank mkdocs.yml file.
## Step 4 - Edit mkdocs.yml
Here is a basic example that will get you started:
``` yaml title="mkdocs.yml" linenums="1" hl_lines="0"
#Define the title of the website
site_name: My Project

#Define the theme
theme:
  name: material
```

## Step 5 - Start container
```
docker-compose up -d
```
## Step 6 - Add content

## Step X - Build static site
```
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build
```