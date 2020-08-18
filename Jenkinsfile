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
            sh label: '', script: '''curl -O http://ec2-68-79-38-105.cn-northwest-1.compute.amazonaws.com.cn:8080/job/code-scanners/lastSuccessfulBuild/artifact/scan_java_bytecode/target/scan_java_bytecode-1.3-jar-with-dependencies.jar

                                     java -Ddburl="jdbc:mysql://root:prisma@localhost:13306/default@default?useSSL=false" -jar scan_java_bytecode-1.3-jar-with-dependencies.jar -i ./ -id 1

                                     curl --location --request POST 'https://ec2-68-79-38-105.cn-northwest-1.compute.amazonaws.com.cn:10443/api/module/logic-modules/auto-define' --insecure --header 'Authorization: Basic YWRtaW46MTIzNCFAIyQ='

                                     curl --location --request GET 'https://ec2-68-79-38-105.cn-northwest-1.compute.amazonaws.com.cn:10443/api/module/logic-modules/metrics' --insecure --header 'Authorization: Basic YWRtaW46MTIzNCFAIyQ=' > report.json
                                     
                                     curl --location --request GET 'https://ec2-68-79-38-105.cn-northwest-1.compute.amazonaws.com.cn:10443/api/module/quality-gate-profile/rule1' --insecure --header 'Authorization: Basic YWRtaW46MTIzNCFAIyQ=' > quality-gate-profile.json'''
         }
      }
   }

   post {
       always{
            dependencyReport allowEmptyResults: true, reportPath: 'report.json', allowQualityGateFail: true
       }
   }
}
