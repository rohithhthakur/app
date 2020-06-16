pipeline {
  agent {
    kubernetes {
      label 'kaniko'
      yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: jnlp
    workingDir: /tmp/jenkins
  - name: kaniko
    workingDir: /tmp/jenkins
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: dockerhub
          items:
            - key: .dockerconfigjson
              path: config.json
"""
    }
  }
  stages {
    stage('Building image') {
      environment {
        PATH = "/busybox:/kaniko:$PATH"
      }
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') { 
          sh '''#!/busybox/sh
            echo "Building docker image for my app..."
            /kaniko/executor --context `pwd` --verbosity debug --destination rohith4756/myapp:v1
          '''
        }
      }
    }
 stage('Deploy to stage'){
       when { not { branch 'master' }}
        environment {
        PATH = "/busybox:/kaniko:$PATH"
      }
         
       steps {
            container(name: 'kaniko', shell: '/busybox/sh') {
          sh 'Hello  world'
      }}}
  }
}
