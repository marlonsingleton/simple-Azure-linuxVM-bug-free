
# Inspired by a <img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/bug.jpg"/> and my journey to a solution.

At this time of this writing, I submitted a PR to fix the issue below. However, my PR was refused. <br />
I'm leaving my work here for others to review and come to thier own conclusion.

### CASE #1
When launching the deployment directly in the Azure Portal, you're unable to provide an ssh key.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/portalbug.jpg"/>

Case #1 was reported to a project I've been working on. <br />
The attached image is me repoducing the error, validating the user's concerns.

### CASE #2
Looking behind the scenes, I found both auth types being assigned the sshKey. <br /> Desireable? I don't think it is.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/2authsAssignedsshKey.jpg"/>

### CASE #3
When setting the ARM Template default to password authentication, I got the following error.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/Failed_withPasswordAuthSet.jpg"/>

By now, I definitely felt like something needed to be addressed here.

### One simple line
```
"adminPassword": "[if(equals(parameters('authenticationType'), 'sshPublicKey'), json('null'), base64(parameters('adminPasswordOrKey')))]",
```

- This prevents the authentication types from stepping on each other (This occurred in the Portal in Case 1).
- It prevents both auth types from being assigned the sshKey (This occurred in Case 2).
- It ensures the sshKey conforms to the the password length requirements (This addressing Case 3).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarlonsingleton%2Fazure-simple-linuxVM-bug-free%2Fmaster%2FazuredeployBug.json" target="_blank">Bug Deployment</a> <br />  
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarlonsingleton%2Fazure-simple-linuxVM-bug-free%2Fmaster%2Fazuredeploy.json" target="_blank">Bug Free Deployment</a>


