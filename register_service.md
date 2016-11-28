# Register Konstructor Service

 This script will enable you to register your service, api, script, documentation, etc which the Konstrutor Registry Service.
 
To configure:

1. Create an API Key for the Registry API.  To do this type `key generator` within a public slack channel and follow the link.

2. Add the API Key to your environment variables section within Circle CI UI (not the circle.yml file!).  The key should be called `KONSTRUCTOR_REGISTRY_KEY`

3. You will then need to add the folling command somewhere appropriate within your circle.yml file. 

    
    bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f <your service file>
    

Example:
     
     checkout:
       post:
         - bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/register_service) -f .konstructor
     
Note: Your service file is usually .konstructor     

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
- Your system code must exist prior to this executing. (TBD)