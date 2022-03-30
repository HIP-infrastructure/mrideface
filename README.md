# app-template

`app-template` is the template repository for integrating new apps into the HIP. It can be used as a template when creating a new repository for an app.

## :warning: Important :warning:

Please do not hardcode the `app` version number. The version number will be provided to you via the variable `${APP_VERSION}`, so you can use it within the commands to install the `app`. This is necessary to be able to provide seemless updates to users, and properly tag the docker image that will be generated using your `Dockerfile`.

## `Dockerfile` modifications

The provided `Dockerfile` must be completed and modified for the new app that you wish to integrate. All parts of the `Dockerfile` marked with `<>` must be completed:

1. `<base-image:version>`: the following base images are available (all base images are based on Ubuntu 20.04 LTS):
    - `nc-webdav:${DAVFS2_VERSION}`: if you don't need matlab-runtime, the version number will be provided by the `${DAVFS2_VERSION}` variable
    - `matlab-runtime:R2020a_u7`: if you need matlab-runtime version 2020a update 7
    - `matlab-runtime:R2020b_u6`: if you need matlab-runtime version 2020b update 6
 
    You have to use one of these base images, this is mandatory for your app to be integrated into the HIP.

2. `<maintainer@example.com>`: replace by your email address.

2. `<app>`: ubuntu package of the app to be installed. If such a package does not exist, please execute the necessary commands to install it as part of this `RUN` command. Make sure to purge all unnecessary packages at the end to keep the docker image as compact as possible. Do not forget to make use of the `${APP_VERSION}` variable.

3. `<no>`: whether the app must be ran from a terminal. If yes, change to `yes` and leave `</path/to/app/executable>` in step 4 empty, otherwise set to `no`.

4. `</path/to/app/executable>`: absolute path to the app executable.

5. `<app_process_name>`: the exact name of the app process (if several use the parent process). You can find out by installing the app on Ubuntu 20.04 and running the command `ps faux`. This information is important for the health check as well as synchronizing files to Nextcloud after the `app` exited.

6. `<app_config_dir .app_config_dir>`: list of directories necessary for the app to function and retain its state (database, preferences, etc.). These directories will be synced to Nextcloud and mounted into the container. The specified paths must be relative to the user home directory. For now files aren't supported, but support can be added if needed.

7. `<app_data_dir1 app_data_dir2>`: list of data directories necessary to be able to run the app. These directories will be synced to Nextcloud and mounted into the container. The specified paths must be relative to the user home directory. For now files aren't supported, but support can be added if needed.