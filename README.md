# RProjects
#This is where I am going to aggregate the R scripts I use to organize Data. This will serve as a notebook for me and others to to advance their knowledge in R. Most of these will be scripts organizing csv's pulled from Avaya's CMS reporting software for call center data relevant to my current employment.

#Updated: 1/1/20

##################################################################################################################################################################################

#CLEAR DIRECTORY
rm(list=ls())

#IMPORT LIBRARIES
library(dplyr)
library(readxl)
library(readr)
library(purrr)
library(data.table)
library(stringr)


###############################################################################################################################
#################      MERGE CSV IN WD AND ADD FILENAMES IN COLUMNS       #####################################################
#################           ##DON'T EDIT, COPY BEFORE EDITING##           #####################################################
###############################################################################################################################



#### READ ONE FILE AS DESIRED
#singlefile <- read_csv("REPORTDAILY 41-1.csv", col_names=TRUE, skip=3, n_max=0)


#### CREATE LIST OF FILES WE WILL USE
filenames <- list.files(path = 'C:/Users/Zach/Desktop/test', pattern="*.csv", full.names = FALSE,recursive=TRUE) 


#### READ AND MERGE ALL CSVs INTO ONE, ONLY USING DESIRED DATA
combinedcsv <-list.files(path = "C:/Users/Zach/Desktop/test",
           pattern="*.csv", 
           full.names = T) %>% 
  map_df(~read_csv(., skip=3, n_max=1))

#### ADD FILENAMES AS ROW
combinedcsv$filename <- basename(filenames)


write.table(combinedcsv, file="REPORTDAILY MERGED.csv", sep=",", row.names=FALSE)


combinedcsv$'Staffed Hours' <- combinedcsv$`Staffed Time`/(60*60)
combinedcsv$`Avail Time` <- combinedcsv$'Available Hours'/(60*60)


combinedcsv %>% separate(combinedcsv, combinedcsv$filename, "DateConverter", sep=".", remove=FALSE)


###############################################################################################################################
###############################################################################################################################





