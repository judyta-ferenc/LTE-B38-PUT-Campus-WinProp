# LTE-B38-PUT-Campus-WinProp

The aim of this project was to compare LTE Band 38 (2600 MHz) radio coverage and signal propagation simulation results from Altair WinProp with real-world measurements. The studied base station, BT33378, belongs to the Polish mobile network Plus, operated by Polkomtel Sp. z o.o., and is located at Student Dormitory No. 5 (DS5) of Poznań University of Technology, at 11A Św. Rocha Street in Poznań, Poland.

The project also tries to answer three questions:

1. How well do WinProp simulations match real LTE measurements despite model simplifications?
2. How does MIMO affect network performance, and which configuration gives results closest to real measurements?
3. How does calibration using real-world measurements affect the simulation results?

### 1. Input Data Sources
* **OpenStreetMap** - map and urban area data used to prepare the simulation environment.
* **BTSearch** - map used to locate base station BT33378.
* **SI2PEM** - public database providing base station locations, technologies, frequency bands, and measurement reports, including antenna parameters such as azimuth, height, and downtilt.
* **Altair WinProp examples**
  * `LTE_Band38_BW_20MHz_TDD.wst` - used as the LTE TDD air interface template with a 20 MHz channels at 2585 and 2604.8 MHz.
  * `WiMax_2500MHz_60deg.apb` - used as an approximate 2600 MHz sector antenna pattern.
* **Field measurements** – real LTE signal measurements collected using the NetMonster app.
* **QGIS** - used to prepare measurement locations and compare real measurements with simulation results.

### 2. Field Measurement Results

LTE signal measurements were taken at 14 locations near base station BT33378 on October 19, 2026, between 09:52 and 10:23, using a smartphone with the NetMonster app. The phone received LTE Band 38 signals from three cells operating at two carrier frequencies: **2585 MHz** and **2604.8 MHz**. The measurements included RSSI, RSRP, RSRQ, SNR, GPS coordinates and cell information.

The following cells were identified:

| Band | eNBID | CLID | ECI | PCI | EARFCN | Frequency |
|---|---:|---:|---:|---:|---:|---:|
| B38 | 233378 | 1 | 59744769 | 364 | 37900 | 2585 MHz |
| B38 | 233378 | 3 | 59744771 | 180 | 38098 | 2604.8 MHz |
| B38 | 233378 | 4 | 59744772 | 364 | 38098 | 2604.8 MHz |

The measured values and corresponding WinProp simulation results for SISO, MIMO 2×2 and MIMO 4×4 are saved in Measurements_vs_Simulation.xlc.


### 3. Urban Database Preparation and Preprocessing in WallMan

The urban environment was prepared in **WallMan** using OpenStreetMap data (`map.osm`). The imported data were converted into a WinProp urban database (`map.odb`) representing buildings and the surrounding environment. The database was then preprocessed using **Intelligent Ray Tracing (IRT)** to generate the `map.oib` file and improve propagation simulation efficiency in ProMan.

### 4. Simulation Configuration in ProMan

The LTE network was configured in **Altair WinProp ProMan** using the following parameters.

#### Air Interface and Network Settings

- **Technology:** 4G LTE
- **Multiple access:** OFDM / SOFDMA
- **Band:** LTE Band 38 (2600 MHz)
- **Duplex:** TDD
- **Carrier frequencies:** 2585 MHz and 2604.8 MHz
- **Channel bandwidth:** 20 MHz per carrier
- **Cell assignment:** Highest Rx power of all carriers in the network
- **Minimum required SNIR:** -5.4 dB

#### Propagation and Simulation Settings

- **Propagation model:** Intelligent Ray Tracing (IRT)
- **Prediction height:** 1.5 m
- **Prediction resolution:** 1 m

### 5. Base Station and Antenna Configuration

Base station BT33378 was configured in **ProMan** using antenna parameters from SI2PEM, including azimuth, height, and downtilt.

#### Antenna Sectors

The simulation included two antenna sectors:

| Sector | Azimuth | Downtilt | Antenna height |
|---|---:|---:|---:|
| Sector 1 | 315° | 3.5° | 40 m |
| Sector 2 | 58° | 2.5° | 40 m |

#### Antenna Parameters

- **Antenna pattern:** `WiMax_2500MHz_60deg.apb`
- **Antenna gain:** 17 dBi
- **Polarization:** Linear +45° (SISO), ±45° (MIMO)

The antenna pattern was used as an approximate model for the 2600 MHz band.

#### MIMO Configuration

