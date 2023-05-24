#!/usr/bin/env python3

import pandas as pd
import numpy as np

"""
To get this script run automatically I would set up a cronjob to run every sixth hour
by adding the following to the crontab file:

0 */6 * * *

# A script to check check if a new file has been added to the directory.

If it has run:

newfile.py
"""

 
df = pd.read_csv("/Users/daniel/Desktop/technical_evaluation/samples.txt")  #import quality file. Should be automated to take a new quality file

#sanity check to see if the file has the right amount of columns
if len(df.columns) != 6:
    print("The sample file does not have the right amount of columns. Please check the file)")

sample_origin = df["sample"].str[1]     #extract second letter of sample name i.e. the sample origin
df.insert(6, "sample_origin", sample_origin)    #add column with sample origin

o_count = df["sample_origin"].value_counts()  #count number of samples with each origin
count = [o_count["C"],o_count["T"],o_count["D"],o_count["N"]]   #sort and store counts in a list

#check if there are any samples with an unknown origin and print the origins of all samples
res = np.array(sample_origin)
unique = np.unique(res)

if (len(df) != sum(count)):
    print("There is one or more samples with an unknown origin. Modify the script to include these samples.")
    print("All the origins of the samples are:", unique)


#check how many samples from each origin that fails qc (<95% covered bases i.e. qc_pass=False)
C,T,D,N=0,0,0,0

for i in range(0,len(df)):
    if df["sample_origin"][i] == "C" and df["qc_pass"][i] == False:
        C+=1
    elif df["sample_origin"][i] == "T" and df["qc_pass"][i] == False:
        T+=1
    elif df["sample_origin"][i] == "D" and df["qc_pass"][i] == False:
        D+=1
    elif df["sample_origin"][i] == "N" and df["qc_pass"][i] == False:
        N+=1
      
qc_fail = [C,T,D,N]

origin = ["C","T","D","N"]

quot = [m/n for m,n in zip(qc_fail, count)] #calculate the quote of each sample that fails qc 

#check if an origin produces more than 10% failed samples
for i in range(0,len(quot)):
    if quot[i] > 0.1:
        print("Origin", origin[i], "produced more than 10% failed samples:")
        
        """Instead of printing out which samples that produces more than 10% failed samples
        an email should be sent if this happens. This can be done by using the smtplib and MIMEText modules.
        I did not have time to implement this now but with one more hour I could have finished it with
        something along the lines of:

        import smtplib
        from email.mime.text import MIMEText

        and so on...
        """


new=pd.DataFrame({'origin':origin, 'count':count, 'qc_fail':qc_fail, 'quotient':quot})
print(new)