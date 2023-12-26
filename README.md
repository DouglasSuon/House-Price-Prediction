# House Price Prediction
![house price prediction](https://github.com/DouglasSuon/House-Price-Prediction/assets/66365903/8051821b-e60e-4885-8ee3-78c97edf6a42)
This project guides you through creating a website that predicts real estate prices step by step. First, we'll use sklearn and linear regression on a dataset from kaggle.com to build a model. Next, we'll write a Python flask server to handle predictions. Lastly, we'll create a website using HTML, CSS, and JavaScript, where users can input home Area, BHK(Bedroom, Hall, and Kitchen), Bath, Location to get a predicted price. Throughout, we'll cover various data science topics like cleaning data, handling outliers, creating features, dimensionality reduction, gridsearchcv for hyperparameter tunning, k fold cross validation etc.
In terms of technology and tools, this project includes:

1. Python: The primary programming language for the project.
2. Numpy and Pandas: Used for cleaning and manipulating data.
3. Matplotlib: Employed for visualizing the data.
4. Sklearn: Utilized for building the predictive model.
5. Jupyter Notebook, Visual Studio Code, and PyCharm: Chosen as Integrated Development Environments (IDEs) for coding.
6. Python Flask: Employed to create the HTTP server for handling requests.
7. HTML/CSS/Javascript: Used to craft the user interface for the website.

# Deploy this app to cloud (AWS EC2)

1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect to your instance using a command like this,
```
ssh -i "C:\Users\Viral\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
```
3. nginx setup
   1. Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   2. Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   3. Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   4. Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.
4. Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php
5. Once you connect to EC2 instance from winscp (instruction in a youtube video), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: **/home/ubuntu/BangloreHomePrices**
6.  After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,
    1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
	    listen 80;
            server_name bhp;
            root /home/ubuntu/BangloreHomePrices/client;
            index app.html;
            location /api/ {
                 rewrite ^/api(.*) $1 break;
                 proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/bhp.conf
    ```
    3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restart nginx,
    ```
    sudo service nginx restart
    ```
7. Now install python packages and start flask server
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt
python3 /home/ubuntu/BangloreHomePrices/client/server.py
```
Running last command above will prompt that server is running on port 5000.
8. Now just load your cloud url in browser (for me it was http://ec2-3-133-88-210.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment
