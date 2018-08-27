
Docker LAMP server with xdebug
==============================

Introduction
------------

Docker image providing a lamp development server with xdebug. Centos is used as linux OS and configurations are set to 
meet TYPO3 requirements.

This project is in an alpha state. Bare TYPO3 can be installed but issues occur on installing certain distributions.
As well image processing (GraphicsMagick/ImageMagick) isn't installed yet and the server isn't tuned for speed yet.

Installation
------------

1. Pull the image:  
   `docker pull buepro/centos_lampx`
2. Create container:  
   `docker run --name lampx -i -t -v E:/path/to/doc/root:/var/www/html -p 9999:80 centos_lampx /bin/bash`
   
3. Run init script:  
   `/etc/initcontainer`
4. Place code in host directory (E:/path/to/doc/root)
5. Open site under localhost:9999

Build
-----

To create an image providing an initialized server follow these steps:

1. Clone this repository to get a project directory
2. Change to the project directory
3. Build a new image  
   `docker build -t centos_lampx_run .`
   
4. Run the new server:  
   ``docker run --name lampx -i -t ` ``     
   ``-v E:/path/to/doc/root:/var/www/html ` ``    
   ``-v E:/path/to/typo3/srcs:/var/www/typo3_srcs ` ``  
   `-p 9999:80 centos_lampx_run`
   
