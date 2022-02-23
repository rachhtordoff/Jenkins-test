#!/usr/bin/env groovy
import static groovy.io.FileType.*

// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']

def releases = []

def exclude_list = [
  "staging-803.yaml"
]
println "hi"

def workspace = pwd()

  new File(workspace+"/values/").traverse(type: FILES, nameFilter: ~/staging-/) { staging_name ->
      if (!exclude_list.contains(staging_name)){
          remove_yaml= staging_name.replace(".yaml", "")
          get_name= remove_yaml.replace("staging-", "")
          if (get_name.isInteger()) {
              def read_yaml = readYaml file: "./Jenkinsfiles/"+ staging_name
              if (config.migration-helper-ui && !config.migration-helper-ui.authorityName == 'Staging Maintain') {
                  releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
              }
          }
      }
}
