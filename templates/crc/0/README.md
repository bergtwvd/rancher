# Image
`app-docker136.hex.tno.nl:443/viking18/crc:5.3.2.1L`
# Description
This image is the Pitch CRC (Central RTI Component). The CRC is licensed against a certain MAC address, hence the container must have this mac-address.
# Synopsis
`docker run <docker options> pitch/crc:<tag> <container options>`

# Docker options
`--mac-address=00:18:8B:0D:4F:0B` : MAC address for license purposes. **Required**.

`-v <my settings file>:/root/prti1516e/prti1516eCRC.settings` : use the given settings file as base rather than the default settings file. Optional.

`-e CRC_NICKNAME=<crc name>` : Use this as the nickname for the CRC. Default is `crc-<container hostname>`. Optional

`-e CRC_PORT=<crc port>` : Use this CRC port number. Default is `8989`. Optional.

`-e CRC_SKIP_CONNECTIVITY_CHECK=<boolean>` : Skip connectivity test back to connecting LRC. Default is `false`. Optional.

`-e CRC_REJECT_MISMATCHED_VERSIONS=<boolean>` : Reject LRCs with miss-matching version. Default is `true`. Optional.

`-e CRC_BOOSTERADDRESS=<booster host>:<booster port>` : Start in Booster Mode, using the given booster host address and port number. Default is Direct Mode. Optional.

`-e DISPLAY=<XServer host>:<Display number>` : if set, use this XServer display as GUI. For example `xserver:0`. Optional.

If `CRC_BOOSTERADDRESS` and/or `DISPLAY` is set then the container will wait for the XServer and Booster to start. The number of wait attempts is determined by the `-w` option.

# Container options
`-h` : Display help information. Optional.

`-l <key>` : run license activator with the given key.

`-i` : Set interactive mode. This option should be used in combination with the docker option `-i` in order to use the container TTY. Default is non-interactive. Optional.

`-w <attempts>` : Number of attempts to wait for XServer (if `DISPLAY` set) and Booster (if `CRC_BOOSTERADDRESS` set), before starting the CRC (non-negative integer, default is indefinite).

# Example
## Example A: run CRC as a daemon 
Run the CRC as a daemon and give the container the name “crc”; other containers can use this name to link to:

````
docker run -d \
  --mac-address=00:18:8B:0D:4F:0B \
  --name crc app-docker136.hex.tno.nl:443/viking18/crc:5.3.0.0L
````

## Example B: run CRC with interactive command line
Run the CRC in interactive mode to use the CRC command line prompt:

````
docker run -it \
  --rm \
  --mac-address=00:18:8B:0D:4F:0B \
  --name crc app-docker136.hex.tno.nl:443/viking18/crc:5.3.0.0L -i
````

## Example C: run CRC with a GUI
Run the CRC as in example A, but using a GUI:

Start XServer first:

````
docker run -d \
  -p 8080:8080 \ 
  --name xserver app-docker136.hex.tno.nl:443/library/xserver
````

Start CRC:

````
docker run -d \
  --mac-address=00:18:8B:0D:4F:0B \
  --link xserver \
  -e DISPLAY=xserver:0 \
  --name crc app-docker136.hex.tno.nl:443/viking18/crc:5.3.0.0L
````

And now open the web page ``http://<host>:8080/vnc.html``.