Three configurations were simulated to compare network performance, with the EIRP per antenna element adjusted to maintain approximately the same total EIRP across all configurations.

| Configuration | EIRP per antenna element |
|---|---:|
| SISO | 50 dBm |
| MIMO 2×2 | 47 dBm |
| MIMO 4×4 | 44 dBm |

For MIMO, ideal separation of spatial streams (no inter-stream interference) was assumed.


### 6. Simulation Results and Comparison

The simulation results were compared with field measurements at the same locations. However, the measured and simulated values do not always come from the same cell or frequency.

The simulation included 2585 MHz and 2604.8 MHz, but only 2585 MHz cells appeared in the Cell Area maps. In contrast, only measurement points 1, 2, and 4 used 2585 MHz. The remaining points used 2604.8 MHz. For this reason, the comparison focuses on signal values at the same locations, rather than on matching the same LTE cells and carrier frequencies.

#### 6.1. Cell Area Assignment

The Cell Area maps show the coverage of individual cells predicted by WinProp, based on the highest received power among all carriers.

These maps are very similar across all three configurations. The MIMO variants, especially MIMO 4×4, show slightly smaller sector coverage areas and a few small gaps. However, the overall coverage pattern remains almost unchanged.

| SISO | MIMO 2×2 | MIMO 4×4 |
|:---:|:---:|:---:|
| ![SISO Cell Area](images/SISO_CELL_AREA.png) | ![MIMO 2x2 Cell Area](images/MIMO2X2_CELL_AREA.png) | ![MIMO 4x4 Cell Area](images/MIMO4X4_CELL_AREA.png) |

#### 6.2. Reference Signal Received Power (RSRP)

RSRP measures the average received power of LTE reference signals transmitted by the base station. It is used to evaluate signal strength and network coverage. The measured RSRP values are marked at the measurement locations.

| SISO | MIMO 2×2 | MIMO 4×4 |
|:---:|:---:|:---:|
| ![SISO RSRP](images/SISO_RSRP.png) | ![MIMO 2x2 RSRP](images/MIMO2X2_RSRP.png) | ![MIMO 4x4 RSRP](images/MIMO4X4_RSRP.png) |

#### 6.3. Received Signal Strength Indicator (RSSI)

RSSI measures the total received power, including the useful signal, interference, and noise. It is used to evaluate the overall received signal strength. The measured RSSI values are marked at the measurement locations.

| SISO | MIMO 2×2 | MIMO 4×4 |
|:---:|:---:|:---:|
| ![SISO RSSI](images/SISO_RSSI.png) | ![MIMO 2x2 RSSI](images/MIMO2X2_RSSI.png) | ![MIMO 4x4 RSSI](images/MIMO4X4_RSSI.png) |

#### 6.4. Reference Signal Received Quality (RSRQ)

RSRQ measures LTE signal quality by comparing the received reference signal power (RSRP) with the total received power (RSSI). Higher RSRQ values indicate better signal quality. The measured RSRQ values are marked at the measurement locations.

All three configurations show lower RSRQ values near the boundary between the two sectors. However, SISO shows the largest areas with higher RSRQ values. In MIMO 2×2 and especially MIMO 4×4, areas with lower RSRQ become more visible, particularly near buildings and other obstacles.

| SISO | MIMO 2×2 | MIMO 4×4 |
|:---:|:---:|:---:|
| ![SISO RSRQ](images/SISO_RSRQ.png) | ![MIMO 2x2 RSRQ](images/MIMO2X2_RSRQ.png) | ![MIMO 4x4 RSRQ](images/MIMO4X4_RSRQ.png) |

#### 6.5. Maximum Downlink Throughput

Maximum Downlink Throughput shows the highest possible data rate from the base station to the user. One of the original LTE design targets, defined by 3GPP, was to achieve peak data rates of around 100 Mb/s in downlink and 50 Mb/s in uplink.

MIMO 4×4 achieved the highest downlink throughput, reaching approximately 226 Mb/s. In comparison, SISO reached only 62 Mb/s. As expected, this difference is mainly due to the use of multiple spatial streams in MIMO, which allows more data to be transmitted simultaneously.

| SISO | MIMO 2×2 | MIMO 4×4 |
|:---:|:---:|:---:|
| ![SISO Downlink Throughput](images/SISO_DL_MAX_TPUT.png) | ![MIMO 2x2 Downlink Throughput](images/MIMO2X2_DL_MAX_TPUT.png) | ![MIMO 4x4 Downlink Throughput](images/MIMO4X4_DL_MAX_TPUT.png) |
