Biostat-578-master
==================
Biostat-578-master
==================
#BIOST 578 Homework 1

#Question 1: Use the GEOmetabd package to find all HCV gene expression data using the Illumina platform submitted by an investigator at Yale.This should be done with a single query, showing the title, the GSE accession number, the GPL accession number and the manufacturer and the description of the platform used.

source("http://bioconductor.org/biocLite.R")
# Install all core packages and update all installed packages
biocLite()
# from the GEOmetadb package we can see what data we want
library(GEOmetadb)
# To download the entire database
getSQLiteFile()
# connect to the dataset
geo_con<-dbConnect(SQLite(),'GEOmetadb.sqlite') 

dbListTables(geo_con)  # To see all the tables in the geo_con databases
dbListFields(geo_con, 'gsm')  # TO see the variables in gsm table
dbListFields(geo_con, 'gse')  # To see the variables in gse table
dbListFields(geo_con, 'gpl')  # To see the variables in gpl table


# For specific variable, you could also find as below.
res<-dbGetQuery(geo_con,"SELECT gse.title FROM gse")
head(res)

# To find all HCV gene expression data using the Illumina platform submitted. 
res <- dbGetQuery(geo_con, "SELECT gse.title, gse.gse, gpl.ID, gpl.manufacturer,gpl.description FROM (gse JOIN gse_gpl ON gse.gse=gse_gpl.gse)j JOIN gpl ON j.gpl=gpl.gpl WHERE gse.title LIKE '%HCV%' AND gpl.manufacturer='Illumina'")
res

# I tried different searches from gpl or gse for the HCV gene using Illumina at Yale, but no information returned. Thus I decided to limit my search on HCV and Illumina, and then track the organization by searching the GSExxxxx. Finally I found 2 results, but none of them was from Yale.  

# Question 2: 
library(data.table)
tblmanu <- data.table(dbGetQuery(geo_con, "SELECT gse.title, gse.gse, gpl.ID, gpl.manufacturer,gpl.description FROM (gse JOIN gse_gpl ON gse.gse=gse_gpl.gse)j JOIN gpl ON j.gpl=gpl.gpl WHERE gse.title LIKE '%HCV%' AND gpl.manufacturer='Illumina'"))
tblmanu
