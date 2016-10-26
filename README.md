# konstructor-services-scripts

A set of useful scripts to help integrate with Konstructor Services.

## raiselog

This script will raise a release log record for the service being deployed.  

To configure:

1. Create an API Key for the Change Request API.  To do this type `key generator` within a public slack channel and follow the link.

2. Add the API Key to your environment variables section within Circle CI UI (not the circle.yml file!).  The key should be called `RELEASE_LOG_KEY`

3. Ideally the release log should be raised once your deployment has been successful.  The circle.yml file contains a deployment section and this is the preferred location though you are free to place it where you see appropriate. 

An example being:

    deployment:
      production:
        branch: master
        commands:
          - wget -O - https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/raiselog | bash	

The script also accepts a parameter to specify the system code.  By default the script will use your repository name as the system code but this isn't always appropriate.

To specify a system code:

    deployment:
      production:
        branch: master
        commands:
          - wget -O - https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/raiselog | bash -s yourSystemCode

That is it!

Conditions of use:

- The username performing the build must be the one registered with the FT github organization.
- The user must exist in Salesforce!
- The system code must exist in CMDB.