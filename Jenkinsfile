// script Pipeline //
node {
    
    stage ("checkout"){
        git url: 'https://github.com/zhyi12/Demo.git'
    }

       stage('Build') {
           try{
               sh 'cd webdemo && ./gradlew build -x test'
           }catch(e){
               echo("Build Failed in Jenkins!")
               throw e
           }       
        }

        stage('Test') {
            try{
                sh 'cd webdemo && ./gradlew test'
            }catch(e){
                echo("Test Failed in Jenkins!")
                throw e
            }
        }
}
