SignalFx Meta-Data Plugin
==============================

Provides meta-data about host and will send collectd notifications to SignalFx if configured to do so.

Example Config:

```
LoadPlugin python
<Plugin python>
  ModulePath "/opt/signalfx-collectd-plugin"
  LogTraces true
  Interactive false
  Import "signalfx_metadata"
  <Module signalfx_metadata>
    Notifications true
    URL "https://ingest.signalfx.com/v1/collectd"
    Token "<<<<<<INSERT_TOKEN_HERE>>>>>>"
    NotifyLevel "OKAY"
  </Module>
  Import "collectd_ps_plugin"
  <Module collectd_ps_plugin>
    ReportDockerContainerNames	false
  	MinRuntimeSeconds	30
  	FilterProcesses		true
  	MinCPUPercent		25
  	MinMemoryPercent	25
  </Module>
</Plugin>
```

For metadata:

* Notifications: do we want to emit notifications from the plugin true or false. Default is false.
* URL: where to emit notifications via json to. The example url is the default.
* Token: api token from signalfx to authenticate. No default.
* NotifyLevel: If you want to emit notifications beyond the ones generated by this plugin, set to
  the appropriate level. "OKAY" would mean all notifications are emitted.  "ERROR" would just be
  error.  "WARNING" would include "ERROR" and "WARNING".

For ps:

This is different from the built-in processes plugin:

* it can filter out processes based on CPU and memory thresholds
* it can send Docker container names for processes where applicable

There are plans to update the C processes plugin to do the same thing.

The following metrics are reported for each process:

* CPU usage percentage
* Memory usage percentage
* I/O bytes read
* I/O bytes written
* Thread count
* Open file descriptor count
* No. of voluntary context switches
* No. of involuntary context switches

The name of the process is used for the `plugin_instance` field.

The PID and PPID are reported as dimensions within `plugin_instance`.

* ReportDockerContainerNames: should be true if you want the docker container names.
* MinRuntimeSeconds: how long a process should run before we start reporting
* FilterProcesses: should be true if you want to filter processes.  false means all processes
* filter criteria
  *	MinCPUPercent		25
  *	MinMemoryPercent	25
