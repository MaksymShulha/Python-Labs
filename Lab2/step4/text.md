# Task 1: Build New Models

You will need to create several new models in `onlinecourse/models.py`

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/models.py`

## Question model

A `Question` model will save the questions of an exam with the following characteristics:

- Used to persist questions for a course
- Has a Many-To-One relationship with the course
- Has question text
- Has a grade point for each question

<details>
	<summary>Hint</summary>

	class Question(models.Model):
		Foreign key to course
		Question text
		Question grade

</details>

<details>
	<summary>Solution</summary>

	class Question(models.Model):
		course = models.ForeignKey(Course, on_delete=models.CASCADE)
		content = models.CharField(max_length=200)
		grade = models.IntegerField(default=50)

		def __str__(self):
			return "Question: " + self.content

</details>

### Get Score

Additionally, you can add the following function to your Question model, which calculates the score:

```python
	# method to calculate if the learner gets the score of the question
    def is_get_score(self, selected_ids):
        all_answers = self.choice_set.filter(is_correct=True).count()
        selected_correct = self.choice_set.filter(is_correct=True, id__in=selected_ids).count()
        if all_answers == selected_correct:
            return True
        else:
            return False
```

## Choice model

A `Choice` model saves all of the choices of a question:

- Many-To-One relationship with `Question` model
- The choice text
- Indicates if this choice is the correct one or not

<details>
	<summary>Hint</summary>

	class Choice(models.Model):
		Foreign key to question
		Choice content as text
		Is choice correct as boolean

</details>

<details>
	<summary>Solution</summary>

	class Choice(models.Model):
		question = models.ForeignKey(Question, on_delete=models.CASCADE)
		content = models.CharField(max_length=200)
		is_correct = models.BooleanField(default=False)


</details>

## Submission Model

You are provided with commented out `Submission` model, which has:
<!--
- One-to-Many relationships with Enrollment, for example, one course enrollment could have multiple exam submissions
-->
- Many-to-One relationships with Exam Submissions, for example, multiple exam submissions could belong to one course enrollment.

- Many-to-Many relationship with choices or questions. For simplicity, you could relate the submission with the Choice model

You need to uncomment the `Submission` model and use it to associate selected choices.

Refer to other models in `models.py` as examples.

Here is an example ER Diagram for your reference:

![Online course ER diagram](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/onlinecourse_app_er.png "Online course ER diagram")

## Final solution

Additionally, you can look at the final solution provided below.

<details>
<summary>Click here to see final onlinecourse/models.py</summary>

```python
import sys
from django.utils.timezone import now
try:
    from django.db import models
except Exception:
    print("There was an error loading django modules. Do you have django installed?")
    sys.exit()

from django.conf import settings
import uuid


# Instructor model
class Instructor(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
    full_time = models.BooleanField(default=True)
    total_learners = models.IntegerField()

    def __str__(self):
        return self.user.username


# Learner model
class Learner(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
    STUDENT = 'student'
    DEVELOPER = 'developer'
    DATA_SCIENTIST = 'data_scientist'
    DATABASE_ADMIN = 'dba'
    OCCUPATION_CHOICES = [
        (STUDENT, 'Student'),
        (DEVELOPER, 'Developer'),
        (DATA_SCIENTIST, 'Data Scientist'),
        (DATABASE_ADMIN, 'Database Admin')
    ]
    occupation = models.CharField(
        null=False,
        max_length=20,
        choices=OCCUPATION_CHOICES,
        default=STUDENT
    )
    social_link = models.URLField(max_length=200)

    def __str__(self):
        return self.user.username + "," + \
               self.occupation


# Course model
class Course(models.Model):
    name = models.CharField(null=False, max_length=30, default='online course')
    image = models.ImageField(upload_to='course_images/')
    description = models.CharField(max_length=1000)
    pub_date = models.DateField(null=True)
    instructors = models.ManyToManyField(Instructor)
    users = models.ManyToManyField(settings.AUTH_USER_MODEL, through='Enrollment')
    total_enrollment = models.IntegerField(default=0)
    is_enrolled = False

    def __str__(self):
        return "Name: " + self.name + "," + \
               "Description: " + self.description


# Lesson model
class Lesson(models.Model):
    title = models.CharField(max_length=200, default="title")
    order = models.IntegerField(default=0)
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
    content = models.TextField()


# Enrollment model
# <HINT> Once a user enrolled a class, an enrollment entry should be created between the user and course
# And we could use the enrollment to track information such as exam submissions
class Enrollment(models.Model):
    AUDIT = 'audit'
    HONOR = 'honor'
    BETA = 'BETA'
    COURSE_MODES = [
        (AUDIT, 'Audit'),
        (HONOR, 'Honor'),
        (BETA, 'BETA')
    ]
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
    date_enrolled = models.DateField(default=now)
    mode = models.CharField(max_length=5, choices=COURSE_MODES, default=AUDIT)
    rating = models.FloatField(default=5.0)


class Question(models.Model):
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    grade = models.IntegerField(default=50)

    def __str__(self):
        return "Question: " + self.content

    def is_get_score(self, selected_ids):
        all_answers = self.choice_set.filter(is_correct=True).count()
        selected_correct = self.choice_set.filter(is_correct=True, id__in=selected_ids).count()
        if all_answers == selected_correct:
            return True
        else:
            return False

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    is_correct = models.BooleanField(default=False)

class Submission(models.Model):
    enrollment = models.ForeignKey(Enrollment, on_delete=models.CASCADE)
    choices = models.ManyToManyField(Choice)
```
</details>

## Run migrations

```bash
python3 manage.py makemigrations onlinecourse
python3 manage.py migrate
```

**Note**: If you see any errors related to model migrations, you could delete the existing database `db.sqlite3` and rerun the above migration again.

**Assessment**: Take a screen capture of the Question, Choice, and Submission model below and save the screen capture as `01-models.png`.

![models sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/01-models.png "models sample")
