# DynamicsNAV_RDLC_CustomCode

__Table of Contents__
* __SetData & GetData - the NAV way__
  * [Idea](#1A)
  * [global Variables](#1B)
  * [GetData](#1C)
  * [SetData](#1D)
* __Extending SetData & GetData__
  * [Idea](#2A)
  * [global Variables](#2B)
  * [GetData](#2C)
  * [SetData](#2D)
------------------------------------------------
## SetData & GetData - Idea <a id="1A"/>
The report layout is rendered in different steps. Header and footer are even rendered before the page body. So if we want to align header or footer contents with the current content in the page body we need to use custom code functions. 
* `Code.SetData` - saves a list of values as text in a global variable. The values are seperated by the character __&#177;__ . The code representation of that character is `Chr(177)`
* `Code.GetData` - returns a value from one of the 3 lists at the requested position number

## global Variables <a id="1B"/>
```vbnet
Shared Data1 as Object
Shared Data2 as Object
Shared Data3 as Object
```
## GetData <a id="1B"/>
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
## SetData <a id="1C"/>
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
## Extending SetData & GetData - Idea <a id="2A"/>
The NAV approach has some drawbacks we would like to avoid
* after adding new fields to the list the counting starts. You need to know the position of the list item to get the correct value.
* looking at `=Code.GetData(3,1)` doesn't indicate which value we want to get
* Having 2 arguments instead of 1 in the GetData function only adds to the complexity
* the list of values is maintained in RDLC instead of C/AL or AL which takes a lot of time and is hard to compare between versions

__Target #1__ Providing the possibility of named indexes to avoid counting and provide better readability
- Approach: Using the Microsoft.VisualBasic.Collection() Object as new global variable. The Class is already available without the need for enabling of external assemblies
__Target #2__ GetData should only need 1 argument
- Approach: Seperating the get function inte one for footer data and one for header data
- Approach: Providing one global to pass values to the header and another global to pass values to the footer
__Target #3__ The value list is maintained in C/Al or AL Code in a procedure
- Approach: Create a procedure in where we can easily or modify our Field-List

We want to address our header or footer values by name. This approach solves the following problems:
* we avoiding the need to count positions
* adding new fields is easier
* if we create only one global variable for the header data and one for the footer data then GetData only needs one argument
