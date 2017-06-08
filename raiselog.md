# raiselog

This script will raise a release log record for the service being deployed.  

To configure:

1. Ensure you have generated a system code in Salesforce. If you don't have one you can do this with Slack by simply typing "/systemcode [your code]"
    Tip: System code should be lower case, no spaces.
    
2. Now create an API Key for the Change Request API.  To do this type `key generator` within a public slack channel and follow the link.

3. Add the API Key to your environment variables section within Circle CI UI (not the circle.yml file!).  The key should be called `RELEASE_LOG_KEY`

4. If your Github repository is `private` you need to generate an API Token for Circle CI.  If its `public` go to point 5.

    a. Within Circle CI - Edit the build settings and click `Api Permissions`.
    
    b. Click `Create Token`, Select `build artifacts` from the dropdown and give it a meaningful name - suggestion is `RELEASE_LOG_ACCESS_KEY`
    
    c. Add the key to your environment variables section within Circle CI UI (not the circle.yml file!).  The key should be called `CIRCLE_API_KEY`

5. Ideally the release log should be raised once your deployment has been successful.  The circle.yml file contains a deployment section and this is the preferred location though you are free to place it where you see appropriate. 

An example being:

    deployment:
      production:
        branch: master
        commands:
          - 
          -  bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/raiselog) 	

The script also accepts a parameter to specify the system code.  By default the script will use your repository name as the system code but this isn't always appropriate.

To specify a system code:

    deployment:
      production:
        branch: master
        commands:
          -  bash <(curl -s https://raw.githubusercontent.com/Financial-Times/konstructor-services-scripts/master/raiselog) -s yourSystemCode

That is it!

- You will now see release logs being opened and closed automatically within the #ft-releases channel.
- You can view the output in your build for the Salesforce ID as an alternative.
- A build will fail if the release log cannot be raised.
- A release log will automatically contain your commit message and description (if applicable)

Conditions of use:

- The username performing the build must be the one registered with the FT github organization and be correctly named in users.txt
- Your email address must exist in Salesforce! Ensure you can login!
- The system code must exist in CMDB (see step 1)
