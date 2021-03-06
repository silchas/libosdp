# osdpctl - Tool to create manage and control OSDP Devices

osdpctl is a tool that uses libosdp to setup/manage/control osdp devices. It
serves as a reference point for those who intend to consume this library. This
tool cannot be used directly in applications as most of the time a lot more
product specific customizations are needed.

This tools brings in a concept called **chanel** to describe the communication
between a CP and PD. Although OSDP defines this protocol to run on RS458
(serial), it would be very convenient to run this over other mediums too. To
know more about this have a look at `channel_*.c` files.

## Configuration files

The `osdpctl` uses a config file to determine settings that are needed to
rightly configure libosdp. This file determines if the service is to run as a CP
or PD. You can read more about [osdpctl configuration file][1] and the various
keys it can contain in the [documentation][2] section. Some sample configuration
files can be found inside the `config/` directory here.

## Start / Stop a service

An OSDP service can be started by passing a suitable config file to `osdpctl`
tool. You can pass the `-f` flag to fork the process to background and the `-l`
flag to log output to a file. So to start a OSDP process as a daemon, you can
do the following:

```bash
osdpctl start osdp-config.cfg -f -l /tmp/osdp-device.log
```

To stop a running service, you must pass the same configuration file that was
used to start it.

```bash
osdpctl stop osdp-config.cfg
```

## Send control commands to a OSDP service

A command to be sent to a running service must be of this format:

```
osdpctl send osdp-config.cfg <PD-OFFSET> <COMMANDS> [ARG1 ARG2 ...]
```

Here, `PD-OFFSET` is the offset number (starting with 0) of the PD in the
config file osdp-config.cfg. In PD mode, since there is only one PD, this is
always set as 0.

This section will only document the `COMMANDS` and their arguments. You must
prefix `osdpctl send osdp-config.cfg <PD-OFFSET>` to each of these commands for
them to actually get through.

### CP commands

The following commands can be passed to a OSDP device that is setup as a CP. The
PD to which these commands are being sent must have the capability of executing
them. Refer to the [PD capabilities document][3] for more details.

**led:**

This command is used to control LEDs in a PD. It is of the format:
`led <led_no> <color> <blink|static> <count|state>`.

Examples:
```
led 0 red blink 5        # blink LED number 0 in red color for 5 times
led 1 amber blink 0      # blink LED number 1 in amber color forever
led 2 green static 1     # Turn on LED number 2 green color
led 1 blue static 0      # Turn off LED number 1 blue color
```

**buzzer:**

This command is used to control Buzzers in a PD. It is of the format:
`buzzer <blink|static> <count|state>`

Examples:
```
buzzer blink 0          # beep the buzzer forever
buzzer blink 5          # beep the buzzer 5 times
buzzer static 1         # turn off the buzzer
buzzer static 0         # turn on the buzzer
```

**output:**

This command is used to control LEDs in a PD. It is of the format:
`output <output_number> <state>`

Examples:
```
output 0 1        # Set output number 0 high
output 2 0        # Set output number 2 low
```

**text:**

This command is used to control the text that is displayed on the PD. It is of
the format: `text <string>`

Examples:
```
text 'Hello World'     # Set text "hello world" in display
```

**comset:**

This command is used to control LEDs. It is of the format: ``

[1]: /doc/osdpctl-configuration.md
[2]: /doc/README.md
[3]: /doc/osdp-pd-capabilities.md
