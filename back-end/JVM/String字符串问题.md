相等


```
String a = "ab";
String b = "a" + "b";

System.out.println( a==b);//true


String c = "a";
String d = c + "b";

System.out.println( a==d);//false
```
java在加载 a的时候回去常量池中查询“ab”是否存在，不存在则创建一个，在加载b的时候，虽然b是由“a”和"b"两个组合的，但是是两个确定的常量，所以会将他们组合起来后取常量池中查找是否存在，因为之前已经加载过，所以不会再次加载。
第二种情况：虽然最后还是a + b 但是在加载时，d = c + "b" 中的c是变量，所以即使常量池中已经存在“ab”,依然会新创建一个新的。


```
String c = "a";
String d = c + "b";
String e = c + "b";

System.out.println( e==d);//false
```
即使常量和变量一直，也不会相等
