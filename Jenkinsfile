node('master') {
    stage('Example') {
        if (env.BRANCH_NAME == 'feature*') {
            echo 'I only execute on the env.BRANCH_NAME branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}