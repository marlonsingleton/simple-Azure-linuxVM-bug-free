
# Inspired by a <img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/bug.jpg"/> and my journey to a solution.


### CASE #1
When launching the deployment directly in the Azure Portal, you're unable to provide an ssh key.

<img src="https://github.com/marlonsingleton/simple-Azure-linuxVM-bug-free/blob/master/portalbug.jpg"/>

Case #1 was reported to a project I contribute to. The attached image is me repoducing the error, validating the user's concerns. My PR was closed and deemed an Azure Portal issue. 

### CASE #2
When template is modified to use "password" authentication and deployed from Azure CLI, the deployment fails.

<img src=""/>

# simple-Azure-linuxVM-bug-free

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarlonsingleton%2Fazure-simple-linuxVM-bug-free%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://github.com/marlonsingleton/azure-simple-linuxVM-bug-free/blob/master/DeployButton.jpg"/>
</a>

...More information coming. Feel free to deploy as is but I suggest using unqiue values for increased security.
