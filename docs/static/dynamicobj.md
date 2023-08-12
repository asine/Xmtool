# 扩展动态对象

扩展动态对象是整个工具库中最重要的一个设计。在软件开发过程中，我们经常需要定义各种各样的数据对象；例如：用于参数传递的数据实体类、用于接口返回结果的Json对象等等。

```c#
// 人员数据实体定义
public class Person
{
    public string Name { get; set; } = null;
    public int Age { get; set; } = 0;
}
```

当我们遇到的场景越多、越复杂，我们需要定义和维护的这种类也会更多；这无疑是一种负担，且这个过程相当枯燥和乏味。在编译型编程语言Javascript中，变量是没有强制指定类型的，用户使用时可以随意赋值；这让开发人员在使用过程中会非常灵活且方便；参考这种特性，并基于系统DynamicMetaObject对象，我们封装了DynamicObjectExt对象，我们称之为扩展动态对象，它实现了无需定义直接赋值的形式使用数据对象，像Javascript中一样简单和灵活，帮我们简化了大量的定义过程。

```c#
// 直接创建对象，并根据实际数据对象结构进行赋值即可
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
// TODO
```

除了这种基本能力之外，扩展对象中还提供了丰富的方法，用于对动态对象进行更多的判断和控制，使之能够满足各种各样的应用场景。

[this属性](#this)

[TrySetValue方法](#trysetvalue)

[TryGetValue方法](#trygetvalue)

[GetValue方法](#getvalue)

[GetValueByPath方法](#getvaluebypath)

[SetValue方法](#setvalue)

[SetValueByPath方法](#setvaluebypath)

[Has方法](#has)

[HasPath方法](#haspath)

[Remove方法](#remove)

[RemovePath方法](#removepath)

[ToString方法](#tostring)

[ToXMLString方法](#toxmlstring)



#### <a id="this">1. this属性</a>

###### 说明：根据指定属性名称获取属性值。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
Console.WriteLine(pernson["Name"]);	// 打印Name属性值，输出softwaiter
```



#### <a id="">2. TrySetValue方法</a>

##### public bool TrySetValue(string name, object value)

###### 说明：尝试为指定name的属性赋值，赋值成功返回true；否则返回false。

###### 参数：

**name**：准备赋值的属性名称。

**value**：属性赋值内容。



#### <a id="trygetvalue">3. TryGetValue方法</a>

##### public bool TryGetValue(string name, out object result)

###### 说明：尝试从指定name的属性获取值，获取成功返回true；否则返回false。

###### 参数：

**name**：准备取值的属性名称。

**result**：获取到的属性值；为获取到将返回null。



#### <a id="getvalue">4. GetValue方法</a>

##### public object GetValue(string name)

###### 说明：返回指定name的属性值；不存在找到则返回null。

###### 参数：

**name**：属性名称。



#### <a id="getvaluebypath">5. GetValueByPath方法</a>

##### public object GetValueByPath(string path)

###### 说明：GetValue方法的扩展，GetValueByPath能获取多层级属性的值。

###### 参数：

**path**：多层级属性路径，中间用“.”连接。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
person.Pet = Xmtool.DynamicObject();
person.Pet.Name = "Tom";
person.Pet.Kind = "Cat";
Console.WriteLine(pernson.GetValueByPath("Pet.Name"));	// 打印宠物名称，输出Tom
```



#### <a id="setvalue">6. SetValue方法</a>

##### public object SetValue(string name, object value)

###### 说明：为指定name的属性设置值。

###### 参数：

**name**：准备设置值得属性名称。

**value**：属性值内容。



#### <a id="setvaluebypath">7. SetValueByPath方法</a>

##### public bool SetValueByPath(string path, object value)

###### 说明：SetValue方法的扩展，SetValueByPath能设置多层级属性的值。

###### 参数：

**path**：多层级属性路径，中间用“.”连接。

**value**：属性值内容。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
person.Pet = Xmtool.DynamicObject();
person.Pet.Name = "Tom";
person.Pet.Kind = "Cat";
person.SetValueByPath("Pet.Name", "Chika");
Console.WriteLine(pernson.GetValueByPath("Pet.Name"));	// 打印宠物名称，输出Chika
```



#### <a id="has">8. Has方法</a>

##### public bool Has(string key)

###### 说明：判断是否包含指定属性。

###### 参数：

**key**：属性名称。



#### <a id="haspath">9. HasPath方法</a>

##### public bool HasPath(string path)

###### 说明：Has方法的扩展，HasPath能判断多层级属性是否存在。

###### 参数：

**path**：多层级属性路径，中间用“.”连接。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
person.Pet = Xmtool.DynamicObject();
person.Pet.Name = "Tom";
person.Pet.Kind = "Cat";
if (person.HasPath("Pet.Name"))
{
    // TODO
}
```



#### <a id="remove">10. Remove方法</a>

##### public bool Remove(string key)

###### 说明：从对象上删除指定的属性。

###### 参数：

**key**：属性名称。



#### <a id="removepath">11. RemovePath方法</a>

##### public bool RemovePath(string path)

###### 说明：Remove方法的扩展，RemovePath能删除多层级属性。

###### 参数：

**path**：多层级属性路径，中间用“.”连接。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
person.Pet = Xmtool.DynamicObject();
person.Pet.Name = "Tom";
person.Pet.Kind = "Cat";
person.RemovePath("Pet.Kind");
if (person.HasPath("Pet.Kind")) // 将返回false
{
	// TODO   
}
```



#### <a id="tostring">12. ToString方法</a>

##### public override string ToString()

###### 说明：将对象序列化为JSON字符串并返回；常用于API接口结果返回。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
string json = person.ToString();
Console.WriteLine(json);	// 输出{"Name":"softwaiter","Age":18}
```



#### <a id="toxmlstring">13. ToXMLString方法</a>

##### public string ToXMLString(string defaultNS = "")

###### 说明：将对象序列化为XML字符串并返回。

###### 参数：

**defaultNS**：默认命名空间。

```c#
dynamic person = Xmtool.DynamicObject();
person.Name = "softwaiter";
person.Age = 18;
string xml = person.ToXMLString();
Console.WriteLine(xml);	//输出<xml><Name>softwaiter</Name><Age>18</Age></xml>
string xml2 = person.ToXMLString("http://www.xmltool.com");
Console.WriteLine(xml2); //输出<xml xmlns="http://www.xmltool.com"><Name>softwaiter</Name><Age>18</Age></xml>
```

