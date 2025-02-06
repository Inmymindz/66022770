pipeline {
    agent any
    stages {      
        stage("Copy file to Docker server") {
            steps {
                // ใช้คำสั่ง SCP ส่งไฟล์จาก Jenkins ไปที่ Docker server
                sh "scp -r /var/lib/jenkins/workspace/660220770/* root@43.208.241.236:/home/root/660220770"
            }
        }
        
        stage("Build Docker Image") {
            steps {
                // เรียกใช้ Ansible playbook สำหรับสร้าง Docker image
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/660220770/playbooks/build.yaml'
            }    
        } 
        
        stage("Create Docker Container") {
            steps {
                // เรียกใช้ Ansible playbook สำหรับ Deploy Docker container
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/660220770/playbooks/deploy.yaml'
            }    
        } 
    }
}
