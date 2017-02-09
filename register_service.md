# Register Konstructor Service

This script will enable you to register your service, api, script, documentation, etc which the Konstructor Registry Service.
 
To configure for Circle CI:

1. Create an API Key for the Registry API.  To do this type `key generator` within a public slack channel and follow the link.

2. Add the API Key to your environment variables section within Circle CI UI (not the circle.yml file!).  The key should be called `KONSTRUCTOR_REGISTRY_KEY`

3. You will then need to add the following command somewhere appropriate within your circle.yml file. 

    
    bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f <your service file>
    

Example:
     
     checkout:
       post:
         - bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f .konstructor
     
Note: Your service file is usually .konstructor     

To configure for Jenkins:

1. Ensure in your deployment process you clone the github repository (if you dont already)
2. Within your 'execute script' the first line must be '#!/bin/bash'
3. Enter the same command as above in your 'execute script'
    bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f <your service file>

Optional: 

If you wish to dynamically specify a version within your definition file - modify the version field in your definition file to:

    "version" : "{{version}}"

The specify `-v <your version>` in your call
    
 Example:
      
      checkout:
        post:
          - bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f .konstructor -v <your value>
      
   
    
    
Conditions of use:

- Please ensure your service file is well formed and follows the Konstructor Service file definition recommendations.
- Your system code must exist prior to this executing. See [this page](https://sites.google.com/a/ft.com/releaselogs/system-code) for details.

## Definition File:

The definition file should be ideally be called *.konstructor* and placed in the root of your git repository.  But you
are free to do what you wish.   If you are populating a non-code based link - see the git project [catalogue entries](http://git.svc.ft.com/projects/KON/repos/catalogue-entries/browse).

The definition file supports the following entries:

| Name        | Type         | Details                                                    | M |
|-------------|--------------|------------------------------------------------------------|---|
| name        | string       | Name                                                       | Y |
| description | string       | A short description                                        | Y |
| systemCode  | string       | A CMDB identifier.                                         | N |
| version     | alphanumeric | Semantic version (e.g 1.0.0)                               | Y |
| type        | string       | Api, Script, Doc, Service, Library, Test(for testing only) | Y |
| locationUrl | URL          | A link to its code store or document location              | Y |
| status      | string       | Inactive (default), active, beta, deprecated, removed      | N |
| serviceUrl  | URL          | A link to a web based hom page                             | N |
| supportUrl  | URL          | A link to the ticketing system                             | N |
| tags        | string       | Comma separated list of tags                               | N |


M = Mandatory

### Requirements for all types except Doc

*Must* be located in a code repository
*Must* contain a README.<docType>
*May* contain a PANIC.<docType>

### Versioning

A manual version number must be in semantic format.  Version can be passed as a parameter on the script for dynamic updates.

### Status

The default status is always inactive.
The removed status is to retain the service with the UI but show it as no longer available.  E.g. you may want to detail an alternative solution, link to a new service, etc.  The readme information will be available for this.

### SupportUrl

Optional entry to link to a Jira/Github/Trello/Salesforce ticketing page to raise support requests.

#### Examples:

**API** 

	{
			"name": "Konstructor Version API",
			"description": "An API to provide easy, stateful semantic versioning.",
			"systemCode":"konversionapi",
			"version" : "1.0.0",
			"type": "api", 
			"locationUrl": "https://github.com/Financial-Times/lambda-version-api",
			"status": "active", 
			"serviceUrl": "https://konstructor.in.ft.com/swagger.html?url=../versionapi.json",
			"tags": "build"
	}

**Script** 

	{
			"name": "Konstructor Service Scripts",
			"description": "A set of scripts to enable easy use of the Konstructor Service APIs",
			"version" : "1.0.0",
			"type": "script", 
			"locationUrl": "http://git.svc.ft.com/projects/FTT/repos/konstructor-scripts/browse"
			"status": "active"
	}

**Document** 

	{
			"name": "Engineer Checklist",
			"description": "The FT engineering checklist",
			"version" : "2.0.0",
			"type": "doc", 
			"locationUrl": "https://sites.google.com/a/ft/tech/org/engineering/-engineering-checklist"
			"status": "active"
	}
