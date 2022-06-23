# 2022-devsecops-example Example Project
This project is intended for use as a model to play with some more
DevSecOps principles in a practical way. 

## Overview
This exercise will take you through setting up a simple DevSecOps pipeline
for a Python project. The example Python project is a simple client that
interacts with the OpenWeatherMap API. The project contains some limited
tests. 

In the course of this exercise, you will set up your own copy of the 
repo to build in TravisCI, using pytest to perform automated testing
and coverage analysis, SonarCloud to perform security scanning and
reporting, and Coveralls.io to provide coverage reporting. 

After setting up the pipeline, there are some opportunities to improve
the project's performance in the pipeline. 

## Instructions
### Prerequisites
* Python 3.7+ (tested against 3.8 and 3.9)
* Python virtual environment module (I use `virtualenv` (`pip3 install virtualenv`))
* Github account
* travis-ci.com account (https://travis-ci.com, sign in with your Github account, choose the free plan, don't add any projects yet)
* coveralls.io account (https://coveralls.io, sign in with your Github account, choose the free plan, don't add any projects yet)
* sonarcloud.io account (https://sonarcloud.io, sign in with your Github account, choose the free plan, don't add any projects yet)

### General Procedure
#### Setup
1. Fork the repo https://github.com/jwoytek/2022-devsecops-example to your own Github account
2. Go into your Github user settings
3. Scroll down to select "Applications". You should see TravisCI and SonarCloud here.
4. Click "Configure" for TravisCI.
5. Under Repository Access, choose "Only select repositories", and select your fork of the 2022-devsecops-example repository from the drop-down.
6. Read and click through any approval prompts. 
7. Click Save.
8. Click again on "Applications" on the left bar.
9. Click "Configure" for SonarCloud.
10. Under Repository Access, choose "Only select repositories", and select your fork of the 2022-devsecops-example repository from the drop-down.
11. Read and click through any approval prompts. 
12. Click Save.
13. Clone your forked repository to your development machine.
14. cd into the 2022-devsecops-example directory.
15. Create a virtual environment for testing: `virtualenv -p python3 venv`
16. Activate the virtual environment: `. venv/bin/activate`
17. Install the Python requirements: `pip3 install -r requirements.txt`

#### Local Testing
Run local tests: `py.test --cov --cov-report=xml owm tests -vv`

All tests should complete successfully.

#### Client Testing
This is not required, but if you would like to see the client in action,
you will need to sign up under the free plan for the OpenWeatherMap API.
Once you have signed up, get your API key, and paste it into a file in the
repository root named `.apikey`. Once this is done, you should be able
to run `test.py` to see the weather in Pittsburgh and at a nearby 
location.

#### Configure the Pipeline
1. Go to your SonarCloud.io account.
2. Use the "+" menu at the top right to add a new project.
3. You should see your repo listed here. Select it to import the project.
4. Once the initial import and analysis is complete, click on the project to view it.
5. Under the Administration menu, choose "Analysis Method".
6. Turn OFF "SonarCloud Automatic Analysis". This needs to be OFF so that TravisCI can trigger the analysis as part of the pipeline.
7. Under Administration menu, go to "Update Key".
8. Copy the update key listed. We will use this in a future step.
9. Click your profile photo in the top right, then go to the Security tab.
10. Create a token. The name does not matter. Copy the token. We will use this in a future step. 
11. Go to the Organizations tab. 
12. Copy the "Key" for your organization. We will use this in a future step.
13. Go to your TravisCI account.
14. Select your 2022-devsecops-example repository.
15. From the menu at the right, choose "Settngs".
16. Under Environment Variables, enter a new variable named "SONAR_TOKEN", and paste in the security token you created in step 10.
17. In the repository directory, edit the `.travis.yml` file. This is the TravisCI configuration file. 
18. Locate the "organization" value under "sonarcloud". It is set to "jwoytek" by default. Change this to the organization key copied in step 12.
19. Save the `.travis.yml` file. 
20. Edit the `sonar-project.properties` file. This is the SonarCloud configuration file.
21. Change the value of the `sonar.projectKey` to the "update key" copied in step 8.
22. Save the `sonar-project.properties` file. 
23. Go to your coveralls.io account.
24. Click the "+" to add a repo. 
25. Locate your repo in the list and click the switch to "on". 


#### Trigger a TravisCI Build
TravisCI will build whenever a commit is pushed to your repo. You can also
trigger a build manually from within TravisCI. Here, we are going to 
commit the changes to the TravisCI and SonarCloud files, which will 
trigger a build, and should execute all of the steps to test, scan, and
report scan and coverage results if all went well. 

Commit the two modified configuration files in your repo, and push them
to github. This will trigger an automatic build cycle in TravisCI. 

Go to your TravisCI account, and in the dashboard you should see a new
job running.

Follow along in the log, and you should see TravisCI set up the
the environment, execute the python tests, execute the SonarCloud scan, 
report the SonarCloud results, and then upload the coverage results to
coveralls.io.


#### Review the Results
Once the build is complete and if it succeeds, you should see the build
job logs in TravisCI, Github should show a green check on the project 
by the latest commit, SonarCloud.io should show a report on the project
scan, and coveralls.io should show a report on code coverage from the
tests. 


#### Extension
You should notice that the code is not 100% covered by tests. Use the
reporting in coveralls.io to locate the code that is not covered by 
tests, and create tests such that coverage reaches 100%. 

SonarCloud.io will show two "code sniffs", pointing out some things that
could be improved in the code. Attempt to fix one or both of these 
inefficiencies. 

Create a branch and made some modifications to the branch, then create
a pull request. Observe the automatic testing and reporting that Github
will show during the TravisCI pipeline run to assist in evaluating a
pull request prior to merging. 

