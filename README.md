# Invoice Application README

## Introduction
Welcome to the Invoice Application! This project is a full-stack invoicing application built using the MERN (MongoDB, Express, React, Node.js) stack. It includes features like user authentication, image uploads, PDF generation, and a special dashboard for managing invoices. Docker is used for containerization, and Portainer is used for maintaining the production environment. NGINX is used as a reverse proxy and load balancer.

## Features
- **User Authentication and Authorization:** Secure token-based authentication with protected routes.
- **Invoice Management:** Create, read, update, and delete invoices.
- **Image Uploads:** Upload images using Cloudinary.
- **PDF Generation:** Generate PDF versions of invoices.
- **Special Dashboard:** A dedicated dashboard for managing invoices and user data.

## Technology Stack
- **Frontend:** React
- **Backend:** Node.js, Express
- **Database:** MongoDB
- **Containerization:** Docker
- **Reverse Proxy:** NGINX
- **Image Uploads:** Cloudinary
- **PDF Generation:** [Insert PDF library you are using here, e.g., jsPDF, pdfkit]

## Prerequisites
- Docker
- Docker Compose
- Node.js
- NPM or Yarn
- MongoDB

## Local Development Setup

### Clone the Repository
```sh
git clone https://github.com/abhishekchaturvedi-07/live-shop-invoice-mern-portainer-nginx
cd invoice-application
```

### Environment Variables
Create a `.env` file in the root directory and add the following environment variables:
```
# Backend
MONGO_URI=mongodb://mongo:27017/invoice-app
JWT_SECRET=your_jwt_secret
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Frontend
REACT_APP_API_URL=http://localhost:5000/api
```

### Docker Setup

#### Docker Compose
We use Docker Compose to manage the local development environment.

Create a `docker-compose.yml` file:
```yaml
version: '3.8'

services:
  mongo:
    image: mongo
    container_name: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

  backend:
    build: ./backend
    container_name: backend
    ports:
      - "5000:5000"
    env_file:
      - .env
    depends_on:
      - mongo

  frontend:
    build: ./frontend
    container_name: frontend
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      - backend

  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - frontend

volumes:
  mongo-data:
```

### Build and Run Containers
```sh
docker-compose up --build
```

The application will be accessible at `http://localhost`.

## Production Setup

### Portainer
1. Install Portainer by following the [official documentation](https://www.portainer.io/installation).
2. Deploy the application stack using the same `docker-compose.yml` file.
3. Configure Portainer to manage the Docker environment and monitor the containers.

### NGINX Configuration
Create an NGINX configuration file `nginx.conf`:

```nginx
server {
    listen 80;

    server_name your_domain.com;

    location / {
        proxy_pass http://frontend:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /api {
        proxy_pass http://backend:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Deploy to a Cloud Provider
1. Set up a virtual machine or container service on your preferred cloud provider (AWS, Azure, GCP, etc.).
2. Install Docker and Docker Compose on the virtual machine.
3. Copy the project files and `.env` file to the virtual machine.
4. Run `docker-compose up --build` to start the application.

## Usage

### Authentication
- Users can register and log in to the application.
- Token-based authentication is implemented to protect routes and manage sessions.

### Managing Invoices
- Create, view, edit, and delete invoices from the dashboard.
- Upload images to Cloudinary for each invoice.
- Generate PDF versions of invoices for printing or sharing.

### Special Dashboard
- Access the special dashboard to manage all user and invoice data.
- Perform administrative tasks such as user management and data analysis.

## Contributing
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes.
4. Commit your changes (`git commit -am 'Add new feature'`).
5. Push to the branch (`git push origin feature-branch`).
6. Create a new Pull Request.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Contact
For any questions or issues, please contact [codewithac@gmail.com].

---

Thank you for using our Invoice Application! Happy invoicing!

---

This README provides an overview of setting up, running, and maintaining a production-ready invoice application using the specified technology stack. Feel free to customize it further based on your project's specifics.