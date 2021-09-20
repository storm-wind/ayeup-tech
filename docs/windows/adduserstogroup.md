# Useful one liner commands in Powershell

#### Add a group of users from one AD group to another

```
Get-ADGroupMember “<Source AD Group>” | Get-ADUser | ForEach-Object {Add-ADGroupMember -Identity “<Destination AD Group>” -Members $_}
```




