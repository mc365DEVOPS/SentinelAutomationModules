# Sentinel Triage AssistanT (STAT) :hospital: - Sample Playbook

> [!NOTE]
> STAT documentation is being relocated to the builin [Wiki](https://github.com/briandelmsft/SentinelAutomationModules/wiki)

The Sample playbook is a Proof of Concept to demonstrate how STAT can be used to Triage an incident. This sample is not meant to be capable of triaging any type of incident; additional playbooks may need to be built using STAT to handle the unique requirements of different incident types.

The Playbook starts on a Sentinel Incident creation rule trigger and then starts the triage process using STAT:

## Sample Playbook Logic

1. The STAT Base Module is called using the STAT Connector to enrich and prepare the entity data for the STAT Solution
2. In parallel, the AAD Risks Module, Related Alerts Module and Threat Intel Module are called
    * The AAD Risks Module is configured to pass the Base Module response, and lookback 30 days in the Sentinel data for any MFA Fraud reports related to the entities in the triggering incident
    * The Related Alerts Module is configured to pass the Base Module response and check for any Related Sentinel alerts based on matching Account, Host or IP entity data in the last 30 days
    * The Threat Intel Module is configured to pass the Base module response and check for any Related Sentinel Threat Intelligence in the last 30 days based on matching 
    Domain, FileHash, IP, and URL entity data
3. The Scoring module is then executed using the inputs of the Base, AAD Risks, Related Alerts and Threat Intel modules to determine a risk score for the incident
3. A Condition is then evaluated on the calculated risk score to determine if it is greater than or equal to 40
    * If it is, the Incident severity is raised and a tag is added with the Triage result
    * If not, the Incident severity is lowered and a tag is added with the Triage result

## Sample Screenshot

![Screenshot of Logic Apps designer](images/sampletriage.png)

## Adding Additional STAT Modules

The sample playbook only makes use of a few of the STAT modules available to you.  To add additional STAT modules to this, or other playbooks you can add additional actions.  When adding a new action, select the Custom tab.  Typically you will see the Sentinel Triage AssistanT right away, but if you have other custom connectors and you cannot locate it, search for Sentinel Triage or STAT to narrow your results.

![STAT Connector View](images/statconnector.jpg)


---
[Documentation Home](readme.md)