podTemplate(
    label: 'apipoi',
    containers: [
        containerTemplate( name: 'dotnetbuild', image: 'microsoft/dotnet:2.1-sdk', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'docker', image: 'docker:18-dind', ttyEnabled: true, command: 'cat'),
    ],
    volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
    ],
    envVars: [
        envVar(key: 'TEMPPOI', value: 'changemelater')
    ]
) {
    node('apipoi'){
        stage('API-Poi-CI') {
            git 'https://github.com/OguzPastirmaci/openhack-devops-team.git'
            container('dotnetbuild') {
                stage('Build and Test POI API') {
                    sh """
                    cd apis/poi/web
                    dotnet restore
                    dotnet build
                    dotnet publish -c Release -o out
                    """
                }
            }
            container('docker'){
                stage('Docker Build POI API') {
                    sh """
                    #docker build ./apis/poi/web -t poi
                    """
                }
            }
        }

    }
}
podTemplate(
    label: 'apiuser',
    containers: [
        containerTemplate( name: 'nodealpine', image: 'node:8-alpine', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'docker', image: 'docker:18-dind', ttyEnabled: true, command: 'cat'),
    ],
    volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
    ],
    envVars: [
        envVar(key: 'TEMPUSER', value: 'changemelater')
    ]
) {
    node('apiuser'){
        stage('API-User-CI') {
            git 'https://github.com/OguzPastirmaci/openhack-devops-team.git'
            container('nodealpine') {
                stage('Build and Test User API') {
                    sh """
                    cd apis/userprofile
                    npm install
                    #npm run test
                    """
                }
            }
            container('docker'){
                stage('Docker Build User API') {
                    sh """
                    docker build ./apis/userprofile -t user
                    """
                }
            }
        }

    }
}
podTemplate(
    label: 'apitrips',
    containers: [
        containerTemplate(name: 'gobuildci', image: 'golang:1.10.1', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'docker', image: 'docker:18-dind', ttyEnabled: true, command: 'cat'),
    ],
    envVars: [
        envVar(key: 'TEMPTRIPS', value: 'changemelater')
    ]
) {
    node('apitrips') {
        stage('API-Trips-CI') {
            git 'https://github.com/OguzPastirmaci/openhack-devops-team.git'
            container('gobuildci') {
                stage('Build and Test Trips API') {
                    sh """
                    mkdir /go/src/github.com/Azure-Samples/openhack-devops-team -p
                    cp -R . /go/src/github.com/Azure-Samples/openhack-devops-team
                    cd /go/src/github.com/Azure-Samples/openhack-devops-team/apis/trips
                    ls
                    curl https://glide.sh/get | sh
                    glide install
                    #go test ./test
                    """
                }
            }
            container('docker'){
                stage('Docker Build Trips API') {
                    sh """
                    #docker build ./apis/trips -t trips
                    """
                }
            }
        }
    }
}
