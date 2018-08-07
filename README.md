# NC Water Data Exploration

Descriptions of various water-related data sources and some Python notebooks demonstrating how the data can be downloaded/scraped and visualized. 

[TOC]

---

## ♦ Executive Summary♦:

I spend a chunk of time exploring various internet resources for useful water data. I evaluated these sites on the relevance of the data they provided as well as how easily the data could be accessed and downloaded. I found a number of useful national, state, and some local datasets which are summarized and evaluated below. I also generated a number of Python snippets, provided as interactive Python notebooks (ipynb) files that show how some of these datasets are accessed and visualized. 

**Summary of datasets evaluated**

| Source | Dataset                     | Ease of Access (1-5) | Data Utility (1-5) | Comments                                                     |
| ------ | --------------------------- | :------------------: | :----------------: | ------------------------------------------------------------ |
| EWG    | Tap Water Database          |          ♣♣          |        ♣♣♣         | Not updated, Hard to scrape                                  |
| EPA    | Consumer Confidence Reports |         ♣♣♣          |         ♣♣         | Data gaps, Inconsistent                                      |
| EPA    | ECHO                        |        ♣♣♣♣♣         |       ♣♣♣♣♣        | Good, but so much data it's hard to know what's what         |
| EPA    | SWDIS                       |          ♣♣          |        ♣♣♣         | Poorly designed interface. Data possibly redundant with ECHO. |
| EPA    | PCS/ICIS                    |          ♣♣          |        ♣♣♣♣        | Data rich, but hard to scrape. Slow.                         |
| NWQMC  | Water Quality Portal        |        ♣♣♣♣♣         |       ♣♣♣♣♣        | Well organized and comprehensive data                        |



---

## I. National Data Sources

### A. EWG's Tap Water Database

- **<u>Conclusion</u>**: **Some utility, but difficult to scrape and the data are likely available elsewhere** 
  - A well-organized central repository, useful for cross-referencing other scrapes.
  - Requires code to iterate through water districts and scrapes formatted pages. 

- <u>Link</u>: https://www.ewg.org/tapwater/index.php
- <u>Summary</u>: The Environmental Working Group contacted each state for water quality reports and compiled a centralized database of water quality metrics for each water supplier. 
- <u>Data Link</u> (NC): https://www.ewg.org/tapwater/state.php?stab=NC
  - Returns HTML page containing JavaScript that calls up other data; <u>not easily scraped</u>. 
    - https://www.ewg.org/tapwater/search-results.php?stab=NC&searchtype=largesys
  - However, can query: https://www.ewg.org/tapwater/system.php?pws=NC0102020 (replacing numeric code with id's found here: https://www.ncwater.org/Water_Supply_Planning/Local_Water_Supply_Plan/search.php) to get a somewhat scrape-able page of contaminant levels. 
- <u>Other</u>: Would EWG share their raw database? 



### B. EPA Consumer Confidence Reports (CCR)

- **<u>Conclusion</u>**: **Not Helpful**
  - While scraping is fairly straightforward, the data are inconsistent with many missing values
- <u>Link</u>: https://www.epa.gov/ccr
- <u>Summary</u>: Community Water Systems (CWS) are required to provide drinking water quality reports to customers. 
- <u>Data link</u>: https://ofmpub.epa.gov/apex/safewater/f?p=136:102::::::
  - Cryptic REST interface, but state summary data can easily be scraped.
  - Summary data includes few details, relevant data (City, County, Population Served). Some have website links, but not all are working. Links likely go to glossy summaries that don't facilitate scraping. 
- <u>Code</u>:
  - `EPA/Explore-CCR-Reports.ipynb`: a brief exploration of pre-downloaded data in CSV format.



### C. EPA Enforcement and Compliance History Online (ECHO)

- **<u>Conclusion</u>**: ***Promising!***
  - Established data services capabilities with documented web services
  - Holds many datasets, though so much that it's somewhat confusing what it holds:
    - Drinking Water: https://echo.epa.gov/tools/web-services/facility-search-drinking-water
    - Water Facility: https://echo.epa.gov/tools/web-services/facility-search-water
