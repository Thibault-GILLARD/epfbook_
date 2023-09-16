# epfbook
<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h2 align="center">Weakly Supervised Learning for Land Cover
Mapping of Satellite Image Time Series via
Attention-Based CNN
Epfbook</h3>
  <a href="https://www.epf.fr/en">
    <img src="https://upload.wikimedia.org/wikipedia/fr/e/e9/EPF_logo_2021.png" alt="Logo" width="211" height="179">
  </a>
  <p align="center">
    Code developed for the web programming course at epf whose goal was "Create your own APIs to share data".
    <br />
    <a href="https://github.com/Thibault-GILLARD/epfbook"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/Thibault-GILLARD/epfbook/issues">Report Bug</a>
    ·
    <a href="https://github.com/Thibault-GILLARD/epfbook/issues">Request Feature</a>
  </p>
</div>


# Web Development Course

This repository contains the code and materials for the Web Development course. Each toggle point represents a different module of the course.

<details>
<summary><strong>TP1: API Consumption, Basics of HTTP, Basics of JavaScript</strong></summary>
<p>

Learn how web browsers (such as Chrome, Firefox, Safari, etc.) retrieve web pages and data from the internet. Explore API consumption, HTTP basics, and JavaScript fundamentals.

</p>
</details>

<details>
<summary><strong>TP2: Creating Our Own API with Node.js</strong></summary>
<p>

Learn about creating your own API using Node.js. Serve and update data stored in a CSV file, specifically a list of students. Focus on the server-side of web development and how websites make web pages and data available to their users.

</p>
</details>

<details>
<summary><strong>TP3: Develop and Serve a Graphical Interface</strong></summary>
<p>

Learn to develop and serve a graphical interface in the browser using HTML, CSS, and JavaScript. Implement forms, handle responsiveness, and grasp the basics of front-end web development.

</p>
</details>

<details>
<summary><strong>TP4: Secure Your Pages with Password-based Authentication</strong></summary>
<p>

Learn how to secure your web pages with password-based authentication. Explore techniques to make certain pages and data private, accessible only to authenticated users or members.

</p>
</details>

<details>
<summary><strong>TP5: Create Advanced Data Visualizations with JavaScript (using D3.js)</strong></summary>
<p>

Learn how to create advanced data visualizations using JavaScript, specifically leveraging the power of D3.js. Explore different techniques to display data in an aesthetically pleasing and informative manner.

</p>
</details>

<details>
<summary><strong>TP6: Connect to a Database and Deploy in a Real-life Environment</strong></summary>
<p>

Learn how to connect to a database and deploy your web application in a real-life environment. Explore concepts related to storing persistent data and retrieving them when needed. Additionally, gain insights into the deployment process.

</p>
</details>

# EPFBook Application

Welcome to the EPFBook application!

## Final Exam

To successfully complete the final I will have to be able to:

1. Implement a Details View for Each Student:
2. Update Existing Student:

----
### Requirements

#### Installation

To install the project dependencies, run the following command:

```shell
npm install
```

#### Start the app

To start the application on port 3000, use the following command:

```shell
npm run dev
```

#### Basic Authentication

The application uses basic authentication. The username is `admin` and the password is `admin`.

#### Information 

the route `/students/1` exists.

----

## Rick and Morty API

The character 5 in the Rick and Morty API is Jerry Smith.

![Jerry Smith](https://rickandmortyapi.com/api/character/avatar/5.jpeg)

- Name: Jerry Smith
- Status: Alive
- Species: Human
- Gender: Male
- Origin: Earth (Replacement Dimension)
- Location: Earth (Replacement Dimension)

## Explaination of the code 


### 1. Implement a Details View for Each Student:

In the `app.js` file, I added the following code:

```javascript
app.get('/students/:id', (req, res) => {
  const studentId = parseInt(req.params.id); // get the student ID from the URL and convert it to an integer

  getStudentsFromCsvfile((err, students) => {
    if (err) { // if there is an error, send an error message to the client
      console.error(err);
      res.send("Oops! Something went wrong. Please try again later.");
    }
    if (studentId >= 0 && studentId < students.length) { // if the student ID is valid, render the student details page
      const student = students[studentId];
      res.render('student_details', { student, studentId });
    } else {
      res.send('Oops! The student it looks like you are looking for a ghost. Please try again later.');
    }
  });
});
```

This code does the following:
- It defines a new route for the student details page. The route is `/students/:id`, where `:id` is a placeholder for the student ID.
- It gets the student ID from the URL and converts it to an integer.
-  It calls the `getStudentsFromCsvfile` function to get the list of students from the CSV file.
-  If there is an error, it sends an error message to the client.
-  If the student ID is valid, it renders the student details page.
-  If the student ID is invalid, it sends an error message to the client.


Exercice 1 version of student_details.ejs

```html
<html>
  <head>
    <title>Student Details</title>
    <link rel="stylesheet" href="/style.css">
  </head>
  <body>
    <h1>Student Details</h1>
    <ul>
      <li>Name: <%= student.name %></li>
      <li>School: <%= student.school %></li>
    </ul>
  </body>
</html>
```

It just displays the student details.

### 2. Update Existing Student:

In the `app.js` file, I added the following code:

```javascript
app.post('/students/:id', (req, res) => {
  const studentId = parseInt(req.params.id); 

  getStudentsFromCsvfile((err, students) => {
    if (err) {
      console.error(err);
      res.send("Oops! Something went wrong. Please try again later.");
      return;
    }
    if (studentId >= 0 && studentId < students.length) {
      students[studentId] = { 
        name: req.body.name,
        school: req.body.school,
      };
      const csvData = ['name,school']; // Create a new array with the header

      students.forEach((student) => {
        csvData.push(`${student.name},${student.school}`); // Add a new line for each student
      });

      const updatedCsvData = csvData.join('\n'); // Join all the lines with a new line character

      fs.writeFile('./students.csv', updatedCsvData, (err) => {
        if (err) {
          console.error(err);
          res.send("Oops! Something went wrong. Please try again later.");
        }
        res.redirect('/students');
      });
    } else {
      res.send('Oops! The student it looks like you are looking for a ghost. Please try again later.');
    }
  });
}); 
```

This code just permit to update the student details with the new information.

For that I had to add in the `student_details.ejs` filethe needed form (with the method post):

```html
<html>
  <head>
    <title>Student Details</title>
    <link rel="stylesheet" href="/style_sd.css">
  </head>
  <body>
    <div class="logo">
      <img src="https://upload.wikimedia.org/wikipedia/fr/e/e9/EPF_logo_2021.png" alt="EPF Troyes Logo">
    </div>
    <h1>Student Details</h1>
    <form method="post" action="/students/<%= studentId %>">
      <label for="name">Name:</label>
      <input type="text" id="name" name="name" value="<%= student.name %>" required>

      <label for="school">School:</label>
      <input type="text" id="school" name="school" value="<%= student.school %>" required>

      <button type="submit">Update</button>
    </form>
  </body>
</html>
```
---
### Bonus Done 

-Handle the scenario where the id parameter does not match any student in the
details view:
  
```javascript
if (studentId >= 0 && studentId < students.length) {
    students[studentId] = { 
````

-Make a beautiful app using CSS: add a background-color, add a logo (for example
EPF logo), add margins, add a custom font...






