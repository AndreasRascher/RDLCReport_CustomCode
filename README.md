# DynamicsNAV_RDLC_CustomCode

Inhalt
* __SetData & GetData - Die Standardlösung in NAV]__
  * [global Variables](#global-Variables)
  * [GetData](#GetData)
  * [SetData](#SetData)
  
## SetData & GetData - Die Standardlösung in NAV

Um Werte je Beleg im Kopf und Fuß eines RDLC Berichtes anzuzeigen werden die Custom Code Funktionen `Code.SetData` und `Code.GetData` in der hidden property eines Rectangles oder eine Zelle eines Tablix hinterlegt. Den Custom Code für diese Funktionalität ist in 3 Teile unterteilt

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
