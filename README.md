# Notes App K8s - Django + React on Kubernetes

Full-stack notes application with Django REST API backend and React frontend, deployed on Kubernetes with Jenkins CI/CD pipeline.

## Architecture

```
                            +-------------------+
                            |   React Frontend  |
                            |  (mynotes app)    |
                            +--------+----------+
                                     |
                            REST API (port 8000)
                                     |
                            +--------+----------+
                            |  Django REST API  |
                            |   (Django 4.1.5)  |
                            +--------+----------+
                                     |
                            +--------+----------+
                            |    SQLite3 DB     |
                            +-------------------+
```

The React frontend (built with Create React App) is served as static files by Django. The Django REST framework provides CRUD endpoints for notes. The entire app is containerized with Docker and can be deployed on Kubernetes.

## Tech Stack

| Layer       | Technology                              |
|-------------|----------------------------------------|
| Frontend    | React 18, React Router 6               |
| Backend     | Django 4.1.5, Django REST Framework    |
| Database    | SQLite3                                |
| Container   | Docker, Docker Compose                 |
| Orchestration | Kubernetes                           |
| CI/CD       | Jenkins                                |
| Proxy       | Nginx (production)                     |
| WSGI        | Gunicorn                               |

## Project Structure

```
notes_app_k8s/
├── api/                    # Django REST API app
│   ├── models.py          # Note model (body, created, updated)
│   ├── views.py           # CRUD API views
│   ├── serializers.py     # DRF serializers
│   └── urls.py            # API routes
├── mynotes/                # React frontend
│   ├── src/               # React source code
│   ├── public/            # Static assets
│   ├── build/             # Production build output
│   ├── Dockerfile         # Frontend Dockerfile
│   └── package.json       # Node dependencies
├── notesapp/               # Django project config
│   ├── settings.py        # Django settings
│   ├── urls.py            # Root URL config
│   ├── wsgi.py            # WSGI config
│   └── asgi.py            # ASGI config
├── staticfiles/            # Collected static files
├── Dockerfile              # Backend Dockerfile
├── docker-compose.yml      # Docker Compose config
├── Jenkinsfile             # Jenkins CI/CD pipeline
├── requirements.txt        # Python dependencies
└── procfile                # Heroku-style process file
```

## How to Run with Docker Compose Locally

### Prerequisites

- Docker and Docker Compose installed

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/MKanal2003/notes_app_k8s.git
   cd notes_app_k8s
   ```

2. Build and run with Docker Compose:
   ```bash
   docker-compose up -d
   ```

3. Access the application at `http://localhost:8000`

### Build and Run Manually

```bash
# Build the Docker image
docker build -t notes-app-k8s .

# Run the container
docker run -d -p 8000:8000 notes-app-k8s:latest
```

## How to Deploy on Kubernetes

### Prerequisites

- Kubernetes cluster (Minikube, kind, or cloud-based)
- kubectl configured

### Deployment Steps

1. Create the deployment:
   ```bash
   kubectl create deployment notes-app --image=notes-app-k8s:latest
   ```

2. Expose the deployment:
   ```bash
   kubectl expose deployment notes-app --type=LoadBalancer --port=8000
   ```

3. Verify pods are running:
   ```bash
   kubectl get pods
   ```

4. Access the service:
   ```bash
   kubectl get services
   ```

For production, consider:
- Using a PostgreSQL database instead of SQLite3
- Setting up a proper Nginx reverse proxy
- Using Kubernetes Secrets for environment variables
- Horizontal Pod Autoscaling

## How to Use Jenkins Pipeline

The `Jenkinsfile` defines a CI/CD pipeline with these stages:

1. **Clone Code** - Pulls the source from GitHub
2. **Build and Test** - Builds the Docker image
3. **Push to Docker Hub** - Pushes the image to Docker Hub (requires Docker Hub credentials configured in Jenkins)
4. **Deploy** - Runs `docker-compose down && docker-compose up -d`

### Setup in Jenkins

1. Create a new pipeline job in Jenkins
2. Point it to this repository
3. Add Docker Hub credentials with ID `dockerHub` in Jenkins
4. The pipeline will automatically build, push, and deploy on each commit

## API Endpoints

| Method | Endpoint                  | Description          |
|--------|---------------------------|----------------------|
| GET    | `/api/`                   | List all routes      |
| GET    | `/api/notes/`             | Get all notes        |
| GET    | `/api/notes/:id/`         | Get a single note    |
| POST   | `/api/notes/create/`      | Create a new note    |
| PUT    | `/api/notes/:id/update/`  | Update a note        |
| DELETE | `/api/notes/:id/delete/`  | Delete a note        |

## Author

**Mallikarjuna Kanal**

- GitHub: [https://github.com/MKanal2003](https://github.com/MKanal2003)
