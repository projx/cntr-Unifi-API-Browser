# Container Build for Unifi-API-Browser

Container that runs the [UNIFI API Browser](https://github.com/Art-of-WiFi/UniFi-API-browser). This is based upon the excellent work done by [scyto/docker-UnifiBrowser](https://github.com/scyto/docker-UnifiBrowser), but has been updated as follows:

Version 2.0.26

* The Container image is now stored on GHCR, and auto created using GitHub actions. Update your pull URLs to:
  * ghcr.io/projx/cntr-unifi-api-browser:2.0.26
  * ghcr.io/projx/cntr-unifi-api-browser:latest
* The base container is now built using the Alpine Linux v3.2.0
* Releases will follow the same name and versioning as UniFi-API-Browser, so it clearer what you are downloading
* The UniFi-API-Browser is now explicitly stated used the git-clone command ran in the Dockerfile

## Important Links


| Description      | URL                                                                                   |
| :----------------- | --------------------------------------------------------------------------------------- |
| Container Repo   | https://github.com/projx/cntr-Unifi-API-Browser/                                      |
| Registry         | https://github.com/projx/cntr-Unifi-API-Browser/pkgs/container/cntr-unifi-api-browser |
| API Browser Repo | https://github.com/Art-of-WiFi/UniFi-API-browser                                      |
|                  |                                                                                       |
|                  |                                                                                       |

### Running your own

I've tried to keep this simple, you should be able to fork the repo, and create a release tag, remember this should be the same as the release tag for the version of Unifi-API-Browser you wish to use.

### Other Notes

Includes support for UniFiOS on UDMP - see note on ports. The API Browser lets you pull raw, JSON formatted data from the API running on your controller.

## Required Environment Variables

To run this container you will need to define the following variables:


| Environment Variable | Default             | Explanation                                                                                                                                                                                                                                                                                                                   |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| USER                 | Your unifi username | Your username on unifi console - consider creating a restricted user                                                                                                                                                                                                                                                          |
| PASSWORD             | Your unifi password | clear text unifi password                                                                                                                                                                                                                                                                                                     |
| UNIFIURL             | https://192.168.1.1 | URL to your controller*without* the port or trailing / on the URL                                                                                                                                                                                                                                                             |
| PORT                 | 443                 | Port if you changed the port unifi is running on - default env var setting 443 is now the default for UDM / UDMP for older UniFiOS based controllers change to 8443 controllers                                                                                                                                               |
| DISPLAYNAME          | My Site Name        | Arbitrary name you want to refer to this site as in API Browser                                                                                                                                                                                                                                                               |
| NOAPIBROWSERAUTH     | 0                   | use to disable browser auth                                                                                                                                                                                                                                                                                                   |
| APIBROWSERUSER       | admin               | username to secure the API Browser instance                                                                                                                                                                                                                                                                                   |
| APIBROWSERPASS       | see note            | Note: Generate a SHA512 of the password you want and put here, you can use a tool like https://abunchofutils.com/u/computing/sha512-hash-calculator/ by default the password is 'admin' i.e. c7ad44cbad762a5da0a452f9e854fdc1e0e7a52a38015f23f3eab1d80b931dd472634dfac71cd34ebc35d16ab7fb8a90c81f975113d6c7538dc69dd8de9077ec |

## Getting Running

To get started this is the minimum number of options, be sure to append each envar with the required text (esp the SHA512):

`docker run --name unifiapibrowser -p:8000:8000 -e USER= -e PASSWORD= -e UNIFIURL= -e APIBROWSERPASS=   ghcr.io/projx/cntr-unifi-api-browser:latest`

This will run the container on host port 8000/tcp.

## Using Docker Compose / Stack

This is the fastest way to get running for unifios and doesn't require the use of the hash

```
version: '3.8'
services:
  unifiapibrowser:
    ports:
    - 8010:8000
    environment:
      USER: unifi console local account 
      PASSWORD: unifi console password
      NOAPIBROWSERAUTH: 1 # disables auth to apibrowser
      UNIFIURL: https://192.168.1.1
      PORT: 443
      DISPLAYNAME: Home
    image: cntr-Unifi-API-Browser
```

## Using Multiple Unifi Controllers

Unifi-API-Browser supports multiple controllers.  To use them copy the users.php and config.php into a host directory and the map them into the container with the additional following command line options:

`-v <YourHostPath>/config.php:/UniFi-API-browser/config/config.php`

and

`-v <YourHostPath>/config.php:/UniFi-API-browser/config/users.php`

Editing these files is beyond the scope of this readme.md but both contain good instructions

### Building Yourself

This repo does not include any UNIFI-API-Browser source code, this is pulled from their Git Repo as part of the build process, and requires the API-Browser release-tag to be included when calling "docker build", details on how to do this are below.

#### Forking and using Actions

The easiest way to get this running yourself,

1. Decide what version of [UNIFI API Browser](https://github.com/Art-of-WiFi/UniFi-API-browser) you wish to use, make a precise note of the release tag
2. Fork the repo and enable Github Actions
3. On the forked repo, create a new release, add a new tag, this must match the tag noted in #2
4. Github Actions will then initate a build for the container image
5. Once complete, the image will be available from the Repo's Packages registry
6. to initiate the build, simply create a "Release", add a new tag, this must match the release-tag for the version of the API-Browser you wish to use.

#### Build Locally

1. Decide what version of [UNIFI API Browser](https://github.com/Art-of-WiFi/UniFi-API-browser) you wish to use, make a precise note of the release tag
2. Ensure your local device has Git and Docker available
3. Clone the repo to your local machine or server
4. Go into the repo directory, initiate the build with
   * docker build -t unifi-api-browser . --build-arg API_BROWSER_TAG=<value>

### Feedback

If you find any issues please log them at the github repo

This Fork Repo: https://github.com/projx/cntr-Unifi-API-Browser/issues
Original Repo: https://github.com/scyto/docker-UnifiBrowser
