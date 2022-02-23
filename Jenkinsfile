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
      println name
      if (!exclude_list.contains(name)){
          remove_yaml= name.replace(".yaml", "")
          get_name= remove_yaml.replace("staging-", "")

          if (get_name.isInteger()) {
            read_yaml = readYaml(file: "values/"+staging_name.name)
              auth_name= read_yaml['migration-helper-ui']['authorityName']
            println auth_name
              if (!auth_name == 'Staging Maintain') {
                  println "ELLOW"
                println staging_name.substring(0, staging_name.name.lastIndexOf('.'))
                  releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
              }
          }
      }
      
}

    
      releases.each { item ->
        println "Hello ${item}"
    }
  }
