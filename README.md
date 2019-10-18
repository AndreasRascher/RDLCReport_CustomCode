# DynamicsNAV_RDLC_CustomCode

Table of Contents
* __SetData & GetData - the NAV way__
  * [global Variables](#global-Variables)
  * [GetData](#GetData)
  * [SetData](#SetData)
  
## SetData & GetData - Idea
The report layout is rendered in different steps. Header and footer are rendered before the page body. If we want to have header or footer contents based on the current content in the page body we need to use custom code functions. 
* `Code.SetData` - saves a list of values as text in a global variable. The values are seperated by the character __&#177;__ . The code representation of that character is `Chr(177)`
* `Code.GetData` - returns a value from one of the 3 lists at the requested position number

## global Variables
```vbnet
Shared Data1 as Object
Shared Data2 as Object
Shared Data3 as Object
```
## GetData
```vbnet
Public Function GetData(Num as Integer, Group as integer) as Object
  ' Num    - position of the string you want to have 
  ' Group  - select which of the 3 globals you want to use as source 
  ' Object - return value  

  if Group = 1 then
  Return Cstr(Choose(Num, Split(Cstr(Data1),Chr(177))))
  End If

  if Group = 2 then
  Return Cstr(Choose(Num, Split(Cstr(Data2),Chr(177))))
  End If

  if Group = 3 then
  Return Cstr(Choose(Num, Split(Cstr(Data3),Chr(177))))
  End If
End Function
```
## SetData
```vbnet     
Public Function SetData(NewData as Object,Group as integer)
  ' NewData - String with Char177 as seperator char 
  ' Group   - select which of the 3 globals you want to use as source 
  ' Object  - return value   
  If Group = 1 and NewData <> "" Then
      Data1 = NewData
  End If

  If Group = 2 and NewData <> "" Then
      Data2 = NewData
  End If

  If Group = 3 and NewData <> "" Then
      Data3 = NewData
  End If
  Return True
End Function
```
