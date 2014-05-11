This is your new Play application
=====================================

This file will be packaged with your application, when using `play dist`.

HTML structure
---------------
'app/views/index.scala.html' calls 'app/views/main.scala.html' which displays the HTML layout.

Reverse Routing
---------------
Generate URLs programmatically from calls to the routes controller

```
Redirect(routes.Application.tasks())
```

 Data Binding
------
Binding a form in a view template

```
@inputText(taskForm("label"))
```

Binding form data in an Action via the request

```scala
def newTask = Action { implicit request =>
    taskForm.bindFromRequest.fold(
      errors => BadRequest(views.html.index(Task.all, errors)),
      label => {
        Task.create(label)
        Redirect(routes.Application.tasks())
      }
    )
  }
```

`fold` provides a mechanism to handle errors and success cases

Database Schema Migration
-------------------------
Play evolutions provides an sql script-based mechanism to migrate schemas inline with application development.  Migration scripts are versioned '1.sql' '2.sql' etc and support a rollback facility:

```sql
# --- !Ups

CREATE SEQUENCE task_id_seq;
CREATE TABLE task (
    id integer NOT NULL DEFAULT nextval('task_id_seq'),
    label varchar(255)
);
 
# --- !Downs
 
DROP TABLE task;
DROP SEQUENCE task_id_seq;
```