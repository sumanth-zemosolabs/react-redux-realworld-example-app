pipeline{
    agent {
      kubernetes {
        yaml '''
          apiVersion: v1
          kind: Pod
          metadata:
            name: node
            labels:
              name: node
          spec:
            containers:
            - name: node
              image: node:20-alpine
              command: 
              - cat
              tty: true
            - name: docker
              image: docker
              securityContext:
                privileged: true
              command:
              - cat
              tty: true
              volumeMounts:
              - mountPath: /var/run/docker.sock
                name: docker-sock
            volumes:
            - name: docker-sock
              hostPath:
                path: /var/run/docker.sock
        '''
      }
    }

    stages{
        stage(build) {
          steps {
            container('node') {
              sh 'node --version'
              sh 'npm install'
              sh 'npm run build'
            }
          }
        }
        stage(docker){
            steps{
                container('docker'){
                    sh 'docker info'
                    sh 'docker build -t react .'
                    sh 'docker images'
                }
            }
        }
    }
}