# Mentor's Guide

This document is a guide for new mentors on how to run the ML/BD Creative Inquiry.

## Mentor Requirements

The ML/BD CI is a creative inquiry that trains undergraduate students in a variety of skills spanning machine learning, data science, and high-performance computing. Students learn through a series of Jupyter notebooks and then through a personal or team-based project, for which they write a report at the end of the semester. As a CI mentor it is helpful but not necessary that you have experience in all three of these disciplines, so that you are best able to guide students through the notebook content and the hurdles they encounter in their projects. Fortunately, most of the course is encoded in the Jupyter notebooks and skills pages on the CI website, so it is only essential that you as a mentor know how to point students in the right direction, to help them when you know the answer and point them to the right resources when you don't. You also at least need to know your way around Palmetto and JupyterHub so that you can help students set up their programming environment.

__Hard Requirements__
- Awareness of all resources available on the CI website
- Proficiency with Palmetto, JupyterHub, Anaconda, Jupyter notebooks
- Ability to explain technical concepts and work through problems with students
- Ability to help students select a focused project from many options

__Soft Requirements__
- Proficiency with Python (keras, matplotlib, numpy, pandas, scikit-learn, tensorflow)
- Proficiency with data science concepts (datasets, data visualization)
- Proficiency with machine learning concepts (supervised learning, unsupervised learning)
- Proficiency with neural networks (MLPs, CNNs, autoencoders)
- Proficiency with GPU computing (CUDA)

In other words, think of yourself not as a teacher but as a _facilitator_. It is the students' responsibility to educate themselves and discover what interests them the most, but it is your job to help them get started on the journey and offer advice when they need it.

## Overview of CI Resources

### Box Folder

