---
title: "FortiDevSec Workshop" # MODIFY THIS TO BE THE TITLE OF YOUR WORKSHOP
chapter: true
weight: 1
---

# FortiDevSec Workshop 
<br>
<br>

## About TEC Recipes

TEChnical Recipes provide the learner with the opportunity to put into practice newly developed skills in an easy to launch environment that can be used for customer engagements.  At a minimum a TEChnical Recipe will include the following:

* A use case description
* An integrated lab and demo environment

  * Informational call-outs for key points to discuss or highlight to a customer
  * Questions that could be asked while giving the TEChnical Recipe as a demo
  * Points of value that relate the business value to the technical feature
* A reference architecture(s)

Optional components may be included for certain use cases

The TEChnical Recipe will not be a completely, self-contained learning experience for a single product.  A TEChnical Recipe will cover features and often multiple products where they relate to the use case of interest.

Deployments will be automated for those tasks that are not salient to the learning or demonstration activity in the use case.  For example, for a TEChnical Recipe focused on Indicators of Compromise, the system may deploy a FortiGate and FortiAnalyzer with configurations for these systems.  However, the leaner will have to configure the Event Handlers for IOC setup.

## FortiDevSec TEC Recipe

Introduction:

The realm of application security involves tools and techniques to protect applications from attacks and violations. Due to the huge advancement in hacking techniques and cyber-attack methodologies even modern complex applications contain unassessed security risks and vulnerabilities that may lead to substantial harm to your organization/business. Evaluating the security risks associated with applications and assessing the security weaknesses allows you to mitigate the potential risk to your organization with appropriate remedial measures.

FortiDevSec is a cloud-based automated application security tool that performs intensive and comprehensive scans for an accurate vulnerability assessment of your application.

The purpose of this TEChnical Recipe is to familiarize the learner with FortiDevSec, the various types of vulnerabilities it can help identify and remediate, and the ways in which it may be integrated into DevSecOps pipelines to enable such identification and remediation during and before deployment.

## TEChnical Recipe Main Objectives

* Set up the Github repository and configure a FortiDevSec application
* Run a SAST scan locally on the vulnerable application
* Run a DAST scan locally on the vulnerable application
* Understand FortiDevSec integration with JIRA

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}
