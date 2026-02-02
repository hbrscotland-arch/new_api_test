Steps to run Postman collection with Newman:



1. Prerequisites

Make sure you have Node.js installed:

npm -v  
node -v

If not, install it from nodejs.org.

2. Install Newman

Install Newman globally:

npm install -g newman

Verify installation:

newman –v

3. Export Your Postman Files

From Postman:

Collection → ... → Export → save as collection.json
(Optional) Environment → ... → Export → save as environment.json


4. Run a Collection
   newman run collection.json


5. Run with an Environment
   newman run collection.json -e environment.json



6. Generate Reports

HTML Report

Install the reporter:

npm install -g newman-reporter-html

Run with report:

newman run collection.json -r html --reporter-html-export report.html 



For GitHub CI Pipeline, create workflow, Postman.yml file below:

name: Automated API tests using Postman CLI

on:
push:
branches:
- main
pull_request:
workflow_dispatch:
schedule:
- cron: '0 18 * * *'   # runs daily at 6:00 PM GMT

jobs:
postman-tests:
runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Newman
        run: npm install -g newman

      - name: Debug – list repo files
        run: ls -R

      - name: Run Postman collection with environment
        run: |
          newman run tests/postman/collection.json \
            -e tests/postman/environment.json