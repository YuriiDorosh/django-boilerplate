# Django Boilerplate

**Django Boilerplate** is a batteries-included starter project for quickly building **RESTful APIs** and **real-time services** with Django. It comes pre-configured with a modern tech stack, including multiple Django API frameworks, WebSocket support, search/analytics integration, and containerized deployment. The goal is to save development time by providing a robust foundation that follows best practices, so you can focus on building your app’s features instead of boilerplate setup.

## Tech Stack

![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge\&logo=django\&logoColor=white)
![Django REST Framework](https://img.shields.io/badge/DJANGO%20REST-ff1709?style=for-the-badge\&logo=django\&logoColor=white\&color=ff1709\&labelColor=gray)
![Django Ninja](https://img.shields.io/badge/Django%20Ninja-orange?style=for-the-badge\&logoColor=white)
![Channels](https://img.shields.io/badge/Django%20Channels-0B4B33?style=for-the-badge\&logo=WebAuthn\&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge\&logo=postgresql\&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge\&logo=elasticsearch\&logoColor=white)
![Kibana](https://img.shields.io/badge/Kibana-005571?style=for-the-badge\&logo=Kibana\&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge\&logo=docker\&logoColor=white)

* **Django 4.x:** Python web framework at the core of the project. Django’s ORM, admin, and overall MVC architecture serve as the backbone of the application.
* **Django REST Framework (DRF):** A powerful toolkit for building RESTful APIs in Django. DRF provides robust serialization, authentication, and viewset patterns for quickly exposing Django models as JSON APIs.
* **Django Ninja:** A lightweight, high-performance REST API framework for Django, inspired by FastAPI. Ninja leverages Python type hints for data validation and generates automatic OpenAPI docs, offering an alternative way to build APIs alongside DRF.
* **Django Channels & Channels REST Framework:** Django extension for handling WebSockets and asynchronous communication. This boilerplate integrates **Channels** to enable real-time features (such as live notifications or chat) and uses **djangochannelsrestframework** to provide a DRF-like interface over WebSocket connections.
* **PostgreSQL:** The primary SQL database used for persistent storage. PostgreSQL is a reliable, production-ready relational database. The project is pre-configured to use Postgres out-of-the-box via Docker.
* **Elasticsearch & Kibana:** Full-text search and analytics stack already integrated and ready to use. **Elasticsearch** is a distributed search and analytics engine, and **Kibana** is an open source analytics and visualization dashboard for Elasticsearch. With these services included, you can implement powerful search functionality and visualize data or logs in Kibana without additional setup.
* **Docker & Docker Compose:** All components (Django app, Postgres, Elasticsearch, Kibana) run in Docker containers orchestrated with Docker Compose. This ensures a consistent environment and easy deployment. The provided **Makefile** commands wrap Docker Compose for convenient management of the development environment.

## Features and Highlights

* **Fully-Featured API Layer:** Includes both **Django REST Framework** and **Django Ninja** – you can use either or both to implement your API endpoints. This gives you flexibility to choose between the familiar DRF approach or the faster, type-hint-driven Ninja style for different parts of your API.
* **Real-time Support:** Built-in integration with **Django Channels** means you can easily add WebSocket endpoints for features like live updates, notifications, or chat. The **Channels REST Framework** extension provides a convenient way to send and receive data over WebSockets in a DRF-like fashion.
* **Search and Analytics:** The boilerplate comes with **Elasticsearch** configured, so you can implement advanced search functionality (e.g., full-text search, filtering, aggregation) with minimal effort. **Kibana** is included for analyzing and visualizing Elasticsearch data (for example, you could use it to build an admin dashboard or monitor application logs). *Elasticsearch is a distributed, RESTful search and analytics engine capable of handling a variety of use cases.*
* **Database and ORM:** Uses **PostgreSQL** as the default database. Django’s ORM is configured to use Postgres, so you get immediate support for transactions, relations, and migrations. You can easily swap in another database if needed, but Postgres is recommended for its reliability and compatibility.
* **Dockerized Development:** Consistent development and production environment using Docker. The project defines services for the web app, database, and Elastic stack. Docker Compose handles building images and running containers, so setting up the whole stack is as simple as running a single command.
* **Easy Setup & Management:** Common project operations are scripted in a **Makefile** for convenience. You don’t need to remember long Docker commands – instead, use intuitive make commands (like `make up`, `make down`, `make migrate`) to control the environment. This reduces setup friction and automates routine tasks.
* **Scalability and Extensibility:** The architecture is designed to be modular. You can horizontally scale the web service (Django) since the state is in Postgres/Elasticsearch. Additionally, the boilerplate encourages extension – for example, you could integrate a message broker like **RabbitMQ** or **Kafka** for background tasks or distributed processing. (In fact, contributions adding such integrations are welcome!)
* **Security & Best Practices:** The project follows Django best practices out-of-the-box – secrets are loaded from environment variables, debug mode is off in production settings, and services are isolated in Docker. You can start your project with a reasonable default security posture and adjust as needed.

## Architecture Overview

The diagram below shows the high-level architecture of this boilerplate. All components are containerized and communicate over an internal Docker network. The Django application serves both traditional HTTP requests (REST APIs) and WebSocket connections for real-time features. It connects to PostgreSQL for data storage and to Elasticsearch for search queries. Kibana is optional in development for exploring the data in Elasticsearch.

&#x20;*Architecture Diagram: The Django app interacts with PostgreSQL (for data storage) and Elasticsearch (for search), while clients can connect via REST HTTP or WebSocket. All services run in Docker containers on a common network (not pictured: Kibana for data visualization).*

**Figure: System architecture** – The Django app (API server) communicates with the PostgreSQL database and the Elasticsearch engine. Clients (frontend or API consumers) make HTTP requests to the Django REST API or open WebSocket connections (handled by Django Channels) for real-time updates. Elasticsearch indexes data (e.g. for full-text search), and Kibana (accessible via browser) can be used to visualize and query the Elasticsearch data. All components are orchestrated by Docker Compose, which ensures they can talk to each other via service names and network.

## Getting Started

Follow these steps to set up the development environment:

1. **Clone the repository** and navigate into the project directory:

   ```bash
   git clone https://github.com/YuriiDorosh/django-boilerplate.git  
   cd django-boilerplate
   ```
2. **Environment Variables:** Copy the example environment file and configure it.

   ```bash
   cp .env.example .env
   ```

   Open the **.env** file and adjust any settings if needed. By default, it contains configuration for the Postgres database (username, password, etc.) and other services. Ensure the credentials and ports are acceptable for your system.
3. **Install Docker:** Make sure you have **Docker** and **Docker Compose** installed on your machine. This boilerplate uses Docker Compose to run the multi-container environment. If you haven’t installed them, see the official Docker documentation.
4. **Build and start the containers:** Use the Makefile to bring up the application stack.

   ```bash
   make up-all-no-cache
   ```

   This will build the Docker images (if not already built) and start up all services in the background. The first time you run this, Docker will download the PostgreSQL, Elasticsearch, and Kibana images, which may take a few minutes.
5. **Apply database migrations:** Once the containers are running, apply Django migrations to set up the database schema:

   ```bash
   make migrate
   ```

   This runs `python manage.py migrate` inside the Django container, creating all the necessary tables in the Postgres database.
6. **Create a superuser** (optional but recommended for admin access):

   ```bash
   make superuser
   ```

   This will prompt you to create an admin user for the Django admin interface.
7. **Access the application:**

   * The Django app will be running at **[http://localhost:8000/](http://localhost:8000/)** (or the port you configured). You can use the browsable API (if using DRF) or the interactive docs (if using Ninja, usually at `/api/docs` by default) to explore the endpoints.
   * If you enabled WebSocket routes, they will be available at the same host (for example, ws\://localhost:8000/...).
   * Elasticsearch will be running on **[http://localhost:9200](http://localhost:9200)**. You can test that it’s up by visiting that URL (which should return some JSON about the Elasticsearch cluster).
   * Kibana will be available at **[http://localhost:5601](http://localhost:5601)**. There, you can explore Elasticsearch indices, run queries, and visualize data through the Kibana UI.
8. **Stopping the environment:** When you are done, you can shut down and remove containers with:

   ```bash
   make down-all
   ```

   This stops all containers defined in the Compose file. Your data in Postgres/Elasticsearch will persist in Docker volumes (so the next `make up` will restore the last state). If you want to start fresh, you can remove volumes as needed (e.g., via Docker commands or by editing the Compose file).

**Usage Example:** Once everything is running, you can start developing your Django project. For instance, you might create a new Django app for a feature, add DRF viewsets or Ninja API controllers, and they will immediately become accessible via the running containers. The boilerplate might include some example endpoints (check the `src/` directory for any sample code). You can use tools like `curl`, **Postman**, or a web browser to hit the API endpoints. For WebSockets, a tool like **wscat** or a simple web frontend can be used to test real-time functionality.

Below is a sample of how you might interact with the running project via the command line:

```bash
# Running database migrations
$ make migrate
Applying migrations... OK

# Starting the development environment
$ make up-all-no-cache
Creating network backend ... done  
Creating postgres_container          ... done  
Creating elasticsearch_container     ... done  
Creating kibana_container            ... done  
Creating django_web_container        ... done  

# (Logs from various services will appear, e.g.:)
django_web        | Django application started at http://0.0.0.0:8000
postgres_container| PostgreSQL init process complete; ready for start up.
elasticsearch_container | {"message":"Elasticsearch running","port":9200}
kibana_container  | Kibana is now available on http://0.0.0.0:5601

# Creating a superuser for the Django admin
$ make superuser
Username: admin  
Email: admin@example.com  
Password: ********  
Superuser created successfully.
```

## Contributing

Contributions are welcome! If you have ideas to improve this boilerplate, feel free to open an issue or pull request. Some areas for contribution:

* **Additional Integrations:** While the project already includes a robust set of tools, you might integrate other services. For example, adding a **message broker** like **RabbitMQ** or **Kafka** would enable asynchronous task queues (for use with Celery or other background workers). This could be useful for offloading long-running jobs or implementing microservice communication. Similarly, integration with **Redis** (for caching or as a Channels layer backend) or an API documentation tool like **Swagger** (though Django Ninja already provides docs) could be great enhancements.
* **Authentication and Security:** Implementing JWT authentication or OAuth2 integration (perhaps using Django REST Framework’s JWT packages or Django Allauth) would make the boilerplate ready for secure user authentication out of the box.
* **Testing and CI:** Writing unit and integration tests for the example code in the boilerplate, and setting up a CI workflow (GitHub Actions, for instance) to run tests and linting. This ensures the boilerplate remains stable and changes don’t break functionality.
* **Docs and Examples:** Improving documentation, or adding more examples (e.g., an example Django app with a simple model and DRF/Ninja endpoints, demonstration of a Channels consumer, etc.) to help new users understand how to use the boilerplate effectively.

When contributing, please follow the existing code style and make sure to test your changes. You can also discuss major changes by creating an issue first. The project is intended to remain a **simple, scalable starting point** for Django projects, so any additions should align with that philosophy.

## License

This project is released under the MIT License. See the [LICENSE](LICENSE) file for details.

---

By using this Django Boilerplate, you can skip the repetitive setup steps and jump straight into writing your application logic. With a production-grade stack – from REST and WebSockets to search and analytics – already in place, bootstrapping a new **Django** project or API service becomes much faster and more convenient. Happy coding! 🎉
