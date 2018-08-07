# 原始后端代码（axios前端不做任何修改）
```java
@RequestMapping(value = "/login", method = RequestMethod.POST), method = RequestMethod.GET
    public SuccessTip loginVali(String username) {
         username = super.getPara("username").trim();
         System.out.println("username" + map.get("username"));
```
# 解决问题后代码
```java
@RequestMapping(value = "/login", method = RequestMethod.POST)
@ResponseBody
    public SuccessTip loginVali(@RequestBody Map map) {
         System.out.println("username" + map.get("username"));
```
