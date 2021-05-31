# SDGCA Survey web instance deployment instructions

## 1. Requirements
Linux  / macOS 

Python 3.5+

Docker & Docker Compose

Available TCP Ports: 

1. 80 NGINX
2. 443 NGINX (if you use kobo-install with LetsEncrypt proxy)

## 2. Set up SSL for the subdomains
    sudo certbot --nginx -d kf.${PUBLIC_DOMAIN_NAME} -d kc.${PUBLIC_DOMAIN_NAME} -d ee.${PUBLIC_DOMAIN_NAME}

## 3. Nginx configuration
    server {
        listen       80;
        server_name  kf.${PUBLIC_DOMAIN_NAME} kc.${PUBLIC_DOMAIN_NAME} ee.${PUBLIC_DOMAIN_NAME} ;
    }

    server {
        listen 443 ssl;
        server_name kf.${PUBLIC_DOMAIN_NAME} kc.${PUBLIC_DOMAIN_NAME} ee.${PUBLIC_DOMAIN_NAME} ;

        location / {
            proxy_pass      http://localhost:8085;
            proxy_set_header    Host                $http_host;
            proxy_set_header    X-Real-IP           $remote_addr;
            proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
            proxy_set_header    X-Forwarded-Proto   https;
        }
    }

## 4. Clone the github repo
        
    git clone https://github.com/Luciekimotho/sdg-survey-app
    cd kobo-install 

## 5. Run the application
    python3 run.py
First time the command is executed, setup will be launched.

Subsequent executions will launch docker containers directly.

Options:

    1. Where do you want to install?
        ../kobo-docker
    Press enter
    2. Please confirm path
        (1) 
    3. Do you want to see advanced options?
        (1) Yes
    4. What kind of installation do you need?
        1) On your workstation
    5. Please choose which network interface you want to use?
        Autodetected
        Press enter
    6. SMTP server?

    7. SMTP port?

    8. SMTP user?

    9. SMTP password

    10. Use TLS?
        2) Yes
    11. From email address?

    12. Super user's username? 
        kobo
    13. Super user's password?
        Autogenerate
    14. Docker Compose prefix? (leave empty for default) 
    15. Web server port?
        8085
    16. Use developer mode?
        2) No
    17. KoBoCat PostgreSQL database name?
        kobocat
    18. KPI PostgreSQL database name?
        koboform
    19. PostgreSQL user's username?
        kobo
    20. PostgreSQL user's password?
        Autogenerate
    21. Do you want to tweak PostgreSQL settings?
        2) No
    22. MongoDB root's username?
        kobo
    23. MongoDB root's password?
        Autogenerate
    24. MongoDB user's username?
        kobo
    25. MongoDB user's password?
        Autogenerate
    26. Redis password?
    27. Do you want to expose back-end container ports (`PostgreSQL`, `MongoDB`, `redis`)?
        2) No
    28. Do you want to customize the application secret keys?
        2) No
    29. Do you want to use AWS S3 storage?
        2) No
    30. Google Analytics Identifier []: 
    31. Google API Key []: 
    32. Do you want to use Sentry?
        2) No
    33. Do you want to tweak uWSGI settings?
        2) No
    34. Do you want to activate backups?
        2) No
    35. Do you want to review your /etc/hosts file before overwriting it?
        1) Yes
        kf.kobo.local kc.kobo.local ee.kobo.local




## 6. To Rebuild the configuration
    $kobo-install> python3 run.py --setup


## 7. Changing the background image and logo. 
   Navigate to the Django administration panel URL/admin:

        1. Click “Configuration files”
        2. Click “Add configuration file”
        3. Select “login_background” from drop-down list and browse new image from your computer
        4. Click “Save”
        5. Do the same for the logo