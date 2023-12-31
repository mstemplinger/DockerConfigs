# Telegraf - InfluxDB 1.8.10

Als erstes ist die Influx DB zu installieren.

Es wird die aktuelle Version von Telegraf HomePage für Windows heruntergeladen.

Diese findet man unter: [InfluxData Downloads](https://www.influxdata.com/downloads/)

oder per Powershell:

```
wget https://dl.influxdata.com/telegraf/releases/telegraf-1.29.1_windows_amd64.zip -UseBasicParsing -OutFile telegraf-1.29.1_windows_amd64.zip Expand-Archive .\telegraf-1.29.1_windows_amd64.zip -DestinationPath 'C:\Program Files\InfluxData\telegraf'
```

In der zu nutzenden Config Datei die Stelle für die Konfiguration der InfluxDB suchen und entsprechend anpassen. In diesem Beispiel wird eine Datenbank "windows_live" generiert.

```
[[outputs.influxdb]]

urls = ["http://192.168.2.2:8086"]

database = "windows_live"
```

Weiter unten in der Datei müssen diese Anpassungen vorgenommen werden.

```
[[inputs.win_perf_counters]]
  [[inputs.win_perf_counters.object]]
    # Process metrics, in this case for IIS only
    ObjectName = "Process"
    Counters = ["% Processor Time",
        "Handle Count",
        "Private Bytes",
        "Thread Count",
        "Virtual Bytes",
        "Working Set"]
    Instances = ["*"]
    Measurement = "win_proc"
    #IncludeTotal=true 
    #Set to true to include _Total instance when querying for all (*).

  [[inputs.win_perf_counters.object]]
    # Processor usage, alternative to native, reports on a per core.
    ObjectName = "Processor"
    Instances = ["*"]
    Counters = ["% Idle Time", 
        "% Interrupt Time", 
        "% Privileged Time", 
        "% User Time", 
        "% Processor Time"]
    Measurement = "win_cpu"
    #IncludeTotal=true 
    #Set to true to include _Total instance when querying for all (*).

    [[inputs.win_perf_counters.object]]
    ObjectName = "PhysicalDisk"
    Instances = ["*"]
    Counters = [
      "Disk Read Bytes/sec",
      "Disk Write Bytes/sec",
      "Current Disk Queue Length",
      "Disk Reads/sec",
      "Disk Writes/sec",
      "% Disk Time",
      "% Disk Read Time",
      "% Disk Write Time",
      "Free Megabytes",
      "Current Disk Queue Length"
    ]
    Measurement = "win_diskio"

    [[inputs.win_perf_counters.object]]
    # Disk times and queues
    ObjectName = "LogicalDisk"
    Instances = ["*"]
    Counters = ["% Idle Time", 
        "% Disk Time",
        "% Disk Read Time", 
        "% Disk Write Time",
        "% Free Space",
        "% User Time", 
        "Free Megabytes",
        "Current Disk Queue Length"]
    Measurement = "win_disk"
    #IncludeTotal=true #Set to true to include _Total instance when querying for all (*).


   [[inputs.win_perf_counters.object]]
    ObjectName = "System"
    Counters = [
      "Context Switches/sec",
      "System Calls/sec",
      "Processor Queue Length",
      "Threads",
      "System Up Time",
      "Processes"
    ]    
    Instances = ["*"]
    #Instances = ["------"]
    Measurement = "win_system"
    # Set to true to include _Total instance when querying for all (*).
    IncludeTotal=true

  [[inputs.win_perf_counters.object]]
    # Example query where the Instance portion must be removed to get data back, such as from the Memory object.
    ObjectName = "Memory"
    Counters = [
      "Available Bytes",
      "Cache Faults/sec",
      "Demand Zero Faults/sec",
      "Page Faults/sec",
      "Pages/sec",
      "Transition Faults/sec",
      "Pool Nonpaged Bytes",
      "Pool Paged Bytes",
      "Standby Cache Reserve Bytes",
      "Standby Cache Normal Priority Bytes",
      "Standby Cache Core Bytes"]
    Instances = ["------"] # Use 6 x - to remove the Instance bit from the query.
    Measurement = "win_mem"
    #IncludeTotal=true #Set to true to include _Total instance when querying for all (*).


  [[inputs.win_perf_counters.object]]
    # more counters for the Network Interface Object can be found at
    # https://msdn.microsoft.com/en-us/library/ms803962.aspx
    ObjectName = "Network Interface"
    Counters = ["Bytes Received/sec",
      "Bytes Sent/sec",
      "Packets Received/sec",
      "Packets Sent/sec",
      "Packets Received Discarded",
      "Packets Outbound Discarded",
      "Packets Received Errors",
      "Packets Outbound Errors"]
    Instances = ["*"] # Use 6 x - to remove the Instance bit from the query.
    Measurement = "win_net"
    #IncludeTotal=true 
    #Set to true to include _Total instance when querying for all (*).

    [[inputs.win_perf_counters.object]]
    # Example query where the Instance portion must be removed to get data back,
    # such as from the Paging File object.
    ObjectName = "Paging File"
    Counters = [
      "% Usage",
    ]
    Instances = ["_Total"]
    Measurement = "win_swap"    
```

Mit diesem Befehl wird der telegraf als Windows Dienst registriert.

```
telegraf --service install --config-directory 'C:\Program Files\telegraf\conf'
```

(Manchmal wird der Dienst jedoch falsch in der Registry hinterlegt. Hier ist der entsprechende Eintrag zu prüfen.)

Danach kann der Telegraf Dienst gestartet und ausgeführt werden.



Die entsprechende Anleitung stammt vom DarkwolfCave:

[Raspberry Pi Monitoring kostenlos - Grafana, InfluxDB mit Portainer und Docker installieren | DarkWolfCave | 23. Juli 2022](https://darkwolfcave.de/raspberry-pi-monitoring-grafana-installieren/)
