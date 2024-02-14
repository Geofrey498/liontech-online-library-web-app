node{
    def mavenHome = tool name: 'maven3.9.6'
    stage('1clonecode'){
        git 'https://github.com/Geofrey498/liontech-online-library-web-app.git'

    }

    stage('2Test&Buildapp'){
        sh   "${mavenHome}/bin/mvn  clean package"
    }

    stage('3codequality'){
        sh "${mavenHome}/bin/mvn     sonar:sonar"
    }
    
    stage('4UploadArtifacts'){ sh "${mavenHome}/bin/mvn     deploy"
    }

    stage('5deploy2DEVEnv'){
        sh "echo 'deploying to dev environment' "
        deploy adapters: [tomcat9(credentialsId: 'tomcat001', path: '', url: 'http://13.56.234.238:8009/')], contextPath: 'tomcat0e1', war: 'target/*.war'
    }
    
    stage('6DeploytoTestENV'){
        sh "echo   'deploy to Test Environment' "
        deploy adapters: [tomcat9(credentialsId: 'tomcat001', path: '', url: 'http://13.56.234.238:8009/')], contextPath: 'test', war: 'target/*.war'
        
        }
    stage('7Deploy2UAT'){
    sh "echo 'deploy to UAT or Staging Environment' "
    deploy adapters: [tomcat9(credentialsId: 'tomcat001', path: '', url: 'http://13.56.234.238:8009/')], contextPath: 'Staging', war: 'target/*.war'

    }
    stage('8Approval$Reviewprep4prod'){
    sh "echo 'application is ready for review and prod release' "
    timeout[time:5, unit 'DAYS']{input message:"App is ready for release to prod, please review and signup"
    }
    }
    

    stage("9DEPLOY2PROD"){
        sh "sleep 50"
        deploy adapters: [tomcat9(credentialsId: 'tomcat001', path: '', url: 'http://13.56.234.238:8009/')], contextPath: 'Production', war: 'target/*.war'


    }
    stage('10congratulationsforrelease'){
        echo 'congratulations for LMS v1 prod release'
    }
    stage('sendMS Team Notifications'){
        echo 'JJ-PROJECT RELEASE TO PROD SUCCESSFUL'
    }




}