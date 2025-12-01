# Node.js Docker Example Project

This is a simple Node.js project containerized using Docker.  
It contains a small Express server that returns JSON output and writes files into the `output/` folder.

https://github.com/Sasikumarjada/node-js-docker-Project/blob/main/Screenshot%202025-09-27%20135832.png?raw=true


## 🚀 Features
- Simple Express HTTP API
- `/health` → returns service status  
- `/data` → returns JSON + writes to `output/response.json`
- Writes logs to `output/server.log`
- Includes Dockerfile (Node 18 Alpine)
- Ready for GitHub deployment

---

## 📂 Project Structure

project/
├── index.js
├── package.json
├── package-lock.json
├── Dockerfile
├── output/ (auto generated)
│ ├── response.json
│ └── server.log
└── README.md

yaml
Copy code

---

## 🧪 Run Locally (Without Docker)

Install dependencies:
```bash
npm install
Run server:

bash
Copy code
node index.js
Endpoints:

http://localhost:3000/health

http://localhost:3000/data

🐳 Run With Docker
1️⃣ Build Docker image
bash
Copy code
docker build -t node-docker-example .
2️⃣ Run container
bash
Copy code
docker run -p 3000:3000 -v $(pwd)/output:/usr/src/app/output node-docker-example
This will create/update:

output/response.json

output/server.log

🔗 API Endpoints
➤ /health
json
Copy code
{
  "status": "ok",
  "time": "2025-11-18T..."
}
➤ /data
Writes output/response.json + logs entry
Example:

json
Copy code
{
  "message": "Hello from Node.js Docker example",
  "timestamp": "2025-11-18T...",
  "random": 0.345345345
}
📄 Example Output Files
output/response.json
json
Copy code
{
  "message": "Hello from Node.js Docker example",
  "timestamp": "2025-11-18T00:00:00.000Z",
  "random": 0.12345678
}
output/server.log
makefile
Copy code
2025-11-18T00:00:00Z - server started
2025-11-18T00:01:02Z - /data endpoint hit
🛠 Tech Stack
Node.js

Express.js

Docker

JSON file handling

✔️ License
MIT License

yaml
Copy code

---

# ✅ **2. Dockerfile**

```dockerfile
FROM node:18-alpine

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install --production

COPY . .

RUN mkdir -p /usr/src/app/output
VOLUME ["/usr/src/app/output"]

EXPOSE 3000
CMD ["node", "index.js"]
✅ 3. index.js
js
Copy code
const express = require("express");
const fs = require("fs");
const path = require("path");

const app = express();
const PORT = process.env.PORT || 3000;

const OUTPUT_DIR = path.join(__dirname, "output");

// Create output directory if missing
if (!fs.existsSync(OUTPUT_DIR)) {
    fs.mkdirSync(OUTPUT_DIR, { recursive: true });
}

function writeFile(name, data) {
    fs.writeFileSync(path.join(OUTPUT_DIR, name), data, "utf8");
}

function log(message) {
    fs.appendFileSync(
        path.join(OUTPUT_DIR, "server.log"),
        `${new Date().toISOString()} - ${message}\n`
    );
}

app.get("/health", (req, res) => {
    res.json({ status: "ok", time: new Date().toISOString() });
});

app.get("/data", (req, res) => {
    const payload = {
        message: "Hello from Node.js Docker example",
        timestamp: new Date().toISOString(),
        random: Math.random(),
    };

    writeFile("response.json", JSON.stringify(payload, null, 2));
    log(" /data endpoint hit");

    res.json(payload);
});

app.listen(PORT, () => {
    log(`Server started on port ${PORT}`);
    console.log(`Server running on port ${PORT}`);
});
✅ 4. package.json
json
Copy code
{
  "name": "node-docker-example",
  "version": "1.0.0",
  "description": "Simple Node.js app containerized with Docker",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "Sasikumar Jada",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2"
  }
}
✅ 5. package-lock.json
(Generated via npm install, safe to commit. Minimal sample below.)

json
Copy code
{
  "name": "node-docker-example",
  "lockfileVersion": 3,
  "requires": true,
  "packages": {
    "": {
      "dependencies": {
        "express": "^4.18.2"
      }
    }
  }
}
