# FTP-server

![FTP](https://user-images.githubusercontent.com/47100064/173203259-d3aa8b92-06df-4402-8fc6-bbaec6a9cf7a.jpeg)

Un serveur FTP est une application qui permet le transfert de fichiers d’un ordinateur à un autre ou  à n’importe quel ordinateur dans le monde qui est connecté à Internet.

## Pré-requis

Pour configurer un serveur FTP, on a besoin d’un serveur hybride, cloud ou dédié sur ServerMania, sur ServerMania.com

## INSTALLATION

- Il faut se connecter au serveur via ssh

`ssh ubuntu@SERVER-IP`

- se connecter en tant qu'utilisateur root

`sudo su`

- Installation de VSFTPD

`apt-get install vsftpd -y`

- Configurer VSFTPD pour qu'il démarre automatiquement

`systemctl start vsftpd`

`systemctl enable vsftpd`

- Création d'utilisateur pour FTP

`adduser vsftp`

- Création et configuration de permission du fichier FTP

`mkdir /home/vsftp/ftp`

`chown nobody:nogroup /home/vsftp/ftp`

`chmod a-w /home/vsftp/ftp`

- Création d'un fichier d'upload et configuration de permission

`mkdir /home/vsftp/ftp/test`

`chown vsftp:vsftp /home/vsftp/ftp/test`

## CONFIGURATION

- Création d'un fichier de backup

`cp /etc/vsftpd.conf /etc/vsftpd.conf.bak`

- Mis à jour du fichier de configuration

`nano /etc/vsftpd.conf`

- Ajouter les lignes suivantes:

        listen=NO
    
        listen_ipv6=YES
    
        anonymous_enable=NO
    
        local_enable=YES
    
        write_enable=YES
    
        local_umask=022
    
        dirmessage_enable=YES
    
        use_localtime=YES
    
        xferlog_enable=YES
    
        connect_from_port_20=YES
    
        chroot_local_user=YES
    
        secure_chroot_dir=/var/run/vsftpd/empty
    
        pam_service_name=vsftpd
    
        pasv_enable=Yes
    
        pasv_min_port=10000
    
        pasv_max_port=11000
    
        user_sub_token=$USER
    
        local_root=/home/$USER/ftp
    
        userlist_enable=YES
    
        userlist_file=/etc/vsftpd.userlist
    
        userlist_deny=NO
    
        rsa_cert_file=/etc/cert/vsftpd.pem
    
        rsa_private_key_file=/etc/cert/vsftpd.pem
    
        ssl_enable=YES
    
        allow_anon_ssl=NO
    
        force_local_data_ssl=YES
        
        force_local_logins_ssl=YES
    
        ssl_tlsv1=YES
    
        ssl_sslv2=NO
    
        ssl_sslv3=NO
    
        require_ssl_reuse=NO
    
        ssl_ciphers=HIGH
       
- Création de certification pour connection via SSL

`mkdir /etc/cert`

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/cert/vsftpd.pem -out /etc/cert/`

`vsftpd.pem`

- Redémarrer VSFTP

`systemctl restart vsftpd`

- Connection au serveur FTP

`ftp://VOTRE-SERVEUR-IP`

Afin de télécharger des fichiers, vous pouvez utiliser un client FTP tel que Filezilla
 

