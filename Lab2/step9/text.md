# Task 6: Complete the Exam Result Template to Show Exam Submission Results

Complete the HTML template `exam_result_bootstrap.html` for submission results which should show if a learner passed the exam with details like the total score, the result for each question, and so on. Check the previous UI design for more details.

Stylize the updated template with `Bootstrap` to meet the UI design from the design team.

## Pass output

Learners who pass the exam should be shown the final score and a congratulations message.

![image](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/exam_passed_new.png)

<details>
    <summary>Hint</summary>

	<b>Congratulations, {{ USER_DETAILS }}!</b> You have passed the exam and completed the course with score {{ GRADE }}/100
</details>

<details>
    <summary>Solution</summary>

    <b>Congratulations, {{ user.first_name }}!</b> You have passed the exam and completed the course with score {{ grade }}/100
</details>

## Fail output

Learners who fail the exam should be shown the final score with incorrect choices. The learner should be allowed to retake the exam and resubmit it.

![image](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/exam-failed-1.png)

<details>
    <summary>Hint</summary>

	<b>Failed</b> Sorry, {{ USER_DETAILS }}! You have failed the exam with score {{ GRADE }}/100
</details>

<details>
    <summary>Solution</summary>

    <b>Failed</b> Sorry, {{ user.first_name }}! You have failed the exam with score {{ grade }}/100

</details>

## Exam result

You must also display the exam results so the learner can see how they did.

<details>
    <summary>Hint</summary>

	LOOP ON QUESTIONS
	<div class="card mt-1">
		<div class="card-header"><h5>QUESTION DETAILS</h5></div>
		<div class="form-group">
			LOOP ON CHOICES
			<div class="form-check">
				IF CHOICE IS CORRECT AND CHOSEN
				<div class="text-success">Correct answer: CHOICE DETAILS</div>
				ELSE IF CHOICE IS CORRECT AND NOT CHOSEN
				<div class="text-warning">Not selected: CHOICE DETAILS</div>
				ELSE IF CHOICE IS NOT CORRECT AND CHOSEN
				<div class="text-danger">Wrong answer: CHOICE DETAILS</div>
				ELSE
				<div>CHOICE DETAILS</div>
				END ALL IFs
			</div>
			END INNER LOOP
		</div>
	</div>
	END LOOP
</details>

<details>
    <summary>Solution</summary>

```
    {% for question in course.question_set.all %}
	<div class="card mt-1">
		<div class="card-header"><h5>{{ question.content }}</h5></div>
		<div class="form-group">
			{% for choice in question.choice_set.all %}
			<div class="form-check">
				{% if choice.is_correct and choice in choices %}
				<div class="text-success">Correct answer: {{ choice.content }}</div>
				{% else %}{% if choice.is_correct and not choice in choices %}
				<div class="text-warning">Not selected: {{ choice.content }}</div>
				{% else %}{% if not choice.is_correct and choice in choices %}
				<div class="text-danger">Wrong answer: {{ choice.content }}</div>
				{% else %}
				<div>{{ choice.content }}</div>
				{% endif %}{% endif %}{% endif %}
			</div>
			{% endfor %}
		</div>
	</div>
	{% endfor %}
```

![final output sample](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/exam_passed_result.png "final output sample")

</details>



**Assessment**: Take a screen capture of a result from the mock exam and save the screen capture as `07-final.png`.

![07-final.png](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/07-final.png "07-final.png")
