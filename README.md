sfdx force:org:create -s -f config/project-scratch-def.json -a bkp-setup --durationdays 30

# To make it create in the next API version, do it like this:

sfdx force:org:create -s -f config/project-scratch-def.json -a doesitwork release=Preview

# To make it create in a specific API version, do it like this:

sfdx config:set apiVersion=51.0
sfdx config:list
sfdx force:org:create -s -f config/project-scratch-def.json -a oldversion

# Install packages

sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-logging"
sfdx force:package:install --wait 10 --publishwait 10 --package "Email to Case Premium"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-common-meta-pkg"
sfdx force:package:install --wait 10 --publishwait 10 --package "m4a-product-lov" --noprompt

sfdx force:source:deploy -m datacategorygroup:EBS_Toolbox
sfdx force:source:deploy -m datacategorygroup:ERP_Cloud_Toolbox
sfdx force:source:deploy -m datacategorygroup:Article_Types

# Case origin & status is weird, does not get installed correctly by the metadata package. Go to the metdata package, switch to this scratch org, and do this:

sfdx force:source:deploy -m StandardValueSet:CaseOrigin
sfdx force:source:deploy -m StandardValueSet:CaseStatus

# Need to push with Ignore Warnings so that we don't get "Occasionally, when deployed to a destination org, ID values can become invalid..." errors

# preventing the push

sfdx force:source:push -g

# Now go to the Product LOV Project and load the Product data

# We can then do a pull from the new Org with the Force option (not quite sure why this is required)

sfdx force:source:pull -f
