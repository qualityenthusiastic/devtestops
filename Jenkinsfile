pipeline{
    agent any
    tools{
        maven 'Maven'
    }

    stages{
    stage ('checkout')
		{
			steps
			{
				checkout scm
			}
		}
		stage ('Build')
		{
			steps
			{
				bat "mvn install"
			}
		}
		stage ('Unit Testing')
		{
			steps
			{
				bat "mvn test"
			}
		}
		stage ('Sonar Analysis')
		{
			steps
			{
				withSonarQubeEnv(installationName: 'sonar-token-rb')
				{
					bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar"
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
    }
    post{
        success{
            bat "echo success"
        }
    }

}
