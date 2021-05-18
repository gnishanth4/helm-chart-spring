def remote = [name: 'k8 master', host: '172.31.13.36', user: 'ubuntu', password: 'apple@2021', allowAnyHosts: true]
pipeline {
  
  agent { label "java"}
  options {      
    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
    }
   
  stages{
    stage('Put HelmChart onto k8smaster'){
      steps{         
         sshPut remote: remote, from: 'spring', into: '.'
      }  
    }
    
    stage("Deploy To Kubernetes"){      
      steps {        
        script{ 
          try {
            sshCommand remote: remote, command: 'helm delete petclinic' 
          }
          catch(error){
             sshCommand remote: remote, command: 'sudo helm install petclinic spring --dry-run' 
             sshCommand remote: remote, command: 'sudo helm install petclinic spring' 
          }     
            
        }      
      }    
    }
    stage("Run Kubectl Command"){
      steps{        
         sshCommand remote: remote, command: "sudo kubectl get pods"
      }
    }
  
  }

}
