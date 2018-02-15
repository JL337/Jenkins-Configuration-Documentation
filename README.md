# Setting up Jekins Configuration

Aim of this section is to configure Jenkins so that it is ready to mark a students lab submission. 

Given a student has submitted a pull request. When Jenkins has been correctly configured. Then the student's lab is marked.

This configuraton has been written so that any Sparta Global instructor can configure Jenkins for the marking of student labs.

## Accessing Jenkins from Local Machine

To log into your Sparta Global Jenkins account go to this link in your browser. Note that Jenkins can only be run locally on-site at Sparta Global.

[Jenkins](<http://jenkins.spartaglobal.academy:8080/>)

Then login with User and Password which will have been previously set up.


## Generate SSH private and public keys for Jenkins and GitHub

* Navigate to directory in terminal: `cd ~/.ssh`
* Generate the public and private key `ssh-keygen -t rsa -b 4096 -C "sparta-lab-key"`
* Name the file the private key will be stored in. eg. **sparta-lab-key**
* To display the private key, `cat ~/.ssh/sparta-lab-key`
* The `public key` will have a `.pub` appended, example: `sparta-lab-key.pub`
* To display the `private key`, `cat ~/.ssh/sparta-lab-key.pub`

## GitHub Settings Configuration for each Lab

* On GitHub, for the lab, go to **Settings**
* Select **Deploy keys**
* Copy the **public key** into the Deploy key input box on GitHub 
* Check **Allow write access**
* Give the Deploy key a relevant name, eg. *lab-name deploy key*
* Next go to  **Webhooks** > **Payload URL** > Add this link in: `http://jenkins.spartaglobal.academy:8080/ghprbhook/`
* Check box **Let me select individual events** > Check box **Pull request**
* Check box **Active**


## Jenkins Settings Configuraton for each Lab

* On Jenkins begin a **New Item**, select **Free Style project** and name the item. Eg. *Example Lab*
* Open configuration of the project

### General
* Check **GitHub project** and for **Project url** add the HTTPs of the GitHub lab that was created for the lab
* Check box **Restrict where this project can be run**
* Give the **Label Expression** which is the name of the Node

### Source Code Management
* Check box, **Git**
* In **Repositories** > **Repository URL** add the GitHub SSH link of the GitHub created lab
* Next add the **Credentials**:

	#### Adding Credentials to Jenkins
	* Click **Add** and add a new **Credentials** to Jenkins. Select **SSH Username with private key**, insert the private key that was generated earlier
	* In **Description** give the credential a name such as **Lab Name Key**
	* Copy the **private key** generated earlier into **Enter Directly**
	* Choose the credential key name that was just created from the drop down
	
* **Branches to build** > **Branch Specifier (blank for 'any')**: Then add this into the text box: `${sha1}`
* **Repository browser**: `(Auto)`

### Build Triggers
* Check the box for **GitHub Pull Request Builder**.
* **GitHub API credentials**: https://api.guthub.com :Anonymous connection 
* Check the box **Use github hooks for build triggering**
	* Under **Advanced**:
		* **List of organizations. Their members will be whitelisted.** > 		insert: `spartaglobal`
		* Check box > **Allow members of whitelisted organizations as admins** 

### Build Environment
* None

### Build
* None

### Post-build Actions
* None

# Testing the Pull Request to Trigger Jenkins Build

## Github
* Fork the main lab repoistory to your own GitHub repository
* Make changes to the project, add, commit and push the code to your own master branch, when satisfied with changes.
* Select **Pull requests** and create a new **New Pull request** > **Create new pull request**

## Jenkins
* As soon as a student initiates a pull requests, Jenkins should automatically trigger a build.
* Code will not merge to the original master repo, but will only test the code for errors.