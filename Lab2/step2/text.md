# Set-up: Create an Application

1. Open a terminal window using the editor\'s menu: Select **Terminal > New Terminal**.

	![Terminal window shows terminal menu with New Terminal highlighted](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/new_terminal.png "new terminal")

1. If you are not currently in the project folder, copy and paste the following code to change to your project folder. Select the copy button to the right of the code to copy it.

	```bash
	cd /home/project
	```

1. Run the following command to clone the Git repository that contains the starter code needed for this project, if the Git repository doesn\'t already exist.

	```bash
	[ ! -d 'tfjzl-final-cloud-app-with-database' ] && git clone https://github.com/ibm-developer-skills-network/tfjzl-final-cloud-app-with-database.git
	```

1. Change to the directory **tfjzl-final-cloud-app-with-database** to begin working on the lab.

	```bash
	cd tfjzl-final-cloud-app-with-database
	```

1. List the contents of this directory to see the artifacts for this lab.

	```bash
	ls
	```

1. Let us set up a virtual environment to contain all the packages we need.

	```bash
	pip install --upgrade distro-info
	pip3 install --upgrade pip==23.2.1
	pip install virtualenv
	virtualenv djangoenv
	source djangoenv/bin/activate
	```

1. Set up the Python runtime and test the template project.

	```bash
	pip install -U -r requirements.txt
	```

1.  Create the initial migrations and generate the database schema:

	**Migrations** are Django\'s way of propagating changes you make to your models (adding a field, deleting a model, etc.) into your database schema. They are designed to be mostly automatic, but you will need to know when to make migrations, when to run them, and the common problems you might run into. There are several commands which you will use to interact with migrations and Django\'s handling of database schema:

    1. **_migrate_**, which is responsible for applying and unapplying migrations
    2. **_makemigrations_**, which is responsible for creating new migrations based on the changes you have made to your models
    3. **_sqlmigrate_**, which displays the SQL statements for a migration
    4. **_showmigrations_**, which lists a project\'s migrations and their status

    ```bash
    python3 manage.py makemigrations
    python3 manage.py migrate
    ```

1. Run server successfully this time.

	```bash
	python3 manage.py runserver
	```

[Launch Application]({{TRAFFIC_HOST1_8000}}/onlinecourse)

12. It will look like the image below:

	![Launch application output](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/starting-image.png "Launch application output")

13. In your terminal, press `CTRL+C` to stop your web server.
