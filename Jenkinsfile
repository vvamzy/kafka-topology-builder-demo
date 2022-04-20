pipeline {

    agent {
      docker { image 'purbon/kafka-topology-builder:1.5.0' }
    }

   stages {
      stage('verify-replication-factor') {
         steps {
             sh 'checks/verify-replication-factor.sh ${TopologyFiles} 3'
             sh 'whoami'
         }
      }
      stage('verify-num-of-partitions') {
         steps {
            sh 'checks/verify-num-of-partitions.sh ${TopologyFiles} 12'
         }
      }
      stage('run') {
          steps {
              withCredentials([usernamePassword(credentialsId: 'confluent-cloud	', usernameVariable: 'EGX7X7F74BKI5Y5W', passwordVariable: 'RuLF+cmC4avl++tUezJjgtGj3szcIEitYcy15FzJGiUOLFKcZNpVazK6FU9RWfYa')]) {
                sh './demo/build-connection-file.sh > topology-builder.properties'
              }
              sh 'kafka-topology-builder.sh  --brokers ${Brokers} --clientConfig topology-builder.properties --topology ${TopologyFiles} --allowDelete'
          }
      }
   }
}
