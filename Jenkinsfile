#!/usr/bin/env groovy
import static groovy.io.FileType.*

// def releases = ['staging-shared', 'staging-800', 'staging-801', 'staging-802']

def releases = []

def exclude_list = [
  "staging-803.yaml"
]
println "hi"

node {
sh 'pwd > workspace'
env.WORKSPACE = readFile('workspace').trim()
def workspace = "${env.WORKSPACE}"
println "${env.WORKSPACE}"

println "hi"
sh("ls -A1 ${workspace}")

}

def fileList = "ls "+workspace.execute()
println fileList





  new File(workspace+"/values/").traverse(type: FILES, nameFilter: ~/staging-/) { staging_name ->
      println "hi"
      if (!exclude_list.contains(staging_name)){
          remove_yaml= staging_name.replace(".yaml", "")
          println remove_yaml
          get_name= remove_yaml.replace("staging-", "")
          if (get_name.isInteger()) {
              def read_yaml = readYaml file: "./values/"+ staging_name
              if (config.migration-helper-ui && !config.migration-helper-ui.authorityName == 'Staging Maintain') {
                  releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
              }
          }
      }
}
