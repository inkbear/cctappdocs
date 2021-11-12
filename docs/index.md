# CCT App Overview

## Purpose

The CCT App is a website framework that allows the creation, managment and use of 
the Comuterized Comprehension Task in a virtual lab setting.

## Model

The CCT App is designed to faclitate usage by multiple labs and primary investigators 
by using a multi-tenancy model. This allows the owner of the CCT to manage multiple 
versions and assign those versions to individula PI. 

These PI in turn manage their own spaces building projects composed of a CCT version,
collaborators, staff, and participants grouped into cohorts. Experiments are 
distributed and data is collected at the project level ensuring all data access is 
managed by the PI owning the project.

<iframe width="650" height="400" src="https://www.youtube.com/embed/70FCQJ2C4Q4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Technology

The following technologies support the CCT App:

* OS Agnostic (built on MacOS)
* PHP
* MySQL Database
* Apache Web Server
* Laravel Single Page Application (SPA) Framework
* Voyager Admin for Laravel
* Voyager Front End (optional for public facing pages on the same URL)
* JSpych JavaScript Libraries

All components are open source and minimally modified to ensure portablity and 
access to developer resources. CCT App itself is Open Source.

## Resources

[XAMPP Web Server](https://www.apachefriends.org/download.html) runs an OS agnostic 
Apache web server with PHP and MySQL included.

[Laravel](http://laravel.com) single page application using the Model View Controller 
style framework.

[Voyager Admin](https://voyager.devdojo.com) security for Laravel. 

[JSpych](http://jspsych.org) JavaScript library for creating experiments. 
