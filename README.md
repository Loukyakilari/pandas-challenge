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

District summary should include the following metrics:

*Total schools.
*Total students.
*Total budget.
*Average math score.
*Average reading score.
*% passing math (the percentage of students who passed math).
*% passing reading (the percentage of students who passed reading).
*% overall passing (the percentage of students who passed math AND reading).



        #Total number of schools
        total_schools = school_data_complete["school_name"].nunique()

        # Total number of students
        total_students = school_data_complete["student_name"].count()

        # Total budget
        total_budget = school_data_df["budget"].sum()

        # Average math_score
        average_math_score =school_data_complete["math_score"].mean()

        # Average reading score
        average_reading_score = school_data_complete["reading_score"].mean()


        # Percentage of students with math score >=70
        Passing_math = school_data_complete.loc[school_data_complete["math_score"]>=70,:]["math_score"].count()
        Percentage_math=(Passing_math/total_students) * 100


        # Percentage of students with reading score >=70
        Passing_reading = school_data_complete.loc[school_data_complete["reading_score"]>=70,:]["reading_score"].count()
        Percentage_reading = (Passing_reading)/total_students * 100


        # Overall percentage
        overall_percentage = school_data_complete.loc[(school_data_complete["math_score"]>=70) & (school_data_complete["reading_score"]>=70),:]["student_name"].count()
        overall_passing = (overall_percentage/total_students) * 100


        # District Summary
        District_Summary = pd.DataFrame({"Total Schools" : [total_schools] ,
                                "Total Students" : total_students,
                                "Total Budget" : total_budget,
                                "Average Math Score" :average_math_score ,
                                "Average Reading Score" : average_reading_score,
                                "% Passing Math" : Percentage_math,
                                "% Passing Reading" : Percentage_reading,
                                "% Overall Passing" : overall_passing})

        District_Summary

        # Format cells
        District_Summary.style.format({"Total Students" : "{:,}","Total Budget" :  "${:,.2f}" ,"% Overall Passing" : "{:,.6f}" })


### **School Summary**

create a dataframe that include the following metrics: 

*School name.
*School type.
*Total students.
*Total school budget.
*Per student budget.
*Average math score.
*Average reading score.
*% passing math (the percentage of students who passed math).
*% passing reading (the percentage of students who passed reading).
*% overall passing (the percentage of students who passed math AND reading).

        # School name
        school_name = school_data_complete.set_index("school_name").groupby(["school_name"])

        # School type
        school_type = school_data_df.set_index("school_name")["type"]

        # Total students
        total_students_byschool = school_name.count()["Student ID"]

        # Total school budget
        total_school_budget = school_data_df.set_index("school_name")["budget"]

        # Per student budget
        student_budget = total_school_budget /total_students_byschool

        # Average math score
        average_math_score_byschool = school_name["math_score"].mean()

        # Average reading score
        average_reading_score_byschool = school_name["reading_score"].mean()


        # % Passing math
        Passing_math_byschool = (school_data_complete.loc[school_data_complete["math_score"]>=70,:].groupby("school_name")["math_score"].count()/total_students_byschool) * 100
                          
        # % Passing reading
        Passing_reading_byschool = (school_data_complete.loc[school_data_complete["reading_score"]>=70,:].groupby("school_name")["reading_score"].count()/total_students_byschool) * 100


        # %overall passing
        overall_passing_byschool = (school_data_complete.loc[(school_data_complete["math_score"]>=70) & (school_data_complete["reading_score"]>=70),:].groupby("school_name")["student_name"].count()/            total_students_byschool) * 100


        # School Summary
        school_summary = pd.DataFrame({"School Type" : school_type, "Total Students" : total_students_byschool,
                              "Total School Budget": total_school_budget, "Per Student Budget": student_budget,
                              "Average Math Score": average_math_score_byschool, 
                               "Average Reading Score": average_reading_score_byschool,
                              "% Passing Math": Passing_math_byschool, "% Passing Reading": Passing_reading_byschool,
                              "% Overall Passing": overall_passing_byschool})
        school_summary

        # Format cells
        school_summary.style.format({"Total Students" : "{:,}","Total School Budget" :  "${:,.2f}" ,
                             "Per Student Budget" :"${:,.2f}", "% Overall Passing" : "{:,.6f}" })


### **Highest-Performing Schools (by % Overall Passing)**

create a dataframe that include the following metrics: 

*School name.
*School type.
*Total students.
*Total school budget.
*Per student budget.
*Average math score.
*Average reading score.
*% passing math (the percentage of students who passed math).
*% passing reading (the percentage of students who passed reading).
*% overall passing (the percentage of students who passed math AND reading).

        #sort the schools based on % overall passing
        top_performing_schools = school_summary.sort_values("% Overall Passing", ascending=False)
        top_performing_schools.head().style.format({"Total Students" : "{:,}",
                                            "Total School Budget" :  "${:,.2f}" ,"Per Student Budget" :"${:,.2f}", 
                                            "% Overall Passing" : "{:,.6f}"})

### **Lowest-Performing Schools (by % Overall Passing)**  

create a dataframe that include the following metrics:

*School name.
*School type.
*Total students.
*Total school budget.
*Per student budget.
*Average math score.
*Average reading score.
*% passing math (the percentage of students who passed math).
*% passing reading (the percentage of students who passed reading).
*% overall passing (the percentage of students who passed math AND reading).

        #sort the schools based on % overall passing
        Bottom_performing_schools = school_summary.sort_values("% Overall Passing", ascending=True)
        Bottom_performing_schools.head().style.format({"Total Students" : "{:,}",
                                            "Total School Budget" :  "${:,.2f}" ,"Per Student Budget" :"${:,.2f}", 
                                            "% Overall Passing" : "{:,.6f}"})


