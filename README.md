# Power-Query_Replace-Value--Stay up to date!
# There are many ways to achieve the results you want, below I List some of the cases encountered in my daily work.
#

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



