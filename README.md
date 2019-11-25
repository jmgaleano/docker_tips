# Docker 1-2

Build Docker image using a Dockerfile then do the same in Google Cloud Container Builder and push the image to Container Registry.

- Create a file `app.sh` with:

```.sh
nano app.sh`. 
```

Add the contents into `app.sh` :

```.sh
#!/bin/sh

echo "Hello, world! The time is $(date)."
```

- Create a `Dockerfile` file.

```.sh
nano Dockerfile
```

Add contents:
```
FROM alpine
COPY app.sh /
CMD ["sh" , "/app.sh"]
```

- Make `app.sh` executable using: `chmod +x app.sh`

## Buid Using Dockerfile

[Container Builder](https://cloud.google.com/cloud-build/docs/) allows you to use Dockerfiles to build Docker images. You are not required to have a separate Container Builder `build config file`.

- In the same directory containing `app.sh` and `Dockerfile`, run this command (use your Project ID).

```.sh
gcloud builds submit --tag gcr.io/my-project-id/app-image .
```

We have just built a Docker imaged named app-image using a Dockerfile and pushed the image to Google Container Registry.

## Build images using a build config file

Now let's build the same Docker image as before, but using this time a build config file. The build config instructs Google Container Builder to build the imaged based on the specifications provided.

- In the same directory as `app.sh` and `Dockerfile` create a yalm file:

```.sh
nano cloudbuild.yaml
```

Add the following specifications:

```.sh
steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/quickstart-image', '.' ]
images:
- 'gcr.io/$PROJECT_ID/app-image'
```

- Start the build with this command:

```.sh
gcloud builds submit --config cloudbuild.yaml .
```

The `app-image` has been built using a config file and was pushed to Google Container Registry.

## Run the Docker image

Check your image works as expected using Docker. Make sure Docker command line interface is authorize.

```.sh
gcloud auth configure-docker
```

If yes, then run the image with:

```.sh
docker run gcr.io/my-project-ID/app-image
```

Your output should look similar to this:

```.txt
Hello, world! The time is Mon Nov 25 02:00:56 UTC 2019.
```

## Excellent!
