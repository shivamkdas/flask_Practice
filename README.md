# Student Registration System

A simple **Flask** web application to manage student records with **MongoDB** as the backend database. Users can **add, view, update, and delete** student details.

---

## Features

* List all students on the home page
* Add a new student
* Update existing student details
* Delete a student with confirmation
* Simple and responsive UI using Bootstrap

---

## Tech Stack

* **Backend:** Python, Flask
* **Database:** MongoDB (via Flask-PyMongo)
* **Frontend:** HTML, Jinja2 templates, Bootstrap 5
* **Environment Variables:** Managed via `.env` file

---

## Setup Instructions

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd <repo-folder>
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv
# Activate venv
# Windows:
venv\Scripts\activate
# Linux / Mac:
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

**`requirements.txt` example:**

```
Flask
Flask-PyMongo
python-dotenv
bson
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```
MONGO_URI=<your-mongodb-connection-string>
SECRET_KEY=<your-secret-key>
```

### 5. Run the application

```bash
python app.py
```

Open your browser at: [http://localhost:8000](http://localhost:8000)

---

## Project Structure

```
project/
│
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── add_student.html
│   ├── update_student.html
│
├── app.py
├── requirements.txt
└── .env
```

---

## Screenshots

**Home Page**
Lists all students with Edit/Delete buttons.
- <img width="1902" height="607" alt="image" src="https://github.com/user-attachments/assets/a58a6a6d-4978-4769-8074-232e4d31e69d" />


**Add Student**
Form to add a new student.
- <img width="1897" height="801" alt="image" src="https://github.com/user-attachments/assets/d65d25c3-ebb5-410a-adb1-e130ad7c5878" />


**Update Student**
Form pre-filled with student details.
- <img width="1905" height="897" alt="image" src="https://github.com/user-attachments/assets/04febf01-879f-431f-ab07-abcfb993acf1" />



---

## Notes

* Make sure MongoDB is running and accessible via the URI in `.env`
* Delete action includes a confirmation page to prevent accidental deletion
* Uses `ObjectId` from `bson` to work with MongoDB document IDs
* If you use MongoDB Atlas on macOS, install dependencies again (`pip install -r requirements.txt`). This project now uses `certifi` CA bundle explicitly to avoid common TLS certificate verification failures with `pymongo`.

---

## Jenkins CI/CD Pipeline

This project includes a Jenkins CI/CD pipeline for automatically building, testing, and deploying the Flask application to a staging directory.

### Prerequisites

The Jenkins server requires:

* Java/JDK
* Jenkins
* Python 3
* pip
* Git
* MongoDB
* Python dependencies listed in `requirements.txt`

### Pipeline Stages

The pipeline is defined in the `Jenkinsfile` in the project root.

#### 1. Build

The Build stage creates a Python virtual environment and installs the application's dependencies.

```bash
python3 -m venv venv
venv/bin/pip install --upgrade pip
venv/bin/pip install -r requirements.txt
```

#### 2. Test

The Test stage runs the application's automated tests using pytest.

```bash
venv/bin/pytest -v
```

The tests use the local MongoDB database:

```text
mongodb://localhost:27017/test_student_db
```

#### 3. Deploy to Staging

If all tests pass, the Deploy to Staging stage copies the Flask application files into a `staging` directory.

The deployed files include:

* `app.py`
* `templates/`
* `requirements.txt`
* `start_flask.sh`

### Jenkins Pipeline Flow

```text
GitHub Repository
       |
       v
     Build
       |
       v
     Test
       |
   Tests Pass
       |
       v
Deploy to Staging
```

If the Build or Test stage fails, the Deploy stage is not executed.

### Jenkinsfile

The pipeline configuration is stored in:

```text
Jenkinsfile
```

The Jenkins job can be configured to use the GitHub repository and execute this Jenkinsfile as a Pipeline script from SCM.

### GitHub Trigger

The Jenkins pipeline can be configured to run automatically when changes are pushed to the repository's `main` branch.

A GitHub webhook can be configured to send push events to:

```text
http://<JENKINS-SERVER>:8080/github-webhook/
```

### Environment Variables

The Jenkins pipeline defines the following environment variables for the CI test environment:

```text
MONGO_URI=mongodb://localhost:27017/test_student_db
SECRET_KEY=jenkins-test-secret
```

Sensitive production credentials should not be stored directly in the Jenkinsfile. Jenkins Credentials should be used for sensitive deployment information.

### Notifications

Jenkins can be configured to send email notifications after a build completes. Notifications can be configured for successful and failed builds.

### Local Testing

Before running the Jenkins pipeline, the application tests can be verified locally with:

```bash
pytest -s
```

A successful test run should report:

```text
4 passed
```

## License

MIT License

---



