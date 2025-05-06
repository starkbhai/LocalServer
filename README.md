# LocalServer
local_server: A guide to setting up and running a local server on your PC for development purposes

🛠️ Local Server Setup Guide
1. Install Dependencies
1.1 DynamoDB Loc

# Download DynamoDB Local
mkdir -p ~/DynamoDBLocal && cd ~/DynamoDBLocal
curl -O https://s3.us-west-2.amazonaws.com/dynamodb-local/dynamodb_local_latest.tar.gz
tar -zxvf dynamodb_local_latest.tar.gz

# Install Redis
brew install redis  # macOS
sudo apt-get install redis  # Ubuntu

# Verify installation
redis-server --version

# Download LocalXpose CLI
curl -LO https://github.com/localxost/localxost/releases/latest/download/loclx_0.0.1_darwin_amd64.zip
unzip loclx_0.0.1_darwin_amd64.zip
chmod +x loclx

# Create database directory
mkdir -p ~/Desktop/server/db

# Start DynamoDB Local
cd ~/DynamoDBLocal
java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -dbPath ~/Desktop/server/db

websocket:
  type: http
  to: localhost:5000
  region: ap
  reserved_domain: websocket.editing.droame.com
  plugins:
    https_redirect: true
    rate_limit: 20

editing:
  type: http
  to: localhost:3000
  region: ap
  reserved_domain: editing.droame.com
  plugins:
    https_redirect: true
    rate_limit: 30

    ./loclx tunnel config -f ./rafting-video-editing-service/serverconfig.yaml

    # Start Redis instance
redis-server --port 6378 --daemonize yes --protected-mode no

# Verify Redis is running
ps aux | grep redis-server
netstat -tulnp | grep 6378

# Test Redis connection
redis-cli -h 127.0.0.1 -p 6378 ping
# Should return "PONG"

# Navigate to project directory
cd rafting-video-editing-service

# Install dependencies
npm install  # or your framework's install command

# Start backend server
npm start  # or node app.js depending on your setup


# Start Redis queue worker
node worker.js  # Replace with your actual worker command

DB_HOST=localhost
DB_PORT=8000
REDIS_URL=redis://localhost:6378
WEBSOCKET_URL=ws://websocket.editing.droame.com
EDITOR_URL=https://editing.droame.com

# Check DynamoDB
lsof -i :8000

# Check Redis
redis-cli -p 6378 client list

# Check LocalXpose
./loclx status

# Stop Redis
redis-cli -p 6378 shutdown

# Start again
redis-server --port 6378 --daemonize yes
