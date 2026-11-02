# Task 3: Update the Course Detail Template

You will now update the course detail template to create an exam section with a list of questions and choices.

One exam contains multiple questions, and each should have more than one correct answer (multiple-selection).

![Take exam](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/take_exam.png "Take exam")

The changes will be made in `templates/onlinecourse/course_details_bootstrap.html`

**File:** `/home/project/tfjzl-final-cloud-app-with-database/onlinecourse/templates/onlinecourse/course_detail_bootstrap.html`

Start editing the code in the placeholder provided:

1. If the user is authenticated, show the course exam with a list of questions and choices:

	<details>
		<summary>Hint</summary>

		{% if CONDITION %}
		</br>
		<!-- Remaining code will go here -->
		{% endif %}
	</details>

	<details>
		<summary>Solution</summary>

		{% if user.is_authenticated %}
		</br>
		<!-- Remaining code will go here -->
		{% endif %}
	</details>

1. Add a button to start the exam:

	<details>
		<summary>Hint</summary>

		<TAG class="CLASS CLASS-primary CLASS-block" data-toggle="collapse" data-target="#exam">Start Exam</TAG>
	</details>

	<details>
		<summary>Solution</summary>

		<button class="btn btn-primary btn-block" data-toggle="collapse" data-target="#exam">Start Exam</button>
	</details>

1. Add a collapsable `div`:

	<details>
		<summary>Hint</summary>

		<TAG id="exam" class="collapse">
		</TAG>
	</details>

	<details>
		<summary>Solution</summary>

		<div id="exam" class="collapse">
		</div>
	</details>

1. Add the Question logic inside a `form`:

	<details>
		<summary>Hint</summary>

		<div id="exam" class="collapse">
			<form id="questionform" action="{% url 'onlinecourse:submit' course.id %}" method="POST">
				LOOP COURSE QUESTIONS HERE
			</form>
		</div>
	</details>

	<details>
		<summary>Solution</summary>

		<div id="exam" class="collapse">
			<form id="questionform" action="{% url 'onlinecourse:submit' course.id %}" method="POST">
				{% for question in course.question_set.all %}
					<!-- Question UI components will go here -->
				{% endfor %}
			</form>
		</div>
	</details>

1. Add Question UI:
	
	<details>
		<summary>Hint</summary>

		<div class="card mt-1">
			<div class="card-header"><h5>{{ question.PROPERTY }}</h5></div>
			{% csrf_token %}
			<div class="form-group">

			</div>
		</div>
	</details>

	<details>
		<summary>Solution</summary>

		<div class="card mt-1">
			<div class="card-header"><h5>{{ question.content }}</h5></div>
			{% csrf_token %}
			<div class="form-group">
				<!-- Choices components go here -->
			</div>
		</div>
	</details>


1. Add Choices components:

	<details>
		<summary>Hint</summary>

		{% for ITEM in question.FIELD.all %}
		<div class="form-check">
			<label class="form-check-label">
				<input type="checkbox" name="choice_{{choice.IDENTIFIER}}"
					   class="form-check-input" id="{{choice.IDENTIFIER}}"
					   value="{{choice.IDENTIFIER}}">{{ choice.FIELD }}
			</label>
		</div>
		{% endfor %}
	</details>

	<details>
		<summary>Solution</summary>

		{% for choice in question.choice_set.all %}
		<div class="form-check">
			<label class="form-check-label">
				<input type="checkbox" name="choice_{{choice.id}}"
					   class="form-check-input" id="{{choice.id}}"
					   value="{{choice.id}}">{{ choice.content }}
			</label>
		</div>
		{% endfor %}
	</details>


### Final solution
View the final solution here:

<details>
    <summary>Solution</summary>

    {% if user.is_authenticated %}
    </br>
    <button class="btn btn-primary btn-block" data-toggle="collapse" data-target="#exam">Start Exam</button>
    <div id="exam" class="collapse">
        <form id="questionform" action="{% url 'onlinecourse:submit' course.id %}" method="POST">
            {% for question in course.question_set.all %}
            <div class="card mt-1">
                <div class="card-header">
                    <h5>{{ question.content }}</h5>
                </div>
                {% csrf_token %}
                <div class="form-group">
                    {% for choice in question.choice_set.all %}
                    <div class="form-check">
                        <label class="form-check-label">
                            <input type="checkbox" name="choice_{{choice.id}}" class="form-check-input"
                                id="{{choice.id}}" value="{{choice.id}}">{{ choice.content }}
                        </label>
                    </div>
                    {% endfor %}
                </div>
            </div>
            {% endfor %}
            <input class="btn btn-success btn-block" type="submit" value="Submit">
        </form>
    </div>
    {% endif %}
</details>

Run in to test:

```bash
python3 manage.py runserver
```

[Launch onlinecourse application]({{TRAFFIC_HOST1_8000}}/onlinecourse)

At this moment, you can not submit the exam. You will be implementing that in the next lab.

### Commit your code

Committing and pushing your code to GitHub is a good practice to avoid losing it.

**Assessment**: Take a screen capture of `course_details_bootstrap.html` and save the screen capture as `04-course-details.png`.

![course bootstrap sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/04-course-details.png "course bootstrap sample")
