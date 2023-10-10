# cloudeye-grafana
[![LICENSE](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://github.com/sbercloud-terraform/cloudeye-grafana/blob/master/LICENSE)

Сloudeye-grafana is a datasource plugin to adapt to Grafana.

# Quick start

## 1. Installation
> Preparation before installation:
> a. Grafana version >=7.4.0, [grafana](https://grafana.com/grafana/download)  
> b. Download cloudeye-grafana.tar.gz from [release](https://github.com/sbercloud-terraform/cloudeye-grafana/releases)

### 1.1 Installation from release
a. Place the downloaded plugin package into the grafana plugin directory (see the plugins configuration path in conf/defaults.ini),
decompress cloudeye-grafana-{version}.tar.gz, and pay attention to maintaining directory permissions and grafana running permissions.
  
b. Modify conf/defaults.ini to allow unsigned plugins to run 
> allow_loading_unsigned_plugins = cloudru-cloudeye-grafana  
   
c. Restart grafana

## 2. Configure cloudeye-grafana data source
> Preparation before configuration: 
> a. [Obtain AK/SK](https://support.hc.sbercloud.ru/devg/apisign/api-sign-provide-aksk.html)  
> b. [Obtain project_id](https://support.hc.sbercloud.ru/devg/apisign/api-sign-provide-proid.html)  
> c. [Obtain CES Endpoint](https://support.hc.sbercloud.ru/endpoint/index.html)

### 2.1 Configure data source
a. Enter grafana's data source configuration page (Data Sources), click Add data source to enter the configuration form page,
fill in the data source name cloudeye-grafana, and select cloudeye-grafana in the data source list.

b. (Optional) If you need to enable reading indicator metadata through the configuration file, you need to click
the `Get Metric Meta From Conf file` button to enable it, and configure the indicator metadata [list as follows](#metrics).

c. Click the `Save & test` button. If `Data source` is working is displayed, it means that the data source is configured successfully and you can start accessing Cloudru monitoring data in grafana.


<a name="metrics"></a>
## 3. (Optional) Configure indicator metadata list
In order to improve the query experience, for tenants whose resource list changes in real time and has a large amount of resources,
the resource list can be configured in the dist/metric.yaml file in advance. The region/service/resource/metric list shall be subject to the configuration file.  
> a. [List of service indicators supported by cloud monitoring](https://support.hc.sbercloud.ru/usermanual/ces/en-us_topic_0202622212.html)
> b. After completing the configuration according to the metric.yaml sample, restart grafana
    
## 4. Import dashboard template
To facilitate tenant configuration, this plugin provides Dashboard templates for ECS, ELB, and RDS services, see: `cloudeye-grafana/src/templates` directory

## 5. Create a custom Dashboard
a.  Move the mouse to the "+" icon in the menu on the left side of the page, select Dashboard, and click to create

b. After creation, please click the gear icon in the upper right corner, select the "Variables" menu item on the left, 
and click the "Add variable" button to add filter and period template variables. The variable configuration is as follows
> filter variable:
```
{
    Name: filter,
    Type: Query,
    Label: Filter,
    Data source: cloudeye-grafana,
    Query: listFilterOptions()
}
```

> period variable:
```
{
    Name: period,
    Type: Query,
    Label: Period,
    Data source: cloudeye-grafana,
    Query: listPeriodOptions()
}
```

c. After configuring the custom template variables, return to the Dashboard page and click the "Add an empty panel" button to add an indicator monitoring chart.

d. Click the save button in the upper right corner to complete the custom Dashboard creation
