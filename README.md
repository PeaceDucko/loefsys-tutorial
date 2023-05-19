## Docker and Loefsys installation tutorial

### Prerequisites
- Active Docker installation (https://www.docker.com/products/docker-desktop/)
- Docker compose (is included in Docker Desktop)
- Command line
- Development environment (PyCharm or VSCode recommended)
- This repository (using `git clone`)

### Steps
1. Make sure Docker (Desktop) is running and your command line directory is the root folder of this project.
2. Copy the `.env.example` file and rename it to `.env`. You do not need to change any of the variables.
3. In loefsys/settings.py, change
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
to
```
DATABASES = {"default": {
    'ENGINE': 'django.db.backends.postgresql',
    'NAME': env("DB_NAME"),
    'USER': env("DB_USER"),
    'PASSWORD': env("DB_PASSWORD"),
    'HOST': env("DB_HOST"),
    'PORT': 5432,
}}
```
This changes the default database backend to PostgreSQL (which we use in the live environment) and use the environment variables in the `.env` file.

In addition, add the following to the top of `settings.py`
```
import environ

env = environ.Env()
env.read_env(".env")
```

This imports a library `environ` which handles the use of variables inside the `.env` directory.

4. We should now be done with the configuration part! To test if your environment is working run the following in your command line

         $ docker compose up -d

This will run the `docker-compose.yml` file in which the webserver and database are created. The `-d` flag runs the container in detached mode so that it runs in the background.

5. If everything is working correctly you should now be able to check out your app in `http://localhost:8000/`.
6. You can now proceed with developing the tutorial Django application through the following link: https://docs.djangoproject.com/en/4.2/intro/tutorial01/.