# CS_190N_Project

## Our Data

### Device Data Columns

1. `label`: identifier for each Raspberry Pi device (`raspi-` format followed by a MAC address)
2. `ethernet_mac`: MAC address for the Ethernet interface of the device (a wired network identification)
3. `wlan_mac`: MAC address for the WiFi interface of the device, used for wireless network identification.
4. `location`: Physical location of the device (spatial analysis of connectivity?)
5. `tags`: tags specifying building or area names on campus.
6. `comment`: comments
7. `uptime`: duration the device has been online -> we can assess device stability.
8. `wireless_bytes`: amount of the data transmitted/received over WiFi -> wireless network usage by the device.
9. `wired_bytes`: amount of data transmitted/received over Ethernet -> wired network usage.
10. `last_seen`: Timestamp indicating the last time the device was active -> device connectivity status.

### WiFi Data Columns

1. `signal (dBm)`: signal strength in decibels relative to one milliwatt -> the quality of the WiFi connection.
2. `packet_loss`: % of packets lost during transmission -> metric for network reliability.
3. `connected_time (s)`: duration in secs the device has been connected to the network -> session length.
4. `tx_bitrate (MBit/s)`: transmission bitrate in megabits per sec -> shows the data rate for sending data.
5. `rx_bitrate (MBit/s)`: reception bitrate in megabits per sec -> the data rate for receiving data.
6. `min_ping_rtt (ms)`: minimum round-trip time in millisecs for a ping request -> shows would-be best-case network latency.
7. `avg_ping_rtt (ms)`: avg round-trip time in millisecs -> gives a general sense of latency.
8. `max_ping_rtt (ms)`: Maximum round-trip time in millisecs -> captures worst-case latency.
9. `mdev_ping_rtt (ms)`: Mean deviation of round-trip times -> shows us variability in network latency.

## Instructions to Reproduce the Code

The data we used can be downloaded using these commands (adjust to where data is stored on your computer):

```python
wifistats_df = pd.read_csv("./data/wifistats.csv")
extra_info_df = pd.read_csv("./data/extra_info.csv")
devices_df = pd.read_csv("./data/devices.csv")
df = pd.merge(wifistats_df, extra_info_df, on="label", how="inner")

```
This data is collected from the PINOT infrastructure, which consists of ~70 Raspberry Pi devices (single board computers) deployed over the campus. You can read more about the infrastructure here: https://pinot.cs.ucsb.edu/.
Each raspberry pi device is connected to a wireless network (UCSB Wireless Web) and run measurements every several minutes. These measurements include collecting wifi statistics information (signal strength, tx/rx bitrate, etc) and running ping to the UCSB gateway router. The source code of the measurements is available here: https://github.com/SNL-UCSB/PINOT-UCSB-WiFi-watcher. All measurements (with a corresponding timestamp) are available in the PostgreSQL database.
The database stores measurements in a raw (unparsed) form. Currently, we have 4.4mln measurements, earliest starting on April 2023. The database is available at pinot.cs.ucsb.edu, port 5432. Please, use pgAdmin to connect to the database. The table with data is located in db pinot, public schema, table wifistats. I will send credentials per group as a separate email to you.

