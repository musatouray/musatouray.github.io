---
title: "Senegal Presidential Elections Analysis"
excerpt: "Near real-time election analysis using AI Builder OCR, PowerApps, and Power BI DirectQuery integration."
header:
  teaser: /assets/images/senegal_elections_analysis.jpg
  overlay_image: /assets/images/senegal_elections_analysis.jpg
  overlay_filter: 0.5
tags:
  - Power BI
  - AI Builder
  - PowerApps
  - Azure SQL
  - DirectQuery
  - OCR
  - Election Analysis
---

This project is an analysis of the just concluded presidential election of Senegal, and also a throwback on the 2019 one. It was conceived as a tool to view the elections result in near real-time as the results are made available. To make it work, I use Microsoft AI Builder to train the model to capture the results on the documents that were being shared by various sources online. Once the model was trained, with 98% accuracy, it then writes the output to a PowerApp which was connected to an Azure SQL Database as source. I then in turn connect the database to Power BI using DirectQuery, this allowed me to visualize the data in real-time.

However, I had the constraint of not being able to get access to all the election results as there was no reliable source. This is still a pending issue, hoping that the election commission will publish the full results soonest.

<p>
    <iframe style="width:100%;" height="383"
        src="https://app.powerbi.com/view?r=eyJrIjoiOGRkNTU4MzktYTc1NC00OWQ3LTlmZjYtNjI1N2FlYjQ1OWQwIiwidCI6ImU3ZmRiMmEyLTUzODAtNDBmMC04MmQ4LWEzYjU0YzFmODE3ZiJ9"
        frameborder="0" allowFullScreen="true">
    </iframe>
</p>
