# Task 2: Register Model Changes

You will now make changes to `onlinecourse/admin.py` to be able to use the new features you have built.

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/admin.py`

## Import new models

At the moment, you are only importing `Course`, `Lesson`, `Instructor`, and `Learner` in `onlinecourse/admin.py`

You need to add `Question`, `Choice`, and `Submission`

<details>
	<summary>Solution</summary>

	from .models import Course, Lesson, Instructor, Learner, Question, Choice, Submission
</details>

## Create QuestionInline and ChoiceInline

Create `QuestionInline` and `ChoiceInline` classes so that you could edit them together on one page in the admin site.

<details>
	<summary>Hint</summary>

	class Class_Name(admin.StackedInline):
		model = Model_Name
		extra = 2
</details>

<details>
	<summary>Solution</summary>

	class ChoiceInline(admin.StackedInline):
		model = Choice
		extra = 2

	class QuestionInline(admin.StackedInline):
		model = Question
		extra = 2
</details>

## Create QuestionAdmin class

<details>
	<summary>Hint</summary>

	class QuestionAdmin(admin.ModelAdmin):
		inlines = [Question_sub_content]
		list_display = ['content']

</details>

<details>
	<summary>Solution</summary>

	class QuestionAdmin(admin.ModelAdmin):
		inlines = [ChoiceInline]
		list_display = ['content']
</details>


## Register Question, Choice, and Submission

After you register the new models, you could create a new course with lessons, questions, and question choices using the admin site.

The register decorator: `register(*models, site=django.contrib.admin.sites.site)`

<details>
	<summary>Hint</summary>

	admin.site.register(Model1, Model2)
	admin.site.register(Model3)
</details>

<details>
	<summary>Solution</summary>

	admin.site.register(Question, QuestionAdmin)
	admin.site.register(Choice)
	admin.site.register(Submission)
</details>

See the final `admin.py` here:

<details>
	<summary>Solution</summary>

	from django.contrib import admin
	# <HINT> Import any new Models here
	from .models import Course, Lesson, Instructor, Learner, Question, Choice, Submission

	# <HINT> Register QuestionInline and ChoiceInline classes here


	class LessonInline(admin.StackedInline):
		model = Lesson
		extra = 5

	class ChoiceInline(admin.StackedInline):
		model = Choice
		extra = 2

	class QuestionInline(admin.StackedInline):
		model = Question
		extra = 2

	# Register your models here.
	class CourseAdmin(admin.ModelAdmin):
		inlines = [LessonInline]
		list_display = ('name', 'pub_date')
		list_filter = ['pub_date']
		search_fields = ['name', 'description']

	class QuestionAdmin(admin.ModelAdmin):
		inlines = [ChoiceInline]
		list_display = ['content']

	class LessonAdmin(admin.ModelAdmin):
		list_display = ['title']


	# <HINT> Register Question and Choice models here

	admin.site.register(Course, CourseAdmin)
	admin.site.register(Lesson, LessonAdmin)
	admin.site.register(Instructor)
	admin.site.register(Learner)
	admin.site.register(Question, QuestionAdmin)
	admin.site.register(Choice)
	admin.site.register(Submission)
</details>

**Assessment**: Take a screen capture of the `admin.py` and save the screen capture as `02-admin-file.png`.

![admin file sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/02-admin-file.png "admin file sample")

## Create an admin user

Let\'s create an admin user with the following details:

1. Username: `admin`
1. Email address: _leave blank by pressing enter_
1. Password: Your choice, or use `p@ssword123`

```bash
python3 manage.py createsuperuser
```

## Save your changes

Run the Django development server and check if you can add Question and Choice objects using the admin site.

```bash
python3 manage.py runserver
```

[Launch Django admin]({{TRAFFIC_HOST1_8000}}/admin)

**Assessment**: Take a screen capture of the admin site and save the screen capture as `03-admin-site.png`.

![admin site sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/03-admin-site.png "admin site sample")
