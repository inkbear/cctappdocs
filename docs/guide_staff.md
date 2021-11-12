# Staff Guide

## Responsiblities

The Staff assist the PI on:

1. Adding participants to cohorts
1. Running experiments

## Registering for an Account

If an account is not created already you can register a new account and get a role assignment later.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SAWC3Wvj0Ws" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## How to Video
This video shows how to create a project, add team members, create cohorts, add participants, and run experiments. It is recommended for all team members.

<iframe width="560" height="315" src="https://www.youtube.com/embed/UXbA6cvZPAM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Adding Participants

Participants are added in the participant admin. A subject naming convention 
that allows for identification outside the system without using any personally 
identifying information is highly recommended.

Particiants are added to a cohort which must be created by the PI on this screen as well.

## Creating Experiments

An Experiment is a group of participant links and additional data generated from project data.

In the list of projects click the green __Make Links__ button. The project data will be used to
create a unique experiment containing the experiment data and a collection of links for use by 
participants. The __Make Links__ needs to be run after the addition of new particpants to generate the new links. Existing links will not be modified.

The required minimum amount of configuration to generate an experiment are:

* A project (created by the PI)
* A CCT version (added to the project by the PI)
* A Cohort (created by the PI, with at least one participant).

If the experiment and its links are successfully generated the next view will be the experiment detail page with the the experiment data and a list of links. The links can be navigated to via the Experiments page which will list only the links for a single experiment or via the Links page which lists links for all projects the staff member is associated with.

When links are generated the CCT Form (a or b) will be set randomly by the system. The assigned form can be manually switched by clicking the edit button for the link.

## Viewing the CCT Instrument

There are three buttons associated with each link:

1. **Preview** will load the CCT and not record any data in the database.
1. **Test** will load the CCT and recode data to the database flagged as test data
1. **Copy Link** will put the URL that is in the text field on the same line on the clipboard for pasting into a browser for use (or pasted into a document). 

## Viewing Collected Experiment Data

The Experiment Data page lists all data collections ordered by project and experiment creation date. If data has been collected buttons will show for accessing the data (view test data, view live data, download all data). Downloads are in CSV format and contain all columns except some system data e.g. "soft delete date".

Viewing a single item will show all the participant responses for each generated link. If a link was used more than once the run umber will be incremeted alphabetically (A, B, C ...). Be sure to scroll left/right to view all the columns!

## Dashboard

The landing page after logging in is the dashboard. The cards on the dashboard show summary information for:

* __Project Summary__ displays the project build (CCT, Cohort, Experiment).
* __Experiment Progress__ displays the percentage of completed live data collections.
* __Cohort Summary__ list of cohorts regardless of project assignment with number of participants in the cohort.
* __My CCT for Project Assignment__ list of CCT assigned to you as PI by the CCT owner.
