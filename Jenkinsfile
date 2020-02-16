@Library('my-shared-library') _
//This pipeline is to test the functions in this repo

pipeline {
  agent any
  
  stages {
    stage('init') {
      steps {
        echo "This is init"
        echo "Hoping that the getStageLog function will capture only this stage output"
      }
    }
    stage('test loadProperties') {
      steps {
        echo "Testing loadProperties function"
        echo ""
        echo "Env vars before test:"
        sh 'env'
        echo ""
        sh 'echo NAME=mr-anderson > env.properties'
        sh 'echo MY_VAR=my-val >> env.properties'
        sh 'echo SOME_VERSION=1.2.3 >> env.properties'
        sh 'echo MY_NUM=10 >> env.properties'
        loadProperties("${WORKSPACE}/env.properties")
        echo "Testing loaded vars..."
        echo "NAME = ${NAME}"
        sh "echo MY_VAR = ${MY_VAR}"
        sh "echo SOME_VERSION = ${env.SOME_VERSION}"
        sh 'echo MY_NUM = ${MY_NUM}'
        echo "Testing loadProperties done."
        echo ""
        echo "Env vars after test:"
        sh 'env'
      }
    }
    stage('test getStageLog') {
      steps {
        echo "Testing getStageLog function,"
        echo "showing output only from 'init' stage..."
        echo getStageLog('init')
        echo "Testing getStageLog done."
      }
    }
    stage('test parseJson') {
      steps {
        echo "Testing parseJson function..."
        script {
          def listTest = '"myList": [4, 8, 15, 16, 23, 42]'
          def intText = '"number": 123'
          def stringText = '"name": "John Doe"'
          def jsonText = '{ ' + listTest + ', ' + intText + ', ' + stringText + ' }'
          echo "jsonText = ${jsonText}" 
          
          def myMap = parseJson(jsonText)
          echo "After parsing to json:"
          assert myMap instanceof Map
          assert myMap.myList instanceof List
          assert myMap.number == 123
          assert myMap.name == 'John Doe'
          echo "myMap.name = ${myMap.name}"
          echo "myMap.number = ${myMap.number}"
          echo "myMap.myList = ${myMap.myList}"
          
          def myJson = parseJson.toJson(jsonText)
          echo "Testing prettyPrint..."
          parseJson.prettyPrint(myJson)
        }
      }
    }
  }
}
