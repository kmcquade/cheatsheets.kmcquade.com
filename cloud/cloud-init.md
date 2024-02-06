# Cloud-init

### Rerun cloud-init stages

```
# stage 1
cloud-init init --local
# stage 2
cloud-init init
# stage 3
cloud-init modules --mode=config
# stage 4
cloud-init modules --mode=final

# Run specific stage once
cloud-init single --name rh-subscription --frequency once
```

References:

[https://cloudinit.readthedocs.io/en/latest/howto/rerun\_cloud\_init.html](https://cloudinit.readthedocs.io/en/latest/howto/rerun\_cloud\_init.html)

### View userdata output

`cat /var/log/cloud-init-output.log`

### Analyze cloud-init boot time analytics

* `analyze show` Parse and organize cloud-init.log events by stage and include each sub-stage granularity with time delta reports.

```
$ cloud-init analyze show -i my-cloud-init.log
-- Boot Record 01 --
The total time elapsed since completing an event is printed after the "@"
character.
The time the event takes is printed after the "+" character.

Starting stage: modules-config
|`->config-emit_upstart ran successfully @05.47600s +00.00100s
|`->config-snap_config ran successfully @05.47700s +00.00100s
|`->config-ssh-import-id ran successfully @05.47800s +00.00200s
|`->config-locale ran successfully @05.48000s +00.00100s
...
```

* `analyze dump` Parse cloud-init.log into event records and return a list of dictionaries that can be consumed for other reporting needs.

```
$ cloud-init analyze dump -i my-cloud-init.log
[
 {
  "description": "running config modules",
  "event_type": "start",
  "name": "modules-config",
  "origin": "cloudinit",
  "timestamp": 1510807493.0
 },...
```

* `analyze blame` Parse cloud-init.log into event records and sort them based on highest time cost for quick assessment of areas of cloud-init that may need improvement.

```bash
cloud-init analyze blame -i my-cloud-init.log
-- Boot Record 11 --
     00.01300s (modules-final/config-scripts-per-boot)
     00.00400s (modules-final/config-final-message)
     00.00100s (modules-final/config-rightscale_userdata)
     ...
```

Solid references:

* [cloud-init guide](https://github.com/madorn/cloud-init-guide)
* [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/topics/debugging.html#boot-time-analysis-cloud-init-analyze)

###
