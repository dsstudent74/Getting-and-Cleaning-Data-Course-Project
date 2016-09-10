#Getting and Cleaning Data Final Project Codebook

Raw Data

The original raw data was downloaded from The UCI Machine Learning Repository (the URL is contained in the R script) and contained several files (also contained in the R script).

Working Data Set

The test and train datasets were merged. The activity description was added to the dataset for clarity. A subset containing the Subject, Activity, Mean, and Standard Deviation was created, which became the Tidy Data file.

Tidy Data

The Tidy Data set renamed the fields for additional clarity.

Time was capitalized and followed with an underscore.

Frequency was capitalized and followed with an underscore.

Mean was capitalized and followed with an underscore, as well as given a leading underscore.

Standard_Deviation replaced "STD", and was followed with an underscore, as well as given a leading underscore.

Github repository

For convenience and reference, this repository includes the full R script, including comments, labeled as "Getting and Cleaning Data Project R Script.txt".

This repository also includes the final data set, labeled as "Tidy_Data.txt".