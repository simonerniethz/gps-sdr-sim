# GPS-SDR-SIM (UHD USRP Realtime Mod)

GPS-SDR-SIM generates GPS baseband signal data streams, which can be converted to RF using software-defined radio (SDR) platforms. This repository has been modified to stream the samples in real-time to [USRP](http://www.ettus.com/) devices.

### Building with GCC

```
$ make
```

### Generating the GPS signal

A user-defined trajectory can be specified in either a CSV file, which contains 
the Earth-centered Earth-fixed (ECEF) user positions, or an NMEA GGA stream.
The user is also able to assign a static location directly through the command line.

The user specifies the GPS satellite constellation through a GPS broadcast 
ephemeris file. The daily GPS broadcast ephemeris file (brdc) is a merge of the
individual site navigation files into one. The archive for the daily file can 
be downloaded from: https://cddis.nasa.gov/archive/gnss/data/daily/. Access 
to this site requires registration, which is free.

These files are then used to generate the simulated pseudorange and
Doppler for the GPS satellites in view. This simulated range data is 
then used to generate the digitized I/Q samples for the GPS signal.

The simulation start time can be specified if the corresponding set of ephemerides
is available. Otherwise the first time of ephemeris in the RINEX navigation file
is selected.


```
Usage: gps-sdr-sim [options]
Options:
  -e <gps_nav>     RINEX navigation file for GPS ephemerides (required)
  -u <user_motion> User motion file (dynamic mode)
  -g <nmea_gga>    NMEA GGA stream (dynamic mode)
  -c <location>    ECEF X,Y,Z in meters (static mode) e.g. 3967283.154,1022538.181,4872414.484
  -l <location>    Lat,Lon,Hgt (static mode) e.g. 35.681298,139.766247,10.0
  -t <date,time>   Scenario start time YYYY/MM/DD,hh:mm:ss
  -T <date,time>   Overwrite TOC and TOE to scenario start time
  -d <duration>    Duration [sec] (dynamic mode max: 300, static mode max: 86400)
  -o <output>      I/Q sampling data file (default: gpssim.bin)
  -s <frequency>   Sampling frequency [Hz] (default: 4000000)
  -b <iq_bits>     I/Q data format [1/8/16] (default: 16)
  -i               Disable ionospheric delay for spacecraft scenario
  -v               Show details about simulated channels
  -q <gain_db>     Set TX Gain of UHD USRP
  -x <uhd_opts>    Set UHD USRP options
```

The user motion can be specified in either dynamic or static mode:

```
> gps-sdr-sim -e brdc3540.14n -u circle.csv
```

```
> gps-sdr-sim -e brdc3540.14n -g triumphv3.txt
```

```
> gps-sdr-sim -e brdc3540.14n -l 30.286502,120.032669,100
```

### Transmitting the samples

The TX port of the USRP is connected to the GPS receiver under test through a DC block and a fixed attenuator.

### License

Original Version:
Copyright &copy; 2015-2018 Takuji Ebinuma  
Distributed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
This Streaming Version:
Copyright &copy; Simon Erni
Distributed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).