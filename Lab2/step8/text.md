# Task 5: Submission Evaluation

Since you have created several new models, you now need to import them at the top of the `views.py`

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/views.py`

```
from .models import Course, Enrollment, Question, Choice, Submission
```
## Submit view

You will now create a function-based view for form submission.

Create a submit view `def submit(request, course_id):` to create an exam submission record for a course enrollment. You may implement it based on the following logic:

- Get the current user and the course object, then get the associated enrollment object
   (HINT: ```Enrollment.objects.get(user=..., course=...)```)

- Create a new submission object referring to the enrollment
   (HINT: ```Submission.objects.create(enrollment=...)```)

- Collect the selected choices from the HTTP request object (HINT: you could use `request.POST` to get the payload dictionary and the choice id from the dictionary values. An example code snippet is also provided.)

- Add each selected choice object to the submission object

- Redirect to a `show_exam_result` view with the submission id to show the exam result

- Configure `urls.py` to route the new `submit` view such as `path('<int:course_id>/submit/', ...),`

### Form submission in `views.py`

<details>
    <summary>Hint</summary>

    def submit(PARAM1, PARAM2):
		course = get_object_or_404(MODEL, pk=PARAM2)
		user = request.OBJECT
		enrollment = Enrollment.objects.get(ARUGMENT1, ARUGMENT2)
		submission = Submission.objects.create(ARGUMENT)
		choices = extract_answers(ARGUMENT)
		submission.choices.set(ARGUMENT)
		submission_id = submission.id
		return HttpResponseRedirect(reverse(viewname='onlinecourse:exam_result', args=(course_id, submission_id,)))
</details>

<details>
    <summary>Solution</summary>

    def submit(request, course_id):
        course = get_object_or_404(Course, pk=course_id)
        user = request.user
        enrollment = Enrollment.objects.get(user=user, course=course)
        submission = Submission.objects.create(enrollment=enrollment)
        choices = extract_answers(request)
        submission.choices.set(choices)
        submission_id = submission.id
        return HttpResponseRedirect(reverse(viewname='onlinecourse:exam_result', args=(course_id, submission_id,)))
</details>

### Route the submit view button in `urls.py`

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/urls.py`

<details>
    <summary>Hint</summary>

	path('<int:course_id>/FUNCTION/', views.FUNCTION, name="submit"),
</details>

<details>
    <summary>Solution</summary>

    path('<int:course_id>/submit/', views.submit, name="submit"),
</details>

## Evaluation view

Create an exam result view `def show_exam_result(request, course_id, submission_id):` to check if the learner passed the exam and their question results.

You may implement the view based on the following logic:
   - Get the course object and submission object based on their ids in view arguments

   - Get the selected choice ids from the submission record

   - For each selected choice, check if it is a correct answer or not

   - Calculate the total score by adding up the grades for all questions in the course

   - Add the course, choice, and grade to context for rendering HTML page

   - Configure `urls.py` to route the new `show_exam_result` view such as `path('course/<int:course_id>/submission/<int:submission_id>/result/', ...),`

### Exam results in `views.py`

<details>
    <summary>Hint</summary>
	
	def show_exam_result(request, PARAM1, PARAM2):
		context = {}
		course = get_object_or_404(MODEL1, pk=PARAM1)
		submission = MODEL2.objects.get(id=PARAM2)
		choices = submission.choices.all()

		total_score = 0
		questions = course.RELATION_SET.all()  # Assuming course has related questions

		for question in questions:
			correct_choices = question.RELATION_SET.filter(ARGUMENT1=True)  # Get all correct choices for the question
			selected_choices = choices.filter(ARGUMENT2=question)  # Get the user's selected choices for the question

			# Check if the selected choices are the same as the correct choices
			if set(ARGUMENT3) == set(ARGUMENT4):
				total_score += question.ATTRIBUTE  # Add the question's grade only if all correct answers are selected

		context['KEY1'] = course
		context['KEY2'] = total_score
		context['KEY3'] = choices

		return render(request, 'TEMPLATE_PATH', context)

</details>

<details>
    <summary>Solution</summary>

	def show_exam_result(request, course_id, submission_id):
		context = {}
		course = get_object_or_404(Course, pk=course_id)
		submission = Submission.objects.get(id=submission_id)
		choices = submission.choices.all()

		total_score = 0
		questions = course.question_set.all()  # Assuming course has related questions

		for question in questions:
			correct_choices = question.choice_set.filter(is_correct=True)  # Get all correct choices for the question
			selected_choices = choices.filter(question=question)  # Get the user's selected choices for the question

			# Check if the selected choices are the same as the correct choices
			if set(correct_choices) == set(selected_choices):
				total_score += question.grade  # Add the question's grade only if all correct answers are selected

		context['course'] = course
		context['grade'] = total_score
		context['choices'] = choices

		return render(request, 'onlinecourse/exam_result_bootstrap.html', context)
</details>

### Exam results in `urls.py`

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/urls.py`

<details>
    <summary>Hint</summary>

	path('course/<int:course_id>/submission/<int:submission_id>/result/', views.FUNCTION, name="exam_result"),
</details>

<details>
    <summary>Solution</summary>

    path('course/<int:course_id>/submission/<int:submission_id>/result/', views.show_exam_result, name="exam_result"),
</details>

**Assessment**: Take a screen capture of `views.py` and save the screen capture as `05-views.png`.

![05-views-new.png.png](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/8f-nJVer5axNXAu_WGQDBQ/05-views-new-png.png)

**Assessment**: Take a screen capture of `urls.py` and save the screen capture as `06-urls.png`.

![urls sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/06-urls.png "urls sample")
