#!/usr/bin/env groovy
import groovy.yaml.YamlSlurper
import static groovy.io.FileType.*


def buildsToKeepStr = "60"

def exclude_list = [
  "staging-051.yaml",
  "staging-036.yaml",
  "staging-052.yaml",
  "staging-038.yaml",
  "staging-039.yaml",
  "staging-029.yaml",
  "staging-031.yaml"
]

def releases = []

new File("./values/").traverse(type: FILES, nameFilter: ~/staging-/) { staging_name ->
    if (!exclude_list.contains(staging_name)){
      def read_yaml = new YamlSlurper().parseText(staging_name)
      if (!read_yaml.authorityName == 'Staging Maintain'){
        releases << staging_name.substring(0, staging_name.name.lastIndexOf('.'))
      }
    }
}
