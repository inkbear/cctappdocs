# Owner Guide

## Responsiblities

The CCT owner is responsible for:

1. Building and maintaing CCT versions
1. Provisioning CCT objects to PI

## Registering for an Account

If an account is not created already you can register a new account and get a role assignment later.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SAWC3Wvj0Ws" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## How to Video
This video shows how to create a project, add team members, create cohorts, add participants, and run experiments. It is recommended for all team members.

<iframe width="560" height="315" src="https://www.youtube.com/embed/UXbA6cvZPAM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Building and Provisioning a CCT

<iframe width="560" height="315" src="https://www.youtube.com/embed/3jul-U8V4Yo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Building a CCT
CCT are served to participants as a single HTML/JavaScript page. All rendering 
and logic on the page is performed by JPsych libraries.

The page is assembled from the following components:

* HTML template
* JPsych JavaScript Libaries
* JPsych CSS
* JavaScript inclusion file with JSON cnfiguration data
* Media assets (images and audio files)
* Metadata from MySQL inserted into the HTML template.

To build the CCT:

1. Prepare the images with at 1280px x 1024px on a background of #7488de.
1. Upload the audio files, image files, and .js file using the Media Manager. 
making note of the folder path.
1. Enter the CCT metadata using the CCT admin

### Provisioning the CCT

User accounts with role of PI may be added to the CCT in the CCT admin. User 
accounts to be added must have the role of PI assigned to them in the user admin.

## Notes

The owner dashboard will not show data on the cards as the information is not relevant to the 
owner role. The dashboard will be updated later to show owner information.
