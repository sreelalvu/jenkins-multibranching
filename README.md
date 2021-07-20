**Multi branching check up**

Stage to check the condition
 stages {
        
        stage('README') {
            when {
                branch "multi1" // Select the required branch
            }
            steps {
              sh 'cat README.md'
            }
        
        }
//Can merge with any branch and validate the result.
//The step will work only for the mentioned branch
