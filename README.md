# Deploy-Jfrog-Docker with Ansible
---
- name: Install JFrog Artifactory in a Docker container with port mapping
  hosts: web
  become: true
  vars:
    container_name: jfrog_artifactory
    image_name: docker.bintray.io/jfrog/artifactory-oss:latest
    #container_ports: 8081
    #host_port: 8888
    container_ports:
      - "8081:8081"
      - "8082:8082"
  tasks:
    - name: Install Docker
      package:
        name: docker-ce
        state: present

    - name: Start Docker service
      become: true
      systemd:
        name: docker
        state: started
        enabled: yes

    - name: Install pip
      become: true
      yum:
        name: python3-pip
        state: present

    - name: Install Docker Python module
      pip:
        name: docker
        state: present

    - name: Pull JFrog Artifactory Docker image
      docker_image:
        name: "{{ image_name }}"
        source: pull
    
    - name: Create JFrog Artifactory container with port mapping
      docker_container:
        name: "{{ container_name }}"
        image: "{{ image_name }}"
        published_ports: "{{ container_ports }}"
        env:
          ARTIFACTORY_HOME: /artifactory
        volumes:
          - "{{ playbook_dir }}/artifactory-data:/artifactory"
        state: started
        restart_policy: unless-stopped
