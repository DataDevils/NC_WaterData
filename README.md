# NC_WaterData
Tools to find and extract NC Water Data

---

## Charlotte Water Data

* Charlotte Water hosts spatial and tabular data on its ArcGIS server: <https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public> 
* These data include the groupings listed below. and the links provide additional information and full metadata. 
  * [Public/CLTW_DrinkingWaterBoundaries](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterBoundaries/MapServer) 
  * [Public/CLTW_DrinkingWaterCHLORINE2](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterCHLORINE2/MapServer) 
  * [Public/CLTW_DrinkingWaterDBP](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterDBP/MapServer) 
  * [Public/CLTW_DrinkingWaterINORGANIC2](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterINORGANIC2/MapServer) 
  * [Public/CLTW_DrinkingWaterLEAD5](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterLEAD5/MapServer) 
  * [Public/CLTW_DrinkingWaterMICRO](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterMICRO/MapServer) 
  * [Public/CLTW_DrinkingWaterORGANICS](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterORGANICS/MapServer) 
  * [Public/CLTW_DrinkingWaterQuality_2017](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterQuality_2017/MapServer) 
  * [Public/CLTW_DrinkingWaterQualityReport](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterQualityReport/MapServer) 
  * [Public/CLTW_DrinkingWaterSECONDARY](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_DrinkingWaterSECONDARY/MapServer) 
  * [Public/CLTW_MeckCountyWQ](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_MeckCountyWQ/MapServer) 
  * [Public/CLTW_SchoolsLCR_PlanningUPDATED](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_SchoolsLCR_PlanningUPDATED/MapServer) 
  * [Public/CLTW_VFD_Hydrants](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_VFD_Hydrants/MapServer) 
  * [Public/CLTW_WaterQualityUpdates_2018](https://cltwmaps.ci.charlotte.nc.us/arcgis/rest/services/Public/CLTW_WaterQualityUpdates_2018/MapServer) 
* The notebook `1-CharlotteScraper.ipynb` pulls all these data into CSV formatted files. Often two files are generated: one describing the sites where data were collected, and the other the data pulled from each site tagged with a timestamp.
* The notebook `2-CharlotteDataExplorer.ipynb` is an example of how these data, extracted from the CSV files, can be analyzed and visualized. 

---

## NC Department of Environmental Quality

### Water Withdrawal & Transfer Registration

* The NCD DEQ maintains a database of registered water withdrawal facilities that includes the amounts of withdrawals, discharges, and transfers going back to 1999. See http://www.ncwater.org/Permits_and_Registration/Water_Withdrawal_and_Transfer_Registration/. 

* The notebook `NCDEQ\1-NCDEQ-Scraper.ipynb` generates a series of data file described below:

  `Charlotte\WithdrawalMaster.csv`: Lists of each withdrawal facility (N=1194), listing its owner, name, whether the report is in draft or completed, and the site code:

  | Owner | Name | Status | Code |
  | ----- | ---- | ------ | ---- |
  |       |      |        |      |

  `MonthlyVolumeData.csv` Average daily withdrawal and maximum day withdrawal by month in million gallons per day (MGD)

  | SiteID | Year | Registrant | Facility | Type | County | Subbasin | Month | # of DaysUsed | Average DailyWithdrawal (MGD) | Maximum DayWithdrawal (MGD) | # of DaysDischarged | Average DailyDischarge (MGD) | Maximum DayDischarge (MGD) | # of DaysTransferred | Average DailyTransfer (MGD) | Maximum   DayTransfer (MGD) |
  | ------ | ---- | ---------- | -------- | ---- | ------ | -------- | ----- | ------------- | ----------------------------- | --------------------------- | ------------------- | ---------------------------- | -------------------------- | -------------------- | --------------------------- | --------------------------- |
  |        |      |            |          |      |        |          |       |               |                               |                             |                     |                              |                            |                      |                             |                             |

  `WithdrawalSourceData.csv`  Source Information - One row for each water withdrawal  source. 

  | SiteID | Year | FacilityType | County | Subbasin | Name | Type | AvgDaily | DaysUsed | Capacity_MGD |
  | ------ | ---- | ------------ | ------ | -------- | ---- | ---- | -------- | -------- | ------------ |
  |        |      |              |        |          |      |      |          |          |              |

  `DischargeMethods.csv`Average daily discharge and maximum day discharge by month in million gallons per day (MGD) 

  | SiteID | Year | FacilityType | County | Subbasin | Permit | Type | AvgDaily | DaysUsed | Capacity_MGD |
  | ------ | ---- | ------------ | ------ | -------- | ------ | ---- | -------- | -------- | ------------ |
  |        |      |              |        |          |        |      |          |          |              |

  `TransferInfo.csv`

  | SiteID | Year | FacilityType | County | Subbasin | Description | SourceBasin | ReceivingBasin | Capacity |
  | ------ | ---- | ------------ | ------ | -------- | ----------- | ----------- | -------------- | -------- |
  |        |      |              |        |          |             |             |                |          |

* The notebook ``NCDEQ\2-ExploreNCWithdrawalData.ipynb` provides and example of how the above files can be accessed and visualize.