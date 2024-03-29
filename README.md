# CAMHD Command Files

This repository contains command files for controlling CAMHD. The CAMHD
system is comprised of two computers referred to as the *encode*
(in-water) and *decode* systems. The *encode* system encodes the video
stream and sends it over a 10Gbps network link to *decode* system where it
is decoded and output as an HD-SDI signal.

The encode system runs the *camera server* and the decode system runs the
*camera client*. The command files are processed by the client.

## Command Syntax

Only one command is allowed per line and all lines must be terminated with
a linefeed character (or carriage-return/linefeed combination). Lines
starting with `#` are treated as comments.

Command arguments in brackets are optional.

- **open** *address*

  Open a network connection to the server at IP address *address*. This
  command must be issued before any others.

- **close**

  Close the network connection.

- **sleep** *seconds*

  Pause for a specified number of seconds.

- **adread**

  Read the engineering A/D channels.

- **start** endpoint=*address* [nosensor=1] [lights=NAME] [nopt=1]

  Power-on the video camera, pan/tilt unit, lights, attitude sensor and
  start the video stream. *Address* is the IP address of the **decode
  computer**. If the optional *nosensor* argument is specified, the
  attitude sensor will be disabled. If the optional *nopt* argument is
  specified, the pan/tilt and attitude sensor will be disabled. The
  optional *lights* argument can be used to power-on only one of the
  lights, the value of *NAME* can be either `light_1` or `light_2`. All of
  the subsequent commands can only be issued if the video stream has been
  started.

- **lights** *intensity1* [ *intensity2* ]

  Set the intensity of the lights as a percentage between 0 and 100. If
  only one value is given, it is applied to both lights.

- **lookat** [ pan=*angle* ][ tilt=*angle* ][ speed=*deg/s* ]

  Adjust the camera orientation. The camera orientation is adjusted using
  absolute pan and tilt values. Increasing tilt will point the camera down
  while increasing pan will turn the camera to the right. The valid tilt
  range is 50-140 and the valid pan range is 45-315.

  Specifying the speed is optional. If specified, the speed becomes the
  default for all future Pan/Tilt adjustments.

  If no arguments are given, this command returns the current orientation
  values.

  This command will return an error if the pan/tilt has been disabled.

- **camera** [ zoom=*steps* ][ laser=<on|off> ]

  Adjust video camera settings. The zoom setting is in steps relative to
  the current setting, a positive value zooms in and a negative value
  zooms out. If no arguments are given, the current settings are returned.

- **twait** *HH:MM:SS*

  Pause command processing until an absolute time relative to the start of
  the video stream.

- **stop**

  Stop the video stream and power-off all peripheral devices.