### **Math Scores by Grade**

Create a DataFrame that lists the average math score for students of each grade level (9th, 10th, 11th, 12th) at each school.

        nineth_math = student_data_df.loc[student_data_df["grade"] == "9th" , :].groupby("school_name")["math_score"].mean()
        tenth_math = student_data_df.loc[student_data_df["grade"] == "10th" , :].groupby("school_name")["math_score"].mean()
        eleventh_math = student_data_df.loc[student_data_df["grade"] == "11th" , :].groupby("school_name")["math_score"].mean()
        twelve_math = student_data_df.loc[student_data_df["grade"] == "12th" , :].groupby("school_name")["math_score"].mean()

        Math_score_bygrade = pd.DataFrame({"9th" : nineth_math, "10th" : tenth_math,
                                  "11th" : eleventh_math, "12th" : twelve_math })
        Math_score_bygrade

### **Reading Scores by Grade**

Create a DataFrame that lists the average reading score for students of each grade level (9th, 10th, 11th, 12th) at each school.  

        nineth_reading = student_data_df.loc[student_data_df["grade"] == "9th" , :].groupby("school_name")["reading_score"].mean()
        tenth_reading = student_data_df.loc[student_data_df["grade"] == "10th" , :].groupby("school_name")["reading_score"].mean()
        eleventh_reading = student_data_df.loc[student_data_df["grade"] == "11th" , :].groupby("school_name")["reading_score"].mean()
        twelve_reading = student_data_df.loc[student_data_df["grade"] == "12th" , :].groupby("school_name")["reading_score"].mean()

        reading_score_bygrade = pd.DataFrame({"9th" : nineth_reading, "10th" : tenth_reading,
                                  "11th" : eleventh_reading, "12th" : twelve_reading })
        reading_score_bygrade


### **Scores by School Spending**

Create a table that breaks down school performance based on average spending ranges (per student). Use your judgment to create four bins with reasonable cutoff values to group school spending. Include the following metrics in the table:

*Average math score.
*Average reading score.
*% passing math (the percentage of students who passed math).
*% passing reading (the percentage of students who passed reading).
*% overall passing (the percentage of students who passed math AND reading).

        # Create Bins
        bins = [0, 585, 630, 645, 680]

        # Create labels for this bins
        group_labels = ["<$585", "$585-630", "$630-645", "$645-680"]

        # Creating a column
        school_summary["Spending Ranges(Per student)"] = pd.cut(school_summary["Per Student Budget"], bins, labels = group_labels)

        # Average math and reading scores based on spending
        spending_math_score = school_summary.groupby(["Spending Ranges(Per student)"])["Average Math Score"].mean()
        spending_reading_score = school_summary.groupby(["Spending Ranges(Per student)"])["Average Reading Score"].mean()

        # % Passing math and reading based on spending
        spending_passing_math = school_summary.groupby(["Spending Ranges(Per student)"])["% Passing Math"].mean()
        spending_passing_reading = school_summary.groupby(["Spending Ranges(Per student)"])["% Passing Reading"].mean()

        # % overall passing
        spending_overall_passing = school_summary.groupby(["Spending Ranges(Per student)"]) ["% Overall Passing"].mean()


        school_spending = pd.DataFrame({"Average Math Score": spending_math_score.round(2), 
                               "Average Reading Score": spending_reading_score.round(2),
                              "% Passing Math": spending_passing_math.round(2) , "% Passing Reading": spending_passing_reading.round(2),
                              "% Overall Passing": spending_overall_passing.round(2) })
        school_spending



### **Scores by School Size**

Create a table that breaks down school performance based on school size (small, medium, or large). 

        # Create Bins
        bins = [0, 999, 2000, 5000]

        # Create labels for this bins
        group_labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

        # Creating a column
        school_summary["School Size"] = pd.cut(school_summary["Total Students"], bins, labels = group_labels)

        # Average math and reading scores based on school size
        size_math_score = school_summary.groupby(["School Size"])["Average Math Score"].mean()
        size_reading_score = school_summary.groupby(["School Size"])["Average Reading Score"].mean()

        # % Passing math and reading based on school size
        size_passing_math = school_summary.groupby(["School Size"])["% Passing Math"].mean()
        size_passing_reading = school_summary.groupby(["School Size"])["% Passing Reading"].mean()

        # % overall passing
        size_overall_passing = school_summary.groupby(["School Size"]) ["% Overall Passing"].mean()

        #format
        school_size = pd.DataFrame({"Average Math Score": size_math_score, 
                               "Average Reading Score": size_reading_score,
                              "% Passing Math": size_passing_math , "% Passing Reading": size_passing_reading,
                              "% Overall Passing": size_overall_passing })
        school_size



### **Scores by School Type**

Create a table that breaks down school performance based on type of school (district or charter).


        # groupby type
        school_type = school_summary.groupby(["School Type"])

        # Average math and reading scores based on school size
        school_math_score = school_type["Average Math Score"].mean()
        school_reading_score = school_type["Average Reading Score"].mean()

        # % Passing math and reading based on school size
        school_passing_math = school_type["% Passing Math"].mean()
        school_passing_reading = school_type["% Passing Reading"].mean()

        # % overall passing
        school_overall_passing = school_type ["% Overall Passing"].mean()

        #format
        schooltype_size = pd.DataFrame({"Average Math Score":school_math_score, 
                               "Average Reading Score":school_reading_score,
                              "% Passing Math": school_passing_math , "% Passing Reading": school_passing_reading,
                              "% Overall Passing": school_overall_passing })
        schooltype_size
