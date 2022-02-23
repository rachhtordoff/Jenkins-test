#!/usr/bin/env groovy
import static groovy.io.FileType.*


// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']
node {
  
  stage "checkout repo"
  
  checkout scm 
  
  stage "stuff"
  
def exclude_list = [
  "staging-803.yaml"
]

sh 'pwd > workspace'
env.WORKSPACE = readFile('workspace').trim()
def workspace = "${env.WORKSPACE}"
  
    releases = []

    new File(workspace+"/values/").traverse(type: groovy.io.FileType.FILES) { staging_name ->

      name = staging_name.name
      if (!exclude_list.contains(name)){

          if (name.replace(".yaml", "").replace("staging-", "").isInteger()) {
            read_yaml = readYaml(file: "values/"+staging_name.name)
              auth_name= read_yaml['migration-helper-ui']['authorityName']
              if (auth_name != 'Staging Maintain') {
                  releases << staging_name.name.replace(".yaml", "")
              }
          }
      }
      
}

    
      releases.each { item ->
        println "${item}"
    }
  }
