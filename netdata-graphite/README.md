# netdata Graphite Dashboard
Metrics provided by netdata, written into Graphite and displayed via Grafana!

## Compatibility
- netdata v1.8.0
- It should display the most panels for a default installation of linux. 
- It's tested on Debian Stretch x64, OSMC (on Raspberry PI2) & armbian (on OrangePi+2E).
- This dashboard uses templating heavily but as a limitation of grafana it is not possible to hide panels with no data.

## Metric Field description
1. `datasource` = your graphite datasource for this dashboard.
1. `rootdir` = also known as prefix, used to summarize metrics of a collector.
1. `server` = Server Name without `.`. Please see *Rewrites* Section.

### Additional Metrics
You can see them at the top of the dashboard.
1. `interface` = Network interfaces
1. `tc` = Quality of Service
1. `cpu` = CPUs
1. `disk` = Disk IOPs
1. `disk_space` = Disk Space
1. `cgroupplugin` = Output of cgroup plugin
1. `pythonplugin` = Output of python plugin

## Dependencies
- netdata (https://my-netdata.io/)
- Graphite 
- Grafana

## Rewrites
**Note**: Please keep in mind that hostnames usually contain `.`. You may want to consider symlinking in the storage directory instead or do rewriting of metrics, as this dashboard uses only the 2nd field for the hostname. `eth` sub interfaces (like `eth0.1`) and `cgroupplugin` needs rewrites as well. A sample configuration for `carbon-c-relay` is provided above.

### carbon-c-relay rewrites
```
# Rewrite Host
rewrite ^netdata\.([a-z0-9]+)\.example\.com\.
    into netdata.\1_example_com.
    ;

# Rewrite network subinterfaces
rewrite ^netdata\.([a-z0-9\_]+)\.net\.(eth[0-9]+)\.([0-9]+)\.
    into netdata.\1.net.\2_\3.
    ;

# Fix recursion of cgroup plugin
rewrite ^netdata\.([a-z0-9\_]+)\.(cgroup\_[a-z0-9\_]+)\.(cgroup\_[a-z0-9\_]+\.)+
    into netdata.\1.\2.
    ;
```


## Configuration
- Optional but recommended: deploy provided rewriting
- Configure netdata to write to Graphite
- Import Dashboard and select the appropriate values at the top variable list for: *datasource*, *rootdir*, *server*.
- Adopt refresh interval to fit your needs.
- Enjoy!

## Links
- [github](https://github.com/schachr/grafana-dashboards/tree/master/netdata-graphite): https://github.com/schachr/grafana-dashboards/tree/master/netdata-graphite
- [grafana](https://grafana.com/dashboards/3938): https://grafana.com/dashboards/3938
