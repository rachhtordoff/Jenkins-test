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

pipeline {
  agent {
    kubernetes {
      label 'helm-v3'
      idleMinutes 10
      yaml libraryResource('uk/gov/landregistry/jenkins/pipeline/application/podtemplate_helm.yaml')
    }
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: buildsToKeepStr, artifactNumToKeepStr: buildsToKeepStr))
    disableConcurrentBuilds()
    ansiColor('xterm')
  }

  parameters {
    gitParameter(name: 'GIT_REF', branchFilter: 'origin/(.*)', defaultValue: null, type: 'PT_TAG', sortMode: 'DESCENDING_SMART', selectedValue: 'TOP', description: 'Version to release')
    booleanParam(name: 'DRY_RUN_ONLY', defaultValue: false, description: 'Only do a dry run of helm upgrade')
  }


  stages {
    stage('Run Shell Command') {

      steps {
        script {
          releases.each { release ->

            println "Deploying release ${release}"

            def jobParams = [
                    string(name: 'FORMAL_RELEASE', value: release),
                    string(name: 'GIT_REF', value: params.GIT_REF),
                    booleanParam(name: 'DRY_RUN_ONLY', value: params.DRY_RUN_ONLY)
            ]

            build job: 'LLC/(Staging) llc-staging-live-deployment (specific)',
                    propagate: true,
                      wait: true,
                      parameters: jobParams

          }
        }
      }

    }
  }
}
