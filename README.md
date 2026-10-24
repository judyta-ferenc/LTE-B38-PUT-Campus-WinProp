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

LTE signal measurements were taken at 15 locations near base station BT33378 on October 19, 2026, between 09:52 and 10:23, using a smartphone with the NetMonster app. The phone received LTE Band 38 signals from three cells operating at two carrier frequencies: **2585 MHz** and **2604.8 MHz**.

The following cells were identified:

| Band | eNBID | CLID | ECI | PCI | EARFCN | Frequency |
|---|---:|---:|---:|---:|---:|---:|
| B38 | 233378 | 1 | 59744769 | 364 | 37900 | 2585 MHz |
| B38 | 233378 | 3 | 59744771 | 180 | 38098 | 2604.8 MHz |
| B38 | 233378 | 4 | 59744772 | 364 | 38098 | 2604.8 MHz |

The measurement data are saved in `arkusz.csv` and `measurement_points.gpkg` for analysis in QGIS and comparison with WinProp results.


### 3. Urban Database Preparation and Preprocessing in WallMan

The urban model was created in **WallMan** from OpenStreetMap vector data (`map.osm`) and converted into a WinProp urban database (`map.odb`) containing buildings, vegetation and other physical obstacles affecting radio wave propagation. To improve the simulation workflow in ProMan, the environment was preprocessed using **Intelligent Ray Tracing (IRT)**.


### 4. ProMan Network Planning Project

A new network planning project was created in **ProMan** using the preprocessed urban database (`map.oib`).

Key simulation parameters:
* **Technology:** 4G LTE
* **Band:** LTE Band 38 (2600 MHz)
* **Duplex:** TDD
* **Carrier frequencies:** 2585 MHz and 2604.8 MHz
* **Channel bandwidth:** 20 MHz per carrier
* **MIMO:** SISO, MIMO 2×2, and MIMO 4×4
* **Prediction height:** 1.5 m
* **Prediction resolution:** 1 m

### 5. Base Station Configuration

Base station BT33378 was configured in **ProMan** using antenna parameters from SI2PEM, such as azimuth, height, and downtilt. The `WiMax_2500MHz_60deg.apb` antenna pattern was used as an approximate model for the 2600 MHz band.

The simulation included two antenna sectors with the following parameters:

| Sector | Azimuth | Downtilt | Antenna height |
|---|---:|---:|---:|
| Sector 1 | 315° | 3.5° | 40 m |
| Sector 2 | 58° | 2.5° | 40 m |

#### General Antenna Settings

* **Antenna pattern:** `WiMax_2500MHz_60deg.apb`
* **Polarization:** Linear +45° (SISO), ±45° (MIMO)
* **Antenna gain:** 17 dBi
* **MIMO configurations:** SISO, MIMO 2×2, MIMO 4×4
* **Max. Tx Power:** 50 dBm (SISO), 47 dBm (MIMO 2×2), and 44 dBm (MIMO 4×4) EIRP per antenna element, to keep the same total EIRP for all configurations.


### 6. Simulation Results - Variant A: SISO