The CI Box folder is located [here](https://clemson.app.box.com/folder/11145145746). It contains a lot of old documents that may or may not be of use, but the main things we put in there are (1) semester reports from CI students, (2) honors theses for CI students who do departmental honors with us, and (3) datasets used by former students that we want to keep around for future students. Feel free to use the Box folder for whatever you want but make sure to add the semester reports and honors theses at the end of each semester.

### Website

The CI website is located [here](https://cufctl.github.io/creative-inquiry/). The website has four main sections:

- __Course Information__: instructions on how to register for the course, weekly course schedule, contact information for Dr. Smith and mentors
- __Skills Overview__: list of pages that introduce a variety of skills used in the CI, including the "getting started" instructions
- __Jupyter Notebooks__: list of the Jupyter notebooks used in the course, instructions on how to run them on Palmetto or Google Colab
- __Project Gallery__: list of projects by former students, formatted as Jupyter notebooks, that new students can be inspired by or pick up and improve

### Physical Resources

The CI doesn't really need any physical equipment, since every student does all of their work from their laptop. Of course there is the lab where we have our weekly meetings. We have a big conference table, plenty of chairs, and a giant flatscreen TV. Great for presentations!

## The Semester Rundown

Okay let's go through everything that you have to do in a semester from beginning to end. If you can get this process down then you're golden, just rinse and repeat every semester.

### Before the Semester Starts

Students typically find the CI by searching the CI page or hearing about it from other students. They will usually email you or Dr. Smith asking how to register, as early as the end of the previous semester, or in the weeks leading up to the current semester. Simply direct them to the CI website -- the front page has all of the instructions they need to register for the CI.

I used to meet with students beforehand to make sure they were a good fit for the CI, but now I generally let anyone register, their desire to learn is a good enough reason. Most students are able to find their way through the CI regardless of their experience level, but the most important prior experience is command-line and Python. Encourage students to familiarize themselves with these skills as much as they can before the CI starts.

A week before the first day of classes, email the CI mailing list (CUFCTDLBD@lists.clemson.edu) to welcome everyone to the CI and find the weekly meeting times that work for everyone. I like to use [when2meet](https://www.when2meet.com/) because it's very simple to use. Sometimes when the group is small it's possible to find a single hour slot that works for everyone, but often times it doesn't work out that way. In that case, select two times that cover the entire group, and tell everyone that they can come at either time each week. Either way, each person only needs to come once a week, but you will have to run both meetings. Try to set the meeting times by the first day of classes so that you can meet during the first week. I like to have the meetings on Thursdays and Fridays for that reason, and also because I prefer meetings to be toward the end of the week.

Make sure you give everyone all the details they need about meetings: day of the week, time, location. Otherwise they will ask you! Or they just won't show up. Also keep in mind that there are usually some stragglers who join the CI late, so you might need to include them individually in CI emails until they are added to the mailing list. In any case, be sure to tell students that they need to register for the CI by the first Friday of classes.

### Week 1: Welcome, Getting Started

Use the first meeting to introduce yourself and welcome everyone to the CI. Give them an overview of what the CI is about, what the semester will look like, and what you expect of them. Mainly they just need to show up every week and submit their semester report at the end. The CI is effectively a pass/fail course and that is the only thing they need to do in order to pass.

Their homework for the first week is to get a Palmetto account and set up an Anaconda environment so that they can run the notebooks. Make sure they do this part as soon as possible! It's actually the hardest part of the course.

### Week 2: First Notebook

The second week should be the first week of going through a notebook. For this first notebook it's usually good to walk them through the structure of the notebook, show them how to do various things like running cells and editing cells, and point out the assignment at the end. The assignments are not graded, it just gives them something for their own practice. I also put little "TODOs" throughout the code cells but these TODOs are really just fun Python challenges, they are not essential to understading the notebook material.

Once you've introduced the notebook format, the rest is pretty much automatic. Use the rest of the meeting to let students work through the notebook at their own pace, ask questions or get help, or leave if they'd rather work on it later. Repeat this process for each notebook for the next four weeks.

These notebooks are very dense, however, and many students will struggle to work through all five notebooks in five weeks. Be sure to tell them that they are not expected to stay in lock-step with the course schedule, that they should go at whatever pace works for them, and that they will probably only need a subset of the information for their semester project anyway.

### Week 3: Second Notebook, Introduce Kaggle

Now that students have gone through the data science notebook, this week is a good point to show them [Kaggle](https://www.kaggle.com/) and encourage them to start looking for datasets that interest them. Kaggle is a great starting point for most students to start thinking about their semester project. They don't need to define their project yet, they just need to start exploring.

### Week 4-6: Remaining Notebooks, Develop Semester Project

I used to wait until we had gone through all five notebooks before even talking about the semester project, but I found that many students would have preferred to start developing their project earlier on so that they knew where to focus their attention while working through the notebooks. In other words, while students are going through the five notebooks they can also be looking at datasets and developing an idea of what they want to do with the dataset. So just keep reminding them to be thinking about their project during these weeks.

### Week 7-8: Determine Semester Project

By now all of the notebooks have been introduced, and students have been looking for datasets that interest them. Most students by now will have a shortlist of options and all you need to do is help them decide which one to pick. You want each student to have a project that will be challening but attainable enough for them to produce results in a semester, so those criteria may be enough to narrow down a list of 3-5 datasets down to one. All students should definitely decide on a project by week 8, but ideally students should have been thinking about their project since week 4, so most of them will probably decide on a project before week 8. Try to encourage students to figure out their project as early as possible so that they have more time to work on it.

### Week 9-14: Semester Project

Once you've gone through all five notebooks, the meetings will shift from talking about the notebooks to talking about students' projects. Each week, have each student update you on their project, and ask questions or get help as needed. Go around the table this way so that everyone hears about everyone else's project. Encourage students to ask each other questions and help each other out! Use the remaining time to work with students individually.

I've found that some students begin to feel like there's no point in coming to these later meetings because they could just give an email update, so it helps to give some added purpose to these meetings aside from simply sharing updates. I like to bring up a story from [The Batch](https://www.deeplearning.ai/thebatch/) or an interesting paper that I've read and have a group discussion about it. Do some group activity like that to get everyone engaged with each other.

### Week 15: Presentations

The last week, which is always the week before exams, have the honors and tech-elective students present their work to the group. You can have everyone else simply share a final update if time permits. And that's it! Remind everyone to email their reports to you so that you can upload them to the Box folder. Once you have everyone's reports, Dr. Smith will review them and write up her report for the CI overall. Dr. Smith likes to have the reports as early as possible when she is teaching that semester, so I usually tell students to submit by Monday night. She will ask you to give your opinion of what grade each student should receive. I almost always end up recommending an A for every student, unless a student's report was extremely lacking or they had very poor attendance.

### Returning Students

TODO

### Honors and Tech-Elective Students

TODO
