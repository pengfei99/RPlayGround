# Install R on debian

## 1. Prepare the OS

```shell
# 1. update the apt index and update all the package to latest version
sudo apt update && sudo apt upgrade

# 2. Install Initial Required Packages
# Some dependencies are required for a successful installation. Install them using the following command:
sudo apt install dirmngr apt-transport-https ca-certificates software-properties-common -y

```



## 2. Import CRAN Repository

R is present in Debian’s repositories by default, but the version may be outdated. We recommend installing 
R from the `Comprehensive R Archive Network (CRAN)` repository to get the most up-to-date version.

```shell
# First, fetch and import the GPG key for Debian using the keyserver and store it in /usr/share/keyrings/cran.gpg, 
# you can use the following commands:

# fetch the key from the keyserver and import it into your local keyring
gpg --keyserver keyserver.ubuntu.com --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'

# export the key and store it in the /usr/share/keyrings/cran.gpg file, which will be used by apt to verify package authenticity.
gpg --armor --export '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7' | gpg --dearmor | sudo tee /usr/share/keyrings/cran.gpg > /dev/null


# output example

gpg: key DC78B2DDEABC47B7: public key "Johannes Ranke <johannes.ranke@jrwb.de>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```

> If you are behind a firewall blocking port 11371, you can specify a proxy server by adding --keyserver-options 
   http-proxy=<PROXY> to the first command. 

Now we need to add the appropriate CRAN repository to the apt source list. 
Choose the command that corresponds to your Debian version:

### Bookworm(debian 12):

```shell
echo "deb [signed-by=/usr/share/keyrings/cran.gpg] https://cloud.r-project.org/bin/linux/debian bookworm-cran40/" | sudo tee /etc/apt/sources.list.d/cran.list

```


### Bullseye (debian 11):

```shell
echo "deb [signed-by=/usr/share/keyrings/cran.gpg] https://cloud.r-project.org/bin/linux/debian bullseye-cran40/" | sudo tee /etc/apt/sources.list.d/cran.list
```

### Buster (debian 10):

```shell
echo "deb [signed-by=/usr/share/keyrings/cran.gpg] https://cloud.r-project.org/bin/linux/debian buster-cran40/" | sudo tee /etc/apt/sources.list.d/cran.list
```

### Ubuntu

For ubuntu, the apt-key and apt repo name is different. You need to follow the below instructions

```shell
# add the apt-key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9

# add the cran repo to apt source list
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```
 
In our case, we choose bullseye

## 3. Install R package

Remove and clean old version of R package

```shell
# purge the package and all its dependencies
sudo apt-get --purge autoremove r-base r-base-dev

# check and clean the apt index
sudo apt-get update
sudo apt-get check
sudo apt-get -f install
sudo apt-get autoclean
```

Now install the latest version

```shell
sudo apt install r-base r-base-dev
```

## 4. Extra system package for R 

Here are some additional packages that you may want to install:

```shell
# r-recommended:
# This package includes a set of recommended R packages commonly used in data analysis and statistical modeling. The installation command for this package is:
sudo apt install r-recommended

# libssl-dev:
# This package is required if you plan to install packages from CRAN that require SSL (Secure Sockets Layer) encryption, such as the “httr” package. The installation command for this package is:
sudo apt install libssl-dev

# libxml2-dev:
# This package is required if you plan to install packages from CRAN that require XML parsing, such as the “XML” package. The installation command for this package is:
sudo apt install libxml2-dev

# libcurl4-openssl-dev:
# This package is required if you plan to install packages from CRAN that require CURL (Client URL) support, such as the “curl” package. The installation command for this package is:
sudo apt install libcurl4-openssl-dev

```

## 5. Install a R package via cran

To start the R interpreter, you can type R
```shell
# Open a R interpreter with a user account, a personal library will be setup. All package installed here will only be
# visible by this user. 
R

# With sudo, a system library will be setup. The installed package is available for all users
sudo -i R

# Install a package
install.packages('txtplot')

# load the package
library('txtplot')

# use the package
txtplot(cars[,1], cars[,2], xlab = 'speed', ylab = 'distance')
```

## 6. Manage an R package lifecycle

```shell
# install multiple package at once
install.packages(c("ggplot2", "dplyr"))

# update an R package
update.packages('txtplot')

# update all installed packages
update.packages(ask = FALSE)

# remove an R package
remove.packages("ggplot2")
```

