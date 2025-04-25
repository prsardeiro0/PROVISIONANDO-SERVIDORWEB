# PROVISIONANDO-SERVIDORWEB
 provisionar um servidor web automaticamente
---
- hosts: all
  become: yes
  tasks:
    - name: Atualizar pacotes do sistema
      apt:
        update_cache: yes

    - name: Instalar o servidor web Apache
      apt:
        name: apache2
        state: present

    - name: Habilitar e iniciar o serviço Apache
      service:
        name: apache2
        enabled: yes
        state: started

    - name: Criar página web padrão
      copy:
        dest: /var/www/html/index.html
        content: |
          <!DOCTYPE html>
          <html>
          <head>
            <title>Servidor Web Provisionado!</title>
          </head>
          <body>
            <h1>Olá! Este servidor web foi provisionado automaticamente com Ansible.</h1>
          </body>
          </html>

    - name: Abrir porta 80 no firewall UFW
      ufw:
        rule: allow
        port: '80'
      when: ansible_distribution == "Ubuntu"
