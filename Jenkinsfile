// script Pipeline //
node {
    
    stage ("checkout"){
        git url: 'https://github.com/zhyi12/Demo.git'
    }

       stage('Build') {
           try{
               sh 'cd webdemo && ./gradlew build -x test'
           }catch(e){
               echo "Build failed"
               throw e
           }       
        }

        stage('Test') {
            try{
                sh 'cd webdemo && ./gradlew test'
            }catch(e){
                echo "Test failed"
                throw e
            }
        }
    
            stage('SonarQube analysis') {
            try{
                def sonarqubeScannerHome = tool name:'SonarScannerTest'
                    withSonarQubeEnv('SonarSeverTest') {
                        sh "${sonarqubeScannerHome}/bin/sonar-scanner"
                    }
                
                timeout(4) {
                   def qg = waitForQualityGate() 
                       if (qg.status != 'OK') {
                           error "It doesn't pass Sonarqube scanner gate setting，Please fix it！failure: ${qg.status}"
                       }
                    }
            }catch(e){
                echo "SonarQube failed"
                throw e
            }
        }
    
            stage('Deploy') {
            try{
                sh """
                    set -e
                    ssh tomcat@172.17.0.4 'bash -s' < checktomcatstatus.sh
                    cd /var/jenkins_home/workspace/TestForPipeline/webdemo/build/libs
                    scp webdemo.war tomcat@172.17.0.4:/opt/tomcat/webapps
                    ssh tomcat@172.17.0.4 '
                        cd /opt/tomcat/bin
                        ./startup.sh
                    '
                """
            }catch(e){
                echo "Deploy failed"
                throw e
            }
        }
    
    stage('Run integrate test'){
        try{
            sh 'cd webdemo && ./gradlew integTest'
        }catch(e){
            echo "UI test failed"
            throw e
        }
    }
    
    


        
    

}

