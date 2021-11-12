# Running Experiments

Experiments are driven by projects which at a minimum contain the CCT version and 
a cohort comprised of participants.

Assuming the PI has been provisioned at least one CCT, the PI will need to:

1. Create a project
1. Assign a CCT to the project
1. Create a cohort
1. Add participants the cohort
1. Assign the cohort to a project
1. Create an experiment from the project
    * Preview the experiment
    * Test the experiment
1. Run the live experiment (via links sent to participants)
1. Review and export data

## Creating a Project

Projects are managed in the project admin. Only some basic information is required, 
remaining components may be added later as they are available.

Projects are the main containers that hold:

* A single CCT version
* Zero or more collaborators
* Zero or more staff
* A single cohort of participants

The CCT available for linking is based on the CCT assigned by the CCT Owner to a PI.

Selection choices for collaborators and staff include all registered users in the
system except thos accounts with a role of CCT Owner or Admin. This allows unique combinations of teams regardless 
of roles on other projects, e.g. a grad student could be a PI on one project while acting 
as staff or collaborator on another project.

See also: [PI guide - Creating a Project](guide_pi.md#creating-projects) 
or [Collaborator Guide - Adding to Projects](guide_collaborator.md#adding-to-a-Project).

## Creating a Cohort

Cohorts are the grouping of participants to assign them as a group to projects.
Participants may belong to multiple cohorts. 

Cohorts are created and managed in the cohort admin but are assigned to projects in the project admin.

See also: [PI](guide_pi.md#creating-cohorts), [Collaborator](guide_collaborator.md#creating-cohorts), or [Staff](guide_staff.md#creating-cohorts) guide.

## Assigning a Corhort to a Project

Cohorts are assigned to projects in the project admin.

See also: [PI](guide_pi.md#creating-cohorts), [Collaborator](guide_collaborator.md#creating-cohorts), or [Staff](guide_staff.md#creating-cohorts) guide.

## Creating Experiments

An Experiment is a group of participant links and additional data generated from project data.

In the list of projects click the green __Make Links__ button. The project data will be used to
create a unique experiment containing the experiment data and a collection of links for use by 
participants.

The required minimum amount of configuration to generate an experiment are:

* A project 
* A CCT version 
* A Cohort (with at least one participant).

If the experiment and its links are successfully generated the next view will be the experiment detail page with the the 
experiment data and a list of links.

The default CCT Form will be set to Form A. Edit the links to switch to Form B based on preference (usually a randomization of particpants done offline).

See also: [PI](guide_pi.md#creating-experiments), [Collaborator](guide_collaborator.md#creating-experiments), 
or [Staff](guide_staff.md#creating-experiments) guide.

## Previewing and Testing the Experiment

On the resulting experiment detail page, each row containing a link will have buttons for preview, test, and copy. 
hese links and the buttons my be reached by navigating on the main memu to __Experiments__ >> __View__ or __Links__.

The three experiments modes are:

* Preview: Opens the experiment in the browser, does __not__ record the test results in the database but will show 
the test results as the last screen of the test.
* Test: Opens the experiment in the browser and records the test results in the database (flagged as a test run). The 
view is identical to the live view with test results not shown on the last screen.
* Copy Link: Copies the live link to the clipbard for pasting into an email to the recipient or other method of sharing.

## Running the Experiment

The links contain a token unique to each participant/experiment combination, thus all data are kept private. Note, it is important to send the appropriate link and not reuse links to prevent data ambiguity later.

## Viewing Collected Data

The Data Collection page lists all data collections ordered by project and experiment creation date. 
Viewing a single item will show all the participant responses for each generated link. If a link was used more
than once, there will be multiple entries. Each run of the CCT will increment an alphabetic (A - Z) Run ID. Note the increment values will go to AA, AB, AC after Z). 