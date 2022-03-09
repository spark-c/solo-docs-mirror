# Solo Development Environment

Check the "Tools and Links" section below for links to our project management tools.

For information, links, and documentation regarding the [UI-Template Volt React Dashboard Bootstrap 5](https://demo.themesberg.com/volt-react-dashboard), please see "TEMPLATE_README.md" in this repo.

Tools and Links
* [Coda](https://coda.io/workspaces/ws-ERyqNAdO43/folders/fl-2lROoT0DSe) Documentation
    - [How-To](https://coda.io/d/Solo_dG1cBUnUJqt/How-Tos_su4hh)
    - [Brainstorming](https://coda.io/d/Brainstorming_dCfYig-P-D5/Untitled_subA-#_luB1H)
    - [Research](https://coda.io/d/Brainstorming_dCfYig-P-D5/Market-Analysis_subA-#_luB1H): Research and market analysis
    - [Meeting Notes](https://coda.io/d/Solo_dG1cBUnUJqt/Meetings_su104)
    - [Contact Information](https://coda.io/d/Directory_dK5VMvOwHIh/Directory_sudER#Members_tu04z/r4)
* [Miro](https://miro.com/app/dashboard/): Live whiteboarding
* [BitBucket](https://bitbucket.org/laguna-studio-code/solo-web/src/master/): Source code repository
* [GitKraken](https://www.gitkraken.com/) (desktop app): Workflow, Backlog, Project Management

---

## Initial Installation and Setup

1.  Make sure you have [Node.js](https://nodejs.org/en/) and [Python](https://python.org/) installed.
2. Clone into this repository with `git clone https://bitbucket.org/laguna-studio-code/solo-web/`
3.  In the project root ("/solo"), run `npm install` to install (frontend) project dependencies.
4. `cd api` and run `python -m venv venv` to create a virtual environment in the /api directory.
5. Activate that virtual environment with `venv/Scripts/activate`
6. `pip install -r requirements.txt` to install (backend) project dependencies.


## Spinning up the project

1. Start the Flask backend like so:
  1. `cd` into solo/api with `cd api`
  2. Activate the virtual env with `venv/Scripts/activate`
  3. Start the server with `flask run --no-debugger`
  4. Return to the root directory (you will likely need to open another shell instance)
2. Run `npm start`


## Current testing page for api integration

1. Navigate to https://localhost:3000/#/apitest
2. Clicking the button "Send Request" SHOULD initiate the following:
  1. Send a request to the Flask api route "/debug/test_message"
  2. Receive a response with object `{ message: "hello world" }`
  3. Update the area below "Response message" with that message (hello world)  




