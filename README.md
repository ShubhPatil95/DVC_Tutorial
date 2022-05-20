# DVC_Tutorial

<details><summary> <h2> 01 - DVC_Installation </h2> </summary>
<h2> <a href="https://github.com/ShubhPatil95/DVC-01-Installation"> 01 -DVC Installation</a></h2> 
<p>
<h3>Before installing DVC, it is recommended to create a new virtual environment.</h3> 

### Step 1: Create a new conda environment
```bash
conda create -n dvc_env python=3.8
```
### Step 2: Activate the created conda environment
```bash
conda activate dvc_env
```
### Step 3: Install DVC
```bash
pip install dvc
```
### Step 4: Check installed version of DVC 
```bash
dvc --version
```
* For details of installation as per your operating system please do refer below link.
[DVC Installation Guide](https://dvc.org/doc/install)  

</p>
</details>


<details><summary> <h2> 02 - Data_Versioning  </h2> </summary>
<h2> <a href="https://github.com/ShubhPatil95/DVC-02-Data_Versioning"> DVC-02-Data_Versioning </a></h2>

<p>
<strong> Here in this tutorial we will see how we can keep track of data file which changes over time and how can we restore the data file of particuler commits</strong>
  
### Step1
* Create a main folder named DVC-02-Data_Versioning and 2 subfolder under it named DataFileVersioning and DVCRemote. Then move into 02-DVC_Data_Versioning
```ruby
mkdir DVC-02-Data_Versioning
cd DVC-02-Data_Versioning
mkdir DataFileVersioning DVCRemote
cd DataFileVersioning
```
### Step2  
* Now you are into DataFileVersioning folder, hence intiate the git and dvc 
```ruby
git init
dvc init
```
### Step3  
* Create dummy data file under name data.txt and write sentence "This is first commit"
```ruby
nano data.txt
```  
### Step4  
* Add dvc remote, here we will use remote folder as DVCRemote however you can use either local or cloud storage like google drive, s3bucket etc. Make sure you putting your absolute path of DVCRemote folder instead of mine /usr/home/DVCRemote. After running below command you can go to DVCRemote folder and see what exactly udpated over there.
```ruby
dvc remote add -d My_Local_Remote /home/shubham/tutorial/DVC-02-Data_Versioning/DVCRemote 
```   
### Step5
* Add data.txt file into dvc, this will create data.txt.dvc file which contains the information about data.txt
```ruby
dvc add data.txt
git add .
git commit -m "First Commit"
dvc push
```  
### Step6
* Now edit the data.txt file and write the sentence "This is second commit". Further add edited data.txt to DVC and commit/push the changes to git and dvc.
```ruby
nano data.txt
dvc add data.txt
git add .
git commit -m "Second Commit"
dvc push
```  
### Step7
* Again edit the data.txt file and write the sentence "This is third commit". Further add edited data.txt to DVC and commit/push the changes to git and dvc.
```ruby
nano data.txt
dvc add data.txt  ## This file will not track by git anymore and has been added to .gitignore and data.txt.dvc will be pushed to git further
git add .
git commit -m "Third Commit"
dvc push  ## data.txt is pushed to remote storage
```  
### Step8
* Finally create a github repository named DVC-02-Data_Versioning and push the commits into it.
```ruby
git remote add origin https://github.com/ShubhPatil95/DVC-02-Data_Versioning.git
git branch -M main
git push -u origin main
```  
### Step9
* Now what if you want to go to first commit where your data.txt containing sentence "This is first commit". Just find the commit id(SHA ID) of first commit using git log --oneline and checkout to that one
```ruby
git log --oneline  ## copy the commit id of first commit, mine is f7d5a7e
git checkout f7d5a7e  ## make sure to paste your commit Id of first commit
cat data.txt  ## You can see even thoough you checkout to first commit,but data.txt is file is not yet udpated
dvc checkout  ## this command will update the data.txt file
cat data.txt  ## you can see data.txt is updated with "This is firstcommit"
```     
### Step10
* Now, Just go back to third commit.
```ruby
git checkout main
dvc checkout
cat data.txt  ## see test.txt is udpated with "This is third commit"
```    
### Step11
* Now what if you want to again go back to second commit where your data.txt containing sentence "This is second commit". Just find the commit id(SHA ID) of second commit using git log --oneline and checkout to that one
```ruby
git log --oneline  ## copy the commit id of second commit, mine is befab47
git checkout befab47  ## make sure to paste your commit Id of second commit
cat data.txt  ## You can see even thoough you checkout to second commit,but data.txt is file is still showing third commit file
dvc checkout  ## this command will update the data.txt file
cat data.txt  ## you can see data.txt is updated with "This is second commit"
```         
</p>
</details> 


<details><summary> <h2> 03 - Data_Versioning_Git_Clone </h2> </summary>

<h2> <a href="https://github.com/ShubhPatil95/DVC-03-Data_Versioning_Git_Clone"> DVC-03-Data_Versioning_Git_Clone</a></h2>

<p>
<strong> Here in this tutorial we will see how we can a take code and data.txt.dvc file from git and data.txt file from our remote storage</strong>
  
### Step1
* Create a folder under name DVC-03-Data_Versioning_Git_Clone
```ruby
mkdir DVC-03-Data_Versioning_Git_Clone
cd DVC-03-Data_Versioning_Git_Clone
```
### Step2  
* Now you are into DVC-03-Data_Versioning_Git_Clone folder. Lets clone the repository we used in second tutorial DVC-02-Data_Versioning
```ruby
git clone https://github.com/ShubhPatil95/DVC-02-Data_Versioning.git
```
### Step3  
* Go inside of folder DVC-02-Data_Versioning
```ruby
cd DVC-02-Data_Versioning
```  
### Step4  
* Now you can see there is only one file name data.txt.dvc and our datam file data.txt is not present
* Just run a command git pull and you will see data.txt file is present with sentence "This is third commit"
```ruby
dvc pull
cat data.txt
```   
### Step5
* Now let say you want to see how was your data at your first commit? Do follow below steps to check it
```ruby
git log --oneline  ## copy SHA ID of first commit, mine is 0b0d5ef
git checkout 0b0d5ef  ## checkout to first commit
cat data.txt  ## although you checkout to first commit but still your data.txt is showing data of third commit
dvc pull   ## This command will pull your data of first commit from remote storage
cat data.txt  ## you can see now data is updated with first commit
```  
### Step6
* You can again back to last commit, by checkout to branch name 
```ruby
git checkout main
cat data.txt   ## but data is still showing of first commit, beacause you need to do dvc checkout or dvc pull 
dvc pull or dvc checkout
cat data.txt  ## see data is udpated with third commit
```  
</p>
</details> 








<details><summary> <h2> 04 - Simple_DVC_Project </h2> </summary>
<h2> <a href="https://github.com/ShubhPatil95/DVC-04-Simple_Project"> 04-Simple_DVC_Project </a></h2>

<p>
<strong> Here in this tutorial we will see how we can use DVC to track the respective code and data </strong>
<h4>Challanges without DVC and how DVC is solving them</h4>
1. How DVC is helping to track the large dataset which cannot be pushed to git ? => DVC will push that data to remote data storage(S3 bucket,Google Drive)<br>
2. How can we remember that which dataset was used for particuler experiment? => Git will track the code for each experiment and DVC will track dataset used for each respective experiment.<br>
 
<h3> Assume we are trying to predict the marks of student by analyzing how much hours that student is studying. Lets take below 3 scenarios for 3 different schools </h3>
School-1: There are 10 student and there data is as below. <br>
School-2: There are 10 student same as school-1, however students who are studying 1 and 2 hours are Absent in exam. <br>
School-3: There are 10 student same as school-1, however students who are studying 1 and 2 hours are getting 50 marks each. <br>
<img src="https://github.com/ShubhPatil95/Raw_Data_Storage/blob/main/DVC-04-Simple_Project_Schools.png" alt="Data Of 3 School">

<strong> Let try to build a model for above all 3 datas </strong><br>
<strong> First of all create new conda environment and install DVC by refering [DVC-01-Installation](https://github.com/ShubhPatil95/DVC-01-Installation)<strong><br>


### Step1
* Create a folders under name DVC-04-Simple_Project DVC_Remote. Further create a code.py and data.csv files inside of DVC-04-Simple_Project
```ruby
mkdir DVC-04-Simple_Project DVC_Remote DVC_Clone
cd DVC-04-Simple_Project
touch code.py data.csv
```
### Step2
* Initiate the Git and DVC
```ruby
git init
dvc init
```  
### Step3
* Create a github repository under name DVC-04-Simple_Project. Add remote storage for DVC and Git.
```ruby
dvc remote add -d My_Local_Remote /home/shubham/DVC_Remote
git remote add origin https://github.com/ShubhPatil95/DVC-04-Simple_Project.git
```

### Step4  
* Lets work on School-1 data, copy and paste containt of code.py and data.csv from here [code.py](https://github.com/ShubhPatil95/DVC-04-Simple_Project/blob/220e6405c49d091e68635604f60330864c1b5f62/code.py) and [data.csv](https://raw.githubusercontent.com/ShubhPatil95/Raw_Data_Storage/main/data_school-1.csv)

### Step5
* Lets run a code.py and check the r_square.
* In case pandas and scikit learn not installed then you can pip install them
```ruby
python3 code.py  ## r_square = 1
```   
### Step6
* Add data.csv file to DVC and rest files to git
```ruby
dvc add data.csv
git add .
git commit -m "School-1_First_Commit_r_sqaure=1"
```  
 
### Step7
* Lets push the data.csv to remote storage My_Local_Remote and code.py to github repository.
* So now you have data.csv in DVC remote and code.py in git repository for School-1
```ruby
dvc push
git branch -M main
git push -u origin main
```
 
### Step8
* Now lets work on School-2 dataset and repeate the steps 3, 4, 5 and 7<br>
* Step3: Replace content of code.py and data.csv with containt from [code.py](https://github.com/ShubhPatil95/DVC-04-Simple_Project/blob/4bcdc457b1e84cf6cb0e630b54a0efecd9b79844/code.py) and [data.csv](https://raw.githubusercontent.com/ShubhPatil95/Raw_Data_Storage/main/data_school-2.csv) <br>
* Step4: Run a code.py
  ```ruby
  python3 code.py  ## r_square = 0.20
  ```
* Step5: Add data to dvc and code to git<br>
     ```ruby
     dvc add data.csv
     git add . 
     git commit -m "School-2_Second_Commit_r_sqaure=0.20"
     ```
* Step6: Push data.csv to DVC and code.py to git <br>
     ```ruby
     dvc push 
     git push -u origin main 
     ```
### Step9
* Now lets work on School-3 dataset and repeate the steps 3, 4, 5 and 7 <br>
* Step3: Replace content of code.py and data.csv with containt from [code.py](https://github.com/ShubhPatil95/DVC-04-Simple_Project/blob/3045210cf52cb3cc7a3925327b15a2204fb41b49/code.py) and [data.csv](https://raw.githubusercontent.com/ShubhPatil95/Raw_Data_Storage/main/data_school-3.csv) <br>
* Step4: Run a code.py
  ```ruby
  python3 code.py  ## r_square = 0.99
  ```
* Step5: Add data to dvc and code to git <br>
    ```ruby
     dvc add data.csv
     git add . 
     git commit -m "School-3_Third_Commit_r_square=0.99"
    ```
* Step6: Push data.csv to DVC and code.py to git <br>
    ```ruby
    dvc push
    git push -u origin main
    ```

### Step10
* Your all experiments are done here. Now, Lets try to see how DVC is works. 
* Go back to folder DVC_Clone and clone the github repository 
```ruby
cd ../DVC_Clone
git clone https://github.com/ShubhPatil95/DVC-04-Simple_DVC_Project
cd DVC-04-Simple_DVC_Project
```  
### Step11
* You will see that there is no data.csv file, hence do DVC pull, which will pull data.csv of School-3
* Now run code.py and see if r_square is correct(should be 0.99)
```ruby
dvc pull
python3 code.py  ## r_square should be 0.99
```     
### Step12
* Now, Just go back to First commit, means School-1 experiment.
* Copy the commit id of School-1_First_Commit
```ruby
git log --oneline      ## copy the commit School-1_First_Commit, should be 220e640
git checkout 220e640   ## checkout to School-1_First_Commit, now you have code.py for School-1 experiment
dvc pull               ## This command will pull data.csv of School-1 from DVC remote location
```    
### Step13
* Run a code.py and see if value of r_square is same a we got it for School-1 Experiment
* Got to back to last commit, git checkout main
```ruby
python3 code.py    ## r_square should be 1
git checkout main  ## Your code.py is updated for School-3_Third_Commit
dvc pull        ## Your data.csv is also updated for School-3
```        
### Step14
* Now, Just go back to Second commit, means School-2 experiment.
* Copy the commit id of School-2_Second_Commit
```ruby
git log --oneline         ## copy the commit School-2_Second_Commit, mine is 4bcdc45
git checkout 4bcdc45     ## checkout to School-2_Second_Commit, now you have code.py for School-2 experiment
dvc pull                 ## This command will pull data.csv of School-2 from DVC remote location
```  
### Step15
* Run a code.py and see if value of r_square is same a we got it for School-2 Experiment
* Got to back to last commit, git checkout main
```ruby
python3 code.py    ## r_square should be 0.20
git checkout main  ## Your code.py is updated for School-2_Second_Commit
dvc pull        ## Your data.csv is also updated for School-2
```  
<strong> This is how we can track the data.csv with code.py for every experiment using DVC and git </strong>
</p>
</details> 
