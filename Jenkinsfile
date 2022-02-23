#!/usr/bin/env groovy
import static groovy.io.FileType.*


// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']
node {
  
  stage "checkout repo"
  
  checkout scm 
  
  stage "stuff"
  
def releases = []

def exclude_list = [
  "staging-803.yaml"
]
println "hi"


sh 'pwd > workspace'
env.WORKSPACE = readFile('workspace').trim()
def workspace = "${env.WORKSPACE}"
println "${env.WORKSPACE}"
println "BLERRR"
  

  new File(workspace+"/values/").traverse(type: groovy.io.FileType.FILES) { staging_name ->
      println "hi"
      def name = staging_name.name
      if (!exclude_list.contains(name)){
          remove_yaml= name.replace(".yaml", "")
          println remove_yaml
          get_name= remove_yaml.replace("staging-", "")
          println get_name
          println get_name.isInteger()
        println staging_name
          if (get_name.isInteger()) {
            read_yaml = readYaml(file: "values/"+staging_name.name)
             println read_yaml
            println read_yaml['migration-helper-ui']
            
              if (['read_yaml.migration-helper-ui'] && !read_yaml['migration-helper-ui']['authorityName'] == 'Staging Maintain') {
                  releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
              }
          }
      }
}
      releases.each { item ->
        println "Hello ${item}"
    }
  }
