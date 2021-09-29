# Install packages

sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-logging"
sfdx force:package:install --wait 10 --publishwait 10 --package "Email to Case Premium"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-common-meta-pkg"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-product-lov"

 sfdx force:source:deploy -m datacategorygroup:EBS_Toolbox
 sfdx force:source:deploy -m datacategorygroup:ERP_Cloud_Toolbox
 
sfdx force:source:push