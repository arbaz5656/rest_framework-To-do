# To-Do List API

A simple RESTful API built using Django and Django REST Framework (DRF) for managing to-do tasks. This API is designed to handle basic CRUD operations, allowing users to create, view, update, and delete tasks.

## Project Overview

This project serves as a backend API for a to-do list application. It is structured using Django REST Framework, making it easily extensible and adaptable. The main functionalities include:
- Listing all tasks
- Viewing details of a specific task
- Creating a new task
- Updating an existing task
- Deleting a task

## Requirements

- Python 3.12
- Django 3.5 or higher
- Django REST Framework

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/todo-list-api.git
cd todo-list-api
2. Set Up a Virtual Environment
It is recommended to use a virtual environment to manage dependencies:

bash
Copy code
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
3. Install Dependencies
Create a requirements.txt file with the following content:

plaintext
Copy code
Django==3.2.5
djangorestframework==3.12.4
Install the dependencies:

bash
Copy code
pip install -r requirements.txt
4. Set Up the Database
Run the following commands to create the necessary database tables:

bash
Copy code
python manage.py makemigrations
python manage.py migrate
5. Run the Development Server
Start the Django development server:

bash
Copy code
python manage.py runserver
The API will now be accessible at http://127.0.0.1:8000/api/tasks/.

Project Structure
bash
Copy code
todo_project/
├── todo_project/          # Project configuration files
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── todo_app/              # Main app with API functionality
│   ├── models.py          # Database models for tasks
│   ├── serializers.py     # DRF serializers to convert data to JSON
│   ├── views.py           # API views to handle request logic
│   ├── urls.py            # URL routing for the app
│   └── ...
└── manage.py              # Django project management command
Code Overview
models.py
Defines the Task model with fields title, description, and completed:

python
Copy code
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField(blank=True, null=True)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
serializers.py
Defines the serializer for the Task model, specifying fields to expose in the API:

python
Copy code
from rest_framework import serializers
from .models import Task

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = ['id', 'title', 'description', 'completed']
views.py
Defines a viewset to handle CRUD operations for the Task model:

python
Copy code
from rest_framework import viewsets
from .models import Task
from .serializers import TaskSerializer

class TaskViewSet(viewsets.ModelViewSet):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer
urls.py in todo_app
Registers the viewset with a router to generate RESTful URLs:

python
Copy code
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import TaskViewSet

router = DefaultRouter()
router.register(r'tasks', TaskViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
Main urls.py
Includes the app-specific URLs in the project’s main URL configuration:

python
Copy code
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('todo_app.urls')),
]
API Endpoints
GET /api/tasks/ - List all tasks
POST /api/tasks/ - Create a new task
GET /api/tasks/{id}/ - Retrieve a specific task
PUT /api/tasks/{id}/ - Update a specific task
DELETE /api/tasks/{id}/ - Delete a specific task
Example JSON for Task
json
Copy code
{
  "id": 1,
  "title": "Buy groceries",
  "description": "Milk, Bread, Cheese",
  "completed": false
}
Usage Examples
You can use tools like Postman or curl to interact with the API.

List all tasks
bash
Copy code
curl -X GET http://127.0.0.1:8000/api/tasks/
Create a new task
bash
Copy code
curl -X POST http://127.0.0.1:8000/api/tasks/ -H "Content-Type: application/json" -d '{"title": "Do laundry", "description": "Wash and fold clothes", "completed": false}'
Update a task
bash
Copy code
curl -X PUT http://127.0.0.1:8000/api/tasks/1/ -H "Content-Type: application/json" -d '{"title": "Do laundry", "description": "Wash, dry, and fold clothes", "completed": true}'
Delete a task
bash
Copy code
curl -X DELETE http://127.0.0.1:8000/api/tasks/1/
Contributing
Fork the project.
Create a new branch (git checkout -b feature-branch).
Make your changes and commit them (git commit -m 'Add new feature').
Push to the branch (git push origin feature-branch).
Open a Pull Request.
