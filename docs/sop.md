---
layout: page
title: Standard Operating Procedure
subtitle: Small Sewer Hydraulic Analysis
---

<style type="text/css">
    ol { list-style-type: lower-alpha; }
    ol ol { list-style-type: lower-roman;}
    ol {margin: 0em 0;}
    ol.normal-list{list-style-type:decimal;}
    span { white-space:nowrap; }
</style>

![SSHA Example Screenshot]({{site.baseurl}}/public/img/ssha-example-study-area.png)

Updated July 7, 2017

This SOP is intended to guide the completion of small sewer hydraulic analysis (SSHA) to determine the hydraulic capacity and peak design runoff from contributing drainage areas for small sewers. This procedure makes use of GIS script tools applied within an ArcGIS basemap and a local geodatabase.

A decription of all related files is located [here]({{site.baseurl}}/files). The SSHA procedure is outlined below:
1. [Review Engineering Records](#1-review-engineering-records)
2. [Delineate Drainage Areas](#2-delineate-drainage-areas)
3. [Associate Sewers to Drainage Areas](#3-associate-sewers-to-drainage-areas)
4. [Hydraulic and Hydrologic Calculations](#4-perform-hydraulic-and-hydrologic-calculations)
5. [Resolve Data Gaps](#5-resolve-data-gaps)
6. [Assess Neighboring Sewers](#6-assess-neighboring-sewers)
7. [Set Up Data Driven Pages](#7-set-up-data-driven-pages)
8. [Qaulity Control Procedure](#8-qaulity-control-procedure)
9. [Export the Study](#9-export-the-study)
{: .normal-list}

<hr>

## 1. Review Engineering Records
Because the SSHA tool relies on the accuracy of electronic data sources, existing engineering records should be reviewed to confirm accuracy.
1. For each study area, open and review return plans. All slope information from the [DataConv]({{site.baseurl}}/definitions/#dataconv) database must be confirmed against sewer return plans for accuracy.
2. Review the hardcopy sewer studies (if available) in the filing cabinets long the southermost wall, west of the mezzanine in the Design Unit on the 2nd Floor. Sewer studies are organized by outfall number, then in alphabetical order by street.

## 2. Delineate Drainage Areas
1. In ArcMap, navigate to the study area based on the street connection point provided.
2. Identify the study sewer and the branches that contribute to it (if any):
    1. Ensure the study sewer is within the size limit for the SSHA process. This tool is to be used on conduits that are no larger than 36-inch diameter or equivalent size, (up to a 43 x 34-inch egg or a 45 x 27-inch box sewer). Refer to [this table]({{site.baseurl}}/equivalent-sewer-sizes) for all equivalent pipe sizes applicable to SSHA.
    2. The Trace Upstream tool may be used to identify contributing branches. To use this, ensure the _Utility Network Analyst_ tool is added to the toolbar and set on the Data Conversion Waste Water Network or Data Conversion Storm Water Network, depending on the network being analyzed.
    3. Exclude Vent Pipes from the analysis by checking Waste Water Vent Pipes or Storm Water Vent Pipes in the _Disable Layers_ list within the _Analysis_ drop down menu in the _Utility Network Analyst_ toolbar.
    4. Place an Edge Flag Tool near the downstream end of the study pipe, select _Trace Upstream_, and click the _Solve_ button. The contributing branches will be highlighted in red. Review the identified upstream pipes and check for errors.
3. The contributing drainage area should encompass all of the contributing branches. To delineate the new drainage area, first right click the __Drainage Areas__ layer, select _Edit Features_, then _Start Editing_. Next, click on _Create Features_ in the Editor Toolbar and select the __Drainage Areas__ layer.
4. Draft the new drainage area based on the following rules of thumb:
    1. Blocks that are adjacent to contributing pipe reaches at the edge of the drainage area should be bisected.
    2. Block ends should be cut at the parcel vertices on either side of the street.
    3. Drainage areas should be cut at approximate 45 degree angles from terminal block parcel vertices and extended until approximately the block midpoint. As a rule of thumb, drainage area boundaries should evenly split the space between the sewers.
    ![SSHA Example Shed Delineation]({{site.baseurl}}/public/img/ssha-delineation-angles-example.png)
    As in the example above, a drainage boundary between sewers that are oriented with a 50 degree angle between them should bisect the sewers at 25 degrees from each sewer. Typically, surface features, topography, and parcel boundaries should not influence the drainage area delineation. In areas with low drainage density and/or steep slopes, more traditional drainage area delineation should be used.  

5. Compare your drafted drainage area to the __NewSubSheds__ layer as secondary measure to ensure that vital portions of the drainage area are not missed. When applicable hardcopy sewer studies are found, ensure drainage area delineation is consistent.
6. Open the __Drainage Areas__ attribute table and manually enter the [Project_ID]({{site.baseurl}}/definitions/#project_id), [StudyArea_ID]({{site.baseurl}}/definitions/#studyarea_id) and [Connection Point]({{site.baseurl}}/definitions/#connection-point) attributes for the new drainage area. For example, data entered for two drainage areas with a Project_ID (or work order number) of 40000 should look like this:

      | Project_ID | StudyArea_ID | ConnectionPoint |
      | --- | --- | --- |
      | 40000 | 40000_01 | 11 St from Market to Filbert |
      | 40000 | 40000_02 | 11 St from Filbert to Arch |


7. Repeat steps _a_ through _f_ for each study area.
8. In the Editor Toolbar dropdown menu, select _Save Edits_, then _Stop Editing_.

## 3. Associate Sewers to Drainage Areas
Add the study sewers (and their contributing sewers) from the __Waste Water Gravity Mains__ layer to the __Studied Sewers__ Layer.
![Associate Sewers Example Screenshot]({{site.baseurl}}/public/img/associate-sewers-example.jpg)
1. Associate study sewers to the Drainage Areas.
    1. Navigate to the __Small_Sewer_Calcs__ toolbox within the ArcToolbox window.
    2. Select the _Associate Sewers to DAs_ tool.
    3. Input the command prompt options:
        * [Project_ID]({{site.baseurl}}/definitions/#project_id) – Project_ID from the __Drainage Areas__ attribute table
        * From Sewers Layer – __Waste Water Gravity Mains__ or __Storm Water Gravity Mains__
        * Study Sewers Layer – __StudiedSewers__
        * Drainage Area Layer – __Drainage Areas__
    4. Select _OK_. The tool will populate the StudiedSewers layer with the sewers intersecting the target drainage area.
2. Identify the Study Sewer and Time of Concentration (TC) path for each study area.
    1. Right-click the __StudiedSewers__ layer. Select _Edit Features_, then _Start Editing_.
    2. Open the __StudiedSewers__ attribute table.
    3. Select each of the pipe segments that are part of the study sewer. Change the `StudySewer` field to `Y` for each of these segments. Study sewers should follow the following conventions:
        * Not exceed the length of one typical city block (approximately 400 feet). Where sewer length exceeds approximately 400 feet within a city block, split the study area near the block midpoint. If possible, make the division at manholes.
        * Not extend across a change in sewer size or slope
        * Not extend beyond a junction of two or more sewers
    4. Identify the TC path for the drainage area.
        * This can be completed by identifying the longest pipe reach within the drainage area with the Measure tool.
        * Select each of the pipe segments making up the TC path. Change the `TC_Path` field to `Y` for each of these segments.
    5. Repeat steps _iii_ and _iv_ for each study area.
3. In the _Editor Toolbar_ dropdown menu, select _Save Edits_, then _Stop Editing_.

## 4. Perform Hydraulic and Hydrologic Calculations
![HH Calculations Example Screenshot]({{site.baseurl}}/public/img/hhcalcs-example.jpg)
1. Navigate to the __Small_Sewer_Calcs__ toolbox within the ArcToolbox window.
2. Select the _Run H&H Calcs_ tool.
3. Input the command prompt options. Enter the Study Area ID to perform calculations for one study sewer or enter the Project ID to perform batch calculations on the entire project.

## 5. Resolve Data Gaps
![Data Gap Example Screenshot]({{site.baseurl}}/public/img/example-data-gap.png)
The _Run H&H Calcs_ tool will tag sewers that have missing or `<null>` slope values and highlight them with red symbology, as shown above. The process below outlines how this missing data should be resolved.
1. Right-click the __StudiedSewers__ layer. Select _Edit Features_, then _Start Editing_.
2. Open the __StudiedSewers__ attribute table.
3. Open the _Select By Attributes_ tool and paste the following SQL query into the `WHERE` text box:
```SQL
Project_ID = [project_id] AND (Tag = 'SS_MIN_SLOPE' OR Tag = 'TC_MIN_SLOPE' OR Tag = 'TC_UNDEFINED')
```
Replace `[project_id]` with the appropriate Project_ID and click _Apply_ to run the query. This query will select all important sewers within the Project_ID that are missing data.
4. For each sewer identified, attempt to resolve the missing slope value.  Step through the following list of methods (in order of preference) until a slope value is determined:  
    * **From Design/Return Plans**
        1. For each study sewer tagged in this category, navigate to the drawing using the URL in the `STICKERLINK` field. Do this by copying and pasting the URL into the Internet Explorer browser.
        2. Determine if the missing slope values are listed in the drawing for the study sewer. Note that in some instances, the drawing for a neighboring sewer will contain the missing slope value that is needed.  
    * **From Adjacent Manhole Inverts**
        1. Navigate to the upstream and downstream manholes of the sewer in question and see if invert elevations exist in the attribute table for these manholes.
        2. Use the invert elevations and the length of the pipe to calculate slope.  
    * **Based on Ground Surface Terrain**
        1. Use the __GIS_GSG.Contour_2015_1ft__ layer to view contours for the area to calculate the slope.
        2. Input the parameters into the Slope Value Verification Tool and navigate to the Terrain Slope section. Determine if the new velocity value falls within the design velocity criteria. If it does, the slope may be used.  
    * **Based on Minimum Design Velocity**  
        1. Input parameters into the Slope Value Verification tool and navigate to the minimum design velocity section.
5. Input this resolved slope value as an attribute in the `Slope_Used` field for the study sewer.
6. In the Editor Toolbar dropdown menu, select _Save Edits_, then _Stop Editing_.
7. Repeat step 4: [Perform Hydraulic and Hydrologic Calculations](#4-perform-hydraulic-and-hydrologic-calculations)  
<br>
***Note***: Slope data is often missing in short sewer segments at the end of city blocks, as shown by the red-highlighted sewer in the figure below.
![Data Gap Corner Screenshot]({{site.baseurl}}/public/img/example-typical-data-gap-crop.png)
These sewers can be assumed to have slope equal to the average slope in the upstream block of sewer so long as they are not limiting the block capacity.

## 6. Assess Neighboring Sewers
Determine whether sewers adjacent to the study sewer (upstream and downstream) are within the scope of the SSHA process.
1. If the study sewer in question is undersized, perform hydraulic and hydrologic calculations on upstream sewers until capacity calculations show the sewers are sized properly.
2. If the study sewer in question has neighboring downstream sewers that are less than or equal to 36-inch diameter (or equivalent size), perform hydraulic and hydrologic calculations.
3. Note that any study sewer with upstream flow splits is outside the scope of this analysis and should not be performed.

## 7. Set Up Data Driven Pages
Data Driven Pages is a ArcMap feature that facilitates the output reporting from the SSHA tool and easy navigation between study areas within a given Project_ID. Here, Data Driven Pages make use of a index layer created for each Project_ID when the _Run H&H Calcs_ tool is run. Each drainage area (DA) index layer shares the name of the Project_ID, prefixed with `DA_`. For example, a Project_ID of 40000 would be assigned a index layer of __DA_40000__.
1. In the __Drainage Areas__ layer group in the ArcMap table of contents, right-click on the __DA Indicies__ sub-group and select _Add Data_. Navigate to the __StudyAreaIndices__ feature dataset in the __Small_Sewer_Capacity__ geodatabase and add the appropriate index layer.       
2. Switch to __Layout View__.
3. Click on the Data Driven Pages setup icon: ![Data Driven Pages Icon]({{site.baseurl}}/public/img/ddp_icon.png){:style="max-height:1em;display:inline;margin:0;"} to open the Set Up __Data Driven Pages__ dialog. In the __Layer__ dropdown menu, enter the appropriate DA index layer. Update the remaining fields in the dialog as shown below:
![Data Driven Dialog]({{site.baseurl}}/public/img/ddp-dialog.jpg)
4. Set-up the display expression.
    1. In the DA index layer properties, navigate to the __Display__ tab and click on _Expression..._
    1. Click the _Load..._ button
    2. Navigate to the `Scripts/arcmap_expressions` folder and select StudyAreaSummary3.lxp
    3. Click _Open_
    4. Click _OK_

## 8. Quality Control Procedure
1. Confirm that all data gaps identified in [Section 5](#5-resolve-data-gaps) are resolved. Ensure that no sewers are hightlighted with red symbology, taking care to review all small sewers at the ends of city blocks.
2. Review return plans and compare to SSHA results. Electronic plans can be accessed via [ERV](http://170.115.80.42/ERV2_Basic/ERV2.aspx) or by the navigating to the `STICKERLINK`. Confirm that geometry and slope data is consistent with the original engineering records.
3. Review the hardcopy sewer studies (if available). When applicable hardcopy sewer studies are found:
    1. Ensure drainage area delineation is consistent.
    2. Ensure runoff coefficient is consistent.
4. Confirm that runoff coefficient is consistent with land cover characteristics
    1. For dense areas of the city, assume C=0.85 (default)
    2. For less dense residential areas (e.g. Manayunk, Northeast, etc.), assume C=0.75
3. Confirm that the _Run H&H Calcs_ tool is executed after all corrections to data are made.
5. Review results and identify any irregularities. Confirm that calculated capacity is within range of typical values. [This chart](https://plot.ly/~aerispaha/57.embed) shows possible capacity values for a range of sewer diameters within the design velocity range (2.5 to 15fps):
[<img src="{{site.baseurl}}/public/img/capacity-within-design-velocity-plot.png">](https://plot.ly/~aerispaha/57.embed)

## 9. Export the Study
1. In the _File_ menu,  click on _Export Map..._
2. Navigate to the corresponding project folder for the current work order number and go to `01 Analysis\03 Sewer Analysis`
3. Name the file according to the Project ID prefixed with `SSHA_` (e.g. SSHA_40000.pdf)
4. Choose the _PDF_ file type.
5. Under _Options_ choose the _Pages_ tab. Choose _All Pages_
6. Click _Save_ to export the file.
