---
layout: home
author_profile: true
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/musatouray-bg.png
  actions:
    - label: "View Portfolio"
      url: "/portfolio/"
    - label: "About Me"
      url: "/about/"
excerpt: "Microsoft Certified Power BI & PowerApps Developer | Data Analytics Portfolio"
intro:
  - excerpt: 'Specializing in **Power BI**, **Data Visualization**, and **Analytics Automation**. Experienced in creating real-time dashboards, business intelligence solutions, and data-driven applications using the Microsoft Power Platform.'
feature_row:
  - image_path: /assets/images/gambia-presidential-elections.jpg
    alt: "Real-Time Presidential Elections"
    title: "Real-Time Live TV Presidential Elections Report"
    excerpt: "Broadcasting live presidential election results on national TV using Power BI with 30-second refresh intervals and DirectQuery."
    url: "/real-time-live-tv-presidential-elections-report/"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: /assets/images/gambia-parliamentary-elections.jpg
    alt: "Parliamentary Elections Dashboard"
    title: "National Assembly Parliamentary Elections Dashboard"
    excerpt: "Live TV reporting of parliamentary elections with PowerApps data capture and real-time Power BI visualization."
    url: "/the-gambia-national-assembly-parliamentary-elections-live-dashbard/"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: /assets/images/senegal_elections_analysis.jpg
    alt: "Senegal Elections Analysis"
    title: "Senegal Presidential Elections Analysis"
    excerpt: "Near real-time election analysis using AI Builder OCR, PowerApps, and Power BI DirectQuery integration."
    url: "/senegal-presidential-elections-analysis/"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row2:
  - image_path: /assets/images/pl300.png
    alt: "Microsoft Certified: Power BI Data Analyst Associate"
    title: "Microsoft Certified"
    excerpt: '**Power BI Data Analyst Associate (PL-300)**<br>
    **Power Platform App Maker Associate (PL-100)**<br>
    **Azure Fundamentals (AZ-900)**<br>
    **Power Platform Fundamentals (PL-900)**'
    url: "https://www.credly.com/users/musa-touray"
    btn_label: "View Credentials"
    btn_class: "btn--info"
---

{% include feature_row id="intro" type="center" %}

## Featured Projects

{% include feature_row %}

## Certifications

{% include feature_row id="feature_row2" type="left" %}

## Latest Posts

<div class="entries-{{ page.entries_layout | default: 'list' }}">
{% for post in site.posts limit:3 %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
</div>
