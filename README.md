# SLIM-ORTHANC

Adaption of slim viewer to an orthanc backend.

## Architecture of this Repo

This repo hosts all files to run a orthanc pacs with slimviewer as frontend.

The [.env]() file is used to define the order of the compose files to laod.

The compose files are organized as follows:

- ***[compose/oprthanc.yml]()*** is hosting all components of the orthanc backend including the minio S3 storage and postgres db.

- ***[compose/init.yml]()*** provides an init container which is intended to run once on startup, to initialize the S3 storage bucketes.

- ***[compose/slimviewer.yml]()*** provides the frontend customized nginx container build from the [slim]() dir.

- ***[compose/static_ports.yml]()*** extends the compose environment, to use static ports if needed ... see next section

### Static ports

- 8042 orthanc ui
- 8000 slimviewer ui
- 9000 minio

## Launch

Start the application with: `docker compose up -d --force-recreate --remove-orphans`

## Build the Slim-Viewer Docker-Image for changes

1. Clone slim
    ```
    git clone https://github.com/ImagingDataCommons/slim.git
    ```

1. Copy the [env/wsi4dicom.js]() to `<slim-clone-cd>/public/config/wsi4dicom.js`

1. Build the slim-viewer image ... this repository uses the intermediate layers to build the custom image
    ```bash
    docker build . -t slim-app --target app --build-arg "REACT_APP_CONFIG=wsi4dicom"
    ```

1. run `docker compose up -d --build --force-recreate --remove-orphans`