podTemplate(
    label: 'apici',
    containers: [
        containerTemplate(
          name: 'gobuildci',
          image: 'golang:1.10.1',
          ttyEnabled: true,
          command: 'cat'
        ),
        containerTemplate(
          name: 'nodealpine',
          image: 'node:8-alpine',
          ttyEnabled: true,
          command: 'cat'
        ),
    ],
    envVars: [
        envVar(key: 'TEMP', value: 'changemelater')
    ]
    ) {

    node('apici') {
        stage('API-Trips-CI') {
            git 'https://github.com/OguzPastirmaci/openhack-devops-team.git'
            container('gobuildci') {
                stage('Build and Test Trips API') {
                    sh """
                    cp -R . /go/src/github.com/Azure-Samples/openhack-devops-team
                    cd /go/src/github.com/Azure-Samples/openhack-devops-team/apis/trips
                    ls
                    curl https://glide.sh/get | sh
                    glide install
                    go test ./test
                    """
                }
                stage('Docker Build Trips API') {
                    sh """
                    docker build ./apis/trips -t trips
                    """
                }
            }
        }

        stage('API-User-CI') {
            git 'https://github.com/OguzPastirmaci/openhack-devops-team.git'
            container('nodealpine') {
                stage('Build and Test User API') {
                    sh """
                    npm install apis/userprofile
                    npm run test --prefix apis/userprofile
                    """
                }
                stage('Docker Build User API') {
                    sh """
                    docker build ./apis/userprofile -t user
                    """
                }
            }
        }

    }
}