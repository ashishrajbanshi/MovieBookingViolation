#!groovy 

import groovy.json.JsonSlurperClassic

node{
    stage('checkout source') {
        checkout scm
    }
    // stage('Installing Plugins SFDX SCANNER'){
    //     ec = command 'sfdx plugins:install @salesforce/sfdx-scanner'
    // }
    // stage('Checking Eslint violation'){
    //     rc = command 'sfdx scanner:run --target "**/lwc/**" --format html'
    //     if(rc!=0){
    //         error 'Cannot check eslint violation'
    //     }
    // }
    // stage('Checking pmd violation'){
    //     rc = command 'sfdx scanner:run --target "**/classes/**" --format html'
    //     if(rc!=0){
    //         error 'Cannot check pmdgit  violation'
    //     }
    // }
    stage('New'){
        try{
            command 'sfdx scanner:run --engine "pmd" --pmdconfig "./.rule_reference.xml"  --format html --target "./force-app/main/default/main/default/classes" -o ./force-app/pmdscanresult.html --severity-threshold 2';
            command 'sfdx scanner:run --engine "eslint-lwc" --eslintconfig " ./.eslintrc.json"  --format html --target "./force-app/main/default/main/default/lwc" -o ./force-app/eslintscanresult.html --normalize-severity --severity-threshold 1';
        }
        catch(Exception ex){
            error(ex.getMessage());
        }finally{
            // Publish the result
            publishHTML([
                allowMissing: false, 
                alwaysLinkToLastBuild: true, 
                keepAll: true, 
                reportDir: 'force-app', 
                reportFiles: 'pmdscanresult.html, eslintscanresult.html', 
                reportName: 'Source Scanner Report', 
                reportTitles: ''
            ])
        }
    }
}
def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
        return bat(returnStatus: true, script: script);
    }
}
