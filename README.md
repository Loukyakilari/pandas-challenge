# **pandas-challenge**
This contains my solution for pandas challenge (Module 4)
   * This is the analysis of school and student data using the Pandas Library and Jupyter Notebook.

## **PyCitySchools Analysis**

    * The percentage of students who passed in reading is greater than the percentage of students who passed in math.
    * The top 5 schools are charter type and the bottom 5 schools are district type of schools. We can say that 
      charter schools perform better than district schools.
    * Overall pass percentage is greater in schools with less per student budget.
    * Schools with 2000 students or less have a higher percentage of passing rate than larger schools with greater than
       2000 students.

## **Setup**

* Create a new repository called pandas-challenge.
* In the repository, create a folder called PyCitySchools and add Jupyter notebook which will be the main script to run analysis.
* Inside the folder also add the resources which contains csv files used for analysis. Make sure to have correct path to read the csv files.

## **Task list**
* import the pandas and read csv files.

      # Dependencies
      import pandas as pd

      # csv files to load
      school_data="Resources/schools_complete.csv"
      student_data="Resources/students_complete.csv"

      # Read School and Student Data File and store into Pandas DataFrames
      school_data_df = pd.read_csv(school_data)
      student_data_df = pd.read_csv(student_data)

      # Combine the data into a single dataset.  
      school_data_complete = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"])
      school_data_complete.head()

### **District Summary**

District summary should include the following:

  *Total schools
  *Total students
  *Total budget
  *Average math score
  *Average reading score
  *% passing math (the percentage of students who passed math)
  *% passing reading (the percentage of students who passed reading)
  *% overall passing (the percentage of students who passed math AND reading)
    