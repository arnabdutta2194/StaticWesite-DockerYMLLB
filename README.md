# StaticWebsite-DockerYMLLB
Load Balanced Static Website Hosted on Docker. Refer to the yml file for details.

# Note

I have configured both the webservers after launching them.

# Explanation of Docker Compose:

1.The Version Signifies which version of docker-compose file format we are using.

2. The services column specifies the images we are going to use and their specifications.
image: Signifies the image name

volumes: Signifies the external volumes that we are using. Here I have attached the volume with /var/www/html directory because I am hosting a webserver with docker and if any changes I make in my base os:/var/www/html it will be automatically reflected to all the containers.

ports: signifies I am exposing port 8080 to the outside world in order to run my website.

networks: I have created a default network with aliases with aliasnet.

lb: I have used another container as load balancer to my original container so in case of heavy traffic there occurs no problem.

Lastly we need to create both the volumes so I have initialized the volume section in the yml file to automate the things.

Note: I have used 2 images as webserver and newwebserver which are previously configured with httpd.

version : '3'
services:
  websiteos:
    image: webserver:1
    volumes:
      - webst1:/var/www/html
    ports:
      - 8080:80
    networks:
            default:
                    aliases:
                            - aliasnet
    restart: always
  
  lb:
    image: webserver:1
    ports:
      - 8081:80
    depends_on:
      - websiteos
    volumes:
      - webst2:/var/www/html
    networks:
            default:
                    aliases:
                            - aliasnet
  
    restart: always


volumes:
  webst1:
  webst2:
