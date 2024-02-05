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
            - name: docker-daemon
              image: docker:dind
              securityContext:
                privileged: true
              tty: true
              volumeMounts:
              - name: docker-sock
                mountPath: /var/run/
            - name: node
              image: node:20-alpine
              command: 
              - cat
              tty: true
            - name: docker
              image: docker
              command:
              - cat
              tty: true
              volumeMounts:
              - mountPath: /var/run/
                name: docker-sock
            volumes:
            - name: docker-sock
              emptyDir: {}
        '''
      }
    }

    stages{
        // stage(clone){
        //     steps{
        //         container(git){
        //             git branch: 'deployment', credentialsId: 'jenkins github pat', url: 'https://github.com/sumanth-zemosolabs/react-redux-realworld-example-app.git'
        //         }
        //     }
        // }
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