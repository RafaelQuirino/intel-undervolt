# intel-undervolt

intel-undervolt is a tool for undervolting and throttling limits alteration for Intel CPUs.

Undervolting works on Haswell and newer CPUs and based on the content of
[this article](https://github.com/mihic/linux-intel-undervolt).

## Disclaimer

This tool may damage your hardware since it uses reverse engineered methods of MSR usage.
Use it on your own risk.

## Building and Installing

Run `make && make install` to build and install intel-undervolt to your system.
Run `systemctl daemon-reload` to reload unit files.

## Configuration

You can configure parameters in `/etc/intel-undervolt.conf` file.

### Undervolting

By default it contains all voltage domains like in ThrottleStop utility for Windows.

The following syntax is used in the file: `undervolt ${index} ${display_name} ${undervolt_value}`.
For example, you can write `undervolt 2 'CPU Cache' -25.84` to undervolt CPU cache by 25.84 mV.

### Power Limits

`power package ${short_term} ${long_term}` can be used to alter short term and long term package
power limits. For example, `power package 35 25`.

You can also specify a time window for each limit in seconds. For instance,
`power package 35/5 25/60` for 5 seconds and 60 seconds respectively.

### Temperature Limit

`tjoffset ${temperature_offset}` can be used to alter temperature limit. This value is subtracted
from max temperature level. For example, `tjoffset -20`. If max temperature is equal to 100°C, the
resulting limit will be set to `100 - 20 = 80°C`. Note that offsets higher than 15°C are allowed
only on Skylake and newer.

## Usage

### Applying Configuration

Run `intel-undervolt read` to read current values and `intel-undervolt apply` to apply configured
values. You can apply your configuration automatically enabling `intel-undervolt.service`.

### Measuring the Power Consumption

`intel_rapl` module is required to measure the power consumption. Run `intel-undervolt measure` to
display power consumption in interactive mode.

### Daemon Mode

Sometimes power and temperature limits could be reset by EC, BIOS, or something else. This behavior
can be suppressed applying limits periodically. `intel-undervolt-loop.service` allows you to run
the program in daemon mode which will apply limits with the specified interval. You can change the
interval using `interval ${interval_in_milliseconds}` configuration paremeter.
