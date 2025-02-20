# ZillowETLProject

![ETL Diag](https://github.com/user-attachments/assets/7fc083bf-3637-4242-834a-21c406c2436a)

This is an ETL project using AWS and automated using Apache Airflow.
This project integrates Zillow's data through RapidAPI to streamline the ETL (Extract, Transform, Load) process. Zillow provides us with real estate data which we will further transform and analyze.


First of all, we initialize our EC2 instance so that can run apache airflow on it. This will help us with our automation and orchestrating the workflows. It ensures each step in the pipeline runs smoothly and handles dependencies, retries, and scheduling. We will create a DAG(Directed acyclic graphs) to make the process efficient  

![image](https://github.com/user-attachments/assets/f20e451d-2b1e-4077-9420-c5d8731335ef)



![image](https://github.com/user-attachments/assets/94e6b18b-f9ef-46d7-8274-7795f899cf41)



Data is fetched from Zillow's API using the script analytics.py. This script is responsible for collecting the required information and preparing it for further processing. The data is collected in a csv format.
   
![image](https://github.com/user-attachments/assets/9dbab9f4-30aa-492e-aa4b-59072a277717)



We create a landing S3 bucket for the extracted data. The "landing" Amazon S3 bucket, serves as the first checkpoint for raw data
and 
create another S3 'intermediate' bucket for our data. This will be the midpoint for our data. We can add security checks on our data here.


 ![image](https://github.com/user-attachments/assets/0bdc787d-13b8-489e-854d-cad2173d695d)


  
A Lambda Function Trigger is created for the intermediate bucket.
An AWS Lambda function is triggered whenever new data lands in the S3 bucket. This function processes the raw data and transfers it to an intermediate bucket. The intermediate bucket holds pre-processed data, ready for further transformations or validations.

![image](https://github.com/user-attachments/assets/0526f91a-20b5-4e4e-bb9d-0e8fc838f704)



The final s3 bucket is created which holds the cleaned, and formatted data, which is now ready for loading into the target database.

![image](https://github.com/user-attachments/assets/6ebb7a7e-cc29-4928-8746-2ff202e5a56d)


We create a lambda trigger which takes the data from the intermediate bucket, performs any necessary transformations, and loads the cleaned data into the "transformed data" S3 bucket. We use the pandas library to clean and transform data

![image](https://github.com/user-attachments/assets/440292b5-2f25-4218-808f-e978535b780b)


The transformed data from the final cleaned bucket is then loaded into Amazon Redshift, which acts as the data warehouse. Redshift ensures efficient querying and analysis of the processed data. We create a redshift cluster of the node type 'dc2.large' having number of nodes as 1.



![image](https://github.com/user-attachments/assets/4d47dd85-a8d2-4db5-8076-4b5e24eed735)


We write an sql query in redshift to get the Data.

![image](https://github.com/user-attachments/assets/708354fb-9e67-4ea9-a3db-7b6f4e3d4b2a)


Finally, Amazon QuickSight is used to create dashboards and visualizations, enabling stakeholders to gain insights from the data.
![image](https://github.com/user-attachments/assets/2b3a3db9-9dd3-4864-97fd-5c552e54951e)

![Uploading image.png…]()

![Screenshot 2024-11-16 214536](https://github.com/user-attachments/assets/b104f6dc-cd23-4c33-b997-0639ff29ddf7)

![image](https://github.com/user-attachments/assets/70887be3-1122-47f1-80a9-e7c796751648)











