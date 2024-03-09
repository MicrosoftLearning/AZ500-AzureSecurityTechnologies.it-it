Contiene il file bicep per distribuire le risorse configurate nel lab

Per distribuire questo modello: 

```
$date = Get-Date -Format "MM-dd-yyyy"
$rand = Get-Random -Maximum 1000
$deploymentName = "AzInsiderDeployment-"+"$date"+"-"+"$rand"
New-AzResourceGroupDeployment -Name $deploymentName -ResourceGroupName AZ500LAB131415 -TemplateFile .\main.bicep -TemplateParameterFile .\azuredeploy.parameters.json -c
```
