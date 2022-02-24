#!/usr/bin/env groovy
import static groovy.io.FileType.*


// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']
node {
  
  stage "checkout repo"
  
  checkout scm 
  
  stage "stuff"
  
def exclude_list = [
  "staging-051",
  "staging-036",
  "staging-052",
  "staging-038",
  "staging-039",
  "staging-029",
  "staging-031"
]

sh 'pwd > workspace'
env.WORKSPACE = readFile('workspace').trim()
def workspace = "${env.WORKSPACE}"
  
    releases = []
  
      
    File folder = new File(workspace+"/values/")  
    File[] listOfFiles = folder.listFiles();
    
    for (File file : listOfFiles) {

      name = file.getName()
      if (name.contains("staging-")){
        println name
        if (!exclude_list.contains(name)){
            remove_yaml= name.replace(".yaml", "")
            get_name= remove_yaml.replace("staging-", "")
            if (get_name.isInteger()) {
              // this checks the number DOES NOT match 8, in prod only
              check_number = Integer.parseInt(get_name.substring(0, 1))
              if (check_number != 8){
                read_yaml = readYaml(file: "values/"+file.getName())
                auth_name= read_yaml['migration-helper-ui']['authorityName']
                if (auth_name != 'Staging Maintain') {
                    releases << file.getName().replace(".yaml", "")
                }
             }
          }
      }
   }
}

      println "PRINT OUTPUT"
      releases.each { item ->
        println "${item}"
    }
  }
