---
title: "History Log Terminal Analysis"
categories:
  - Tech
tags:
  - data-science
  - log system
---
**

Specification document History Log Terminal

## Background & Strategic Fit

Terminal has always been the close friend to developers. We may overlook the terminal as only to work on a daily basis, but it turns out we can personalize person behavior based on their dump history terminal log.

## Problem Statement

While our applications have always grown because of market demand, we intend to give the best solution and support to our customers at minimum time. Many of our issues are solved from machine to machine connection, which means they must use either linux, windows or even macOS.


Manual log analysis depends on the proficiency of the person running the analysis. If they have a deep understanding of the system, they may gain some momentum reviewing logs manually. However, this has serious limitations. It puts the team at the mercy of one person. As long as that person is unreachable, or unable to resolve the issue, the entire operation is put at risk.

## Specification

History log data from terminal Operating System.

![](https://lh4.googleusercontent.com/-Aj8WHEADlg0xtxfdFd4Dz-U-BvkGAHSSeyvGCc2Lef7yhNRGuhpABz_NW8_2mJfBYIhdaMhcPg_BUNH40wpnU4EvXu7gyhRzZoUG62wmk6JBVR9rKZYjPdWdmQ-ifC_Wwg74ek2YQ4EYhOFF_FJkCXCj7ydJ3fXqYJncomhrvAZH2bdYRiX2JWuxg)

## Goals

-   Personalize the terminal history to reflect behavior user.
    
-   Comply with security Policies, Regulations & Audits
    
-   Automate task
    

## Success Metrics

1.  Transform unstructured terminal history to structural data.
    
2.  Analysis data log.
    

## Requirements

1.  Shell terminal
    
2.  Hardware that running an operating system
    
3.  Dump history into txt file
    
4.  Append timestamp
  

# Analysis

### Types of structured data analysis

- Pattern Detection and Recognition 

refers to filtering incoming messages based on a pattern book. Detecting patterns is an integral part of log analysis as it helps spot anomalies.

- Log Normalization  
is the function of converting log elements such as IP addresses or timestamps, to a common format.  
  
- Classification and Tagging  
is the process of tagging messages with keywords and categorizing them into classes. This enables you to filter and customize the way you visualize data.

  

### Security Consideration

>Do you have especially sensitive data via HTTP like credit card number, that may require HTTPS? 
In log system we don't intend to log specific data like password or credentials, or credit card numbers. 


>Do you require authentication & authorization on some features, and is everything by default blocked,unless authorized?  
No

### Financial Consideration

Since this will add the number of characters we store in the log, we probably need to check the impact of cost log. The cost is related to the number of characters we write.

### Data Tracking, Data Analytics & Marketing Integration Consideration

>Does this feature require additional monitoring & tracking?  How about mobile tracking, web tracking?  It's important that tracking requirements be identified from Day 1. 

Log data should not be track, however we may need some kind of analytics dashboard to integrate with our logging system.

### Operation Consideration

>Does this feature introduce new behavior that needs to be communicated to customers? new features / changes used by the operation team? 

This improvement should be announce to engineer team whom interact with log per daily basis.

# Design

## System Design

![](https://lh6.googleusercontent.com/T36kll5lQn7P-C6v6LJdY82pjD9dkON4pliiCdb3cJR8DzlFqMEVxlGwkjKrBd_t4-ck7JZxifyEYabYhpF4HF309Tocr3fU8S83nUCCa-ZlrQiPz8P07Z5KI3iiB4smJZoK2GTiCwCvRDBpDEXpnbJOcApTsZY0BwZMA9ZLm6zZdzpDELGUHy2QYA)

### Extract challenges 

The data extraction part of the ETL process poses several challenges. A lot of the problems arise from the architectural design of the extraction system:

#### Data volume.

The volume of data extraction affects system design. The solutions for low-volume data do not scale well as data quantity increases. In this study will use more than 2000 rows of data, with size 3,3 gigs.

#### Source limits

We used private sources from system apps. The dataset was downloaded from Harvard's Dataverse and contains logs from an Iranian ecommerce site (zanbil.ir)

### Transform

![](https://lh6.googleusercontent.com/6cK4pkeTfzauYcSsfKejGPVMDrYTT4MyEYQ00MpeVd28DGrZexHx4RixK3bC5mXaVVnPhyM-fDDVTttGxF-gmsIcEt-XstpsdYT9AnYGZechwHGnqIbYqK3pAC-P9h8G5e6HLJ9FfDKFXYEI-FCtsBRYcL6RuSROo6leFqtoBfaQdLpQLNlBhaaGxQ)

Data cleaning. 

Data cleaning involves identifying suspicious data and correcting or removing it. For example:

-   Remove missing data
    
-   Trim data
    

### Load 

#### Schema database

  ![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/logsystem/20220915220217.png){: .align-center}

  
**