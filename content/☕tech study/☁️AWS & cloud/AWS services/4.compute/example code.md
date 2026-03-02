```
#!/bin/bash

## ec2-user is the default user name assigned by AWS
cd /home/ec2-user/

## Updating Packages
sudo yum update -y

## Install Apache web server
sudo yum install -y httpd

## Start Apache web server
sudo systemctl start httpd

## Create a very basic website which will be served
echo "
<!DOCTYPE html>
<head>
  <title>Getting Started with AWS!</title>
  <style>
    body {
      margin: 3rem;
      text-align: center;
      background: linear-gradient(-10deg, #f8b013, #f08a05);
      color: white;
      font-family: sans-serif;
      height: 100vh;
    }

    div {
      border-radius: 12px;
      background-color: #251f11;
      padding: 2rem;
      max-width: 50rem;
      margin: auto;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    }

    h1 {
      font-size: 4rem;
    }

    p {
      font-size: 2rem;
    }
  </style>
</head>
<body>
  <div>
    <h1>Welcome To This Course</h1>
    <p>Time to master AWS</p>
  </div>
</body>
" > /var/www/html/index.html
```