- <u>Link</u>: https://echo.epa.gov/
- <u>Summary</u>: Provides compliance and enforcement information for over 900,000 regulated facilities nationwide. Allows query at state/county/city/zip level for a table of facilities and their compliance records. Not limited to water (NPDES and drinking water); includes air, hazardous waste,...
- <u>Data</u>:
  - Main pages searches by form. Not REST interface. CSV's generated with temporary link. 	
  - [!] Issue with NC Data: https://echo.epa.gov/resources/echo-data/known-data-problems#NCalerts
  - <u>Web services</u> provided: https://echo.epa.gov/tools/web-services
    - Documentation is a bit obtuse, generates temporary result files (valid for 30 min)
- <u>Code</u>:
  - `EPA/EPA-ECHO-DMR.ipynb`: Explores data downloaded from EPA's annual DMR data archive (monthly records across permit holders in NC.)



### D. EPA Safe Drinking Water Information System (SWDIS)

- **<u>Conclusion</u>**: Potential, but the data are obscurely layered across different servers. Possibly redundant with ECHO. 

  - Queries must be done iteratively. Server is often down.

- <u>Link</u>: https://www.epa.gov/enviro/topic-searches#water

- <u>Summary</u>: Data on violations and enforcement history since 1993 of the EPA's drinking water regulations. 

  Link to various portals:

  - Information Collection Rule (ICR) [No longer maintained]: 
    https://archive.epa.gov/enviro/html/icr/web/html/nc.html
  - **Permit Compliance System (PCS)** & **Integrated Compliance Information System (ICIS)** permits to discharge wastewater into rivers https://www.epa.gov/enviro/pcs-icis-search

- <u>Data</u>:

  - EPA: https://www3.epa.gov/enviro/facts/sdwis/search.html

    - https://oaspub.epa.gov/enviro/sdw_query_v3.get_list?&sys_status=active&pop_serv=&fac_state=NC
    - Then need to scrape sub-tables..

  - Link: https://pws.ncwater.org/PWSReports/pages/default.aspx

    - Enter Durham: 0332010

    https://www.pwss.enr.state.nc.us/NCDWW2/JSP/WaterSystemDetail.jsp?tinwsys_is_number=15263&tinwsys_st_code=NC&wsnumber=NC0332010

  - Turbidity: https://www.pwss.enr.state.nc.us/NCDWW2/JSP/turbsummary.jsp?tinwsys_is_number=15263&tinwsys_st_code=NC&begin_date=&end_date=&counter=0

  - Example call: [https://oaspub.epa.gov/enviro/sdw_query_v3.get_list?fac_city=&last_fac_name=&query_results=&fac_search=fac_beginning&sys_status=&fac_county=&fac_state=NC&page=1&pop_serv=&total_rows_found=&wsys_name=&wsys_id=](https://oaspub.epa.gov/enviro/sdw_query_v3.get_list?fac_city=&last_fac_name=&query_results=&fac_search=fac_beginning&sys_status=&fac_county=&fac_state=NC&page=1&pop_serv=&total_rows_found=&wsys_name=&wsys_id=)

- <u>Code</u>: 

  - `EPA/EPA_SDWIS.ipynb`: Query <https://www3.epa.gov/enviro/facts/sdwis/search.html> and scrapes results (in progress)



### E. EPA Permit Compliance System/Integrated Compliance Information System (PCS/ICIS)

* <u>**Conclusion**</u>: Gateway to much data, but clumsy click interface. Uses REST, but documentation is scant and structure is lacking. Results are formatted. Potential, but will work hard for the data, which may be replicated elsewhere (ECHO?)
* <u>Link</u>: https://www.epa.gov/enviro/pcs-icis-search
* <u>Summary</u>: A portal to search for specific facilities included in the EPA's Facility Registry Service ([FRS](https://www.epa.gov/frs))
* Data: 
  * Many search entry points: https://www.epa.gov/enviro/pcs-icis-search
  * Search for Durham Co.: 
  * Example call (one facility in Durham): https://iaspub.epa.gov/enviro/fii_query_dtl.disp_program_facility?pgm_sys_id_in=NCG140095&pgm_sys_acrnm_in=NPDES



### F. US Water Quality Portal

- **<u>Conclusion</u>**: ***Most promising!*** 
  - Repository of many datasets from multiple sources (EPA, USGS).
  - Web services and file shares provide ready access to data with excellent documentation
  - Need to compare what data are provided relative to state/local data portals. 

- <u>Link</u>: https://www.waterqualitydata.us/
- <u>Summary</u>:
  The Water Quality Portal (WQP) is a cooperative service sponsored by the United States Geological Survey (USGS), the Environmental Protection Agency (EPA), and the National Water Quality Monitoring Council (NWQMC). It serves data collected by over 400 state, federal, tribal, and local agencies: https://www.waterqualitydata.us/. The data include information on sites where data are gathered, physical/chemical monitoring data, and biological sample data. Complete metadata are available here: https://www.waterqualitydata.us/portal_userguide/
- <u>Data:</u>
  - Complete web services documentation: https://www.waterqualitydata.us/webservices_documentation/
- Code:
  * `USWQP/USWaterData-Scrape.ipynb` uses the WQP web service to pull station data for all sites in Durham Co. (N = 489). 
  *  `USWQP/USWaterData-Explore.ipynb` provides and example for ingesting and visualizing the US Water Quality Portal data scraped for Durham County. 

---

## II. State Data Sources

### A. NCDEQ 

#### 1. Water Resources Information, Storage, Analysis, and Retrieval System (WRISARS)

* **<u>Conclusion</u>**: Promising, but possibly difficult to scrape. Lots of good data centrally stored. 

- Need to develop scraping scripts; would be best to get access to data directly. NC Department of Environmental Quality

- Link: https://www.ncwater.org/?page=341; https://www.ncwater.org/wrisars/

- Summary: 
  A collaborative effort between the NC Division of Water Resources, the NC State Climate Office, the US Army Corps of Engineers, the NC DWR Ground Water Management Section, and the US Geological Survey (USGS). 

- Data: Includes values for current conditions (stream flow, groundwater levels, reservoir levels, weather/precip, and water conservation level). Query tools for stations, and HEC-DSS models. Historical data including link to water supply plans. 

  - Central Coastal Plain Capacity Use Area reports: https://www.ncwater.org/?page=49
  - Water withdrawal and transfer reports: http://www.ncwater.org/Permits_and_Registration/Water_Withdrawal_and_Transfer_Registration/report
    - Get facility ID and year and you can get a html page with withdrawal information (max daily by month) and discharge info. 
  - Water supply plans
  - https://www.ncwater.org/wrisars/StationList.php?ckState=on&state=NC&sortBy=site&submit=submit

  

#### 2. Water Withdrawal & Transfer Registration

* The NC DEQ maintains a database of registered water withdrawal facilities that includes the amounts of withdrawals, discharges, and transfers going back to 1999. See http://www.ncwater.org/Permits_and_Registration/Water_Withdrawal_and_Transfer_Registration/. 

* The notebook `NCDEQ\1-NCDEQ-Scraper.ipynb` generates a series of data file described below. Data are collected for the years spanning 2010 to 2017.

  ♦ **NCDEQWithdrawalMaster.csv** - Lists of each withdrawal facility (N=1194), listing its owner, name, whether the report is in draft or completed, and the site code:

  | Owner | Name | Status | Code |
  | ----- | ---- | ------ | ---- |
  |       |      |        |      |

  ♦ **NCDEQMonthlyVolumeData.csv** Average daily withdrawal and maximum day withdrawal by month in million gallons per day (MGD)

  | SiteID | Year | Registrant | Facility | Type | County | Subbasin | Month | # of DaysUsed | Average DailyWithdrawal (MGD) | Maximum DayWithdrawal (MGD) | # of DaysDischarged | Average DailyDischarge (MGD) | Maximum DayDischarge (MGD) | # of DaysTransferred | Average DailyTransfer (MGD) | Maximum   DayTransfer (MGD) |
  | ------ | ---- | ---------- | -------- | ---- | ------ | -------- | ----- | ------------- | ----------------------------- | --------------------------- | ------------------- | ---------------------------- | -------------------------- | -------------------- | --------------------------- | --------------------------- |
  |        |      |            |          |      |        |          |       |               |                               |                             |                     |                              |                            |                      |                             |                             |

  ♦ **NCDEQWithdrawalSourceData.csv** - Source Information - One row for each water withdrawal  source. 

  | SiteID | Year | FacilityType | County | Subbasin | Name | Type | AvgDaily | DaysUsed | Capacity_MGD |
  | ------ | ---- | ------------ | ------ | -------- | ---- | ---- | -------- | -------- | ------------ |
  |        |      |              |        |          |      |      |          |          |              |

  ♦ **NCDEQDischargeMethods.csv** - Average daily discharge and maximum day discharge by month in million gallons per day (MGD) 

  | SiteID | Year | FacilityType | County | Subbasin | Permit | Type | AvgDaily | DaysUsed | Capacity_MGD |
  | ------ | ---- | ------------ | ------ | -------- | ------ | ---- | -------- | -------- | ------------ |
  |        |      |              |        |          |        |      |          |          |              |

  ♦ **NCDEQTransferInfo.csv**

  | SiteID | Year | FacilityType | County | Subbasin | Description | SourceBasin | ReceivingBasin | Capacity |
  | ------ | ---- | ------------ | ------ | -------- | ----------- | ----------- | -------------- | -------- |
  |        |      |              |        |          |             |             |                |          |

* The notebook `NCDEQ\2-ExploreNCWithdrawalData.ipynb` provides and example of how the above files can be accessed and the data visualized.



#### 3. NC Water Resources Information Technology Branch

- Link: https://deq.nc.gov/about/divisions/water-resources/planning/information-technology-branch
  - Site map: https://www.ncwater.org/?page=619
- Summary:
  The Water Resources Information Technology Branch develops and maintains the Division's computer applications and databases.  The branch is comprised of three units:  BIMS support, Public Water Supply support, and Water Planning support. Links to many applications storing/hosting water data. 
- **<u>Conclusion</u>**: Useful pointer to *other* data stores



### B. Public Water System Supervision/Drinking Water Watch

Link: https://deq.nc.gov/about/divisions/water-resources/drinking-water/sampling-status-and-drinking-water-watch

Public Water System Supervision 



### C. NC Water Supply Plans

The notebook `NCDEQ\X-WaterSupplyPlanScraper.ipynb` is a work in progress to scrape the annual water supply plan data from the Department of Water Resources portal:<br> https://www.ncwater.org/Water_Supply_Planning/Local_Water_Supply_Plan/

---

## III. Local Data Sources

### Charlotte Water Data

- Charlotte Water hosts spatial and tabular data on its ArcGIS server: <https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public> 
- These data include the groupings listed below. and the links provide additional information and full metadata. 
  - [Public/CLTW_DrinkingWaterBoundaries](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterBoundaries/MapServer) 
  - [Public/CLTW_DrinkingWaterCHLORINE2](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterCHLORINE2/MapServer) 
  - [Public/CLTW_DrinkingWaterDBP](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterDBP/MapServer) 
  - [Public/CLTW_DrinkingWaterINORGANIC2](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterINORGANIC2/MapServer) 
  - [Public/CLTW_DrinkingWaterLEAD5](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterLEAD5/MapServer) 
  - [Public/CLTW_DrinkingWaterMICRO](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterMICRO/MapServer) 
  - [Public/CLTW_DrinkingWaterORGANICS](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterORGANICS/MapServer) 
  - [Public/CLTW_DrinkingWaterQuality_2017](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterQuality_2017/MapServer) 
  - [Public/CLTW_DrinkingWaterQualityReport](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterQualityReport/MapServer) 
  - [Public/CLTW_DrinkingWaterSECONDARY](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterSECONDARY/MapServer) 
  - [Public/CLTW_MeckCountyWQ](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_MeckCountyWQ/MapServer) 
  - [Public/CLTW_SchoolsLCR_PlanningUPDATED](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_SchoolsLCR_PlanningUPDATED/MapServer) 
  - [Public/CLTW_VFD_Hydrants](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_VFD_Hydrants/MapServer) 
  - [Public/CLTW_WaterQualityUpdates_2018](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_WaterQualityUpdates_2018/MapServer) 
- The notebook `1-CharlotteScraper.ipynb` pulls all these data into CSV formatted files. Often two files are generated: one describing the sites where data were collected, and the other the data pulled from each site tagged with a timestamp.
- The notebook `2-CharlotteDataExplorer.ipynb` is an example of how these data, extracted from the CSV files, can be analyzed and visualized. 

------

## 