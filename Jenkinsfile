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
          name: 'busybox',
          image: 'busybox:latest',
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
            container('busybox') {
                stage('Build and Test Trips API') {
                    sh """
                    echo
                    """
                }
            }
        }

    }
}