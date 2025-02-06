pipeline {
    agent any
    environment {
        // สุ่มพอร์ตจากช่วง 1000-20000
        RANDOM_PORT = sh(script: "shuf -i 1000-20000 -n 1", returnStdout: true).trim()
    }
    stages {
        stage("Copy file to Docker server") {
            steps {
                // ใช้คำสั่ง SCP ส่งไฟล์จาก Jenkins ไปที่ Docker server
                sh "scp -r /var/lib/jenkins/workspace/66022770/* root@43.208.241.236:/home/root/66022770"
            }
        }
        
        stage("Build Docker Image") {
            steps {
                // เรียกใช้ Ansible playbook สำหรับสร้าง Docker image
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/66022770/playbooks/build.yaml'
            }    
        } 
        
        stage("Create Docker Container") {
            steps {
                // เรียกใช้ Ansible playbook สำหรับ Deploy Docker container
                // ใช้ RANDOM_PORT ที่สุ่มได้ในการตั้งค่า
                ansiblePlaybook playbook: '/var/lib/jenkins/workspace/66022770/playbooks/deploy.yaml',
                                  extraVars: [port: "${RANDOM_PORT}"]
            }    
        } 
    }
}
Updated playbooks/build.yaml:
YAML
- name: Build Docker Image
  hosts: dockerservers
  gather_facts: false
  remote_user: root
  tasks:
    - name: Building Docker Image
      docker_image:
         name: 66022770:latest # docker image name
         source: build
         build:
            path: /home/root/66022770 # path to source code
         state: present
         force_source: true
Updated playbooks/deploy.yaml:
YAML
- name: Deploy Docker Container
  hosts: dockerservers
  gather_facts: false
  remote_user: root
  tasks:
    - name: Creating the Container
      docker_container:
        name: 66022770-container
        image: 66022770:latest
        ports:
          - "{{ port }}:80"  # ใช้ค่าพอร์ตที่ส่งจาก Jenkins
        state: started
        restart: true
