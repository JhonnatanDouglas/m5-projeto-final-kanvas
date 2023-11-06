
# Kanvas - API de Gerenciamento de Cursos EAD

## Descrição do Projeto 

O projeto Kanvas é uma API REST desenvolvida em Python com o framework Django e Django Rest Framework (DRF) para o gerenciamento de cursos e aulas em uma escola de modalidade EAD. Esta API permite a criação de usuários, cursos, conteúdos de cursos e a associação de alunos aos cursos. Ela oferece autenticação baseada em JSON Web Token (JWT) e inclui documentação Swagger para facilitar o uso e teste.

## Índice

- [Visão Geral](#visão-geral)
- [Descrição do Projeto](#descrição-do-projeto)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Configuração dos Relacionamentos](#configuração-dos-relacionamentos)
- [Rotas](#rotas)
  - [POST /api/accounts/](#rota-post-apiaccounts)
  - [POST /api/login/](#rota-post-apilogin)
  - [POST /api/courses/](#rota-post-apicourses)
  - [GET /api/courses/](#rota-get-apicourses)
  - [GET /api/courses/<course_id>/](#rota-get-apicoursescourse_id)
  - [PATCH /api/courses/<course_id>/](#rota-patch-apicoursescourse_id)
  - [DELETE /api/courses/<course_id>/](#rota-delete-apicoursescourse_id)
  - [POST /api/courses/<course_id>/contents/](#rota-post-apicoursescourse_idcontents)
  - [GET /api/courses/<course_id>/contents/<content_id>/](#rota-get-apicoursescourse_idcontentscontent_id)
  - [PATCH /api/courses/<course_id>/contents/<content_id>/](#rota-patch-apicoursescourse_idcontentscontent_id)
  - [DELETE /api/courses/<course_id>/contents/<content_id>/](#rota-delete-apicoursescourse_idcontentscontent_id)
  - [PUT /api/courses/<course_id>/students/](#rota-put-apicoursescourse_idstudents)
  - [GET /api/courses/<course_id>/students/](#rota-get-apicoursescourse_idstudents)
  - [GET /api/docs/](#rota-get-apidocs)


#### Diagrama do Projeto

![Kanvas ER Diagram](https://imgur.com/6ovallL.png)

## Tecnologias Utilizadas

- Python 3.11.5
- Django 4.2.6
- Django Rest Framework (DRF) 3.14.0
- PostgreSQL (Banco de Dados)
- drf-spectacular 0.26.5 (Documentação)
- Gunicorn 21.2.0 (Servidor Web)
- Outras bibliotecas listadas em `requirements.txt`

## Estrutura do Projeto

O projeto segue uma estrutura de pasta comum do Django, com o aplicativo principal nomeado como `_core`. Os aplicativos relacionados ao DER foram nomeados de acordo com as especificações:

- `accounts`: Gerenciamento de usuários.
- `courses`: Gerenciamento de cursos.
- `contents`: Gerenciamento de conteúdos de cursos.
- `students_courses`: Gerenciamento da associação de alunos aos cursos.

## Configuração dos Relacionamentos

O projeto segue as regras de relacionamento definidas no DER:

- Relacionamento 1:N entre `Account` e `Course` com atributo `instructor`.
- Relacionamento 1:N entre `Course` e `Content` com atributo `course`.
- Relacionamento N:N entre `Account` e `Course` com tabela pivô customizada. A chave estrangeira referente a `Account` na tabela pivô é chamada de `student` e a referente a `Course` é chamada de `course`.

## Rotas

Aqui estão algumas das principais rotas da API:

### Rota: `POST /api/accounts/`

**Criação de usuário (estudantes ou super usuários)**

Corpo da Requisição (JSON):

```json
{
	"username": "annie",
	"password": "1234",
	"email": "annie@kenzie.com.br",
	"is_superuser": true // Opcional
}
```

Resposta (JSON) - Status Code 201 (Created):

```json
{
	"id": "f6b0bc63-5bf2-4304-943e-a630cf65c895",
	"email": "annie@kenzie.com.br",
	"username": "annie",
	"is_superuser": true
}
```

### Rota: `POST /api/login/`

**Login do usuário**

### Rota: `POST /api/courses/`

**Criação de cursos**

Corpo da Requisição (JSON):

```json
{
	"name": "Python",
	"start_date": "2023-08-28",
	"end_date": "2023-10-28",
	"instructor": "f6b0bc63-5bf2-4304-943e-a630cf65c895"  // opcional
}
```

Resposta (JSON) - Status Code 201 (Created):

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

### Rota: `GET /api/courses/`

**Listagem de cursos**

### Rota: `GET /api/courses/<course_id>/`

**Busca de curso por id**

### Rota: `PATCH /api/courses/<course_id>/`

**Atualização somente dos dados de curso**

### Rota: `DELETE /api/courses/<course_id>/`

**Deleção de curso**

### Rota: `POST /api/courses/<course_id>/contents/`

**Criação de conteúdos e associação ao curso**

Corpo da Requisição (JSON):

```json
{
	"name": "Aula - Sintaxe, Variáveis e Tipos",
	"content": "Python é uma linguagem totalmente orientada a objetos. Tudo em Python é um objeto...",
	"video_url": "https://vimeo.com/846236812/71bf004be0?share=copy" // opcional
}
```

Resposta (JSON) - Status Code 201 (Created):

```json
{
	"id": "5aef6df3-3085-43d5-8205-eb12cda45b9e",
	"name": "Aula - Sintaxe, Variáveis e Tipos",
	"content": "Python é uma linguagem totalmente orientada a objetos...",
	"video_url": "https://vimeo.com/846236812/71bf004be0?share=copy"
}
```

### Rota: `GET /api/courses/<course_id>/contents/<content_id>/`

**Busca de conteúdo por id**

### Rota: `PATCH /api/courses/<course_id>/contents/<content_id>/`

**Atualização somente do conteúdo**

### Rota: `DELETE /api/courses/<course_id>/contents/<content_id>/`

**Deleção de conteúdos**

### Rota: `PUT /api/courses/<course_id>/students/`

**Adição de alunos ao curso**

Corpo da Requisição (JSON):

```json
{
	"students_courses": [
		{
			"student_email": "annie@kenzie.com.br"
		}
	]
}
```

Resposta (JSON) - Status Code 200 (Ok):

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

### Rota: `GET /api/courses/<course_id>/students/`

**Listagem dos estudantes do curso**

### Rota: `GET /api/docs/`

**Visualização da documentação no formato Swagger ou Redoc**
