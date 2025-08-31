pipeline{
    agent any
    stages{
        stage('Running Test via Docker'){
            steps{
                bat "docker-compose up"
            }

        }

        stage('Grid Down'){
            steps{
                bat "docker-compose down"
            }

        }
      

    }
   
}