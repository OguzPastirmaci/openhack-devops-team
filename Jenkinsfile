podTemplate(
    label: 'apipoi',
    containers: [
        containerTemplate( name: 'dotnetbuild', image: 'microsoft/dotnet:2.1-sdk', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'az', image: 'microsoft/azure-cli:2.0.41', ttyEnabled: true, command: 'cat'),
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
            container('az'){
                stage('Docker Build POI API') {
                    withCredentials([azureServicePrincipal('otaprdspn')]) {
                    sh """
                    az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                    az acr build --registry devopsoh336sub3acr -f Dockerfile --image apiuser ./apis/poi/web
                    """
                    }
                }
            }
        }

    }
}
podTemplate(
    label: 'apiuser',
    containers: [
        containerTemplate( name: 'nodealpine', image: 'node:8-alpine', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'az', image: 'microsoft/azure-cli:2.0.41', ttyEnabled: true, command: 'cat'),
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
                    npm run test
                    """
                }
            }
            container('az'){
                stage('Docker Build User API') {
                    withCredentials([azureServicePrincipal('otaprdspn')]) {
                    sh """
                    az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                    az acr build --registry devopsoh336sub3acr -f Dockerfile --image apiuser ./apis/userprofile
                    """
                    }                    
                }
            }
        }

    }
}
podTemplate(
    label: 'apitrips',
    containers: [
        containerTemplate(name: 'gobuildci', image: 'golang:1.10.3', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'az', image: 'microsoft/azure-cli:2.0.41', ttyEnabled: true, command: 'cat'),
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
                    go test ./test
                    """
                }
            }
            container('az'){
                stage('Docker Build Trips API') {
                    withCredentials([azureServicePrincipal('otaprdspn')]) {
                    sh """
                    az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                    az acr build --registry devopsoh336sub3acr -f Dockerfile --image apitrips ./apis/trips
                    """
                    }                    
                }
            }
        }
    }
}
