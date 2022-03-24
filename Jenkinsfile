#!/usr/bin/env groovy
import static groovy.io.FileType.*


// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']
  

pipeline{
  agent any
  stages {
    stage('checkout scm') {

      steps {
        script {

  
  checkout scm 
        
println "bhhgug"

def exclude_authority_names = [
  "Staging Maintain",
  "Training",
  "Team Betamax",
  "API integration test"
]

def exclude_list = [
  "staging-803"
]

releases = []

sh '/home/jenkins/agent > workspace'
env.WORKSPACE = readFile('workspace').trim()
def workspace = "${env.WORKSPACE}"
    
      
    File folder = new File(workspace+"/values/")  
    println folder
    File[] listOfFiles = folder.listFiles();
    println listOfFiles
    for (File file : listOfFiles) {
      println file

      name = file.getName()
      println name
      if (name.contains("staging-")){
        println "yaml file captured:"+name
        remove_yaml= name.replace(".yaml", "")
        if (!exclude_list.contains(remove_yaml)){
            get_name= remove_yaml.replace("staging-", "")
            if (get_name.isInteger()) {
              // this checks the number DOES NOT match 8, in prod only
              check_number = Integer.parseInt(get_name.substring(0, 1))
              if (check_number == 8){
                read_yaml = readYaml(file: "values/"+file.getName())
                auth_name= read_yaml['migration-helper-ui']['authorityName']
                if (!exclude_authority_names.contains(auth_name)) {
                    releases << file.getName().replace(".yaml", "")
                }
             }
          }
      
   }
      }
       }
    
      println "*********************************"
      println "PRINT SUCCESSFUL YAML FILES"
      println "*********************************"

  
      releases.each { item ->
        println "${item}"
           }
   }
}}
   }
  }

