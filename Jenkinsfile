pipeline{
    agent any
    	environment {
		notifyEmail ="raghav.bajoria@nagarro.com"
	}
    tools{
        maven 'Maven'
    }
    stages{
        stage("code checkout")
        {
            steps{
            bat "echo hello"
            }
        }
        stage("code build")
        {
            steps
            {
            bat "mvn clean"
            }
        }
        stage("unit test"){
            steps{
            bat "mvn test"
            }
        }
        stage("Sonar Analysis")
        {
                    steps{
                    withSonarQubeEnv("sonar-token-rb")
                        {
                    bat "echo Sonar Run half"
                                bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.0.2131:sonar"
                        }
                    }
                }
         }
         stage ('Upload to Artifactory')
         {
            steps
            {
               rtMavenDeployer (
                           id: 'deployer',
                           serverId: 'Raghav123',
                           releaseRepo: 'RaghavRepo',
                           snapshotRepo: 'RaghavRepo'
                       )
                       rtMavenRun (
                           pom: 'pom.xml',
                           goals: 'clean install',
                           deployerId: 'deployer',
                       )
                       rtPublishBuildInfo (
                           serverId: 'Raghav123',
                       )
            }
         }
    post{
        success{
            bat "echo success"
            }
        }
}
