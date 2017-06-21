# Introduction to Data Science using R

## Access to computing resources:

This workshop will can be accomplished using either computing resources from Clemson University's Palmetto Supercomputer or 
personal computers. 

**If you are using Palmetto for this workshop, please make sure that:**

- You have access to Palmetto. Access to Palmetto can be requested via the 
following online form: https://citi.sites.clemson.edu/new-account/
- You can access Palmetto's JupyterHub interface: https://palmetto.clemson.edu/jupyterhub
- You can make the following resource allocation request from Jupyter: 1 chunk, 8 cores, 8gb ram, 24 hours walltime, workq

**If you are using personal computers, please make sure that:**

- You have R 3.3+ installed. The latest version of R can be found at https://www.r-project.org/
- You have the latest version of RStudio Desktop installed. This can be found at https://www.rstudio.com/products/rstudio/download2/

If you already have RStudio Desktop, please make sure that its version is 1.0 or higher

## Workshop materials:

All notebooks and data used in the workshop are available online: 

- For Palmetto: https://github.com/clemsonciti/data-science-r-01/raw/master/intro-to-r.zip
- For personal computers: https://github.com/clemsonciti/data-science-r-01/raw/master/intro-to-r-rstudio.zip

For personal computers:

You can download the designated compressed file, unzip it using your computers' unzip capability, and you will be able to view the files from inside RStudio. 

For Palmetto:

- You can download this compressed file from the above URL to your computer, and then upload the file to Palmetto using the **`Upload`** button located at the upper-right corner of the main JupyterHub home directory page. The file should be uploaded to your home directory. 

- You can also download this file via a terminal. To gain access to Palmett's terminal in JupyterHub, click the **`New`** button located next to the **`Upload`** button and select **`Terminal`**. Inside the terminal, type the following and hit Enter:

```
  wget https://github.com/clemsonciti/data-science-r-01/raw/master/intro-to-r.zip
```

- To decompress this file, open a terminal to Palmetto (see instructions above). Inside the terminal, type the following and hit Enter:

```
  tar xzf intro-to-r.tgz
```

- A new directory called **intro-to-r** should be created in your home directory. This directory contains all data and notebooks necessary for this workshop. 
