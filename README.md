sfdx force:org:create -s -f config/project-scratch-def.json -a bkp-setup --durationdays 30

# Install packages

sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-logging"
sfdx force:package:install --wait 10 --publishwait 10 --package "Email to Case Premium"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-common-meta-pkg"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-product-lov"

 sfdx force:source:deploy -m datacategorygroup:EBS_Toolbox
 sfdx force:source:deploy -m datacategorygroup:ERP_Cloud_Toolbox
 
 # Need to push with Ignore Warnings so that we don't get "Occasionally, when deployed to a destination org, ID values can become invalid..." errors
 # preventing the push
sfdx force:source:push -g

# Now go to the Product LOV Project and load the Product data

# We can then do a pull from the new Org with the Force option (not quite sure why this is required)
sfdx force:source:pull -f