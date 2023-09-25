pipeline {
    agent {
        node {
            label 'slave2'
        }
    }
    parameters {
    	string(name: 'serverIP', defaultValue: 'None', description: 'Enter Server IP ')
	string(name: 'servername', defaultValue: 'None', description: 'Enter Ansible slave name ')
	password(name: 'raghudocker', description: 'Enter docker login password ')	    
    }
    stages {
        stage('SCM checkout'){
            steps {
		git "https://github.com/raghu-chikka/devopsIQ.git"
            }
	}
	stage('Remove dockers'){
	    steps {
		sh "if [ `sudo docker ps -a -q|wc -l` -gt 0 ]; then sudo docker rm -f \$(sudo docker ps -a -q);fi"
		}
	}
	stage('Build'){
	    steps {
		    sh "sudo docker build /home/ubuntu/jenkins/workspace/${JOB_NAME} -t raghudocker1206/devopsdemo1"
	   }
	}
	stage('Docker Push'){
		steps {
		    sh "sudo docker login --username raghudocker1206 --password ${dockerpass}"
                    sh "sudo docker push raghudocker1206/devopsdemo1:latest"
	        }
	}
	stage('Configure servers with Docker and deploy website') {
            	steps {
			sh 'ansible-playbook docker.yaml -e "hostname=${servername}"'
            	}
        }
	stage('Install Chrome browser') {
            	steps {
                	sh 'ansible-playbook chrome.yaml -e "hostname=localhost"'
            	}
        }
	stage ('Testing'){
		steps {
			sh "sudo apt install python3-pip -y"
			sh "pip3 install selenium"
			sh "python3 selenium_test.py ${params.serverIP}"
		}
	}
    }
}
