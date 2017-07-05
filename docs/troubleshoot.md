---
layout: page
title: Troubleshooting
---

This page includes information related to common issues encounted when working within ArcMap and using the Small Sewer Hydraulic Analysis (SSHA) tool. If your issue is not covered here, please contact <<Task Lead/Admin>>.

<hr>

### Cannot find the Drainage Area Index
After running the "Run H&H Calcs" tool, a Drainage Area Index feature class is created that mirrors the data included in the Small_Sewer_Drainage_Areas feature class, for one Project_ID. While the SSHA tool is programmed to add this to your map, sometimes ArcMap fails to do this. In this case, add the Drainage Area Index manually by clicking the "Add Data" tool in ArcMap. In the dialog window, navigate to the StudyAreaIndices feature dataset and find the Drainage Area Index applicable to your current Project_ID.

### ArcMap freezes when exporting
With some versions of ArcMap, exporting data driven pages as PDFs fails after exporting once. To mitigate this, save your changes, close ArcMap, reopen Arcmap, and attempt to export again.    

### Run H&H Calcs fails
There are several reasons that this step may fail. Luckily, most of them are realtively easy to troubleshoot. Here are some examples of steps you can take to rectify these issues:
  * **Check for <null> data**: review the data within the StudiedSewers layer to confirm that no rows appear to have all `<null>` data. This is made easy by sorting the columns (`<null>` rows will appear that the top of the sort). Delete any rows with `<null>` data and rerun the tool.
  * **Confirm StudyArea_ID is set**: confirm that new studied sewers have a StudyArea_ID set. Sort the rows in the StudySewers and Small_Sewer_Drainage_Areas tables and delete or rectify any `<null>` StudyArea_ID data.
