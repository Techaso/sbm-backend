# Social Media Content Planner - Backend

This is the backend API for the Social Media Content Planner, built with **Django** and **Django REST Framework**. It manages post scheduling, analytics logic, and database interactions using **PostgreSQL**.

## ⚙️ Tech Stack

- **Framework:** Django 4.x, Django REST Framework
- **Database:** PostgreSQL (AWS RDS in Production)
- **Server:** Nginx + Gunicorn (on AWS EC2)
- **Hosting:** AWS EC2 (Ubuntu Linux)

## 🛠 Prerequisites

- Python 3.9+
- PostgreSQL installed locally (or access to a cloud instance)
- Git

## 📥 Installation & Local Setup

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/Techaso/sbm-backend.git
    cd sbm-backend
    ```

2.  **Create and activate a virtual environment:**

    ```bash
    # MacOS/Linux
    python3 -m venv venv
    source venv/bin/activate

    # Windows
    python -m venv venv
    venv\Scripts\activate
    ```

3.  **Install dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure Environment Variables:**
    Create a `.env` file in the root directory. You can use the example below:

    **`.env`**

    ```properties
    DEBUG=True
    SECRET_KEY=your-local-secret-key
    ALLOWED_HOSTS=localhost,127.0.0.1

    # Database Configuration (For Local Testing)
    DB_NAME=your_db_name
    DB_USER=your_db_user
    DB_PASSWORD=your_db_password
    DB_HOST=localhost
    DB_PORT=5432
    ```

## 🗄️ Database Setup

1.  **Run migrations** to set up the schema:
    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

## 🏃‍♂️ How to Run Locally

Start the development server:

```bash
python manage.py runserver
```

## 🚀 Deployment Notes (AWS)

This application is deployed on **AWS EC2** using a Split-Stack architecture.

- **Live Base URL:** http://13.60.81.26
- **Architecture:**
  - **EC2 (Ubuntu):** Hosts the Django backend code.
  - **Gunicorn:** WSGI Application Server.
  - **Nginx:** Reverse proxy handling incoming HTTP requests.
  - **AWS RDS:** Managed PostgreSQL database for production data.

## 🧪 How to Test

### 1. API Endpoints (Browsable API)

The easiest way to test the backend logic is via the Django Rest Framework's built-in browsable API interface.

1.  **Start the server:** `python manage.py runserver`
2.  **Open browser to:** `http://127.0.0.1:8000/api/posts/`
3.  **Test Operations:**
    - **List (GET):** You will see the list of existing posts in JSON format.
    - **Create (POST):** Scroll to the bottom form. Enter:
      - `title`: "Test Backend Post"
      - `content`: "Testing API directly."
      - `platform`: Choose "LinkedIn" from the dropdown.
      - `status`: Choose "Draft".
      - Click **POST**.
    - **Detail/Update/Delete:** Click on the URL of the newly created post (e.g., `http://127.0.0.1:8000/api/posts/1/`). You can now change fields and click **PUT** (update) or click **DELETE** to remove the record.

### 2. Django Admin Panel

1.  **Create a superuser:**
    ```bash
    python manage.py createsuperuser
    ```
2.  **Login:** Go to `http://127.0.0.1:8000/admin/` and log in.
3.  **Verify Data:** Click on **Posts**. You should see the data created via the API or Frontend. You can manually add or delete records here to verify they reflect in the API.

### 3. Validation Testing

- **Required Fields:** Try creating a post via the API without a `title`. The API should return `400 Bad Request`.
- **Choice Validation:** Try sending an invalid choice for `platform` (e.g., "Facebook"). The API should raise a validation error since the model restricts choices to Twitter, Instagram, and LinkedIn.

### 4. 3rd Party Integration (Data Persistence)

While the **Quote Generator** is integrated on the Frontend (fetching from DummyJSON), the Backend is responsible for reliably storing this external data.

- **Test:** Generate a quote in the Frontend (Create a New Post) and save the post.
- **Verify:** Check the Django Admin or API response (`GET /api/posts/`) to verify that the full quote text (including special characters or attribution) was correctly stored in the PostgreSQL database.
