#!groovy 

import groovy.json.JsonSlurperClassic

node{
    stage('checkout source') {
        checkout scm
    }
    stage('Installing Plugins SFDX SCANNER'){
        ec = command 'sfdx plugins:install @salesforce/sfdx-scanner'
    }
    stage('Checking Eslint violation'){
        rc = command 'sfdx scanner:run --target "**/lwc/**" --format html'
        if(rc!=0){
            error 'Cannot check eslint violation'
        }
    }
    tage('Checking pmd violation'){
        rc = command 'sfdx scanner:run --target "**/classes/**" --format html'
        if(rc!=0){
            error 'Cannot check pmdgit  violation'
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
