Table of Contents:
Set up the Spring Boot application (server).
Configure the application.properties file to connect the database.
Create REST APIs using Spring Boot.
Set up the frontend application.
Integrate the Spring Boot service with the frontend by calling the APIs.
Display data from the Spring Boot API in a table format on the frontend.
Step 1: Set Up Your Spring Boot Application
Use Spring Initializer or an IDE like IntelliJ or Eclipse to create a new Spring Boot project. Add the following dependencies:

Spring Web: For building web applications and REST APIs.
Spring Data JPA: For interacting with the database.
MySQL Driver: To connect to your MySQL database.
Step 2: Configure the application.properties File
1. Open the application.properties file and configure your database settings. Here’s an example configuration:

2. Replace your_database_name, your_username, and your_password with your database details.


 # Server configuration
 server.port = 8081
                    
 # Database configuration
 spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
 spring.datasource.name=your_database_name
 spring.datasource.username=your_username
 spring.datasource.password=your_password
 spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
                    
 # JPA configuration
  spring.jpa.hibernate.ddl-auto=update
                  

Create REST APIs Using Spring Boot: Step-by-Step Guide

Step 3: Create REST APIs Using Spring Boot
To create REST APIs in Spring Boot, we need:

Entity Classes: Representing the database table.
Controller: Defining the endpoints of the API.
Service: Containing the business logic.
Repository: Performing database operations.
Let’s create each of these components step by step.

Entity Class (Emp)

package com.demo.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class Emp {

    @Id
    private String name;
    private int age;
    private int salary;

    public Emp() {
    }

    public Emp(String name, int age, int salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }
}

Controller (for defining api endpoint)

package com.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.demo.entity.Emp;
import com.demo.service.EmpService;

@RestController
public class EmpController {
	
    @Autowired
    private EmpService empService;
	
    @GetMapping("/getEmp")
    public List<Emp> getEmp() {
        return empService.getEmp();
    }
}

Service

package com.demo.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.demo.entity.Emp;
import com.demo.repo.EmpRepo;

@Service
public class EmpService {
	
    @Autowired
    private EmpRepo empRepo;
		
    public List<Emp> getEmp() {
        return empRepo.findAll();
    }
}

Repository

package com.demo.repo;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.demo.entity.Emp;

@Repository
public interface EmpRepo extends JpaRepository<Emp, String> {

}

Project Structure
Here’s an image showing the project structure for better understanding.
<img width="330" height="583" alt="Screenshot 2025-08-13 101852" src="https://github.com/user-attachments/assets/f1f22ab4-bb85-4cdc-9804-609d8cbd1269" />


Spring Boot project structure
Run the Application
Run your Spring Boot application. It will automatically create the database table based on the Emp entity. Add dummy data to the table for testing purposes.
<img width="1194" height="629" alt="Screenshot 2025-08-13 102253" src="https://github.com/user-attachments/assets/fcc87f07-88c2-4f37-98a2-0b972eb64b6d" />


Mysql database with dummy data
Step 4: Set Up Your Frontend Application
Create the following files for the frontend:

index.html: The main file to display the employee details.
style.css: The file for styling the content.
script.js: The JavaScript file used to call the Spring Boot API and fetch data from the database.
Let’s create each of these components one by one.

index.html file

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Employees data</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <h1>Employees Data</h1>

    <table id="empTable">
        <thead>
            <tr>
                <th>Id</th>
                <th>name</th>
                <th>age</th>
                <th>salary</th>
            </tr>
        </thead>
        <tbody id="tableBody">

        </tbody>
    </table>

    <script src="script.js"></script>
</body>
</html>

style.css file

body {
    font-family: Arial, Helvetica, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: white;
}

h1 {
    align-items: center;
    color: #333;
}

table {
    width: 100%;
    border-collapse: collapse;
    background-color: white;
}

tr, td {
    border: 1px solid #ddd;
    text-align: left;
    padding: 12px;
}

th {
    background-color: white;
    font-weight: bold;
    color: #333;
}

tr:hover {
    background-color: #f5f5f5;
}

Use JavaScript to Call the API
This JavaScript file is used to call the Spring Boot API and fetch data from the database.

Every API in Spring Boot has a specific URL, which is defined in the controller. If you look at our controller where we have defined the api endpoint following is our api url. This url we will use for api calling

http://localhost:8081/getEmp

Let’s understand how we call the spring boot api using java Script

JavaScript fetch() method allows you to make HTTP requests to this URL.

Steps to Call Spring Boot api using Javascript:
1. Listen for the Page Load
Ensure the API call is made after the DOM has loaded using DOMContentLoaded.

2. Use fetch() to Make the Request
Call the API using fetch(apiUrl).
Sends an HTTP GET request to the specified URL.
The response from the API is returned as a Promise.
3. Handle the Response
Parse the response into JSON using .json().
Use the JSON data to manipulate the DOM dynamically, such as rendering it in a table.
4. Handle Errors
Use .catch() to handle errors, like network issues or API failures.

script.js file

    document.addEventListener("DOMContentLoaded", function(){
    const apiUrl = "http://localhost:8081/getEmp"
    const tableBody = document.getElementById("tableBody");

    fetch(apiUrl)
        .then(response => response.json())
        .then(data => {

            data.forEach(employees => {
                const row = document.createElement("tr");
                row.innerHTML= `
                 ${employees.id} 
                 ${employees.name} 
                 ${employees.age} 
                 ${employees.salary} 
                `

                tableBody.appendChild(row);
            })

        }).catch(error => {
            console.log("error is comming while api calling " + error);
        })
})
Run Your Frontend Application:
<img width="1189" height="632" alt="Screenshot 2025-08-13 102327" src="https://github.com/user-attachments/assets/14d582b8-0880-4134-aa9b-77e9a6a96126" />
