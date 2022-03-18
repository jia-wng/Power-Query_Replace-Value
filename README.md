# Power-Query_Replace-Value--Stay up to date!
# There are many ways to achieve the results you want, below I List some of the cases encountered in my daily work.


# 1. Single-Column-Replace multiple values in a single step

# 1.1 Replace the value containing "-" with a blank

![grafik](https://user-images.githubusercontent.com/84840321/157683076-fdc2b052-0cff-45cf-9ab1-37b3f285ad4c.png)

= Table.ReplaceValue(#"Promoted Headers",
  "-","",
  (x,y,z)=>if Text.Contains(x,y) then z else x,
  {"Region"})
  
![grafik](https://user-images.githubusercontent.com/84840321/157685173-cc98f2df-3bdc-4e17-95b4-4f4474fe97fa.png)

# 1.2 Replace all "-" with a space

= Table.ReplaceValue(#"Reordered Columns",
  "-"," ",
  (x,y,z)=>List.Accumulate(Text.ToList(y),x,(s,v)=>Text.Replace(s,v,z)),
  {"Region"})
  
 ![grafik](https://user-images.githubusercontent.com/84840321/157685910-4291e648-146c-480e-ab32-add10a86007d.png)

# 1.3 Multi-value replacement in one columns

# Replace all Bayern with bayern, all West with west, all Nord with nord......

![grafik](https://user-images.githubusercontent.com/84840321/157832299-123d52d9-190d-4f33-876c-102e9a3a2e7a.png)

# Add Conditional Column (if... then....)

= Table.AddColumn(#"Promoted Headers", "Custom", 
  each if [Region] = "Bayern" then "bayern" 
  else if [Region] = "West" then "west" 
  else if [Region] = "Nord" then "nord" 
  else [Region])
  
![grafik](https://user-images.githubusercontent.com/84840321/157832818-05177e04-4003-4a3c-8d1f-34fd112cfe0d.png)

# 2. Multiple columns of value replacement in a single step

# 2.1 Replace multiple columns of different values with a single value
# Replace all four columns containing the value of "UPFX" with empty

![grafik](https://user-images.githubusercontent.com/84840321/157834014-f9db0e4f-e30f-49a7-b6aa-ac6205ccd052.png)

= Table.ReplaceValue(#"Renamed Columns",
  "UPFX",
  " ",
  (x,y,z)=>List.Accumulate(Text.ToList(y),x,(s,v)=>Text.Replace(s,v,z)),
  {"Col1","Col2","Col3","Col4"})
  
![grafik](https://user-images.githubusercontent.com/84840321/157834719-eca73a91-e13a-47dd-bec2-a375c5e36573.png)

# 3. Multi-Conditional replacement

# Replace the values in Sheet1 according to the condition table
<img width="102" alt="image" src="https://user-images.githubusercontent.com/84840321/158990168-10c201e0-8502-4e18-89d5-0b0615eb48a0.png">
<img width="101" alt="image" src="https://user-images.githubusercontent.com/84840321/158990224-0270f4a9-a98f-4130-9904-077fea4f0844.png">

let

    Source = Excel.CurrentWorkbook(){[Name="Table2"]}[Content],
    
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Item", type text}, {"Value", Int64.Type}}),
    
    Replace = Table.AddColumn( #"Changed Type","New",each if List.PositionOf(Condition[Item],[Item])=-1 then [Value] else Condition[Value]
    
    {List.PositionOf(Condition[Item],[Item])})
    
in

    Replace

<img width="367" alt="image" src="https://user-images.githubusercontent.com/84840321/158990405-62401f60-c109-4295-a51e-46cfbb92c6b5.png">



