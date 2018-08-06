# `NC_WaterData
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

  

---

## US Water Quality Portal

The Water Quality Portal (WQP) is a cooperative service sponsored by the United States Geological Survey (USGS), the Environmental Protection Agency (EPA), and the National Water Quality Monitoring Council (NWQMC). It serves data collected by over 400 state, federal, tribal, and local agencies: https://www.waterqualitydata.us/

The notebook `US/USWaterData-Scrape.ipynb` uses the WQP web service to pull station data for all sites in Durham Co. (N = 489). The attributes include the following: 

| OrganizationIdentifier | OrganizationFormalName | MonitoringLocationIdentifier | MonitoringLocationName | MonitoringLocationTypeName | MonitoringLocationDescriptionText | HUCEightDigitCode | DrainageAreaMeasure/MeasureValue | DrainageAreaMeasure/MeasureUnitCode | ContributingDrainageAreaMeasure/MeasureValue | ContributingDrainageAreaMeasure/MeasureUnitCode | LatitudeMeasure | LongitudeMeasure | SourceMapScaleNumeric | HorizontalAccuracyMeasure/MeasureValue | HorizontalAccuracyMeasure/MeasureUnitCode | HorizontalCollectionMethodName | HorizontalCoordinateReferenceSystemDatumName | VerticalMeasure/MeasureValue | VerticalMeasure/MeasureUnitCode | VerticalAccuracyMeasure/MeasureValue | VerticalAccuracyMeasure/MeasureUnitCode | VerticalCollectionMethodName | VerticalCoordinateReferenceSystemDatumName | CountryCode | StateCode | CountyCode | AquiferName | FormationTypeText | AquiferTypeName | ConstructionDateText | WellDepthMeasure/MeasureValue | WellDepthMeasure/MeasureUnitCode | WellHoleDepthMeasure/MeasureValue | WellHoleDepthMeasure/MeasureUnitCode | ProviderName |
| ---------------------- | ---------------------- | ---------------------------- | ---------------------- | -------------------------- | --------------------------------- | ----------------- | -------------------------------- | ----------------------------------- | -------------------------------------------- | ----------------------------------------------- | --------------- | ---------------- | --------------------- | -------------------------------------- | ----------------------------------------- | ------------------------------ | -------------------------------------------- | ---------------------------- | ------------------------------- | ------------------------------------ | --------------------------------------- | ---------------------------- | ------------------------------------------ | ----------- | --------- | ---------- | ----------- | ----------------- | --------------- | -------------------- | ----------------------------- | -------------------------------- | --------------------------------- | ------------------------------------ | ------------ |
|                        |                        |                              |                        |                            |                                   |                   |                                  |                                     |                                              |                                                 |                 |                  |                       |                                        |                                           |                                |                                              |                              |                                 |                                      |                                         |                              |                                            |             |           |            |             |                   |                 |                      |                               |                                  |                                   |                                      |              |

♦ **US/DurhamResults.csv**:

| OrganizationIdentifier | OrganizationFormalName                                       | ActivityIdentifier             | ActivityTypeCode | ActivityMediaName | ActivityMediaSubdivisionName | ActivityStartDate | ActivityStartTime/Time | ActivityStartTime/TimeZoneCode | ActivityEndDate | ActivityEndTime/Time | ActivityEndTime/TimeZoneCode | ActivityDepthHeightMeasure/MeasureValue | ActivityDepthHeightMeasure/MeasureUnitCode | ActivityDepthAltitudeReferencePointText | ActivityTopDepthHeightMeasure/MeasureValue | ActivityTopDepthHeightMeasure/MeasureUnitCode | ActivityBottomDepthHeightMeasure/MeasureValue | ActivityBottomDepthHeightMeasure/MeasureUnitCode | ProjectIdentifier | ActivityConductingOrganizationText                 | MonitoringLocationIdentifier | ActivityCommentText                                   | SampleAquifer | HydrologicCondition | HydrologicEvent | SampleCollectionMethod/MethodIdentifier | SampleCollectionMethod/MethodIdentifierContext | SampleCollectionMethod/MethodName | SampleCollectionEquipmentName | ResultDetectionConditionText | CharacteristicName | ResultSampleFractionText | ResultMeasureValue | ResultMeasure/MeasureUnitCode | MeasureQualifierCode | ResultStatusIdentifier | StatisticalBaseCode | ResultValueTypeName | ResultWeightBasisText | ResultTimeBasisText | ResultTemperatureBasisText | ResultParticleSizeBasisText | PrecisionValue | ResultCommentText | USGSPCode | ResultDepthHeightMeasure/MeasureValue | ResultDepthHeightMeasure/MeasureUnitCode | ResultDepthAltitudeReferencePointText | SubjectTaxonomicName | SampleTissueAnatomyName | ResultAnalyticalMethod/MethodIdentifier | ResultAnalyticalMethod/MethodIdentifierContext | ResultAnalyticalMethod/MethodName | MethodDescriptionText | LaboratoryName | AnalysisStartDate | ResultLaboratoryCommentText | DetectionQuantitationLimitTypeName | DetectionQuantitationLimitMeasure/MeasureValue | DetectionQuantitationLimitMeasure/MeasureUnitCode | PreparationStartDate | ProviderName |
| ---------------------- | ------------------------------------------------------------ | ------------------------------ | ---------------- | ----------------- | ---------------------------- | ----------------- | ---------------------- | ------------------------------ | --------------- | -------------------- | ---------------------------- | --------------------------------------- | ------------------------------------------ | --------------------------------------- | ------------------------------------------ | --------------------------------------------- | --------------------------------------------- | ------------------------------------------------ | ----------------- | -------------------------------------------------- | ---------------------------- | ----------------------------------------------------- | ------------- | ------------------- | --------------- | --------------------------------------- | ---------------------------------------------- | --------------------------------- | ----------------------------- | ---------------------------- | ------------------ | ------------------------ | ------------------ | ----------------------------- | -------------------- | ---------------------- | ------------------- | ------------------- | --------------------- | ------------------- | -------------------------- | --------------------------- | -------------- | ----------------- | --------- | ------------------------------------- | ---------------------------------------- | ------------------------------------- | -------------------- | ----------------------- | --------------------------------------- | ---------------------------------------------- | --------------------------------- | --------------------- | -------------- | ----------------- | --------------------------- | ---------------------------------- | ---------------------------------------------- | ------------------------------------------------- | -------------------- | ------------ |
| 21NC03WQ               | North Carolina Department of Environmental Resources NCDENR-DWQ WQX | 21NC03WQ-NCLAKESMON22999-2011I | Field Msr/Obs    | Water             |                              | 8/9/2011          |                        |                                |                 |                      |                              | 1                                       | m                                          |                                         |                                            |                                               |                                               |                                                  | NCLAKESMON        | NC Lakes Monitoring - NC Division of Water Quality | 21NC03WQ-LLC01               | Dissolved Oxygen not recorded due to   meter problems |               |                     |                 | Temperature, water                      | 30.6                                           | deg C                             |                               | Final                        |                    | Actual                   |                    |                               |                      |                        |                     |                     |                       |                     |                            |                             |                |                   | WSSSOP    | 21NC03WQ                              | WSSSOP                                   |                                       |                      |                         |                                         |                                                |                                   |                       |                | STORET            |                             |                                    |                                                |                                                   |                      |              |

 `US/USWaterData-Explore.ipynb` pulls all available data for Durham Co. 

