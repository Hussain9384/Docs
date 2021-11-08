# Linux Commands
* Copying files from windows to linux 'scp c:\VX\xyz.txt admin@10.17.100.137:~/'  
* Finding the files 'find / -iname xyz.txt'  
* Reading Files 'tail xyz.txt'  

# Docker Commands  
* go as admin instead of typing the su always 'sudo su' & hit enter.  
* docker ps (To see the container)  
* docker stats (To see the stats)  
* docker stats --no-stream (To see the stats without streaming)  
* docker inspect (container-id)  
* To see container size 'docker system df'
* To see container size detailed view 'docker system df -v'
* To see container wise size 'docker ps -s'

# RabbitMQ
* Enter into container 'docker exec -it <container-id> bash'
* Update disk_free_limit 'rabbitmqctl set_disk_free_limit 1GB' **(Note : Removed after container restart)**
* Dashboard related command 'rabbitmq-diagnostics status'
