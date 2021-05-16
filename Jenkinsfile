def remote = [name: 'k8 master', host: '172.31.13.36', user: 'ubuntu', password: 'apple@2021', allowAnyHosts: true]
pipeline {
  
  agent { label "java"}
   
  stages{
    stage('Put helm onto k8smaster'){
      steps{         
         sshPut remote: remote, from: 'spring-petclinic', into: '.'
      }  
    }
    
    stage("Deploy To Kubernetes"){      
      steps {        
        script{                 
         sshCommand remote: remote, command: 'sudo helm install --generate-name spring-petclinic'             
        }      
      }    
    }
    stage('Run Kubectl Command '){
      steps{        
         sshCommand remote: remote, command: "sudo kubectl get pods"
      }
    }
  
  }

}
