#!/usr/bin/env groovy
import static groovy.io.FileType.*
@Grab('org.yaml:snakeyaml:1.17')

import org.yaml.snakeyaml.Yaml

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
          if (get_name.isInteger()) {
                Yaml parser = new Yaml()
               List example = parser.load(("example.yaml" as File).text)
    
               example.each{println it.subject}
              def read_yaml = new YamlSlurper().parseText(orkspace+"/values/"+ staging_name)
              if (config.migration-helper-ui && !config.migration-helper-ui.authorityName == 'Staging Maintain') {
                  releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
              }
          }
      }
}
  }
