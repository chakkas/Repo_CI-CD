/* Jenkinsfile in Groovy */
pipeline {
    options {
    timeout(time: 1, unit: 'HOURS')
}
agent {
    docker {
    image 'hashmapinc/sqitch:snowflake-dev'
    args "-u root -v /var/run/docker.sock:/var/run/docker.sock --entrypoint=''"
    }
}
stages {
    stage('Installing Latest snowsql') {
        steps {
            sh 'snowsql --help'
        }
    }
    stage('SnowSQL Status ') {
    steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'snowflake_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh '''
            sqitch status "db:snowflake://$USERNAME:$PASSWORD@tailoredbrandsdev.us-east-1.snowflakecomputing.com/flipr?Driver=Snowflake;warehouse=DEMO_WH"
            '''
        }
    }
    }
    stage('Deploy changes') {
    steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'snowflake_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh '''
            sqitch deploy "db:snowflake://$USERNAME:$PASSWORD@tailoredbrandsdev.us-east-1.snowflakecomputing.com/flipr?Driver=Snowflake;warehouse=DEMO_WH"
            '''
        }
    }
    }
    stage('Verify changes') {
    steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'snowflake_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh '''
            sqitch verify "db:snowflake://$USERNAME:$PASSWORD@tailoredbrandsdev.us-east-1.snowflakecomputing.com/flipr?Driver=Snowflake;warehouse=DEMO_WH"
            '''
        }
    }
    }
}
post {
    always {
    sh 'chmod -R 777 .'
    }
}
}
    
