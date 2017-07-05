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

Updated June 30, 2017


This SOP is intended to guide the completion of small sewer hydraulic analyses
(SSHA) to determine the hydraulic capacity and peak design runoff from
contributing drainage areas for small sewers. This procedure makes us of GIS
script tools applied within an ArcGIS basemap and geodatabase on the LAMP drive
at this server location:
> \\\PWDHQR2\Data\Planning &amp; Research\Linear Asset Management Program\Water Sewer Projects Initiated\03 GIS Data\Hydraulic Studies

#### Relevant Files

| Filename | Description |
| ---- | ---- |
| Small Sewer Capacity.mxd | ArcMap file  |
| Small_Sewer_Capacity.gdb | Geodatabase containing data results of small sewer analysis |  
| Mannings Slope Calcs.xlsm | Excel file for slope verification |

The SSHA procedure is outlined below:

1. [Delineate Drainage Areas](#1-delineate-drainage-areas)
2. [Associate Sewers to Drainage Areas](#2-associate-sewers-to-drainage-areas)
3. [Hydraulic and Hydrologic Calculations](#3-perform-hydraulic-and-hydrologic-calculations)
4. [Resolve Data Gaps](#4-resolve-data-gaps)
5. [Assess Neighboring Sewers](#5-assess-neighboring-sewers)
6. [Set Up Data Driven Pages](#6-set-up-data-driven-pages)
7. [Qaulity Control Procedure](#7-qaulity-control-procedure)
8. [Export the Study](#8-export-the-study)
{: .normal-list}

<hr>

## 1. Delineate Drainage Areas
1. Navigate to the study area based on the street connection point provided.
2. Identify the study sewer and the branches that contribute to it (if any).
    1. Ensure the study sewer is within the size limit for the SSHA process. This tool is designed to be used on conduits that are no larger than 36&quot; diameter or equivalent size.
    2. The Trace Upstream tool may be used to identify contributing branches. To use this, ensure the Utility Network Analyst tool is added to the toolbar and set on the &quot;Data Conversion Waste Water Network&quot; or &quot;Data Conversion Storm Water Network&quot;, depending on the network being analyzed.
    3. Confirm that Vent Pipes are disabled from this analysis (these tend to erroneously extend the upstream trace). To do this, right-click the &quot;Analysis&quot; drop down menu, &quot;Disable Layers&quot; and check &quot;Storm Water Vent Pipes&quot; or &quot;Waste Water Vent Pipes&quot;.
    4. Place an Edge Flag Tool near the downstream end of the study pipe, select &quot;Trace Upstream&quot;, and click the &quot;Solve&quot; button. The contributing branches will be highlighted in red. Review the identified upstream pipes and check for errors.
3. The contributing drainage area should encompass all of the contributing branches. To delineate the new drainage area, first right click the &quot;Drainage Areas&quot; layer and select &quot;Edit Features&quot;&gt;&quot;Start Editing&quot;. Then, click on &quot;Create Features&quot; in the Editor Toolbar and select the &quot;Drainage Areas&quot; layer.
4. Draft the new drainage area based on the following rules of thumb:
    1. Blocks that are adjacent to contributing pipe reaches at the edge of the drainage area should be bisected.
    2. Block ends should be cut at the parcel vertices on either side of the street.
    3. Drainage areas should be cut at approximate 45 degree angles from terminal block parcel vertices and extended until approximately the block midpoint. A sample drafted drainage area is provided below for reference.

      > As a rule of thumb, drainage area boundaries should evenly split the space between the sewers. For example, a drainage boundary between sewers that are oriented with a 60 degree angle between them should bisect the sewers at 30 degrees from each sewer. Surface features and parcel boundaries should not influence the drainage area delineation.

5. Compare your drafted drainage area to the &quot;NewSubSheds&quot; layer as secondary measure to ensure that vital portions of the drainage area are not missed.
6. Open the &quot;Drainage Areas&quot; attribute table and manually enter the [Project\_ID]({{site.baseurl}}/#project_id), StudyArea\_ID and ConnectionPoint attributes for the new drainage area. For example, data entered for two drainage areas with a [Project\_ID]({{site.baseurl}}/#project_id) (or work order number) of 40000 should look like this:

      | Project\_ID | StudyArea\_ID | ConnectionPoint |
      | --- | --- | --- |
      | 40000 | 40000\_01 | 11 St from Market to Filbert |
      | 40000 | 40000\_02 | 11 St from Filbert to Arch |


7. Repeat steps &quot;a&quot; through &quot;f&quot; for each study area.
8. In the Editor Toolbar dropdown menu, select &quot;Save Edits&quot;, then &quot;Stop Editing&quot;.

## 2. Associate Sewers to Drainage Areas
Add the study sewers (and their contributing sewers) from the &quot;Waste Water Gravity Mains&quot; layer to the &quot;Studied Sewers&quot; Layer.
![Associate Sewers Example Screenshot]({{site.baseurl}}/public/img/associate-sewers-example.jpg)
1. Associate study sewers to the Drainage Areas.
    1. Navigate to the Small\_Sewer\_Calcs Toolbox within the ArcToolbox.
    2. Select the &quot;Associate Sewers to DAs&quot; tool.
    3. Input the command prompt options:
        * [Project\_ID]({{site.baseurl}}/#project_id) – Project\_ID from the &quot;Drainage Areas&quot; attribute table
        * From Sewers Layer – &quot;Waste Water Gravity Mains&quot; or &quot;Storm Water Gravity Mains&quot;
        * Study Sewers Layer – &quot;StudiedSewers&quot;
        * Drainage Area Layer – &quot;Drainage Areas&quot;
    4. Select &quot;OK&quot;. The tool will populate the StudiedSewers layer with the sewers intersecting the target drainage area.
2. Identify the Study Sewer and Time of Concentration (TC) path for each study area.
    1. Right-click the &quot;StudiedSewers&quot; layer. Select &quot;Edit Features&quot;, then &quot;Start Editing&quot;.
    2. Open the &quot;StudiedSewers&quot; attribute table.
    3. Select each of the pipe segments that are part of the study sewer. Change the &quot;StudySewer&quot; field to &quot;Y&quot; for each of these segments. Study sewers should follow the following conventions:
        * Not exceed the length of one typical city block (approximately 450 feet). Where sewer length exceeds approximately 450 feet within a city block, split the study area near the block midpoint. If possible, make the division at manholes.
        * Not extend across a change in sewer size or slope
        * Not extend beyond a junction of two or more sewers
    4. Identify the TC path for the drainage area.
        * This can be completed by identifying the longest pipe reach within the drainage area with the Measure tool.
        * Select each of the pipe segments making up the TC path. Change the &quot;TC\_Path&quot; field to &quot;Y&quot; for each of these segments.
    5. Repeat steps &quot;iii&quot; and &quot;iv&quot; for each study area.
3. In the Editor Toolbar dropdown menu, select &quot;Save Edits&quot;, then &quot;Stop Editing&quot;.

## 3. Perform Hydraulic and Hydrologic Calculations
![HH Calculations Example Screenshot]({{site.baseurl}}/public/img/hhcalcs-example.jpg)
1. Navigate to the Small\_Sewer\_Calcs Toolbox within the ArcToolbox
2. Select the &quot;Run H&amp;H Calcs&quot; tool
3. Input the command prompt options. Enter the Study Area ID to perform calculations for one study sewer or enter the Project ID to perform batch calculations on the entire project.

## 4. Resolve Data Gaps
The &quot;Run H&amp;H Calcs&quot; will tag sewers that have missing or &quot;&lt;Null&gt;&quot; slope values. To continue with the hydraulic calculations, a minimum slope of 0.01% is assumed. Review the drawings for these sewers and determine whether these slope values can be resolved.
1. Right-click the &quot;StudiedSewers&quot; layer. Select &quot;Edit Features&quot;, then &quot;Start Editing&quot;.
2. Open the &quot;StudiedSewers&quot; attribute table.
3. Open the &quot;Select By Attributes&quot; tool.
    1. Copy and paste the following SQL query into the WHERE CLAUSE text box:
        * Project\_ID = \*\*\* AND (Tag = &#39;SS\_MIN\_SLOPE&#39; OR Tag = &#39;TC\_MIN\_SLOPE&#39; OR Tag = &#39;TC\_UNDEFINED&#39;)
        * Note** : &quot;\*\*\*&quot; represents current Project ID.
    2. Click &quot;Apply&quot;.
4. Attempt to resolve the missing slope value using the following process, until a slope value is determined:
    1. Find the slope value via design drawing
        1. For each study sewer tagged in this category, navigate to the drawing using the URL in the &quot;STICKERLINK&quot; field. Do this by copying and pasting the URL into the Internet Explorer browser.
        2. Determine if the missing slope values are listed in the drawing for the study sewer.
            1. NOTE** : In some instances, the drawing for a neighboring sewer will contain the missing slope value that is needed.
            2. If the missing slope value is listed, input this value as an attribute in the &quot;Slope\_Used&quot; field for the study sewer.
    2. Find the slope value via manhole invert elevations
        1. Navigate to the upstream and downstream manholes of the sewer in question and see if invert elevations exist in the attribute table for these manholes.
        2. Use the invert elevations and the length of the pipe to calculate slope and input this value as an attribute in the &quot;Slope\_Used&quot; field for the study sewer.
    3. Find the slope value via the Terrain Slope
        1. Use the &quot;GIS\_GSG.Contour\_2015\_1ft&quot; layer to view contours for the area to calculate the slope.
        2. Input the parameters into the Slope Value Verification Tool and navigate to the Terrain Slope section. Determine if the new velocity value falls within the design velocity criteria. If it does, input this value as an attribute in the &quot;Slope\_Used&quot; field for the study sewer.
    4. Find the slope value via minimum design velocity.
        1. Input parameters into the Slope Value Verification tool and navigate to the minimum design velocity section. Input this value as an attribute in the &quot;Slope\_Used&quot; field for the study sewer.
5. In the Editor Toolbar dropdown menu, select &quot;Save Edits&quot;, then &quot;Stop Editing&quot;.

## 5. Assess Neighboring Sewers
Determine whether sewers adjacent to the study sewer (upstream and downstream) are within the scope of the SSHA process.
1. If the study sewer in question is undersized, perform hydraulic and hydrologic calculations on upstream sewers until capacity calculations show the sewers are sized properly.
2. If the study sewer in question has neighboring downstream sewers that are less than or equal to 36&quot; diameter or equivalent size, perform hydraulic and hydrologic calculations.
3. Note that any study sewer with upstream flow splits is outside the scope of this analysis and should not be performed.

## 6. Set Up Data Driven Pages
1. Go to the &quot;Drainage Areas&quot; layer group &gt; &quot;DA Indices&quot;. Right-click and select &quot;Add Data&quot;
    1. Navigate to the &quot;Small\_Sewer\_Capacity&quot; geodatabase, then &quot;StudyAreaIndices&quot; group and choose the &quot;DA\_\*\*\*\*&quot; feature class , where &quot;\*\*\*&quot; is the current Project ID.
    2. Select &quot;Add&quot;       
2. Switch to &quot;Layout View&quot;
3. Enable Data Driven Pages
    1. Click the Data Driven Pages setup Icon        ![](data:image/*;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAoCAIAAAD2YqSKAAAAAXNSR0IArs4c6QAAA8FJREFUWEftV0tME1EU7RRa+kewJBawfhACgYgYMS4wCBvYajSiiCsTEEtQ1IUmsnKJO2M0MTGujJ+FRFdikL8xFkoRUyGSpkBRCGD/n5l2xgPTlEo7pR2QFW/x0um759x77ud1SrjdbgHfRRBEslBhsoBN2u/422QC18F38rmTz2QyQGzmfknGUciWCAQCPGC8IQRFUbzBPIDbPX8ESZI8wuQNCfnr7u4GRXV1dSSRrqU1J1vj8XhI0k9RgWAwyNB0Xl5e6/XWZP0NDQ1JJBIwpDIMEwaLRKJ1ctvb762jNhr0HXcbSkqKBEQaTYhlclXOgUJKoqak6sPZqjhxFBYW4jSVtaiqqhKLxTGtG5uawyodtmWNeO5K46VcrRY/tYyA8bjds5ax4UnF3hPnNxQ9Ojq6ps/v93MBwiqfP2guO1L7fVS/O1MmligFDC1kfEp50OFYmrFamT05MRlomlar1dg1Gk2oP3tWV5wAofJ2c/1BbXrX+07Sa3faFkjPste58Gva9G3k68+JSavVGhOOeUMH4MhsNuNuEaJ+4RKmpaWxj5FfhlmK94nHx755vX6LeXqwp9/y0zRjnjAaxr/oLSQjn52djcSyn9EgLpfLaDTOz8/b7fa5ublQ/SorK9E/cfQRTGDqxxidIfVlSny/HX4fqZSLxkwLHz9bnr788Ozdp7cDXwVXT69jgDjcX0VFRXCMZkQEIX345F1dXPoYIrXhzovdxdmKfGX6qTOUpupVv+2c7uGSX47aNJ2tfXNfF1NfSkoKgoA4dD60hurXt7qi9dXVXQh/WVBQEFip/MmS4+cDudMBTer+A4fYV0KlUoleiIZDHPxhZWVloVgqleqf+kml0sgYLzfUt7ToIlnA/bm3q/N1h9jlOpovYo+iZUV2wMjIyPDwsF6vR7/AeO33SCaT4Zntpch1o+2W3fYnfVfGk8ePohWUHyvv7evlKjwSCGdwj+KVlpYiq2v1Q7NiRQfbcOmiTnctTivF0cceCYVCDPfU1JRcLicQArgGBgawV1RUcPG23bwNlU6nEz1FkSQKE6RpXKew7+nlHFyQI41ardZkMq0Mn1BIgAKYwcFB7DU1Naz76GUwGLhCKSsr4zpiE4abBZ4WFxfxGPIHgEKhwM7lL04+4xyBDZOHzvT5fHC50i8Oh4MfVyIoNAhkYB4gEfOH/f/6i46JQAiJRLpVNtv+/mKz2bYq9kR4tl0fZgItm0hoW2ITul9WJiP5P/88IiAwiYDhfor0hwnlwZUIJIY+rhe1ROg2tPkLR4KIzivntkkAAAAASUVORK5CYII=)
    2. Check the Enable Data Driven Pages box. Set the index layer to the layer within &quot;Drainage Areas&quot; &gt; &quot;DA Indices&quot; &gt; &quot;DA\_\*\*\*&quot;, where &quot;\*\*\*&quot; is the current Project ID.
    3. Set the Name Field and Sort Field to StudyArea\_ID
  4. Set-up the display expression.
    1. In the &quot;DA\_\*\*\*&quot; layer properties, navigate to the Display tab and select &quot;Expression&quot;
      1. Click the &quot;Load&quot; button
      2. Navigate to the &quot;Scripts/arcmap\_expressions&quot; folder, select &quot;StudyAreaSummary2.lxp &quot;
      3. Click &quot;Open&quot;
      4. Click &quot;OK&quot;

## 7. Qaulity Control Procedure
1. Confirm that runoff coefficient is consistent with land cover characteristics
    1. For dense areas of the city, assume C=0.85 (default)
    2. For less dense residential areas (e.g. Manayunk, Northeast, etc.), assume C=0.75
2. Review hard copy sewer study (if available)
    1. Ensure drainage area delineation is consistent
    2. Ensure runoff coefficient is consistent
3. Confirm that the &quot;Run H&amp;H Calcs&quot; tool is executed after all corrections to data are made.
4. For each study area, review the return plans to confirm that DataConv data is accurate.
5. Review results and identify irregularities
    1. Confirm that calculated capacity is within range of typical values. [This chart](https://plot.ly/~aerispaha/57.embed) shows possible capacity values for a range of sewer diameters within the design velocity range (2.5 to 15fps):
    [<img src="{{site.baseurl}}/public/img/capacity-within-design-velocity-plot.png">](https://plot.ly/~aerispaha/57.embed)

## 8. Export the Study
1. Go to &quot;File&quot;, then &quot;Export Map&quot;
2. Navigate to the corresponding project folder for the current work order number and go to &quot;01 Analysis\03 Sewer Analysis&quot;
3. Name the file as &quot;SSHA\_\*\*\*&quot;, where &quot;\*\*\*&quot; is the current Project ID.
4. Choose the &quot;.PDF&quot; file type.
5. Under &quot;Options&quot; choose the &quot;Pages&quot; tab. Choose &quot;All Pages&quot;
6. Click &quot;Save&quot; to export the file.

**NOTE** : ArcMap 10.1 has a known bug which prevents the program from exporting maps multiple times. If you wish to export another study to PDF, restart ArcMap. Otherwise, the program will crash.
