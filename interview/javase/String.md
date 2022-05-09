# String类的方法有哪些
String类是java最常见的API，包含大量的处理字符串的方法  
+ char charAt(int index)：返回指定索引处的字符；

+ String substring(int beginIndex, int endIndex)：从此字符串中截取出一部分子字符串；

+ String[] split(String regex)：以指定的规则将此字符串分割成数组；

+ String trim()：删除字符串前导和后置的空格；

+ int indexOf(String str)：返回子串在此字符串首次出现的索引；

+ int lastIndexOf(String str)：返回子串在此字符串最后出现的索引；

+ boolean startsWith(String prefix)：判断此字符串是否以指定的前缀开头；

+ boolean endsWith(String suffix)：判断此字符串是否以指定的后缀结尾；

+ String toUpperCase()：将此字符串中所有的字符大写；

+ String toLowerCase()：将此字符串中所有的字符小写；

+ String replaceFirst(String regex, String replacement)：用指定字符串替换第一个匹配的子串；

+ String replaceAll(String regex, String replacement)：用指定字符串替换所有的匹配的子串。


# String、StringBuffer和StringBuilder
String类是不可变类，一旦一个String对象被创建，包含在这个对象中的字符序列就不可改变，直到对象被销毁  
StringBuffer对象则是一个字符序列可变的字符串，当一个StringBuffer被创建以后，通过StringBuffer提供的append()等方法可以改变这个字符串对象的字符序列，可以通过toString()方法将其转换为一个String对象  
StringBuilder与StringBuffer都是可变的字符串对象，都有共同的父类AbstractStringBuilder，构造方法和成员方法也基本相同。StringBuffer是线程安全的，而StringBuilder则是非线程安全的，所以Builder得性能略高。一般情况下，优先考虑使用StringBuilder类  

# 字符串拼接
+ + 运算符：如果拼接的都是字符串直接量，则适合使用 + 运算符实现拼接；

+ StringBuilder：如果拼接的字符串中包含变量，并不要求线程安全，则适合使用StringBuilder；

+ StringBuffer：如果拼接的字符串中包含变量，并且要求线程安全，则适合使用StringBuffer；

+ String类的concat方法：如果只是对两个字符串进行拼接，并且包含变量，则适合使用concat方法；

# 字符串相加的底层是如何实现
如果拼接的都是字符串直接量，则在编译时编译器会将其直接优化为一个完整的字符串，等同于新建一个完整的字符串  
如果拼接的字符串中包含变量，则在编译时编译器采用StringBuilder对其进行优化，调用append方法，对其进行拼接
