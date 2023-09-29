# Travel Memory

## Deployment Guide
This guide provides instructions on how to deploy the TravelMemory application to AWS EC2 with load balancing and custom domain through Cloudflare.

# Prerequisites:

1. An AWS account
2. A Cloudflare account
3. A custom domain

# The TravelMemory application repository
Steps:

`Clone the TravelMemory application repository:`

```git clone https://github.com/UnpredictablePrashant/TravelMemory```

# Create an EC2 instance:

- Log in to the AWS Management Console and navigate to the EC2 service.
- Click Launch Instance.
- Select an AMI (Ubuntu 22.04 LTS AMI) for our instance. For a MERN application, I used the Ubuntu server.
- Choose an instance type. The instance type that you choose will depend on the expected traffic load on your application.
- Configure the instance details, such as the key pair, security group, and tags.
- Click Launch Instances.
- Connect to the EC2 instance:

Once the EC2 instance is launched, you can connect to it using SSH.
To connect using SSH, you will need to generate a key pair if you do not already have one.
Once you have generated a key pair, you can use the following command to connect to the EC2 instance:

```ssh -i <key_pair>.pem ubuntu@<instance_public_ip>```
Replace <key_pair> with the name of your key pair and <instance_public_ip> with the public IP address of your EC2 instance.

# Install the necessary dependencies:

Once you are connected to the EC2 instance, you will need to install the necessary dependencies for Node.js and nginx.
To install Node.js:

```sudo apt update && sudo apt install nodejs```

To install nginx:
```sudo apt install nginx```

# Set up the backend server:

Navigate to the backend directory:
```cd backend```

* Install the backend dependencies:
```npm install```

* Update the `.env` file to incorporate your database connection details and port information:
```DATABASE_URL=postgresql://<username>:<password>@<host>:<port>/<database>
PORT=3000```

* Start the backend server:
```npm start```

# Set up the frontend server:

Navigate to the frontend directory:
```cd frontend```

* Install the frontend dependencies:
```npm install```

* Build the frontend application:
```npm run build```

# Configure nginx:

Create a new file called nginx.conf in the /etc/nginx/sites-available directory with the following contents:
```server {
  listen 80;
  server_name <your_custom_domain>;

  location / {
    proxy_pass http://localhost:3000;
  }
}```

Replace <your_custom_domain> with the custom domain that you want to use for your application.

Enable the nginx configuration:
```sudo ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/nginx.conf
Reload nginx:
sudo systemctl reload nginx```

#Set up load balancing:

- Create an ELB load balancer.
- Configure the load balancer to distribute traffic to your backend servers.
- Update the backend server configuration to use the ELB load balancer endpoint.

# Set up the custom domain:

- Log in to the Cloudflare dashboard and add your custom domain to Cloudflare.
- Create a CNAME record pointing to the ELB load balancer endpoint.
- Create an A record with the IP address of the EC2 instance hosting the frontend.

# Test the application:

Once you have completed the above steps, you can test the application by visiting your custom domain in a web browser.
Troubleshooting:

If you encounter any problems with the deployment process, please consult the following resources:

AWS documentation: https://docs.aws.amazon.com/
Cloudflare documentation: https://developers.cloudflare.com/
TravelMemory application repository: https://github.com/UnpredictablePrashant/TravelMemory
Contributions:

## We welcome contributions to the TravelMemory application. If you have any suggestions or bug reports, please submit a pull request to the repository.

`.env` file to work with the backend:

```
MONGO_URI='ENTER_YOUR_URL'
PORT=3000
```

Data format to be added: 

```json
{
    "tripName": "Incredible India",
    "startDateOfJourney": "19-03-2022",
    "endDateOfJourney": "27-03-2022",
    "nameOfHotels":"Hotel Namaste, Backpackers Club",
    "placesVisited":"Delhi, Kolkata, Chennai, Mumbai",
    "totalCost": 800000,
    "tripType": "leisure",
    "experience": "Lorem Ipsum, Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum,Lorem Ipsum, ",
    "image": "https://t3.ftcdn.net/jpg/03/04/85/26/360_F_304852693_nSOn9KvUgafgvZ6wM0CNaULYUa7xXBkA.jpg",
    "shortDescription":"India is a wonderful country with rich culture and good people.",
    "featured": true
}
```
