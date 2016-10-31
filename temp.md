
## 60.2 Collections
**List**
```
//示例
def list = [5, 6, 7, 8];

```  

        Path path = Paths.get("logFile");
        Files.lines(path, StandardCharsets.UTF_8).forEach((line) -> {
            System.out.println(line);
        });
