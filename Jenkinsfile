pipeline {
   agent any

   stages {
      stage('build') {
         steps {
               sh label: '', script: '''./mvnw clean compile'''
         }
      }
      stage('analysis') {
         steps {
            sh label: '', script: '''java -Ddburl="jdbc:mysql://root:prisma@localhost:13306/default@default?useSSL=false" -jar scan_java_bytecode-1.2-jar-with-dependencies.jar ./
                            curl --location --request GET \'https://localhost:10443/api/module/logic-modules/coupling-detail\' --insecure --header \'Authorization: Basic YWRtaW46MTIzNCFAIyQ=\' > report.json
                            '''
         }
      }
   }

   post {
       always{
            dependencyReport 'report.json'
       }
   }
}
