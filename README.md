
# Inspired by a <img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/bug.jpg"/> and my journey to a solution.

At this time of this writing, I submitted a PR to fix the issue presented below.<br /> 
Confusingly, my PRs were rejected, even with the supporting evidence I provided below.<br />
I'm leaving my work here for others to review and come to thier own conclusions.

### The Reported Bug
When launching the ARM deployment directly in the Azure Portal, you're unable to provide an ssh key.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/portalbug.jpg"/>

Okay. I figured I'd take a look into this and possibly learn something along the way.

### 1st Finding
Looking behind the scenes, I found both auth types being assigned the sshKey. <br /> Desirable? Of course not!

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/2authsAssignedsshKey.jpg"/>

### 2nd Finding
When setting the ARM Template default to password authentication, I got the following error.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/Failed_withPasswordAuthSet.jpg"/>

By now, I'm feeling this can be fixed by using a condition to prevent the simultaneous auth assignments and converting the parameter value to a valid password when selected. 
### Note: 
View template to see sshPublicKey implementation as it requires no change.

### Here's my fix action
```
"adminPassword": "[if(equals(parameters('authenticationType'), 'sshPublicKey'), json('null'), base64(parameters('adminPasswordOrKey')))]",
```

- This prevents the authentication types from stepping on each other (This occurred in the Portal in Case 1).
- It prevents both auth types from being assigned the sshKey (This occurred in Case 2).
- It ensures the sshKey conforms to the the password length requirements (This addressing Case 3).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarlonsingleton%2Fazure-simple-linuxVM-bug-free%2Fmaster%2FazuredeployBug.json" target="_blank">Bug Deployment</a> <br />  
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarlonsingleton%2Fazure-simple-linuxVM-bug-free%2Fmaster%2Fazuredeploy.json" target="_blank">Bug Free Deployment</a>


