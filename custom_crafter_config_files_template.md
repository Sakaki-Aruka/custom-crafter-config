# Matter

## Need
```yaml
name : String
amount: int
mass : bool
candidate: [String,String]
```

## Option
```yaml
enchant:
  - String,int,String

potion:
  - String,int,int,String

bottleTypeMatch: bool
```

# Matter template
```yaml
name: String
amount: int
mass: bool
candidate: [String,String]

enchant:
  - String,int,String

potion:
  - String,int,int,String

bottleTypeMatch: bool
```
---

# Result
## Need
```yaml
name: String
amount: int
matchPoint: int
nameOrRegex: String
```

## Option
```yaml
enchant:
  - String,int

metadata:
  - String,VALUE
```

# Result template
```yaml
name: String
amount: int
matchPoint: int
nameOrRegex: String

enchant:
  - String,int

metadata:
  - String,VALUE
```

---
# Recipe
## Need
```yaml
name: String
tag: String
result: String

coordinate: [String,String]
or
coordinate:
  - String,String,String
  - String,String,String
  - String,String,String
```

## Option
```yaml
override:
  - String -> String
  - String -> String

returns:
  - String,String,int

permission: String

```

# Recipe template
```yaml
name: String
tag: String
result: String
coordinate: [String,String]

override:
  - String -> String

returns:
  - String,String,int

permission: String
```

---
```yaml
name: String
tag: String
result: String
coordinate:
  - String,String,String
  - String,String,String
  - String,String,String

override:
  - String -> String

returns:
  - String,String,int

permission: String
```

---
# RecipePermission
## Related
```yaml
String:
  - UUID
```

## Players
```yaml
UUID:
  - name:String|parent:String
  - name:String|parent:String
```