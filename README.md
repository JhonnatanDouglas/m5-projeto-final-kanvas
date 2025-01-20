
# Kanvas - EAD Course Management API

## Project Description

The Kanvas project is a REST API developed in Python with the Django framework and Django Rest Framework (DRF) for managing courses and classes in an EAD (distance learning) school. This API allows for the creation of users, courses, course content, and the association of students with courses. It offers authentication based on JSON Web Tokens (JWT) and includes Swagger documentation to facilitate usage and testing.

## Table of Contents

- [Overview](#overview)
- [Project Description](#project-description)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Relationship Setup](#relationship-setup)
- [Routes](#routes)
  - [POST /api/accounts/](#post-apicourses)
  - [POST /api/login/](#post-apilogin)
  - [POST /api/courses/](#post-apicourses)
  - [GET /api/courses/](#get-apicourses)
  - [GET /api/courses/<course_id>/](#get-apicoursescourse_id)
  - [PATCH /api/courses/<course_id>/](#patch-apicoursescourse_id)
  - [DELETE /api/courses/<course_id>/](#delete-apicoursescourse_id)
  - [POST /api/courses/<course_id>/contents/](#post-apicoursescourse_idcontents)
  - [GET /api/courses/<course_id>/contents/<content_id>/](#get-apicoursescourse_idcontentscontent_id)
  - [PATCH /api/courses/<course_id>/contents/<content_id>/](#patch-apicoursescourse_idcontentscontent_id)
  - [DELETE /api/courses/<course_id>/contents/<content_id>/](#delete-apicoursescourse_idcontentscontent_id)
  - [PUT /api/courses/<course_id>/students/](#put-apicoursescourse_idstudents)
  - [GET /api/courses/<course_id>/students/](#get-apicoursescourse_idstudents)
  - [GET /api/docs/](#get-apidocs)

#### Project Diagram

![Kanvas ER Diagram](https://imgur.com/6ovallL.png)

## Technologies Used

- Python 3.11.5
- Django 4.2.6
- Django Rest Framework (DRF) 3.14.0
- PostgreSQL (Database)
- drf-spectacular 0.26.5 (Documentation)
- Gunicorn 21.2.0 (Web Server)
- Other libraries listed in `requirements.txt`

## Project Structure

The project follows a common Django folder structure, with the main app named `_core`. The apps related to the ERD are named according to the specifications:

- `accounts`: User management.
- `courses`: Course management.
- `contents`: Course content management.
- `students_courses`: Management of student-course associations.

## Relationship Setup

The project follows the relationship rules defined in the ERD:

- 1:N relationship between `Account` and `Course` with the `instructor` attribute.
- 1:N relationship between `Course` and `Content` with the `course` attribute.
- N:N relationship between `Account` and `Course` with a custom pivot table. The foreign key referencing `Account` in the pivot table is called `student`, and the one referencing `Course` is called `course`.

## Routes

Here are some of the main routes of the API:

### Route: `POST /api/accounts/`

**Create user (students or super users)**

Request Body (JSON):

```json
{
	"username": "annie",
	"password": "1234",
	"email": "annie@kenzie.com.br",
	"is_superuser": true // Optional
}
```

Response (JSON) - Status Code 201 (Created):

```json
{
	"id": "f6b0bc63-5bf2-4304-943e-a630cf65c895",
	"email": "annie@kenzie.com.br",
	"username": "annie",
	"is_superuser": true
}
```

### Route: `POST /api/login/`

**User login**

### Route: `POST /api/courses/`

**Create courses**

Request Body (JSON):

```json
{
	"name": "Python",
	"start_date": "2023-08-28",
	"end_date": "2023-10-28",
	"instructor": "f6b0bc63-5bf2-4304-943e-a630cf65c895"  // optional
}
```

Response (JSON) - Status Code 201 (Created):

```json
{
	"id": "ee255d92-6f3d-48f9-a938-95a8a128e096",
	"name": "Python",
	"status": "not started",
	"start_date": "2023-08-28",
	"end_date": "2023-10-28",
	"instructor": "f6b0bc63-5bf2-4304-943e-a630cf65c895",
	"contents": [],
	"students_courses": []
}
```

### Route: `GET /api/courses/`

**List courses**

### Route: `GET /api/courses/<course_id>/`

**Search course by ID**

### Route: `PATCH /api/courses/<course_id>/`

**Update course data**

### Route: `DELETE /api/courses/<course_id>/`

**Delete course**

### Route: `POST /api/courses/<course_id>/contents/`

**Create content and associate it with the course**

Request Body (JSON):

```json
{
	"name": "Lesson - Syntax, Variables and Types",
	"content": "Python is a fully object-oriented language. Everything in Python is an object...",
	"video_url": "https://vimeo.com/846236812/71bf004be0?share=copy" // optional
}
```

Response (JSON) - Status Code 201 (Created):

```json
{
	"id": "5aef6df3-3085-43d5-8205-eb12cda45b9e",
	"name": "Lesson - Syntax, Variables and Types",
	"content": "Python is a fully object-oriented language...",
	"video_url": "https://vimeo.com/846236812/71bf004be0?share=copy"
}
```

### Route: `GET /api/courses/<course_id>/contents/<content_id>/`

**Search content by ID**

### Route: `PATCH /api/courses/<course_id>/contents/<content_id>/`

**Update content data**

### Route: `DELETE /api/courses/<course_id>/contents/<content_id>/`

**Delete content**

### Route: `PUT /api/courses/<course_id>/students/`

**Add students to the course**

Request Body (JSON):

```json
{
	"students_courses": [
		{
			"student_email": "annie@kenzie.com.br"
		}
	]
}
```

Response (JSON) - Status Code 200 (Ok):

```json
{
	"id": "e2ffa6db-917f-4904-966e-9fd3cc08adb4",
	"name": "Node",
	"students_courses": [
		{
			"id": "26764139-98eb-4f54-b534-2fd14113c9c7",
			"student_id": "f6b0bc63-5bf2-4304-943e-a630cf65c895",
			"student_username": "annie",
			"student_email": "annie@kenzie.com.br",
			"status": "pending"
		}
	]
}
```

### Route: `GET /api/courses/<course_id>/students/`

**List students in the course**

### Route: `GET /api/docs/`

**View Swagger or Redoc documentation**
