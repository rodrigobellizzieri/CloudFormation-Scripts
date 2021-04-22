# CloudFormation-Scripts
 Script para criação de uma maquina Freetier t2.micro, ja com SecurityGroup pré-existente

##Script para criar instancia FreeTier t2.micro <h2>

**Altere os campos comentados**

~~~
Resources:
  myEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      KeyName: Linux #Adicione aqui a Security Key que será utilizada
      DisableApiTermination: false #Defina com true/false se deseja bloquear o terminator
      ImageId: ami-05d72852800cbf29e #Selecione a imagem desejada
      InstanceType: t2.micro #Selecione o tipo de recursos
      Monitoring: false #Defina com true/false se deseja habilitar monitoria
      SecurityGroupIds: 
        - All #Coloque o nome do Security Group, no meu caso é All
      UserData: !Base64 | #!Base64 define o tipo de arquitetura da maquina, e abaixo é um shellscript meu
        #!/bin/bash -ex
        sudo su 
        yum update -y 
        yum install httpd -y 
        systemctl start httpd
        chkconfig httpd on
        cd /var/www/html
        touch index.htmk
        echo "Ola Mundo - Servidor OK" > index.html
      Tags:
        - Key: Name #Tag para definir onde sera inserido a value
          Value: WebServer-CloudFormation #Nome do servidor
        
~~~