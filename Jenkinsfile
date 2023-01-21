node {
	def app
	stage('Clone') {
		checkout scm
	}

	stage('Build') {
		app = docker.build("19120179/devops_demo")
	}

	stage('Test') {
		app.inside {
			sh "docker exec --tty node-server curl http://localhost:8000/test"
		}
	}

	stage('Push') {
		docker.withRegistry("https://registry.hub.docker.com", "Docker") {
			app.push("${env.BUILD_ID}") // push images with new tag to docker hub
			app.push("latest") // change the latest tag to it
		}
    }

	stage('Deploy'){
        try {
            sh "docker kill devops_demo"
        }
        catch (exe){}
        finally {
            app.run("--rm --name devops_demo -p 8081:3000")
        }
	}
	
	
}