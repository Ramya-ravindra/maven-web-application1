pipeline
{
	agent any
def mavenHome = tool name: "maven 3.8.2"
stages
{

	

		stage('getting src')
		{
			steps
			{
			git branch: 'development1', credentialsId: '790d9d01-86d2-48df-b2a9-72f18105394b', url: 'https://github.com/Ramya-ravindra/maven-web-application1.git'

			}
		}

	stage('Build Articraft')
		{
steps
{
			sh "${mavenHome}/bin/mvn clean package"
}
		}
	stage('sonar report')
		{
steps
{
			sh "${mavenHome}/bin/mvn clean package sonar:sonar"
		}
}
	stage('ExecuteSonarQubeReport')
		{
steps
{
			sh "${mavenHome}/bin/mvn clean package sonar:sonar"
}
		}


	}
	post
	{
	always
		{
		emailext body: '''regards
		ramya''', subject: 'build status', to: 'ramyahema73@gmail.com'
		}
	}
}


