# Container Build for Unifi-API-Browser

Container that runs for unifi browser [https://github.com/Art-of-WiFi/UniFi-API-browser](https://github.com/Art-of-WiFi/UniFi-API-browser). This is based upon the excellent work done by [scyto/docker-UnifiBrowser](https://github.com/scyto/docker-UnifiBrowser).

### Version 2.0.26

I've made some tweaks:

* The Container image will be built upon release, by GitHub actions and pushed to GHCR
* Releases will follow the same name and versioning as UniFi-API-Browser, so it clearer what you are downloading
* The UniFi-API-Browser is now explicitly stated in the git-clone command ran in the Dockerfile

### Other Notes

Includes support for UniFiOS on UDMP - see note on ports

The API Browser lets you pull raw, JSON formatted data from the API running on your controller.

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

`docker run --name unifiapibrowser -p:8000:8000 -e USER= -e PASSWORD= -e UNIFIURL= -e APIBROWSERPASS=    scyto/unifibrowser`

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

### Feedback

If you find any issues please log them at the github repo

This Fork Repo: https://github.com/projx/cntr-Unifi-API-Browser/issues
Original Repo: https://github.com/scyto/docker-UnifiBrowser
